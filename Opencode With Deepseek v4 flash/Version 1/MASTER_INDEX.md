# Enterprise Reverse Engineering Prompt Framework — MASTER INDEX

## FRAMEWORK ID: RE-PROMPT-FRAMEWORK-v1.0
## CLASSIFICATION: Enterprise Prompt Engineering Artifact
## PURPOSE: Guide AI agents to perform complete, systematic reverse engineering of any software repository

---

## FRAMEWORK OVERVIEW

This is a modular, extensible, reusable prompt framework designed to guide any AI coding agent through a complete multi-phase reverse engineering process. The framework covers every aspect of software understanding: structure, architecture, logic, data flow, business rules, AI workflows, dependencies, design decisions, and more.

The framework is organized into three layers:

### LAYER 1: CORE FRAMEWORK
Defines the mission, rules, quality standards, and output specifications.

| # | FILE | PURPOSE |
|---|------|---------|
| 1 | MASTER_INDEX.md | This file. Complete index and navigation map. |
| 2 | MASTER_PROMPT.md | The master orchestration prompt. Entry point for all reverse engineering sessions. |
| 3 | PROJECT_SPECIFICATION.md | Formal specification of what this prompt framework is and how it works. |
| 4 | PROMPT_DESIGN_GUIDE.md | Design philosophy, architecture decisions, and engineering rationale for the framework itself. |
| 5 | MISSION.md | The core mission statement that every AI agent must adopt. |
| 6 | OPERATING_RULES.md | Immutable rules that govern AI agent behavior during reverse engineering. |
| 7 | QUALITY_STANDARDS.md | Quality benchmarks that all output must meet. |
| 8 | OUTPUT_RULES.md | Strict formatting and content rules for all generated documentation. |

### LAYER 2: PHASE PROMPTS
Each PROMPT_N file is a self-contained phase of the reverse engineering process.

| # | FILE | PHASE | PURPOSE |
|---|------|-------|---------|
| 9 | PROMPT_01_SCOUTING.md | 00 — Project Scouting | Initial entry, repo cloning, language detection, high-level survey. |
| 10 | PROMPT_02_STRUCTURE.md | 01 — Structure Analysis | Full folder tree, file map, naming conventions, module boundaries. |
| 11 | PROMPT_03_BUILD_CONFIG.md | 02 — Build & Config Analysis | Build systems, package managers, configuration files, environment setup. |
| 12 | PROMPT_04_DEPENDENCIES.md | 03 — Dependency Graph | All dependencies, their roles, version analysis, dependency tree, license scan. |
| 13 | PROMPT_05_TECH_STACK.md | 04 — Tech Stack | Languages, frameworks, libraries, runtimes, platforms, toolchains. |
| 14 | PROMPT_06_MODULES.md | 05 — Module Analysis | Module responsibilities, public interfaces, internal APIs, module coupling. |
| 15 | PROMPT_07_DEEP_READ.md | 06 — Deep Code Reading | Systematic per-file analysis, class/function extraction, logic capture. |
| 16 | PROMPT_08_ARCHITECTURE.md | 07 — Architecture Reconstruction | System architecture, layers, patterns, architectural decisions. |
| 17 | PROMPT_09_DATA_FLOW.md | 08 — Data Flow Analysis | Data pipelines, transformations, state shapes, data boundaries. |
| 18 | PROMPT_10_CALL_GRAPH.md | 09 — Call Graph & Control Flow | Call graphs, execution paths, control flow, async chains, event loops. |
| 19 | PROMPT_11_FEATURES.md | 10 — Feature Mapping | Feature inventory, feature boundaries, feature interactions. |
| 20 | PROMPT_12_ALGORITHMS.md | 11 — Algorithm Extraction | Core algorithms, business logic, mathematical models, heuristics, search workflows. |
| 21 | PROMPT_13_DESIGN_PATTERNS.md | 12 — Design Patterns | Creational, structural, behavioral patterns; architectural styles. |
| 22 | PROMPT_14_API_BOUNDARIES.md | 13 — API & Service Boundaries | REST, GraphQL, gRPC, internal APIs, service contracts, middleware. |
| 23 | PROMPT_15_STATE_EVENTS.md | 14 — State & Events | State machines, event systems, event sourcing, state transitions. |
| 24 | PROMPT_16_ERROR_CACHE_RETRY.md | 15 — Error Handling & Reliability | Error strategies, retry logic, caching layers, resilience patterns. |
| 25 | PROMPT_17_AI_WORKFLOWS.md | 16 — AI Workflow Analysis | Prompts, agents, RAG, memory, planning, tool calling, LLM integration. |
| 26 | PROMPT_18_CONFIG_ENV.md | 17 — Configuration & Environment | Config management, environment variables, secrets, feature flags. |
| 27 | PROMPT_19_DOCUMENTATION.md | 18 — Documentation Generation | Final documentation assembly, diagram generation, handbook creation. |
| 28 | PROMPT_20_VALIDATION.md | 19 — Cross-Reference & Validation | Integrity checks, consistency validation, completeness audit. |
| 29 | PROMPT_21_STREAMING.md | 20 — Streaming & Reactive [Extended] | Data pipelines, reactive streams, WebSocket/SSE, AI streaming, backpressure. |
| 30 | PROMPT_22_AUTH_ARCHITECTURE.md | 21 — Authentication Architecture [Extended] | Auth providers, token lifecycle, sessions, RBAC, MFA, security assessment. |
| 31 | PROMPT_23_DEPLOYMENT.md | 22 — Deployment Architecture [Extended] | Infrastructure, CI/CD, environments, networking, observability, DR. |

### LAYER 3: SUPPORTING ARTIFACTS

| # | FILE | PURPOSE |
|---|------|---------|
| 32 | CHECKLIST.md | Complete reverse engineering checklist (250+ items). |
| 33 | TEMPLATES.md | Documentation templates for every output type (T1-T16). |
| 34 | DIAGRAM_GUIDE.md | Specifications for generating all diagram types (Mermaid + UML). |
| 35 | GLOSSARY.md | Domain vocabulary and terminology registry. |
| 36 | TROUBLESHOOTING.md | Diagnostics for resolving common failures during the RE process. |
| 37 | README.md | Framework entry point, overview, quick start guide. |
| 33 | TROUBLESHOOTING.md | Diagnostics for resolving common failures during the RE process. |

---

## HOW TO USE THIS FRAMEWORK

### For a Single Session (Small Repo)

1. Read MASTER_PROMPT.md to understand the complete workflow.
2. Read MISSION.md and OPERATING_RULES.md to internalize constraints.
3. Execute PROMPT_01 through PROMPT_20 sequentially.
4. Use CHECKLIST.md for quality validation at the end.

### For Multi-Session (Large Repo)

1. Read MASTER_PROMPT.md.
2. Execute PROMPT_01 (Scouting) to get a high-level map.
3. For each major module identified, run phase prompts 06–12 independently.
4. Assemble results in PROMPT_19 (Documentation Generation).
5. Validate with PROMPT_20 (Validation).

### For Targeted Analysis (Specific Question)

1. Read OPERATING_RULES.md.
2. Select only the phase prompts relevant to the question.
3. Run them in the prescribed order.
4. Use TEMPLATES.md to format output.

---

## ARCHITECTURE NOTES

- **Phase Dependency**: Each phase builds on the outputs of previous phases. Do not skip phases.
- **Self-Validation**: Each prompt ends with validation instructions. Run these before proceeding.
- **Output Persistence**: All outputs must be saved in a structured directory under `re-docs/` in the target repository.
- **Scalability**: For repos with 500+ files, phase prompts instruct the agent to batch analysis by module.
- **Language Agnostic**: The framework works with any programming language, framework, or paradigm.
- **Framework Agnostic**: Works for web apps, mobile apps, CLI tools, libraries, AI agents, embedded systems, monorepos.

---

## CORE PRINCIPLES

1. **Understand Before Documenting**: No documentation is written until the agent proves understanding.
2. **Progressive Depth**: Start broad, then go deep. Never dive deep without context.
3. **Cross-Reference Everything**: Every claim must be backed by file:line references.
4. **Traceability**: Every architectural decision must be traced to source code evidence.
5. **Completeness Over Brevity**: Always prefer comprehensive output over short output.
6. **Modular Understanding**: Understand each part in isolation, then understand the whole.
7. **Multiple Representations**: Document architecture in prose, diagrams, and structured data.
8. **Validation Gates**: Do not proceed to the next phase until current phase validation passes.

---

## OUTPUT DIRECTORY STRUCTURE

```
re-docs/
├── 00-scouting/
├── 01-structure/
├── 02-build-config/
├── 03-dependencies/
├── 04-tech-stack/
├── 05-modules/
├── 06-deep-read/
├── 07-architecture/
├── 08-data-flow/
├── 09-call-graph/
├── 10-features/
├── 11-algorithms/
├── 12-design-patterns/
├── 13-api-boundaries/
├── 14-state-events/
├── 15-error-cache-retry/
├── 16-ai-workflows/
├── 17-config-env/
├── 18-documentation/
│   ├── architecture-guide.md
│   ├── developer-handbook.md
│   ├── rebuild-guide.md
│   ├── engineering-notes.md
│   └── cross-references.md
├── 19-validation/
│   └── validation-report.md
├── 20-streaming/ (optional: skip if no streaming patterns)
├── 21-authentication/ (optional: skip if no auth)
├── 22-deployment/ (optional: skip if not applicable)
├── diagrams/
│   ├── architecture/
│   ├── data-flow/
│   ├── call-graphs/
│   ├── sequences/
│   └── component-graphs/
├── CHECKS.md
└── REVERSE_ENGINEERING_SUMMARY.md
```

---

## VERSION HISTORY

| VERSION | DATE | AUTHOR | CHANGES |
|---------|------|--------|---------|
| 1.0 | 2026-07-24 | RE Prompt Engineer | Initial complete framework (32 files) |
| 1.1 | 2026-07-24 | RE Prompt Engineer | Extended: streaming, auth, deployment, UML, troubleshooting, README (37 files) |

---

## NAVIGATION SHORTCUTS

- **Start Here**: `MASTER_PROMPT.md`
- **Mission**: `MISSION.md`
- **Rules**: `OPERATING_RULES.md`
- **Quality**: `QUALITY_STANDARDS.md`
- **Output Format**: `OUTPUT_RULES.md`
- **Checklist**: `CHECKLIST.md`
- **Templates**: `TEMPLATES.md`
- **Phase 00**: `PROMPT_01_SCOUTING.md`
- **Phase 19**: `PROMPT_20_VALIDATION.md`
- **Phase 20 (Extended)**: `PROMPT_21_STREAMING.md`
- **Phase 21 (Extended)**: `PROMPT_22_AUTH_ARCHITECTURE.md`
- **Phase 22 (Extended)**: `PROMPT_23_DEPLOYMENT.md`
- **Troubleshooting**: `TROUBLESHOOTING.md`
