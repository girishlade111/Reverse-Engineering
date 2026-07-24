# MISSION.md
## Enterprise Reverse Engineering Prompt Framework — Mission Statement

> **Document Type:** Mission Charter
> **Framework Version:** 1.0.0
> **Authority:** Highest. All other files must be consistent with this mission.

---

## 1. Mission Statement

The mission of the Enterprise Reverse Engineering Prompt Framework (EREPF) is to enable an AI coding agent to **completely reverse engineer any software repository** and produce **audit-grade documentation** that empowers a competent engineer to rebuild, operate, extend, and reason about the system as if they had authored it.

"Complete" is not a marketing term. It is a contractual bar: every architectural element, every execution path, every workflow, every algorithm, every internal relationship, every design pattern, every engineering decision, every dependency, every module, every file, every folder, every class, every function, every interface, every API, every service, every utility, every helper, every middleware, every state transition, every event, every prompt, every AI workflow, every reasoning process, every planning pipeline, every tool integration, every data flow, every execution pipeline, and every software boundary MUST be understood by the agent BEFORE documentation is written, and MUST be represented in the final deliverable.

The framework exists because partial reverse engineering is the dominant failure mode of AI-assisted code comprehension. Agents that document what they superficially see, rather than what the system actually does, produce documentation that is worse than none: it misleads future maintainers with confident falsehoods. This framework enforces a discipline of understanding-first, evidence-bound, traceable reconstruction.

---

## 2. Strategic Objectives

The mission decomposes into five strategic objectives, each of which is enforced by specific prompts and quality gates.

### 2.1 Objective A — Total Structural Comprehension
The agent must construct a complete mental and machine-readable model of the repository's structure: its folder topology, file responsibilities, module boundaries, component graph, class hierarchy, and function inventory. No file in scope may remain unanalyzed. Objective A is the responsibility of Phase 1 (PROMPT_01–10).

### 2.2 Objective B — Behavioral Comprehension
The agent must reconstruct how the system behaves at runtime: data flows, control flows, state transitions, event sequences, API contracts, middleware chains, error paths, caching strategies, authentication flows, and persistence interactions. Objective B is the responsibility of Phase 2 (PROMPT_11–20).

### 2.3 Objective C — Cognitive & Architectural Comprehension
The agent must identify the system's intelligence layer — AI/LLM workflows, agent architectures, planning and memory pipelines, RAG, tool calling, streaming — and recognize the design patterns and engineering trade-offs that shape the system. Objective C is the responsibility of Phase 3 (PROMPT_21–25).

### 2.4 Objective D — Reconstructive Documentation
The agent must synthesize its comprehension into documentation that would permit reconstruction: a rebuild guide, an architecture handbook, a developer handbook, cross-references, and validation checklists. Objective D is the responsibility of Phase 4 (PROMPT_26–29).

### 2.5 Objective E — Self-Verification
The agent must verify its own work against quality standards and traceability requirements before declaring the engagement complete. Objective E is the responsibility of PROMPT_30 and the cross-cutting `QUALITY_STANDARDS.md`.

---

## 3. Success Criteria

An engagement is successful if and only if ALL of the following hold:

1. **Coverage.** Every in-scope file is represented in at least one artifact with a documented responsibility. A file with "no findings" is explicitly recorded as analyzed-and-empty.
2. **Traceability.** Every factual claim in every artifact cites a source location (file path + line range or symbol).
3. **Coherence.** No two artifacts contradict each other. Where the system itself is internally inconsistent, the inconsistency is documented, not smoothed.
4. **Reconstructability.** A competent engineer, given only the produced documentation, could rebuild a behaviorally equivalent system. This is validated by the Rebuild Guide (PROMPT_26) and the Cross-Reference Checklists (PROMPT_28).
5. **Quality Conformance.** Every artifact passes the quality checks defined in its producing prompt and in `QUALITY_STANDARDS.md`.
6. **QA Clearance.** PROMPT_30 emits a QA report with zero unresolved blocking findings.

An engagement that satisfies criteria 1–5 but not 6 is **not complete**. An engagement that satisfies 6 but not 1–5 is **impossible by construction** (PROMPT_30 will detect the gaps).

---

## 4. Anti-Goals (What This Framework Does NOT Do)

Stating anti-goals prevents scope creep and clarifies what a consumer should not expect.

- **The framework does not modify the subject repository.** All operations are read-only with respect to the source.
- **The framework does not execute the subject software.** Static and symbolic analysis only; no runtime invocation except where the orchestrator explicitly authorizes sandboxed execution for dynamic confirmation.
- **The framework does not produce marketing, end-user, or business documentation.** Its outputs are engineering documentation.
- **The framework does not infer intent beyond evidence.** Where the original author's intent is not recoverable from code, the framework documents the behavior, not speculation about intent.
- **The framework does not optimize the subject code.** Performance notes are descriptive, not prescriptive, unless the orchestrator requests prescriptive recommendations as a separate engagement.

---

## 5. Ethical & Legal Boundaries

The framework is designed for legitimate reverse engineering: internal codebases, open-source repositories, code the consumer has rights to analyze, and security research within applicable law. The framework MUST NOT be used to:

- Reverse engineer software in violation of its license, terms of service, or applicable law.
- Extract or repurpose proprietary algorithms for unauthorized competitive use.
- Produce documentation that misrepresents the system to deceive auditors, regulators, or customers.
- Bypass technical protection measures (DRM, obfuscation) where prohibited.

The orchestrator is responsible for legal authorization. The agent, upon detecting evidence that the subject repository is protected by license terms prohibiting reverse engineering, MUST halt and emit a `BLOCKED` completion record citing the conflict, escalating to the orchestrator for a written authorization override.

---

## 6. Scope Modifiers

Engagements vary. The framework supports explicit scope modifiers declared at engagement initiation:

- **`SCOPE_FULL`** — all prompts execute, including optional prompts (PROMPT_19, PROMPT_20).
- **`SCOPE_CORE`** — optional prompts are skipped unless their triggers fire (e.g., PROMPT_19 runs if auth code is detected).
- **`SCOPE_TRIAGE`** — only Phase 1 + PROMPT_29 (assembly of the static blueprint). Suitable for first-pass triage of large repositories.
- **`SCOPE_MODULE(target)`** — restricts analysis to a named module and its closure of dependencies. Useful for incremental analysis.

The scope modifier is recorded in the engagement manifest produced by PROMPT_01 and governs dispatch for the entire engagement.

---

## 7. Stakeholders

- **Orchestrator** — owns dispatch, budgets, and authorization. The framework's consumer.
- **Agent (EREPF-Agent)** — executes prompts and produces artifacts.
- **Reviewer** — a human or automated system that validates artifacts against `QUALITY_STANDARDS.md`.
- **End Consumer** — the engineer who will use the documentation to rebuild, operate, or extend the system.

The framework optimizes for the End Consumer's needs. The Reviewer's bar is the gate. The Orchestrator's authority is bounded by the framework's contracts; the orchestrator may not waive traceability or quality requirements.

---

## 8. Mission Termination Conditions

The engagement terminates under one of three conditions:

1. **Normal Completion** — PROMPT_30 emits a QA report with zero blocking findings. All artifacts are registered and conformant.
2. **Controlled Termination** — the orchestrator declares termination after a `BLOCKED` record that cannot be resolved. All artifacts produced to date are sealed with a `TERMINATED` marker and a termination rationale.
3. **Integrity Termination** — the agent detects repository mutation, authorization lapse, or framework corruption. All artifacts are sealed as `INVALIDATED` and the engagement is restarted from PROMPT_01 under a new engagement ID.

There is no "soft exit." An engagement that stops mid-phase without one of these three terminations is in an undefined state and MUST be treated as `INVALIDATED`.

---

## 9. Mission Priority Order

When objectives conflict, the agent resolves conflicts in this priority order:

1. Traceability (never emit un-traced claims)
2. Accuracy (never emit unverifiable claims)
3. Completeness (cover everything in scope)
4. Coherence (no contradictions)
5. Readability (clarity for the End Consumer)
6. Brevity (only after the above are satisfied)

Brevity is the lowest priority. The framework explicitly rejects the optimization of output length at the expense of any higher priority.

---

## 10. Mission Commitment

By loading this framework, all parties — orchestrator, agent, reviewer — commit to the mission as stated. The mission is not a guideline; it is the contract under which the framework operates. Every prompt, every rule, every quality check exists to serve this mission. Any future amendment to the framework MUST be evaluated against whether it advances or retreats from this mission.

The mission is complete when the End Consumer can rebuild and reason about the system from the documentation alone.

---

*End of Mission Statement. Proceed to `PROJECT_SPECIFICATION.md`.*
