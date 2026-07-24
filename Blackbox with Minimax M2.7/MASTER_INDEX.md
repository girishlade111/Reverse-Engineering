# Enterprise Reverse Engineering Prompt Framework — Master Index

> **Version:** 1.0.0  
> **Classification:** Enterprise-Grade AI Prompt Framework  
> **Purpose:** Guide AI agents to comprehensively reverse engineer any software repository  
> **Maintainability:** Modular, Extensible, Reusable

---

## 📋 Framework Overview

This framework is a modular, multi-phase prompt system designed to enable any AI coding agent to completely reverse engineer a software repository. It covers every aspect of software understanding—from structural analysis to deep architectural reconstruction and professional documentation generation.

---

## 🗂️ File Structure

```
reverse-engineering-framework/
│
├── MASTER_INDEX.md              ← You are here
├── MASTER_PROMPT.md             ← Entry point / orchestrator prompt
├── PROJECT_SPECIFICATION.md     ← Framework project specification
├── PROMPT_DESIGN_GUIDE.md       ← Design philosophy & architecture
├── MISSION.md                   ← Core mission statement
├── OPERATING_RULES.md           ← AI agent operating constraints
├── QUALITY_STANDARDS.md         ← Quality benchmarks
├── OUTPUT_RULES.md              ← Documentation output specifications
│
├── PROMPT_01.md                 ← Phase 1: Repository Init & Discovery
├── PROMPT_02.md                 ← Phase 2: Structural Analysis & Mapping
├── PROMPT_03.md                 ← Phase 3: Dependency & Relationship Analysis
├── PROMPT_04.md                 ← Phase 4: Deep Code Analysis
├── PROMPT_05.md                 ← Phase 5: Architecture Reconstruction
├── PROMPT_06.md                 ← Phase 6: Workflow & Execution Path Analysis
├── PROMPT_07.md                 ← Phase 7: Design Pattern & Decision Analysis
├── PROMPT_08.md                 ← Phase 8: AI Workflow & Agent Analysis
├── PROMPT_09.md                 ← Phase 9: Documentation Synthesis
├── PROMPT_10.md                 ← Phase 10: Quality Assurance & Validation
│
├── modules/
│   ├── MODULE_ARCHITECTURE.md
│   ├── MODULE_DEPENDENCY_GRAPH.md
│   ├── MODULE_WORKFLOW_ANALYSIS.md
│   ├── MODULE_AI_WORKFLOW.md
│   ├── MODULE_DATA_FLOW.md
│   ├── MODULE_DOCUMENTATION_GENERATION.md
│   └── MODULE_QUALITY_VALIDATION.md
│
├── templates/
│   ├── TEMPLATE_ARCHITECTURE_DOC.md
│   ├── TEMPLATE_COMPONENT_DOC.md
│   ├── TEMPLATE_SEQUENCE_DIAGRAM.md
│   ├── TEMPLATE_API_DOC.md
│   ├── TEMPLATE_WORKFLOW_DOC.md
│   └── TEMPLATE_REBUILD_GUIDE.md
│
├── checklists/
│   ├── CHECKLIST_ANALYSIS.md
│   ├── CHECKLIST_DOCUMENTATION.md
│   └── CHECKLIST_VALIDATION.md
│
└── standards/
    ├── STANDARDS_ARCHITECTURE.md
    ├── STANDARDS_DOCUMENTATION.md
    └── STANDARDS_REVERSE_ENGINEERING.md
```

---

## 🧭 Navigation Guide

### 1. Start Here
| File | Purpose |
|------|---------|
| `MASTER_PROMPT.md` | The orchestrator prompt. Primary entry point for the AI agent. |
| `MISSION.md` | Understand the core mission and objectives. |

### 2. Understand the Framework
| File | Purpose |
|------|---------|
| `PROJECT_SPECIFICATION.md` | Detailed specifications of this prompt framework. |
| `PROMPT_DESIGN_GUIDE.md` | Architectural decisions behind the prompt design. |

### 3. Operating Constraints
| File | Purpose |
|------|---------|
| `OPERATING_RULES.md` | Rules the AI must follow during reverse engineering. |
| `QUALITY_STANDARDS.md` | Quality benchmarks all output must meet. |
| `OUTPUT_RULES.md` | Documentation format and structure rules. |

### 4. Execution Phases (Sequential)
| Phase | File | Focus Area |
|-------|------|------------|
| 1 | `PROMPT_01.md` | Repository Initialization & Discovery |
| 2 | `PROMPT_02.md` | Structural Analysis & Mapping |
| 3 | `PROMPT_03.md` | Dependency & Relationship Analysis |
| 4 | `PROMPT_04.md` | Deep Code Analysis |
| 5 | `PROMPT_05.md` | Architecture Reconstruction |
| 6 | `PROMPT_06.md` | Workflow & Execution Path Analysis |
| 7 | `PROMPT_07.md` | Design Pattern & Decision Analysis |
| 8 | `PROMPT_08.md` | AI Workflow & Agent Analysis |
| 9 | `PROMPT_09.md` | Documentation Synthesis |
| 10 | `PROMPT_10.md` | Quality Assurance & Validation |

### 5. Domain Modules (Use as Needed)
| File | Purpose |
|------|---------|
| `modules/MODULE_ARCHITECTURE.md` | Deep architecture analysis guidance |
| `modules/MODULE_DEPENDENCY_GRAPH.md` | Dependency graph construction |
| `modules/MODULE_WORKFLOW_ANALYSIS.md` | Workflow and execution path analysis |
| `modules/MODULE_AI_WORKFLOW.md` | AI-specific workflow analysis |
| `modules/MODULE_DATA_FLOW.md` | Data flow and state management analysis |
| `modules/MODULE_DOCUMENTATION_GENERATION.md` | Documentation generation strategies |
| `modules/MODULE_QUALITY_VALIDATION.md` | Quality validation procedures |

### 6. Templates (Reusable Structures)
| File | Purpose |
|------|---------|
| `templates/TEMPLATE_ARCHITECTURE_DOC.md` | Architecture documentation template |
| `templates/TEMPLATE_COMPONENT_DOC.md` | Component documentation template |
| `templates/TEMPLATE_SEQUENCE_DIAGRAM.md` | Sequence diagram template |
| `templates/TEMPLATE_API_DOC.md` | API documentation template |
| `templates/TEMPLATE_WORKFLOW_DOC.md` | Workflow documentation template |
| `templates/TEMPLATE_REBUILD_GUIDE.md` | Rebuild guide template |

### 7. Checklists & Standards
| File | Purpose |
|------|---------|
| `checklists/CHECKLIST_ANALYSIS.md` | Analysis completeness checklist |
| `checklists/CHECKLIST_DOCUMENTATION.md` | Documentation completeness checklist |
| `checklists/CHECKLIST_VALIDATION.md` | Validation and sign-off checklist |
| `standards/STANDARDS_ARCHITECTURE.md` | Architecture documentation standards |
| `standards/STANDARDS_DOCUMENTATION.md` | General documentation standards |
| `standards/STANDARDS_REVERSE_ENGINEERING.md` | Reverse engineering methodology standards |

---

## 🔄 Execution Flow

```
MASTER_PROMPT.md
    │
    ├── READ MISSION.md
    ├── READ OPERATING_RULES.md
    ├── READ QUALITY_STANDARDS.md
    ├── READ OUTPUT_RULES.md
    │
    ├── PHASE 1: PROMPT_01.md
    │       └── Repository Discovery
    │
    ├── PHASE 2: PROMPT_02.md
    │       ├── Structural Analysis
    │       └── MODULE_ARCHITECTURE.md (as needed)
    │
    ├── PHASE 3: PROMPT_03.md
    │       ├── Dependency Analysis
    │       └── MODULE_DEPENDENCY_GRAPH.md (as needed)
    │
    ├── PHASE 4: PROMPT_04.md
    │       └── Deep Code Analysis
    │
    ├── PHASE 5: PROMPT_05.md
    │       └── Architecture Reconstruction
    │
    ├── PHASE 6: PROMPT_06.md
    │       ├── Workflow & Execution Path Analysis
    │       ├── MODULE_WORKFLOW_ANALYSIS.md (as needed)
    │       ├── MODULE_DATA_FLOW.md (as needed)
    │       └── MODULE_AI_WORKFLOW.md (as needed)
    │
    ├── PHASE 7: PROMPT_07.md
    │       └── Design Pattern & Decision Analysis
    │
    ├── PHASE 8: PROMPT_08.md
    │       └── AI Workflow & Agent Analysis
    │
    ├── PHASE 9: PROMPT_09.md
    │       ├── Documentation Synthesis
    │       └── MODULE_DOCUMENTATION_GENERATION.md (as needed)
    │
    └── PHASE 10: PROMPT_10.md
            ├── Quality Assurance & Validation
            └── MODULE_QUALITY_VALIDATION.md (as needed)
```

---

## 🔗 Cross-Reference Map

| Concept | Primary File | Supporting Files |
|---------|-------------|------------------|
| Architecture Analysis | `PROMPT_05.md` | `modules/MODULE_ARCHITECTURE.md`, `templates/TEMPLATE_ARCHITECTURE_DOC.md` |
| Component Analysis | `PROMPT_04.md` | `templates/TEMPLATE_COMPONENT_DOC.md` |
| Dependencies | `PROMPT_03.md` | `modules/MODULE_DEPENDENCY_GRAPH.md` |
| Workflows | `PROMPT_06.md` | `modules/MODULE_WORKFLOW_ANALYSIS.md` |
| AI/Agent Systems | `PROMPT_08.md` | `modules/MODULE_AI_WORKFLOW.md` |
| Data Flow | `PROMPT_06.md` | `modules/MODULE_DATA_FLOW.md` |
| Documentation | `PROMPT_09.md` | `modules/MODULE_DOCUMENTATION_GENERATION.md`, all templates |
| Quality | `PROMPT_10.md` | `modules/MODULE_QUALITY_VALIDATION.md`, all checklists |
| Standards | All phases | `standards/` directory |

---

## 📈 Framework Maturity Model

| Level | Description | Phase Coverage |
|-------|-------------|----------------|
| L1: Basic Discovery | Repository structure, file listing, language detection | Phase 1 |
| L2: Structural Understanding | Module hierarchy, folder responsibilities, naming conventions | Phase 2 |
| L3: Relationship Mapping | Dependency graphs, import analysis, call graphs | Phase 3 |
| L4: Deep Comprehension | Algorithm analysis, logic reconstruction, edge cases | Phase 4 |
| L5: Architectural Reconstruction | Design patterns, architectural decisions, system design | Phase 5, 7 |
| L6: Behavioral Understanding | Workflows, execution paths, state transitions, event flows | Phase 6 |
| L7: AI/Agent Comprehension | Prompt pipelines, agent workflows, reasoning chains | Phase 8 |
| L8: Complete Documentation | Full documentation suite, diagrams, guides | Phase 9 |
| L9: Validated Quality | Verified accuracy, completeness, consistency | Phase 10 |

---

*This framework is designed for continuous improvement. Each phase builds upon the previous, ensuring no aspect of the repository remains unexamined.*

