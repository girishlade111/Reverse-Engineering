# PROMPT_10.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_10: Call Graph & Dependency Graph Construction

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_10
- **Phase:** 1
- **Stage:** 10 of 10 (Phase 1 capstone)
- **Dependencies:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-06 (PROMPT_06), ART-07 (PROMPT_07), ART-08 (PROMPT_08), ART-09 (PROMPT_09).
- **Estimated Tokens:** 12000–18000
- **Output Artifacts:** ART-10 (Graph) — Call Graph & Dependency Graph.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Call Graph & Dependency Graph artifact (ART-10) that constructs the function-to-function call graph and the module-to-module dependency graph from the data emitted by PROMPT_05, PROMPT_06, PROMPT_07, PROMPT_08, and PROMPT_09, emits both as Mermaid graph sources with edge-level citations, and identifies hub functions, leaf functions, critical paths (entry-to-leaf chains), and strongly connected components.

---

## 3. When to Invoke

PROMPT_10 is dispatched when ALL of the following predicates hold:

- PROMPT_09 has emitted a Completion Record satisfying its Handoff Criteria.
- ART-01, ART-05, ART-06, ART-07, ART-08, and ART-09 are present and non-empty.
- ART-09 records at least one function with non-empty `callers` and `callees` lists (else the call graph is trivial and recorded as `EMPTY_CALL_GRAPH`).
- ART-06 records at least one module with at least one inter-module edge (else the dependency graph is trivial and recorded as `EMPTY_DEPENDENCY_GRAPH`).

PROMPT_10 is the Phase 1 capstone; its completion satisfies the Phase 1 exit conditions and authorizes the orchestrator to begin Phase 2 per `PROJECT_SPECIFICATION.md` § 5.2.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; fingerprint re-verification. |
| ART-05 | Map | Entry-point catalog; entry points are the roots of the call graph's reachability analysis. |
| ART-06 | Map | Module dependency graph; this prompt re-emits it as Mermaid with edge-level citations and computes graph-theoretic metrics. |
| ART-07 | Map | Component hierarchy; component `CONTAINS` edges supplement the call graph for UI component composition. |
| ART-08 | Map | Class hierarchy and `IMPLEMENTS` edges; needed to resolve dynamic-dispatch candidate targets and to add class-level edges to the call graph (constructor calls, virtual-method calls). |
| ART-09 | Doc | Function reference; the `callers` and `callees` lists are the seed for the call graph. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R16, R17 (every edge cites its source), R19, R21. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation, and Mermaid graph conventions (§ 7); emit `.mmd` sidecar files. |
| `QUALITY_STANDARDS.md` | Framework file | Apply Graph schema (`§ 4.4`, extends Map) and type-specific bar (aggregate ≥ 30, Coverage ≥ 4, Coherence ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Construct the call graph per § 6.1 from ART-09's caller/callee data.
3. Resolve dynamic-dispatch edges per § 6.2 using ART-08's class hierarchy.
4. Add class-level edges per § 6.3 (constructor calls, virtual-method calls).
5. Construct the dependency graph per § 6.4 from ART-06's module edges.
6. Compute graph metrics per § 6.5 (hub functions, leaf functions, critical paths, SCCs, density).
7. Identify critical paths per § 6.6 (entry-to-leaf chains through high-traffic hubs).
8. Emit Mermaid graph sources per § 6.7 with edge-level citations; decompose graphs > 30 nodes.
9. Cross-check the call graph against ART-05's entry-point reachability per § 6.8; the dead-code candidates from ART-09 must align with unreachable functions here.
10. Emit ART-10 per § 8 with full front-matter, both graphs (Mermaid), metrics, critical paths, SCC catalog, sidecar files, traceability index, open questions.
11. Run the Quality Checks in § 9.
12. Emit the Completion Record per `MASTER_PROMPT.md` § 6; this Completion Record also serves as the Phase 1 exit signal.

---

## 6. Analysis Procedures

### 6.1 Call Graph Construction

Construct a directed graph `G_call = (V, E_call)` where:

- `V` is the set of all in-scope functions `FN-XX` per ART-09.
- `E_call` is the union over all functions of their `callees` lists; each edge `(caller, callee)` carries `call_site` (`file:line`), `kind` (direct | method | static | reference | reflective | dynamic-dispatch), and `external` (false for in-scope callees; true for `EXTERNAL` callees, recorded as separate external-edge list).

External callees (`EXTERNAL`) are recorded in a separate `external_call_edges` list and are NOT added to `V` (the graph is the in-scope subgraph). External edges cite the call site and the external callee's fully-qualified name.

Self-edges (a function calling itself, per ART-09's direct recursion) are preserved as edges `(FN-x, FN-x)` with `kind: direct` and the recursion line cited.

### 6.2 Dynamic-Dispatch Edge Resolution

For every caller-callee edge with `kind: dynamic-dispatch` (per ART-09), resolve the candidate target set:

1. Identify the receiver type at the call site (e.g., `obj.method()` — the static type of `obj`).
2. Look up the static type's class hierarchy in ART-08 (`EXTENDS` and `IMPLEMENTS` edges).
3. Compute the set of overrides — every subclass/implementation that overrides `method`.
4. For each override, add an edge `(caller, override_FN_XX)` with `kind: dynamic-dispatch-resolved` and the call site citation.
5. If the static type is itself concrete (not abstract, not an interface), include the static type's own method as a target.
6. If no overrides exist (the static type is concrete and has the method), add a single edge with `kind: direct` (downgrade from dynamic-dispatch to direct).
7. If the static type cannot be resolved (e.g., the receiver is `any` in TypeScript or `object` in Python pre-3.5), keep the edge as `kind: dynamic-dispatch` with `candidate_targets: UNRESOLVED` and emit an Open Question.

The resolved edges replace the unresolved dynamic-dispatch edges in `E_call`.

### 6.3 Class-Level Edge Augmentation

Augment the call graph with class-level edges:

- **Constructor calls** — every `new ClassName(args)` invocation is an edge `(caller_FN, constructor_FN)` where `constructor_FN` is the class's constructor (per ART-08). Cite the `new` site.
- **Virtual-method calls** — every method invocation on a receiver of an abstract type or interface is resolved via § 6.2.
- **Static-method calls** — `ClassName.method(args)` is an edge `(caller_FN, static_method_FN)`. Cite the call site.
- **Field accesses** — `obj.field` is recorded as a `READS`/`WRITES` edge from the accessing function to the field's `V-XX` (per `PROJECT_SPECIFICATION.md` § 4.2); these edges are part of the data-flow graph (consumed by PROMPT_11) and are recorded here as a separate `field_access_edges` list, not added to `E_call`.

### 6.4 Dependency Graph Construction

Re-emit the module dependency graph from ART-06 as a Mermaid graph with edge-level citations. For every module-level edge `(M_a, M_b)` of type `IMPORTS` or `DEPENDS_ON`:

1. Aggregate the file-level import edges (per ART-06's `evidence_files`) and emit them as the edge's citation list.
2. Record the edge in `E_dep` with `from_module`, `to_module`, `relationship`, `weight`, `evidence_files`.
3. External dependency edges (`M_a → DEP-XX`) are recorded in a separate `external_dependency_edges` list, citing the import statement.

The dependency graph is `G_dep = (M, E_dep)` where `M` is the set of in-scope modules per ART-06 and `E_dep` is the set of in-scope inter-module edges.

### 6.5 Graph Metrics Computation

Compute the following metrics for both graphs.

**Per-node metrics:**

- `in_degree` — number of incoming edges (callers for call graph; importers for dependency graph).
- `out_degree` — number of outgoing edges (callees for call graph; imported modules for dependency graph).
- `betweenness_centrality` — the fraction of shortest paths between any two nodes that pass through this node; computed by Brandes' algorithm.
- `closeness_centrality` — reciprocal of the sum of shortest-path distances to all other reachable nodes.
- `eccentricity` — the maximum shortest-path distance from this node to any reachable node.

**Hub functions** — the top-5% of nodes by `in_degree` in the call graph. Each hub is recorded with `hub_id`, `in_degree`, `betweenness_centrality`.

**Leaf functions** — functions with `out_degree = 0` in the call graph (zero callees, per `PROJECT_SPECIFICATION.md` § Glossary). Each leaf is recorded with `leaf_id`, `in_degree`.

**Per-graph metrics:**

- `node_count`, `edge_count`.
- `density` — `edge_count / (node_count * (node_count - 1))` for a directed graph.
- `average_path_length` — over reachable pairs.
- `diameter` — the maximum shortest-path distance over reachable pairs.
- `clustering_coefficient` — the fraction of transitive triples (a→b, a→c, b→c).

### 6.6 Critical Path Identification

A critical path is an entry-to-leaf chain through high-traffic hubs. Identification procedure:

1. Take every entry-point function `EP_FN` (the first function called by each entry point in ART-05, recorded as the first step of the entry's bootstrap trace).
2. Take every leaf function `L_FN` (per § 6.5).
3. For every `(EP_FN, L_FN)` pair, compute the shortest path in the call graph (by BFS or Dijkstra with unit weights).
4. Score each path by the sum of `betweenness_centrality` of its intermediate nodes (high-traffic hubs add to the score).
5. Rank the top-N paths (N = min(10, total paths); cap to avoid combinatorial explosion on large graphs) by score.
6. Each critical path is recorded with `path_id`, `entry_FN`, `leaf_FN`, `path_length`, `intermediate_nodes`, `score`, `path_citations` (the citation for every edge in the path).

For large graphs (node_count > 1000), the agent MAY sample leaf functions rather than enumerating every `(EP, leaf)` pair; the sampling procedure is documented in the methodology section.

### 6.7 Strongly Connected Component Computation

Compute SCCs using Tarjan's algorithm on both `G_call` and `G_dep`:

- For `G_call`: every SCC with > 1 node is a function-level cycle; every SCC of size 1 with a self-edge is a self-recursive function (already recorded per ART-09's recursion classification). Each function-level cycle is recorded with `scc_id`, `members` (`FN-XX` list), `kind` (cycle | self-recursion), `severity` (MAJOR for runtime cycles, MINOR for type-only cycles — distinguished by inspecting the edges' call sites for runtime invocation).
- For `G_dep`: every SCC with > 1 node is a module-level cycle; cross-reference to ART-06's `circular_dependencies` list (PROMPT_06 already computed these via Tarjan; this prompt re-verifies and emits them as Mermaid subgraphs).

Each SCC is rendered as a Mermaid subgraph in the corresponding diagram per `OUTPUT_RULES.md` § 7.

### 6.8 Reachability Cross-Check

Cross-check the call graph's reachability against ART-09's dead-code candidacy:

1. Compute the set `R` of functions reachable from any entry-point function by BFS over `E_call`.
2. Compute the set `D` of functions flagged `DEAD_CODE_CANDIDATE: true` in ART-09.
3. The expected relationship: `D ⊇ V \ R` (every unreachable function is a dead-code candidate). The converse may not hold: a function may be reachable only via reflection or dynamic dispatch and still be flagged dead-code with reduced confidence.
4. For every function in `V \ R` that is NOT in `D`, emit a `CONTRADICTION` finding per `OPERATING_RULES.md` R33 — the function is unreachable but was not flagged dead-code by PROMPT_09.
5. For every function in `D` that IS in `R`, verify the reachability is via reflection or dynamic dispatch; otherwise emit a `CONTRADICTION` (the function is reachable but was flagged dead-code).

### 6.9 Mermaid Graph Emission

Emit the graphs as Mermaid source per `OUTPUT_RULES.md` § 7:

- **Call graph** — `graph LR` (left-to-right for readability); nodes labeled `FN-XX[short-name]`; edges labeled with the call-site line; edges colored by kind (direct = solid, dynamic-dispatch = dashed, reflective = dotted).
- **Dependency graph** — `graph TD` (top-down); nodes labeled `M-XX[module-name]`; edges labeled with the import statement's file:line.
- **SCC subgraphs** — rendered as `subgraph SCC-XX` blocks within the parent graph.
- **Critical paths** — rendered as a separate `graph LR` highlighting the path's nodes and edges in bold.

Graphs larger than 30 nodes are decomposed by module (for `G_dep`) or by module-cluster (for `G_call`) into sub-diagrams with a master index diagram. The master index shows modules as nodes with edges to their sub-diagrams.

Each Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption per `OUTPUT_RULES.md` § 7.2. The Mermaid source is also written as a `.mmd` sidecar file at `<output_root>/diagrams/<engagement_id>_ART10_D-XX.mmd` per `OUTPUT_RULES.md` § 2.4.

---

## 7. Required Outputs

### ART-10 — Call Graph & Dependency Graph

**Type:** Graph.

**Acceptance Criteria:**

- AC-10.1: The artifact file exists at `<output_root>/artifacts/phase1/ART10_<engagement_id>_call-dep-graph.md`.
- AC-10.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.4 (Graph schema, extends Map).
- AC-10.3: The body contains: Executive Summary, Methodology, Call Graph (Mermaid), Dependency Graph (Mermaid), Hub Functions, Leaf Functions, Critical Paths, Strongly Connected Components, Graph Metrics, Reachability Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-10.4: Every edge in both graphs cites its source line(s).
- AC-10.5: Every Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-10.6: A `.mmd` sidecar file exists for every Mermaid block under `<output_root>/diagrams/`.
- AC-10.7: Hub functions, leaf functions, critical paths, and SCCs are enumerated with metrics.
- AC-10.8: Reachability cross-check findings are recorded; contradictions are flagged per R33.
- AC-10.9: Graphs larger than 30 nodes are decomposed with a master index diagram.

---

## 8. Output Templates

### 8.1 ART-10 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-10
artifact_type: Graph
producing_prompt: PROMPT_10
phase: 1
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
call_graph:
  node_count: <int>
  edge_count: <int>
  density: <0..1>
  average_path_length: <float> | NA
  diameter: <int> | NA
  clustering_coefficient: <0..1>
  hub_functions:
    - fn_id: FN-XX
      in_degree: <int>
      betweenness_centrality: <float>
  leaf_functions: [FN-XX]
  call_edges:
    - from: FN-XX
      to: FN-XX
      kind: direct | method | static | reference | reflective | dynamic-dispatch-resolved
      call_site: <file>:<line>
      external: false
  external_call_edges:
    - from: FN-XX
      to: <fully-qualified-name>
      call_site: <file>:<line>
dependency_graph:
  node_count: <int>
  edge_count: <int>
  density: <0..1>
  average_path_length: <float> | NA
  diameter: <int> | NA
  clustering_coefficient: <0..1>
  dependency_edges:
    - from: M-XX
      to: M-XX
      relationship: IMPORTS | DEPENDS_ON
      weight: <int>
      evidence_files:
        - <file>:<line-range>
      external: false
  external_dependency_edges:
    - from: M-XX
      to: DEP-XX
      relationship: IMPORTS
      weight: <int>
      evidence_files:
        - <file>:<line-range>
strongly_connected_components:
  call_graph:
    - scc_id: SCC-C-01
      members: [FN-XX]
      kind: cycle | self-recursion
      severity: MAJOR | MINOR | INFO
  dependency_graph:
    - scc_id: SCC-D-01
      members: [M-XX]
      kind: cycle
      severity: MAJOR | MINOR | INFO
critical_paths:
  - path_id: CP-01
    entry_fn: FN-XX
    leaf_fn: FN-XX
    path_length: <int>
    intermediate_nodes: [FN-XX]
    score: <float>
    path_citations:
      - <file>:<line>
reachability_cross_check:
  reachable_set_size: <int>
  dead_code_candidates_size: <int>
  unreachable_not_flagged: [FN-XX]
  flagged_but_reachable: [FN-XX]
  contradictions: [{ kind: <text>, fn_id: FN-XX, detail: <text> }]
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
  - id: FN-01
    label: <name>
    kind: function
    parent_id: null
    depth: 0
  - id: M-01
    label: <name>
    kind: module
    parent_id: null
    depth: 0
edges:
  - from: FN-01
    to: FN-02
    relationship: CALLS
    evidence: <file_path>:<line-range>
  - from: M-01
    to: M-02
    relationship: IMPORTS
    evidence: <file_path>:<line-range>
layout_hint: LR | TD
mermaid_source: |
  graph LR
      FN01[FN-01: main] --> FN02[FN-02: dispatch]
      %% edge: src/main.go:18
---
```

### 8.2 ART-10 Body Skeleton

```markdown
# ART-10: Call Graph & Dependency Graph

## 1. Executive Summary
## 2. Methodology
## 3. Call Graph
   ### 3.1 Master Index Diagram
   **Diagram D-01: Call Graph (Overview)**
   ```mermaid
   graph LR
       FN01[FN-01: main] --> FN02[FN-02: dispatch]
       FN02 --> FN03[FN-03: handle]
       %% edge: src/main.go:18
       %% edge: src/dispatch.go:42
   ```
   ### 3.2 Sub-graph: Module M-XX
   ### 3.3 Strongly Connected Components
## 4. Dependency Graph
   **Diagram D-02: Module Dependency Graph**
   ```mermaid
   graph TD
       M01[M-01: auth] --> M02[M-02: users]
       %% edge: src/auth/index.ts:5
   ```
## 5. Hub Functions
## 6. Leaf Functions
## 7. Critical Paths
   **Diagram D-03: Top Critical Path**
## 8. Strongly Connected Components
## 9. Graph Metrics
## 10. Reachability Cross-Check
## 11. Traceability Index
## 12. Open Questions
## 13. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every in-scope function appears as a node in the call graph; every in-scope module appears as a node in the dependency graph. Threshold ≥ 0.99.
- **Q2. Citation Check** — every edge has at least one citation; ≥ 0.99 cited (the bar is higher for graphs because edges are inherently source-bound).
- **Q3. Schema Conformance Check** — validates against § 4.4.
- **Q4. Non-Contradiction Check** — no edge contradicts ART-09's caller/callee data; no module edge contradicts ART-06's `module_edges`.
- **Q5. UNVERIFIED Accounting** — every `UNRESOLVED` dynamic-dispatch edge has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.1 on a 5% sample yields the same edge set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-10.A. Edge Bidirectionality** — for every call edge `(FN_a, FN_b)`, `FN_b.callers` contains `FN_a` (per ART-09).
- **Q-10.B. Dynamic-Dispatch Resolution** — every edge with `kind: dynamic-dispatch` has either been resolved to `kind: dynamic-dispatch-resolved` with concrete candidate targets OR is recorded as `UNRESOLVED` with an Open Question.
- **Q-10.C. SCC Computation** — Tarjan's algorithm runs over both graphs; every cycle of size > 1 is reported.
- **Q-10.D. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment per `OUTPUT_RULES.md` § 7.5.
- **Q-10.E. Sidecar Files** — every Mermaid block has a corresponding `.mmd` file under `<output_root>/diagrams/`.
- **Q-10.F. Reachability Consistency** — `unreachable_not_flagged` and `flagged_but_reachable` are empty OR every discrepancy is a recorded contradiction per R33.
- **Q-10.G. Graph Decomposition** — graphs > 30 nodes are decomposed; the master index diagram exists.

---

## 10. Common Pitfalls

- Do not include external callees as nodes in the call graph; they belong in `external_call_edges`, not in `V`.
- Always resolve dynamic-dispatch edges via ART-08's class hierarchy; an unresolved edge is a gap, not a simplification.
- Do not collapse self-recursive edges into SCCs of size 1 with `kind: self-recursion`; record them distinctly from cycles of size > 1.
- Always cite every edge with at least one `file:line` location; a Mermaid edge without an `edge:` comment is non-conformant per `OUTPUT_RULES.md` § 7.5.
- Always emit a `.mmd` sidecar file for every Mermaid block; the sidecar is the canonical source for re-rendering by PROMPT_25.
- Do not compute critical paths exhaustively for large graphs; sample leaf functions and document the sampling in the methodology section.
- Always cross-check reachability against ART-09's dead-code candidacy; a discrepancy is a real finding (a contradiction per R33), not a flaw in either prompt.
- Always distinguish `IMPORTS` from `DEPENDS_ON` in the dependency graph; type-only imports do not create runtime dependencies and should not be reported as runtime cycles.
- Do not collapse the call graph and dependency graph into a single diagram; they have different node types (functions vs modules) and different consumers.
- Always include the master index diagram for decomposed graphs; an End Consumer navigating the documentation MUST be able to find any sub-diagram from the index.
- Always cap the Mermaid diagram at 30 nodes per `OUTPUT_RULES.md` § 7.5; larger diagrams are unreadable and must be decomposed.
- Do not record field accesses (`READS`/`WRITES`) as call edges; they are data-flow edges consumed by PROMPT_11, recorded in a separate list.

---

## 11. Handoff Criteria

PROMPT_11, PROMPT_12, PROMPT_25, and PROMPT_28 consume ART-10. Handoff requires ALL of:

- HC-10.1: ART-10 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-10.2: Every in-scope function appears as a node in the call graph.
- HC-10.3: Every in-scope module appears as a node in the dependency graph.
- HC-10.4: Every edge in both graphs cites its source.
- HC-10.5: Mermaid diagrams are emitted with `.mmd` sidecar files.
- HC-10.6: Hub functions, leaf functions, critical paths, and SCCs are enumerated.
- HC-10.7: Reachability cross-check is recorded with no unresolved contradictions.
- HC-10.8: `repository_fingerprint_recheck` matches ART-01.
- HC-10.9: No `BLOCKING` open questions remain.

Additionally, as the Phase 1 capstone, PROMPT_10's `status: SUCCESS` satisfies the Phase 1 exit conditions per `PROJECT_SPECIFICATION.md` § 5.2 and authorizes the orchestrator to begin Phase 2.

---

## 12. Cross-References

- **Consumed by:** PROMPT_11 (Data Flow — uses call graph to trace data-flow edges), PROMPT_12 (Control Flow — uses call graph and critical paths as the execution-path seed), PROMPT_25 (Diagram Generation — re-renders Mermaid sources at higher visual fidelity), PROMPT_28 (Cross-Reference Checklists — uses ART-10 as the graph-coverage ground truth).
- **Depends on:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-06 (PROMPT_06), ART-07 (PROMPT_07), ART-08 (PROMPT_08), ART-09 (PROMPT_09).
- **Governing rules:** `OPERATING_RULES.md` R13, R16, R17, R19, R21, R33 (contradiction escalation between ART-10 reachability and ART-09 dead-code).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.4; Graph bar (aggregate ≥ 30, Coverage ≥ 4, Coherence ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4 (sidecar files), § 3.1, § 4, § 6, § 7 (Mermaid graph conventions, edge citations, ≤ 30 nodes, decomposition).
- **Forward reference:** PROMPT_30 verifies that the call graph's reachability set and ART-09's dead-code candidacy are mutually consistent; discrepancies are blocking findings.
- **Phase boundary:** This prompt's `status: SUCCESS` is the Phase 1 exit signal per `PROJECT_SPECIFICATION.md` § 5.2. The orchestrator MUST NOT dispatch PROMPT_11 until HC-10.1 through HC-10.9 are all satisfied.

*End of PROMPT_10. This completes Phase 1 (Intake & Cartography). Orchestrator may proceed to Phase 2 (Dynamics & Behavior) upon satisfaction of § 11.*
