# 02-file-folder-analysis

This document provides a detailed structural analysis of the files and folders in the repository, focusing specifically on the **Canonical Reference Variant (`Hermes With Deepseek v4 flash`)** to demonstrate the framework's true complexity.

## Infrastructure Layer (Layer 1)
These files govern the strict rules of engagement for the AI agent.
* `MASTER_INDEX.md`: The root index mapping out the 9 phases and 35 prompts.
* `MISSION.md`: The definition of done and overarching goals.
* `PROJECT_SPECIFICATION.md`: Defines the scope, constraints, and non-goals.
* `OPERATING_RULES.md`: Instructions for agent pacing, continuation rules, and ambiguity handling.
* `QUALITY_STANDARDS.md`: Strict anti-hallucination rules and the completeness bar.
* `OUTPUT_RULES.md`: Conventions for structuring output and file naming.
* `PROMPT_DESIGN_GUIDE.md`: Design rationale and instructions for extending the framework.
* `FRAMEWORK_DESIGN_PHILOSOPHY.md`: Theoretical underpinning of the AI orchestration approach.
* `PROMPT_DEPENDENCY_MAP.md`: The execution DAG (Directed Acyclic Graph) determining the order of the 35 prompts.
* `VALIDATION_CHECKLISTS.md`: Granular QA checks for every phase.
* `GLOSSARY.md`: Shared definitions.
* `DIAGRAM_TEMPLATES.md`: Pre-approved Mermaid.js schemas to prevent syntax errors.

## Orchestration Layer (Layer 2)
* `MASTER_PROMPT.md`: The single entry point that instructs the LLM how to iterate through Layer 3.

## Execution Layer (Layer 3)
The execution layer consists of 35 specialized prompts organized into 9 discrete phases.

### Phase 1: Discovery (`PROMPT_01` to `03`)
* `PROMPT_01_RECONNAISSANCE.md` (or similar): Initial repository scan.
* `PROMPT_02_TECH_STACK_IDENTIFICATION.md`: Determines languages and frameworks.
* `PROMPT_03_...`: Finalizes discovery inventory.

### Phase 2: Structural Analysis (`PROMPT_04` to `06`)
* `PROMPT_04_...`: Directory architecture mapping.
* `PROMPT_05_...`: Dependency analysis.
* `PROMPT_06_...`: Entry point identification.

### Phase 3: Architecture Reconstruction (`PROMPT_07` to `10`)
* `PROMPT_07` to `10`: Maps the system components, layers, and design patterns.

### Phase 4: Deep Code Analysis (`PROMPT_11` to `15`)
* `PROMPT_11` to `15`: Traces execution paths, data flows, state machines, and concurrency models.

### Phase 5: AI and Automation Analysis (`PROMPT_16` to `20`)
* `PROMPT_16` to `20`: Specific prompts for extracting prompt chains, agent workflows, tool definitions, and RAG pipelines.

### Phase 6: Integration and Boundary Analysis (`PROMPT_21` to `24`)
* `PROMPT_21` to `24`: API extraction, database schemas, external services, and event buses.

### Phase 7: Documentation Generation (`PROMPT_25` to `30`)
* `PROMPT_25` to `30`: Drafts the architecture docs, sequence diagrams, component diagrams, and reference manuals.

### Phase 8: Validation and Quality (`PROMPT_31` to `34`)
* `PROMPT_31` to `34`: Automated self-audit prompts that force the LLM to double-check its Phase 7 outputs against the `VALIDATION_CHECKLISTS.md`.

### Phase 9: Rebuild Package (`PROMPT_35`)
* `PROMPT_35_...`: Synthesizes the validated docs into the final developer rebuild guide.
