# PROMPT_27.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_27: Developer Handbook

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_27
- **Phase:** 4
- **Stage:** 2 of 5
- **Dependencies:** ART-01, ART-03, ART-04, ART-06, ART-09, ART-15, ART-17 (primary); plus ART-02, ART-05, ART-08, ART-10, ART-11, ART-13, ART-14, ART-16, ART-18, ART-19, ART-20, ART-21, ART-22, ART-23, ART-24, ART-25, ART-26 (supporting).
- **Estimated Tokens:** 18000–28000
- **Output Artifacts:** ART-27 (Handbook) — Developer Handbook.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Developer Handbook artifact (ART-27) that synthesizes the engagement's findings into an operational guide for a new engineer joining the project — covering the ten mandatory sections (Project Orientation, Repository Layout, Environment Setup, Development Workflow, Coding Conventions, Module Walkthroughs, Common Tasks, Debugging Guide, Testing Guide, Glossary) — where every section answers "how do I work in this codebase?" and which enables a new engineer to make their first contribution within one working day.

---

## 3. When to Invoke

PROMPT_27 is dispatched when ALL of the following predicates hold:

- Phase 3 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.4.
- PROMPT_26 has emitted its completion record. ART-26 may be `DRAFT` with orchestrator waiver; PROMPT_27 references ART-26 for architecture context but does not depend on its completion for handoff.
- ART-01, ART-03, ART-04, ART-06, ART-09, ART-15, and ART-17 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver. These are the primary inputs; supporting inputs (ART-02, ART-05, etc.) are optional but recommended.
- The engagement manifest's scope modifier is NOT `SCOPE_TRIAGE`.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. Boundary declaration drives the Project Orientation's scope statement. |
| ART-03 | Map | Folder & file tree; the Repository Layout section's navigation guide. |
| ART-04 | Spec | Build & configuration; the Environment Setup section's prerequisites, install steps, env vars, and the Testing Guide's test-runner invocation. |
| ART-06 | Map | Module map; the Module Walkthroughs section's per-module subsections. |
| ART-09 | Doc | Function catalog; the Module Walkthroughs section's function-level guidance and the Common Tasks section's step derivation. |
| ART-15 | Doc | API & interface reference; the Common Tasks section's "add an endpoint" procedure and the Debugging Guide's request-tracing procedure. |
| ART-17 | Doc | Error handling & resilience; the Debugging Guide's common failure modes and the Module Walkthroughs' error-handling guidance. |
| ART-02 | Manifest | Tech stack & dependencies; the Environment Setup's prerequisite versions and the Coding Conventions' framework-specific idioms. |
| ART-05 | Map | Entry points & bootstrap; the Environment Setup's "running locally" procedure. |
| ART-08 | Doc | Class & interface catalog; the Coding Conventions' class-design patterns. |
| ART-10 | Graph | Call & dependency graphs; the Module Walkthroughs' dependency guidance. |
| ART-11 | Graph | Data flow diagrams; the Debugging Guide's data-flow tracing procedure. |
| ART-13 | Doc | State machine catalog; the Debugging Guide's state-related failure modes. |
| ART-14 | Doc | Event workflow catalog; the Debugging Guide's event-related failure modes. |
| ART-16 | Doc | Middleware & pipeline map; the Common Tasks' "add a middleware" procedure. |
| ART-18 | Doc | Caching & performance; the Debugging Guide's cache-related failure modes. |
| ART-19 | Doc | Auth report (optional); the Debugging Guide's auth-related failure modes. |
| ART-20 | Doc | Persistence report (optional); the Common Tasks' "add a migration" procedure. |
| ART-21 | Doc | AI/LLM workflow report (optional); the Module Walkthroughs' AI-module guidance. |
| ART-22 | Doc | Streaming workflow report (optional); the Debugging Guide's streaming-related failure modes. |
| ART-23 | Doc | Design pattern catalog; the Coding Conventions' pattern-usage guidance. |
| ART-24 | Doc | Engineering decision record; the Coding Conventions' "why this convention exists" explanations. |
| ART-25 | Diagrams | The Repository Layout's folder tree diagram and the Module Walkthroughs' component diagrams. |
| ART-26 | Handbook | The Project Orientation's architecture summary (cross-reference, not re-derive). |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid embedding (§ 7), Handbook schema (§ 4.6). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Handbook schema (§ 4.6) and type-specific bar (aggregate ≥ 32, Reconstructive completeness ≥ 4 [applied as Operational completeness], Readability ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Author the Project Orientation section per § 6.1.
3. Author the Repository Layout section per § 6.2 with embedded folder tree from ART-25.
4. Author the Environment Setup section per § 6.3 from ART-04 and ART-02.
5. Author the Development Workflow section per § 6.4 from ART-04's CI/CD pipeline.
6. Author the Coding Conventions section per § 6.5 from ART-08, ART-23, ART-24.
7. Author the Module Walkthroughs section per § 6.6 — for each major module, document where it is, what it does, and how to extend it.
8. Author the Common Tasks section per § 6.7 — derive step-by-step procedures for the canonical task set.
9. Author the Debugging Guide section per § 6.8 from ART-17, ART-11, ART-13, ART-14, ART-18, ART-22.
10. Author the Testing Guide section per § 6.9 from ART-04's test configuration.
11. Author the Glossary section per § 6.10 — define every domain-specific and framework-specific term used in the handbook.
12. Apply the operational-mode filter per § 6.11 — every section MUST answer "how do I work in this codebase?".
13. Embed diagrams from ART-25 per § 6.12 with relative-path links.
14. Cross-check the handbook against the upstream artifacts per § 6.13.
15. Emit ART-27 per § 8 with full front-matter, all ten sections, traceability index, open questions.
16. Run the Quality Checks in § 9.
17. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Project Orientation Section

Author the Project Orientation. The section answers: what does the system do, who uses it, where do I fit in? Subsections:

- **What the system does** — one paragraph derived from ART-01's repository metadata and ART-05's entry points. Cite ART-01 and ART-05.
- **Who uses it** — derived from ART-15's API consumers. Each user category records its name and primary use case.
- **Where the new engineer fits in** — one paragraph explaining the engineer's expected contribution type (feature work, bug fixes, maintenance, on-call). Derived from ART-06's module map (which modules are most active).
- **Architecture in one paragraph** — cross-reference ART-26's Architecture section. Do not re-derive; cite ART-26.

### 6.2 Repository Layout Section

Author the Repository Layout. The section answers: how do I navigate this codebase? Subsections:

- **Top-level directory map** — list every top-level directory with its purpose (from ART-03's folder-purpose inference). Each entry cites ART-03.
- **Folder tree diagram** — embed `D-TREE-01` from ART-25.
- **Where to find things** — a "cheat sheet" mapping common needs to locations: "To find an API endpoint, look in `src/controllers/` (per ART-15)"; "To find a database entity, look in `src/models/` (per ART-20)"; "To find a configuration value, look in `config/` or `process.env` (per ART-04)".
- **Generated files** — list the directories that contain generated files (per ART-03's generated-file detection) and warn the engineer not to edit them.

### 6.3 Environment Setup Section

Author the Environment Setup from ART-04 and ART-02. The section answers: how do I get this running locally? Subsections:

- **Prerequisites** — list the required runtime versions (Node, Python, Java, Go, Rust) from ART-02. List the required package manager (npm, yarn, pnpm, pip, poetry, maven, gradle, cargo, go mod) from ART-02. List the required external services (PostgreSQL, Redis, Kafka) from ART-20 and ART-18.
- **Install steps** — ordered, atomic steps to install the dependencies. Example: (1) `nvm use 20`, (2) `npm install`, (3) `cp .env.example .env`, (4) fill in the env vars (do NOT commit real secrets), (5) `docker compose up -d postgres redis`, (6) `npm run db:migrate`, (7) `npm run dev`. Each step cites ART-04's package scripts.
- **Environment variables** — list every env var from ART-04's env-var extraction. For each, state the name, the purpose, the default, the required-ness, and the example value (with `REDACTED` for secrets per `MISSION.md` § 5).
- **Running locally** — the command to start the application locally. Cite ART-05's entry-point analysis.
- **Running tests** — the command to run the test suite. Cite ART-04's test scripts. Cross-reference § 6.9.

### 6.4 Development Workflow Section

Author the Development Workflow from ART-04's CI/CD pipeline. The section answers: what is the contribution workflow? Subsections:

- **Branching strategy** — state the branching model (GitFlow, GitHub Flow, Trunk-Based Development) inferred from ART-04's CI triggers and ART-01's VCS. If not inferable, state "Branching strategy not detected; consult the team's CONTRIBUTING.md."
- **Commit conventions** — state the commit-message convention (Conventional Commits, etc.) inferred from ART-01's git history (when available). Provide examples.
- **Pull request workflow** — state the PR template (when present in `.github/pull_request_template.md`), the required reviewers (when declared in CODEOWNERS), the required CI checks (from ART-04's CI pipeline).
- **CI pipeline** — state the CI stages (lint, test, build, deploy) from ART-04. State the expected duration and the common failure points.
- **Code review expectations** — state the review criteria inferred from the codebase's patterns (ART-23) and conventions (ART-08). Example: "Reviewers expect new code to follow the Repository pattern (per ART-23's P-XX); direct ORM usage in services is flagged."

### 6.5 Coding Conventions Section

Author the Coding Conventions from ART-08, ART-23, ART-24. The section answers: how do I write code that fits in? Subsections:

- **Naming conventions** — file naming (kebab-case, camelCase, PascalCase, snake_case) from ART-03's naming-convention detection. Class naming from ART-08. Function naming from ART-09. Variable naming from ART-08.
- **Structural conventions** — class structure (fields, constructor, methods order) from ART-08. Module structure (imports, exports, helpers) from ART-06. File structure (one class per file, etc.) from ART-03.
- **Pattern usage** — for each pattern in ART-23, state where it is used and the convention for applying it. Example: "Repository pattern (P-XX) is used for all persistence; new entities MUST have a repository in `src/repositories/`."
- **Framework-specific idioms** — for each framework in ART-02, state the idiomatic usage observed in the codebase. Example: "Express middleware is registered via `app.use()` in `src/app.ts`; new middleware MUST follow the `(req, res, next) => {}` signature."
- **Why this convention exists** — for each convention, cite the relevant `DEC-XX` from ART-24 that explains the rationale. This subsection is critical for engineer buy-in; conventions without rationale are followed grudgingly.

### 6.6 Module Walkthroughs Section

Author the Module Walkthroughs. For each major module (`M-XX` in ART-06), produce a subsection answering: where is this module, what does it do, how do I extend it? Each module subsection contains:

- **Where it is** — the module's directory (from ART-06). Cite ART-06.
- **What it does** — one paragraph derived from ART-06's module map and ART-09's function catalog.
- **How to extend it** — ordered, atomic steps for the most common extension tasks. Examples: "To add a new endpoint: (1) create a controller method in `src/controllers/<module>Controller.ts`, (2) register the route in `src/routes/<module>.ts`, (3) add the route's auth requirement per ART-19, (4) write a test in `tests/<module>.test.ts`." Each step cites the upstream artifact.
- **Common pitfalls** — derived from ART-17's error-handling report. State the common mistakes that lead to errors in this module.
- **Cross-references** — link to ART-26's Module-by-Module Rebuild Guide subsection for the same module.

The Module Walkthroughs section is the longest section. It is decomposed by module; each module is a top-level subsection.

### 6.7 Common Tasks Section

Author the Common Tasks section. The section answers: how do I do X? The canonical task set:

- **Add an endpoint** — ordered steps derived from ART-15's API reference and ART-06's module map. Cite ART-15.
- **Add a component** (when ART-07 is present) — ordered steps for adding a UI component.
- **Add a migration** (when ART-20 is present) — ordered steps for adding a database migration. Cite ART-20.
- **Add a tool** (when ART-21 is present) — ordered steps for adding an LLM tool integration. Cite ART-21.
- **Add a middleware** — ordered steps derived from ART-16's middleware map. Cite ART-16.
- **Add an event handler** (when ART-14 is present) — ordered steps for adding an event handler. Cite ART-14.
- **Add a state transition** (when ART-13 is present) — ordered steps for adding a state transition. Cite ART-13.
- **Add a configuration item** — ordered steps for adding a configuration value. Cite ART-04.
- **Add a test** — ordered steps for adding a test. Cite ART-04.
- **Debug a failing request** — ordered steps cross-referencing § 6.8.

Each task is a numbered procedure with explicit file paths and code patterns. Tasks that depend on absent artifacts (e.g., "add a migration" when ART-20 is absent) are omitted with a note.

### 6.8 Debugging Guide Section

Author the Debugging Guide from ART-17, ART-11, ART-13, ART-14, ART-18, ART-22. The section answers: something is broken, where do I look? Subsections:

- **Where the logs are** — state the log destination (file path, stdout, log aggregator) from ART-04's logging configuration. State the log levels and their meanings.
- **How to trace a request** — ordered steps to trace a request from entry to response, using ART-11's data-flow diagrams and ART-10's call graph. Cite ART-11 and ART-10.
- **Common failure modes** — for each error category in ART-17, state the symptom, the cause, and the diagnostic procedure. Each entry cites ART-17.
- **State-related failures** (when ART-13 is present) — for each state machine, state the common stuck-state scenarios and the recovery procedure. Cite ART-13.
- **Event-related failures** (when ART-14 is present) — for each event, state the common dropped-event scenarios and the diagnostic procedure. Cite ART-14.
- **Cache-related failures** (when ART-18 is present) — state the common stale-cache scenarios and the invalidation procedure. Cite ART-18.
- **Auth-related failures** (when ART-19 is present) — state the common auth-failure scenarios (expired token, insufficient scope) and the diagnostic procedure. Cite ART-19. Do NOT document auth-bypass procedures; that would violate `MISSION.md` § 4 anti-goal.
- **Streaming-related failures** (when ART-22 is present) — state the common backpressure-failure scenarios and the diagnostic procedure. Cite ART-22.
- **Debugging tools** — list the debugging tools available (Chrome DevTools, node --inspect, pdb, jdb, dlv) and the breakpoints that are most useful per module.

### 6.9 Testing Guide Section

Author the Testing Guide from ART-04's test configuration. The section answers: how do I test my changes? Subsections:

- **Test structure** — state the test directory structure and naming convention (from § 6.4 of the Environment Setup, expanded).
- **How to run tests** — the commands to run unit tests, integration tests, end-to-end tests. The commands to run a single test file or a single test case.
- **How to write a test** — ordered steps with a concrete example. The example MUST be derived from an actual test in the codebase (cite ART-04's test files); do not invent a hypothetical example.
- **Test fixtures** — list the available fixtures (factories, mocks, seeds) and how to use them.
- **Coverage** — state the coverage tool (when configured) and the coverage threshold (when enforced). State the command to generate the coverage report.
- **Performance tests** — state the performance test framework (when present) and the procedure to run performance tests.

### 6.10 Glossary Section

Author the Glossary. Define every domain-specific term, framework-specific term, and project-specific abbreviation used in the handbook. Each entry records: `term`, `definition`, `citation` (the upstream artifact where the term is grounded or `EXTERNAL` for industry-standard terms with a reference URL). The Glossary MUST cover at minimum:

- Every `M-XX`'s name and its purpose.
- Every architectural pattern from ART-23.
- Every framework from ART-02.
- Every domain term appearing in ART-15's API reference (e.g., "user", "tenant", "workspace" — whatever the codebase calls its primary entities).
- Every abbreviation used in the codebase (extracted from ART-09's function names and ART-08's class names).

The Glossary is alphabetical. Terms with multiple meanings in different modules are disambiguated with the module prefix.

### 6.11 Operational-Mode Filter

Apply the operational-mode filter to every section. The filter is a per-paragraph check:

- Does the paragraph answer "how do I work in this codebase?" If yes, it passes.
- Does the paragraph only answer "what is this?" If yes, it FAILS the filter. Rewrite the paragraph to add operational guidance. If the paragraph is purely descriptive (e.g., "The system uses PostgreSQL"), rewrite it as "The system uses PostgreSQL (per ART-20). To connect to the local PostgreSQL: `psql postgres://localhost:5432/<db>` (per ART-04). To inspect the schema: `\dt`."

Paragraphs that fail the operational-mode filter are `MAJOR` findings per Q-27.B.

### 6.12 Diagram Embedding

Embed diagrams from ART-25 in the handbook per the procedure in PROMPT_26 § 6.11. The Developer Handbook embeds:

- `D-TREE-01` in the Repository Layout section.
- `D-COMP-XX` for each major module in the Module Walkthroughs section.
- `D-SEQ-REQ-01` (or the first request-lifecycle sequence diagram) in the Debugging Guide's "How to trace a request" subsection.

### 6.13 Coverage Cross-Check

Cross-check the handbook against the upstream artifacts:

1. Every `M-XX` in ART-06 appears in the Module Walkthroughs section.
2. Every common task in § 6.7 cites the upstream artifact that grounds its procedure.
3. Every error category in ART-17 appears in the Debugging Guide's common failure modes.
4. Every term used in the handbook appears in the Glossary.
5. Every claim cites an upstream artifact.

---

## 7. Required Outputs

### ART-27 — Developer Handbook

**Type:** Handbook.

**Acceptance Criteria:**

- AC-27.1: The artifact file exists at `<output_root>/artifacts/phase4/ART27_<engagement_id>_developer-handbook.md`.
- AC-27.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.6 (Handbook schema, including `reconstructive: true` [applied as `operational: true`] and `reconstruction_steps` [applied as `operational_steps`]).
- AC-27.3: The body contains the ten mandatory sections in order: Project Orientation, Repository Layout, Environment Setup, Development Workflow, Coding Conventions, Module Walkthroughs, Common Tasks, Debugging Guide, Testing Guide, Glossary. Plus Traceability Index, Open Questions, Cross-References.
- AC-27.4: Every section passes the operational-mode filter (§ 6.11).
- AC-27.5: Every module (`M-XX`) in ART-06 appears in the Module Walkthroughs section.
- AC-27.6: Every error category in ART-17 appears in the Debugging Guide.
- AC-27.7: Every claim cites an upstream artifact.
- AC-27.8: Every term used in the handbook appears in the Glossary.

---

## 8. Output Templates

### 8.1 ART-27 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-27
artifact_type: Handbook
producing_prompt: PROMPT_27
phase: 4
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
reconstructive: true
operational: true
operational_steps:
  - section: Project Orientation
    steps: [<text>]
  - section: Repository Layout
    steps: [<text>]
  - section: Environment Setup
    steps: [<text>]
  - section: Development Workflow
    steps: [<text>]
  - section: Coding Conventions
    steps: [<text>]
  - section: Module Walkthroughs
    steps: [<text>]
  - section: Common Tasks
    steps: [<text>]
  - section: Debugging Guide
    steps: [<text>]
  - section: Testing Guide
    steps: [<text>]
  - section: Glossary
    steps: [<text>]
modules_covered: [M-XX]
error_categories_covered: [<text>]
common_tasks_documented: [<text>]
glossary_terms: [<term>]
diagrams_embedded: [D-XX]
coverage_cross_check:
  modules_in_art06: [M-XX]
  modules_in_art27: [M-XX]
  missing_modules: [M-XX]
  error_categories_in_art17: [<text>]
  error_categories_in_art27: [<text>]
  missing_error_categories: [<text>]
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

### 8.2 ART-27 Body Skeleton

```markdown
# ART-27: Developer Handbook

## 1. Executive Summary
## 2. Methodology
## 3. Project Orientation
   ### 3.1 What the System Does
   ### 3.2 Who Uses It
   ### 3.3 Where the New Engineer Fits In
   ### 3.4 Architecture in One Paragraph (cross-reference ART-26)
## 4. Repository Layout
   ### 4.1 Top-Level Directory Map
   ### 4.2 Folder Tree Diagram (embed D-TREE-01)
   ### 4.3 Where to Find Things
   ### 4.4 Generated Files
## 5. Environment Setup
   ### 5.1 Prerequisites
   ### 5.2 Install Steps
   ### 5.3 Environment Variables
   ### 5.4 Running Locally
   ### 5.5 Running Tests
## 6. Development Workflow
   ### 6.1 Branching Strategy
   ### 6.2 Commit Conventions
   ### 6.3 Pull Request Workflow
   ### 6.4 CI Pipeline
   ### 6.5 Code Review Expectations
## 7. Coding Conventions
   ### 7.1 Naming Conventions
   ### 7.2 Structural Conventions
   ### 7.3 Pattern Usage
   ### 7.4 Framework-Specific Idioms
   ### 7.5 Why This Convention Exists
## 8. Module Walkthroughs
   ### 8.1 Module M-01: <name>
   - Where it is: <path> (cite ART-06)
   - What it does: <text>
   - How to extend it: <steps>
   - Common pitfalls: <list> (cite ART-17)
   - Cross-reference: ART-26 § 5.1
   ### 8.2 Module M-02: <name>
## 9. Common Tasks
   ### 9.1 Add an Endpoint
   ### 9.2 Add a Component (when applicable)
   ### 9.3 Add a Migration (when applicable)
   ### 9.4 Add a Tool (when applicable)
   ### 9.5 Add a Middleware
   ### 9.6 Add an Event Handler (when applicable)
   ### 9.7 Add a State Transition (when applicable)
   ### 9.8 Add a Configuration Item
   ### 9.9 Add a Test
   ### 9.10 Debug a Failing Request
## 10. Debugging Guide
   ### 10.1 Where the Logs Are
   ### 10.2 How to Trace a Request (embed D-SEQ-REQ-01)
   ### 10.3 Common Failure Modes
   ### 10.4 State-Related Failures (when applicable)
   ### 10.5 Event-Related Failures (when applicable)
   ### 10.6 Cache-Related Failures (when applicable)
   ### 10.7 Auth-Related Failures (when applicable)
   ### 10.8 Streaming-Related Failures (when applicable)
   ### 10.9 Debugging Tools
## 11. Testing Guide
   ### 11.1 Test Structure
   ### 11.2 How to Run Tests
   ### 11.3 How to Write a Test
   ### 11.4 Test Fixtures
   ### 11.5 Coverage
   ### 11.6 Performance Tests
## 12. Glossary
## 13. Coverage Cross-Check
## 14. Traceability Index
## 15. Open Questions
## 16. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every module, every error category, every common task appears in the handbook. Threshold ≥ 0.95.
- **Q2. Citation Check** — ≥ 0.95 of claims cite an upstream artifact.
- **Q3. Schema Conformance Check** — validates against § 4.6 (Handbook schema).
- **Q4. Non-Contradiction Check** — no handbook claim contradicts an upstream artifact or ART-26.
- **Q5. UNVERIFIED Accounting** — every degraded section (when an optional artifact is absent) has a corresponding Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.6 on a 5% sample of modules yields the same extension steps.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-27.A. Operational Completeness** — every section contains at least one operational procedure (numbered steps). A section without operational procedures fails the operational-mode filter and is a `BLOCKING` finding.
- **Q-27.B. Operational-Mode Filter** — every paragraph answers "how do I work in this codebase?" Paragraphs that only answer "what is this?" are `MAJOR` findings.
- **Q-27.C. Module Coverage** — every `M-XX` in ART-06 appears in the Module Walkthroughs section. Missing modules are `BLOCKING` findings.
- **Q-27.D. Error-Category Coverage** — every error category in ART-17 appears in the Debugging Guide. Missing categories are `MAJOR` findings.
- **Q-27.E. Glossary Completeness** — every term used in the handbook appears in the Glossary. Missing terms are `MINOR` findings.
- **Q-27.F. Diagram Embedding** — the Repository Layout embeds `D-TREE-01`; the Module Walkthroughs embed component diagrams; the Debugging Guide embeds a request-lifecycle sequence diagram. Missing diagrams are `MAJOR` findings.
- **Q-27.G. Common-Task Derivation** — every common task in § 6.7 cites the upstream artifact that grounds its procedure. Uncited tasks are `MAJOR` findings.
- **Q-27.H. Test-Example Grounding** — the "How to write a test" example in § 6.9 is derived from an actual test in the codebase, not a hypothetical example. Hypothetical examples are `MAJOR` findings per R22.
- **Q-27.I. Auth-Bypass Prohibition** — the Debugging Guide's auth-related failures do NOT document auth-bypass procedures. Documenting auth-bypass is a `BLOCKING` finding per `MISSION.md` § 4 anti-goal.

---

## 10. Common Pitfalls

- Do not write descriptive-only sections; every section MUST contain operational procedures. Descriptive-only sections fail Q-27.B.
- Always derive procedures from upstream artifacts; inventing procedures not grounded in ART-04/ART-09/ART-15 violates R22.
- Do not omit modules from the Module Walkthroughs; missing modules fail Q-27.C.
- Always include every error category from ART-17 in the Debugging Guide; missing categories fail Q-27.D.
- Always define every term in the Glossary; undefined terms are `MINOR` findings per Q-27.E.
- Do not invent hypothetical test examples; use actual tests from the codebase per Q-27.H.
- Do not document auth-bypass procedures; doing so violates `MISSION.md` § 4 and is `BLOCKING` per Q-27.I.
- Always cross-reference ART-26 for architecture context; re-deriving architecture in ART-27 duplicates effort and risks divergence.
- Do not include performance-improvement recommendations; the handbook is operational, not prescriptive, per `MISSION.md` § 4 anti-goal.
- Always cite the upstream artifact for every claim; uncited claims fail Q2.
- Always provide concrete file paths in procedures; abstract procedures ("edit the controller") are `MINOR` findings.
- Do not omit the "Why this convention exists" subsection in Coding Conventions; conventions without rationale are followed grudgingly and the rationale is the engineer's primary buy-in signal.

---

## 11. Handoff Criteria

PROMPT_28, PROMPT_29, and PROMPT_30 consume ART-27. Handoff requires ALL of:

- HC-27.1: ART-27 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-27.2: All ten mandatory sections are present and pass the operational-mode filter.
- HC-27.3: Every `M-XX` in ART-06 appears in the Module Walkthroughs section.
- HC-27.4: Every error category in ART-17 appears in the Debugging Guide.
- HC-27.5: Every term used in the handbook appears in the Glossary.
- HC-27.6: Every applicable diagram from ART-25 is embedded.
- HC-27.7: `repository_fingerprint_recheck` matches ART-01.
- HC-27.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_28 (Cross-Reference Checklists — verifies that every `M-XX` and error category referenced by ART-27 resolves to upstream entries), PROMPT_29 (Final Documentation Assembly — embeds ART-27 in the assembled document), PROMPT_30 (Self-Review & QA — verifies the operational completeness of ART-27).
- **Depends on:** ART-01, ART-03, ART-04, ART-06, ART-09, ART-15, ART-17 (primary); ART-02, ART-05, ART-08, ART-10, ART-11, ART-13, ART-14, ART-16, ART-18, ART-19, ART-20, ART-21, ART-22, ART-23, ART-24, ART-25, ART-26 (supporting).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22 (no behavior invention), R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.6; Handbook bar (aggregate ≥ 32, Reconstructive completeness ≥ 4 [applied as Operational completeness], Readability ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4 (sidecar files), § 3.1, § 4, § 5 (body structure, extended to 10 sections for developer handbooks), § 6, § 7 (Mermaid conventions), § 8 (cross-reference conventions).
- **Forward reference:** PROMPT_30 (Self-Review & QA) verifies the operational completeness of ART-27 by sampling a section and confirming the procedures are actionable.

*End of PROMPT_27. Orchestrator may dispatch PROMPT_28 upon satisfaction of § 11.*
