# PROMPT_25.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_25: Diagram Generation (Mermaid, UML, Sequence, Flowchart)

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_25
- **Phase:** 3
- **Stage:** 5 of 5 (Phase 3 capstone)
- **Dependencies:** ART-01 through ART-24 (all prior Phase 1, Phase 2, and Phase 3 artifacts).
- **Estimated Tokens:** 18000–28000
- **Output Artifacts:** ART-25 (Diagrams) — Canonical Diagram Set.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Canonical Diagram Set artifact (ART-25) that synthesizes every prior artifact into the engagement's complete visual representation — System Context (C4 L1), Container Diagram (C4 L2), Component Diagrams (C4 L3), Folder/Module Tree, Call Graph, Dependency Graph, Data Flow Diagrams, Sequence Diagrams for every major workflow, State Machine Diagrams for every stateful unit, ER Diagram (when persistence exists), Class Diagram for the core domain, and Deployment Diagram (when IaC exists) — emitted as Mermaid source with edge-level citations and sidecar files, with diagrams exceeding 30 nodes decomposed into sub-diagrams per `OUTPUT_RULES.md` § 7.5.

---

## 3. When to Invoke

PROMPT_25 is dispatched when ALL of the following predicates hold:

- Phase 2 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.3.
- PROMPT_21, PROMPT_22, PROMPT_23, and PROMPT_24 have emitted their completion records. ART-21, ART-22, ART-23, and ART-24 may be `NOT_PRODUCED` under skipped behavior; PROMPT_25 MUST degrade gracefully by skipping the diagrams that depend on the absent artifact and recording Open Questions.
- ART-01 through ART-24 (where produced) are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- At least one prior artifact contains a Mermaid source entry (`mermaid_sources` non-empty in any of ART-11 through ART-24); this guarantees a non-empty diagram set. Otherwise the prompt emits `BLOCKED` with `INPUT_GAP`.

As the Phase 3 capstone, PROMPT_25's `status: SUCCESS` (or `SKIPPED` under conditions in § 3.1) satisfies the Phase 3 exit conditions and authorizes the orchestrator to begin Phase 4 per `PROJECT_SPECIFICATION.md` § 5.4.

### 3.1 Skipped Behavior (No Diagrams Possible)

If all upstream artifacts that produce diagrams (ART-11, ART-12, ART-13, ART-14, ART-15, ART-16, ART-17, ART-20, ART-21, ART-22, ART-23, ART-24) are absent (e.g., engagement under `SCOPE_TRIAGE` halted before Phase 2), the prompt emits a `SKIPPED` completion record per the format in `MASTER_PROMPT.md` § 6 and halts. This is a degenerate case; under normal engagement conditions, PROMPT_25 always produces ART-25.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-02 | Manifest | Tech stack & dependencies; container-diagram (C4 L2) containers are derived from the stack. Deployment diagram's technologies are derived from the stack. |
| ART-03 | Map | Folder & file tree; the Folder/Module Tree diagram is rendered from this. |
| ART-04 | Spec | Build & configuration; Deployment Diagram's environment topology (CI/CD pipelines, IaC) is derived from this. |
| ART-05 | Map | Entry points & bootstrap; sequence diagrams' entry-point lifelines are derived from this. |
| ART-06 | Map | Module map; Component Diagram (C4 L3) components and module boundaries. |
| ART-07 | Map | Component map; Component Diagram component granularity. |
| ART-08 | Doc | Class & interface catalog; Class Diagram is rendered from this. |
| ART-09 | Doc | Function catalog; sequence-diagram activations and Call Graph nodes. |
| ART-10 | Graph | Call & dependency graphs; the Call Graph diagram (top-level + sub-graphs) is rendered from this. |
| ART-11 | Graph | Data flow diagrams; the DFD per significant data type is re-rendered at engagement-level visual fidelity. |
| ART-12 | Graph | Control flow & execution paths; sequence-diagram control-flow alt/opt blocks are derived from this. |
| ART-13 | Doc | State machine catalog; State Machine Diagrams are rendered from this. |
| ART-14 | Doc | Event workflow catalog; event-driven sequence diagrams are derived from this. |
| ART-15 | Doc | API & interface reference; API sequence diagrams and contract validation diagrams. |
| ART-16 | Doc | Middleware & pipeline map; request-lifecycle sequence diagrams. |
| ART-17 | Doc | Error handling; sequence-diagram error-path opt blocks. |
| ART-18 | Doc | Caching & performance; caching strategy diagrams. |
| ART-19 | Doc | Auth report (optional); auth-flow sequence diagrams. |
| ART-20 | Doc | Persistence report (optional); ER Diagram is rendered from this when present. |
| ART-21 | Doc | AI/LLM workflow report; AI-workflow sequence diagrams are rendered from this when present. |
| ART-22 | Doc | Streaming workflow report; streaming-workflow sequence diagrams are rendered from this when present. |
| ART-23 | Doc | Design pattern catalog; pattern-instance class diagrams. |
| ART-24 | Doc | Engineering decision record; decision-context diagrams. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid conventions (§ 7) including diagram types by use case (§ 7.4) and diagram quality (§ 7.5: ≤ 30 nodes, edge labels, edge citations, decomposition). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Diagrams schema (§ 4.1 + Diagrams type) and type-specific bar (aggregate ≥ 30, Coverage ≥ 4, Coherence ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Enumerate the required diagram set per § 6.1 (the 12 canonical diagram types).
3. For each diagram type, generate Mermaid source per § 6.2 through § 6.13.
4. Apply the citation-embedding procedure per § 6.14 — every edge in every graph/diagram carries an `edge: file:line` comment.
5. Apply the decomposition heuristic per § 6.15 — any diagram exceeding 30 nodes is decomposed into sub-diagrams with a master index diagram.
6. Emit sidecar files per § 6.16 — every Mermaid block has a corresponding `.mmd` file under `<output_root>/diagrams/`.
7. Emit a rendered `.svg` for every diagram when an adapter is available; otherwise record `RENDERING_SKIPPED` per diagram in `mermaid_sources`.
8. Cross-check the diagram set against the entity registry per § 6.17 — every entity referenced by a diagram MUST resolve to an upstream artifact.
9. Emit ART-25 per § 8 with full front-matter, per-diagram-type sections, the diagram catalog, the sidecar file manifest, traceability index, open questions.
10. Run the Quality Checks in § 9.
11. Emit the Completion Record per `MASTER_PROMPT.md` § 6; this Completion Record also serves as the Phase 3 exit signal.

---

## 6. Analysis Procedures

### 6.1 Required Diagram Set Enumeration

Enumerate the canonical diagram set. Every engagement MUST produce at least the diagrams that have applicable inputs. The 12 canonical diagram types are:

1. **System Context (C4 L1)** — per § 6.2.
2. **Container Diagram (C4 L2)** — per § 6.3.
3. **Component Diagram (C4 L3)** — per § 6.4, one per major component.
4. **Folder/Module Tree** — per § 6.5.
5. **Call Graph (top-level, with sub-graphs for hotspots)** — per § 6.6.
6. **Dependency Graph** — per § 6.7.
7. **Data Flow Diagram (per significant data type)** — per § 6.8.
8. **Sequence Diagrams for every major workflow** — per § 6.9 (request lifecycle, AI workflow, event workflow).
9. **State Machine Diagrams for every stateful unit** — per § 6.10.
10. **ER Diagram (when persistence exists)** — per § 6.11.
11. **Class Diagram for the core domain** — per § 6.12.
12. **Deployment Diagram (when IaC exists)** — per § 6.13.

Each diagram type is mandatory when its input artifacts are present. When the input is absent (e.g., no persistence → no ER diagram), the diagram is `NOT_APPLICABLE` and recorded with rationale.

### 6.2 System Context Diagram (C4 L1)

Generate a Mermaid `C4Context` (or `flowchart LR` if C4 plugin is unavailable) showing the subject system as a single box at the center, with external actors (users, external systems) and their interactions. Nodes: `System`, `Actor_XX`, `External_XX`. Edges labeled with the interaction kind (HTTP, gRPC, async, file). The system box records the system's name (from ART-01), version (when declared), and one-sentence description. External actors are derived from ART-05's entry-point callers and ART-15's external-API clients.

Diagram ID: `D-CTX-01`. Sidecar file: `<engagement_id>_ART25_D-CTX-01.mmd`.

### 6.3 Container Diagram (C4 L2)

Generate a Mermaid `C4Container` (or `flowchart LR`) showing every container (deployable unit) in the system: web frontend, API server, background worker, database, cache, message broker. Containers are derived from ART-02's stack and ART-04's deployment configuration. Each container records: name, technology, responsibility (one sentence). Edges between containers are labeled with the protocol (HTTP, gRPC, JDBC, AMQP, etc.) and the call frequency (when inferable from ART-10's call graph). External systems (per ART-15) appear as `ExternalSystem` nodes.

Diagram ID: `D-CONT-01`. Sidecar file: `<engagement_id>_ART25_D-CONT-01.mmd`. Decompose by deployment environment (dev, staging, prod) when ART-04 records multiple environments.

### 6.4 Component Diagram (C4 L3)

Generate one Mermaid `C4Component` (or `flowchart LR`) per major component (per ART-06's modules and ART-07's components). Each diagram shows the component's internal structure: sub-components, their technologies, their responsibilities, and their dependencies on other components and external interfaces. Each component records: name, technology, responsibility. Edges are labeled with the relationship kind per ART-10's relationship types (`CALLS`, `IMPORTS`, `DEPENDS_ON`, `IMPLEMENTS`).

Diagram IDs: `D-COMP-01` through `D-COMP-NN`, one per major component. Sidecar files accordingly. The master index diagram `D-COMP-00` shows the component-to-diagram mapping.

### 6.5 Folder/Module Tree Diagram

Generate a Mermaid `graph TD` showing the folder tree (per ART-03) with module-grouping wrappers (per ART-06). Each folder node records its role (per ART-03's folder-purpose inference). Files are not shown individually unless they are entry points (`F-XX` from ART-05) or barrel files. The tree is pruned to the top 4 levels; deeper levels are referenced by "see ART-03 for full tree" notes.

Diagram ID: `D-TREE-01`. Sidecar file: `<engagement_id>_ART25_D-TREE-01.mmd`. Decompose by top-level directory when the tree exceeds 30 nodes.

### 6.6 Call Graph Diagram

Generate a Mermaid `graph LR` showing the top-level call graph from ART-10. Nodes are functions (`FN-XX`) or modules (`M-XX`) at the top level. Edges are `CALLS` relationships with citations. Hotspots (functions with high fan-in or high fan-out, per ART-10's hotspot catalog) are expanded into sub-graphs, each with its own diagram ID `D-CG-XX`.

The top-level diagram `D-CG-01` shows the call graph at module granularity (every `M-XX` is a node, every inter-module call is an edge). For each hotspot module, a sub-diagram `D-CG-02`, `D-CG-03`, etc. shows the function-level call graph within that module.

### 6.7 Dependency Graph

Generate a Mermaid `graph LR` showing the dependency graph from ART-10's `external_call_edges` and `dependency_edges`. Nodes are modules (`M-XX`) and external dependencies (`DEP-XX`). Edges are `DEPENDS_ON` and `IMPORTS` relationships. External dependencies are color-coded by ecosystem (npm, pip, maven, etc.). Cycles in the dependency graph are highlighted.

Diagram ID: `D-DEP-01`. Sidecar file accordingly. Decompose by module cluster when > 30 nodes.

### 6.8 Data Flow Diagrams

For each significant data type (`D-XX`) in ART-11, generate a Mermaid `graph LR` showing the flow from source to sink through transformation, sanitization, and boundary-crossing nodes. Nodes: `SRC-XX`, `FN-XX`, `SAN-XX`, `SNK-XX`. Edges labeled with the flow step's `kind` and citation. These diagrams are re-rendered at engagement-level visual fidelity from ART-11's per-data-type diagrams (which may have been emitted as smaller diagrams per the hop budget).

Diagram IDs: `D-DF-01` through `D-DF-NN`, one per significant data type with `≥ 3` flow steps. The sensitive-flow register diagram `D-DF-00` highlights every flagged sensitive flow with red edges. The master module-level diagram `D-DF-Master` shows the module-level data flow.

### 6.9 Sequence Diagrams for Major Workflows

Generate Mermaid `sequenceDiagram` diagrams for every major workflow. The canonical workflow set:

- **Request lifecycle sequence** — for each major API endpoint (`A-XX` from ART-15), a sequence diagram showing client → middleware chain → controller → service → repository → response. Lifelines: client, middleware, controller, service, repository, external-API. Use `alt` blocks for branching, `opt` blocks for error paths (from ART-17), `loop` blocks for retries. Diagram IDs: `D-SEQ-REQ-XX`, one per major endpoint.
- **AI workflow sequence** (when ART-21 is present) — for each AI workflow (`W-XX`), a sequence diagram showing input → prompt-render → LLM-call → response-parse → tool-call (when applicable) → output. Lifelines: client, prompt-render, LLM, tool, memory. Use `loop` blocks for agent iterations, `alt` blocks for tool-call vs no-tool-call branches. Diagram IDs: `D-SEQ-AI-XX`, one per AI workflow.
- **Event workflow sequence** (when ART-14 is present) — for each event-driven workflow, a sequence diagram showing emitter → transport → handler → side-effect. Lifelines: emitter, transport (Kafka/RabbitMQ/etc.), handler, downstream. Use `par` blocks for parallel handlers. Diagram IDs: `D-SEQ-EVT-XX`, one per event workflow.
- **Auth flow sequence** (when ART-19 is present) — for each auth flow, a sequence diagram showing client → auth middleware → identity provider → token validation → resource access. Diagram IDs: `D-SEQ-AUTH-XX`, one per auth flow.
- **Streaming workflow sequence** (when ART-22 is present) — for each streaming workflow (`SW-XX`), a sequence diagram showing producer → operator-chain → consumer with backpressure annotations. Diagram IDs: `D-SEQ-STR-XX`.

Each sequence diagram cites the originating artifact (ART-15/ART-21/ART-14/ART-19/ART-22) and the line ranges that ground each message.

### 6.10 State Machine Diagrams

For each stateful unit (`S-XX` state machine in ART-13), generate a Mermaid `stateDiagram-v2` showing the states, transitions, initial state, and terminal states. Each transition is labeled with the trigger and guard (when present). Reachability per HOOK-03 is visualized: unreachable states are highlighted in red; states without a terminal path are highlighted in orange.

Diagram IDs: `D-STM-XX`, one per state machine. The master state-machine index `D-STM-00` lists all state machines.

### 6.11 ER Diagram (when ART-20 is present)

When ART-20 is present, generate a Mermaid `erDiagram` per module (or per bounded context) showing entities, fields, and relations. Each entity is rendered as `ENT_XX { type field_name }`. Each relation is rendered as `ENT_XX ||--o{ ENT_YY : "relation_name"`. The full schema diagram is decomposed into sub-diagrams when > 30 entities per `OUTPUT_RULES.md` § 7.5.

When ART-20 is `ABSENT`, mark the ER diagram as `NOT_APPLICABLE` with rationale: "No persistence code detected per ART-20; ER diagram not produced."

Diagram IDs: `D-ER-XX`, one per module with persistence.

### 6.12 Class Diagram for Core Domain

Generate a Mermaid `classDiagram` showing the core domain's classes, interfaces, and their relationships. "Core domain" is determined by the modules with the highest entity density (per ART-08's class catalog and ART-06's module map). The diagram shows:

- Classes (`K-XX`) with their key fields and methods (top 5 per class by importance).
- Interfaces (`I-XX`) with their method signatures.
- Inheritance edges (`K-XX --|> K-YY` for `EXTENDS`).
- Implementation edges (`K-XX ..|> I-XX` for `IMPLEMENTS`).
- Association edges (`K-XX --> K-YY` for "uses" relationships per ART-10).
- Composition edges (`K-XX *-- K-YY` for "owns" relationships).
- Pattern-instance annotations (per ART-23) — classes participating in patterns are annotated with the pattern name.

Diagram IDs: `D-CLS-XX`, one per major sub-domain. The master class diagram `D-CLS-00` shows the highest-level structure.

### 6.13 Deployment Diagram (when IaC exists)

When ART-04 records IaC (Terraform, CloudFormation, Pulumi, Kubernetes manifests, Docker Compose), generate a Mermaid `flowchart TD` showing the deployment topology: hosts, containers, networks, volumes, load balancers, external services. Each node records its technology (per ART-04's IaC detection). Edges represent network connections, volume mounts, and dependency orders.

When no IaC is detected, mark the deployment diagram as `NOT_APPLICABLE` with rationale: "No IaC detected per ART-04; deployment diagram not produced."

Diagram ID: `D-DEPL-01`. Sidecar file accordingly.

### 6.14 Citation-Embedding Procedure

Every edge in every graph/diagram MUST carry an `edge: file:line` comment per `OUTPUT_RULES.md` § 7.5. The procedure:

1. For each edge, locate the source line that establishes the edge in the upstream artifact.
2. Append a Mermaid comment `%% edge: <file>:<line>` immediately after the edge definition.
3. For edges derived from aggregate claims (per `OPERATING_RULES.md` R20), cite the enumeration procedure: `%% edge: enumeration: all files matching src/services/**/*.ts, 42 matches`.
4. For edges that cannot be cited (e.g., C4 L1 external-actor edges inferred from ART-15's external-API clients), use `%% edge: inferred-from: ART-15:external_clients` and record an Open Question.

Diagrams without edge citations fail Q-25.E and are `BLOCKING`.

### 6.15 Decomposition Heuristic

Apply the decomposition heuristic to any diagram exceeding 30 nodes:

1. Count the nodes in the diagram. If `≤ 30`, no decomposition.
2. If `> 30`, identify a natural clustering key: module (`M-XX`), data type (`D-XX`), or workflow (`W-XX`).
3. Emit one sub-diagram per cluster, with diagram ID `<parent-id>-<cluster-id>`.
4. Emit a master index diagram showing the parent diagram's nodes replaced by cluster nodes, with edges between clusters.
5. The master index diagram MUST have `≤ 30` nodes.
6. Record the decomposition in the diagram's `notes` field.

Decomposition MAY be recursive (a sub-diagram exceeding 30 nodes is further decomposed) up to 2 levels deep. Deeper decomposition indicates the diagram type is too coarse; record an Open Question recommending a different diagram type.

### 6.16 Sidecar File Emission

Every Mermaid block in ART-25 has a corresponding `.mmd` sidecar file under `<output_root>/diagrams/`. The sidecar file's name follows `OUTPUT_RULES.md` § 7.3: `<engagement_id>_ART25_<diagram_id>.mmd`. The sidecar file contains only the Mermaid source (no caption, no surrounding prose).

When an SVG adapter is available (per `OUTPUT_RULES.md` § 2.4), also emit `<engagement_id>_ART25_<diagram_id>.svg`. When no adapter is available, record `RENDERING_SKIPPED` in the diagram's `rendering_status` field with rationale.

### 6.17 Coverage Cross-Check

Cross-check the diagram set against the entity registry:

1. Compute `E_diagrams` = set of all entity IDs referenced by any diagram in ART-25.
2. Compute `E_registry` = set of all entity IDs declared in ART-01 through ART-24.
3. Expected: `E_diagrams ⊆ E_registry` (every diagram entity is a real entity). Entities in `E_diagrams \ E_registry` are `CONTRADICTION` findings per R33 (a diagram references an entity no upstream artifact declared).
4. For each `W-XX`, `SW-XX`, `AG-XX`, `RAG-XX`, `S-XX`, `D-XX` referenced by a sequence diagram, verify the entity exists in the corresponding upstream artifact.
5. Compute `C_diagrams` = set of entity IDs from the registry that appear in at least one diagram. Coverage fraction = `|C_diagrams| / |E_registry|`. The threshold per Q1 is ≥ 0.70 (lower than other prompts because not every entity warrants a diagram; the threshold reflects visual-relevance filtering, not omission).

---

## 7. Required Outputs

### ART-25 — Canonical Diagram Set

**Type:** Diagrams.

**Acceptance Criteria:**

- AC-25.1: The artifact file exists at `<output_root>/artifacts/phase3/ART25_<engagement_id>_diagrams.md`.
- AC-25.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and the Diagrams type extension.
- AC-25.3: The body contains: Executive Summary, Methodology, Diagram Catalog (with per-type sections), Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-25.4: Every applicable diagram type from § 6.1 is emitted OR marked `NOT_APPLICABLE` with rationale.
- AC-25.5: Every Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-25.6: A `.mmd` sidecar file exists for every Mermaid block under `<output_root>/diagrams/`.
- AC-25.7: Every edge in every graph/diagram carries an `edge: file:line` comment.
- AC-25.8: Every diagram with > 30 nodes is decomposed into sub-diagrams with a master index.
- AC-25.9: Coverage cross-check is recorded with no unresolved contradictions.

---

## 8. Output Templates

### 8.1 ART-25 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-25
artifact_type: Diagrams
producing_prompt: PROMPT_25
phase: 3
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
diagrams:
  - diagram_id: D-CTX-01
    type: c4-context | c4-container | c4-component | folder-tree | call-graph | dependency-graph | data-flow | sequence | state-machine | er | class | deployment | pattern-instance | decision-context
    title: <text>
    source_artifacts: [ART-XX]
    node_count: <int>
    decomposed: true | false
    sub_diagrams: [D-XX]
    master_index: D-XX | NA
    sidecar_file: <relative-path>
    rendering_status: rendered-svg | rendering-skipped | pending
    rendering_rationale: <text> | NA
    applicable: true | false
    not_applicable_rationale: <text> | NA
    notes: <text>
mermaid_sources:
  - diagram_id: D-CTX-01
    title: <text>
    sidecar_file: <relative-path>
    node_count: <int>
    edge_count: <int>
coverage_cross_check:
  entities_in_diagrams: [F-XX | K-XX | FN-XX | M-XX | D-XX | S-XX | W-XX | SW-XX | AG-XX | RAG-XX | ...]
  entities_in_registry: [F-XX | K-XX | FN-XX | M-XX | D-XX | S-XX | W-XX | SW-XX | AG-XX | RAG-XX | ...]
  entities_in_diagrams_not_in_registry: [<id>]
  coverage_fraction: <0..1>
  contradictions: [{ kind: <text>, entity: <id>, diagram: D-XX, detail: <text> }]
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

### 8.2 ART-25 Body Skeleton

```markdown
# ART-25: Canonical Diagram Set

## 1. Executive Summary
## 2. Methodology
## 3. Diagram Catalog
   ### 3.1 System Context (C4 L1)
   **Diagram D-CTX-01: System Context**
   ```mermaid
   C4Context
       Person(user, "End User", "A user of the system")
       System(sys, "Subject System", "The reverse-engineered system")
       System_Ext(ext, "External API", "Third-party service")
       Rel(user, sys, "Uses", "HTTPS")
       Rel(sys, ext, "Calls", "HTTPS")
       %% edge: inferred-from: ART-15:external_clients
   ```
   ### 3.2 Container Diagram (C4 L2)
   **Diagram D-CONT-01: Containers**
   ### 3.3 Component Diagrams (C4 L3)
   **Diagram D-COMP-01: <component>**
   ### 3.4 Folder/Module Tree
   **Diagram D-TREE-01: Folder Tree**
   ### 3.5 Call Graph
   **Diagram D-CG-01: Top-Level Call Graph (Module Level)**
   **Diagram D-CG-02: Hotspot <module> (Function Level)**
   ### 3.6 Dependency Graph
   **Diagram D-DEP-01: Dependency Graph**
   ### 3.7 Data Flow Diagrams
   **Diagram D-DF-01: D-XX Flow**
   **Diagram D-DF-00: Sensitive Flow Register**
   **Diagram D-DF-Master: Module-Level Data Flow**
   ### 3.8 Sequence Diagrams
   **Diagram D-SEQ-REQ-01: Request Lifecycle for <endpoint>**
   **Diagram D-SEQ-AI-01: AI Workflow W-XX**
   **Diagram D-SEQ-EVT-01: Event Workflow <event>**
   ### 3.9 State Machine Diagrams
   **Diagram D-STM-01: <stateful-unit>**
   ### 3.10 ER Diagram
   (or "### 3.10 ER Diagram — NOT_APPLICABLE: ART-20 absent")
   ### 3.11 Class Diagram
   **Diagram D-CLS-00: Core Domain Class Diagram**
   ### 3.12 Deployment Diagram
   (or "### 3.12 Deployment Diagram — NOT_APPLICABLE: no IaC detected")
## 4. Coverage Cross-Check
## 5. Traceability Index
## 6. Open Questions
## 7. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — coverage_fraction of entities appearing in at least one diagram ≥ 0.70 (lower threshold than other prompts due to visual-relevance filtering).
- **Q2. Citation Check** — ≥ 0.95 of diagram edges cited.
- **Q3. Schema Conformance Check** — validates against the Diagrams type extension.
- **Q4. Non-Contradiction Check** — no diagram asserts an edge that contradicts an upstream artifact's edges (e.g., a call-graph diagram with an edge not in ART-10).
- **Q5. UNVERIFIED Accounting** — every `inferred-from` edge citation, every `NOT_APPLICABLE` diagram, and every `RENDERING_SKIPPED` entry has a corresponding Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.2 on a 5% sample of diagrams yields the same node and edge set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-25.A. Diagram-Type Coverage** — every applicable diagram type from § 6.1 is emitted OR marked `NOT_APPLICABLE` with rationale. A missing applicable diagram type is a `BLOCKING` finding.
- **Q-25.B. Sidecar File Completeness** — every Mermaid block has a corresponding `.mmd` file under `<output_root>/diagrams/`. A missing sidecar file is a `BLOCKING` finding.
- **Q-25.C. Decomposition Enforcement** — no diagram exceeds 30 nodes without being decomposed. A diagram > 30 nodes is a `BLOCKING` finding per `OUTPUT_RULES.md` § 7.5.
- **Q-25.D. Caption Presence** — every Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption. A diagram without a caption is a `MAJOR` finding per `OUTPUT_RULES.md` § 7.2.
- **Q-25.E. Edge Citation** — every edge in every graph/diagram carries an `edge: file:line` (or `edge: inferred-from: ...`) comment. An uncited edge is a `MAJOR` finding.
- **Q-25.F. Sequence-Diagram Lifeline Grounding** — every lifeline in a sequence diagram resolves to a real entity (`A-XX`, `FN-XX`, `M-XX`, `CL-XX`, `W-XX`, `E-XX`, `SW-XX`). Unresolved lifelines are `CONTRADICTION` findings per R33.
- **Q-25.G. State-Machine HOOK-03 Visual Verification** — every state machine diagram highlights unreachable states (red) and no-terminal states (orange) per HOOK-03. Absent highlighting when violations exist is a `MAJOR` finding.
- **Q-25.H. ER-Diagram Persistence Closure** — when ART-20 is present, every entity (`ENT-XX`) appears in at least one ER diagram. Missing entities are `COVERAGE_GAP` findings.
- **Q-25.I. Class-Diagram Pattern Annotation** — every class participating in a pattern (per ART-23) is annotated with the pattern name in the class diagram. Missing annotations are `MINOR` findings.
- **Q-25.J. Master-Index Presence** — every decomposed diagram set has a master index diagram with `≤ 30` nodes. A missing or oversized master index is a `MAJOR` finding.

---

## 10. Common Pitfalls

- Do not omit edge citations; uncited edges fail Q-25.E and are `MAJOR`. Use `edge: inferred-from: ...` for edges that cannot be cited to a single line.
- Always decompose diagrams > 30 nodes; oversized diagrams fail Q-25.C and are `BLOCKING`.
- Always emit `.mmd` sidecar files; the Markdown embed is for human reading, the sidecar is for re-rendering by downstream consumers (PROMPT_29's export adapter).
- Do not invent entities in diagrams; every node must resolve to an upstream artifact's entity per Q-25.F. Inventing entities violates R21.
- Always mark `NOT_APPLICABLE` diagrams with rationale; an absent diagram without rationale is treated as a missing diagram and is `BLOCKING` per Q-25.A.
- Always emit a master index for decomposed diagram sets; a decomposition without a master index fails Q-25.J.
- Do not over-decompose; diagrams with < 5 nodes do not warrant their own diagram ID. Merge small diagrams into a parent diagram.
- Always cite the upstream artifact in the diagram's `source_artifacts` field; a diagram without source attribution cannot be cross-checked per Q4.
- Do not render `.svg` files when no adapter is available; instead record `RENDERING_SKIPPED` with rationale. Attempting to render without an adapter produces broken output.
- Always highlight HOOK-03 violations in state machine diagrams; the visual highlighting is the QA-readable signal that PROMPT_30 will look for.
- Do not include every entity in the class diagram; the class diagram is for the core domain. Including every class produces a > 30-node diagram that requires decomposition, defeating the purpose.
- Always annotate pattern participation in the class diagram; the annotation is the visual link between ART-23 (patterns) and ART-08 (classes).

---

## 11. Handoff Criteria

PROMPT_26, PROMPT_27, and PROMPT_29 consume ART-25. Handoff requires ALL of:

- HC-25.1: ART-25 status is `REVIEWED` or `DRAFT` with orchestrator waiver, OR `SKIPPED` per § 3.1 with downstream degradation declared.
- HC-25.2: Every applicable diagram type from § 6.1 is emitted OR marked `NOT_APPLICABLE` with rationale.
- HC-25.3: Every Mermaid block has a `.mmd` sidecar file.
- HC-25.4: No diagram exceeds 30 nodes without decomposition.
- HC-25.5: Every edge in every diagram carries a citation.
- HC-25.6: Coverage cross-check is recorded with no unresolved contradictions.
- HC-25.7: `repository_fingerprint_recheck` matches ART-01.
- HC-25.8: No `BLOCKING` open questions remain.
- HC-25.9: PROMPT_25's `status: SUCCESS` (or `SKIPPED` per § 3.1) satisfies the Phase 3 exit conditions and authorizes the orchestrator to begin Phase 4 per `PROJECT_SPECIFICATION.md` § 5.4.

---

## 12. Cross-References

- **Consumed by:** PROMPT_26 (Rebuild Guide — embeds ART-25's C4 diagrams and class diagrams), PROMPT_27 (Developer Handbook — embeds ART-25's folder tree and request-lifecycle sequence diagrams), PROMPT_29 (Final Documentation Assembly — re-renders ART-25's diagrams in the export format), PROMPT_28 (Cross-Reference Checklists — verifies that every diagram entity resolves to an upstream artifact).
- **Depends on:** ART-01 through ART-24 (all prior artifacts).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, Diagrams type extension; Diagrams bar (aggregate ≥ 30, Coverage ≥ 4, Coherence ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4 (sidecar files), § 3.1, § 4, § 6, § 7 (Mermaid conventions, diagram types by use case § 7.4, diagram quality § 7.5: ≤ 30 nodes, edge labels, edge citations, decomposition).
- **HOOK integration:** HOOK-03 (State Reachability) is visualized in state machine diagrams per Q-25.G; PROMPT_30 verifies the visual highlighting matches the HOOK-03 findings from ART-13.
- **Forward reference:** PROMPT_30 (Self-Review & QA) verifies that every diagram referenced by ART-26, ART-27, or ART-29 resolves to an entry in ART-25, and that no diagram asserts an edge that contradicts an upstream artifact.

*End of PROMPT_25. Orchestrator may dispatch PROMPT_26 upon satisfaction of § 11; this also satisfies the Phase 3 exit conditions per `PROJECT_SPECIFICATION.md` § 5.4.*
