# System Design

The system implements a **Three-Layer Prompt Architecture** designed to isolate configuration, orchestration, and execution.

## Layer 1 — Infrastructure (Configuration & Reference)
**Responsibilities:** Provides the ground rules, standard definitions, and quality metrics that apply globally across the framework.
**Components:** `MASTER_INDEX`, `MISSION`, `OPERATING_RULES`, `QUALITY_STANDARDS`, `PROJECT_SPEC`, `PROMPT_DESIGN_GUIDE`, `OUTPUT_RULES`.

## Layer 2 — Orchestration (Control Plane)
**Responsibilities:** Sequences, loads, and coordinates the execution of sub-prompts.
**Components:** `MASTER_PROMPT.md`
**Mechanism:** Instructs the LLM on which infrastructure files to load into context and in what order to execute the Layer 3 execution prompts.

## Layer 3 — Execution (Data Plane)
**Responsibilities:** Performs the actual analysis and documentation of the target codebase.
**Components:** The 9 (or 10) Phase Prompts (`PROMPT_01` through `PROMPT_10`).
**Mechanism:** Each prompt takes the target codebase and prior phase artifacts as input to generate specific Markdown documentation outputs.
