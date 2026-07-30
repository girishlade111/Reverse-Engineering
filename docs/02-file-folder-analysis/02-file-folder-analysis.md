# 02-file-folder-analysis

This document provides a generalized structural analysis of the files and folders in the repository. Because the repository contains multiple variants of the same framework, the file structure within each variant heavily overlaps.

## Excluded Folders/Files
* `.git/` - Version control artifacts.
* `docs/` - Generated documentation from this analysis.

## Core Infrastructure Files (Present in most variants)
These files form the non-executable configuration & reference layer (Layer 1 & Layer 2):

* `MASTER_INDEX.md`: The root index mapping out the phases and the framework's file structure.
* `MASTER_PROMPT.md`: The orchestration prompt given to the AI to execute all subsequent phases.
* `MISSION.md`: The definition of done and overarching goals for the reverse engineering effort.
* `PROJECT_SPECIFICATION.md`: Defines the scope, constraints, and non-goals of the target analysis.
* `OPERATING_RULES.md`: Instructions for agent turn-to-turn pacing, continuation rules, and ambiguity handling.
* `QUALITY_STANDARDS.md`: Anti-hallucination rules and the completeness bar.
* `OUTPUT_RULES.md`: Conventions for structuring output, file naming, and diagramming.
* `PROMPT_DESIGN_GUIDE.md`: Design rationale and instructions for extending the framework.

## Execution Prompt Files (Layer 3)
Each variant contains a sequence of executable prompts, generally numbered sequentially.

### Variant: Hermes With Deepseek v4 flash (Canonical)
**Purpose:** The canonical reference framework containing the full 36-prompt implementation with 13 supporting files.
* `PROMPT_01_*.md` to `PROMPT_09_*.md` (and beyond): Sequential execution prompts covering discovery, architecture, deep code analysis, diagrams, tech stack, documentation, validation, etc.
* `templates/` (if present): Mermaid and documentation templates used during execution.

### Variant: Opencode With Deepseek v4 flash (Version 1 & 2)
**Purpose:** Optimized for the Opencode agent runtime.
* **Version 1:** Contains 23 executable prompts with supplementary files (checklists, templates, troubleshooting).
* **Version 2:** Reorganized into 10 core prompts plus 14 specialized prompts (`PROMPT_S*.md`) covering sub-domains (Memory, DB, Auth). Also includes pre-generated handbooks.

### Variant: Claude
**Purpose:** Compact 20-file adaptation optimized for Claude Code.
* Condenses the standard phases into 10 numbered analysis prompts designed for a single session within Claude's context window.

### Variant: Qwen
**Purpose:** Variant tailored to Qwen model execution.
* Contains `prompts/` directory with discovery, tech stack, and architecture extraction prompts.
* Contains `templates/` for architecture and component markdown formatting.

### Variant: Gemini With Gemini 3.1 Pro
**Purpose:** Minimal starting point optimized for Gemini 3.1 Pro's extended context window.
* Contains only 3 foundational files (`MASTER_INDEX.md`, `MISSION.md`, `OPERATING_RULES.md`).

### Variant: Blackbox, GLM5.1, Mistral
**Purpose:** Adaptations for these specific LLMs. They largely follow the same core file distribution of Master Prompts and numbered Phase Prompts.

---
*Note: Due to the repetitive nature of the framework variants, trivial and boilerplate files (like empty placeholders or redundant prompt variations) are grouped conceptually above. No structural business logic or backend source code exists in this repository.*
