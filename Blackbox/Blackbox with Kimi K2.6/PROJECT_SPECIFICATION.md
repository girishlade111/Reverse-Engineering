# Project Specification

## Enterprise Reverse Engineering Prompt Framework

---

## 1. Framework Identification

| Field | Value |
|-------|-------|
| **Framework Name** | Enterprise Reverse Engineering Prompt Framework |
| **Version** | 1.0.0 |
| **Classification** | Enterprise-Grade AI Prompt Architecture |
| **Domain** | Software Reverse Engineering & Technical Documentation |
| **Target AI** | Any LLM-based coding agent with file system access |
| **Repository Scope** | Any software repository (any language, any size, any domain) |

---

## 2. Technical Architecture

### 2.1 Framework Structure

```
REVERSE_ENGINEERING_FRAMEWORK/
├── MASTER_INDEX.md              # Complete framework map
├── MASTER_PROMPT.md             # Master orchestration prompt
├── MISSION.md                   # Mission, vision, objectives
├── OPERATING_RULES.md           # Immutable operating rules
├── PROJECT_SPECIFICATION.md     # This document
├── PROMPT_DESIGN_GUIDE.md       # Design patterns & conventions
├── QUALITY_STANDARDS.md         # Quality benchmarks
├── OUTPUT_RULES.md              # Output formatting rules
│
├── DOMAIN_2_DISCOVERY/
│   ├── PROMPT_01_REPO_DISCOVERY.md
│   ├── PROMPT_02_LANGUAGE_DETECTION.md
│   ├── PROMPT_03_DEPENDENCY_MAPPING.md
│   └── PROMPT_04_CONFIG_ANALYSIS.md
│
├── DOMAIN_3_ARCHITECTURE/
│   ├── PROMPT_05_ARCHITECTURE_HIGH_LEVEL.md
│   ├── PROMPT_06_MODULE_DECOMPOSITION.md
│   ├── PROMPT_07_COMPONENT_ANALYSIS.md
│   ├── PROMPT_08_DATA_FLOW_MAPPING.md
│   └── PROMPT_09_DEPENDENCY_GRAPH.md
│
├── DOMAIN_4_DEEP_INTELLIGENCE/
│   ├── PROMPT_10_CLASS_FUNCTION_ANALYSIS.md
│   ├── PROMPT_11_ALGORITHM_EXTRACTION.md
│   ├── PROMPT_12_DESIGN_PATTERN_DETECTION.md
│   ├── PROMPT_13_ERROR_HANDLING_ANALYSIS.md
│   └── PROMPT_14_STATE_MANAGEMENT.md
│
├── DOMAIN_5_WORKFLOW/
│   ├── PROMPT_15_EXECUTION_PIPELINE.md
│   ├── PROMPT_16_EVENT_WORKFLOW.md
│   ├── PROMPT_17_AI_WORKFLOW_ANALYSIS.md
│   ├── PROMPT_18_TOOL_INTEGRATION.md
│   └── PROMPT_19_CACHING_PERFORMANCE.md
│
├── DOMAIN_6_DOCUMENTATION/
│   ├── PROMPT_20_DOCUMENTATION_ARCHITECTURE.md
│   ├── PROMPT_21_DOCUMENTATION_TECHNICAL.md
│   ├── PROMPT_22_DOCUMENTATION_DEVELOPER.md
│   ├── PROMPT_23_DOCUMENTATION_DIAGRAMS.md
│   └── PROMPT_24_DOCUMENTATION_QUALITY.md
│
└── DOMAIN_7_VALIDATION/
    ├── PROMPT_25_VALIDATION_ENGINEERING.md
    ├── PROMPT_26_VALIDATION_COVERAGE.md
    ├── PROMPT_27_CROSS_REFERENCE.md
    └── PROMPT_28_FINAL_REVIEW.md
```

### 2.2 Processing Pipeline

```
┌──────────────────────────────────────────────────────────────────┐
│                     PHASE 0: INITIALIZATION                       │
│  Load MASTER_PROMPT.md → Read MISSION → Apply OPERATING_RULES    │
└────────────────────────────┬─────────────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────────────┐
│                   PHASE 1: DISCOVERY (Prompts 01-04)              │
│  Scan → Detect → Map Dependencies → Analyze Config               │
└────────────────────────────┬─────────────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────────────┐
│               PHASE 2: ARCHITECTURE (Prompts 05-09)               │
│  High-Level → Module → Component → Data Flow → Dependency Graph  │
└────────────────────────────┬─────────────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────────────┐
│             PHASE 3: DEEP INTELLIGENCE (Prompts 10-14)            │
│  Classes → Algorithms → Patterns → Errors → State                │
└────────────────────────────┬─────────────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────────────┐
│               PHASE 4: WORKFLOW (Prompts 15-19)                   │
│  Execution → Events → AI Workflows → Tools → Caching             │
└────────────────────────────┬─────────────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────────────┐
│             PHASE 5: DOCUMENTATION (Prompts 20-24)                │
│  Architecture → Technical → Developer → Diagrams → Quality       │
└────────────────────────────┬─────────────────────────────────────┘
                             │
┌────────────────────────────▼─────────────────────────────────────┐
│               PHASE 6: VALIDATION (Prompts 25-28)                 │
│  Engineering → Coverage → Cross-Reference → Final Review         │
└──────────────────────────────────────────────────────────────────┘
```

---

## 3. Module Specifications

### 3.1 Prompt Module Structure

Each prompt file follows this structure:

```markdown
# PROMPT_XX: [Name]

## Classification
- **Domain:** [Domain Name]
- **Phase:** [Phase Number]
- **Prerequisites:** [List of prerequisite prompts]
- **Dependencies:** [Files/modules needed before execution]

## Objective
[Clear statement of what this prompt achieves]

## Input Requirements
[What information must be available before executing this prompt]

## Analysis Tasks
[Numbered list of specific analysis tasks]

## Output Requirements
[What must be produced as output]

## Quality Checks
[Verification steps for this prompt's output]

## Continuation Rules
[How to handle if analysis exceeds limits]
```

### 3.2 Data Flow Between Prompts

| Output From | Input To | Data Transferred |
|-------------|----------|-----------------|
| P01 | P02, P03, P04 | File tree, repo metadata, structure map |
| P02 | P03, P05 | Language list, framework list, version info |
| P03 | P05, P09 | Dependency graph, package list |
| P04 | P05, P08 | Config map, env vars, build config |
| P05 | P06, P07, P08 | High-level architecture diagram |
| P06 | P07, P09 | Module decomposition map |
| P07 | P10, P12 | Component interface specifications |
| P08 | P15, P16 | Data flow diagrams |
| P09 | P05, P06 | Dependency graph |
| P10 | P11, P12 | Class/function catalog |
| P11 | P13, P14 | Algorithm specifications |
| P12 | P20, P21 | Design pattern documentation |
| P13 | P15, P19 | Error handling specifications |
| P14 | P15, P16 | State machine specifications |
| P15 | P20, P21 | Execution pipeline documentation |
| P16 | P20, P21 | Event workflow documentation |
| P17 | P20, P21 | AI workflow documentation |
| P18 | P20, P21 | Tool integration documentation |
| P19 | P20, P21 | Caching/performance documentation |
| P20-P24 | P25-P28 | Draft documentation |
| P25-P28 | Final Output | Validated documentation |

---

## 4. Quality Specifications

### 4.1 Documentation Quality Levels

| Level | Description | Required For |
|-------|-------------|--------------|
| **L1: Inventory** | File listing with brief purpose | All files |
| **L2: Summary** | Module-level overview with key interfaces | All modules |
| **L3: Detailed** | Function-level documentation with signatures | Core modules |
| **L4: Comprehensive** | Full architectural analysis with diagrams | Architecture documentation |
| **L5: Production** | Complete rebuild-ready documentation | Final deliverable |

### 4.2 Accuracy Requirements

| Metric | Target | Measurement |
|--------|--------|-------------|
| Factual Accuracy | 100% | Verified against source code |
| Completeness | 100% | All files/modules covered |
| Traceability | 100% | Every claim has source reference |
| Consistency | 100% | No contradictory statements |
| Gap Documentation | 100% | All unknowns explicitly noted |

---

## 5. Extension Points

The framework supports extension through:

1. **New Prompt Modules** — Add new prompts to any domain
2. **Domain-Specific Adapters** — Customize prompts for specific tech stacks
3. **Quality Plugins** — Add specialized quality checks
4. **Output Templates** — Customize documentation formats
5. **Analysis Depth Controls** — Adjust depth parameters per module

---

## 6. Constraints & Limitations

### 6.1 Framework Constraints
- Requires file system access to the target repository
- Assumes the AI agent can read and analyze source code files
- Documentation quality depends on source code quality
- Cannot recover deleted or missing code

### 6.2 Known Limitations
- Cannot execute code to verify runtime behavior
- Cannot access external documentation not in the repository
- Cannot interview developers for context
- Analysis is limited to what can be inferred from static code
