# MASTER_INDEX.md
## Enterprise Reverse Engineering Prompt Framework — Master Index

> **Document Type:** Master Index / Navigation Hub
> **Framework Version:** 1.0.0
> **Status:** Production-Ready
> **Maintainer:** Prompt Architecture Office (PAO)
> **Last Reviewed:** 2026-07-24

---

## 1. Purpose of This Index

This file is the canonical navigation hub for the **Enterprise Reverse Engineering Prompt Framework** (EREPF). It enumerates every file in the prompt project, defines the reading order, declares cross-file dependencies, and provides a single source of truth for which prompt to invoke at which stage of a reverse-engineering engagement.

Any AI agent, human reviewer, or automation pipeline that consumes this framework MUST begin by reading this index in full. Skipping the index risks executing prompts out of order, missing required inputs, or producing documentation that violates the framework's quality contracts. The index is intentionally exhaustive: it lists every file, every phase, every artifact, and every cross-reference so that downstream consumers never need to discover structure by exploration.

The index is also the version-control anchor. When the framework evolves, this file is updated first, and all other files derive their canonical numbering, naming, and dependency declarations from it. Treat any file not listed here as out-of-scope or deprecated.

---

## 2. Project Inventory

The framework is organized into four tiers: **Governance**, **Specification**, **Operational Rules**, and **Execution Prompts**. Each tier has a distinct responsibility and a distinct consumer.

### 2.1 Tier A — Governance & Strategy

| File | Role | Primary Consumer |
|------|------|------------------|
| `MASTER_INDEX.md` | Navigation hub and canonical file registry | All agents, humans, automation |
| `MASTER_PROMPT.md` | The system-level prompt that boots the agent and loads the framework | Orchestrator / runtime |
| `MISSION.md` | Statement of mission, success criteria, and ethical boundaries | All agents |
| `PROJECT_SPECIFICATION.md` | Formal specification of the framework's scope, inputs, outputs, and contracts | Architects, reviewers |

### 2.2 Tier B — Design & Methodology

| File | Role | Primary Consumer |
|------|------|------------------|
| `PROMPT_DESIGN_GUIDE.md` | Authoring rules for new prompts; composition, token budgeting, modularity | Prompt authors |
| `OPERATING_RULES.md` | Hard rules the agent must obey during execution (governance layer) | All agents |
| `QUALITY_STANDARDS.md` | Quality bars, scoring rubrics, validation gates | Reviewers, self-check |
| `OUTPUT_RULES.md` | Format, structure, naming, and delivery rules for all artifacts | All agents |

### 2.3 Tier C — Execution Prompts (Phase 1: Intake & Cartography)

| File | Prompt ID | Title | Phase |
|------|-----------|-------|-------|
| `PROMPT_01.md` | PROMPT_01 | Repository Intake & Boundary Definition | 1 |
| `PROMPT_02.md` | PROMPT_02 | Technology Stack & Dependency Analysis | 1 |
| `PROMPT_03.md` | PROMPT_03 | Folder & File System Cartography | 1 |
| `PROMPT_04.md` | PROMPT_04 | Build System & Configuration Analysis | 1 |
| `PROMPT_05.md` | PROMPT_05 | Entry Points & Bootstrap Analysis | 1 |
| `PROMPT_06.md` | PROMPT_06 | Module Architecture Extraction | 1 |
| `PROMPT_07.md` | PROMPT_07 | Component Architecture Analysis | 1 |
| `PROMPT_08.md` | PROMPT_08 | Class & Interface Documentation | 1 |
| `PROMPT_09.md` | PROMPT_09 | Function-Level Reverse Engineering | 1 |
| `PROMPT_10.md` | PROMPT_10 | Call Graph & Dependency Graph Construction | 1 |

### 2.4 Tier C — Execution Prompts (Phase 2: Dynamics & Behavior)

| File | Prompt ID | Title | Phase |
|------|-----------|-------|-------|
| `PROMPT_11.md` | PROMPT_11 | Data Flow Analysis | 2 |
| `PROMPT_12.md` | PROMPT_12 | Control Flow & Execution Path Analysis | 2 |
| `PROMPT_13.md` | PROMPT_13 | State Management Analysis | 2 |
| `PROMPT_14.md` | PROMPT_14 | Event Workflow Analysis | 2 |
| `PROMPT_15.md` | PROMPT_15 | API & Interface Documentation | 2 |
| `PROMPT_16.md` | PROMPT_16 | Middleware & Pipeline Analysis | 2 |
| `PROMPT_17.md` | PROMPT_17 | Error Handling & Resilience Analysis | 2 |
| `PROMPT_18.md` | PROMPT_18 | Caching & Performance Strategy | 2 |
| `PROMPT_19.md` | PROMPT_19 | Authentication & Authorization Analysis (Optional) | 2 |
| `PROMPT_20.md` | PROMPT_20 | Database & Persistence Layer Analysis (Optional) | 2 |

### 2.5 Tier C — Execution Prompts (Phase 3: Intelligence & Patterns)

| File | Prompt ID | Title | Phase |
|------|-----------|-------|-------|
| `PROMPT_21.md` | PROMPT_21 | AI / LLM Workflow Analysis | 3 |
| `PROMPT_22.md` | PROMPT_22 | Streaming Workflow Analysis | 3 |
| `PROMPT_23.md` | PROMPT_23 | Design Pattern Identification | 3 |
| `PROMPT_24.md` | PROMPT_24 | Engineering Decisions & Trade-off Reconstruction | 3 |
| `PROMPT_25.md` | PROMPT_25 | Diagram Generation (Mermaid, UML, Sequence, Flowchart) | 3 |

### 2.6 Tier C — Execution Prompts (Phase 4: Synthesis & Delivery)

| File | Prompt ID | Title | Phase |
|------|-----------|-------|-------|
| `PROMPT_26.md` | PROMPT_26 | Rebuild Guide & Architecture Handbook | 4 |
| `PROMPT_27.md` | PROMPT_27 | Developer Handbook | 4 |
| `PROMPT_28.md` | PROMPT_28 | Cross-Reference & Validation Checklists | 4 |
| `PROMPT_29.md` | PROMPT_29 | Final Documentation Assembly | 4 |
| `PROMPT_30.md` | PROMPT_30 | Self-Review & Quality Assurance | 4 |

---

## 3. Canonical Reading Order

A new agent joining an engagement MUST read files in this order. Deviation from this order is permitted only when explicitly allowed by `OPERATING_RULES.md`.

1. `MASTER_INDEX.md` (this file)
2. `MASTER_PROMPT.md`
3. `MISSION.md`
4. `PROJECT_SPECIFICATION.md`
5. `OPERATING_RULES.md`
6. `QUALITY_STANDARDS.md`
7. `OUTPUT_RULES.md`
8. `PROMPT_DESIGN_GUIDE.md`
9. `PROMPT_01.md` → `PROMPT_30.md` (in numeric order, executed as scheduled by the orchestrator)

---

## 4. Execution Pipeline (High-Level)

The framework executes in four phases. Each phase produces artifacts that the next phase consumes. Prompts within a phase MAY run in parallel where independence permits; cross-phase execution is strictly sequential.

```
Phase 1: Intake & Cartography  ──►  Phase 2: Dynamics & Behavior
        (PROMPT_01 → 10)                    (PROMPT_11 → 20)
                │                                   │
                ▼                                   ▼
        Phase 4: Synthesis & Delivery  ◄──  Phase 3: Intelligence & Patterns
                (PROMPT_26 → 30)                  (PROMPT_21 → 25)
```

### 4.1 Phase 1 — Intake & Cartography
Establish the physical and structural map of the repository: boundaries, stack, folders, build, entry points, modules, components, classes, functions, and graphs. Output: a static blueprint of the system.

### 4.2 Phase 2 — Dynamics & Behavior
Animate the static blueprint by tracing data flow, control flow, state transitions, events, APIs, middleware, error handling, caching, auth, and persistence. Output: a behavioral model of the system.

### 4.3 Phase 3 — Intelligence & Patterns
Identify AI/LLM workflows, streaming pipelines, design patterns, engineering trade-offs, and synthesize visual diagrams. Output: a cognitive and architectural interpretation of the system.

### 4.4 Phase 4 — Synthesis & Delivery
Assemble the rebuild guide, developer handbook, cross-references, final documentation, and perform self-review. Output: the deliverable documentation suite.

---

## 5. Artifact Registry

All artifacts produced by the framework are registered here. The orchestrator MUST maintain this registry as prompts emit artifacts.

| Artifact ID | Producing Prompt | Artifact Type | Description |
|-------------|------------------|---------------|-------------|
| ART-01 | PROMPT_01 | Manifest | Repository boundary declaration |
| ART-02 | PROMPT_02 | Manifest | Tech stack & dependency inventory |
| ART-03 | PROMPT_03 | Map | Folder & file tree with responsibilities |
| ART-04 | PROMPT_04 | Spec | Build & configuration map |
| ART-05 | PROMPT_05 | Map | Entry point & bootstrap trace |
| ART-06 | PROMPT_06 | Map | Module map |
| ART-07 | PROMPT_07 | Map | Component map |
| ART-08 | PROMPT_08 | Doc | Class & interface reference |
| ART-09 | PROMPT_09 | Doc | Function reference |
| ART-10 | PROMPT_10 | Graph | Call & dependency graphs |
| ART-11 | PROMPT_11 | Graph | Data flow diagrams |
| ART-12 | PROMPT_12 | Graph | Control flow & execution path maps |
| ART-13 | PROMPT_13 | Doc | State machine catalog |
| ART-14 | PROMPT_14 | Doc | Event workflow catalog |
| ART-15 | PROMPT_15 | Doc | API & interface reference |
| ART-16 | PROMPT_16 | Doc | Middleware & pipeline map |
| ART-17 | PROMPT_17 | Doc | Error handling & resilience report |
| ART-18 | PROMPT_18 | Doc | Caching & performance report |
| ART-19 | PROMPT_19 | Doc | Auth & authorization report (optional) |
| ART-20 | PROMPT_20 | Doc | Database & persistence report (optional) |
| ART-21 | PROMPT_21 | Doc | AI/LLM workflow report |
| ART-22 | PROMPT_22 | Doc | Streaming workflow report |
| ART-23 | PROMPT_23 | Doc | Design pattern catalog |
| ART-24 | PROMPT_24 | Doc | Engineering decision record |
| ART-25 | PROMPT_25 | Diagrams | Mermaid / UML / sequence / flowchart set |
| ART-26 | PROMPT_26 | Handbook | Rebuild guide & architecture handbook |
| ART-27 | PROMPT_27 | Handbook | Developer handbook |
| ART-28 | PROMPT_28 | Checklist | Cross-reference & validation checklists |
| ART-29 | PROMPT_29 | Suite | Final documentation assembly |
| ART-30 | PROMPT_30 | Report | Self-review & QA report |

---

## 6. Cross-Reference Map

The framework is heavily cross-referenced. The following matrix shows which prompts feed which. A "→" indicates that the source prompt's output is a required input for the target prompt.

| Source | Targets |
|--------|---------|
| PROMPT_01 | PROMPT_02, 03, 04, 05, 06 |
| PROMPT_02 | PROMPT_04, 23, 24, 26 |
| PROMPT_03 | PROMPT_06, 07, 08, 09, 28 |
| PROMPT_04 | PROMPT_05, 26 |
| PROMPT_05 | PROMPT_12, 16 |
| PROMPT_06 | PROMPT_07, 10, 11 |
| PROMPT_07 | PROMPT_08, 10, 25 |
| PROMPT_08 | PROMPT_09, 10, 23 |
| PROMPT_09 | PROMPT_10, 12, 28 |
| PROMPT_10 | PROMPT_11, 12, 25, 28 |
| PROMPT_11 | PROMPT_13, 21, 25 |
| PROMPT_12 | PROMPT_13, 14, 17 |
| PROMPT_13 | PROMPT_14, 17, 25 |
| PROMPT_14 | PROMPT_16, 22 |
| PROMPT_15 | PROMPT_16, 28 |
| PROMPT_16 | PROMPT_17, 22 |
| PROMPT_17 | PROMPT_24, 26 |
| PROMPT_18 | PROMPT_24, 26 |
| PROMPT_19 | PROMPT_15, 26 |
| PROMPT_20 | PROMPT_11, 13, 26 |
| PROMPT_21 | PROMPT_22, 24, 25 |
| PROMPT_22 | PROMPT_24 |
| PROMPT_23 | PROMPT_24, 26 |
| PROMPT_24 | PROMPT_26, 27 |
| PROMPT_25 | PROMPT_26, 27, 29 |
| PROMPT_26 | PROMPT_29 |
| PROMPT_27 | PROMPT_29 |
| PROMPT_28 | PROMPT_30 |
| PROMPT_29 | PROMPT_30 |
| PROMPT_30 | (terminal) |

---

## 7. Versioning & Extension Policy

The framework follows semantic versioning (`MAJOR.MINOR.PATCH`).

- **MAJOR**: changes that break the contract of any existing prompt (renumbering, removal, altered inputs/outputs).
- **MINOR**: additive changes (new prompts, new optional sections, new artifacts).
- **PATCH**: corrections that do not alter contracts (typos, clarifications, expanded examples).

When extending the framework with new prompts, the author MUST:
1. Reserve the next numeric PROMPT ID.
2. Register the new file in this index (Sections 2 and 5).
3. Declare dependencies in Section 6.
4. Author the file using `PROMPT_DESIGN_GUIDE.md`.
5. Validate the file against `QUALITY_STANDARDS.md`.
6. Append a worklog entry per `OPERATING_RULES.md`.

---

## 8. Terminology Anchor

To prevent drift, the following terms are canonically defined here and used identically across all files. Full definitions live in `PROJECT_SPECIFICATION.md` § Glossary.

- **Repository (Subject)** — the software system being reverse-engineered.
- **Agent** — the AI coding agent executing the framework.
- **Artifact** — any durable output produced by a prompt.
- **Manifest** — a structured inventory file.
- **Map** — a structural or topological representation.
- **Graph** — a node-edge representation (call graph, data flow, etc.).
- **Handbook** — a human-facing explanatory document.
- **Checklist** — a validation instrument.
- **Phase** — a top-level execution grouping (1–4).
- **Stage** — a single prompt execution within a phase.

---

## 9. Quick-Start for Orchestrators

An orchestrator (human or runtime) bootstrapping an engagement SHOULD:

1. Copy the entire framework directory into the agent's working context.
2. Load `MASTER_PROMPT.md` as the system prompt.
3. Provide the subject repository path or archive.
4. Invoke PROMPT_01.
5. On each prompt's completion, validate against its `Quality Checks` section and `QUALITY_STANDARDS.md`.
6. Advance to the next prompt only when handoff criteria are satisfied.
7. Terminate at PROMPT_30, whose output is the final QA report.

---

## 10. File Integrity Checklist

Before any engagement, verify that every file below is present and non-empty. Missing files invalidate the engagement.

- [ ] `MASTER_INDEX.md`
- [ ] `MASTER_PROMPT.md`
- [ ] `MISSION.md`
- [ ] `PROJECT_SPECIFICATION.md`
- [ ] `PROMPT_DESIGN_GUIDE.md`
- [ ] `OPERATING_RULES.md`
- [ ] `QUALITY_STANDARDS.md`
- [ ] `OUTPUT_RULES.md`
- [ ] `PROMPT_01.md` through `PROMPT_30.md`

---

*End of Master Index. Proceed to `MASTER_PROMPT.md`.*
