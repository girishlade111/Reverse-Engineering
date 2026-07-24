# PROMPT_24.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_24: Engineering Decisions & Trade-off Reconstruction

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_24
- **Phase:** 3
- **Stage:** 4 of 5
- **Dependencies:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-04 (PROMPT_04), ART-17 (PROMPT_17), ART-18 (PROMPT_18), ART-20 (PROMPT_20), ART-21 (PROMPT_21), ART-22 (PROMPT_22), ART-23 (PROMPT_23).
- **Estimated Tokens:** 14000–22000
- **Output Artifacts:** ART-24 (Doc) — Engineering Decision Record.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Engineering Decision Record artifact (ART-24) that reconstructs every significant engineering decision evident in the subject repository — architecture choices, technology choices, data choices, concurrency choices, error-handling choices, and AI choices — and for each decision records the alternatives considered (inferred from comments, dependencies, and structure), the trade-off (what was gained and what was sacrificed), the context (constraints that drove the choice), and the decision kind (documented or inferred) — so that a downstream engineer can preserve the original intent when rebuilding.

---

## 3. When to Invoke

PROMPT_24 is dispatched when ALL of the following predicates hold:

- Phase 2 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.3.
- PROMPT_21, PROMPT_22, and PROMPT_23 have emitted their completion records. ART-21, ART-22, and ART-23 may be `NOT_PRODUCED` under skipped behavior; PROMPT_24 MUST degrade gracefully by skipping the corresponding decision-category analysis (§ 6.7 AI choices when ART-21 is absent; § 6.4 streaming choices when ART-22 is absent) and recording Open Questions.
- ART-01, ART-02, ART-04, ART-17, and ART-18 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-20 may be `NOT_PRODUCED` under skipped behavior; when absent, data-choice analysis (§ 6.3) is degraded to technology-only decisions and an Open Question is recorded.

PROMPT_24 is not gated by a marker-detection trigger; every codebase embodies some engineering decisions, so the prompt always produces ART-24 (possibly with `decision_count: 0` and a rationale that no significant decisions were detected — rare).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-02 | Manifest | Tech stack & dependencies; technology choices are reconstructed from the declared dependencies and their versions. The presence of competing dependencies (e.g., both `sequelize` and `typeorm`) is a strong signal of a technology decision. |
| ART-04 | Spec | Build & configuration map; configuration choices (env vars, config layers, feature flags) are engineering decisions. |
| ART-17 | Doc | Error-handling & resilience report; error-handling choices (exceptions vs results, retry policies, circuit breakers) are reconstructed here. |
| ART-18 | Doc | Caching & performance report; caching choices (cache-aside, write-through, TTL strategies) are reconstructed here. |
| ART-20 | Doc | Database & persistence report; data choices (SQL vs NoSQL, normalization level, schema design) are reconstructed here. When `ABSENT`, data-choice analysis is degraded. |
| ART-21 | Doc | AI/LLM workflow report; AI choices (which model, which pattern, which parameters) are reconstructed here. When `ABSENT`, AI-choice analysis is skipped. |
| ART-22 | Doc | Streaming workflow report; streaming/backpressure choices are reconstructed here. When `ABSENT`, streaming-choice analysis is skipped. |
| ART-23 | Doc | Design pattern catalog; pattern choices are reconstructed here. Each `P-XX` with `rationale_kind: documented` or `inferred` is a pattern-choice decision. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22 (no behavior invention — restricts inferred trade-offs), R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (§ 4.5) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Enumerate every decision point per § 6.1 by scanning ART-02, ART-04, ART-17, ART-18, ART-20, ART-21, ART-22, ART-23 for decision candidates.
3. For each decision point, reconstruct the alternatives per § 6.2 (inferred from comments, dependencies, structure).
4. For each decision point, analyze the trade-off per § 6.3 (what was gained, what was sacrificed).
5. For each decision point, infer the context per § 6.4 (constraints that drove the choice).
6. Classify each decision by category per § 6.5 (architecture, technology, data, concurrency, error-handling, caching, AI, streaming, pattern).
7. Detect ADRs and inline decision comments per § 6.6; mark these as `decision_kind: documented`.
8. For AI choices (when ART-21 is present), reconstruct model-choice, pattern-choice, parameter-choice decisions per § 6.7.
9. For streaming choices (when ART-22 is present), reconstruct backpressure-choice, buffer-choice, error-handling-choice decisions per § 6.8.
10. For each decision, assign a confidence level per § 6.9 (HIGH | MEDIUM | LOW based on evidence strength).
11. Emit Mermaid decision-context diagrams per § 6.10.
12. Cross-check the decision catalog against ART-02's dependency list and ART-23's pattern instances per § 6.11; decisions referencing unlisted dependencies or patterns are `CONTRADICTION` findings per R33.
13. Emit ART-24 per § 8 with full front-matter, per-category sections, per-decision subsections, traceability index, open questions.
14. Run the Quality Checks in § 9.
15. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Decision-Point Identification

Enumerate every decision point. A decision point is a locus where the original authors chose among viable alternatives. Decision points are identified by scanning the consuming artifacts for:

- **Dependency signals** — every dependency in ART-02 represents a technology choice. Competing dependencies (e.g., both `jest` and `mocha`) signal a partially-migrated decision. Deprecated dependencies signal a deferred decision.
- **Configuration signals** — every configuration item in ART-04 (env var, config-file entry, feature flag) represents a choice. Boolean flags (`USE_NEW_ROUTER=true`) signal an in-flight decision.
- **Pattern signals** — every `P-XX` in ART-23 is a pattern-choice decision. Architectural patterns (`MVC`, `Hexagonal`, `CQRS`) are high-impact decisions.
- **Error-handling signals** — every error-handling strategy in ART-17 (exceptions vs results, retry policy, circuit breaker, bulkhead) is a decision.
- **Caching signals** — every cache strategy in ART-18 (cache-aside, write-through, write-back, TTL) is a decision.
- **Persistence signals** (when ART-20 is present) — every persistence model (`PS-XX`), every ORM (`ORM-XX`), every transaction boundary (`TXN-XX`) is a decision.
- **AI signals** (when ART-21 is present) — every LLM client (`CL-XX`), every agent architecture (`AG-XX`), every RAG pipeline (`RAG-XX`), every model parameter (`LC-XX.parameters`) is a decision.
- **Streaming signals** (when ART-22 is present) — every backpressure strategy, every buffer strategy, every error-handling strategy is a decision.
- **ADR signals** — files in `docs/adr/`, `docs/architecture/decisions/`, `decisions/` directories; ADRs numbered `NNNN-title.md`.
- **Comment signals** — inline comments of the form `// We use X because ...`, `# Chose X over Y because ...`, `/* Decision: use X */`.

Each decision point records: `decision_id` `DEC-XX`, `category`, `title` (one sentence), `decision_kind` (documented | inferred), `confidence` (HIGH | MEDIUM | LOW), `evidence` (list of citations), `participants` (list of `M-XX`/`K-XX`/`FN-XX`/`CL-XX`/`AG-XX` involved), `citation`.

### 6.2 Alternative Reconstruction

For each decision, reconstruct the alternatives considered. Alternatives are inferred from:

- **Documented alternatives** — ADRs often enumerate alternatives explicitly ("Considered: Y, Z; chose X because ..."). Inline comments sometimes mention alternatives ("We used X instead of Y because ...").
- **Dependency-presence alternatives** — competing dependencies in ART-02 suggest the alternatives. If both `sequelize` and `typeorm` are present, the alternative to using TypeORM is using Sequelize (and vice versa).
- **Structural alternatives** — the absence of a pattern suggests the alternative. If the codebase uses Repository pattern, the alternative is direct ORM usage in services.
- **Ecosystem-standard alternatives** — for common decisions, the ecosystem-standard alternatives are reconstructed. For "use PostgreSQL," the standard alternatives are MySQL, SQLite, MongoDB. These reconstructions are marked `decision_kind: inferred` and `confidence: MEDIUM` unless documented.
- **Convention alternatives** — for language-convention decisions (e.g., "use exceptions over Result types in Rust"), the alternative is the language-idiomatic opposite.

Each alternative records: `alternative_id` `ALT-XX`, `decision_id`, `name`, `rejection_rationale` (when documented), `rejection_evidence` (citation or `INFERRED_FROM_ECOSYSTEM_STANDARD`), `citation`. Alternatives reconstructed from ecosystem standards MUST be marked `decision_kind: inferred` and `confidence: LOW` to distinguish weakly-evidenced alternatives from documented ones.

### 6.3 Trade-off Analysis

For each decision, analyze the trade-off. The trade-off has two parts:

- **Gained** — what the chosen option provides. The gained properties are inferred from the option's typical benefits and from the codebase's actual usage. Example: choosing Repository pattern gains testability (services depend on interfaces, not ORM types) and persistence-technology independence. The gained properties MUST cite the codebase's actual usage that realizes the benefit (e.g., `tests/user.service.test.ts:14` shows the service being tested with a mock repository).
- **Sacrificed** — what the chosen option gives up. The sacrificed properties are inferred from the alternatives' typical benefits. Example: choosing Repository pattern sacrifices direct ORM-query expressiveness (every query must be expressed as a repository method). The sacrificed properties MUST cite the codebase's actual limitations (e.g., absence of ad-hoc queries in services) OR be marked `INFERRED_FROM_PATTERN_TAXONOMY` with hedged language.

Each trade-off records: `trade_off_id` `TO-XX`, `decision_id`, `gained` (list of properties), `sacrificed` (list of properties), `evidence_citations` (list of citations), `inference_kind` (documented | inferred-from-usage | inferred-from-taxonomy).

Trade-offs MUST NOT assert benefits the codebase does not realize. Inferring "Repository pattern gains performance" without evidence is forbidden per R22. Inferring "Repository pattern gains testability" is permitted when the codebase's tests actually exercise the repository abstraction; the test file's citation is required.

### 6.4 Context Inference

For each decision, infer the context — the constraints that drove the choice. Context is reconstructed from:

- **Declared constraints** — env vars, config comments, ADRs that explicitly state constraints. Example: ADR `0007-use-postgres.md` says "We require ACID transactions for financial data."
- **Domain-implied constraints** — the codebase's domain (financial, healthcare, real-time, batch) implies certain constraints. A financial codebase implies strong-consistency constraints; a real-time codebase implies latency constraints.
- **Team-implied constraints** — the codebase's tech stack implies team-skill constraints. A TypeScript codebase using TypeORM implies the team knows TypeScript; choosing SQLAlchemy over TypeORM in a Python service would imply cross-language capability.
- **External constraints** — deployment environment (Kubernetes, serverless, on-prem), compliance regime (GDPR, HIPAA, SOC2), and integration requirements (must connect to legacy system X).

Each context records: `context_id` `CTX-XX`, `decision_id`, `constraint`, `constraint_kind` (declared | domain-implied | team-implied | external), `evidence_citation` (required for `declared`; recommended for others). Contexts reconstructed from domain-implied or team-implied constraints MUST be marked `decision_kind: inferred` and `confidence: LOW` unless supported by declared evidence.

### 6.5 Decision Categorization

Categorize each decision into one or more categories:

- **Architecture** — monolith vs microservices, sync vs async, layered vs hexagonal, serverless vs containerized.
- **Technology** — ORM vs raw SQL, framework vs library, build tool, test framework, message broker, cache technology.
- **Data** — SQL vs NoSQL, normalization level, schema design, migration tool, sharding strategy.
- **Concurrency** — thread-per-request vs event-loop, async vs sync, actor model vs shared-state, thread pool size.
- **Error-handling** — exceptions vs Result types, retry policy, circuit breaker, bulkhead, timeout strategy, dead-letter strategy.
- **Caching** — cache-aside vs write-through vs write-back, TTL strategy, eviction policy, cache-technology (Redis vs Memcached vs in-process).
- **AI** — model choice (OpenAI vs Anthropic vs local), agent architecture (ReAct vs Plan-and-Execute), RAG variant, parameter choice (temperature, max_tokens), prompt engineering pattern.
- **Streaming** — backpressure strategy, buffer strategy, error-handling strategy, completion semantics.
- **Pattern** — design pattern choice (cross-reference ART-23's `P-XX`).

Each decision records `categories` (list of one or more of the above). Multi-category decisions are common (e.g., "Use PostgreSQL with Prisma" is both Technology and Data).

### 6.6 ADR & Comment Detection

Detect every Architecture Decision Record and every inline decision comment:

- **ADR files** — files matching `docs/adr/*.md`, `docs/architecture/decisions/*.md`, `decisions/*.md`, `adr/*.md`. ADRs typically follow the Michael Nygard template (Context, Decision, Status, Consequences). Each ADR records: `adr_id` `ADR-XX`, `title`, `status` (proposed | accepted | deprecated | superseded), `file_path`, `decision_id` (the corresponding `DEC-XX`), `citation`.
- **Inline comments** — comments matching `/^\/\/ (We|I|Chose|Decision|Why|Note):/`, `# (We|I|Chose|Decision|Why|Note):`, `/\* (We|I|Chose|Decision|Why|Note): \*/`. Each comment records: `comment_id` `CM-XX`, `file_path`, `line_range`, `decision_id` (the corresponding `DEC-XX`), `citation`.
- **Commit messages** — when the VCS is Git (per ART-01) and `git log` is available, scan commit messages for decision patterns (`use X instead of Y`, `switch from X to Y`, `add X for Y`). Each commit records: `commit_id` `COM-XX`, `sha`, `message`, `decision_id`, `citation`. Commit-message evidence is weaker than ADR evidence and is marked `confidence: MEDIUM` at most.

A decision with at least one ADR, comment, or commit as evidence is marked `decision_kind: documented`. A decision with only structural evidence is marked `decision_kind: inferred`.

### 6.7 AI-Choice Reconstruction (when ART-21 is present)

For each AI-related decision, reconstruct:

- **Model choice** — which model(s) the codebase uses (per `CL-XX.vendor` and `LC-XX.parameters.model`). Alternatives: other vendor models, local models. Trade-off: capability vs cost vs latency vs privacy. Context: domain constraints (e.g., healthcare implies local models for privacy).
- **Agent architecture choice** — which pattern (`AG-XX.architecture`). Alternatives: other patterns. Trade-off: autonomy vs control vs cost. Context: task complexity.
- **RAG variant choice** — which RAG variant (`RAG-XX.variant`). Alternatives: naive RAG, HyDE, FLARE, Self-RAG, Corrective RAG. Trade-off: retrieval quality vs latency vs cost. Context: corpus characteristics.
- **Parameter choice** — temperature, max_tokens, top_p (per `LC-XX.parameters`). Alternatives: other values. Trade-off: determinism vs creativity vs latency. Context: task type (classification → low temperature; brainstorming → high temperature).
- **Tool-calling choice** — whether to use native tool-calling (`tools=[...]`) or ReAct-style parsing. Trade-off: reliability vs portability. Context: model capability.
- **Memory choice** — short-term vs long-term, vector store backend (per `MEM-XX`). Trade-off: persistence vs simplicity vs cost. Context: session length, personalization requirements.

When ART-21 is `ABSENT`, skip § 6.7 entirely and record `OQ-XX: "ART-21 ABSENT; AI-choice analysis skipped"` as an Open Question.

### 6.8 Streaming-Choice Reconstruction (when ART-22 is present)

For each streaming-related decision, reconstruct:

- **Backpressure strategy choice** — credit-based vs lossy vs blocking vs none (per `SW-XX.backpressure_strategy`). Alternatives: the other strategies. Trade-off: throughput vs memory vs correctness. Context: producer/consumer rate differential, value criticality.
- **Buffer strategy choice** — bounded vs unbounded vs windowed (per `SW-XX.buffer_strategy`). Alternatives: the other strategies. Trade-off: memory vs latency vs drop-rate. Context: memory budget, latency budget.
- **Error-handling choice** — retry vs dead-letter vs skip vs propagate (per `SW-XX.error_handling`). Alternatives: the other strategies. Trade-off: reliability vs complexity vs latency. Context: value criticality, downstream tolerance.
- **Completion-semantics choice** — producer-completes vs never-completes vs consumer-cancels (per `SW-XX.completion_semantic`). Alternatives: the other strategies. Trade-off: simplicity vs flexibility. Context: stream nature (finite vs infinite).

When ART-22 is `ABSENT`, skip § 6.8 entirely and record `OQ-XX: "ART-22 ABSENT; streaming-choice analysis skipped"` as an Open Question.

### 6.9 Confidence Assignment

Assign a confidence level to each decision:

- **HIGH** — the decision is documented by an ADR, an inline comment, or a commit message that explicitly states the decision and rationale. Multiple lines of evidence agree.
- **MEDIUM** — the decision is documented but the rationale is partial; OR the decision is inferred from strong structural evidence (competing dependencies present, pattern with named participants matching the pattern's typical purpose).
- **LOW** — the decision is inferred from ecosystem standards alone, with no direct evidence in the codebase. LOW-confidence decisions are still recorded (they inform the rebuild) but are flagged for PROMPT_30's QA review.

Confidence is independent of `decision_kind`: a documented decision can be MEDIUM (partial rationale) and an inferred decision can be MEDIUM (strong structural evidence).

### 6.10 Mermaid Decision-Context Diagram Emission

Emit Mermaid diagrams per `OUTPUT_RULES.md` § 7. Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

- **Per-decision context diagram** — for each significant decision (`confidence: HIGH` or `MEDIUM`), a `flowchart TD` showing the decision, its alternatives, its trade-off (gained/sacrificed), its context constraints, and its evidence citations. Nodes: `DEC-XX` (decision), `ALT-XX` (alternatives), `CTX-XX` (constraints), evidence file nodes.
- **Per-category overview diagram** — one `graph LR` per category showing all decisions in the category, with edges to shared participants.
- **Decision-timeline diagram** — when commit-message evidence is available, a `gitGraph` diagram showing the commits that introduced or changed decisions, in chronological order.

### 6.11 Coverage Cross-Check

Cross-check the decision catalog against ART-02's dependency list and ART-23's pattern instances:

1. Compute `D_02` = set of dependencies in ART-02.
2. Compute `D_24` = set of dependencies referenced by decisions in ART-24 (as evidence or as alternatives).
3. Expected: every "Technology" category decision references at least one dependency in `D_02`. Decisions referencing dependencies not in `D_02` are `CONTRADICTION` findings per R33.
4. Compute `P_23` = set of pattern instances in ART-23.
5. Compute `P_24` = set of pattern instances referenced by "Pattern" category decisions in ART-24.
6. Expected: every "Pattern" category decision references at least one `P-XX` in `P_23`. Pattern decisions referencing unlisted patterns are `CONTRADICTION` findings.
7. Cross-check AI-choice decisions against ART-21's `CL-XX`, `AG-XX`, `RAG-XX`, `LC-XX` when ART-21 is present. Mismatches are `CONTRADICTION` findings.
8. Cross-check streaming-choice decisions against ART-22's `SW-XX` when ART-22 is present. Mismatches are `CONTRADICTION` findings.

---

## 7. Required Outputs

### ART-24 — Engineering Decision Record

**Type:** Doc.

**Acceptance Criteria:**

- AC-24.1: The artifact file exists at `<output_root>/artifacts/phase3/ART24_<engagement_id>_engineering-decisions.md`.
- AC-24.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-24.3: The body contains: Executive Summary, Methodology, Architecture Decisions, Technology Decisions, Data Decisions, Concurrency Decisions, Error-Handling Decisions, Caching Decisions, AI Decisions (or "Skipped: ART-21 absent"), Streaming Decisions (or "Skipped: ART-22 absent"), Pattern Decisions, ADR & Comment Catalog, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-24.4: Every decision records `category`, `decision_kind`, `confidence`, `evidence`, `alternatives`, `trade_off`, `context`, `citation`.
- AC-24.5: Every `documented` decision cites at least one ADR, inline comment, or commit message.
- AC-24.6: Every `inferred` decision cites structural evidence (dependencies, patterns, or code structure).
- AC-24.7: Every Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-24.8: A `.mmd` sidecar file exists for every Mermaid block.
- AC-24.9: Coverage cross-check is recorded with no unresolved contradictions.

---

## 8. Output Templates

### 8.1 ART-24 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-24
artifact_type: Doc
producing_prompt: PROMPT_24
phase: 3
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
decisions:
  - id: DEC-01
    title: <sentence>
    categories: [architecture, technology, data, concurrency, error-handling, caching, ai, streaming, pattern]
    decision_kind: documented | inferred
    confidence: HIGH | MEDIUM | LOW
    evidence:
      - kind: adr | comment | commit | dependency | pattern | config | code-structure
        citation: <file>:<line-range>
        note: <text>
    participants: [M-XX | K-XX | FN-XX | CL-XX | AG-XX | RAG-XX | SW-XX | P-XX]
    alternatives:
      - id: ALT-01
        name: <text>
        rejection_rationale: <text> | INFERRED_FROM_ECOSYSTEM_STANDARD
        rejection_evidence: <file>:<line-range> | INFERRED_FROM_ECOSYSTEM_STANDARD
        citation: <file>:<line-range> | INFERRED_FROM_ECOSYSTEM_STANDARD
    trade_off:
      gained: [<text>]
      sacrificed: [<text>]
      evidence_citations: [<file>:<line-range>]
      inference_kind: documented | inferred-from-usage | inferred-from-taxonomy
    context:
      - id: CTX-01
        constraint: <text>
        constraint_kind: declared | domain-implied | team-implied | external
        evidence_citation: <file>:<line-range> | INFERRED
    citation: <file>:<line-range>
adrs:
  - id: ADR-01
    title: <text>
    status: proposed | accepted | deprecated | superseded
    file_path: <path>
    decision_id: DEC-XX
    citation: <file>:<line-range>
inline_comments:
  - id: CM-01
    file_path: <path>
    line_range: <start-end>
    decision_id: DEC-XX
    citation: <file>:<line-range>
commits:
  - id: COM-01
    sha: <hex>
    message: <text>
    decision_id: DEC-XX
    citation: <file>:<line-range>
coverage_cross_check:
  dependencies_in_art02: [DEP-XX]
  dependencies_in_art24: [DEP-XX]
  unresolved_dependencies: [DEP-XX]
  patterns_in_art23: [P-XX]
  patterns_in_art24: [P-XX]
  unresolved_patterns: [P-XX]
  contradictions: [{ kind: <text>, entity: <id>, detail: <text> }]
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

### 8.2 ART-24 Body Skeleton

```markdown
# ART-24: Engineering Decision Record

## 1. Executive Summary
## 2. Methodology
## 3. Architecture Decisions
   ### 3.1 DEC-01: <title>
   - Kind: documented | inferred (confidence: HIGH/MEDIUM/LOW)
   - Evidence: <citations>
   - Alternatives: <list>
   - Trade-off: gained <list>, sacrificed <list>
   - Context: <list of constraints>
   **Diagram D-01: DEC-01 Context**
## 4. Technology Decisions
## 5. Data Decisions
   (or "## 5. Data Decisions — Degraded: ART-20 absent" with OQ reference)
## 6. Concurrency Decisions
## 7. Error-Handling Decisions
## 8. Caching Decisions
## 9. AI Decisions
   (or "## 9. AI Decisions — Skipped: ART-21 absent" with OQ reference)
## 10. Streaming Decisions
   (or "## 10. Streaming Decisions — Skipped: ART-22 absent" with OQ reference)
## 11. Pattern Decisions
## 12. ADR & Comment Catalog
## 13. Coverage Cross-Check
## 14. Traceability Index
## 15. Open Questions
## 16. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every dependency in ART-02 has at least one Technology decision (or is listed in a `NO_DECISION` register). Threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of decisions cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no decision contradicts ART-02, ART-23, ART-21 (when present), or ART-22 (when present).
- **Q5. UNVERIFIED Accounting** — every `LOW` confidence decision and every `INFERRED_FROM_ECOSYSTEM_STANDARD` alternative has a corresponding Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.1 on a 5% sample of dependencies yields the same decision set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-24.A. Decision-Kind/Evidence Consistency** — every `documented` decision cites at least one ADR, comment, or commit. A `documented` decision without such evidence is a `MAJOR` finding.
- **Q-24.B. Inferred-Evidence Specificity** — every `inferred` decision cites structural evidence (dependencies, patterns, code structure). An inferred decision without structural evidence is a `MAJOR` finding per R22.
- **Q-24.C. Trade-off Realization** — every `gained` property in a trade-off cites the codebase's actual usage that realizes the benefit, OR is marked `inferred-from-taxonomy` with hedged language. Asserting unrealized benefits is a `MAJOR` finding per R22.
- **Q-24.D. Alternative Coverage** — every decision has at least one alternative. A decision with no alternatives is `BLOCKING` (it indicates the decision point was not actually a decision).
- **Q-24.E. AI-Choice Closure** — when ART-21 is present, every `CL-XX`, `AG-XX`, `RAG-XX`, and `LC-XX` is referenced by at least one AI decision. Mismatches are `COVERAGE_GAP` findings.
- **Q-24.F. Streaming-Choice Closure** — when ART-22 is present, every `SW-XX` is referenced by at least one streaming decision. Mismatches are `COVERAGE_GAP` findings.
- **Q-24.G. Pattern-Choice Closure** — every `P-XX` in ART-23 with `rationale_kind: documented` or `inferred` is referenced by at least one Pattern decision. Mismatches are `COVERAGE_GAP` findings.
- **Q-24.H. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment per `OUTPUT_RULES.md` § 7.5.
- **Q-24.I. Sidecar Files** — every Mermaid block has a corresponding `.mmd` file under `<output_root>/diagrams/`.
- **Q-24.J. Confidence Discipline** — `LOW` confidence decisions are < 30% of the catalog; an excess suggests over-inference and triggers a `MINOR` finding for review.

---

## 10. Common Pitfalls

- Do not infer trade-offs the codebase does not realize; asserting "Repository pattern gains performance" without evidence violates R22. Trade-offs MUST cite actual usage.
- Always prefer documented decisions; before recording an inferred decision, search for ADRs, inline comments, and commit messages. Documented decisions are first-class evidence.
- Do not record a decision for every dependency; only significant choices (where viable alternatives existed) count. Recording "use Jest" as a decision when Jest is the only test framework present and no alternatives are evidenced is over-inference.
- Always record at least one alternative per decision; a decision with no alternatives is not a decision per Q-24.D.
- Distinguish `documented` from `inferred` rigorously; an inferred decision mislabeled as documented is a `MAJOR` finding per Q-24.A.
- Always mark `INFERRED_FROM_ECOSYSTEM_STANDARD` alternatives with `confidence: LOW`; ecosystem-standard alternatives are weakly evidenced and must not be presented as authoritative.
- Do not omit AI decisions when ART-21 is present; omitting model choices, agent architectures, or RAG variants that ART-21 records is a `COVERAGE_GAP` per Q-24.E.
- Do not omit streaming decisions when ART-22 is present; omitting backpressure or buffer choices that ART-22 records is a `COVERAGE_GAP` per Q-24.F.
- Always cite the codebase's actual usage for `gained` trade-off properties; citing the pattern's typical benefit without codebase realization violates Q-24.C.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders the diagrams from the sidecar source.

---

## 11. Handoff Criteria

PROMPT_26 and PROMPT_27 consume ART-24. Handoff requires ALL of:

- HC-24.1: ART-24 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-24.2: Every decision records `category`, `decision_kind`, `confidence`, `evidence`, `alternatives`, `trade_off`, `context`, `citation`.
- HC-24.3: Every decision has at least one alternative.
- HC-24.4: Every `documented` decision cites ADR/comment/commit evidence.
- HC-24.5: AI decisions are cataloged (or ART-21 is `ABSENT` with an Open Question).
- HC-24.6: Streaming decisions are cataloged (or ART-22 is `ABSENT` with an Open Question).
- HC-24.7: Mermaid diagrams are emitted with `.mmd` sidecar files; diagrams > 30 nodes are decomposed.
- HC-24.8: Coverage cross-check is recorded with no unresolved contradictions.
- HC-24.9: `repository_fingerprint_recheck` matches ART-01.
- HC-24.10: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_26 (Rebuild Guide — consumes ART-24's decisions to preserve original intent in the rebuild), PROMPT_27 (Developer Handbook — consumes ART-24's decisions to explain "why the code is the way it is" to new engineers), PROMPT_28 (Cross-Reference Checklists — verifies that every `DEC-XX` referenced by ART-26 or ART-27 resolves to an entry in ART-24).
- **Depends on:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-04 (PROMPT_04), ART-17 (PROMPT_17), ART-18 (PROMPT_18), ART-20 (PROMPT_20), ART-21 (PROMPT_21), ART-22 (PROMPT_22), ART-23 (PROMPT_23).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22 (no behavior invention — restricts inferred trade-offs and rationales), R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4 (sidecar files), § 3.1, § 4, § 6, § 7 (Mermaid conventions, edge citations, ≤ 30 nodes, decomposition).
- **Forward reference:** PROMPT_30 (Self-Review & QA) verifies that every `DEC-XX` referenced by ART-26 or ART-27 resolves to an entry in ART-24, and that no `inferred` trade-off violates R22 (no behavior invention).

*End of PROMPT_24. Orchestrator may dispatch PROMPT_25 upon satisfaction of § 11.*
