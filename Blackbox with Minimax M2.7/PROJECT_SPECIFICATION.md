# Prompt Framework — Project Specification

> **Document:** PROJECT_SPECIFICATION.md  
> **Version:** 1.0.0  
> **Purpose:** Define the specifications of this prompt framework itself

---

## 1. FRAMEWORK IDENTITY

| Property | Value |
|----------|-------|
| **Name** | Enterprise Reverse Engineering Prompt Framework |
| **Version** | 1.0.0 |
| **Type** | Multi-file, modular AI prompt system |
| **Target AI** | AI coding agents (e.g., Claude, GPT, Gemini, Codex) |
| **License** | Internal Enterprise Use |
| **Language** | English (Markdown) |

---

## 2. FRAMEWORK ARCHITECTURE

### 2.1 Structural Design

The framework follows a **Hub-and-Spoke** architecture:

- **Hub:** `MASTER_PROMPT.md` — orchestrates the entire process
- **Spokes:** 10 phase prompts (`PROMPT_01.md` through `PROMPT_10.md`)
- **Modules:** Domain-specific deep-dive modules
- **Templates:** Reusable documentation structures
- **Standards:** Quality and formatting standards
- **Checklists:** Verification and validation checklists

### 2.2 Design Patterns Used

| Pattern | Application |
|---------|-------------|
| **Pipeline** | Sequential phases with gates |
| **Chain of Responsibility** | Each phase hands off to the next |
| **Strategy** | Modules can be swapped for different strategies |
| **Template Method** | Templates define the skeleton of documentation |
| **Blackboard** | Working knowledge base shared across phases |
| **Observer** | Cross-phase feedback loops |
| **Mediator** | MASTER_PROMPT.md mediates between phases |

### 2.3 File Dependency Graph

```
MASTER_PROMPT.md
  ├── MISSION.md (prerequisite)
  ├── OPERATING_RULES.md (prerequisite)
  ├── QUALITY_STANDARDS.md (prerequisite)
  ├── OUTPUT_RULES.md (prerequisite)
  │
  ├── PROMPT_01.md (phase 1)
  ├── PROMPT_02.md (phase 2)
  │     └── modules/MODULE_ARCHITECTURE.md (optional)
  ├── PROMPT_03.md (phase 3)
  │     └── modules/MODULE_DEPENDENCY_GRAPH.md (optional)
  ├── PROMPT_04.md (phase 4)
  ├── PROMPT_05.md (phase 5)
  ├── PROMPT_06.md (phase 6)
  │     ├── modules/MODULE_WORKFLOW_ANALYSIS.md (optional)
  │     └── modules/MODULE_DATA_FLOW.md (optional)
  ├── PROMPT_07.md (phase 7)
  ├── PROMPT_08.md (phase 8)
  │     └── modules/MODULE_AI_WORKFLOW.md (optional)
  ├── PROMPT_09.md (phase 9)
  │     ├── modules/MODULE_DOCUMENTATION_GENERATION.md (optional)
  │     ├── templates/TEMPLATE_ARCHITECTURE_DOC.md
  │     ├── templates/TEMPLATE_COMPONENT_DOC.md
  │     ├── templates/TEMPLATE_SEQUENCE_DIAGRAM.md
  │     ├── templates/TEMPLATE_API_DOC.md
  │     ├── templates/TEMPLATE_WORKFLOW_DOC.md
  │     └── templates/TEMPLATE_REBUILD_GUIDE.md
  └── PROMPT_10.md (phase 10)
        └── modules/MODULE_QUALITY_VALIDATION.md (optional)
```

---

## 3. PHASE SPECIFICATIONS

### 3.1 Phase Summary

| Phase | Name | Primary Goal | Depth | Estimated Output |
|-------|------|-------------|-------|------------------|
| 1 | Discovery | Map the repository surface | Surface | File tree, metadata |
| 2 | Structural Analysis | Understand organization | Medium | Module map, folder docs |
| 3 | Dependency Analysis | Map relationships | Medium-High | Dependency graphs |
| 4 | Deep Code Analysis | Understand logic | High | Function docs, algorithms |
| 5 | Architecture Reconstruction | Reconstruct system design | High | Architecture documents |
| 6 | Workflow Analysis | Trace execution paths | High | Workflow diagrams |
| 7 | Design Patterns | Identify patterns | Medium | Pattern catalog |
| 8 | AI Workflow Analysis | Understand AI systems | High (if AI repo) | AI workflow docs |
| 9 | Documentation Synthesis | Generate final docs | Complete | Full documentation |
| 10 | Quality Assurance | Validate everything | Complete | Validation report |

### 3.2 Phase Entry/Exit Criteria

| Phase | Entry Criteria | Exit Criteria |
|-------|---------------|---------------|
| 1 | MASTER_PROMPT.md read | File tree complete, metadata collected |
| 2 | Phase 1 complete | Module map built, folder analysis done |
| 3 | Phase 2 complete | Dependency graph constructed |
| 4 | Phase 3 complete | All code analyzed, logic understood |
| 5 | Phase 4 complete | Architecture reconstructed and documented |
| 6 | Phase 5 complete | All workflows traced and documented |
| 7 | Phase 6 complete | Design patterns cataloged |
| 8 | Phase 7 complete (if applicable) | AI workflows fully documented |
| 9 | All analysis phases complete | All documentation generated |
| 10 | Phase 9 complete | Validation report generated, all checks passed |

---

## 4. MODULE SPECIFICATIONS

Each module follows this structure:

```markdown
# Module: [NAME]

## Purpose
[Why this module exists]

## When to Use
[Conditions for invoking this module]

## Methodology
[Detailed methodology for the analysis]

## Output Requirements
[What the module must produce]

## Quality Criteria
[How to validate the module's output]

## Cross-References
[Links to related phases, templates, and standards]
```

---

## 5. TEMPLATE SPECIFICATIONS

Each template follows this structure:

```markdown
# Template: [NAME]

## Purpose
[What this template is for]

## Structure
[The template structure with placeholders]

## Usage Guidelines
[How to fill in the template]

## Example
[Illustrative example]

## Quality Checklist
[Checklist for template usage]
```

---

## 6. STANDARDS SPECIFICATIONS

Standards files define:
- Formatting conventions
- Terminology usage
- Diagram standards (Mermaid, PlantUML)
- Code block conventions
- Cross-referencing rules
- File naming conventions
- Metadata requirements

---

## 7. CHECKLIST SPECIFICATIONS

Checklists define:
- Per-phase completion verification items
- Documentation completeness checks
- Quality validation checks
- Cross-reference verification items
- Final sign-off criteria

---

## 8. EXTENSIBILITY

### 8.1 Adding a New Phase
1. Create `PROMPT_NN.md` following the phase prompt structure.
2. Update `MASTER_PROMPT.md` phase sequence.
3. Update `MASTER_INDEX.md`.
4. Add entry/exit criteria to this specification.
5. Add to the file dependency graph.

### 8.2 Adding a New Module
1. Create `modules/MODULE_NAME.md` following the module structure.
2. Update `MASTER_INDEX.md` module section.
3. Reference it from relevant phase prompts.

### 8.3 Adding a New Template
1. Create `templates/TEMPLATE_NAME.md` following the template structure.
2. Update `MASTER_INDEX.md` template section.
3. Reference it from `PROMPT_09.md` and `MODULE_DOCUMENTATION_GENERATION.md`.

---

## 9. FRAMEWORK QUALITY METRICS

| Metric | Target | Measurement |
|--------|--------|-------------|
| Phase Completion Rate | 100% | All phases completed |
| File Coverage | 100% | Every file documented |
| Documentation Accuracy | > 95% | Verified by cross-reference |
| Diagram Completeness | All applicable | No missing diagrams |
| Validation Pass Rate | 100% | All checks pass |
| Confidence Threshold | > 80% | All findings above threshold |

---

*This specification defines what this framework IS. Every prompt in the framework is designed to implement this specification faithfully.*

