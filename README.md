# Enterprise Reverse Engineering Prompt Framework

> **Version:** 1.0 | **Status:** COMPLETE | **Total Prompt Files:** 36 | **Infrastructure Files:** 12 | **Total Phases:** 9

A modular, extensible, and reusable framework for AI-powered reverse engineering of any software repository — from single-file utilities to multi-service distributed systems with AI/agent workflows. This framework guides an LLM-based AI agent through a systematic, multi-phase analysis pipeline that produces complete, accurate, and actionable documentation.

---

## Table of Contents

- [What This Framework Is](#what-this-framework-is)
- [Why It Exists](#why-it-exists)
- [Architecture Overview](#architecture-overview)
- [Quick Start](#quick-start)
- [The 9-Phase Pipeline](#the-9-phase-pipeline)
  - [Phase 1 — Discovery](#phase-1--discovery)
  - [Phase 2 — Structural Analysis](#phase-2--structural-analysis)
  - [Phase 3 — Architecture Reconstruction](#phase-3--architecture-reconstruction)
  - [Phase 4 — Deep Code Analysis](#phase-4--deep-code-analysis)
  - [Phase 5 — AI & Automation Analysis](#phase-5--ai--automation-analysis-conditional)
  - [Phase 6 — Integration & Boundary Analysis](#phase-6--integration--boundary-analysis)
  - [Phase 7 — Documentation Generation](#phase-7--documentation-generation)
  - [Phase 8 — Validation & Quality](#phase-8--validation--quality)
  - [Phase 9 — Rebuild Package](#phase-9--rebuild-package-optional)
- [Quality Assurance System](#quality-assurance-system)
- [Output Structure](#output-structure)
- [Design Principles](#design-principles)
- [Prerequisites](#prerequisites)
- [File Inventory](#file-inventory)
- [Prompt Dependency Map](#prompt-dependency-map)
- [Glossary](#glossary)
- [Extending the Framework](#extending-the-framework)
- [Contributing](#contributing)
- [License](#license)

---

## What This Framework Is

This is not a software application. It is a **prompt engineering framework** — a collection of 36 interconnected, versioned, and dependency-tracked prompt files (Markdown `.md` files) organized into 9 sequential phases. Each prompt file is a standalone unit of analysis that can be reused across different repositories. Combined, they form a pipeline that guides an AI agent from first encounter to complete documentation of any software system.

The framework produces documentation of such quality that any competent engineer can fully understand, modify, extend, or **rebuild the system from the documentation alone**.

---

## Why It Exists

Software repositories are knowledge silos. The design intent, architectural decisions, and operational knowledge encoded in source code are invisible to everyone who did not write it. This framework exists to:

1. **Extract knowledge from code** — transform implicit design into explicit documentation
2. **Preserve architectural intent** — capture not just what the code does, but *why* it was designed that way
3. **Enable knowledge transfer** — allow new engineers, auditors, and AI systems to understand any repository
4. **Support rebuild and migration** — provide sufficient detail to rebuild a system from scratch
5. **Identify improvement opportunities** — surface architectural debt, anti-patterns, and optimization targets
6. **Accelerate onboarding** — replace weeks of code reading with structured documentation hours

### Scope

**Covered:**
- Any programming language (compiled, interpreted, transpiled)
- Any software domain (web, mobile, desktop, embedded, AI/ML, data pipeline, infrastructure)
- Any repository size (single file to multi-repo monorepo)
- Any architecture style (monolithic, microservices, event-driven, serverless, agent-based)
- Any AI maturity (no AI components to complex multi-agent systems)

**Not covered:**
- Security vulnerability exploitation (this is for understanding, not attacking)
- Legal/forensic reverse engineering (no binary decompilation or DRM circumvention)
- Performance benchmarking (architecture understanding, not runtime profiling)
- Code modification (analysis only; no code generation or refactoring)

---

## Architecture Overview

### Three-Layer Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    LAYER 1 — INFRASTRUCTURE                       │
│                                                                   │
│  MASTER_INDEX  MISSION  OPERATING_RULES  QUALITY_STANDARDS       │
│  PROJECT_SPEC  PROMPT_DESIGN_GUIDE  FRAMEWORK_PHILOSOPHY        │
│  PROMPT_DEPENDENCY_MAP  GLOSSARY  DIAGRAM_TEMPLATES             │
│  VALIDATION_CHECKLISTS  OUTPUT_RULES                            │
│  (12 non-executable configuration & reference files)            │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                   LAYER 2 — ORCHESTRATION                         │
│                                                                   │
│                      MASTER_PROMPT.md                             │
│            Sequences, loads, and coordinates all sub-prompts      │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    LAYER 3 — EXECUTION                            │
│                                                                   │
│  36 prompt files organized into 9 sequential phases              │
│  Each prompt has: MISSION → PREREQUISITES → SYSTEM PROMPT        │
│  → EXECUTION INSTRUCTIONS → OUTPUT SPEC → QUALITY GATE → HANDOFF │
└─────────────────────────────────────────────────────────────────┘
```

### Pipeline Architecture

```
[Repository]
    → Phase 1:  DISCOVERY        (inventory, stack detection)
    → Phase 2:  STRUCTURAL       (architecture skeleton)
    → Phase 3:  ARCHITECTURE     (component map)
    → Phase 4:  DEEP CODE        (code-level understanding)
    → [Branch Point]
        ├─ → Phase 5: AI & AUTOMATION (if AI patterns detected)
        └─ → (skip if no AI patterns)
    → Phase 6:  INTEGRATION      (boundaries & contracts)
    → Phase 7:  DOCUMENTATION    (handbooks, guides, diagrams)
    → Phase 8:  VALIDATION       (quality gates & sign-off)
    → Phase 9:  REBUILD PACKAGE  (optional — rebuild from docs)
    → [Complete Documentation Set]
```

---

## Quick Start

### 1. Load the Framework

Present the MASTER_PROMPT.md to your AI agent as the system-level instruction. This file contains the orchestrator logic that sequences all sub-prompts.

### 2. Provide the Target Repository

Make the target repository accessible to the AI agent (local filesystem, URL, or uploaded archive).

### 3. Execute Phase-by-Phase

The framework follows a strict sequential pipeline. Each phase produces output that feeds into the next. The agent will:
- Read the sub-prompt for the current phase
- Execute the analysis against the repository
- Produce structured output with diagrams
- Pass through the built-in quality gate
- Handoff context to the next phase

### 4. Review and Validate

Phase 8 (Validation & Quality) automatically cross-validates all output for accuracy, completeness, and consistency before final sign-off.

### 5. Output

All documentation is written to a `docs/reverse-engineering/` directory in the target repository.

### AI Agent Requirements

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| Context Window | 32K tokens | 128K+ tokens |
| File I/O | Read files, write files | Full filesystem access |
| Diagram Support | Mermaid rendering | Mermaid with styling |
| Multi-step Execution | Sequential prompts | Full pipeline automation |

---

## The 9-Phase Pipeline

### Phase 1 — Discovery

**Prompts:** P01–P03 (3 files) | **Status:** Mandatory

The entry point. The AI agent performs an initial scan of the repository to establish a baseline understanding.

| Prompt | Purpose | Key Output |
|--------|---------|------------|
| P01 — Repository Scan | Identify repository type, build system, language distribution, and top-level structure | Language breakdown, build system identification, repo type classification |
| P02 — File Inventory | Comprehensive file listing with categorization by type, language, and purpose | Complete file inventory with classification |
| P03 — Technology Stack Detection | Detect frameworks, libraries, tools, and infrastructure dependencies | Tech stack matrix with versions and purpose |

---

### Phase 2 — Structural Analysis

**Prompts:** P04–P06 (3 files) | **Status:** Mandatory

Build the structural skeleton of the repository — how files and modules are organized.

| Prompt | Purpose | Key Output |
|--------|---------|------------|
| P04 — Folder Architecture | Analyze directory structure, naming conventions, and organizational patterns | Folder architecture diagram and rationale |
| P05 — Module Dependency Graph | Map dependencies between modules, packages, and namespaces | Dependency graph with import/require analysis |
| P06 — Entry Point Analysis | Identify all entry points — CLIs, APIs, constructors, main functions, event handlers | Entry point registry with invocation signatures |

---

### Phase 3 — Architecture Reconstruction

**Prompts:** P07–P10 (4 files) | **Status:** Mandatory

Reconstruct the system architecture — how components interact and what patterns they follow.

| Prompt | Purpose | Key Output |
|--------|---------|------------|
| P07 — System Architecture | Reconstruct high-level system architecture from code structure | Architecture diagram with component relationships |
| P08 — Component Decomposition | Identify, classify, and document all components | Component catalog with responsibilities and interfaces |
| P09 — Layer Analysis | Identify architectural layers and their interactions | Layer diagram with data flow between layers |
| P10 — Design Pattern Recognition | Detect and document applied design patterns | Pattern catalog with code locations and rationale |

---

### Phase 4 — Deep Code Analysis

**Prompts:** P11–P15 (5 files) | **Status:** Mandatory

The deepest analytical phase — understand how the code actually works.

| Prompt | Purpose | Key Output |
|--------|---------|------------|
| P11 — Data Flow Analysis | Trace data through the system from input to output | Data flow diagrams and transformation pipeline |
| P12 — Execution Path Reconstruction | Map primary execution paths and control flow | Execution flow diagrams with branching logic |
| P13 — State Management Analysis | Analyze state management approach | State machine diagrams and state transition tables |
| P14 — Error Handling & Retry Strategy | Document error handling patterns | Error classification, handling matrix, and recovery flows |
| P15 — Concurrency & Performance Analysis | Analyze concurrency model and performance architecture | Concurrency model diagrams and bottleneck analysis |

---

### Phase 5 — AI & Automation Analysis

**Prompts:** P16–P20 (5 files) | **Status:** Conditional

**Execute only if AI patterns (prompts, agents, LLM calls) are detected in the repository.** This is the framework's conditional branch — it activates automatically when Phase 3 or Phase 4 identifies AI components.

| Prompt | Purpose | Key Output |
|--------|---------|------------|
| P16 — Prompt Architecture Analysis | Analyze prompt files, templates, and system prompts | Prompt architecture map with dependency chain |
| P17 — Agent Workflow Reconstruction | Decompose agent workflows and decision trees | Agent workflow diagrams with handoff points |
| P18 — Tool Integration Analysis | Map tool definitions, MCP servers, and external integrations | Tool registry with invocation patterns |
| P19 — Planning & Reasoning Pipeline | Analyze planning/reasoning systems (ReAct, Chain-of-Thought, etc.) | Planning pipeline with reasoning step decomposition |
| P20 — Memory & RAG Workflow Analysis | Analyze memory systems and retrieval pipelines | Memory architecture with vector store and retrieval topology |

---

### Phase 6 — Integration & Boundary Analysis

**Prompts:** P21–P24 (4 files) | **Status:** Mandatory

Analyze how the system communicates — internally and externally.

| Prompt | Purpose | Key Output |
|--------|---------|------------|
| P21 — Internal API Contract Analysis | Document internal APIs, interfaces, and contracts | API contract catalog with request/response schemas |
| P22 — External Service Integration | Map all external dependencies and integrations | External dependency matrix with integration patterns |
| P23 — Event Stream & Workflow | Analyze event-driven communication and async workflows | Event catalog with producer/consumer maps |
| P24 — Configuration & Environment Analysis | Document configuration system and environment management | Configuration schema with environment variable registry |

---

### Phase 7 — Documentation Generation

**Prompts:** P25–P30 (6 files) | **Status:** Mandatory

Synthesize all analysis into structured documentation deliverables.

| Prompt | Purpose | Key Output |
|--------|---------|------------|
| P25 — Architecture Handbook | Comprehensive system architecture reference | Full architecture handbook with all diagrams |
| P26 — Developer Handbook | Practical development guide | Developer onboarding guide with code conventions |
| P27 — Rebuild Guide | Step-by-step rebuild instructions | Complete rebuild specification from scratch |
| P28 — API Reference & Class Catalog | Complete API and class documentation | API reference with all endpoints and class catalog |
| P29 — Engineering Notes & Cross-References | Capture edge cases, gotchas, and wisdom | Engineering notes with cross-module references |
| P30 — Validation & Handover Protocol | Final handoff documentation | Handover summary with known gaps and assumptions |

---

### Phase 8 — Validation & Quality

**Prompts:** P31–P34 (4 files) | **Status:** Mandatory

Cross-validate all output before final delivery.

| Prompt | Purpose | Key Output |
|--------|---------|------------|
| P31 — Cross-Phase Accuracy Validation | Validate factual accuracy across all phases | Accuracy report with verified/corrected findings |
| P32 — Completeness Deep Audit | Audit for gaps and missing coverage | Completeness report with gap analysis |
| P33 — Consistency & Contradiction Verification | Find contradictions between phases | Consistency report with resolutions |
| P34 — Final Quality Gate & Sign-off | Final quality check and sign-off | Sign-off certificate with quality score |

---

### Phase 9 — Rebuild Package

**Prompts:** P35–P36 (2 files) | **Status:** Optional

Optional phase for teams planning to rebuild the system.

| Prompt | Purpose | Key Output |
|--------|---------|------------|
| P35 — Rebuild Package Assembly | Package all rebuild artifacts | Complete rebuild package with dependency list |
| P36 — Rebuild Verification Protocol | Define how to verify the rebuild | Verification checklist with acceptance criteria |

---

## Quality Assurance System

The framework has a built-in multi-layer quality system — no external test framework required.

### Layer 1: Per-Prompt Quality Gates

Every one of the 36 prompts has a mandatory **QUALITY GATE** section with pass/fail criteria. Each gate verifies that the prompt's output meets its specific quality bar before handoff to the next prompt.

### Layer 2: 10 Quality Standards (Q1–Q10)

Defined in `QUALITY_STANDARDS.md`:

| ID | Standard | Description |
|----|----------|-------------|
| Q1 | Accuracy | All claims verified against source code |
| Q2 | Completeness | No missing components, files, or relationships |
| Q3 | Traceability | Every architectural claim traces to code |
| Q4 | Structural Quality | Clean diagrams, consistent formatting |
| Q5 | Diagram Quality | Every diagram has a clear purpose and legend |
| Q6 | Consistency | No contradictions across phases |
| Q7 | Clarity | Accessible to engineers unfamiliar with the codebase |
| Q8 | Verifiability | Every claim is testable by the reader |
| Q9 | Quality Gates | Every gate passed before phase completion |
| Q10 | Continuous Improvement | Lessons captured for future runs |

### Layer 3: Validation Checklists

`VALIDATION_CHECKLISTS.md` provides phase-level checklists — comprehensive pass/fail criteria for each of the 9 phases.

### Layer 4: Phase 8 Dedicated Validation

Four dedicated prompts (P31–P34) perform cross-phase validation, accuracy verification, completeness auditing, and final sign-off.

---

## Output Structure

When executed against a target repository, the framework produces this documentation tree:

```
docs/reverse-engineering/
├── SUMMARY.md                           # Executive summary
├── _analysis/                           # Working notes (internal)
│   ├── 01_scan_notes.md
│   ├── 02_dependency_trace.md
│   └── ...
├── 01_discovery_report.md
├── 02_structural_analysis.md
├── 03_architecture_reconstruction.md
├── 04_deep_code_analysis.md
├── 05_ai_automation_analysis.md         (conditional)
├── 06_integration_boundary_analysis.md
├── 07_documentation/
│   ├── ARCHITECTURE_HANDBOOK.md
│   ├── DEVELOPER_HANDBOOK.md
│   ├── REBUILD_GUIDE.md
│   └── diagrams/                        # Mermaid diagrams
├── 08_validation_report.md
└── 09_rebuild_package/                  (optional)
    ├── BUILD_INSTRUCTIONS.md
    └── DEPENDENCIES.md
```

Expected total output: **50–500+ pages** of structured reverse engineering documentation, depending on repository size and complexity.

---

## Design Principles

### 1. Modularity

Each prompt is a self-contained unit with defined inputs, outputs, dependencies, and success criteria. This enables:
- **Reuse** — Individual prompts can be extracted and used in other contexts
- **Testing** — Each prompt can be evaluated independently
- **Replacement** — A prompt can be swapped without affecting the rest of the pipeline
- **Parallelization** — Independent prompts can run simultaneously

### 2. Separation of Concerns

Each prompt has exactly one analytical concern. No prompt covers topics from another prompt's domain. This ensures:
- No duplicate analysis
- Clear ownership of each analytical dimension
- Consistent depth across all dimensions
- Verifiable completeness

### 3. Progressive Deepening (Funnel Architecture)

The framework narrows scope but deepens analysis as phases progress:

```
Phase 1:  ████████████████████████████████  (100% files, shallow)
Phase 2:  ████████████████████████████████  (100% files, structural)
Phase 3:  ██████████████████████░░░░░░░░░░  (80% files, architectural)
Phase 4:  ████████████░░░░░░░░░░░░░░░░░░░░  (40% files, deep)
Phase 5:  ██████░░░░░░░░░░░░░░░░░░░░░░░░░░  (20% files, AI-specific)
Phase 6:  ████████████████████░░░░░░░░░░░░  (60% files, integration)
Phase 7:  ████████████████████████████████  (100% consolidation)
Phase 8:  ████████████████████████████████  (100% verification)
```

### 4. Multi-Resolution Analysis

Analysis operates at three levels simultaneously:
- **Macro:** System architecture, organizational structure, deployment topology
- **Meso:** Component responsibilities, module dependencies, communication patterns
- **Micro:** Function behavior, state transitions, error handling, edge cases

Every finding at one level must be traceable to implementation at the next level down and justifiable at the level above.

### 5. Understanding Before Documentation

No documentation is written until the system is fully understood. Every prompt enforces a "understand first, document second" discipline.

### 6. Operating Rules

The framework enforces 12 binding rules (defined in `OPERATING_RULES.md`) that govern AI agent behavior:
1. Never guess — if you cannot verify, state the uncertainty
2. Trace every finding back to source code
3. If a prompt's quality gate fails, do not proceed
4. Preserve context for dependent prompts
5. Follow output formatting rules exactly
6. If repository patterns contradict framework assumptions, document the deviation

---

## Prerequisites

### For Execution

- An AI agent with a **32K+ token context window** (128K+ recommended)
- File read/write capabilities on the target repository
- Mermaid diagram rendering support (for diagram output)
- The target repository must be **accessible in its entirety** (not partial extracts)

### For the Framework Itself

- This framework requires **no installation, dependencies, or runtime environment**
- It is a collection of Markdown files — readable by any text editor or LLM
- **Zero external dependencies** — no npm, pip, Docker, or any package manager required

---

## File Inventory

### Infrastructure Files (12)

| File | Lines | Purpose |
|------|-------|---------|
| `MASTER_INDEX.md` | 307 | Framework map, table of contents, quick-start guide |
| `MASTER_PROMPT.md` | 192 | Orchestrator — loads and sequences sub-prompts |
| `MISSION.md` | 110 | Core mission, philosophy, principles, success criteria |
| `PROJECT_SPECIFICATION.md` | 251 | Formal specification — architecture, interfaces, contracts |
| `PROMPT_DESIGN_GUIDE.md` | 199 | Design decisions, language adaptation, common pitfalls |
| `FRAMEWORK_DESIGN_PHILOSOPHY.md` | 191 | Design thinking, failure mode analysis, rationale |
| `OPERATING_RULES.md` | 174 | 12 binding rules for AI agents |
| `OUTPUT_RULES.md` | 243 | Documentation format, diagrams, tables, style rules |
| `QUALITY_STANDARDS.md` | 188 | 10 quality standards (Q1–Q10) with measurable criteria |
| `GLOSSARY.md` | 155 | 40+ standardized terminology definitions |
| `PROMPT_DEPENDENCY_MAP.md` | 180 | Full directed dependency graph and context handoff table |
| `DIAGRAM_TEMPLATES.md` | 410 | 13 reusable Mermaid diagram templates with style guide |
| `VALIDATION_CHECKLISTS.md` | 284 | Phase-level quality checklists for sign-off |

### Executable Prompts (36)

| Phase | ID | Prompt | Status |
|-------|----|--------|--------|
| **Phase 1 — Discovery** | P01 | Repository Scan | Mandatory |
| | P02 | File Inventory | Mandatory |
| | P03 | Technology Stack Detection | Mandatory |
| **Phase 2 — Structural Analysis** | P04 | Folder Architecture | Mandatory |
| | P05 | Module Dependency Graph | Mandatory |
| | P06 | Entry Point Analysis | Mandatory |
| **Phase 3 — Architecture Reconstruction** | P07 | System Architecture Reconstruction | Mandatory |
| | P08 | Component Decomposition | Mandatory |
| | P09 | Layer Analysis | Mandatory |
| | P10 | Design Pattern Recognition | Mandatory |
| **Phase 4 — Deep Code Analysis** | P11 | Data Flow Analysis | Mandatory |
| | P12 | Execution Path Reconstruction | Mandatory |
| | P13 | State Management Analysis | Mandatory |
| | P14 | Error Handling & Retry Strategy | Mandatory |
| | P15 | Concurrency & Performance Analysis | Mandatory |
| **Phase 5 — AI & Automation Analysis** | P16 | Prompt Architecture Analysis | Conditional |
| | P17 | Agent Workflow Reconstruction | Conditional |
| | P18 | Tool Integration Analysis | Conditional |
| | P19 | Planning & Reasoning Pipeline | Conditional |
| | P20 | Memory & RAG Workflow Analysis | Conditional |
| **Phase 6 — Integration & Boundaries** | P21 | Internal API Contract Analysis | Mandatory |
| | P22 | External Service Integration | Mandatory |
| | P23 | Event Stream & Workflow Analysis | Mandatory |
| | P24 | Configuration & Environment Analysis | Mandatory |
| **Phase 7 — Documentation Generation** | P25 | Architecture Handbook Generation | Mandatory |
| | P26 | Developer Handbook Generation | Mandatory |
| | P27 | Rebuild Guide Generation | Mandatory |
| | P28 | API Reference & Class Catalog | Mandatory |
| | P29 | Engineering Notes & Cross-References | Mandatory |
| | P30 | Validation & Handover Protocol | Mandatory |
| **Phase 8 — Validation & Quality** | P31 | Cross-Phase Accuracy Validation | Mandatory |
| | P32 | Completeness Deep Audit | Mandatory |
| | P33 | Consistency & Contradiction Verification | Mandatory |
| | P34 | Final Quality Gate & Sign-off | Mandatory |
| **Phase 9 — Rebuild Package** | P35 | Rebuild Package Assembly | Optional |
| | P36 | Rebuild Verification Protocol | Optional |

---

## Prompt Dependency Map

The `PROMPT_DEPENDENCY_MAP.md` file defines the full directed dependency graph. Key structural rules:

- **Sequential within phases:** Prompts within a phase are ordered and must execute in sequence
- **Sequential across phases:** Each phase depends on all prior phases
- **Conditional branching:** Phase 5 executes only if AI/automation patterns are detected (decision point after Phase 4)
- **Optional extension:** Phase 9 is entirely optional and depends on Phase 7 (documentation) rather than Phase 8
- **Context handoff:** Each prompt receives context from its predecessors and passes output to its successors

```
P01 → P02 → P03
  ↓
P04 → P05 → P06
  ↓
P07 → P08 → P09 → P10
  ↓
P11 → P12 → P13 → P14 → P15
  ↓
  ├─ → (AI patterns detected?) → P16 → P17 → P18 → P19 → P20
  └─ → (no AI patterns) ─────── skip
  ↓
P21 → P22 → P23 → P24
  ↓
P25 → P26 → P27 → P28 → P29 → P30
  ↓
P31 → P32 → P33 → P34
  ↓
P35 → P36  (optional)
```

---

## Glossary

Core terminology standardized across all prompts. Full definitions in `GLOSSARY.md`.

| Term | Definition |
|------|------------|
| **Repository** | The target software system being reverse-engineered |
| **Prompt** | A single `.md` file containing instructions for the AI agent |
| **Phase** | A logical group of prompts with a shared analytical goal |
| **Analysis Artifact** | The structured output produced by executing a prompt |
| **Quality Gate** | A mandatory pass/fail checkpoint at the end of each prompt |
| **Context Handoff** | Structured information passed from one prompt to its dependents |
| **Architecture Handbook** | The comprehensive system architecture documentation |
| **Developer Handbook** | Practical guide for engineers working on the system |
| **Rebuild Guide** | Specification for rebuilding the system from scratch |
| **Dependency Graph** | Map of relationships between prompts or between code modules |
| **Design Pattern** | Recurring architectural solution detected in the code |

---

## Extending the Framework

### Adding a New Prompt

1. Follow the standard prompt template structure:
   - Title, Phase, Dependencies, Input, Output, Effort
   - MISSION section
   - PREREQUISITES section
   - SYSTEM PROMPT section (the executable instructions)
   - EXECUTION INSTRUCTIONS section
   - OUTPUT SPECIFICATION section
   - QUALITY GATE section (with pass/fail criteria)
   - HANDOFF section (what to pass to dependent prompts)

2. Update the dependency map (`PROMPT_DEPENDENCY_MAP.md`)
3. Add the prompt to `MASTER_INDEX.md`
4. If needed, update `VALIDATION_CHECKLISTS.md`

### Adapting to Specific Domains

- The framework is language-agnostic by design
- For AI-specific systems, enable Phase 5 (auto-detected by the framework)
- For embedded systems, add domain-specific prompts in a new phase or as replacements
- For data pipelines, emphasize data flow (P11) and event workflows (P23)

### Common Pitfalls

- **Premature documentation:** Do not write output until analysis is complete
- **Shallow analysis:** Each prompt must achieve depth, not just coverage
- **Context loss:** Use structured handoff sections between prompts
- **Quality gate bypass:** Never skip quality gates — they are the framework's immune system

---

## Contributing

This framework is designed to be a living project. Contributions that improve coverage, accuracy, depth, or extensibility are welcome.

### Contribution Guidelines

1. Maintain the modular architecture — each prompt must have a single concern
2. Preserve the dependency structure — update `PROMPT_DEPENDENCY_MAP.md` with any changes
3. All prompts must include quality gates — no gate, no merge
4. Maintain backward compatibility or document breaking changes
5. Follow the standard prompt template structure
6. Update `MASTER_INDEX.md` and `GLOSSARY.md` as needed
7. Keep diagram templates in `DIAGRAM_TEMPLATES.md` and use them consistently

### Areas for Extension

- **New phases:** Add specialized phases for domains like embedded systems, gaming, or firmware
- **Language-specific deep-dives:** Create focused prompts for particular language ecosystems
- **Tool integrations:** Add prompts for analyzing Docker, Kubernetes, Terraform, etc.
- **Compliance overlays:** Add prompts for regulatory compliance analysis (SOC2, HIPAA, PCI-DSS)
- **Security-focused variants:** Extend for security audit use cases

---

## License

This project is currently unlicensed. All rights reserved.

---

*Enterprise Reverse Engineering Prompt Framework v1.0 — 36 prompts, 9 phases, 12 infrastructure files. Zero dependencies. Maximum depth.*