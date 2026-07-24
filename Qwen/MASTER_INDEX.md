# Enterprise Reverse Engineering Prompt Framework

## Master Index

This document serves as the central navigation point for the complete Reverse Engineering Prompt Framework.

---

## Framework Overview

**Framework Name:** Enterprise Reverse Engineering Prompt Framework (ERE-PF)

**Version:** 1.0.0

**Purpose:** A comprehensive, modular, and extensible prompt framework designed to enable AI coding agents to completely reverse engineer any software repository with maximum technical accuracy, engineering depth, and documentation quality.

**Target Users:** AI coding agents, LLM-based analysis systems, automated documentation generators

---

## Document Structure

### Core Framework Documents

| Document | Purpose |
|----------|---------|
| [MISSION.md](./MISSION.md) | Defines the core mission, objectives, and scope of the reverse engineering task |
| [PROJECT_SPECIFICATION.md](./PROJECT_SPECIFICATION.md) | Detailed specifications for the reverse engineering project |
| [PROMPT_DESIGN_GUIDE.md](./PROMPT_DESIGN_GUIDE.md) | Guidelines for how prompts should be structured and executed |
| [OPERATING_RULES.md](./OPERATING_RULES.md) | Rules governing AI behavior during reverse engineering |
| [QUALITY_STANDARDS.md](./QUALITY_STANDARDS.md) | Quality metrics and standards for output |
| [OUTPUT_RULES.md](./OUTPUT_RULES.md) | Formatting and structural rules for generated documentation |

### Master Prompts

| Document | Purpose |
|----------|---------|
| [MASTER_INDEX.md](./MASTER_INDEX.md) | This file - central navigation |
| [MASTER_PROMPT.md](./MASTER_PROMPT.md) | The primary entry point prompt that orchestrates the entire reverse engineering process |

### Execution Prompts

| Document | Purpose |
|----------|---------|
| [PROMPT_01_REPOSITORY_DISCOVERY.md](./prompts/PROMPT_01_REPOSITORY_DISCOVERY.md) | Initial repository scanning and inventory |
| [PROMPT_02_TECH_STACK_ANALYSIS.md](./prompts/PROMPT_02_TECH_STACK_ANALYSIS.md) | Technology stack identification and analysis |
| [PROMPT_03_ARCHITECTURE_EXTRACTION.md](./prompts/PROMPT_03_ARCHITECTURE_EXTRACTION.md) | System architecture discovery |
| [PROMPT_04_CODE_STRUCTURE_ANALYSIS.md](./prompts/PROMPT_04_CODE_STRUCTURE_ANALYSIS.md) | Code structure and organization analysis |
| [PROMPT_05_DEPENDENCY_MAPPING.md](./prompts/PROMPT_05_DEPENDENCY_MAPPING.md) | Dependency graph construction |
| [PROMPT_06_DATA_FLOW_ANALYSIS.md](./prompts/PROMPT_06_DATA_FLOW_ANALYSIS.md) | Data flow and state management analysis |
| [PROMPT_07_CONTROL_FLOW_ANALYSIS.md](./prompts/PROMPT_07_CONTROL_FLOW_ANALYSIS.md) | Control flow and execution path analysis |
| [PROMPT_08_API_INTERFACE_ANALYSIS.md](./prompts/PROMPT_08_API_INTERFACE_ANALYSIS.md) | API and interface documentation |
| [PROMPT_09_BUSINESS_LOGIC_EXTRACTION.md](./prompts/PROMPT_09_BUSINESS_LOGIC_EXTRACTION.md) | Business logic and domain model extraction |
| [PROMPT_10_AI_WORKFLOW_ANALYSIS.md](./prompts/PROMPT_10_AI_WORKFLOW_ANALYSIS.md) | AI-specific workflow analysis (for AI projects) |
| [PROMPT_11_SECURITY_ANALYSIS.md](./prompts/PROMPT_11_SECURITY_ANALYSIS.md) | Security patterns and authentication analysis |
| [PROMPT_12_TESTING_COVERAGE_ANALYSIS.md](./prompts/PROMPT_12_TESTING_COVERAGE_ANALYSIS.md) | Test structure and coverage analysis |
| [PROMPT_13_BUILD_DEPLOYMENT_ANALYSIS.md](./prompts/PROMPT_13_BUILD_DEPLOYMENT_ANALYSIS.md) | Build system and deployment configuration |
| [PROMPT_14_DOCUMENTATION_SYNTHESIS.md](./prompts/PROMPT_14_DOCUMENTATION_SYNTHESIS.md) | Final documentation synthesis and validation |

### Supporting Templates

| Document | Purpose |
|----------|---------|
| [templates/ARCHITECTURE_TEMPLATE.md](./templates/ARCHITECTURE_TEMPLATE.md) | Template for architecture documentation |
| [templates/COMPONENT_TEMPLATE.md](./templates/COMPONENT_TEMPLATE.md) | Template for component documentation |
| [templates/FUNCTION_TEMPLATE.md](./templates/FUNCTION_TEMPLATE.md) | Template for function/method documentation |
| [templates/DIAGRAM_TEMPLATE.md](./templates/DIAGRAM_TEMPLATE.md) | Template for diagram specifications |
| [templates/CHECKLIST_TEMPLATE.md](./templates/CHECKLIST_TEMPLATE.md) | Template for validation checklists |

### Utility Documents

| Document | Purpose |
|----------|---------|
| [GLOSSARY.md](./GLOSSARY.md) | Terminology and definitions |
| [BEST_PRACTICES.md](./BEST_PRACTICES.md) | Best practices for reverse engineering |
| [TROUBLESHOOTING.md](./TROUBLESHOOTING.md) | Common issues and solutions |

---

## Framework Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    MASTER_PROMPT.md                         │
│              (Orchestration & Entry Point)                  │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                   MISSION.md                                │
│              (Mission Definition & Scope)                   │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        ▼                     ▼                     ▼
┌──────────────┐    ┌──────────────────┐    ┌──────────────┐
│ OPERATING_   │    │ PROJECT_         │    │ PROMPT_      │
│ RULES.md     │    │ SPECIFICATION.md │    │ DESIGN_      │
│ (Rules)      │    │ (Specs)          │    │ GUIDE.md     │
└──────────────┘    └──────────────────┘    └──────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                  EXECUTION PROMPTS                          │
│            (PROMPT_01 through PROMPT_14+)                   │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│                    TEMPLATES                                │
│           (Standardized Output Formats)                     │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────┐
│              QUALITY STANDARDS & OUTPUT RULES               │
│            (Validation & Formatting)                        │
└─────────────────────────────────────────────────────────────┘
```

---

## Usage Instructions

### For AI Agents

1. **Start with MASTER_PROMPT.md** - This is your entry point
2. **Review MISSION.md** - Understand your objectives
3. **Follow OPERATING_RULES.md** - Adhere to all constraints
4. **Execute prompts sequentially** - Follow the numbered prompt order
5. **Use templates** - Maintain consistent output format
6. **Validate against QUALITY_STANDARDS.md** - Ensure quality
7. **Format per OUTPUT_RULES.md** - Maintain consistency

### Execution Flow

```
MASTER_PROMPT → Mission Understanding → Rule Compliance → 
Prompt Execution (Sequential) → Template Application → 
Quality Validation → Output Formatting → Final Synthesis
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2024 | Initial framework release |

---

## Related Documents

- [MISSION.md](./MISSION.md) - Core mission statement
- [MASTER_PROMPT.md](./MASTER_PROMPT.md) - Entry point prompt
- [QUALITY_STANDARDS.md](./QUALITY_STANDARDS.md) - Quality requirements

---

*This framework is designed for enterprise-grade reverse engineering of software repositories.*
