# QUALITY_STANDARDS.md
## Enterprise Reverse Engineering Prompt Framework — Quality Standards

> **Document Type:** Quality Bar & Validation Specification
> **Framework Version:** 1.0.0
> **Authority:** Binding. Enforced by PROMPT_28 and PROMPT_30.
> **Audience:** Reviewers, agents, orchestrators.

---

## 1. Purpose of This Document

This document defines the **quality bar** that every artifact produced by the framework MUST meet. It also defines the **scoring rubric**, the **validation gates**, the **artifact schemas**, and the **reviewer hooks**. Quality is not aspirational in this framework; it is a measurable, enforced property of every artifact, and artifacts that fail the bar are rejected and re-dispatched.

The standards exist because reverse-engineering documentation is worthless if untrustworthy. A reader of the documentation will make engineering decisions based on it; if the documentation is shallow, contradictory, or untraceable, those decisions will be wrong and the cost will be paid downstream. The standards encode the minimum properties an artifact must have to be trustworthy enough to act on.

This document is consumed by three parties: the agent (which self-checks against § 3 before emitting a Completion Record), the reviewer (which independently validates per § 6), and PROMPT_30 (which runs the final QA pass per § 7).

---

## 2. Quality Dimensions

Every artifact is scored along eight dimensions, each on a 0–5 scale. The aggregate is the sum (max 40). An artifact is conformant only if every dimension ≥ 3 and aggregate ≥ 28. Higher bars apply to certain artifact types (§ 5).

### 2.1 Coverage (0–5)
The fraction of in-scope entities the artifact represents.
- **5:** Every in-scope entity represented; unanalyzable entities explicitly marked `UNANALYZABLE` with rationale.
- **4:** ≥ 95% represented; gaps documented.
- **3:** ≥ 90% represented; gaps documented.
- **2:** 70–90% represented; gaps undocumented.
- **1:** < 70% represented.
- **0:** Coverage not measurable.

### 2.2 Traceability (0–5)
The fraction of factual claims with valid source citations.
- **5:** 100% of claims cite a source location; citations verified by PROMPT_28.
- **4:** ≥ 98% cited.
- **3:** ≥ 95% cited.
- **2:** 80–95% cited.
- **1:** < 80% cited.
- **0:** No citation discipline.

### 2.3 Accuracy (0–5)
The fraction of cited claims that are verifiable against the source.
- **5:** 100% of sampled citations verify.
- **4:** ≥ 98% verify.
- **3:** ≥ 95% verify.
- **2:** 80–95% verify.
- **1:** < 80% verify.
- **0:** Verification not possible (citations fabricated).

### 2.4 Depth (0–5)
The presence of mechanism, not just description.
- **5:** Every described behavior is explained in terms of the code that implements it; alternative paths documented.
- **4:** Most behaviors mechanistically explained.
- **3:** Behaviors described with implementation references.
- **2:** Behaviors described without implementation references.
- **1:** Superficial description.
- **0:** No description.

### 2.5 Coherence (0–5)
Internal consistency and cross-artifact consistency.
- **5:** No internal contradictions; no contradictions with other artifacts; system inconsistencies explicitly flagged.
- **4:** No contradictions; minor wording inconsistencies.
- **3:** No factual contradictions; structural inconsistencies.
- **2:** Minor factual contradictions.
- **1:** Major contradictions.
- **0:** Incoherent.

### 2.6 Precision (0–5)
Specificity of identifiers and line ranges.
- **5:** Every reference uses file path + line range + symbol name.
- **4:** Every reference uses file path + symbol name.
- **3:** Most references use file path + symbol.
- **2:** Generic references ("the router").
- **1:** Vague references.
- **0:** No references.

### 2.7 Completeness (0–5)
Presence of all required sections per the Artifact Contract.
- **5:** All required sections present and populated.
- **4:** All sections present; one under-populated.
- **3:** All sections present; multiple under-populated.
- **2:** Missing section.
- **1:** Multiple missing sections.
- **0:** Structure absent.

### 2.8 Readability (0–5)
Clarity, structure, navigability for the End Consumer.
- **5:** Clear hierarchy, navigable, diagrams where useful, no jargon without definition.
- **4:** Clear hierarchy; minor navigability gaps.
- **3:** Structured; some dense passages.
- **2:** Weakly structured.
- **1:** Unstructured.
- **0:** Unreadable.

---

## 3. Per-Prompt Quality Checks (Self-Check)

Every prompt's § 9 Quality Checks MUST include the following baseline checks in addition to prompt-specific checks. The agent evaluates these before emitting its Completion Record.

### Q1. Coverage Check
- **Predicate:** `coverage_fraction ≥ threshold` (threshold declared per prompt; default 0.90).
- **On fail:** Report `COVERAGE_GAP` with list of uncovered entities.

### Q2. Citation Check
- **Predicate:** `cited_claims / total_claims ≥ 0.95`.
- **On fail:** Report `CITATION_GAP` with uncited claims.

### Q3. Schema Conformance Check
- **Predicate:** Artifact conforms to its registered schema (§ 4).
- **On fail:** Report `SCHEMA_VIOLATION` with field-level detail.

### Q4. Non-Contradiction Check
- **Predicate:** No claim in this artifact contradicts a claim in any registered artifact.
- **On fail:** Report `CONTRADICTION` with both claim references.

### Q5. UNVERIFIED Accounting
- **Predicate:** Every `UNVERIFIED` marker has a corresponding Open Questions entry.
- **On fail:** Report `ORPHAN_UNVERIFIED`.

### Q6. Idempotence Spot-Check
- **Predicate:** Re-running a sampled sub-procedure yields equivalent entity set.
- **On fail:** Report `NON_IDEMPOTENT` with divergence detail.

### Q7. Handoff Readiness
- **Predicate:** All Handoff Criteria (prompt § 11) satisfied.
- **On fail:** Report `HANDOFF_BLOCK`.

---

## 4. Artifact Schemas

Every artifact type has a registered schema expressed as YAML front-matter. Schemas are normative; PROMPT_28 validates conformance.

### 4.1 Generic Artifact Header (all types)

```yaml
---
engagement_id: <uuid>
artifact_id: ART-XX
artifact_type: Manifest | Map | Spec | Doc | Graph | Diagrams | Handbook | Checklist | Suite | Report
producing_prompt: PROMPT_XX
phase: 1 | 2 | 3 | 4
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score:
  coverage: <0..5>
  traceability: <0..5>
  accuracy: <0..5>
  depth: <0..5>
  coherence: <0..5>
  precision: <0..5>
  completeness: <0..5>
  readability: <0..5>
  aggregate: <0..40>
status: DRAFT | REVIEWED | APPROVED | REJECTED | TERMINATED | INVALIDATED
source_coverage:
  - path: <file_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-XX
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-XX
    source: <file_path>:<start-end>
    symbol: <name>
---
```

### 4.2 Manifest Schema (extends header)
```yaml
items:
  - id: <entity_id>
    name: <string>
    type: <entity_type>
    location: <file_path>:<line>
    attributes: { ... }
```

### 4.3 Map Schema (extends header)
```yaml
nodes:
  - id: <entity_id>
    label: <string>
    kind: <entity_type>
edges:
  - from: <entity_id>
    to: <entity_id>
    relationship: <relationship_type>
    evidence: <file_path>:<line>
```

### 4.4 Graph Schema (extends Map schema)
Adds `layout_hint` and `mermaid_source` fields. The Mermaid source is also written as a `.mmd` sidecar file.

### 4.5 Doc Schema (extends header)
```yaml
sections:
  - id: S-XX
    title: <string>
    claims: [C-XX, ...]
```

### 4.6 Handbook Schema (extends Doc)
Adds `reconstructive: true` and a `reconstruction_steps` array.

### 4.7 Checklist Schema (extends header)
```yaml
checks:
  - id: CHK-XX
    description: <string>
    predicate: <deterministic expression>
    status: PASS | FAIL | NA
```

### 4.8 Report Schema (extends header)
```yaml
findings:
  - id: F-XX
    severity: BLOCKING | MAJOR | MINOR | INFO
    description: <text>
    affected_artifacts: [ART-XX, ...]
    resolution: <text>
```

---

## 5. Type-Specific Quality Bars

Certain artifact types carry higher bars because of their downstream criticality.

| Artifact Type | Min Aggregate | Min Critical Dimensions |
|---------------|---------------|-------------------------|
| Manifest | 30 | Coverage ≥ 4, Traceability ≥ 4 |
| Map | 30 | Coverage ≥ 4, Precision ≥ 4 |
| Graph | 30 | Coverage ≥ 4, Coherence ≥ 4 |
| Spec | 32 | Accuracy ≥ 4, Depth ≥ 4 |
| Doc | 30 | Traceability ≥ 4, Depth ≥ 4 |
| Handbook | 32 | Reconstructive completeness ≥ 4, Readability ≥ 4 |
| Checklist | 32 | Predicate determinism = 5, Completeness = 5 |
| Report | 32 | Findings traceable = 5 |

---

## 6. Reviewer Validation Protocol

### 6.1 Sampling
For each artifact, the reviewer independently verifies a random sample of citations:
- Artifacts with ≤ 50 claims: 100% verified.
- Artifacts with 51–500 claims: 50 verified.
- Artifacts with > 500 claims: 100 verified + 10% of remainder.

### 6.2 Independent Re-Run
For prompts declared idempotent (most are), the reviewer re-runs a sampled sub-procedure and confirms the entity set matches. Divergence > 2% triggers `NON_IDEMPOTENT`.

### 6.3 Cross-Artifact Consistency
The reviewer checks a sample of entity references across artifacts. A reference to entity X in artifact A MUST resolve to the same entity in artifact B. Resolution mismatches trigger `CONTRADICTION`.

### 6.4 Sign-Off
The reviewer signs off only when:
- All sampled citations verify (Accuracy ≥ 4).
- No cross-artifact contradictions.
- Aggregate score meets the type-specific bar.
- No `BLOCKING` findings.

Sign-off is recorded as a status transition to `REVIEWED`. Final approval to `APPROVED` is the orchestrator's.

---

## 7. Final QA (PROMPT_30)

PROMPT_30 runs the engagement's final QA pass. It checks:

### 7.1 Engagement-Wide Coverage
Every in-scope file appears in at least one artifact's `source_coverage`. Files with no representation are `COVERAGE_GAP` findings.

### 7.2 Engagement-Wide Traceability
A random sample of 200 claims across all artifacts is verified. < 95% verification triggers a `BLOCKING` finding.

### 7.3 Contradiction Sweep
All pairs of artifacts sharing an entity are checked for contradictory claims. Any contradiction is `BLOCKING`.

### 7.4 Reconstructability Test
A reviewer (human or simulated) attempts to reconstruct one module from the documentation alone. Failure to reconstruct triggers a `BLOCKING` finding against the Rebuild Guide (ART-26) and the Developer Handbook (ART-27).

### 7.5 Schema Conformance Sweep
All artifacts are re-validated against their schemas. Violations are `BLOCKING`.

### 7.6 Handoff Closure
All Open Questions across all artifacts are reviewed. Blocking open questions at engagement end are `BLOCKING` findings.

### 7.7 QA Report
PROMPT_30 emits the QA Report (ART-30) listing all findings with severity. Engagement completes only if zero `BLOCKING` findings.

---

## 8. Reviewer Hooks

Reviewer hooks are deterministic checks registered here and executed by PROMPT_30 in addition to the standard checks. Hooks are identified by `HOOK-XX`.

### HOOK-01. Dead-Code Detection Hook
Verifies that the Function Reference (ART-09) flags functions with zero callers (after transitive closure from entry points) as `DEAD_CODE_CANDIDATE`.

### HOOK-02. Orphan Event Hook
Verifies that every event in the Event Workflow Catalog (ART-14) has both an emitter and a handler. Orphans are flagged.

### HOOK-03. State Reachability Hook
Verifies that every state in the State Machine Catalog (ART-13) is reachable from an initial state and that every state can reach a terminal state (where applicable).

### HOOK-04. API Contract Hook
Verifies that every API in ART-15 has documented inputs, outputs, errors, and authentication requirement.

### HOOK-05. Prompt Exposure Hook
For AI workflows (ART-21), verifies that every prompt template is documented verbatim or, where redacted, marked `REDACTED` with rationale.

Hooks are added per `PROJECT_SPECIFICATION.md` § 10.3. New hooks are minor version bumps.

---

## 9. Quality Drift Prevention

### 9.1 Calibration
Quarterly, the framework is re-calibrated by re-running a benchmark engagement against a reference repository. Aggregate scores are compared to the prior calibration. Drift > 1 point on any dimension triggers investigation.

### 9.2 Reviewer Rotation
No reviewer signs off on the same artifact type for more than three consecutive engagements on the same subject. Rotation prevents familiarity bias.

### 9.3 Anti-Gaming
The agent MUST NOT optimize for score without substance. PROMPT_30 includes adversarial checks for score inflation (e.g., claims that cite but do not substantiate). Inflation triggers `BLOCKING`.

---

## 10. Quality Records

All quality scores, sampling results, and reviewer sign-offs are recorded in `<output_root>/quality/quality_log.jsonl`. The log is append-only and is the engagement's quality audit trail. The log is sealed at termination.

---

*End of Quality Standards. Proceed to `OUTPUT_RULES.md`.*
