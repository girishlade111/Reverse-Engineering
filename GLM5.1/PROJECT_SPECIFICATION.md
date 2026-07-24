# PROJECT_SPECIFICATION.md
## Enterprise Reverse Engineering Prompt Framework — Formal Specification

> **Document Type:** Formal Specification
> **Framework Version:** 1.0.0
> **Status:** Normative
> **Audience:** Architects, prompt authors, reviewers, integrators

---

## 1. Purpose & Scope of This Document

This document is the **normative specification** of the Enterprise Reverse Engineering Prompt Framework (EREPF). It defines — with engineering precision — the framework's inputs, outputs, contracts, data model, lifecycle, conformance requirements, and extension points. Where other files describe *how* the agent works, this file describes *what the framework is* in terms that a test harness, a compliance auditor, or a re-implementor could verify against.

This specification is binding. Any implementation, fork, or adaptation of EREPF that does not satisfy the conformance clauses in §11 is not EREPF and MUST NOT claim compatibility. The specification deliberately separates descriptive content (what the framework does) from prescriptive content (what implementations must do), and the prescriptive content is concentrated in §11 Conformance.

The scope of this document is the framework itself. It does not specify the subject repositories the framework consumes; those are governed by `MISSION.md` § Scope Modifiers. It does not specify the host runtime; it assumes only that the runtime can load Markdown files as context and dispatch prompts per `MASTER_PROMPT.md`.

---

## 2. Inputs

### 2.1 Engagement Inputs (provided by orchestrator)

| Input | Type | Required | Description |
|-------|------|----------|-------------|
| `subject_path` | string (path or URL) | Yes | Location of the repository to be reverse-engineered. |
| `scope_modifier` | enum | Yes | One of `SCOPE_FULL`, `SCOPE_CORE`, `SCOPE_TRIAGE`, `SCOPE_MODULE`. |
| `target_module` | string | Conditional | Required iff `scope_modifier = SCOPE_MODULE`. |
| `output_root` | string (path) | Yes | Directory where artifacts are written. |
| `engagement_id` | string (UUID) | Yes | Unique identifier for the engagement. |
| `token_budget` | integer | Yes | Total token budget for the engagement. |
| `tool_budget` | object | Yes | Per-tool invocation caps (read, grep, glob, etc.). |
| `authorization` | string | Yes | Written authorization statement satisfying §5 Ethics. |
| `language_directive` | enum | No | Override for output language. Default: English. |

### 2.2 Framework Inputs (provided by the framework itself)

All files listed in `MASTER_INDEX.md` § 2 are framework inputs. They are read-only within an engagement and may not be modified mid-engagement.

### 2.3 Prompt Inputs (declared per prompt)

Each `PROMPT_XX.md` declares its required inputs in §4 Required Inputs. Inputs are artifact IDs (from `MASTER_INDEX.md` § 5) or framework files. A prompt may not begin execution until all declared inputs are present and non-empty.

---

## 3. Outputs

### 3.1 Artifact Types

The framework produces artifacts of seven canonical types. Every artifact declares exactly one type.

| Type | Extension | Description |
|------|-----------|-------------|
| Manifest | `.md` | Structured inventory (lists of items with metadata). |
| Map | `.md` | Topological or structural representation. |
| Spec | `.md` | Formal description of a system aspect. |
| Doc | `.md` | Narrative engineering documentation. |
| Graph | `.md` + `.mmd` | Graph representation with embedded Mermaid source. |
| Diagrams | `.md` + `.mmd` / `.svg` | Visual diagrams with source. |
| Handbook | `.md` | Human-facing synthesized guide. |
| Checklist | `.md` | Validation instrument. |
| Suite | `.md` (index) + linked files | Composite deliverable. |
| Report | `.md` | QA or review report. |

### 3.2 Artifact Naming Convention

All artifacts follow:

```
<engagement_id>_<artifact_id>_<slug>.<ext>
```

Example: `e2342c10_ART03_folder-tree.md`. The slug is lowercase-kebab and derived from the artifact's title.

### 3.3 Artifact Location

All artifacts are written under `<output_root>/artifacts/`. Subdirectories mirror Phase numbers (`phase1/`, `phase2/`, `phase3/`, `phase4/`). The orchestrator MUST NOT write outside this tree.

### 3.4 Completion Record

Each prompt execution emits a Completion Record per `MASTER_PROMPT.md` § 6. The record is itself an artifact of type Report, named `<engagement_id>_<prompt_id>_completion.md`, stored in `<output_root>/completion/`.

---

## 4. Data Model

### 4.1 Core Entities

The framework reasons over a fixed ontology of entities. Every artifact references these entities by stable ID.

| Entity | ID Prefix | Description |
|--------|-----------|-------------|
| File | `F-` | A source file in the repository. |
| Folder | `D-` | A directory. |
| Module | `M-` | A logical module (may span folders). |
| Component | `C-` | A UI or service component. |
| Class | `K-` | A class or struct. |
| Interface | `I-` | An interface, protocol, or contract. |
| Function | `FN-` | A function or method. |
| Variable | `V-` | A module-level or significant variable. |
| API | `A-` | An endpoint or public API. |
| Event | `E-` | A discrete event. |
| State | `S-` | A state in a state machine. |
| Workflow | `W-` | A named workflow or pipeline. |
| Pattern | `P-` | A design pattern instance. |
| Dependency | `DEP-` | An external dependency. |
| Config | `CFG-` | A configuration item. |
| Prompt | `PR-` | An AI prompt template. |
| Tool | `T-` | An AI tool integration. |

### 4.2 Relationship Types

Entities are connected by typed relationships. The framework recognizes the following relationship kinds, each with an inverse.

| Relationship | Inverse | Semantics |
|--------------|---------|-----------|
| `CONTAINS` | `CONTAINED_IN` | Folder contains file; module contains file. |
| `CALLS` | `CALLED_BY` | Function calls function. |
| `IMPLEMENTS` | `IMPLEMENTED_BY` | Class implements interface. |
| `EXTENDS` | `EXTENDED_BY` | Class extends class. |
| `IMPORTS` | `IMPORTED_BY` | Module imports module. |
| `DEPENDS_ON` | `DEPENDENCY_OF` | Module depends on module. |
| `EMITS` | `EMITTED_BY` | Function emits event. |
| `HANDLES` | `HANDLED_BY` | Function handles event. |
| `TRANSITIONS_TO` | `TRANSITION_FROM` | State transitions to state. |
| `INVOKES` | `INVOKED_BY` | Workflow invokes tool. |
| `READS` / `WRITES` | — | Function reads/writes variable or store. |
| `PART_OF` | `HAS_PART` | Entity is part of a larger entity. |

### 4.3 Artifact Schemas

Each artifact type has a JSON-equivalent schema (expressed as Markdown front-matter). Schemas are normative and validated by PROMPT_28. The schema registry lives in `QUALITY_STANDARDS.md` § Artifact Schemas.

---

## 5. Lifecycle

An engagement progresses through a fixed lifecycle. Each stage has entry and exit conditions.

### 5.1 Stage 0 — Engagement Initiation
- **Entry:** Orchestrator provides all Engagement Inputs (§2.1).
- **Activities:** Framework integrity check (`MASTER_INDEX.md` § 10), authorization validation, budget reservation, artifact directory creation.
- **Exit:** Engagement manifest written; agent booted via `MASTER_PROMPT.md`.

### 5.2 Stage 1 — Phase 1 Execution
- **Entry:** Engagement manifest exists.
- **Activities:** Dispatch PROMPT_01 → PROMPT_10 per dispatch protocol.
- **Exit:** All Phase 1 artifacts registered and quality-checked.

### 5.3 Stage 2 — Phase 2 Execution
- **Entry:** Phase 1 exit conditions met.
- **Activities:** Dispatch PROMPT_11 → PROMPT_20 (skipping optional prompts per scope).
- **Exit:** All Phase 2 artifacts registered and quality-checked.

### 5.4 Stage 3 — Phase 3 Execution
- **Entry:** Phase 2 exit conditions met.
- **Activities:** Dispatch PROMPT_21 → PROMPT_25.
- **Exit:** All Phase 3 artifacts registered and quality-checked.

### 5.5 Stage 4 — Phase 4 Execution
- **Entry:** Phase 3 exit conditions met.
- **Activities:** Dispatch PROMPT_26 → PROMPT_30.
- **Exit:** PROMPT_30 QA report emitted with zero blocking findings.

### 5.6 Stage 5 — Termination
- **Entry:** Stage 4 exit conditions met (normal) OR a termination condition fires (`MISSION.md` § 8).
- **Activities:** Seal artifacts, write engagement summary, archive completion records.
- **Exit:** Engagement closed. Artifact tree is immutable.

---

## 6. Contracts

### 6.1 Prompt Contract

Every `PROMPT_XX.md` MUST declare:
1. **Objective** — one sentence.
2. **When to Invoke** — entry conditions.
3. **Required Inputs** — artifact IDs.
4. **Instructions to AI Agent** — ordered steps.
5. **Required Outputs** — artifact IDs with types.
6. **Quality Checks** — verifiable predicates.
7. **Handoff Criteria** — exit conditions.

A prompt lacking any of these is non-conformant and MUST be amended before use.

### 6.2 Artifact Contract

Every artifact MUST contain:
1. **Header block** — engagement ID, artifact ID, type, producing prompt, timestamp.
2. **Source coverage statement** — which files/symbols were analyzed.
3. **Body** — the substantive content.
4. **Traceability index** — mapping of claims to source locations.
5. **Open questions** — unresolved items, if any.

### 6.3 Traceability Contract

Every factual claim in an artifact body MUST be backed by an entry in the artifact's Traceability Index of the form:

```
[claim_id]: <file_path>:<start_line>-<end_line> (symbol: <symbol_name>)
```

Claims not backed by such an entry are non-conformant. PROMPT_28 enforces this contract.

### 6.4 Non-Contradiction Contract

No two artifacts may assert contradictory facts about the same entity. Where the subject system is itself contradictory, both artifacts MUST cite the contradiction explicitly and mark it `SYSTEM_INCONSISTENCY`. PROMPT_30 detects violations.

---

## 7. Quality Dimensions

Quality is measured along eight dimensions, each scored 0–5 by `QUALITY_STANDARDS.md`. An artifact is conformant only if every dimension scores ≥ 3 and the aggregate ≥ 28.

1. **Coverage** — fraction of in-scope entities represented.
2. **Traceability** — fraction of claims with valid source citations.
3. **Accuracy** — fraction of claims verifiable against source.
4. **Depth** — presence of mechanism, not just description.
5. **Coherence** — internal consistency and cross-artifact consistency.
6. **Precision** — specificity of identifiers and line ranges.
7. **Completeness** — presence of all required sections.
8. **Readability** — clarity, structure, navigability for the End Consumer.

---

## 8. Concurrency & Parallelism

Prompts within a phase MAY be dispatched in parallel batches when:
- They have no inter-dependency in `MASTER_INDEX.md` § 6.
- Their inputs are all available.
- The orchestrator explicitly authorizes the batch.

The agent MUST NOT parallelize across phases. The agent MUST NOT parallelize a prompt against itself (no concurrent executions of the same prompt ID).

When parallel prompts write to the same artifact (rare, only for append-only manifests), the orchestrator MUST serialize writes through a merge step.

---

## 9. Error Model

### 9.1 Error Categories

| Category | Code | Meaning | Recovery |
|----------|------|---------|----------|
| Input Gap | `INPUT_GAP` | A declared input is missing or empty. | Re-run producing prompt. |
| Tool Failure | `TOOL_FAIL` | A required tool invocation failed. | Retry, then escalate. |
| Token Exhaustion | `TOKEN_OUT` | Token budget exhausted mid-prompt. | Resume from checkpoint. |
| Contradiction | `CONTRADICTION` | Two artifacts conflict. | Reconcile in PROMPT_30; may invalidate one. |
| Coverage Gap | `COVERAGE_GAP` | An in-scope entity is unanalyzable. | Mark `UNVERIFIED`; record in PROMPT_28. |
| Authorization | `AUTH_FAIL` | Authorization invalid or lapsed. | Halt; controlled termination. |
| Integrity | `INTEGRITY_FAIL` | Repository mutated or framework corrupted. | Integrity termination. |

### 9.2 Error Propagation

Errors propagate as `BLOCKED` completion records. The orchestrator decides resolution. The agent never silently absorbs an error.

---

## 10. Extension Points

The framework defines three extension points for adaptation without forking:

### 10.1 Domain Plugins
A domain plugin is a set of additional prompts (PROMPT_31+) that analyze domain-specific concerns (e.g., smart-contract safety, embedded-system memory layout). Plugins MUST be registered in `MASTER_INDEX.md` and conform to the Prompt Contract (§6.1).

### 10.2 Output Adapters
An output adapter transforms the canonical Markdown artifacts into other formats (Word, PDF, Confluence, Notion). Adapters MUST preserve the Traceability Contract (§6.3). Adapters are invoked by PROMPT_29 as a post-assembly step.

### 10.3 Reviewer Hooks
A reviewer hook is an automated check executed by PROMPT_30 in addition to the standard QA checks. Hooks are registered in `QUALITY_STANDARDS.md` and MUST be deterministic.

---

## 11. Conformance

An implementation claims EREPF conformance if and only if:

- **C1.** It loads all files in `MASTER_INDEX.md` § 2 as read-only context.
- **C2.** It dispatches prompts in the order defined by `MASTER_INDEX.md` § 4.
- **C3.** It enforces the Prompt Contract (§6.1) for every prompt.
- **C4.** It enforces the Artifact Contract (§6.2) and Traceability Contract (§6.3) for every artifact.
- **C5.** It enforces the Non-Contradiction Contract (§6.4) via PROMPT_30.
- **C6.** It supports all four scope modifiers (`MISSION.md` § 6).
- **C7.** It emits a Completion Record per `MASTER_PROMPT.md` § 6 for every prompt execution.
- **C8.** It terminates engagements only per `MISSION.md` § 8.
- **C9.** It does not modify the subject repository.
- **C10.** It supports all error categories in §9.1.

Partial conformance (e.g., skipping optional prompts under `SCOPE_CORE`) is permitted and remains conformant. Non-conformance in any of C1–C10 invalidates the conformance claim.

---

## 12. Glossary

- **Engagement** — A single execution of the framework against one subject repository under one scope.
- **Artifact** — A durable output of a prompt, conforming to §3.
- **Manifest** — An inventory artifact.
- **Map** — A structural/topological artifact.
- **Graph** — A node-edge artifact (call graph, data flow).
- **Handbook** — A synthesized human-facing artifact.
- **Checklist** — A validation artifact.
- **Phase** — A top-level execution grouping (1–4).
- **Stage** — A single prompt execution within a phase.
- **Subject** — The repository being reverse-engineered.
- **Orchestrator** — The party that dispatches prompts and owns budgets.
- **Agent** — The AI executor booted by `MASTER_PROMPT.md`.
- **Reviewer** — The party that validates artifacts against `QUALITY_STANDARDS.md`.
- **End Consumer** — The engineer who uses the documentation.

---

## 13. References

- `MASTER_INDEX.md` — File registry and reading order.
- `MASTER_PROMPT.md` — Runtime bootstrap.
- `MISSION.md` — Mission, success criteria, anti-goals.
- `OPERATING_RULES.md` — Hard rules.
- `QUALITY_STANDARDS.md` — Quality bars and scoring.
- `OUTPUT_RULES.md` — Format and delivery rules.
- `PROMPT_DESIGN_GUIDE.md` — Prompt authoring rules.

---

*End of Project Specification. Proceed to `PROMPT_DESIGN_GUIDE.md`.*
