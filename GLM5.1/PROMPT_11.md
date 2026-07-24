# PROMPT_11.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_11: Data Flow Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_11
- **Phase:** 2
- **Stage:** 1 of 10 (Phase 2 opener)
- **Dependencies:** ART-01 (PROMPT_01), ART-06 (PROMPT_06), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10).
- **Estimated Tokens:** 14000–20000
- **Output Artifacts:** ART-11 (Graph) — Data Flow Diagrams.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Data Flow Diagrams artifact (ART-11) that enumerates every significant data type in the subject repository, traces each data type's journey from origin to terminal through the call graph and module map, identifies every transformation, validation, serialization, and boundary crossing along each journey, catalogs every source and sink, flags every sanitization point, and flags every flow that carries PII or other sensitive data.

---

## 3. When to Invoke

PROMPT_11 is dispatched when ALL of the following predicates hold:

- Phase 1 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.2 (PROMPT_10 emitted `status: SUCCESS`).
- ART-01, ART-06, ART-08, ART-09, and ART-10 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-10 records at least one in-scope call edge (`call_edges` non-empty) or at least one field-access edge (`field_access_edges` non-empty); else the data-flow graph is trivial and recorded as `EMPTY_DATA_FLOW`.
- ART-09 records at least one function with a declared or inferred parameter or return type; absent any typed surface, the prompt degrades to value-flow-by-name analysis and records `TYPE_INFORMATION_SPARSE` as an Open Question.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-06 | Map | Module boundaries; data-flow edges are aggregated to the module level for the master data-flow diagram. |
| ART-08 | Doc | Class catalog; class fields (`V-XX`) and accessors are the data carriers traversed by flows. Constructor parameters seed object-construction flows. |
| ART-09 | Doc | Function signatures, parameters, return types, and side-effect records; each function is a node in the data-flow graph with declared input and output ports. |
| ART-10 | Graph | Call graph and `field_access_edges`; the call graph provides the topology along which data propagates, and field-access edges are first-class data-flow edges. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R16, R17, R19, R21, R22, R23 (UNVERIFIED), R33 (contradiction escalation). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, and Mermaid graph conventions (§ 7) including `.mmd` sidecar files. |
| `QUALITY_STANDARDS.md` | Framework file | Apply Graph schema (`§ 4.4`, extends Map) and type-specific bar (aggregate ≥ 30, Coverage ≥ 4, Coherence ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Enumerate significant data types per § 6.1 by clustering function parameters, return types, and class fields emitted by ART-08 and ART-09.
3. Enumerate data sources and sinks per § 6.2 by scanning entry points, I/O call sites, persistence APIs, and external-API call sites.
4. For every significant data type, trace the transformation chain per § 6.3 from origin to terminal, bounded by 30 hops and by ART-10's call-graph reachability.
5. Detect every boundary crossing per § 6.4 (process boundary, network boundary, persistence boundary, serialization boundary, trust boundary).
6. Identify every sanitization and validation point per § 6.5.
7. Flag every sensitive-data flow per § 6.6 by matching data-type names and field names against the sensitive-data lexicon in § 6.6.1.
8. Aggregate function-level flows to module-level flows per § 6.7 for the master diagram.
9. Emit Mermaid `graph LR` data-flow diagrams per § 6.8 with edge-level citations; decompose diagrams > 30 nodes by module.
10. Cross-check the data-flow node set against ART-09's parameter/return-type inventory per § 6.9; unaccounted entities are `CONTRADICTION` findings per R33.
11. Emit ART-11 per § 8 with full front-matter, per-data-type flow sections, master module-level diagram, sensitive-flow register, sanitization-point catalog, traceability index, open questions.
12. Run the Quality Checks in § 9.
13. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Significant Data-Type Identification

Identify significant data types by clustering the declared and inferred types emitted by ART-08 and ART-09. A significant data type is one that satisfies at least one of the following predicates: (a) the type is the parameter or return type of a public API function recorded in ART-09; (b) the type is a class field of a `K-XX` entity recorded in ART-08; (c) the type is the payload of an `E-XX` event detected by PROMPT_14 (forward reference: PROMPT_14 records `E-XX` payloads); (d) the type crosses a system boundary per § 6.4; (e) the type is persisted via a database or file write detected by PROMPT_20 (forward reference).

For each significant data type, assign a `D-XX` identifier and record: `name`, `language_type` (primitive | class | interface | struct | record | enum | alias | generic | union), `definition_location` (`file:line-range, symbol`), `carriers` (list of `FN-XX` whose signature mentions the type), `field_carriers` (list of `V-XX` fields of that type), `external: false` for in-scope types or `external: true` with `EXTERNAL` citation for library types. Primitive aliases such as `string` are NOT assigned `D-XX` identifiers individually; instead, the prompt records a single `D-PKT-STRING` aggregate entry and notes downstream flows by their semantic role rather than by primitive type.

Cluster structurally identical types (e.g., a TypeScript `interface UserDTO` and a Python `UserDTO` dataclass) only when a shared schema file (OpenAPI, Proto, JSON Schema) is detected in ART-04; absent a shared schema, treat each language-specific definition as a distinct `D-XX`.

### 6.2 Source and Sink Enumeration

Enumerate every data source and every data sink in the subject repository. A source is a site where data enters the in-scope code; a sink is a site where data leaves the in-scope code or crosses a trust boundary.

**Source categories** (record each with `source_id` `SRC-XX`, `category`, `symbol`, `file:line-range`, `data_type` `D-XX`):

- User input — HTTP request body/params/headers, CLI args, stdin reads, UI form inputs. Cross-reference ART-05's route registrations and CLI entries.
- External API responses — `fetch()`, `axios.get()`, `requests.get()`, `http.Client.Get()`, `HttpClient.SendAsync()`, gRPC client calls, GraphQL client queries. The call site is the source location.
- Database reads — `SELECT`, `Model.objects.get()`, `prisma.user.findUnique()`, `User.find()`, `repo.findById()`, `db.QueryRow()`. Cite the query call site.
- File reads — `fs.readFile()`, `open()`, `os.Open()`, `File.ReadAllText()`, `ifstream`. Cite the read call.
- Queue consumption — `channel.consume()`, `consumer.subscribe()`, `queue.process()`, `@KafkaListener`, `@RabbitListener`. Cite the consumer registration.
- Environment and config — `process.env`, `os.Getenv()`, `Environment.GetEnvironmentVariable()`, config-file reads (cross-reference ART-04). Treated as sources because the environment is outside the in-scope code.
- Random and time — `Math.random()`, `crypto.randomBytes()`, `time.Now()`, `DateTime.UtcNow`. Treated as sources because the values are non-deterministic.

**Sink categories** (record each with `sink_id` `SNK-XX`, `category`, `symbol`, `file:line-range`, `data_type` `D-XX`):

- HTTP responses — `res.send()`, `res.json()`, `Response.ok()`, `return ResponseEntity`. Cite the response call.
- External API requests — outbound HTTP/gRPC/GraphQL calls (the request payload is the sink).
- Database writes — `INSERT`, `UPDATE`, `DELETE`, `save()`, `persist()`, `prisma.user.create()`, `repo.save()`. Cite the write call.
- File writes — `fs.writeFile()`, `os.Create()`, `File.WriteAllText()`, `ofstream`.
- Queue publishes — `channel.publish()`, `producer.send()`, `kafkaTemplate.send()`. Cite the publish call.
- Logs — `console.log()`, `log.info()`, `logger.log()`, `print()`, `fmt.Println()`. Log sinks are flagged when they receive sensitive data per § 6.6.
- Process exits — `process.exit()`, `os.Exit()`, `Environment.Exit()`. The exit code is the sink datum.

### 6.3 Transformation Chain Tracing

For every significant data type `D-XX`, trace its journey from origin to terminal. The trace procedure is bounded by 30 call-hops and by ART-10's call-graph reachability set.

1. Identify every `SRC-XX` that produces a value of type `D-XX` (per § 6.2).
2. From each source, walk forward along ART-10's call edges, following the data through assignments, parameter passes, return-value propagations, and field writes.
3. At every hop, record a `flow_step` with `step_id`, `kind` (assign | param-pass | return-propagate | field-write | transform | validate | serialize | deserialize | sanitize | cross-boundary), `from_node` (`FN-XX` or `SRC-XX`), `to_node` (`FN-XX` or `SNK-XX`), `data_type` `D-XX`, `citation` (`file:line-range`), and `notes`.
4. Stop the trace when the data reaches a `SNK-XX` or when 30 hops elapse without reaching a sink (mark `FLOW_INCOMPLETE` with an Open Question).
5. When a transformation occurs (the data type changes — e.g., `UserDTO` → `UserEntity` via `toEntity()`), record the transformation function as a node and create a new flow segment with the transformed type.
6. When a flow branches (a value of type `D-XX` is passed to multiple callees), record each branch as a separate downstream flow segment with a shared ancestor.

The trace uses a worklist algorithm seeded by the source set; each worklist entry is `(current_FN, D-XX, hops_remaining)`. Cycles in the call graph (per ART-10's SCC catalog) are detected and the trace does not loop indefinitely — when a cycle is entered, the trace records `CYCLE_ENTERED` with the SCC ID and continues with the cycle's exit edges only.

### 6.4 Boundary-Crossing Detection

Detect every boundary crossing along each flow. A boundary crossing is a `cross-boundary` step recorded in the flow's step list. The categories are:

- **Process boundary** — IPC, RPC, subprocess invocation, message-queue send/receive. Detected by call patterns (`child_process.exec`, `subprocess.run`, `os.exec`, `Process.Start`, `spawn()`).
- **Network boundary** — HTTP, gRPC, WebSocket, raw TCP. Detected by client-call patterns enumerated in § 6.2's external-API source/sink category.
- **Persistence boundary** — database write/read, file write/read, cache write/read. Detected by persistence-API patterns; cross-reference PROMPT_20's persistence catalog (forward reference).
- **Serialization boundary** — `JSON.stringify()`, `JSON.parse()`, `pickle.dumps()`, `Marshal()`, `serde_json::to_string()`, `XmlSerializer.Serialize()`. The pre-serialization type and the post-serialization type are both recorded; the boundary is the serializer call.
- **Trust boundary** — entry from an untrusted source (user input, external API) into a trusted region. Trust boundaries are the most security-relevant crossings and are flagged for PROMPT_19 (auth) and the sensitive-data flow register (§ 6.6).

Each boundary crossing records `boundary_kind`, `crossing_symbol`, `crossing_citation`, `pre_boundary_type`, `post_boundary_type`.

### 6.5 Sanitization-Point Identification

Identify every sanitization and validation point along each flow. A sanitization point is a function call that takes potentially untrusted input and either returns trusted output, throws on invalid input, or both. A validation point is a function call that asserts properties of the input without transforming it.

**Sanitization patterns by language/ecosystem:**

- **Node.js** — `express-validator`'s `body('x').escape()`, `DOMPurify.sanitize()`, `validator.escape()`, `xss()`, Joi/Ajv `validate()` with sanitization, `mysql.escape()`, `sql.escape()`.
- **Python** — `bleach.clean()`, `markupsafe.escape()`, `html.escape()`, `urllib.parse.quote()`, `shlex.quote()`, Django form `clean()` methods, Marshmallow `load()` with `required=True`.
- **Go** — `html.EscapeString()`, `url.QueryEscape()`, `template.HTMLEscapeString()`, `regexp.QuoteMeta()`, prepared-statement parameterization (`db.Query(sql, args...)`).
- **Rust** — ` ammonia::clean()`, `htmlescape::encode_minimal()`, `url::form_urlencoded`, serde validation attributes.
- **Java** — `StringEscapeUtils.escapeHtml4()`, OWASP Encoder `Encode.forHtml()`, `Pattern.quote()`, JSoup `clean()`, Bean Validation `@Valid` + `Validator.validate()`.
- **C#** — `WebUtility.HtmlEncode()`, `Uri.EscapeDataString()`, `Regex.Escape()`, DataAnnotations `[RegularExpression]`, `[Range]`.
- **Ruby** — `CGI.escapeHTML()`, `ERB::Util.html_escape()`, `URI.encode_www_form_component()`, Rails `sanitize()`, Strong Parameters `params.require().permit()`.
- **PHP** — `htmlspecialchars()`, `filter_var()` with `FILTER_SANITIZE_*`, prepared statements via PDO/MySQLi.

Each sanitization point records `sanitize_id` `SAN-XX`, `kind` (escape | validate | parameterize | strip | transform), `target_field` (the input field name, when applicable), `attacking_vector` (XSS | SQLi | path-traversal | SSRF | command-injection | LDAP | NoSQL | generic), `function_id` `FN-XX`, `citation`.

### 6.6 Sensitive-Data Flow Flagging

Flag every flow that carries PII or other sensitive data. A flow is flagged when its `D-XX` data type or any field of its carrier type matches the sensitive-data lexicon (§ 6.6.1). Each flagged flow is recorded in a dedicated `sensitive_flows` register with `flow_id`, `data_type`, `sensitive_fields`, `source` `SRC-XX`, `sink` `SNK-XX`, `sanitization_points` (list of `SAN-XX`), `risk_assessment` (HIGH | MEDIUM | LOW based on whether sanitization or encryption is applied at every trust-boundary crossing).

#### 6.6.1 Sensitive-Data Lexicon

The sensitive-data lexicon is matched case-insensitively against field names, parameter names, and type names. The default lexicon includes the following stems; engagement-specific stems (e.g., domain-specific identifiers) are added when discovered and recorded as Open Questions for PROMPT_19 to consume.

- PII: `password`, `passwd`, `pwd`, `secret`, `token`, `jwt`, `apiKey`, `api_key`, `accessToken`, `refreshToken`, `sessionId`, `ssn`, `social_security`, `passport`, `driver_license`, `dob`, `date_of_birth`, `birth_date`.
- Financial: `creditCard`, `credit_card`, `cardNumber`, `cvv`, `pan`, `accountNumber`, `routing_number`, `iban`, `bic`.
- Health: `diagnosis`, `icd10`, `icd9`, `medical_record`, `phi`.
- Contact: `email`, `phone`, `phoneNumber`, `address`, `zipCode`, `postal_code`.
- Biometric: `fingerprint`, `retina`, `faceId`, `biometric`.

The lexicon is extendable per engagement; extensions are recorded in the artifact's Open Questions with rationale.

### 6.7 Module-Level Aggregation

Aggregate function-level flows to module-level flows for the master data-flow diagram. For every pair of modules `(M_a, M_b)`, compute the set of `D-XX` types that flow from any function in `M_a` to any function in `M_b` along the traced flows. Each module-level edge records `from_module` `M-XX`, `to_module` `M-XX`, `data_types` (list of `D-XX`), `flow_count`, `evidence_flows` (list of `flow_id`). Module-level flows that cross a trust boundary are highlighted in the master diagram with a distinct edge style (dashed red).

### 6.8 Mermaid Data-Flow Diagram Emission

Emit Mermaid `graph LR` diagrams per `OUTPUT_RULES.md` § 7. Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file at `<output_root>/diagrams/<engagement_id>_ART11_D-XX.mmd`.

- **Per-data-type flow diagram** — one diagram per significant data type with `≥ 3` flow steps. Nodes: `SRC-XX`, `FN-XX`, `SAN-XX`, `SNK-XX`. Edges labeled with the flow step's `kind` and the citation (`file:line`).
- **Sensitive-flow register diagram** — a single diagram highlighting every flagged sensitive flow with red edges; non-sensitive flows are omitted for readability.
- **Master module-level diagram** — `graph LR` with `M-XX` nodes; edges labeled with the count of `D-XX` types flowing; sensitive flows rendered as dashed red edges. Decomposed into sub-diagrams by module cluster when the master exceeds 30 nodes per `OUTPUT_RULES.md` § 7.5.

Edge styles: solid black for in-process flows, dashed blue for serialization boundaries, dashed green for persistence boundaries, dashed purple for network boundaries, dashed red for trust-boundary crossings involving sensitive data.

### 6.9 Coverage Cross-Check

Cross-check the data-flow node set against ART-09's parameter and return-type inventory:

1. Compute the set `P` of all `D-XX` types appearing as a parameter or return type in any `FN-XX` recorded by ART-09.
2. Compute the set `F` of all `D-XX` types appearing in at least one traced flow.
3. The expected relationship: `F ⊇ P ∩ significant` (every significant type appears in at least one flow). Types in `P` that are not in `F` are `COVERAGE_GAP` findings recorded in Open Questions; if the gap is structural (the type is referenced but never flows because the carrying function is dead code), cross-reference ART-09's `DEAD_CODE_CANDIDATE` flag.
4. Types in `F` that are not in `P` indicate a flow trace that introduced a type ART-09 did not record; this is a `CONTRADICTION` finding per R33.

---

## 7. Required Outputs

### ART-11 — Data Flow Diagrams

**Type:** Graph.

**Acceptance Criteria:**

- AC-11.1: The artifact file exists at `<output_root>/artifacts/phase2/ART11_<engagement_id>_data-flow.md`.
- AC-11.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.4 (Graph schema, extends Map).
- AC-11.3: The body contains: Executive Summary, Methodology, Significant Data Types, Sources and Sinks, Per-Data-Type Flow Diagrams, Sanitization Points, Sensitive-Data Flow Register, Master Module-Level Diagram, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-11.4: Every flow step cites its source line range.
- AC-11.5: Every Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-11.6: A `.mmd` sidecar file exists for every Mermaid block under `<output_root>/diagrams/`.
- AC-11.7: Every source and sink is cataloged with `category`, `symbol`, `file:line-range`, and `data_type`.
- AC-11.8: Every sanitization point records its `kind`, `target_field`, `attacking_vector`, and `function_id`.
- AC-11.9: Every sensitive flow records its source, sink, sanitization points, and `risk_assessment`.
- AC-11.10: Diagrams larger than 30 nodes are decomposed with a master index diagram.

---

## 8. Output Templates

### 8.1 ART-11 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-11
artifact_type: Graph
producing_prompt: PROMPT_11
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
significant_data_types:
  - id: D-01
    name: <type-name>
    language_type: primitive | class | interface | struct | record | enum | alias | generic | union
    definition_location: <file>:<line-range>
    carriers: [FN-XX]
    field_carriers: [V-XX]
    external: false
sources:
  - id: SRC-01
    category: user-input | external-api | database | file | queue | env | random
    symbol: <name>
    file: <path>
    line_range: <start-end>
    data_type: D-XX
sinks:
  - id: SNK-01
    category: http-response | external-api | database | file | queue | log | exit
    symbol: <name>
    file: <path>
    line_range: <start-end>
    data_type: D-XX
flows:
  - flow_id: FL-01
    data_type: D-XX
    source: SRC-XX
    sink: SNK-XX | INCOMPLETE
    steps:
      - step_id: FS-01
        kind: assign | param-pass | return-propagate | field-write | transform | validate | serialize | deserialize | sanitize | cross-boundary
        from_node: SRC-XX | FN-XX
        to_node: FN-XX | SNK-XX
        citation: <file>:<line-range>
        notes: <text>
    status: COMPLETE | FLOW_INCOMPLETE | CYCLE_ENTERED
sanitization_points:
  - id: SAN-01
    kind: escape | validate | parameterize | strip | transform
    target_field: <name>
    attacking_vector: XSS | SQLi | path-traversal | SSRF | command-injection | LDAP | NoSQL | generic
    function_id: FN-XX
    citation: <file>:<line-range>
sensitive_flows:
  - flow_id: FL-XX
    data_type: D-XX
    sensitive_fields: [<field-name>]
    source: SRC-XX
    sink: SNK-XX
    sanitization_points: [SAN-XX]
    risk_assessment: HIGH | MEDIUM | LOW
module_flows:
  - from_module: M-XX
    to_module: M-XX
    data_types: [D-XX]
    flow_count: <int>
    evidence_flows: [FL-XX]
coverage_cross_check:
  param_return_types_size: <int>
  flowed_types_size: <int>
  unflowed_significant_types: [D-XX]
  flowed_unrecorded_types: [D-XX]
  contradictions: [{ kind: <text>, type_id: D-XX, detail: <text> }]
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
nodes:
  - id: D-01
    label: <type-name>
    kind: data-type
    parent_id: null
    depth: 0
edges:
  - from: SRC-01
    to: FN-01
    relationship: FLOWS_TO
    evidence: <file_path>:<line-range>
layout_hint: LR
mermaid_source: |
  graph LR
      SRC01[SRC-01: HTTP body] --> FN01[FN-01: parseBody]
      FN01 --> SAN01[SAN-01: validateBody]
      SAN01 --> SNK01[SNK-01: db.save]
      %% edge: src/handler.ts:42
---
```

### 8.2 ART-11 Body Skeleton

```markdown
# ART-11: Data Flow Diagrams

## 1. Executive Summary
## 2. Methodology
## 3. Significant Data Types
## 4. Sources and Sinks
   ### 4.1 Sources
   ### 4.2 Sinks
## 5. Per-Data-Type Flow Diagrams
   ### 5.1 D-01: <type-name>
   **Diagram D-01: D-01 Flow**
   ```mermaid
   graph LR
       SRC01[SRC-01: req.body] --> FN01[FN-01: parseBody]
       FN01 --> SAN01[SAN-01: validate]
       SAN01 --> SNK01[SNK-01: db.save]
       %% edge: src/handler.ts:42
   ```
   <ordered step list>
## 6. Sanitization Points
## 7. Sensitive-Data Flow Register
   **Diagram D-02: Sensitive Flows**
## 8. Master Module-Level Diagram
   **Diagram D-03: Module-Level Data Flow**
## 9. Coverage Cross-Check
## 10. Traceability Index
## 11. Open Questions
## 12. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every significant data type has at least one traced flow or is recorded `UNFLOWED` with rationale. Threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of flow steps cited.
- **Q3. Schema Conformance Check** — validates against § 4.4.
- **Q4. Non-Contradiction Check** — no flow edge contradicts ART-10's call graph (every `from_node → to_node` pair must be a valid call edge or a source/sink-to-function edge).
- **Q5. UNVERIFIED Accounting** — every `FLOW_INCOMPLETE`, `CYCLE_ENTERED`, and `UNFLOWED` entry has a corresponding Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.3 on a 5% sample of flows yields the same step list.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-11.A. Source/Sink Catalog Completeness** — every entry-point function in ART-05 has its input parameters mapped to a `SRC-XX` and its output (return value, response, side-effect write) mapped to a `SNK-XX`.
- **Q-11.B. Sanitization-Point Coverage** — every flow crossing a trust boundary (per § 6.4) from an untrusted source has at least one `SAN-XX` recorded before the first trusted-region node, or is flagged in `sensitive_flows` with `risk_assessment: HIGH`.
- **Q-11.C. Sensitive-Flow Coverage** — every `D-XX` matching the § 6.6.1 lexicon appears in `sensitive_flows` or has an Open Question explaining why it was excluded.
- **Q-11.D. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment per `OUTPUT_RULES.md` § 7.5.
- **Q-11.E. Sidecar Files** — every Mermaid block has a corresponding `.mmd` file under `<output_root>/diagrams/`.
- **Q-11.F. Hop Budget Enforcement** — no flow exceeds 30 hops without being marked `FLOW_INCOMPLETE` with an Open Question.
- **Q-11.G. Cycle Handling** — every cycle entered is recorded with the SCC ID from ART-10 and does not produce duplicate step entries.

---

## 10. Common Pitfalls

- Do not treat primitive types (`string`, `int`) as significant data types individually; aggregate them and document flows by semantic role rather than by primitive.
- Always cite the call site that produces a source value; an uncited source is a `CITATION_GAP` per Q2.
- Do not infer data flow from comments or README descriptions; trace the actual call graph per R22.
- Always cap flow traces at 30 hops; unbounded traces recurse through cycles and exhaust the token budget per R29.
- Do not omit sanitization points that fail (e.g., a validation that throws); the throw site is itself a sanitization event and must be recorded.
- Always flag trust-boundary crossings involving sensitive data as `HIGH` risk in the sensitive-flow register; omitting the flag defeats the security downstream consumption by PROMPT_19.
- Do not collapse two distinct data types because they share a name across languages; absent a shared schema file, treat them as distinct `D-XX` entities.
- Always cross-check the flow node set against ART-09's parameter inventory; an unaccounted type is a real contradiction per R33, not a stylistic gap.
- Do not record a `cross-boundary` step without recording the pre- and post-boundary types; the type change is the evidentiary signal of the boundary.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders the diagrams from the sidecar source.
- Do not include external callees (library functions) as nodes in the flow graph; they belong in `external_call_edges` per ART-10 and are referenced from `SRC-XX`/`SNK-XX` entries.

---

## 11. Handoff Criteria

PROMPT_13, PROMPT_21, and PROMPT_25 consume ART-11. Handoff requires ALL of:

- HC-11.1: ART-11 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-11.2: Every significant data type has at least one traced flow or is recorded `UNFLOWED` with rationale.
- HC-11.3: Every entry point in ART-05 has its inputs and outputs mapped to sources and sinks.
- HC-11.4: Every trust-boundary crossing in a sensitive flow has a sanitization point or a `HIGH` risk flag.
- HC-11.5: Mermaid diagrams are emitted with `.mmd` sidecar files; diagrams > 30 nodes are decomposed.
- HC-11.6: Coverage cross-check is recorded with no unresolved contradictions.
- HC-11.7: `repository_fingerprint_recheck` matches ART-01.
- HC-11.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_13 (State Management — uses data-flow traces to identify state mutations and persisted state), PROMPT_21 (AI/LLM Workflow — uses data flows to identify prompt inputs and LLM-output sinks), PROMPT_25 (Diagram Generation — re-renders the Mermaid sources at higher visual fidelity).
- **Depends on:** ART-01 (PROMPT_01), ART-06 (PROMPT_06), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33 (contradiction escalation between ART-11 flow types and ART-09 parameter types).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.4; Graph bar (aggregate ≥ 30, Coverage ≥ 4, Coherence ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4 (sidecar files), § 3.1, § 4, § 6, § 7 (Mermaid graph conventions, edge citations, ≤ 30 nodes, decomposition).
- **Forward reference:** PROMPT_19 (Authentication & Authorization) consumes the `sensitive_flows` register to validate that every authenticated API endpoint's sensitive flows are protected; PROMPT_28 (Cross-Reference Checklists) verifies that every `D-XX` referenced by ART-13, ART-15, or ART-21 resolves to an entry in ART-11.

*End of PROMPT_11. Orchestrator may dispatch PROMPT_12 upon satisfaction of § 11.*
