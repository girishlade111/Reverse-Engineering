# PROMPT_DESIGN_GUIDE.md
## Enterprise Reverse Engineering Prompt Framework — Prompt Design Guide

> **Document Type:** Authoring Standard
> **Framework Version:** 1.0.0
> **Audience:** Prompt authors, framework extenders
> **Authority:** Normative for all `PROMPT_XX.md` files.

---

## 1. Purpose of This Guide

This guide defines how to author, extend, and maintain the `PROMPT_XX.md` files that constitute the executable core of the framework. Every prompt file in the framework MUST conform to the structure, style, and contracts defined here. Prompts that deviate are non-conformant per `PROJECT_SPECIFICATION.md` § 11 (C3) and MUST be amended before use.

The guide exists because prompt quality is the dominant determinant of reverse-engineering quality. A well-designed prompt produces deterministic, traceable, complete outputs; a poorly designed prompt produces hallucinated, partial, contradictory outputs. The discipline encoded here is the difference between a framework that scales and a framework that drifts.

This guide is also the reference for the framework's extension mechanism. When a domain plugin (see `PROJECT_SPECIFICATION.md` § 10.1) adds prompts beyond PROMPT_30, those prompts MUST be authored per this guide. The guide therefore governs both the initial corpus and all future growth.

---

## 2. Canonical Prompt File Structure

Every `PROMPT_XX.md` MUST contain the following twelve sections, in this order, with these exact headings. Additional subsections are permitted within a section; reordering or omitting sections is not.

```
1. Prompt Metadata
2. Objective
3. When to Invoke
4. Required Inputs
5. Instructions to AI Agent
6. Analysis Procedures
7. Required Outputs
8. Output Templates
9. Quality Checks
10. Common Pitfalls
11. Handoff Criteria
12. Cross-References
```

The rationale for each section follows.

### 2.1 Section 1 — Prompt Metadata
A compact metadata block. Fields: Prompt ID, Phase, Stage number, Dependencies (artifact IDs or prompt IDs), Estimated Tokens, Output Artifacts (artifact IDs), Author, Last Revised. This block is machine-parsed by the orchestrator for dispatch planning.

### 2.2 Section 2 — Objective
A single declarative sentence stating what the prompt accomplishes. The objective MUST be testable: a reviewer reading only the objective and the Required Outputs should be able to judge whether the prompt succeeded. Vague objectives ("understand the system") are forbidden; precise objectives ("produce a call graph covering every function in scope with edge-level source citations") are required.

### 2.3 Section 3 — When to Invoke
Entry conditions expressed as predicates over the engagement state. Examples: "Phase 1 has exited" or "PROMPT_06 artifact ART-06 exists and has coverage ≥ 0.95". This section is the dispatch guard; the orchestrator MUST NOT dispatch the prompt unless all predicates are true.

### 2.4 Section 4 — Required Inputs
An enumerated list of artifact IDs (from `MASTER_INDEX.md` § 5) and framework files the prompt consumes. Each input MUST state the consuming purpose (e.g., "ART-03 — used to resolve file-to-module mapping"). A prompt may not consume artifacts not listed here.

### 2.5 Section 5 — Instructions to AI Agent
The ordered, imperative instructions the agent executes. Instructions are numbered, atomic, and verifiable. Each instruction begins with a verb (Analyze, Enumerate, Trace, Verify, Emit). Conditional branches are explicit (`IF <predicate> THEN <action> ELSE <action>`). No instruction may depend on implicit context.

### 2.6 Section 6 — Analysis Procedures
The detailed, repeatable procedures the agent follows. This is the technical heart of the prompt: the specific analyses, heuristics, and verification steps. Procedures are written so that a different agent (or a human) following them would produce the same outputs from the same inputs. Procedure steps cross-reference the entity ontology in `PROJECT_SPECIFICATION.md` § 4.

### 2.7 Section 7 — Required Outputs
An enumerated list of artifact IDs the prompt produces, each with type (per `PROJECT_SPECIFICATION.md` § 3.1) and acceptance criteria. Acceptance criteria are predicates the artifact MUST satisfy. An output whose acceptance criteria fail blocks handoff.

### 2.8 Section 8 — Output Templates
Templates the agent fills to produce structured outputs. Templates use the framework's front-matter schema (see `QUALITY_STANDARDS.md` § Artifact Schemas). Templates MUST be complete: every field the schema requires is present in the template. Free-form narrative is permitted only in fields the schema designates as narrative.

### 2.9 Section 9 — Quality Checks
Verifiable predicates the agent evaluates against its own outputs before emitting the Completion Record. Each check has an ID, a description, and a pass/fail criterion. Checks MUST be deterministic given the same inputs and outputs. Failed checks MUST be reported in the Completion Record; the agent MUST NOT silently retry to mask failures.

### 2.10 Section 10 — Common Pitfalls
A curated list of failure modes the prompt is known to be vulnerable to, each with a mitigation. This section captures hard-won knowledge from prior engagements and is updated whenever a new failure mode is observed. Pitfalls are written in the imperative ("Do not assume…", "Always verify…").

### 2.11 Section 11 — Handoff Criteria
Exit conditions the prompt's outputs MUST satisfy to permit dispatch of dependent prompts. Handoff criteria are stricter than quality checks: a prompt may pass its quality checks but still block handoff if a downstream-critical property is missing (e.g., "call graph covers ≥ 99% of in-scope functions" — quality checks verify the graph's integrity; handoff verifies its coverage).

### 2.12 Section 12 — Cross-References
Links to related prompts, rules, and standards. This section maintains the framework's navigability and is the human-readable mirror of the machine-readable Cross-Reference Map in `MASTER_INDEX.md` § 6.

---

## 3. Authoring Principles

### 3.1 Determinism Over Fluency
A prompt should produce the same output for the same input regardless of agent "mood." Prefer explicit procedures over open-ended invitations. "Enumerate every function whose name matches `/^handle/` and record its callers" is conformant; "look at the handlers and describe what they do" is not.

### 3.2 Source-Bound Claims
Every instruction that produces a factual claim MUST bind the claim to a source location. Instructions should embed the citation format (`<file_path>:<line_range>`) directly in their output templates so the agent cannot emit a claim without a citation slot.

### 3.3 Atomicity
Each instruction does one thing. Composite instructions ("analyze the module and write its documentation") are split into atomic steps ("enumerate the module's files"; "extract each file's exported symbols"; "build the internal dependency subgraph"; "emit the module documentation from the assembled model").

### 3.4 Verifiability
Every instruction's output must be checkable. If you cannot write a predicate that confirms the instruction was executed correctly, the instruction is too vague. Rewrite it until a predicate is expressible.

### 3.5 Modularity
A prompt should produce artifacts usable by multiple downstream prompts. Avoid prompt-local representations that only the current prompt understands; emit canonical entities (per `PROJECT_SPECIFICATION.md` § 4.1) and canonical relationship types (§ 4.2).

### 3.6 Idempotence
Re-running a prompt against the same inputs MUST produce equivalent outputs (same entity coverage, same relationship set, same claims modulo wording). Idempotence is verified by PROMPT_30 on a sampled basis.

### 3.7 Graceful Degradation
When an input is partial (e.g., a file is binary and unanalyzable), the prompt MUST degrade gracefully: mark the entity `UNVERIFIED`, continue with the analyzable remainder, and record the gap. Crashing the prompt is forbidden.

---

## 4. Token & Tool Budgeting

### 4.1 Token Budget per Prompt
Each prompt declares an Estimated Tokens figure in its metadata. The estimate covers prompt consumption, source reading, intermediate reasoning, and output emission. Estimates are calibrated against the largest in-scope repositories the framework targets.

If actual consumption exceeds 130% of the estimate, the prompt MUST checkpoint and resume rather than silently truncate output. Checkpoint format: write partial artifacts with a `PARTIAL` marker and a resume cursor.

### 4.2 Tool Budget per Prompt
Each prompt declares the tool categories it expects to use (read, grep, glob, ast-parse). The orchestrator reserves budgets per category. The agent MUST NOT exceed the reserved budget without an explicit re-reservation.

### 4.3 Batching
Where a prompt iterates over many entities (e.g., every function in scope), the agent SHOULD batch reads to amortize tool overhead. Batching is a performance optimization and MUST NOT alter the prompt's outputs.

---

## 5. Style & Tone

- **Imperative voice** for instructions and procedures ("Enumerate…", "Verify…", "Emit…").
- **Declarative voice** for outputs and metadata ("The module exports…", "The function calls…").
- **Third person** throughout. Never "you" or "I".
- **Specific identifiers** over generic nouns. Always prefer `dispatchRequest()` over "the dispatch function".
- **No hedging** in instructions. Avoid "consider", "may want to", "could". Use "MUST", "SHOULD", "MAY" per RFC 2119.
- **No filler**. Every sentence carries technical content.

---

## 6. Anti-Patterns (Forbidden)

The following prompt designs are forbidden because they correlate with degraded output quality:

1. **Open-ended prompts** — "Describe the architecture." Rewrite as a structured procedure with enumerated outputs.
2. **Implicit assumptions** — "Assume standard React conventions." Instead, verify conventions from source and proceed accordingly.
3. **Output before understanding** — any prompt that emits documentation before completing its analysis procedures.
4. **Untraceable claims** — any output template that lacks a citation slot.
5. **Hidden state** — any prompt that depends on agent memory not declared in § 4 Required Inputs.
6. **Best-effort silence** — any prompt that fails without emitting a Completion Record.
7. **Cross-phase shortcuts** — any prompt that produces artifacts belonging to a later phase.
8. **Format drift** — any prompt that introduces a schema not registered in `QUALITY_STANDARDS.md`.

---

## 7. Extension Workflow

When adding a new prompt (PROMPT_31+):

1. **Reserve the ID** in `MASTER_INDEX.md` § 2 and § 5.
2. **Draft the prompt** per § 2 of this guide.
3. **Declare dependencies** in `MASTER_INDEX.md` § 6.
4. **Register output artifacts** in `MASTER_INDEX.md` § 5 with types and acceptance criteria.
5. **Validate** the draft against § 3 Authoring Principles and § 6 Anti-Patterns.
6. **Pilot** the prompt on a representative repository; iterate until idempotent and deterministic.
7. **Version-bump** the framework per `MASTER_INDEX.md` § 7.
8. **Append a worklog entry** per `OPERATING_RULES.md`.

---

## 8. Maintenance

Prompts are living artifacts. Each prompt records its Last Revised date. The framework's quarterly review cycle re-validates every prompt against:

- Current entity ontology (`PROJECT_SPECIFICATION.md` § 4).
- Current quality standards (`QUALITY_STANDARDS.md`).
- Current output rules (`OUTPUT_RULES.md`).
- Field observations recorded in worklogs.

A prompt that fails re-validation is deprecated, not silently patched. Deprecation is a minor version bump; replacement is a major version bump.

---

## 9. Checklist for Prompt Authors

Before declaring a prompt complete, the author verifies:

- [ ] All twelve sections present, in order, with exact headings.
- [ ] Objective is a single testable sentence.
- [ ] When-to-Invoke predicates are deterministic.
- [ ] Required Inputs enumerate every consumed artifact.
- [ ] Instructions are atomic, imperative, numbered.
- [ ] Procedures are repeatable by an independent executor.
- [ ] Required Outputs have acceptance criteria.
- [ ] Output Templates conform to registered schemas.
- [ ] Quality Checks are deterministic predicates.
- [ ] Common Pitfalls capture known failure modes.
- [ ] Handoff Criteria are stricter than Quality Checks.
- [ ] Cross-References are complete and bidirectional.
- [ ] Estimated Tokens figure is calibrated.
- [ ] No anti-patterns from § 6 are present.

---

*End of Prompt Design Guide. Proceed to `OPERATING_RULES.md`.*
