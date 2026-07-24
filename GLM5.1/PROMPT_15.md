# PROMPT_15.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_15: API & Interface Documentation

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_15
- **Phase:** 2
- **Stage:** 5 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10).
- **Estimated Tokens:** 12000–18000
- **Output Artifacts:** ART-15 (Doc) — API & Interface Reference.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the API & Interface Reference artifact (ART-15) that documents every public API in the subject repository — HTTP endpoints, RPC methods, GraphQL operations, CLI commands, library public APIs (exported functions and classes), and event subscriptions — with each API's contract, parameters, responses, error cases, authentication requirement, rate limits, versioning, and deprecation status, and detects OpenAPI/Proto/GraphQL schema files and API gateways and routing maps.

---

## 3. When to Invoke

PROMPT_15 is dispatched when ALL of the following predicates hold:

- Phase 1 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.2.
- ART-01, ART-05, ART-08, ART-09, and ART-10 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-05 records at least one `route_registration` OR ART-08 records at least one `public` class/interface OR ART-09 records at least one `exported` function OR ART-04 (referenced transitively) records at least one OpenAPI/Proto/GraphQL schema file (else `NO_PUBLIC_APIS` and the prompt emits a minimal catalog with that finding).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-05 | Map | Route registrations (`RR-XX` entries) — the seed for HTTP endpoint detection. |
| ART-08 | Doc | Class catalog; public classes and exported interfaces are library API candidates. Constructor signatures and public methods are part of the API contract. |
| ART-09 | Doc | Function catalog; exported functions are library API candidates. Function signatures drive contract extraction. |
| ART-10 | Graph | Call graph; for each API handler, the call graph identifies downstream callees for contract inference (which functions implement which API). |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid graph conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). HOOK-04 (API Contract) is enforced. |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect APIs per category per § 6.1 (HTTP, RPC, GraphQL, CLI, library, event subscriptions).
3. For every API, extract the contract per § 6.2 (parameters, body, response, status codes, error cases).
4. Enumerate error cases per § 6.3 from declared exceptions, status-code returns, and validation throws.
5. Infer authentication requirements per § 6.4 from middleware decorators, route guards, and auth-library usage.
6. Detect API schema files per § 6.5 (OpenAPI, Proto, GraphQL, AsyncAPI, JSON Schema).
7. Construct the routing map per § 6.6 (paths, methods, handlers, gateways, versioned prefixes).
8. Detect rate limits and quotas per § 6.7.
9. Detect versioning and deprecation per § 6.8.
10. Emit Mermaid diagrams per § 6.9 (API surface overview, per-handler call tree, routing map).
11. Cross-check the API catalog against ART-05's route registrations and ART-08/ART-09's public symbols per § 6.10; unaccounted APIs are `CONTRADICTION` findings per R33.
12. Emit ART-15 per § 8 with full front-matter, per-API sections, schema-file catalog, routing map, traceability index, open questions.
13. Run the Quality Checks in § 9.
14. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 API Detection by Category

Detect APIs by category, using ART-05's route registrations as the seed and supplementing with code-pattern scanning.

**HTTP endpoints** — every `RR-XX` from ART-05 is an HTTP endpoint. Each endpoint records `api_id` `A-XX`, `kind: http`, `method` (GET | POST | PUT | DELETE | PATCH | HEAD | OPTIONS | WS), `path_pattern`, `handler_id` `FN-XX`, `file:line-range`, `framework`, `route_metadata` (decoration chain). Cross-reference ART-05's `route_registrations` for the registration order and parent router.

**RPC methods** — gRPC service methods (detected by `.proto` files and generated server stubs), Apache Thrift, JSON-RPC, SOAP, tRPC, Connect-RPC. Each method records `api_id` `A-XX`, `kind: rpc`, `rpc_framework`, `service_name`, `method_name`, `handler_id` `FN-XX`, `file:line-range`. For gRPC, cross-reference the `.proto` file (per § 6.5).

**GraphQL operations** — queries, mutations, and subscriptions. Detected by `@Query()`, `@Mutation()`, `@Subscription()` decorators (NestJS), `type Query { ... }` schema declarations (Apollo, graphql-ruby, Graphene, Strawberry), resolver functions. Each operation records `api_id` `A-XX`, `kind: graphql`, `operation_type` (query | mutation | subscription), `operation_name`, `resolver_id` `FN-XX`, `file:line-range`.

**CLI commands** — every entry point with `kind: cli` in ART-05 plus sub-commands detected by framework patterns: `@click.command()`, `@click.group()` (Click), `app.command()` (Typer), `app.command()` (Commander.js), `cli.AddCommand()` (Cobra), `argparse` subparsers, `picocli` `@Command` (Java), `CommandLine` (C# `System.CommandLine`), `clap` `Parser` (Rust), `StructOpt` (Rust), `Option<T>` (Rust `clap` derive). Each command records `api_id` `A-XX`, `kind: cli`, `command_name`, `subcommand_path`, `handler_id` `FN-XX`, `file:line-range`.

**Library public APIs** — exported functions and exported classes from ART-09 and ART-08. An export is "public" when: (a) TypeScript/JavaScript — declared in `package.json` `exports`/`main`/`module` or marked `export`; (b) Python — declared in `__all__` or in a module reachable from the package's `__init__.py`; (c) Rust — marked `pub` and reachable from the crate root (`lib.rs`); (d) Go — capitalized identifier in a non-internal package; (e) Java — `public` member of a `public` class; (f) C# — `public` member of a `public` class. Each public symbol records `api_id` `A-XX`, `kind: library`, `symbol_id` (`FN-XX` or `K-XX`), `export_path`, `file:line-range`.

**Event subscriptions** — every handler `HD-XX` from PROMPT_14 (forward reference; if ART-14 is not yet emitted, the agent scans for event-handler registrations directly) is a subscription API. Each subscription records `api_id` `A-XX`, `kind: event-subscription`, `event_id` `E-XX`, `handler_id` `FN-XX`, `file:line-range`.

### 6.2 Contract Extraction

For every API, extract the contract:

- **Parameters** — for HTTP: path params, query params, headers, cookies. For RPC/GraphQL: method arguments. For CLI: flags, positional args. For library: function/method parameters. Each parameter records `name`, `type`, `required`, `default`, `validation_rules` (e.g., `min=0`, `max=100`, `pattern=^\w+$`), `source_citation`.
- **Request body** — for HTTP/RPC/GraphQL: the request body schema (`D-XX` from ART-11 when available, otherwise the inferred type). Records `content_type`, `schema_id`, `required`, `max_size` (when declared).
- **Response** — for HTTP: status code, body schema, headers. For RPC/GraphQL: return type. For CLI: stdout/stderr format, exit codes. For library: return type. Each response records `status_code` (HTTP) or `return_type` (other), `schema_id`, `headers` (HTTP), `example` (when available from docstring or schema file).
- **Side effects** — the API's side effects per ART-09 (state mutations, event emissions, external calls). Each side effect is recorded with `kind`, `target`, `citation`.

### 6.3 Error-Case Enumeration

Enumerate every error case for every API:

1. **Declared exceptions** — read from ART-09's `throws/raises`; each declared exception type is an error case with `error_code` (HTTP status inferred from exception type — e.g., `NotFoundException` → 404 — or from explicit `ResponseEntity.status(...)` calls), `error_schema`, `citation`.
2. **Validation errors** — `400 Bad Request` for HTTP, equivalent validation errors for other kinds. Detected by validation-framework throws (`class-validator` `ValidationError`, Bean Validation `ConstraintViolationException`, Pydantic `ValidationError`, Marshmallow `ValidationError`).
3. **Auth errors** — `401 Unauthorized`, `403 Forbidden`. Detected by auth middleware throws (cross-reference § 6.4).
4. **Not-found errors** — `404 Not Found`. Detected by `NotFoundException`, `EntityNotFoundError`, `return null` patterns in handlers.
5. **Conflict errors** — `409 Conflict`. Detected by `ConflictException`, unique-constraint violations.
6. **Rate-limit errors** — `429 Too Many Requests`. Detected by rate-limit middleware (cross-reference § 6.7).
7. **Server errors** — `500 Internal Server Error` and `502/503/504`. Detected by global exception handlers (cross-reference PROMPT_17).

Each error case records `error_id` `ERR-XX`, `api_id` `A-XX`, `status_code` (HTTP) or `error_kind` (other), `error_schema`, `trigger_condition`, `citation`.

### 6.4 Authentication-Requirement Inference

Infer the authentication requirement for every API:

1. **Decorated APIs** — APIs whose handler or class is decorated with an auth decorator: `@UseGuards(JwtAuthGuard)` (NestJS), `@PreAuthorize("hasRole('ADMIN')")` (Spring), `@Authorize` (ASP.NET), `@login_required` (Django), `@jwt_required` (Flask), `before_action :authenticate_user!` (Rails), `@requires_auth` (Falcon).
2. **Middleware-protected APIs** — APIs whose route is matched by an auth middleware (cross-reference ART-05's middleware registrations and PROMPT_16's middleware catalog).
3. **Implicit auth** — APIs in a router that is mounted under an auth middleware (e.g., `app.use('/api', authMiddleware); app.use('/api', router)`). The auth requirement is inherited from the parent router.
4. **No auth** — APIs that do not match any of the above are `no-auth-required`. Public APIs (login, register, health-check, public docs) are expected to be `no-auth-required`; if a sensitive API is `no-auth-required`, flag it as `AUTH_GAP_CANDIDATE` and cross-reference PROMPT_19.

Each API records `auth_requirement` (none | api-key | bearer-jwt | bearer-opaque | basic | session-cookie | oauth2 | mTLS | UNVERIFIED), `auth_scopes` (list of required scopes/roles), `auth_citation`.

### 6.5 Schema-File Detection

Detect API schema files and parse them:

- **OpenAPI / Swagger** — `openapi.json`, `openapi.yaml`, `swagger.json`, `swagger.yaml`, `@OpenAPIDefinition` annotations (Spring), `fastify.swagger()` configs, NestJS `DocumentBuilder`. Parse the schema and cross-reference every `path`/`operationId` to an `A-XX` API entry; mismatches are `CONTRADICTION` findings.
- **Protocol Buffers** — `.proto` files. Parse `service` and `rpc` definitions; cross-reference to gRPC handler implementations.
- **GraphQL SDL** — `.graphql`, `.gql` files, `typeDefs` strings, `buildSchema()` calls. Parse `type Query`, `type Mutation`, `type Subscription`; cross-reference to resolvers.
- **AsyncAPI** — `asyncapi.json`, `asyncapi.yaml`. Parse `channels` and cross-reference to event publishers/subscribers (cross-reference ART-14).
- **JSON Schema** — `.json` schema files referenced by `$ref` in OpenAPI or by validation frameworks. Parse and cross-reference to request/response schemas.

Each schema file records `schema_file_id` `SF-XX`, `format` (openapi | proto | graphql | asyncapi | json-schema), `path`, `version`, `api_count` (the number of APIs the schema defines), `external: false` for in-scope files or `external: true` for vendored schemas.

### 6.6 Routing-Map Construction

Construct the routing map — the full path-to-handler mapping including parent routers, prefixes, and gateways:

1. For every `RR-XX` in ART-05, resolve the full path by concatenating parent router prefixes (e.g., `app.use('/api/v1', userRouter); userRouter.get('/users/:id', getUser)` → `/api/v1/users/:id`).
2. Detect API gateways — reverse proxies that route based on path prefix (Kong, NGINX, AWS API Gateway, Traefik, Envoy). Detected by IaC files (cross-reference ART-04's IaC detection) and gateway configuration files.
3. Detect versioning prefixes — `/v1/`, `/v2/`, `/api/v1/`, header-based versioning (`Accept: application/vnd.api+json; version=1`), query-based versioning (`?version=2`).
4. Detect path-rewriting rules — gateway-level rewrites that change the path before the request reaches the application.

The routing map records `route_id`, `full_path`, `method`, `handler_id` `FN-XX`, `gateway_id` (when applicable), `version_prefix`, `rewrite_rule` (when applicable), `citation`.

### 6.7 Rate-Limit and Quota Detection

Detect rate limits and quotas:

- **Middleware-based** — `express-rate-limit`, `@nestjs/throttler`, `django-ratelimit`, `flask-limiter`, `rack-attack`, Spring `@RateLimit` (via Bucket4j or Resilience4j), ASP.NET `RateLimitingMiddleware`.
- **Gateway-based** — Kong `rate-limiting` plugin, AWS API Gateway throttling, NGINX `limit_req_zone`, Traefik `rateLimit`.
- **Quota-based** — per-tenant quotas detected by quota tables (`tenant_quotas`) and quota-check middleware.

Each rate limit records `rate_limit_id` `RL-XX`, `api_id` `A-XX` or `*` (when applied globally), `limit`, `window`, `key` (ip | user | tenant | api-key), `strategy` (fixed-window | sliding-window | token-bucket | leaky-bucket), `citation`.

### 6.8 Versioning and Deprecation

Detect versioning and deprecation for every API:

- **Versioning** — path version (`/v1/`), header version, query version, content negotiation. Each API records `version`, `versioning_scheme`.
- **Deprecation** — `@Deprecated` (Java/C#/TS/JS), `@deprecated` (JSDoc), `@deprecated` (Python `warnings.warn(DeprecationWarning)`), `DeprecationWarning` raises, OpenAPI `deprecated: true`, sunset headers. Each deprecated API records `deprecated: true`, `deprecation_citation`, `sunset_date` (when declared), `replacement_api_id` (when declared via `@deprecated("use X instead")` or similar).

### 6.9 Mermaid Diagram Emission

Emit Mermaid diagrams per `OUTPUT_RULES.md` § 7:

- **API surface overview** — `graph LR` showing every `A-XX` API grouped by category. Decomposed when > 30 APIs.
- **Per-handler call tree** — for each API handler with `≥ 3` callees in ART-10, a `graph TD` showing the handler's downstream call tree to depth 3. This is the API's "implementation footprint" diagram.
- **Routing map** — `graph LR` showing gateway → router → handler chains for the routing map.

Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

### 6.10 Coverage Cross-Check

Cross-check the API catalog against ART-05's route registrations and ART-08/ART-09's public symbols:

1. Compute `A_routes` = set of APIs derived from ART-05's `route_registrations`.
2. Compute `A_public` = set of APIs derived from ART-08/ART-09's public symbols.
3. Compute `A_15` = set of APIs cataloged in ART-15.
4. Expected: `A_15 ⊇ A_routes ∪ A_public`. APIs in `(A_routes ∪ A_public) \ A_15` are `COVERAGE_GAP` findings.
5. APIs in `A_15 \ (A_routes ∪ A_public)` are APIs cataloged in ART-15 that ART-05/ART-08/ART-09 do not record; these are flagged for review (they may be schema-file-only APIs not yet wired to handlers, or they may be `CONTRADICTION` findings).

---

## 7. Required Outputs

### ART-15 — API & Interface Reference

**Type:** Doc.

**Acceptance Criteria:**

- AC-15.1: The artifact file exists at `<output_root>/artifacts/phase2/ART15_<engagement_id>_apis.md`.
- AC-15.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-15.3: The body contains: Executive Summary, Methodology, API Catalog (by category), Schema Files, Routing Map, Rate Limits, Versioning & Deprecation, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-15.4: Every API records contract (parameters, body, response, errors), auth requirement, version, and deprecation status.
- AC-15.5: Every Mermaid diagram is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-15.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-15.7: Every API has at least one error case or is marked `no-errors-declared`.
- AC-15.8: Coverage cross-check is recorded with no unresolved contradictions.

---

## 8. Output Templates

### 8.1 ART-15 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-15
artifact_type: Doc
producing_prompt: PROMPT_15
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
apis:
  - id: A-01
    kind: http | rpc | graphql | cli | library | event-subscription
    name: <name>
    method: GET | POST | PUT | DELETE | PATCH | HEAD | OPTIONS | WS | NA
    path_pattern: <pattern> | NA
    handler_id: FN-XX
    file: <path>
    line_range: <start-end>
    framework: <name>
    auth_requirement: none | api-key | bearer-jwt | bearer-opaque | basic | session-cookie | oauth2 | mTLS | UNVERIFIED
    auth_scopes: [<scope>]
    auth_citation: <file>:<line-range>
    version: <text>
    versioning_scheme: path | header | query | content-negotiation | NA
    deprecated: true | false
    deprecation_citation: <file>:<line-range> | NA
    sunset_date: <ISO-date> | NA
    replacement_api_id: A-XX | NA
contracts:
  - api_id: A-XX
    parameters:
      - name: <name>
        location: path | query | header | cookie | body | arg | flag | positional
        type: <type>
        required: true | false
        default: <value> | none
        validation_rules: [<rule>]
        source_citation: <file>:<line-range>
    request_body:
      content_type: <text> | NA
      schema_id: D-XX | NA
      required: true | false
      max_size: <int> | UNVERIFIED
    responses:
      - status_code: <int> | NA
        return_type: <type> | NA
        schema_id: D-XX | NA
        headers: { <name>: <type> }
        example: <text> | NA
    side_effects:
      - kind: state-mutation | event-emit | external-call | persistence-write
        target: <name>
        citation: <file>:<line-range>
error_cases:
  - id: ERR-01
    api_id: A-XX
    status_code: <int> | NA
    error_kind: <text>
    error_schema: D-XX | NA
    trigger_condition: <text>
    citation: <file>:<line-range>
schema_files:
  - id: SF-01
    format: openapi | proto | graphql | asyncapi | json-schema
    path: <path>
    version: <text>
    api_count: <int>
    external: false
routing_map:
  - route_id: RM-01
    full_path: <pattern>
    method: GET | POST | ...
    handler_id: FN-XX
    gateway_id: GW-XX | NA
    version_prefix: <text> | NA
    rewrite_rule: <text> | NA
    citation: <file>:<line-range>
rate_limits:
  - id: RL-01
    api_id: A-XX | "*"
    limit: <int>
    window: <text>
    key: ip | user | tenant | api-key
    strategy: fixed-window | sliding-window | token-bucket | leaky-bucket
    citation: <file>:<line-range>
coverage_cross_check:
  apis_from_routes: [A-XX]
  apis_from_public_symbols: [A-XX]
  apis_cataloged: [A-XX]
  coverage_gaps: [A-XX]
  catalog_only: [A-XX]
mermaid_sources:
  - diagram_id: D-01
    title: <text>
    sidecar_file: <relative-path>
    node_count: <int>
source_coverage:
  - path: <file_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <file_path>:<line-range>
    symbol: <name>
sections:
  - id: S-01
    title: <string>
    claims: [C-XX]
---
```

### 8.2 ART-15 Body Skeleton

```markdown
# ART-15: API & Interface Reference

## 1. Executive Summary
## 2. Methodology
## 3. API Catalog
   ### 3.1 HTTP Endpoints
   #### A-01: <method> <path>
   - Handler: FN-XX (<file>:<line-range>)
   - Auth: <requirement>
   - Parameters: <list>
   - Responses: <list>
   - Errors: <list>
   ### 3.2 RPC Methods
   ### 3.3 GraphQL Operations
   ### 3.4 CLI Commands
   ### 3.5 Library APIs
   ### 3.6 Event Subscriptions
## 4. Schema Files
   **Diagram D-01: API Surface Overview**
## 5. Routing Map
   **Diagram D-02: Routing Map**
## 6. Rate Limits
## 7. Versioning & Deprecation
## 8. Coverage Cross-Check
## 9. Traceability Index
## 10. Open Questions
## 11. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every route in ART-05 and every public symbol in ART-08/ART-09 is cataloged; threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of APIs, parameters, responses, errors, and routing-map entries cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no API in ART-15 contradicts ART-05's route registrations or ART-08/ART-09's public symbols. No schema-file API contradicts the schema.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` auth requirement, schema-file mismatch, and `AUTH_GAP_CANDIDATE` has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.1 on a 5% sample of files yields the same API set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-15.A. Contract Completeness (HOOK-04)** — every API has documented parameters, request body (when applicable), response, errors, and authentication requirement.
- **Q-15.B. Schema-File Cross-Reference** — every API defined in a schema file has a corresponding handler `FN-XX` and vice versa.
- **Q-15.C. Auth Consistency** — APIs marked `no-auth-required` that handle sensitive data (cross-reference ART-11's sensitive flows) are flagged `AUTH_GAP_CANDIDATE`.
- **Q-15.D. Error Coverage** — every API has at least one error case or is marked `no-errors-declared` with rationale.
- **Q-15.E. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment.
- **Q-15.F. Sidecar Files** — every Mermaid block has a `.mmd` sidecar file.
- **Q-15.G. Deprecation Tracking** — every deprecated API has a `deprecation_citation` and (when declared) a `replacement_api_id`.

---

## 10. Common Pitfalls

- Do not infer API contracts from README or docstring descriptions alone; verify against the handler's actual signature per R22.
- Always resolve the full path from parent router prefixes; a partial path omits the version segment and misleads consumers.
- Do not omit error cases because they are "obvious"; the `404` for a not-found handler is a first-class error case.
- Always cite the auth decorator or middleware that protects an API; `UNVERIFIED` auth is non-conformant unless explained.
- Do not conflate library APIs with internal functions; only symbols reachable from the package's public exports are library APIs.
- Always cross-check schema files against handler implementations; a schema-file API without a handler is a `COVERAGE_GAP`.
- Do not infer rate limits from the absence of rate-limit middleware; absence is recorded as `no-rate-limit`, not as `UNVERIFIED`.
- Always record deprecation citations verbatim; a deprecated API without a citation is non-conformant.
- Do not collapse distinct APIs that share a path with different methods; `GET /users/:id` and `DELETE /users/:id` are distinct `A-XX` entries.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders diagrams from the sidecar source.
- Do not document private/internal endpoints as public APIs; APIs under `/internal`, `/admin`, `/health` are flagged with `visibility: internal`.

---

## 11. Handoff Criteria

PROMPT_16 and PROMPT_28 consume ART-15. Handoff requires ALL of:

- HC-15.1: ART-15 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-15.2: Every API records contract, errors, and authentication requirement (HOOK-04).
- HC-15.3: Schema files are cataloged and cross-referenced to handlers.
- HC-15.4: Routing map is constructed with full paths.
- HC-15.5: Rate limits, versioning, and deprecation are recorded.
- HC-15.6: Mermaid diagrams are emitted with `.mmd` sidecar files.
- HC-15.7: Coverage cross-check is recorded with no unresolved contradictions.
- HC-15.8: `repository_fingerprint_recheck` matches ART-01.
- HC-15.9: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_16 (Middleware & Pipeline — uses API auth requirements to identify auth middleware chains), PROMPT_19 (Authentication & Authorization — uses auth requirements to validate the auth model), PROMPT_28 (Cross-Reference Checklists — uses ART-15 as the API-coverage ground truth).
- **Depends on:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). HOOK-04 (API Contract) is enforced by PROMPT_30.
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 7.
- **Forward reference:** PROMPT_30 verifies HOOK-04 (every API has contract, errors, and auth) and that every API referenced by ART-16, ART-19, or ART-28 resolves to an entry in ART-15.

*End of PROMPT_15. Orchestrator may dispatch PROMPT_16 upon satisfaction of § 11.*
