# PROMPT_30.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_30: Self-Review & Quality Assurance

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_30
- **Phase:** 4
- **Stage:** 5 of 5 (terminal prompt — no PROMPT_31 follows)
- **Dependencies:** ART-01 through ART-29 (all prior artifacts), `engagement_manifest.json`, `quality/quality_log.jsonl`.
- **Estimated Tokens:** 18000–28000
- **Output Artifacts:** ART-30 (Report) — Self-Review & QA Report.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Self-Review & QA Report artifact (ART-30) — the engagement's terminal quality instrument — by executing every check in `QUALITY_STANDARDS.md` § 7 (engagement-wide coverage, engagement-wide traceability via a 200-claim random sample, contradiction sweep, reconstructability test, schema conformance sweep, handoff closure, and the QA Report itself) together with a full re-execution of every Reviewer Hook (`HOOK-01` through `HOOK-05`) registered at `QUALITY_STANDARDS.md` § 8, enumerating every finding by severity (`BLOCKING` / `MAJOR` / `MINOR` / `INFO`) with affected artifacts and resolution guidance, and declaring the engagement `COMPLETE` only when zero `BLOCKING` findings remain — otherwise declaring the engagement `INCOMPLETE` with the offending artifacts and corrective actions recorded, after which no further prompt is dispatched.

---

## 3. When to Invoke

PROMPT_30 is dispatched when ALL of the following predicates hold:

- Phase 4 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.5; specifically, PROMPT_26, PROMPT_27, PROMPT_28, and PROMPT_29 have all emitted their completion records.
- ART-29 status is `REVIEWED` or `DRAFT` with orchestrator waiver, and Handoff Criteria HC-29.1 through HC-29.10 (per `PROMPT_29.md` § 11) are satisfied. In particular, the assembled document `<engagement_id>_ASSEMBLED.md`, the engagement index `<engagement_id>_INDEX.md`, and the export `<engagement_id>_EXPORT.<format>` exist at their declared paths.
- The engagement manifest carries `final_integrity_seal` (a 64-character lowercase hexadecimal SHA-256 hash) and `final_assembled_document_path`. A manifest missing these fields causes PROMPT_30 to emit `BLOCKED` with `MANIFEST_GAP`.
- `quality/quality_log.jsonl` is readable and appendable; PROMPT_30 MUST NOT proceed if the log is locked, missing, or corrupted.
- ART-01 through ART-29 are all present (mandatory artifacts) or recorded as `SKIPPED` / `NOT_PRODUCED` in the engagement manifest's artifact registry (optional artifacts ART-19, ART-20, and trigger-gated ART-21, ART-22 may be absent under documented conditions). Missing mandatory artifacts cause PROMPT_30 to emit `BLOCKED` with `ARTIFACT_GAP`.
- The registered Output Adapter invoked by PROMPT_29 reported `adapter_compliance: traceability-preserved`. A `traceability-violated` adapter record is itself a `BLOCKING` finding escalated immediately.

### 3.1 Terminal Status

PROMPT_30 is the engagement's terminal prompt. No `PROMPT_31` follows under any branch. The orchestrator terminates the engagement on receipt of the ART-30 completion record. If ART-30 declares the engagement `INCOMPLETE`, the orchestrator records the engagement as terminated-with-findings and surfaces the resolution guidance to the engagement owner; the engagement is NOT re-run automatically.

### 3.2 Scope-Modifier Behavior

PROMPT_30 always executes under the engagement's declared scope modifier. Under `SCOPE_TRIAGE`, PROMPT_30 still runs (it is the engagement's QA gate) but operates on the reduced artifact set assembled by PROMPT_29 (Phase 1 artifacts only); optional checklists in ART-28 that depend on absent Phase 2/3 artifacts are recorded as `NA` with rationale. Under `SCOPE_MODULE`, the engagement-wide coverage sweep (§ 6.2) and the reconstructability test (§ 6.5) are scoped to the declared module; out-of-module entities are excluded from both sweeps and the exclusion is recorded.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | Repository boundary declaration; the source of truth for in-scope paths and `repository_fingerprint` re-verification per R15. |
| ART-02 | Manifest | Tech stack & dependency inventory; cross-checked against ART-26 § 4 (Architecture) and ART-27 § 3 (Project Orientation) for tech-stack coherence. |
| ART-03 | Map | Folder & file tree; the canonical file inventory against which the Coverage Sweep (§ 6.2) compares every other artifact's `source_coverage`. |
| ART-04 | Spec | Build & configuration map; cross-checked against ART-26 § 7 (Configuration Reconstruction) and ART-27 § 5 (Environment Setup). |
| ART-05 | Map | Entry-point & bootstrap trace; cross-checked against ART-26 § 5 (Module Rebuild Guide) for bootstrap reconstruction completeness. |
| ART-06 | Map | Module map; the module partition against which the Reconstructability Test (§ 6.5) selects a target module. |
| ART-07 | Map | Component map; cross-checked against ART-25's C4 L3 component diagrams for coverage. |
| ART-08 | Doc | Class & interface reference; entity registry source for `K-XX` (classes) and `I-XX` (interfaces). |
| ART-09 | Doc | Function reference; entity registry source for `FN-XX`. Consumed by HOOK-01 (Dead-Code Detection) re-execution. |
| ART-10 | Graph | Call & dependency graphs; topology source for the Reconstructability Test and the Bidirectional Link Checklist cross-check. |
| ART-11 | Graph | Data-flow diagrams; cross-checked against ART-25's data-flow diagrams (D-DF-XX) and ART-22's streaming workflows for flow-coverage closure. |
| ART-12 | Graph | Control-flow & execution path maps; cross-checked against ART-25's sequence diagrams for path-coverage closure. |
| ART-13 | Doc | State machine catalog; consumed by HOOK-03 (State Reachability) re-execution. |
| ART-14 | Doc | Event workflow catalog; consumed by HOOK-02 (Orphan Event) re-execution. |
| ART-15 | Doc | API & interface reference; consumed by HOOK-04 (API Contract) re-execution. |
| ART-16 | Doc | Middleware & pipeline map; cross-checked against ART-22 (streaming) and ART-25 (sequence diagrams) for middleware-coverage closure. |
| ART-17 | Doc | Error handling & resilience report; cross-checked against ART-26 § 8 (Operational Concerns). |
| ART-18 | Doc | Caching & performance report; cross-checked against ART-26 § 8. |
| ART-19 | Doc | Auth & authorization report (optional); cross-checked against HOOK-04 auth-requirement fields. May be `SKIPPED`; if so, HOOK-04's auth-completeness sub-check records `NA` with rationale. |
| ART-20 | Doc | Database & persistence report (optional); cross-checked against ART-26 § 6 (Data Model Reconstruction). May be `SKIPPED`. |
| ART-21 | Doc | AI/LLM workflow report (optional, trigger-gated); consumed by HOOK-05 (Prompt Exposure) re-execution. May be `NOT_PRODUCED`; if so, HOOK-05 records `NA`. |
| ART-22 | Doc | Streaming workflow report (optional, trigger-gated); cross-checked against ART-11/ART-16 for stream-coverage closure. |
| ART-23 | Doc | Design pattern catalog; cross-checked against ART-26 § 9 (Engineering Decisions) for pattern-decision coherence. |
| ART-24 | Doc | Engineering decision record; cross-checked against ART-26 § 9 for decision-coverage closure. |
| ART-25 | Diagrams | Canonical diagram set; the diagram inventory against which ART-29's diagram-embedding report and ART-27's module walkthroughs are cross-checked. |
| ART-26 | Handbook | Rebuild guide & architecture handbook; the primary subject of the Reconstructability Test (§ 6.5). |
| ART-27 | Handbook | Developer handbook; the secondary subject of the Reconstructability Test. |
| ART-28 | Checklist | Cross-reference & validation checklists; the pre-terminal validation suite whose findings PROMPT_30 reviews, re-executes, and either confirms or escalates. |
| ART-29 | Suite | Final documentation assembly; provides the assembled document, the engagement index, the export, the integrity seal, and the sidecar inventory. |
| `engagement_manifest.json` | Engagement input | Declares the artifact registry, scope modifier, output root, integrity seal, and adapter compliance. PROMPT_30 reads (and only reads) the manifest; it writes the QA decision back to the manifest in § 6.10. |
| `quality/quality_log.jsonl` | Engagement input | The append-only quality audit trail. PROMPT_30 reads prior entries (reviewer sign-offs, ART-28's checklist results, ART-29's assembly event) and appends its own terminal entry per § 6.11. |
| `QUALITY_STANDARDS.md` | Framework file | The governing authority for PROMPT_30. § 7 (Final QA) enumerates the seven sub-checks PROMPT_30 MUST execute; § 8 enumerates the five HOOKs PROMPT_30 MUST re-execute; § 4 (Artifact Schemas) governs the Schema Conformance Sweep; § 5 (Type-Specific Quality Bars) governs the per-artifact aggregate-score audit; § 6 (Reviewer Validation Protocol) governs the 200-claim sampling procedure. |
| `OUTPUT_RULES.md` | Framework file | § 9 (encoding & line endings) re-verified against every assembled file; § 11.4 (integrity seal) re-verified against the assembled document; § 13 (output adapter contract) re-verified against the adapter record. |
| `OPERATING_RULES.md` | Framework file | Bind R13 (read-only), R15 (fingerprint), R17 (citation format), R19 (no secondary citation), R21 (no invention), R22 (no behavior invention), R23 (UNVERIFIED markers), R24 (no hallucinated citations), R33 (contradiction escalation), R35 (integrity termination), R47 (no silent edits). |
| `PROJECT_SPECIFICATION.md` | Framework file | § 5.5 (Phase 4 exit conditions), § 6 (Traceability Contract), § 12 (glossary for terminology drift detection). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL` and halt — no QA checks are run.
2. Verify the engagement manifest carries `final_integrity_seal` and `final_assembled_document_path`; IF either is missing, emit `BLOCKED` with `MANIFEST_GAP`.
3. Compute the SHA-256 hash of the assembled document's current bytes and compare to `final_integrity_seal`; IF mismatch, emit `BLOCKED` with `SEAL_INVALID` per R35 and halt.
4. Read ART-01 through ART-29 (skipping `SKIPPED`/`NOT_PRODUCED` entries per the manifest) and load their front-matter into the QA working set.
5. Read `quality/quality_log.jsonl` and load prior reviewer sign-offs, ART-28's checklist results, and ART-29's assembly event into the QA working set.
6. Execute the Engagement-Wide Coverage Sweep per § 6.2.
7. Execute the 200-Claim Traceability Sample per § 6.3.
8. Execute the Contradiction Sweep per § 6.4.
9. Execute the Reconstructability Test per § 6.5.
10. Execute the Schema Conformance Sweep per § 6.6.
11. Execute the Handoff Closure Review per § 6.7.
12. Re-execute every Reviewer Hook (`HOOK-01` through `HOOK-05`) per § 6.8; record each hook's PASS/FAIL/NA result and any new findings.
13. Aggregate all findings per § 6.9; assign severity (`BLOCKING` / `MAJOR` / `MINOR` / `INFO`); deduplicate against ART-28's existing findings to avoid double-counting.
14. Generate the QA Report (ART-30) per § 6.10 and § 8.
15. Append the terminal quality-log entry per § 6.11.
16. Update the engagement manifest with `final_qa_decision` (`COMPLETE` or `INCOMPLETE`) and `final_qa_report_path` per § 6.12.
17. Emit ART-30's Completion Record per `MASTER_PROMPT.md` § 6; the Completion Record's `status` field is `SUCCESS` only when zero `BLOCKING` findings remain, else `SUCCESS_WITH_FINDINGS` (engagement `INCOMPLETE`).
18. Halt. No subsequent prompt is dispatched. The orchestrator MUST surface ART-30 to the engagement owner.

---

## 6. Analysis Procedures

### 6.1 Pre-QA Integrity Verification

Before any QA check runs, PROMPT_30 establishes the integrity baseline. The procedure is mandatory because every downstream check relies on the assembled document being byte-identical to the sealed document. The agent re-computes `repository_fingerprint` (per R15) by re-hashing the in-scope path set declared in ART-01; a mismatch indicates the subject repository has been modified mid-engagement and is treated as `INTEGRITY_FAIL`. The agent then re-hashes `<engagement_id>_ASSEMBLED.md` and compares the result to `final_integrity_seal` recorded by PROMPT_29; a mismatch indicates post-seal modification and triggers `SEAL_INVALID` per R35. Finally the agent verifies that `<engagement_id>_INDEX.md` and `<engagement_id>_EXPORT.<format>` both exist and are non-empty; a missing index or export is `BLOCKING`.

The integrity baseline also extends to encoding and line-ending normalization. The agent scans every file in `<output_root>/final/` for forbidden artifacts per `OUTPUT_RULES.md` § 9: UTF-8 BOM bytes (`0xEF 0xBB 0xBF`), CRLF line endings (`\r\n`), standalone CR (`\r`), and trailing whitespace on any line. Any violation is a `BLOCKING` finding against ART-29 (the producing prompt) with `affected_artifacts: [ART-29]` and `resolution_guidance: "Re-dispatch PROMPT_29 with explicit re-normalization; do not modify the assembled document in place per R47."`.

### 6.2 Engagement-Wide Coverage Sweep (QUALITY_STANDARDS.md § 7.1)

The Coverage Sweep verifies that every in-scope file declared in ART-01 appears in at least one artifact's `source_coverage` block. The procedure produces the engagement's coverage map: a join of ART-03's file tree (the canonical inventory) against every artifact's `source_coverage` entries. The agent enumerates the in-scope file set from ART-01, then for each file queries the join; files with zero hits are `COVERAGE_GAP` findings (`MAJOR` by default, `BLOCKING` if the file is a mandatory source file such as an entry point, an API contract, or a state-transition definition).

The sweep also audits per-artifact coverage fractions. Each artifact's front-matter declares `coverage_fraction` (0–1); the agent re-derives the fraction by counting source-covered symbols against the total symbol count in ART-08 (for `K-XX`/`I-XX`) and ART-09 (for `FN-XX`). A discrepancy greater than 0.05 between the declared fraction and the re-derived fraction is a `MAJOR` finding (`SCORE_INFLATION`) against the producing artifact; per `QUALITY_STANDARDS.md` § 9.3 (Anti-Gaming), systematic inflation across multiple artifacts is escalated to `BLOCKING`. The sweep records every gap with `affected_artifacts`, the file path, and a forward-reference to the producing prompt's re-dispatch guidance.

### 6.3 Engagement-Wide Traceability Sample (QUALITY_STANDARDS.md § 7.2)

The Traceability Sample verifies that cited claims across the engagement are traceable to real source locations. The agent collects every `traceability_index` entry from every artifact (each entry has `claim_id`, `source` as `<file_path>:<line-range>`, and `symbol`), deduplicates by `claim_id`, and draws a random sample of 200 claims using a deterministic seed (`sha256(engagement_id)` mapped to an integer seed) so that the sample is reproducible by a reviewer. For each sampled claim, the agent opens the cited file, reads the cited line range, and verifies (a) the file exists, (b) the line range falls within the file's bounds, (c) the cited symbol appears in or near the line range (within ±5 lines tolerance for symbol drift), and (d) the claim's content is substantiated by the cited source.

A claim fails verification if any of (a)–(d) is false. The agent records each failure as a finding: a missing file is `BLOCKING` (`CITATION_FABRICATION` per R24); an out-of-bounds line range is `MAJOR` (`CITATION_DRIFT`); a missing symbol within tolerance is `MAJOR` (`SYMBOL_DRIFT`); a substantiation failure (the source does not support the claim) is `BLOCKING` (`UNSUBSTANTIATED_CLAIM`). Per `QUALITY_STANDARDS.md` § 7.2, if the verification rate across the 200-claim sample is below 95%, the engagement is `INCOMPLETE` with a single `BLOCKING` finding (`TRACEABILITY_BELOW_THRESHOLD`) recording the verification rate, the count of failed claims, and a sample of 10 representative failures for reviewer triage. The 200-claim sample is the minimum; for engagements with fewer than 200 total claims, the sample is the full claim set (100% verification per `QUALITY_STANDARDS.md` § 6.1).

### 6.4 Contradiction Sweep (QUALITY_STANDARDS.md § 7.3)

The Contradiction Sweep verifies that no two artifacts make contradictory claims about the same entity. The agent builds an entity-claim matrix from the Entity Registry (ART-28 § 6.1) joined with every artifact's claims touching each entity. For every entity `XX-NN` shared by two or more artifacts, the agent retrieves the claims each artifact makes about that entity (e.g., its kind, its signature, its callers, its dependencies, its state transitions, its event emissions) and checks for pairwise consistency. A contradiction exists when two artifacts assert incompatible properties (e.g., ART-08 declares `K-03` a class with constructor `(String, int)` while ART-15 records `K-03`'s constructor as `(String, String)`; or ART-13 declares state `S-02 → S-05` while ART-25's state diagram for the same machine omits that transition).

Each detected contradiction is `BLOCKING` (`CONTRADICTION`) with `affected_artifacts: [ART-A, ART-B]`, the entity ID, both claim references, and `resolution_guidance: "Re-dispatch both producing prompts; the contradiction MUST be resolved by the artifact with the lower-quality score per QUALITY_STANDARDS.md § 9.3, or escalated to a human reviewer if scores tie."`. The sweep also detects soft contradictions — cases where two claims are not strictly incompatible but use different terminology for the same concept (e.g., "rate limiter" vs "throttler"). Soft contradictions are `MINOR` (`TERMINOLOGY_DRIFT`) and are recorded for the engagement owner's attention but do not block completion. The sweep is O(N²) in the number of shared entities; for engagements with more than 500 shared entities, the agent samples 500 entity pairs using the deterministic seed and records the sampling fraction.

### 6.5 Reconstructability Test (QUALITY_STANDARDS.md § 7.4)

The Reconstructability Test verifies that a competent engineer could rebuild one module of the subject system using only ART-26 (Rebuild Guide), ART-27 (Developer Handbook), and the supporting artifacts (ART-25 diagrams, ART-09 function reference, ART-08 class reference, ART-15 API reference). The agent selects a target module from ART-06 by deterministically choosing the module with the median cyclomatic complexity (computed from ART-10's call graph: median over modules of the sum of edges' complexities). The median-complexity rule ensures the test is neither trivial (lowest-complexity module) nor adversarial (highest-complexity module).

For the selected module, the agent executes the reconstruction procedure: (1) read ART-26's module-by-module rebuild guide for the target module; (2) extract the declared public API, key algorithms, dependencies, configuration, and rebuild steps; (3) attempt to write a stub implementation skeleton that satisfies the declared public API and dependencies, using only the information in ART-26 and the supporting artifacts; (4) for each declared algorithm, verify that the rebuild guide's algorithm description is sufficient to produce the algorithm — meaning the description names inputs, outputs, side effects, and the procedural steps in order. A reconstruction failure is recorded when (a) the public API cannot be derived from ART-26 alone (missing method signatures, missing return types), (b) a declared dependency's interface is not documented in ART-15 or ART-08, (c) a declared algorithm's steps are insufficient to reproduce the algorithm (missing input validation, missing error path, missing concurrency guarantee), or (d) the rebuild steps are out of order or reference artifacts that do not exist.

Each reconstruction failure is `BLOCKING` (`RECONSTRUCTION_FAILURE`) with `affected_artifacts: [ART-26, ART-27]` (and supporting artifacts as applicable), the failed step, the missing information, and `resolution_guidance: "Re-dispatch PROMPT_26 with explicit instruction to expand the module's rebuild guide; the missing information is <details>."`. The reconstruction skeleton itself is recorded as an attachment to ART-30 (saved to `<output_root>/reviews/<engagement_id>_reconstruction_<module_id>.md`) so a human reviewer can audit the test. Under `SCOPE_MODULE`, the reconstruction target is the declared in-scope module; under `SCOPE_TRIAGE`, the test is `NA` with rationale (Phase 4 handbooks are not produced under triage).

### 6.6 Schema Conformance Sweep (QUALITY_STANDARDS.md § 7.5)

The Schema Conformance Sweep re-validates every artifact against its registered schema per `QUALITY_STANDARDS.md` § 4. The agent enumerates every artifact in the engagement manifest's registry, reads its front-matter, and validates each field against the schema for its declared `artifact_type` (Manifest, Map, Spec, Doc, Graph, Diagrams, Handbook, Checklist, Suite, Report). Validation includes: required fields present, types correct (integers vs strings vs arrays), `quality_score` sub-fields within 0–5 with `aggregate` equal to the sum, `status` in the allowed enum, `open_questions` entries carrying `id`, `question`, `blocking` (boolean), and `traceability_index` entries carrying `claim_id`, `source`, `symbol`.

A schema violation is `BLOCKING` (`SCHEMA_VIOLATION`) with `affected_artifacts: [ART-XX]`, the violated field, the expected vs actual value, and `resolution_guidance: "Re-dispatch PROMPT_XX to correct the front-matter; do not hand-edit per R47."`. The sweep also re-validates type-specific quality bars per `QUALITY_STANDARDS.md` § 5: each artifact's `aggregate` quality score MUST meet the type-specific minimum (e.g., Handbook ≥ 32 with Reconstructive completeness ≥ 4 and Readability ≥ 4). Sub-bar aggregates are `MAJOR` (`QUALITY_BELOW_BAR`) and are recorded with the dimension scores and the deficit. The sweep is deterministic and idempotent: re-running on the same artifact set yields identical findings.

### 6.7 Handoff Closure Review (QUALITY_STANDARDS.md § 7.6)

The Handoff Closure Review verifies that no open question blocks engagement completion. The agent collects every `open_questions` entry from every artifact and from ART-28's Findings Aggregate. For each entry, the agent checks the `blocking` flag: if `blocking: true` and the question is unresolved (no resolution recorded in `quality/quality_log.jsonl` since the question was raised), the question is a `BLOCKING` finding (`OPEN_BLOCKING_QUESTION`) with `affected_artifacts: [ART-XX]`, the question text, the producing prompt, and `resolution_guidance: "Resolve the question via re-dispatch of the producing prompt or via a documented human decision recorded in the quality log."`.

The review also confirms that every prior prompt's Handoff Criteria (HC-XX.Y) are satisfied by re-reading each prompt's § 11 and verifying each criterion against the current artifact state. A previously-satisfied criterion that is no longer satisfied (e.g., a fingerprint mismatch introduced after HC-29.9 was satisfied) is `BLOCKING` (`HANDOFF_REGRESSION`). Non-blocking open questions are recorded as `INFO` (`OPEN_NONBLOCKING_QUESTION`) for the engagement owner's awareness; they do not block completion but are surfaced in the QA Report's recommendations section.

### 6.8 Reviewer Hook Re-Execution (QUALITY_STANDARDS.md § 8)

PROMPT_30 re-executes every Reviewer Hook registered at `QUALITY_STANDARDS.md` § 8. The re-execution is independent of ART-28's HOOK executions; PROMPT_30 does NOT trust ART-28's results and re-runs each hook from primary sources. The procedure for each hook:

**HOOK-01 (Dead-Code Detection).** For every `FN-XX` in ART-09, compute the transitive closure of callers from the entry points declared in ART-05. Any function with zero callers after closure is flagged `DEAD_CODE_CANDIDATE`. Compare the result to ART-09's `dead_code_candidate` flags; mismatches are `BLOCKING` (`HOOK_01_DIVERGENCE`). Functions correctly flagged but lacking rationale in ART-09 are `MAJOR` (`HOOK_01_RATIONALE_MISSING`).

**HOOK-02 (Orphan Event).** For every event `E-XX` in ART-14, verify at least one emitter and at least one handler are recorded. Events with emitter-only are `ORPHAN_HANDLER_ONLY`; events with handler-only are `ORPHAN_EMITTER_ONLY`; events with neither are `ORPHAN_EVENT`. All three are `BLOCKING` (`HOOK_02_ORPHAN`). HOOK-02 is `NA` if ART-14 is `SKIPPED` (no events detected); the `NA` is recorded with rationale.

**HOOK-03 (State Reachability).** For every state machine `S-XX` in ART-13, perform a reachability analysis from the declared initial state: every state MUST be reachable, and every state MUST reach a terminal state (or be explicitly declared `no-terminal` for stateless cycles). Unreachable states are `UNREACHABLE_STATE`; states that cannot reach a terminal are `NO_TERMINAL_STATE`. Both are `BLOCKING` (`HOOK_03_REACHABILITY`). HOOK-03 is `NA` if ART-13 is `SKIPPED`.

**HOOK-04 (API Contract).** For every API `P-XX` in ART-15, verify the contract documents: input parameters (with types), request body schema (for non-GET), response schema (with status codes), error responses (with codes and conditions), and authentication requirement. Missing any of these is `BLOCKING` (`HOOK_04_CONTRACT_INCOMPLETE`). The auth-requirement sub-check cross-references ART-19: if ART-19 declares an endpoint authenticated but ART-15 marks it unauthenticated (or vice versa), the contradiction is `BLOCKING` (`HOOK_04_AUTH_MISMATCH`). HOOK-04 is `NA` only if ART-15 is `SKIPPED`, which is not expected for any engagement.

**HOOK-05 (Prompt Exposure).** For every prompt template declared in ART-21, verify the template is documented verbatim or marked `REDACTED` with rationale. Templates with neither verbatim text nor `REDACTED` rationale are `BLOCKING` (`HOOK_05_PROMPT_NOT_EXPOSED`). Templates marked `REDACTED` must carry a rationale field; missing rationale is `BLOCKING` (`HOOK_05_REDACTION_RATIONALE_MISSING`). HOOK-05 is `NA` if ART-21 is `NOT_PRODUCED` (no AI/LLM detected); the `NA` is recorded with rationale.

Each hook's re-execution result is recorded in ART-30's hook-execution table with `hook_id`, `status` (PASS/FAIL/NA), `finding_count`, `evidence` (sample of 5 results), and `divergence_from_art28` (delta between PROMPT_30's re-execution and ART-28's prior execution). Divergence indicates either ART-28 was wrong (escalated to `BLOCKING` per Q-30.H) or the artifact was modified after ART-28 ran (escalated to `BLOCKING` per R47).

### 6.9 Finding Aggregation & Severity Assignment

The agent aggregates findings from § 6.2 through § 6.8 into a single findings table. Each finding carries: `finding_id` (F-NN, zero-padded), `severity` (`BLOCKING` / `MAJOR` / `MINOR` / `INFO`), `category` (e.g., `COVERAGE_GAP`, `CITATION_FABRICATION`, `CONTRADICTION`, `RECONSTRUCTION_FAILURE`, `SCHEMA_VIOLATION`, `HOOK_02_ORPHAN`), `affected_artifacts` (list of `ART-XX`), `description` (one-paragraph prose), `evidence` (file path + line range + symbol), and `resolution_guidance` (specifying the producing prompt to re-dispatch and the corrective action).

The agent deduplicates findings against ART-28's existing findings: a finding that exactly matches an ART-28 finding (same category, same affected artifacts, same evidence) is recorded as `INFO` (`DUPLICATE_OF_ART28`) with a reference to ART-28's finding ID, not re-counted at the original severity. A finding that partially matches (same category, different evidence) is recorded at its own severity with a `related_art28_finding` reference. New findings not present in ART-28 are recorded at their full severity. The deduplication prevents double-counting in the engagement's quality ledger but does NOT suppress severity escalation — a `BLOCKING` finding is `BLOCKING` regardless of whether ART-28 already recorded it.

### 6.10 QA Report Generation

The agent emits ART-30 per the template in § 8. The QA Report's executive summary states the engagement's terminal status (`COMPLETE` if zero `BLOCKING` findings, else `INCOMPLETE`), the finding counts by severity, the coverage rate, the 200-claim traceability rate, the contradiction count, the reconstruction result (PASS/FAIL with module ID), the schema-conformance rate, and the open-blocking-question count. The body of the report contains the findings table, the hook-execution table, the coverage map summary, the traceability sample summary, the contradiction matrix summary, the reconstruction-test attachment reference, the schema-conformance sweep summary, the handoff-closure summary, and the recommendations section.

The QA Report's terminal decision is computed deterministically: `engagement_status = (block_count == 0) ? "COMPLETE" : "INCOMPLETE"`. There is no override; the orchestrator MAY NOT waive `BLOCKING` findings to force completion. The only resolution path is to re-dispatch the producing prompts identified in each finding's `resolution_guidance`, address the findings, re-run the affected prompts (which re-emit their artifacts with new versions per R47), re-run PROMPT_28 (which re-executes the checklists), re-run PROMPT_29 (which re-assembles and re-seals), and re-run PROMPT_30 (which re-executes the QA pass). The re-run loop continues until either zero `BLOCKING` findings remain or the engagement owner declares the engagement terminated-with-findings.

### 6.11 Quality Log Entry

The agent appends a terminal entry to `quality/quality_log.jsonl`:

```json
{
  "timestamp": "<ISO-8601>",
  "event": "terminal_qa",
  "prompt_id": "PROMPT_30",
  "engagement_id": "<uuid>",
  "qa_report_path": "<output_root>/artifacts/phase4/ART30_<engagement_id>_qa-report.md",
  "engagement_status": "COMPLETE | INCOMPLETE",
  "finding_counts": { "BLOCKING": <int>, "MAJOR": <int>, "MINOR": <int>, "INFO": <int> },
  "coverage_rate": <0..1>,
  "traceability_rate": <0..1>,
  "contradiction_count": <int>,
  "reconstruction_result": "PASS | FAIL | NA",
  "reconstruction_module": "<module_id | NA>",
  "schema_conformance_rate": <0..1>,
  "open_blocking_questions": <int>,
  "hook_results": { "HOOK-01": "PASS|FAIL|NA", "HOOK-02": "PASS|FAIL|NA", "HOOK-03": "PASS|FAIL|NA", "HOOK-04": "PASS|FAIL|NA", "HOOK-05": "PASS|FAIL|NA" },
  "integrity_seal_verified": true | false,
  "fingerprint_recheck_verified": true | false,
  "terminal": true
}
```

The log entry is the engagement's last audit record. The `"terminal": true` field marks the log as sealed per `QUALITY_STANDARDS.md` § 10; no further entries MAY be appended to this engagement's quality log.

### 6.12 Engagement Manifest Update

The agent updates `engagement_manifest.json` with the terminal QA decision:

```json
{
  "final_qa_decision": "COMPLETE | INCOMPLETE",
  "final_qa_report_path": "<output_root>/artifacts/phase4/ART30_<engagement_id>_qa-report.md",
  "final_qa_at": "<ISO-8601>",
  "final_blocking_findings": <int>,
  "final_major_findings": <int>,
  "final_minor_findings": <int>,
  "final_info_findings": <int>,
  "terminal": true
}
```

This is the only write PROMPT_30 performs outside its own artifact. The orchestrator reads the manifest's `final_qa_decision` to determine whether to deliver the assembled suite to the engagement owner (`COMPLETE`) or surface the findings for remediation (`INCOMPLETE`).

---

## 7. Required Outputs

### ART-30 — Self-Review & QA Report

**Type:** Report.

**Acceptance Criteria:**

- AC-30.1: The artifact file exists at `<output_root>/artifacts/phase4/ART30_<engagement_id>_qa-report.md`.
- AC-30.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and the Report schema (§ 4.8) including the `findings` array.
- AC-30.3: The body contains all sections enumerated in § 8.2 in order: Executive Summary, Methodology, Pre-QA Integrity Verification, Coverage Sweep, Traceability Sample, Contradiction Sweep, Reconstructability Test, Schema Conformance Sweep, Handoff Closure Review, Hook Re-Execution, Findings Aggregate, Recommendations, Engagement Decision, Traceability Index, Open Questions, Cross-References.
- AC-30.4: The Engagement Decision section states `COMPLETE` or `INCOMPLETE` deterministically per § 6.10.
- AC-30.5: Every finding carries `finding_id`, `severity`, `category`, `affected_artifacts`, `description`, `evidence`, `resolution_guidance`.
- AC-30.6: The Hook Re-Execution section records every HOOK (01 through 05) with `status` and `evidence`.
- AC-30.7: The reconstruction-test attachment exists at `<output_root>/reviews/<engagement_id>_reconstruction_<module_id>.md` (or `NA` recorded under `SCOPE_TRIAGE`).
- AC-30.8: The quality log entry per § 6.11 is appended with `"terminal": true`.
- AC-30.9: The engagement manifest update per § 6.12 is performed atomically.
- AC-30.10: The Completion Record's `status` field is `SUCCESS` only when zero `BLOCKING` findings remain; otherwise `SUCCESS_WITH_FINDINGS`.

---

## 8. Output Templates

### 8.1 ART-30 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-30
artifact_type: Report
producing_prompt: PROMPT_30
phase: 4
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: 1.0
quality_score:
  coverage: 5
  traceability: 5
  accuracy: 5
  depth: 5
  coherence: 5
  precision: 5
  completeness: 5
  readability: 5
  aggregate: 40
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
integrity_seal_verified: true | false
fingerprint_recheck_verified: true | false
qa_decision: COMPLETE | INCOMPLETE
finding_counts:
  BLOCKING: <int>
  MAJOR: <int>
  MINOR: <int>
  INFO: <int>
coverage_rate: <0..1>
traceability_sample:
  sample_size: <int>
  verified: <int>
  failed: <int>
  verification_rate: <0..1>
  seed: <int>
contradiction_count: <int>
reconstruction_test:
  target_module: <module_id | NA>
  result: PASS | FAIL | NA
  attachment_path: <relative-path | NA>
schema_conformance_rate: <0..1>
open_blocking_questions: <int>
hook_results:
  - hook_id: HOOK-01
    status: PASS | FAIL | NA
    finding_count: <int>
    divergence_from_art28: <int>
  - hook_id: HOOK-02
    status: PASS | FAIL | NA
    finding_count: <int>
    divergence_from_art28: <int>
  - hook_id: HOOK-03
    status: PASS | FAIL | NA
    finding_count: <int>
    divergence_from_art28: <int>
  - hook_id: HOOK-04
    status: PASS | FAIL | NA
    finding_count: <int>
    divergence_from_art28: <int>
  - hook_id: HOOK-05
    status: PASS | FAIL | NA
    finding_count: <int>
    divergence_from_art28: <int>
findings:
  - id: F-01
    severity: BLOCKING | MAJOR | MINOR | INFO
    category: COVERAGE_GAP | CITATION_FABRICATION | CITATION_DRIFT | SYMBOL_DRIFT | UNSUBSTANTIATED_CLAIM | TRACEABILITY_BELOW_THRESHOLD | CONTRADICTION | TERMINOLOGY_DRIFT | RECONSTRUCTION_FAILURE | SCHEMA_VIOLATION | QUALITY_BELOW_BAR | OPEN_BLOCKING_QUESTION | OPEN_NONBLOCKING_QUESTION | HANDOFF_REGRESSION | HOOK_01_DIVERGENCE | HOOK_01_RATIONALE_MISSING | HOOK_02_ORPHAN | HOOK_03_REACHABILITY | HOOK_04_CONTRACT_INCOMPLETE | HOOK_04_AUTH_MISMATCH | HOOK_05_PROMPT_NOT_EXPOSED | HOOK_05_REDACTION_RATIONALE_MISSING | SCORE_INFLATION | SEAL_INVALID | MANIFEST_GAP | ARTIFACT_GAP | INTEGRITY_FAIL | DUPLICATE_OF_ART28
    affected_artifacts: [ART-XX]
    description: <text>
    evidence: <file_path>:<line-range> | <symbol>
    resolution_guidance: <text>
    related_art28_finding: <F-XX | none>
source_coverage:
  - path: <engagement_manifest.json>
    symbol_count: 0
    line_range: 1-1
  - path: <quality_log.jsonl>
    symbol_count: 0
    line_range: 1-1
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <engagement_manifest.json>:<line-range>
    symbol: <name>
---
```

### 8.2 ART-30 Body Skeleton

```markdown
# ART-30: Self-Review & QA Report

## 1. Executive Summary
   - Engagement ID: <uuid>
   - Engagement status: COMPLETE | INCOMPLETE
   - Finding counts: BLOCKING <int>, MAJOR <int>, MINOR <int>, INFO <int>
   - Coverage rate: <0..1>
   - Traceability sample: <verified>/<sample_size> (<rate>)
   - Contradiction count: <int>
   - Reconstruction test: PASS | FAIL (module <module_id>)
   - Schema conformance rate: <0..1>
   - Open blocking questions: <int>
   - HOOK results: HOOK-01 <status>, HOOK-02 <status>, HOOK-03 <status>, HOOK-04 <status>, HOOK-05 <status>
   - Integrity seal verified: true | false
   - Fingerprint recheck verified: true | false
## 2. Methodology
   - QA sub-checks executed per QUALITY_STANDARDS.md § 7.1 through § 7.7
   - Reviewer Hooks re-executed per QUALITY_STANDARDS.md § 8
   - 200-claim traceability sample drawn with seed <int>
   - Reconstruction target module: <module_id> (selected by median cyclomatic complexity)
## 3. Pre-QA Integrity Verification
   - Fingerprint recheck: <result>
   - Integrity seal re-hash: <result>
   - Encoding/line-ending normalization re-scan: <result>
## 4. Coverage Sweep (QUALITY_STANDARDS.md § 7.1)
   - In-scope file count: <int>
   - Covered: <int>, Uncovered: <int>
   - Coverage rate: <0..1>
   - COVERAGE_GAP findings: <count>
   - Per-artifact coverage-fraction audit: <summary>
## 5. Traceability Sample (QUALITY_STANDARDS.md § 7.2)
   - Total claims across engagement: <int>
   - Sample size: 200 (or full set if < 200)
   - Verified: <int>, Failed: <int>
   - Verification rate: <0..1>
   - Representative failures: <list of 10>
## 6. Contradiction Sweep (QUALITY_STANDARDS.md § 7.3)
   - Shared entity count: <int>
   - Pairs checked: <int> (or sampling fraction)
   - Contradictions detected: <int>
   - Soft contradictions (terminology drift): <int>
## 7. Reconstructability Test (QUALITY_STANDARDS.md § 7.4)
   - Target module: <module_id>
   - Reconstruction skeleton: <attachment path>
   - Result: PASS | FAIL
   - Failed steps (if any): <list>
## 8. Schema Conformance Sweep (QUALITY_STANDARDS.md § 7.5)
   - Artifacts validated: <int>
   - Schema violations: <int>
   - Quality-bar deficits: <int>
## 9. Handoff Closure Review (QUALITY_STANDARDS.md § 7.6)
   - Open questions reviewed: <int>
   - Blocking unresolved: <int>
   - Non-blocking unresolved: <int>
   - Handoff-regression findings: <int>
## 10. Hook Re-Execution (QUALITY_STANDARDS.md § 8)
   - HOOK-01 (Dead-Code): <status>, findings <int>, divergence <int>
   - HOOK-02 (Orphan Event): <status>, findings <int>, divergence <int>
   - HOOK-03 (State Reachability): <status>, findings <int>, divergence <int>
   - HOOK-04 (API Contract): <status>, findings <int>, divergence <int>
   - HOOK-05 (Prompt Exposure): <status>, findings <int>, divergence <int>
## 11. Findings Aggregate
   ### 11.1 BLOCKING Findings
   ### 11.2 MAJOR Findings
   ### 11.3 MINOR Findings
   ### 11.4 INFO Findings
## 12. Recommendations
   - Re-dispatch queue: <list of producing prompts to re-run>
   - Engagement owner advisories: <list>
   - Framework improvement opportunities: <list>
## 13. Engagement Decision
   - Status: COMPLETE | INCOMPLETE
   - Rationale: <text>
   - Re-run loop guidance (if INCOMPLETE): <text>
## 14. Traceability Index
## 15. Open Questions
## 16. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every check in `QUALITY_STANDARDS.md` § 7 is executed and recorded. Threshold ≥ 1.0.
- **Q2. Citation Check** — every claim in ART-30 cites either a primary source (artifact, file, log entry) or an ART-28 finding it supersedes. Threshold ≥ 0.95.
- **Q3. Schema Conformance Check** — ART-30 validates against the Report schema (`QUALITY_STANDARDS.md` § 4.8).
- **Q4. Non-Contradiction Check** — ART-30's findings do not contradict the artifacts they reference.
- **Q5. UNVERIFIED Accounting** — every `NA` hook result has a corresponding Open Question explaining why the hook is not applicable.
- **Q6. Idempotence Spot-Check** — re-running § 6.2 through § 6.8 with the same seed yields the same finding set. Non-idempotent sweeps are `BLOCKING` (`NON_IDEMPOTENT_QA`).
- **Q7. Handoff Readiness** — ART-30's terminal decision matches the finding counts (zero `BLOCKING` ⇔ `COMPLETE`).

### Prompt-Specific Checks

- **Q-30.A. Seven Sub-Checks Executed** — every sub-check in `QUALITY_STANDARDS.md` § 7.1 through § 7.7 is executed and recorded in ART-30's body. A missing sub-check is `BLOCKING`.
- **Q-30.B. Five HOOKs Re-Executed** — every HOOK (01 through 05) is re-executed independently from primary sources (not by trusting ART-28). A skipped or trusted-from-ART-28 hook is `BLOCKING`.
- **Q-30.C. 200-Claim Sample Determinism** — the traceability sample is drawn with a deterministic seed recorded in ART-30's front-matter. A non-deterministic or un-seeded sample is `MAJOR`.
- **Q-30.D. Reconstructability Test Performed** — the reconstruction skeleton attachment exists at its declared path (or `NA` recorded under `SCOPE_TRIAGE`). A missing attachment is `BLOCKING`.
- **Q-30.E. Finding Severity Discipline** — every finding's severity matches its category's declared severity (e.g., `CITATION_FABRICATION` is `BLOCKING`, `TERMINOLOGY_DRIFT` is `MINOR`). Severity misassignments are `MAJOR` (`SEVERITY_MISASSIGNMENT`).
- **Q-30.F. Resolution Guidance Specificity** — every `BLOCKING` and `MAJOR` finding's `resolution_guidance` specifies the producing prompt to re-dispatch (e.g., "Re-dispatch PROMPT_09"). Generic guidance is `MAJOR`.
- **Q-30.G. Integrity Seal Verification** — `integrity_seal_verified` is `true` and the re-computed hash matches `final_integrity_seal`. A `false` value or hash mismatch is `BLOCKING` (`SEAL_INVALID`).
- **Q-30.H. HOOK Divergence Handling** — every non-zero `divergence_from_art28` is escalated as a `BLOCKING` finding (either ART-28 was wrong or the artifact was modified post-ART-28). Un-escalated divergence is `BLOCKING`.
- **Q-30.I. Quality Log Sealed** — the terminal quality-log entry is appended with `"terminal": true`. A missing or non-terminal entry is `MAJOR`.
- **Q-30.J. Engagement Manifest Atomic Update** — the manifest update per § 6.12 is atomic and complete; a partial update is `BLOCKING`.
- **Q-30.K. Terminal Decision Determinism** — `qa_decision` is computed as `(block_count == 0) ? "COMPLETE" : "INCOMPLETE"` with no orchestrator override. A non-deterministic decision is `BLOCKING` (`DECISION_NONDETERMINISTIC`).

---

## 10. Common Pitfalls

- Do not trust ART-28's HOOK results; PROMPT_30 MUST re-execute every HOOK from primary sources per Q-30.B. Trusting prior results defeats the engagement's independent verification gate.
- Always draw the 200-claim traceability sample with a deterministic seed; a non-deterministic sample makes the engagement's traceability rate non-reproducible by a reviewer.
- Always compute the reconstruction-test target by the median-complexity rule; choosing the lowest-complexity module makes the test trivial and choosing the highest makes it adversarial. Either choice biases the engagement's terminal decision.
- Do not waive `BLOCKING` findings to force completion; the orchestrator MAY NOT override Q-30.K. The only resolution path is re-dispatch of the producing prompts identified in each finding's `resolution_guidance`.
- Always deduplicate findings against ART-28's existing findings; double-counting inflates the engagement's quality ledger and misleads the engagement owner. Deduplication MUST NOT suppress severity escalation.
- Always re-verify the integrity seal before any QA check runs; running QA on a post-seal-modified document produces findings against the wrong baseline.
- Always record `NA` with rationale for hooks whose target artifact is absent (e.g., HOOK-05 when ART-21 is `NOT_PRODUCED`); a bare `NA` without rationale fails Q5 and Q-30.B.
- Do not edit the assembled document in place to address findings; in-place edits violate R47 and invalidate the integrity seal per R35. Re-dispatch the producing prompt instead.
- Always specify the producing prompt in every finding's `resolution_guidance`; generic guidance ("fix the issue") fails Q-30.F and forces the engagement owner to reverse-engineer which prompt to re-run.
- Always append the terminal quality-log entry with `"terminal": true`; a non-terminal entry allows further appends that would compromise the engagement's audit-trail immutability.
- Do not record a finding's severity by intuition; use the category-to-severity table declared in § 6.9 and § 8.1. Severity misassignment fails Q-30.E and undermines the engagement's quality ledger.
- Always surface `INFO` findings (non-blocking open questions, terminology drift, framework improvement opportunities) in the Recommendations section even though they do not block completion; the engagement owner uses these to plan post-engagement work.

---

## 11. Handoff Criteria

PROMPT_30 is the engagement's terminal prompt; its handoff is the engagement's closure declaration. The orchestrator treats the following criteria as the engagement's exit gate:

- HC-30.1: ART-30 status is `REVIEWED` or `DRAFT` with orchestrator waiver; the Completion Record's `status` is `SUCCESS` (zero `BLOCKING` findings) or `SUCCESS_WITH_FINDINGS` (one or more `BLOCKING` findings).
- HC-30.2: The engagement manifest's `final_qa_decision` matches ART-30's `qa_decision` (`COMPLETE` or `INCOMPLETE`).
- HC-30.3: Every sub-check in `QUALITY_STANDARDS.md` § 7.1 through § 7.7 is executed and recorded in ART-30's body.
- HC-30.4: Every HOOK (01 through 05) is re-executed and recorded in ART-30's Hook Re-Execution section.
- HC-30.5: The reconstruction-test attachment exists at its declared path (or `NA` recorded with rationale under `SCOPE_TRIAGE`).
- HC-30.6: The terminal quality-log entry is appended with `"terminal": true`.
- HC-30.7: `repository_fingerprint_recheck` matches ART-01 and `integrity_seal_verified` is `true`.
- HC-30.8: Every `BLOCKING` finding has `resolution_guidance` specifying a producing prompt to re-dispatch.
- HC-30.9: The 200-claim traceability sample's seed is recorded and the verification rate is ≥ 0.95 (or, if below 0.95, the engagement is `INCOMPLETE`).
- HC-30.10: No `BLOCKING` finding is unaccounted for in the Findings Aggregate.

If HC-30.1 through HC-30.10 are satisfied AND `qa_decision == "COMPLETE"`, the engagement is closed and the deliverables in `<output_root>/final/` are released to the engagement owner. If `qa_decision == "INCOMPLETE"`, the engagement is closed-as-incomplete; the deliverables are surfaced with the QA Report prominently displayed so the engagement owner can remediate and re-run.

---

## 12. Cross-References

- **Consumed by:** Orchestrator (terminal). No further prompt is dispatched. The orchestrator reads `final_qa_decision` to determine delivery vs remediation.
- **Depends on:** ART-01 through ART-29 (all prior artifacts), `engagement_manifest.json`, `quality/quality_log.jsonl`.
- **Governing rules:** `OPERATING_RULES.md` R13 (read-only), R15 (fingerprint recheck), R17 (citation format), R19 (no secondary citation), R21 (no invention), R22 (no behavior invention), R23 (UNVERIFIED markers), R24 (no hallucinated citations), R33 (contradiction escalation), R35 (integrity termination on seal invalidation), R47 (no silent edits — enforced by resolution-guidance requirement).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1 (generic header), § 4.8 (Report schema), § 5 (type-specific quality bars), § 7 (Final QA sub-checks), § 8 (Reviewer Hooks), § 9.3 (Anti-Gaming), § 10 (quality records and sealing).
- **Output authority:** `OUTPUT_RULES.md` § 9 (encoding & line endings — re-verified), § 11.4 (integrity seal — re-verified), § 13 (output adapter contract — re-verified), § 14.3 (delivery confirmation — governed by `final_qa_decision`).
- **Forward reference:** NONE. PROMPT_30 is terminal. No `PROMPT_31` exists under any branch. Engagement re-runs (if `INCOMPLETE`) begin again at the producing prompt(s) identified in ART-30's findings, NOT at PROMPT_31.
- **Reverse reference:** This prompt is referenced by `MASTER_INDEX.md` § 2.6 (Phase 4 prompt registry), § 5 (artifact registry: ART-30 Report), § 6 (cross-reference map: terminal node), `QUALITY_STANDARDS.md` § 7 (Final QA authority), `OUTPUT_RULES.md` § 1 (enforcement authority), `PROMPT_28.md` § 12 (BLOCKING findings escalated here), `PROMPT_29.md` § 12 (integrity seal verified here).

*End of PROMPT_30. Engagement terminates on emission of ART-30's Completion Record. No subsequent prompt is dispatched.*
