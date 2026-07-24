# PROMPT_28.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_28: Cross-Reference & Validation Checklists

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_28
- **Phase:** 4
- **Stage:** 3 of 5
- **Dependencies:** ART-01 through ART-27 (all prior artifacts).
- **Estimated Tokens:** 16000–26000
- **Output Artifacts:** ART-28 (Checklist) — Cross-Reference & Validation Checklists.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Cross-Reference & Validation Checklists artifact (ART-28) — the engagement's validation instrument suite — comprising eleven deterministic checklists (Entity Registry, Citation Validation, Bidirectional Link, Coverage, Schema Conformance, Non-Contradiction, Dead-Code Confirmation [HOOK-01], Orphan Event Confirmation [HOOK-02], State Reachability Confirmation [HOOK-03], API Contract Confirmation [HOOK-04], Prompt Exposure Confirmation [HOOK-05]) — each a list of deterministic predicates with `PASS`/`FAIL`/`NA` status, such that every `FAIL` is escalated to PROMPT_30 as a `BLOCKING` finding with affected artifacts and resolution guidance.

---

## 3. When to Invoke

PROMPT_28 is dispatched when ALL of the following predicates hold:

- Phase 3 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.4.
- PROMPT_26 and PROMPT_27 have emitted their completion records. ART-26 and ART-27 may be `DRAFT` with orchestrator waiver; PROMPT_28 verifies their front-matter and bidirectional links even in `DRAFT` status.
- ART-01 through ART-27 (where produced) are present, non-empty, and accessible. Artifacts marked `NOT_PRODUCED` (e.g., ART-19, ART-20 under `SCOPE_CORE`) are accepted; the corresponding checklists record `NA` with rationale.
- The engagement manifest's scope modifier is NOT `SCOPE_TRIAGE`.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 through ART-27 | All prior artifacts | The complete artifact set under validation. Every artifact's front-matter, body, traceability index, and open questions are inspected. |
| `engagement_manifest.json` | Engagement input | The engagement manifest declares the scope modifier, the engagement ID, and the artifact registry. PROMPT_28 uses the registry to enumerate the expected artifact set. |
| `OPERATING_RULES.md` | Framework file | Bind R15 (fingerprint), R17 (citation format), R18 (traceability index), R19 (no secondary citation), R21 (no invention), R22 (no behavior invention), R23 (UNVERIFIED), R24 (no hallucinated citations), R33 (contradiction escalation). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, cross-reference conventions (§ 8: bidirectional links), encoding & line endings (§ 9). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Checklist schema (§ 4.7); enforce per-prompt quality checks (§ 3); execute HOOKs (§ 8: HOOK-01 through HOOK-05). |
| `PROJECT_SPECIFICATION.md` | Framework file | Entity ontology (§ 4) for the Entity Registry; Traceability Contract (§ 6.3) for the Citation Validation Checklist; Non-Contradiction Contract (§ 6.4) for the Non-Contradiction Checklist. |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Assemble the Entity Registry per § 6.1 by enumerating every entity ID across ART-01 through ART-27.
3. Execute the Citation Validation Checklist per § 6.2 — for every claim in every artifact, verify the citation resolves to a real source location.
4. Execute the Bidirectional Link Checklist per § 6.3 — for every cross-reference between two artifacts, verify the back-reference exists.
5. Execute the Coverage Checklist per § 6.4 — for every in-scope file, verify it appears in at least one artifact's `source_coverage`.
6. Execute the Schema Conformance Checklist per § 6.5 — for every artifact, validate its front-matter against its registered schema.
7. Execute the Non-Contradiction Checklist per § 6.6 — for every pair of artifacts sharing an entity, verify they assert consistent facts.
8. Execute the Dead-Code Confirmation (HOOK-01) per § 6.7.
9. Execute the Orphan Event Confirmation (HOOK-02) per § 6.8.
10. Execute the State Reachability Confirmation (HOOK-03) per § 6.9.
11. Execute the API Contract Confirmation (HOOK-04) per § 6.10.
12. Execute the Prompt Exposure Confirmation (HOOK-05) per § 6.11.
13. Aggregate all `FAIL` findings per § 6.12 — every `FAIL` is recorded with `affected_artifacts`, `resolution_guidance`, and `severity` (`BLOCKING` for HOOK failures and schema violations; `MAJOR` for citation and coverage gaps; `MINOR` for documentation issues).
14. Emit ART-28 per § 8 with full front-matter, the eleven checklists, the aggregate findings, the traceability index, the open questions.
15. Run the Quality Checks in § 9.
16. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Entity Registry Assembly

Assemble the Entity Registry by enumerating every entity ID across ART-01 through ART-27. The registry is the single source of truth for entity resolution; every downstream checklist consults it.

For each artifact, parse the front-matter and body for entity references matching the prefixes enumerated in `PROJECT_SPECIFICATION.md` § 4.1: `F-`, `D-`, `M-`, `C-`, `K-`, `I-`, `FN-`, `V-`, `A-`, `E-`, `S-`, `W-`, `P-`, `DEP-`, `CFG-`, `PR-`, `T-`, plus the artifact-local prefixes (`D-XX` for data types in ART-11, `S-XX` for stateful units in ART-13, `E-XX` for events in ART-14, `CL-XX` for LLM clients in ART-21, `AG-XX` for agents, `MEM-XX` for memory systems, `RAG-XX` for RAG pipelines, `SP-XX` for streaming primitives, `SW-XX` for streaming workflows, `DEC-XX` for engineering decisions, etc.).

Each registry entry records: `entity_id`, `entity_kind`, `declaring_artifact` (the first `ART-XX` to declare the entity), `referencing_artifacts` (list of `ART-XX` that reference the entity), `declaration_location` (`file:line-range` within the declaring artifact), `status` (declared | referenced-only | unresolved). Entities that are referenced but never declared are `unresolved` and are `CONTRADICTION` findings per R33.

### 6.2 Citation Validation Checklist

For every claim in every artifact's `traceability_index`, verify the citation resolves to a real source location in the subject repository. The procedure:

1. Parse the artifact's `traceability_index` to obtain the list of `(claim_id, source, symbol)` triples.
2. For each triple, extract the `file_path` and `line_range` from the `source` field.
3. Verify the `file_path` exists in the in-scope path set (ART-01).
4. Verify the `line_range` falls within the file's actual line count (read the file; if the file is binary or generated, mark `UNVERIFIED` per R16).
5. Verify the `symbol` is present in the cited line range (parse the source file and search for the symbol name within the range).
6. Mark the claim `PASS` if all three checks succeed; `FAIL` otherwise.

Aggregate-claims (per R20) cite an enumeration procedure rather than a single line; for these, verify the procedure is reproducible by re-running it on a sample and confirming the count matches.

Each checklist entry records: `check_id` `CHK-CTX-XX`, `claim_id`, `artifact_id`, `predicate` ("citation resolves to source location"), `status` (`PASS` | `FAIL` | `NA`), `evidence` (`file_path:line_range` of the verification), `failure_detail` (when `FAIL`).

The threshold for the Citation Validation Checklist is `PASS / (PASS + FAIL) ≥ 0.95` per `QUALITY_STANDARDS.md` § 2.2.

### 6.3 Bidirectional Link Checklist

For every cross-reference between two artifacts, verify the back-reference exists. The procedure:

1. Parse every artifact's `Cross-References` section (or front-matter `cross_references` field).
2. For each cross-reference `A → B`, verify that artifact B's `Cross-References` section contains a back-reference `B → A`.
3. Verify the cross-reference's link format conforms to `OUTPUT_RULES.md` § 8.1: `[ART-XX](../phaseN/ARTXX_<engagement_id>_<slug>.md)`.
4. Verify the linked file actually exists at the relative path.

Each checklist entry records: `check_id` `CHK-BIL-XX`, `source_artifact`, `target_artifact`, `predicate` ("bidirectional link present and resolvable"), `status`, `evidence`, `failure_detail`.

The threshold for the Bidirectional Link Checklist is 100% — every cross-reference MUST be bidirectional per `OUTPUT_RULES.md` § 8.3.

### 6.4 Coverage Checklist

For every in-scope file, verify it appears in at least one artifact's `source_coverage`. The procedure:

1. Parse ART-01's in-scope path set.
2. For each in-scope file, search every artifact's `source_coverage` for an entry with `path` matching the file.
3. Mark the file `PASS` if found in at least one artifact; `FAIL` otherwise.

Each checklist entry records: `check_id` `CHK-COV-XX`, `file_path`, `predicate` ("file appears in at least one artifact's source_coverage"), `status`, `evidence` (the artifact ID where the file appears), `failure_detail`.

The threshold for the Coverage Checklist is ≥ 0.99 per `MISSION.md` § 3 success criterion 1 (Coverage). Files that are `UNANALYZABLE` (binary, generated) per R16 are excluded from the denominator and recorded as `NA` with rationale.

### 6.5 Schema Conformance Checklist

For every artifact, validate its front-matter against its registered schema. The procedure:

1. Parse the artifact's front-matter.
2. Look up the artifact's type in `QUALITY_STANDARDS.md` § 4 (Manifest § 4.2, Map § 4.3, Graph § 4.4, Doc § 4.5, Handbook § 4.6, Checklist § 4.7, Report § 4.8, plus the generic header § 4.1).
3. Validate every required field is present and has the correct type.
4. Validate every field's value conforms to its declared enumeration (e.g., `status` MUST be one of `DRAFT`, `REVIEWED`, `APPROVED`, `REJECTED`, `TERMINATED`, `INVALIDATED`).
5. Validate the `quality_score` aggregate equals the sum of the eight dimension scores.

Each checklist entry records: `check_id` `CHK-SCH-XX`, `artifact_id`, `predicate` ("front-matter conforms to registered schema"), `status`, `evidence`, `failure_detail` (field-level when `FAIL`).

The threshold for the Schema Conformance Checklist is 100% — every artifact MUST conform to its schema per `QUALITY_STANDARDS.md` § 3 Q3.

### 6.6 Non-Contradiction Checklist

For every pair of artifacts sharing an entity, verify they assert consistent facts about that entity. The procedure:

1. Parse the Entity Registry (§ 6.1) to obtain the set of shared entities (entities referenced by more than one artifact).
2. For each shared entity, extract the assertions from each referencing artifact's `traceability_index` and body.
3. Compare the assertions for consistency. Consistency is defined as: no two artifacts assert contradictory properties of the same entity (e.g., ART-08 says `K-01` is a class; ART-11 says `K-01` is a data type — these are contradictory).
4. Where the subject system is itself contradictory (per `PROJECT_SPECIFICATION.md` § 6.4), both artifacts MUST cite the contradiction explicitly with `SYSTEM_INCONSISTENCY` markers. Absent the markers, the contradiction is a `FAIL`.

Each checklist entry records: `check_id` `CHK-NCN-XX`, `entity_id`, `artifacts` (the pair), `predicate` ("artifacts assert consistent facts about entity"), `status`, `evidence`, `failure_detail`.

The threshold for the Non-Contradiction Checklist is 100% per `PROJECT_SPECIFICATION.md` § 6.4.

### 6.7 Dead-Code Confirmation (HOOK-01)

Execute HOOK-01 per `QUALITY_STANDARDS.md` § 8. The procedure:

1. Parse ART-09's function catalog for functions flagged `DEAD_CODE_CANDIDATE`.
2. For each flagged function, verify the flag is justified: trace the call graph from ART-10 and confirm no caller exists in the transitive closure from ART-05's entry points.
3. Verify the flagged function is recorded in ART-09's `open_questions` with a `DEAD_CODE_CONFIRMATION` question.
4. Verify the flagged function is NOT recorded as called by any other artifact (ART-11, ART-12, ART-14, ART-21, ART-22) — if it is, the flag is a `FAIL` (the flag is incorrect because the function is reachable via a path ART-09 missed).

Each checklist entry records: `check_id` `CHK-HK1-XX`, `function_id` `FN-XX`, `predicate` ("HOOK-01: dead-code candidate is genuinely unreachable"), `status`, `evidence`, `failure_detail`.

### 6.8 Orphan Event Confirmation (HOOK-02)

Execute HOOK-02 per `QUALITY_STANDARDS.md` § 8. The procedure:

1. Parse ART-14's event catalog for every `E-XX`.
2. For each event, verify it has at least one emitter (an `FN-XX` recorded with `EMITS` relationship) AND at least one handler (an `FN-XX` recorded with `HANDLES` relationship).
3. Events with only an emitter are `ORPHAN_EMITTER_ONLY` — `FAIL`.
4. Events with only a handler are `ORPHAN_HANDLER_ONLY` — `FAIL`.
5. Events with neither emitter nor handler are `ORPHAN_EVENT` — `FAIL`.

Each checklist entry records: `check_id` `CHK-HK2-XX`, `event_id` `E-XX`, `predicate` ("HOOK-02: event has both emitter and handler"), `status`, `evidence`, `failure_detail`.

### 6.9 State Reachability Confirmation (HOOK-03)

Execute HOOK-03 per `QUALITY_STANDARDS.md` § 8. The procedure:

1. Parse ART-13's state machine catalog for every `S-XX` state machine and its states.
2. For each state, verify it is reachable from an initial state (perform a graph traversal from the initial state; any state not visited is `UNREACHABLE` — `FAIL`).
3. For each state, verify it can reach a terminal state (perform a reverse graph traversal from the terminal states; any state not visited is `NO_TERMINAL` — `FAIL`, unless the state machine explicitly declares no terminal states).
4. Verify ART-25's state machine diagrams highlight `UNREACHABLE` and `NO_TERMINAL` states per Q-25.G.

Each checklist entry records: `check_id` `CHK-HK3-XX`, `state_id` `S-XX`, `predicate` ("HOOK-03: state is reachable from initial and can reach terminal"), `status`, `evidence`, `failure_detail`.

### 6.10 API Contract Confirmation (HOOK-04)

Execute HOOK-04 per `QUALITY_STANDARDS.md` § 8. The procedure:

1. Parse ART-15's API reference for every `A-XX`.
2. For each API, verify it has documented: parameters (request body and query params), response (status code and body schema), errors (the error cases), and authentication requirement (the auth mechanism or `none`).
3. APIs missing any of the four are `FAIL`.
4. Verify the API's authentication requirement is consistent with ART-19's auth report (when ART-19 is present). APIs declared `none` in ART-15 but protected by an auth middleware in ART-19 are `FAIL` (contradiction); APIs declared protected in ART-15 but bypassed in ART-19 are `FAIL` (auth-bypass risk).

Each checklist entry records: `check_id` `CHK-HK4-XX`, `api_id` `A-XX`, `predicate` ("HOOK-04: API has documented parameters, response, errors, and authentication requirement"), `status`, `evidence`, `failure_detail`.

### 6.11 Prompt Exposure Confirmation (HOOK-05)

Execute HOOK-05 per `QUALITY_STANDARDS.md` § 8. The procedure:

1. Parse ART-21's prompt-template catalog for every `PR-XX`.
2. For each prompt template, verify it is documented verbatim (`template_body_verbatim` is populated with the actual string) OR marked `REDACTED` with `redaction_rationale`, `redaction_evidence`, and `redacted_length` all populated.
3. Prompt templates with an empty body and no REDACTED marker are `FAIL`.
4. Verify every prompt template referenced by an LLM call (`LC-XX`) in ART-21 has a corresponding `PR-XX`. Dangling references are `FAIL`.
5. Verify every tool's `description` (per ART-21's `T-XX`) is recorded as a `PR-XX` with `kind: tool-description`. Missing tool-description prompts are `FAIL`.

Each checklist entry records: `check_id` `CHK-HK5-XX`, `prompt_id` `PR-XX`, `predicate` ("HOOK-05: prompt template is documented verbatim or REDACTED with rationale"), `status`, `evidence`, `failure_detail`.

### 6.12 Findings Aggregation

Aggregate all `FAIL` findings. Each finding records:

- `finding_id` `F-XX`
- `severity` (`BLOCKING` for HOOK failures and schema violations; `MAJOR` for citation, coverage, and contradiction failures; `MINOR` for documentation issues)
- `checklist` (the originating checklist)
- `affected_artifacts` (list of `ART-XX`)
- `description` (the failure detail)
- `resolution_guidance` (the specific action to resolve the finding — e.g., "Re-dispatch PROMPT_09 to update the function catalog; the function `FN-XX` is referenced by ART-11 but missing from ART-09")

`BLOCKING` findings are escalated to PROMPT_30 for the final QA decision. `MAJOR` and `MINOR` findings are recorded for the engagement's quality audit trail but do not block engagement completion unless PROMPT_30 determines they aggregate to a systemic quality failure.

---

## 7. Required Outputs

### ART-28 — Cross-Reference & Validation Checklists

**Type:** Checklist.

**Acceptance Criteria:**

- AC-28.1: The artifact file exists at `<output_root>/artifacts/phase4/ART28_<engagement_id>_validation-checklists.md`.
- AC-28.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.7 (Checklist schema).
- AC-28.3: The body contains the eleven checklists in order: Entity Registry, Citation Validation, Bidirectional Link, Coverage, Schema Conformance, Non-Contradiction, Dead-Code Confirmation (HOOK-01), Orphan Event Confirmation (HOOK-02), State Reachability Confirmation (HOOK-03), API Contract Confirmation (HOOK-04), Prompt Exposure Confirmation (HOOK-05). Plus Findings Aggregate, Traceability Index, Open Questions, Cross-References.
- AC-28.4: Every checklist entry records `check_id`, `predicate`, `status`, `evidence`, `failure_detail`.
- AC-28.5: Every `FAIL` finding is recorded in the Findings Aggregate with `severity`, `affected_artifacts`, `resolution_guidance`.
- AC-28.6: Every `BLOCKING` finding is escalated to PROMPT_30.
- AC-28.7: Every HOOK (01 through 05) is executed and its results recorded in the corresponding checklist.

---

## 8. Output Templates

### 8.1 ART-28 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-28
artifact_type: Checklist
producing_prompt: PROMPT_28
phase: 4
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
entity_registry:
  total_entities: <int>
  by_kind: { F: <int>, D: <int>, M: <int>, C: <int>, K: <int>, I: <int>, FN: <int>, V: <int>, A: <int>, E: <int>, S: <int>, W: <int>, P: <int>, DEP: <int>, CFG: <int>, PR: <int>, T: <int> }
  unresolved: [ { entity_id: <id>, referencing_artifacts: [ART-XX] } ]
checklists:
  - id: CHK-CTX-01
    name: Citation Validation
    threshold: 0.95
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
  - id: CHK-BIL-01
    name: Bidirectional Link
    threshold: 1.0
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
  - id: CHK-COV-01
    name: Coverage
    threshold: 0.99
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
  - id: CHK-SCH-01
    name: Schema Conformance
    threshold: 1.0
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
  - id: CHK-NCN-01
    name: Non-Contradiction
    threshold: 1.0
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
  - id: CHK-HK1-01
    name: Dead-Code Confirmation (HOOK-01)
    threshold: 1.0
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
  - id: CHK-HK2-01
    name: Orphan Event Confirmation (HOOK-02)
    threshold: 1.0
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
  - id: CHK-HK3-01
    name: State Reachability Confirmation (HOOK-03)
    threshold: 1.0
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
  - id: CHK-HK4-01
    name: API Contract Confirmation (HOOK-04)
    threshold: 1.0
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
  - id: CHK-HK5-01
    name: Prompt Exposure Confirmation (HOOK-05)
    threshold: 1.0
    pass_count: <int>
    fail_count: <int>
    na_count: <int>
    pass_fraction: <0..1>
    status: PASS | FAIL
findings:
  - id: F-01
    severity: BLOCKING | MAJOR | MINOR
    checklist: CHK-XX-XX
    affected_artifacts: [ART-XX]
    description: <text>
    resolution_guidance: <text>
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
checks:
  - id: CHK-XX
    description: <string>
    predicate: <deterministic expression>
    status: PASS | FAIL | NA
---
```

### 8.2 ART-28 Body Skeleton

```markdown
# ART-28: Cross-Reference & Validation Checklists

## 1. Executive Summary
## 2. Methodology
## 3. Entity Registry
   - Total entities: <int>
   - By kind: <breakdown>
   - Unresolved: <list>
## 4. Citation Validation Checklist
   - Threshold: 0.95
   - Pass: <int>, Fail: <int>, NA: <int>
   - Status: PASS | FAIL
   - Failure details: <list>
## 5. Bidirectional Link Checklist
## 6. Coverage Checklist
## 7. Schema Conformance Checklist
## 8. Non-Contradiction Checklist
## 9. Dead-Code Confirmation (HOOK-01)
## 10. Orphan Event Confirmation (HOOK-02)
## 11. State Reachability Confirmation (HOOK-03)
## 12. API Contract Confirmation (HOOK-04)
## 13. Prompt Exposure Confirmation (HOOK-05)
## 14. Findings Aggregate
   ### 14.1 BLOCKING Findings
   ### 14.2 MAJOR Findings
   ### 14.3 MINOR Findings
## 15. Traceability Index
## 16. Open Questions
## 17. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every artifact in the engagement manifest's artifact registry is examined by at least one checklist. Threshold ≥ 0.95.
- **Q2. Citation Check** — ≥ 0.95 of ART-28's own claims cite the artifacts they reference.
- **Q3. Schema Conformance Check** — ART-28 validates against § 4.7 (Checklist schema).
- **Q4. Non-Contradiction Check** — ART-28's findings do not contradict the artifacts they reference.
- **Q5. UNVERIFIED Accounting** — every `NA` checklist entry has a corresponding Open Question explaining why the check is not applicable.
- **Q6. Idempotence Spot-Check** — re-running § 6.2 on a 5% sample of citations yields the same PASS/FAIL status.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-28.A. Checklist Completeness** — all eleven checklists are present and populated. A missing checklist is a `BLOCKING` finding.
- **Q-28.B. Predicate Determinism** — every checklist entry's `predicate` is a deterministic expression that two independent executors would evaluate identically. Non-deterministic predicates are `BLOCKING` findings per `QUALITY_STANDARDS.md` § 5 (Checklist bar: Predicate determinism = 5).
- **Q-28.C. HOOK Execution** — every HOOK (01 through 05) is executed and its results recorded. A missing HOOK execution is a `BLOCKING` finding.
- **Q-28.D. Findings Escalation** — every `BLOCKING` finding is escalated to PROMPT_30 (recorded in `open_questions` with `blocking: true` and a forward-reference to PROMPT_30). Unescalated `BLOCKING` findings are `BLOCKING` findings themselves.
- **Q-28.E. Resolution Guidance Specificity** — every `FAIL` finding's `resolution_guidance` specifies the producing prompt to re-dispatch (e.g., "Re-dispatch PROMPT_09"). Generic guidance ("fix the issue") is a `MINOR` finding.
- **Q-28.F. Entity Registry Closure** — every entity referenced by any artifact appears in the Entity Registry. Unregistered entities are `BLOCKING` findings per R21.
- **Q-28.G. Cross-Artifact Entity Consistency** — for each shared entity, the declaring artifact and the referencing artifacts agree on the entity's kind (e.g., a `K-XX` is a class in every artifact that references it). Kind mismatches are `BLOCKING` findings per R33.

---

## 10. Common Pitfalls

- Do not record `PASS` for a citation without verifying the source location; verification requires reading the cited file. Unverified `PASS` marks are `BLOCKING` per R24.
- Always execute every HOOK; partial HOOK execution is `BLOCKING` per Q-28.C. When the corresponding artifact is absent (e.g., ART-21 absent → HOOK-05 not executable), record `NA` with rationale per Q-28.A.
- Always escalate `BLOCKING` findings to PROMPT_30; unescalated findings defeat the engagement's QA gate per Q-28.D.
- Do not provide generic resolution guidance; specify the producing prompt and the corrective action per Q-28.E.
- Always register every entity in the Entity Registry; unregistered entities fail Q-28.F.
- Do not skip the Bidirectional Link Checklist even when artifacts are `DRAFT`; bidirectionality is a contract per `OUTPUT_RULES.md` § 8.3.
- Always mark `NA` with rationale when an artifact is absent; an absent artifact is not a `FAIL` unless the artifact is mandatory.
- Do not infer entity kind from naming; verify against the declaring artifact's record. A `K-XX` referenced as a data type is a real contradiction per Q-28.G.
- Always distinguish `BLOCKING` from `MAJOR` from `MINOR` severity correctly; HOOK failures and schema violations are `BLOCKING`, citation/coverage/contradiction failures are `MAJOR`, documentation issues are `MINOR`.
- Always re-dispatch the producing prompt for `FAIL` findings rather than directly editing artifacts; direct edits violate R47 (no silent edits).

---

## 11. Handoff Criteria

PROMPT_30 consumes ART-28. Handoff requires ALL of:

- HC-28.1: ART-28 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-28.2: All eleven checklists are present and populated.
- HC-28.3: Every HOOK (01 through 05) is executed or marked `NA` with rationale.
- HC-28.4: Every `BLOCKING` finding is escalated to PROMPT_30.
- HC-28.5: Every `FAIL` finding has `resolution_guidance` specifying the producing prompt.
- HC-28.6: Entity Registry is complete; every referenced entity is registered.
- HC-28.7: `repository_fingerprint_recheck` matches ART-01.
- HC-28.8: No unresolved `BLOCKING` open questions remain (they are escalated to PROMPT_30).

---

## 12. Cross-References

- **Consumed by:** PROMPT_30 (Self-Review & QA — consumes ART-28's findings as inputs to the final QA decision; verifies that every `BLOCKING` finding was addressed or escalated).
- **Depends on:** ART-01 through ART-27 (all prior artifacts), the engagement manifest.
- **Governing rules:** `OPERATING_RULES.md` R15, R17, R18, R19, R21, R22, R23, R24 (no hallucinated citations — directly enforced by the Citation Validation Checklist), R33 (contradiction escalation — directly enforced by the Non-Contradiction Checklist), R47 (no silent edits — enforced by the resolution-guidance requirement).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.7; Checklist bar (aggregate ≥ 32, Predicate determinism = 5, Completeness = 5).
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 8 (cross-reference conventions), § 9 (encoding & line endings — verified for every artifact).
- **HOOK integration:** All five HOOKs (HOOK-01 through HOOK-05) are executed here per `QUALITY_STANDARDS.md` § 8. PROMPT_30 re-executes the HOOKs as part of the terminal QA pass.

*End of PROMPT_28. Orchestrator may dispatch PROMPT_29 upon satisfaction of § 11.*
