# PROMPT_26.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_26: Rebuild Guide & Architecture Handbook

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_26
- **Phase:** 4
- **Stage:** 1 of 5 (Phase 4 opener)
- **Dependencies:** ART-01 through ART-25 (all prior Phase 1, Phase 2, and Phase 3 artifacts).
- **Estimated Tokens:** 22000–35000
- **Output Artifacts:** ART-26 (Handbook) — Rebuild Guide & Architecture Handbook.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Rebuild Guide & Architecture Handbook artifact (ART-26) that synthesizes every prior artifact into a reconstructive handbook enabling a competent engineer to rebuild a behaviorally equivalent system from scratch — covering the nine mandatory sections (System Overview, Architecture, Module-by-Module Rebuild Guide, Data Model Reconstruction, Configuration Reconstruction, Deployment Reconstruction, Testing Strategy, Operational Concerns, Engineering Decisions) — where every section answers "how would I build this?" rather than "what is this?", and which passes the reconstructability test in PROMPT_30 § 7.4.

---

## 3. When to Invoke

PROMPT_26 is dispatched when ALL of the following predicates hold:

- Phase 3 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.4 (PROMPT_25 emitted `status: SUCCESS` or `SKIPPED` per § 3.1).
- ART-01 through ART-25 (where produced) are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-25 is present and contains at least one applicable diagram; if ART-25 is `SKIPPED` per § 3.1, the orchestrator MUST issue a waiver explicitly authorizing PROMPT_26 to proceed without embedded diagrams (the handbook will then contain diagram placeholders citing the absent ART-25).
- The engagement manifest's scope modifier is NOT `SCOPE_TRIAGE` (under `SCOPE_TRIAGE`, Phase 4 is not dispatched per `MISSION.md` § 6).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. Boundary declaration drives the System Overview's scope statement. |
| ART-02 | Manifest | Tech stack & dependencies; the Architecture section's technology choices and the Module-by-Module Rebuild Guide's library prerequisites. |
| ART-03 | Map | Folder & file tree; the Module-by-Module Rebuild Guide's directory structure recommendations. |
| ART-04 | Spec | Build & configuration; the Configuration Reconstruction and Deployment Reconstruction sections. |
| ART-05 | Map | Entry points & bootstrap; the Module-by-Module Rebuild Guide's startup procedure. |
| ART-06 | Map | Module map; the Module-by-Module Rebuild Guide's per-module sections. |
| ART-07 | Map | Component map; the Module-by-Module Rebuild Guide's component-level rebuild steps. |
| ART-08 | Doc | Class & interface catalog; the Module-by-Module Rebuild Guide's class design and the Data Model Reconstruction's entity classes. |
| ART-09 | Doc | Function catalog; the Module-by-Module Rebuild Guide's key-algorithm reconstruction (rebuild steps derived from function-level analysis). |
| ART-10 | Graph | Call & dependency graphs; the Module-by-Module Rebuild Guide's internal structure (call topology). |
| ART-11 | Graph | Data flow diagrams; the Module-by-Module Rebuild Guide's data-handling procedures. |
| ART-12 | Graph | Control flow & execution paths; the Module-by-Module Rebuild Guide's algorithm reconstruction. |
| ART-13 | Doc | State machine catalog; the Module-by-Module Rebuild Guide's stateful-component reconstruction. |
| ART-14 | Doc | Event workflow catalog; the Module-by-Module Rebuild Guide's event-driven-component reconstruction. |
| ART-15 | Doc | API & interface reference; the Module-by-Module Rebuild Guide's public-API reconstruction. |
| ART-16 | Doc | Middleware & pipeline map; the Module-by-Module Rebuild Guide's middleware-chain reconstruction. |
| ART-17 | Doc | Error handling & resilience; the Module-by-Module Rebuild Guide's error-handling reconstruction and the Operational Concerns section. |
| ART-18 | Doc | Caching & performance; the Operational Concerns section and the Module-by-Module Rebuild Guide's cache integration. |
| ART-19 | Doc | Auth report (optional); the Module-by-Module Rebuild Guide's auth-component reconstruction (when present). |
| ART-20 | Doc | Persistence report (optional); the Data Model Reconstruction section (when present). |
| ART-21 | Doc | AI/LLM workflow report; the Module-by-Module Rebuild Guide's AI-component reconstruction (when present). |
| ART-22 | Doc | Streaming workflow report; the Module-by-Module Rebuild Guide's streaming-component reconstruction (when present). |
| ART-23 | Doc | Design pattern catalog; the Module-by-Module Rebuild Guide's pattern preservation. |
| ART-24 | Doc | Engineering decision record; the Engineering Decisions section (with rationale to preserve). |
| ART-25 | Diagrams | The Architecture section's embedded C4 diagrams and the Module-by-Module Rebuild Guide's embedded component/class diagrams. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22 (no behavior invention — restricts rebuild-step recommendations to evidence-bound procedures), R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid embedding (§ 7), Handbook schema (§ 4.6). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Handbook schema (§ 4.6) and type-specific bar (aggregate ≥ 32, Reconstructive completeness ≥ 4, Readability ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Author the System Overview section per § 6.1.
3. Author the Architecture section per § 6.2 with embedded C4 diagrams from ART-25.
4. Author the Module-by-Module Rebuild Guide per § 6.3 — for each module, derive purpose, dependencies, internal structure, public API, key algorithms, and rebuild steps in order.
5. Author the Data Model Reconstruction section per § 6.4 (when ART-20 is present; otherwise degrade to type-level reconstruction from ART-08/ART-11).
6. Author the Configuration Reconstruction section per § 6.5 from ART-04.
7. Author the Deployment Reconstruction section per § 6.6 from ART-04's IaC and ART-25's deployment diagram.
8. Author the Testing Strategy section per § 6.7 from ART-04's test scripts and ART-09's function-level test coverage.
9. Author the Operational Concerns section per § 6.8 from ART-17, ART-18, ART-04's logging configuration.
10. Author the Engineering Decisions section per § 6.9 from ART-24, preserving rationale.
11. Apply the reconstructive-mode filter per § 6.10 — every section MUST answer "how would I build this?", not "what is this?".
12. Embed diagrams from ART-25 per § 6.11 with relative-path links to sidecar files.
13. Cross-check the handbook against the upstream artifacts per § 6.12 — every claim MUST cite the upstream artifact.
14. Emit ART-26 per § 8 with full front-matter, all nine sections, traceability index, open questions.
15. Run the Quality Checks in § 9, including the reconstructability self-test per § 9.A.
16. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 System Overview Section

Author the System Overview. The section answers: what does the system do, who uses it, what is its scope, what is its boundary? Subsections:

- **Purpose** — one paragraph stating the system's purpose, derived from ART-01's repository metadata (README, package description) and ART-05's entry points.
- **Users and personas** — derived from ART-15's API consumers and ART-05's entry-point callers. Each persona records its name, role, and primary use case.
- **Scope and boundary** — derived from ART-01's boundary declaration. In-scope paths are listed; out-of-scope paths are explicitly excluded with rationale.
- **High-level architecture** — one paragraph summarizing the architecture pattern (from ART-23's architectural patterns), the deployment topology (from ART-04), and the dominant technology stack (from ART-02).

Each paragraph MUST cite the upstream artifact. The System Overview MUST NOT exceed 500 words; it is the executive summary, not the full architecture.

### 6.2 Architecture Section

Author the Architecture section. The section answers: how is the system structured at the highest level, and how would I rebuild that structure? Subsections:

- **System Context (C4 L1)** — embed `D-CTX-01` from ART-25. Accompanying paragraph explains the system's external interactions.
- **Container Diagram (C4 L2)** — embed `D-CONT-01` from ART-25. Accompanying paragraph explains each container's responsibility and technology.
- **Component Diagrams (C4 L3)** — embed `D-COMP-XX` for each major component. Accompanying paragraph explains the component's internal structure.
- **Architectural pattern** — state the architectural pattern (from ART-23) and explain why it was chosen (from ART-24's Architecture decisions). Cross-reference the relevant `DEC-XX`.
- **Rebuild steps** — ordered, atomic steps to reproduce the architectural skeleton: (1) create the directory structure, (2) initialize the build system, (3) define the container boundaries, (4) define the component interfaces, (5) wire the dependencies.

### 6.3 Module-by-Module Rebuild Guide

Author the Module-by-Module Rebuild Guide. For each module (`M-XX` in ART-06), produce a subsection answering: how would I rebuild this module? Each module subsection contains:

- **Purpose** — one paragraph stating the module's purpose, derived from ART-06's module map and ART-09's function catalog. Cite ART-06 and ART-09.
- **Dependencies** — list of upstream modules (`M-XX`) and external dependencies (`DEP-XX`) from ART-10. For each dependency, state why it is required.
- **Internal structure** — embed the module's component diagram (`D-COMP-XX`) or call-graph sub-diagram (`D-CG-XX`) from ART-25. Accompanying paragraph explains the internal topology.
- **Public API** — list of exported functions/classes/interfaces from ART-09 and ART-08. For each, state the signature, the responsibility, and the consumers (callers from ART-10).
- **Key algorithms** — for each significant algorithm in the module (functions with high cyclomatic complexity from ART-09 or functions marked as hotspots from ART-10), reconstruct the algorithm in pseudocode. The pseudocode MUST be derived from ART-09's function-level analysis and ART-12's control flow. Cite ART-09 and ART-12. The pseudocode MUST NOT be a verbatim copy of the source (per `MISSION.md` § 5 anti-goal of unauthorized competitive use); it MUST be a behavioral reconstruction in pseudocode form.
- **Rebuild steps** — ordered, atomic steps to rebuild the module: (1) create the module directory, (2) define the module's interfaces (`I-XX` from ART-08), (3) implement the module's classes (`K-XX` from ART-08) in dependency order (lowest-level first), (4) implement the module's functions (`FN-XX` from ART-09) in call-graph order (callees before callers), (5) write the module's tests per § 6.7, (6) integrate with upstream modules, (7) verify against the public API. Each step cites the upstream artifacts it derives from.

The Module-by-Module Rebuild Guide is the longest section. It is decomposed by module; each module is a top-level subsection.

### 6.4 Data Model Reconstruction Section

Author the Data Model Reconstruction section. When ART-20 is present, the section answers: how would I rebuild the persistence layer? Subsections:

- **Persistence model** — state the persistence model (SQL, NoSQL, KV, etc.) from ART-20's `persistence_models`. Cite ART-20.
- **Schema** — for each entity (`ENT-XX`), state the entity name, table/collection name, fields, indexes, and constraints. Embed the ER diagram (`D-ER-XX`) from ART-25.
- **Relations** — list the relations (`REL-XX`) with their cardinality, foreign keys, and on-delete behavior.
- **Migrations** — list the migrations (`MIG-XX`) in order. State the migration tool and the procedure to apply migrations from scratch.
- **Transactions** — list the transaction boundaries (`TXN-XX`) with their scope, isolation level, and rollback triggers.
- **Rebuild steps** — ordered, atomic steps: (1) install the database, (2) install the ORM/ODM, (3) define the entity classes, (4) define the relations, (5) write the initial migration, (6) apply the migration, (7) verify the schema.

When ART-20 is `ABSENT`, the section degrades to type-level reconstruction: for each significant data type (`D-XX` in ART-11), state the type's name, structure, and role. The rebuild steps cover defining these types in the target language. Record an Open Question noting the absence of ART-20.

### 6.5 Configuration Reconstruction Section

Author the Configuration Reconstruction section from ART-04. The section answers: how would I rebuild the configuration layer? Subsections:

- **Configuration sources** — list the configuration sources (env vars, config files, feature flags) from ART-04. For each, state the name, the type, the default value, and the consuming components.
- **Configuration layering** — state the layering order (e.g., default → file → env var → CLI flag) from ART-04's configuration-layering analysis.
- **Secrets management** — state how secrets are managed (env vars, secret managers, vault integrations). The actual secret values are `REDACTED` per `MISSION.md` § 5; only the management procedure is documented.
- **Rebuild steps** — ordered, atomic steps: (1) create the default config file, (2) implement the env-var loader, (3) implement the file loader, (4) implement the layering logic, (5) implement the secrets loader, (6) document each config item.

### 6.6 Deployment Reconstruction Section

Author the Deployment Reconstruction section from ART-04's IaC and ART-25's deployment diagram. The section answers: how would I deploy this system? Subsections:

- **Deployment topology** — embed the deployment diagram (`D-DEPL-01`) from ART-25 (when present). Accompanying paragraph explains each node.
- **IaC tool** — state the IaC tool (Terraform, CloudFormation, Pulumi, Kubernetes, Docker Compose) from ART-04. State the IaC file structure.
- **CI/CD pipeline** — state the CI/CD pipeline (from ART-04's CI/CD extraction) with its stages: build, test, package, deploy. Each stage's commands are listed.
- **Environment promotion** — state the promotion procedure (dev → staging → prod) and the gating criteria.
- **Rebuild steps** — ordered, atomic steps: (1) install the IaC tool, (2) write the IaC configuration, (3) configure the CI/CD pipeline, (4) provision the infrastructure, (5) deploy the application, (6) verify the deployment.

When no IaC is detected, the section degrades to manual deployment instructions: install dependencies, build the application, run the application. Record an Open Question noting the absence of IaC.

### 6.7 Testing Strategy Section

Author the Testing Strategy section from ART-04's test scripts and ART-09's function-level test coverage. The section answers: how would I rebuild the test suite? Subsections:

- **Test framework** — state the test framework(s) from ART-04 (Jest, Mocha, PyTest, JUnit, pytest, Go testing, etc.).
- **Test structure** — state the test directory structure (`tests/`, `__tests__/`, `src/*.test.ts`, `test/*.py`). State the test naming convention.
- **Test categories** — list the test categories present: unit, integration, end-to-end, contract, performance, snapshot. For each category, state the count of tests and the coverage.
- **Test fixtures** — list the test fixtures (factories, mocks, seeds) and their locations.
- **Rebuild steps** — ordered, atomic steps: (1) install the test framework, (2) create the test directory structure, (3) write the unit tests for each module, (4) write the integration tests for each cross-module flow, (5) write the end-to-end tests for each major workflow, (6) configure the test runner, (7) verify coverage.

### 6.8 Operational Concerns Section

Author the Operational Concerns section. The section answers: how would I operate this system in production? Subsections:

- **Logging** — state the logging framework (from ART-04's logging configuration), the log levels, the log format, and the log destination. Cite ART-04.
- **Monitoring** — state the monitoring integration (Prometheus, Grafana, Datadog, New Relic) from ART-04's configuration. List the exported metrics.
- **Error handling** — summarize ART-17's error-handling report. State the dominant error-handling strategy, the retry policies, the circuit breakers, and the dead-letter handling.
- **Caching** — summarize ART-18's caching report. State the cache technologies, the cache strategies, and the TTL policies.
- **Scaling** — state the scaling strategy (horizontal, vertical, auto-scaling) from ART-04's IaC. State the bottlenecks (from ART-18) and the mitigation.
- **Backup and recovery** — state the backup procedure (when persistence is present, from ART-20) and the recovery procedure.
- **Rebuild steps** — ordered, atomic steps to set up the operational tooling: (1) install the logging framework, (2) configure the log levels and format, (3) install the monitoring agent, (4) export the metrics, (5) configure the alerting, (6) document the runbook for each error category.

### 6.9 Engineering Decisions Section

Author the Engineering Decisions section from ART-24. The section answers: what significant decisions did the original authors make, and why? For each decision (`DEC-XX`) in ART-24 with `confidence: HIGH` or `MEDIUM`:

- State the decision title.
- State the alternatives considered (`ALT-XX`).
- State the trade-off (gained and sacrificed, from `TO-XX`).
- State the context constraints (`CTX-XX`).
- State the decision kind (`documented` or `inferred`) and the confidence.
- State the rebuild recommendation: "Preserve this decision in the rebuild because <rationale>." OR "Revisit this decision in the rebuild because <rationale>." (The latter is for `LOW` confidence decisions or decisions with known better alternatives as of the rebuild date.)

`LOW` confidence decisions are listed in a separate subsection with a caveat: "These decisions are inferred from weak evidence; the rebuild may deviate from them without contradicting the original intent."

### 6.10 Reconstructive-Mode Filter

Apply the reconstructive-mode filter to every section. The filter is a per-paragraph check:

- Does the paragraph answer "how would I build this?" If yes, it passes.
- Does the paragraph only answer "what is this?" If yes, it FAILS the filter. Rewrite the paragraph to add rebuild steps or rebuild recommendations. If the paragraph is purely descriptive (e.g., "The system uses PostgreSQL"), rewrite it as "Use PostgreSQL as the primary database (per ART-20) because <context>; install via <procedure>; configure via <config>."

Paragraphs that fail the reconstructive-mode filter are `MAJOR` findings per Q-26.B.

### 6.11 Diagram Embedding

Embed diagrams from ART-25 in the handbook. Each diagram is embedded by relative path:

```markdown
**Diagram D-XX: <Title>**

![D-XX](../diagrams/<engagement_id>_ART25_D-XX.svg)

Source: [ART-25](../phase3/ART25_<engagement_id>_diagrams.md#diagram-d-xx)
```

When the SVG is not rendered (per ART-25's `RENDERING_SKIPPED`), embed the Mermaid source directly:

```markdown
**Diagram D-XX: <Title>**

```mermaid
<mermaid-source>
```

Source: [ART-25](../phase3/ART25_<engagement_id>_diagrams.md#diagram-d-xx)
```

The Mermaid source is read from the sidecar file `<engagement_id>_ART25_D-XX.mmd`.

### 6.12 Coverage Cross-Check

Cross-check the handbook against the upstream artifacts:

1. Every claim in the handbook MUST cite an upstream artifact (ART-01 through ART-25). Uncited claims are `CITATION_GAP` findings per Q2.
2. Every `M-XX` in ART-06 MUST appear in the Module-by-Module Rebuild Guide. Missing modules are `COVERAGE_GAP` findings per Q-26.D.
3. Every `DEC-XX` in ART-24 with `confidence: HIGH` or `MEDIUM` MUST appear in the Engineering Decisions section. Missing decisions are `COVERAGE_GAP` findings.
4. Every architectural pattern from ART-23 MUST be reflected in the Architecture section.
5. When ART-20 is present, every `ENT-XX` MUST appear in the Data Model Reconstruction. Missing entities are `COVERAGE_GAP` findings.
6. When ART-21 is present, every `W-XX` MUST appear in a module's AI-component subsection. Missing workflows are `COVERAGE_GAP` findings.

---

## 7. Required Outputs

### ART-26 — Rebuild Guide & Architecture Handbook

**Type:** Handbook.

**Acceptance Criteria:**

- AC-26.1: The artifact file exists at `<output_root>/artifacts/phase4/ART26_<engagement_id>_rebuild-guide.md`.
- AC-26.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.6 (Handbook schema, including `reconstructive: true` and `reconstruction_steps`).
- AC-26.3: The body contains the nine mandatory sections in order: System Overview, Architecture, Module-by-Module Rebuild Guide, Data Model Reconstruction, Configuration Reconstruction, Deployment Reconstruction, Testing Strategy, Operational Concerns, Engineering Decisions. Plus Traceability Index, Open Questions, Cross-References.
- AC-26.4: Every section passes the reconstructive-mode filter (§ 6.10).
- AC-26.5: Every module (`M-XX`) in ART-06 appears in the Module-by-Module Rebuild Guide.
- AC-26.6: Every embedded diagram cites its source (ART-25) by relative path.
- AC-26.7: Every claim cites an upstream artifact.
- AC-26.8: Every `DEC-XX` in ART-24 with `confidence: HIGH` or `MEDIUM` appears in the Engineering Decisions section.
- AC-26.9: The handbook passes the reconstructability self-test in § 9.A.

---

## 8. Output Templates

### 8.1 ART-26 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-26
artifact_type: Handbook
producing_prompt: PROMPT_26
phase: 4
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
reconstructive: true
reconstruction_steps:
  - section: System Overview
    steps: [<text>]
  - section: Architecture
    steps: [<text>]
  - section: Module-by-Module Rebuild Guide
    steps: [<text>]
  - section: Data Model Reconstruction
    steps: [<text>]
  - section: Configuration Reconstruction
    steps: [<text>]
  - section: Deployment Reconstruction
    steps: [<text>]
  - section: Testing Strategy
    steps: [<text>]
  - section: Operational Concerns
    steps: [<text>]
  - section: Engineering Decisions
    steps: [<text>]
modules_covered: [M-XX]
decisions_covered: [DEC-XX]
diagrams_embedded: [D-XX]
coverage_cross_check:
  modules_in_art06: [M-XX]
  modules_in_art26: [M-XX]
  missing_modules: [M-XX]
  decisions_in_art24: [DEC-XX]
  decisions_in_art26: [DEC-XX]
  missing_decisions: [DEC-XX]
  contradictions: [{ kind: <text>, entity: <id>, detail: <text> }]
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

### 8.2 ART-26 Body Skeleton

```markdown
# ART-26: Rebuild Guide & Architecture Handbook

## 1. Executive Summary
## 2. Methodology
## 3. System Overview
   ### 3.1 Purpose
   ### 3.2 Users and Personas
   ### 3.3 Scope and Boundary
   ### 3.4 High-Level Architecture
## 4. Architecture
   ### 4.1 System Context (C4 L1)
   **Diagram D-CTX-01: System Context**
   <embedded diagram or mermaid source>
   ### 4.2 Container Diagram (C4 L2)
   ### 4.3 Component Diagrams (C4 L3)
   ### 4.4 Architectural Pattern
   ### 4.5 Rebuild Steps
## 5. Module-by-Module Rebuild Guide
   ### 5.1 Module M-01: <name>
   - Purpose: <text> (cite ART-06, ART-09)
   - Dependencies: <list> (cite ART-10)
   - Internal Structure: <text> (embed D-COMP-XX)
   - Public API: <list> (cite ART-08, ART-09)
   - Key Algorithms: <pseudocode> (cite ART-09, ART-12)
   - Rebuild Steps:
     1. <step> (cite ART-XX)
     2. <step>
     ...
   ### 5.2 Module M-02: <name>
   ...
## 6. Data Model Reconstruction
   (or "## 6. Data Model Reconstruction — Degraded: ART-20 absent" with type-level reconstruction)
## 7. Configuration Reconstruction
## 8. Deployment Reconstruction
   (or "## 8. Deployment Reconstruction — Degraded: no IaC" with manual instructions)
## 9. Testing Strategy
## 10. Operational Concerns
## 11. Engineering Decisions
   ### 11.1 DEC-01: <title>
   - Alternatives: <list>
   - Trade-off: gained <list>, sacrificed <list>
   - Context: <list>
   - Kind: documented | inferred (confidence: HIGH/MEDIUM/LOW)
   - Rebuild recommendation: <text>
   ### 11.2 Low-Confidence Decisions
## 12. Coverage Cross-Check
## 13. Traceability Index
## 14. Open Questions
## 15. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every module, every `HIGH`/`MEDIUM` decision, every architectural pattern appears in the handbook. Threshold ≥ 0.95.
- **Q2. Citation Check** — ≥ 0.95 of claims cite an upstream artifact.
- **Q3. Schema Conformance Check** — validates against § 4.6 (Handbook schema, including `reconstructive: true`).
- **Q4. Non-Contradiction Check** — no handbook claim contradicts an upstream artifact.
- **Q5. UNVERIFIED Accounting** — every `LOW` confidence decision and every degraded section has a corresponding Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.3 on a 5% sample of modules yields the same rebuild steps.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-26.A. Reconstructive Completeness** — every section contains at least one "Rebuild Steps" subsection with ordered, atomic steps. A section without rebuild steps fails the reconstructive-mode filter and is a `BLOCKING` finding.
- **Q-26.B. Reconstructive-Mode Filter** — every paragraph answers "how would I build this?" Paragraphs that only answer "what is this?" are `MAJOR` findings.
- **Q-26.C. Module Coverage** — every `M-XX` in ART-06 appears in the Module-by-Module Rebuild Guide. Missing modules are `BLOCKING` findings.
- **Q-26.D. Decision Coverage** — every `DEC-XX` in ART-24 with `confidence: HIGH` or `MEDIUM` appears in the Engineering Decisions section. Missing decisions are `MAJOR` findings.
- **Q-26.E. Diagram Embedding** — every applicable diagram from ART-25 is embedded with a relative-path citation. Missing diagrams are `MAJOR` findings.
- **Q-26.F. Pseudocode Non-Verbatim** — the key-algorithms pseudocode is NOT a verbatim copy of the source. Verbatim copies violate `MISSION.md` § 5 anti-goal. Pseudocode MUST be a behavioral reconstruction. Verbatim copies are `BLOCKING` findings.
- **Q-26.G. Rebuild-Step Atomicity** — each rebuild step is atomic (one action per step) and verifiable (a predicate can confirm the step was executed). Non-atomic or unverifiable steps are `MINOR` findings.
- **Q-26.H. Reconstructability Self-Test** — the agent selects one module at random and simulates the rebuild: reads the module's subsection, performs the rebuild steps mentally, and verifies the result matches the public API and key algorithms. Failure of the self-test is a `BLOCKING` finding.

### 9.A Reconstructability Self-Test Procedure

The reconstructability self-test procedure (referenced by Q-26.H and by PROMPT_30 § 7.4):

1. Select one module (`M-XX`) at random from ART-06.
2. Read the module's subsection in ART-26.
3. Mentally execute each rebuild step in order.
4. For each step, verify: (a) the step is actionable (the engineer knows what to do), (b) the step's inputs are available (the engineer has the prerequisite artifacts), (c) the step's output is checkable (the engineer can verify the step succeeded).
5. After all steps, verify the reconstructed module: (a) the module's public API matches ART-09's exported functions, (b) the module's key algorithms match ART-09's function-level analysis, (c) the module's dependencies match ART-10.
6. If any step fails verification, record the failure as a `BLOCKING` finding with the failing step and the verification gap.
7. The self-test is recorded in the artifact's `reconstruction_steps` field with the tested module's ID and the result (`PASS` or `FAIL` with details).

---

## 10. Common Pitfalls

- Do not write descriptive-only sections; every section MUST contain rebuild steps. Descriptive-only sections fail Q-26.B.
- Always derive rebuild steps from upstream artifacts; inventing rebuild steps not grounded in ART-09/ART-12 violates R22.
- Do not copy source code verbatim into the key-algorithms pseudocode; verbatim copies violate `MISSION.md` § 5 anti-goal and fail Q-26.F. Use pseudocode.
- Always cite the upstream artifact for every claim; uncited claims fail Q2.
- Do not omit modules from the Module-by-Module Rebuild Guide; missing modules fail Q-26.C.
- Always include `HIGH` and `MEDIUM` confidence decisions; omitting them fails Q-26.D.
- Do not embed diagrams without source citations; uncited diagrams fail Q-26.E.
- Always pass the reconstructability self-test; failure is `BLOCKING` per Q-26.H.
- Do not over-degrade when ART-20 or ART-21 is absent; degraded sections still contain rebuild steps for the type-level or non-AI components.
- Always preserve the original engineering decisions' rationale; the rebuild should respect the original intent unless a decision is `LOW` confidence.
- Do not exceed the handbook's scope; the handbook is reconstructive, not prescriptive. Recommendations to improve the system are forbidden (per `MISSION.md` § 4 anti-goal: "Performance notes are descriptive, not prescriptive").

---

## 11. Handoff Criteria

PROMPT_27, PROMPT_28, PROMPT_29, and PROMPT_30 consume ART-26. Handoff requires ALL of:

- HC-26.1: ART-26 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-26.2: All nine mandatory sections are present and pass the reconstructive-mode filter.
- HC-26.3: Every `M-XX` in ART-06 appears in the Module-by-Module Rebuild Guide.
- HC-26.4: Every `DEC-XX` in ART-24 with `confidence: HIGH` or `MEDIUM` appears in the Engineering Decisions section.
- HC-26.5: Every applicable diagram from ART-25 is embedded.
- HC-26.6: The reconstructability self-test (§ 9.A) passes.
- HC-26.7: `repository_fingerprint_recheck` matches ART-01.
- HC-26.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_27 (Developer Handbook — references ART-26 for architecture context), PROMPT_28 (Cross-Reference Checklists — verifies that every `M-XX` and `DEC-XX` referenced by ART-26 resolves to upstream entries), PROMPT_29 (Final Documentation Assembly — embeds ART-26 in the assembled document), PROMPT_30 (Self-Review & QA — runs the reconstructability test against ART-26 per § 7.4).
- **Depends on:** ART-01 through ART-25 (all prior artifacts).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22 (no behavior invention — restricts rebuild-step recommendations to evidence-bound procedures), R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.6; Handbook bar (aggregate ≥ 32, Reconstructive completeness ≥ 4, Readability ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4 (sidecar files), § 3.1, § 4, § 5 (body structure: 7 standard sections, extended to 9 for handbooks), § 6, § 7 (Mermaid conventions), § 8 (cross-reference conventions).
- **Forward reference:** PROMPT_30 (Self-Review & QA) runs the reconstructability test (§ 7.4) by attempting to reconstruct one module from ART-26 alone; failure is `BLOCKING`.

*End of PROMPT_26. Orchestrator may dispatch PROMPT_27 upon satisfaction of § 11.*
