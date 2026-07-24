# PROMPT_16.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_16: Middleware & Pipeline Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_16
- **Phase:** 2
- **Stage:** 6 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-09 (PROMPT_09), ART-12 (PROMPT_12), ART-14 (PROMPT_14), ART-15 (PROMPT_15).
- **Estimated Tokens:** 11000–17000
- **Output Artifacts:** ART-16 (Doc) — Middleware & Pipeline Map.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Middleware & Pipeline Map artifact (ART-16) that identifies every middleware chain and pipeline in the subject repository (HTTP middleware, request/response pipelines, data-processing pipelines, build pipelines, deployment pipelines, event-processing pipelines), records each pipeline's stages in their order of execution with their inputs and outputs and composition pattern (chain-of-responsibility, pipeline, filter), identifies every middleware that can short-circuit, and classifies every cross-cutting concern (logging, auth, CORS, compression, tracing).

---

## 3. When to Invoke

PROMPT_16 is dispatched when ALL of the following predicates hold:

- Phase 1 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.2.
- ART-01, ART-05, ART-09, ART-12, ART-14, and ART-15 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-05 records at least one `middleware_registration` OR ART-14 records at least one event-driven workflow OR ART-04 (referenced transitively) records at least one build/CI pipeline (else `NO_PIPELINES` and the prompt emits a minimal catalog with that finding).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-05 | Map | Middleware registrations (`MW-XX` entries) — the seed for HTTP middleware chain reconstruction. |
| ART-09 | Doc | Function signatures and side effects; middleware functions are detected by their `(req, res, next)` signature and by their registration as handlers. |
| ART-12 | Graph | Control-flow execution paths and async boundaries; middleware chains execute sequentially and the chain's order is enforced by ART-12's branch ordering. |
| ART-14 | Doc | Event workflow catalog; event-driven pipelines (handlers chained by event emission) are detected by chaining `HD-XX` and `EM-XX` pairs. |
| ART-15 | Doc | API catalog; every API's middleware chain is part of its contract. Auth middleware identified per ART-15's auth requirements. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid flowchart conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect middleware and pipelines per category per § 6.1 (HTTP, RPC, data-processing, event-driven, build, deployment).
3. For every pipeline, reconstruct the chain per § 6.2 (ordered stages with inputs and outputs).
4. For every stage, enumerate inputs and outputs per § 6.3 (request, response, context, side effects).
5. Identify the composition pattern per § 6.4 (chain-of-responsibility, pipeline, filter, interceptor, onion).
6. Identify every middleware that can short-circuit per § 6.5 (auth denials, rate-limit rejections, cache hits, validation failures).
7. Classify cross-cutting concerns per § 6.6 (logging, auth, CORS, compression, tracing, metrics, request-id, error-handling).
8. Detect middleware ordering conflicts per § 6.7 (e.g., logging middleware after auth middleware that short-circuits).
9. Emit Mermaid flowchart diagrams per § 6.8 with stage-level citations.
10. Cross-check the middleware catalog against ART-05's registrations and ART-15's auth requirements per § 6.9; unaccounted middleware is `CONTRADICTION` findings per R33.
11. Emit ART-16 per § 8 with full front-matter, per-pipeline sections, short-circuit catalog, cross-cutting-concern classification, traceability index, open questions.
12. Run the Quality Checks in § 9.
13. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Pipeline Detection by Category

Detect pipelines by category, using ART-05's middleware registrations as the seed.

**HTTP middleware chains** — every `MW-XX` from ART-05 belongs to one or more HTTP middleware chains. Each chain records `pipeline_id` `PL-XX`, `kind: http-middleware`, `framework`, `entry_id` `EP-XX` (the server bootstrap that registered the chain), `stages` (ordered list of middleware function IDs), `mount_point` (global, router-specific, route-specific).

**RPC interceptors** — gRPC `Interceptor`, Connect-RPC `HandlerOption`, Apache Thrift `TProcessorEventHandler`, tRPC middleware. Each interceptor chain records `pipeline_id` `PL-XX`, `kind: rpc-interceptor`, `service_name`, `stages`.

**GraphQL middleware/directives** — `fieldMiddleware` (Apollo), `@Directive` (NestJS GraphQL), `shield` rules (graphql-shield), `plugins` (Mercurius). Each chain records `pipeline_id`, `kind: graphql-middleware`, `operation_id` `A-XX`, `stages`.

**Data-processing pipelines** — pipelines that transform data through a sequence of stages. Detected by: Node.js streams (`stream.Transform`, `pipeline()`), Python generators chained (`gen1 | gen2 | gen3` syntax in 3.10+, `yield from`), Rust `Iterator` chains (`.map().filter().collect()`), Java `Stream` pipelines, RxJS `pipe(...)`, Elixir `|>` pipes, Apache Beam, Spark pipelines, Flink pipelines, Kafka Streams topologies, Debezium pipelines. Each pipeline records `pipeline_id`, `kind: data-processing`, `source` (the input stream/topic/file), `sink` (the output), `stages` (the transformation functions in order).

**Event-driven pipelines** — handler-emit chains from ART-14. Each workflow `W-XX` with `≥ 3` steps is a candidate event-driven pipeline. Each records `pipeline_id`, `kind: event-driven`, `trigger_event` `E-XX`, `stages` (the handler functions in order), `terminal_event` `E-XX` (the last event emitted, when the workflow is non-cyclic).

**Build pipelines** — Maven phases (`validate`, `compile`, `test`, `package`, `verify`, `install`, `deploy`), Gradle tasks, npm scripts chained via `pre`/`post` hooks (`prebuild`, `build`, `postbuild`), webpack/vite/rollup plugin chains, Babel preset/plugin chains, Cargo build profiles. Each pipeline records `pipeline_id`, `kind: build`, `tool`, `stages` (the phases/tasks in order).

**Deployment pipelines** — CI/CD pipelines from ART-04 (cross-referenced): GitHub Actions jobs, GitLab CI stages, CircleCI workflows, Jenkins pipelines, ArgoCD sync waves, Helm hook ordering. Each pipeline records `pipeline_id`, `kind: deployment`, `platform`, `stages` (the jobs/stages in order, with dependencies).

### 6.2 Chain Reconstruction

For every pipeline, reconstruct the ordered chain of stages:

1. Start from the pipeline's seed (ART-05's middleware registration order for HTTP; ART-14's workflow steps for event-driven; ART-04's build/CI config for build/deployment).
2. Resolve every stage's `FN-XX` (the function that implements the stage).
3. For HTTP middleware: order is determined by the order of `app.use()` / `app.addHook()` / `app.useMiddleware()` calls in the bootstrap trace (cross-reference ART-05). Router-specific middleware is ordered after global middleware but before route handlers.
4. For event-driven pipelines: order is the workflow's step order from ART-14.
5. For data-processing pipelines: order is the order of `.pipe()` calls (Node streams), `|>` calls (Elixir), or `.map().filter()` chain (Java/Rust streams).
6. For build pipelines: order is the tool's phase ordering (Maven phases are fixed; Gradle tasks are ordered by `dependsOn`; npm `pre`/`post` hooks surround the named script).
7. For deployment pipelines: order is the platform's job-dependency graph (GitHub Actions `needs:`, GitLab CI `stage` + `needs:`, ArgoCD sync waves).

Each stage records `stage_id` `ST-XX`, `pipeline_id` `PL-XX`, `order` (0-based), `fn_id` `FN-XX`, `name`, `file:line-range`, `is_terminal: false` (the last stage before the handler), `handler_stage: true` for the route handler itself.

### 6.3 Stage Input/Output Enumeration

For every stage, enumerate inputs and outputs:

- **Inputs** — for HTTP middleware: `request` (the request object), `response` (the response object, mutable), `next` (the next-function callback), `context` (framework-specific context: Koa `ctx`, Fastify `request`/`reply`, ASP.NET `HttpContext`). For data-processing: the previous stage's output stream. For event-driven: the event payload. For build: the previous phase's artifacts.
- **Outputs** — for HTTP middleware: the mutated `request`/`response`, the `next()` call (which advances to the next stage), or a `response.send()` call (which short-circuits). For data-processing: the transformed stream. For event-driven: the emitted event(s). For build: the phase's produced artifacts.
- **Side effects** — per ART-09's side-effect records: state mutations, event emissions, external calls, persistence writes.

Each stage records `inputs`, `outputs`, `side_effects` (cross-referenced from ART-09), `citation`.

### 6.4 Composition-Pattern Identification

Identify the composition pattern for every pipeline:

- **Chain-of-responsibility** — each stage decides whether to handle the request or pass to the next. Common in HTTP middleware (Express, Koa, Fastify). Each stage calls `next()` to delegate; stages can short-circuit by responding directly.
- **Pipeline (linear)** — each stage unconditionally passes its output to the next. Common in data-processing pipelines (streams, Elixir pipes, Java Streams).
- **Filter** — each stage filters its input, passing only some items to the next. A specialization of the pipeline pattern.
- **Interceptor** — stages wrap the handler with pre- and post-processing. Common in gRPC, Spring AOP, NestJS interceptors. Each interceptor has `before()` and `after()` semantics.
- **Onion** — stages wrap the handler concentrically; the outermost stage enters first and exits last. Koa middleware is onion-shaped (two passes per stage).
- **Pub/sub bus** — stages subscribe to events; execution order is determined by subscription priority, not by explicit chaining. Common in event-driven pipelines.

Each pipeline records `composition_pattern` (chain-of-responsibility | pipeline | filter | interceptor | onion | pub-sub-bus | hybrid), `pattern_citation` (the code structure that evidences the pattern), `pass_count` (1 for single-pass pipelines, 2 for onion/two-pass interceptors).

### 6.5 Short-Circuit Detection

Identify every middleware that can short-circuit — terminate the chain before the handler executes:

- **Auth middleware** — `if (!authenticated) return res.status(401).send();` (Express), `throw new UnauthorizedException()` (NestJS), `if (!req.user) return ctx.throw(401)` (Koa), `if (!authenticated) return reply.code(401).send()` (Fastify).
- **Rate-limit middleware** — `if (overLimit) return res.status(429).send();`.
- **CORS middleware** — preflight `OPTIONS` requests are short-circuited with a `204 No Content` response.
- **Cache-hit middleware** — `if (cached) return res.send(cached);` (server-side caching).
- **Validation middleware** — `if (!valid) return res.status(400).send(errors);` (class-validator, Joi, Bean Validation).
- **Compression negotiation** — typically not a short-circuit but may skip compression for already-compressed responses.
- **Security middleware** — helmet, CSRF protection — `if (csrfInvalid) return res.status(403).send();`.

Each short-circuiting middleware records `short_circuit_id` `SC-XX`, `stage_id` `ST-XX`, `condition_text`, `condition_citation`, `short_circuit_response` (the response sent when short-circuiting), `bypasses_stages` (list of `ST-XX` that are skipped).

### 6.6 Cross-Cutting-Concern Classification

Classify every stage by its cross-cutting concern:

| Concern | Detection Patterns |
|---------|---------------------|
| logging | `morgan`, `winston`, `pino`, `log4j`, `Serilog`, `loguru`, `structlog`, `console.log` middleware |
| auth | `passport`, `express-jwt`, `jsonwebtoken`, `@nestjs/passport`, `Spring Security`, `django.contrib.auth`, `devise`, `authlib`, `next-auth`, `lucia-auth` |
| CORS | `cors` (Node), `@CrossOrigin` (Spring), `django-cors-headers`, `rack-cors` |
| compression | `compression` (Node), `gzip`/`brotli` middleware, `Spring Boot` `server.compression.enabled` |
| tracing | `@opentelemetry/api`, `dd-trace`, `newrelic`, `sentry`, `honeycomb-beeline`, `zipkin`, `jaeger` |
| metrics | `prom-client`, `prometheus`, `micrometer`, `statsd`, `datadog` |
| request-id | `express-request-id`, `@nestjs/microservices` request-id, custom `X-Request-ID` middleware |
| error-handling | `errorhandler` (Express), `@ExceptionHandler` (NestJS), `@ControllerAdvice` (Spring), `error.middleware` (Koa), global error filters |
| validation | `class-validator`, `joi`, `ajv`, `zod`, Bean Validation, Pydantic, Marshmallow |
| rate-limiting | `express-rate-limit`, `@nestjs/throttler`, `django-ratelimit`, `flask-limiter`, `rack-attack` |
| body-parsing | `body-parser`, `express.json()`, `express.urlencoded()`, `multer`, `Spring` `MultipartResolver` |
| session | `express-session`, `cookie-session`, `connect-redis`, `django.contrib.sessions`, `rack-session` |
| i18n | `i18next`, `react-i18next`, `vue-i18n`, `next-intl`, `django` i18n middleware |

Each stage records `concern` (one of the above or `custom`), `concern_citation`.

### 6.7 Middleware-Ordering-Conflict Detection

Detect middleware ordering conflicts — situations where the order of middleware produces incorrect or surprising behavior:

1. **Logging after auth short-circuit** — if logging middleware is registered after auth middleware, denied requests are not logged. Detected by inspecting the chain order and the auth middleware's short-circuit behavior.
2. **CORS after auth** — if CORS middleware is registered after auth, preflight `OPTIONS` requests may be denied. Detected by checking CORS's short-circuit on `OPTIONS`.
3. **Body-parser after handler** — body-parser must be registered before any middleware that reads `req.body`. Detected by checking the chain order against `req.body` accesses.
4. **Error-handler not last** — error-handling middleware (which has 4-arg signature `(err, req, res, next)`) must be registered after all other middleware. Detected by checking the position of error-handling middleware in the chain.
5. **Tracing before request-id** — tracing middleware that generates a trace ID must be registered after request-id middleware (if both are present) to ensure consistent IDs.

Each ordering conflict records `conflict_id` `OC-XX`, `pipeline_id` `PL-XX`, `stage_ids` (the conflicting stages), `description`, `severity` (MAJOR | MINOR | INFO), `citation`.

### 6.8 Mermaid Flowchart Emission

Emit Mermaid `flowchart TD` diagrams per `OUTPUT_RULES.md` § 7:

- **Per-pipeline chain diagram** — one diagram per pipeline showing the ordered stages. Stages rendered as rectangles; short-circuit branches rendered as diamonds with the short-circuit response as the branch target; the handler rendered as a distinct shape (hexagon). Each edge carries an `edge: file:line` comment.
- **Cross-cutting-concern matrix diagram** — `graph LR` showing every pipeline as a column and every cross-cutting concern as a row, with edges indicating which stages implement which concerns.
- **Short-circuit map** — `flowchart TD` showing every short-circuit point and the stages it bypasses.

Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

### 6.9 Coverage Cross-Check

Cross-check the middleware catalog against ART-05's registrations and ART-15's auth requirements:

1. Compute `M_05` = set of middleware `FN-XX` from ART-05's `middleware_registrations`.
2. Compute `M_15` = set of middleware `FN-XX` inferred from ART-15's auth-requirement citations (each auth-protected API's auth middleware is in the set).
3. Compute `M_16` = set of middleware `FN-XX` cataloged in ART-16.
4. Expected: `M_16 ⊇ M_05 ∪ M_15`. Middleware in `(M_05 ∪ M_15) \ M_16` are `COVERAGE_GAP` findings.
5. Middleware in `M_16 \ (M_05 ∪ M_15)` are middleware cataloged in ART-16 that ART-05 and ART-15 do not record; these are flagged for review (they may be dynamic registrations or framework-internal middleware).

---

## 7. Required Outputs

### ART-16 — Middleware & Pipeline Map

**Type:** Doc.

**Acceptance Criteria:**

- AC-16.1: The artifact file exists at `<output_root>/artifacts/phase2/ART16_<engagement_id>_middleware.md`.
- AC-16.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-16.3: The body contains: Executive Summary, Methodology, Pipeline Catalog (by category), Per-Pipeline Chain Diagrams, Short-Circuit Catalog, Cross-Cutting-Concern Classification, Ordering-Conflict Catalog, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-16.4: Every pipeline records its stages in order with inputs, outputs, and side effects.
- AC-16.5: Every Mermaid diagram is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-16.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-16.7: Every short-circuiting middleware is recorded with its condition and bypassed stages.
- AC-16.8: Coverage cross-check is recorded with no unresolved contradictions.

---

## 8. Output Templates

### 8.1 ART-16 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-16
artifact_type: Doc
producing_prompt: PROMPT_16
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
pipelines:
  - id: PL-01
    kind: http-middleware | rpc-interceptor | graphql-middleware | data-processing | event-driven | build | deployment
    framework: <name>
    entry_id: EP-XX | NA
    mount_point: global | router-specific | route-specific | NA
    composition_pattern: chain-of-responsibility | pipeline | filter | interceptor | onion | pub-sub-bus | hybrid
    pattern_citation: <file>:<line-range>
    pass_count: 1 | 2
    stages: [ST-XX]
stages:
  - id: ST-01
    pipeline_id: PL-XX
    order: <int>
    fn_id: FN-XX
    name: <name>
    file: <path>
    line_range: <start-end>
    is_terminal: true | false
    handler_stage: true | false
    inputs: [request | response | next | context | stream | event | artifacts]
    outputs: [mutated-request | mutated-response | next-call | short-circuit-response | transformed-stream | emitted-event | produced-artifacts]
    side_effects: [{ kind: <text>, target: <name>, citation: <file>:<line-range> }]
    concern: logging | auth | CORS | compression | tracing | metrics | request-id | error-handling | validation | rate-limiting | body-parsing | session | i18n | custom
    concern_citation: <file>:<line-range>
short_circuits:
  - id: SC-01
    stage_id: ST-XX
    condition_text: <text>
    condition_citation: <file>:<line-range>
    short_circuit_response: <text>
    bypasses_stages: [ST-XX]
ordering_conflicts:
  - id: OC-01
    pipeline_id: PL-XX
    stage_ids: [ST-XX]
    description: <text>
    severity: MAJOR | MINOR | INFO
    citation: <file>:<line-range>
coverage_cross_check:
  middleware_from_art05: [FN-XX]
  middleware_from_art15: [FN-XX]
  middleware_cataloged: [FN-XX]
  coverage_gaps: [FN-XX]
  catalog_only: [FN-XX]
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

### 8.2 ART-16 Body Skeleton

```markdown
# ART-16: Middleware & Pipeline Map

## 1. Executive Summary
## 2. Methodology
## 3. Pipeline Catalog
   ### 3.1 HTTP Middleware Chains
   #### PL-01: <name>
   **Diagram D-01: PL-01 Chain**
   ```mermaid
   flowchart TD
       REQ[Request] --> ST01[ST-01: logging]
       ST01 --> ST02[ST-02: auth]
       ST02 -->|authenticated| ST03[ST-03: validation]
       ST02 -->|denied| SC01[401 Unauthorized]
       ST03 --> HANDLER[Handler FN-XX]
       %% edge: src/app.ts:42
   ```
   <stage list>
   ### 3.2 Data-Processing Pipelines
   ### 3.3 Event-Driven Pipelines
   ### 3.4 Build Pipelines
   ### 3.5 Deployment Pipelines
## 4. Short-Circuit Catalog
## 5. Cross-Cutting-Concern Classification
   **Diagram D-02: Concern Matrix**
## 6. Ordering-Conflict Catalog
## 7. Coverage Cross-Check
## 8. Traceability Index
## 9. Open Questions
## 10. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every middleware from ART-05 and every auth middleware from ART-15 is cataloged; threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of pipelines, stages, short-circuits, and ordering-conflicts cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no pipeline in ART-16 contradicts ART-05's middleware registrations or ART-15's auth requirements.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` composition pattern, short-circuit condition, and ordering conflict has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.2 on a 5% sample of pipelines yields the same stage list.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-16.A. Stage Order Completeness** — every pipeline's stages are recorded in order with `order` field set; no duplicate orders within a pipeline.
- **Q-16.B. Short-Circuit Coverage** — every stage with `concern: auth`, `concern: rate-limiting`, `concern: validation`, or `concern: CORS` has a short-circuit entry or an Open Question explaining why it cannot short-circuit.
- **Q-16.C. Composition-Pattern Evidence** — every pipeline's `composition_pattern` is backed by a `pattern_citation` that evidences the pattern.
- **Q-16.D. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment.
- **Q-16.E. Sidecar Files** — every Mermaid block has a `.mmd` sidecar file.
- **Q-16.F. Ordering-Conflict Severity** — every `MAJOR` ordering conflict has an Open Question documenting the impact.
- **Q-16.G. Handler Stage Presence** — every HTTP middleware pipeline ends with the handler stage (`handler_stage: true`); absence is flagged.

---

## 10. Common Pitfalls

- Do not infer middleware order from comments or README; trace the actual `app.use()` order per R22.
- Always record both the request and response passes for onion-shaped middleware (Koa); collapsing them loses the two-pass semantics.
- Do not conflate router-specific middleware with global middleware; the mount point changes the chain's effective scope.
- Always cite the short-circuit condition; a short-circuit without a condition citation is non-conformant.
- Do not omit ordering conflicts because they are "intentional"; the conflict is recorded as a finding, and the intent is recorded in the description.
- Always cross-check middleware against ART-05 and ART-15; a middleware referenced elsewhere but missing from ART-16 is a `COVERAGE_GAP`.
- Do not infer the composition pattern from the framework alone; verify by inspecting the stage call structure.
- Always record the side effects of each stage; a stage without side-effect records omits critical behavior for PROMPT_17.
- Do not collapse event-driven pipelines with HTTP pipelines; they have different composition patterns and different consumers.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders diagrams from the sidecar source.
- Do not omit the handler stage from HTTP pipelines; the handler is the terminal stage and is part of the contract.

---

## 11. Handoff Criteria

PROMPT_17 and PROMPT_22 consume ART-16. Handoff requires ALL of:

- HC-16.1: ART-16 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-16.2: Every pipeline records its stages in order with composition pattern.
- HC-16.3: Every short-circuiting middleware is cataloged.
- HC-16.4: Cross-cutting concerns are classified.
- HC-16.5: Ordering conflicts are recorded.
- HC-16.6: Mermaid diagrams are emitted with `.mmd` sidecar files.
- HC-16.7: Coverage cross-check is recorded with no unresolved contradictions.
- HC-16.8: `repository_fingerprint_recheck` matches ART-01.
- HC-16.9: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_17 (Error Handling & Resilience — uses middleware pipelines to identify error-handling middleware and global error filters), PROMPT_22 (Streaming Workflow — uses data-processing pipelines as the seed for streaming-workflow identification).
- **Depends on:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-09 (PROMPT_09), ART-12 (PROMPT_12), ART-14 (PROMPT_14), ART-15 (PROMPT_15).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 7.
- **Forward reference:** PROMPT_30 verifies that every middleware referenced by ART-17 (error handling) or ART-22 (streaming) resolves to an entry in ART-16 and that ordering conflicts are addressed.

*End of PROMPT_16. Orchestrator may dispatch PROMPT_17 upon satisfaction of § 11.*
