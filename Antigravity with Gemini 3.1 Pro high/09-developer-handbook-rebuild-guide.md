# 09-developer-handbook-rebuild-guide

## Rebuild Order
To rebuild this Enterprise Reverse Engineering Prompt Framework from scratch (or to port it to a new LLM environment), follow these steps:

1. **Scaffold the Infrastructure Layer:**
   * Create `MISSION.md` to define the goal of the agent.
   * Create `OPERATING_RULES.md` to define pacing and continuation rules.
   * Create `QUALITY_STANDARDS.md` to define the strict anti-hallucination and evidence requirements.
2. **Build the Orchestrator:**
   * Create `MASTER_PROMPT.md` as the entry point. It must reference and load the infrastructure files.
3. **Build the Phase Prompts (Execution Layer):**
   * *Phase 1 (Discovery):* Create `PROMPT_01_REPOSITORY_INTELLIGENCE.md`. Instruct the LLM to map the folder tree and identify the stack.
   * *Phase 2 (Structure):* Create `PROMPT_02_FILE_FOLDER_ANALYSIS.md`. Instruct the LLM to document the purpose of every non-trivial file.
   * *Phase 3 (Unit Docs):* Create `PROMPT_03_FUNCTION_CLASS_DOCS.md`.
   * *Phase 4 (Architecture):* Create `PROMPT_04_ARCHITECTURE_RECONSTRUCTION.md` to map dependencies and working logic.
   * *Phase 5 (Diagrams):* Create `PROMPT_05_DIAGRAMS.md` enforcing Mermaid.js syntax.
   * *Phase 6 (AI Workflows):* Create `PROMPT_06_AI_AGENT_WORKFLOW.md`.
   * *Phase 7 (Tech Stack):* Create `PROMPT_07_TECH_STACK.md`.
   * *Phase 8 (Conditional):* Create `PROMPT_08_CONDITIONAL_DOCS.md` for APIs/DBs.
   * *Phase 9 (Rebuild Guide):* Create `PROMPT_09_DEVELOPER_HANDBOOK_REBUILD.md`.
   * *Phase 10 (Validation):* Create `PROMPT_10_VALIDATION_QA.md`.
4. **Deploy:**
   * Place the files in a dedicated variant directory (e.g., `MyNewModel/`). Provide instructions to the human user on how to copy-paste the `MASTER_PROMPT.md` into the agent's chat interface.

## Feature Checklist
* **Continuation Mechanism:** If output truncates, the prompt must explicitly instruct the LLM to emit a continuation string and resume without re-greeting.
* **Evidence-based Tagging:** Implement the `[UNVERIFIED - needs confirmation]` tag rule for missing information.
* **Conditional Skipping:** Implement explicit skip logic for Phase 6 (if no AI code) and Phase 8 (if no APIs/DBs exist).

## Non-obvious Gotchas
* **Context Window Limits:** LLMs often struggle with large codebases. The framework mitigates this by forcing depth-first, single-phase execution. Do not combine multiple phases into a single prompt for large repositories.
* **LLM Laziness:** LLMs tend to summarize or skip "boring" files. The framework combats this by explicitly forbidding skipping and requiring one-line purposes for *every* file. This constraint must be strictly enforced in `QUALITY_STANDARDS.md`.

## Known Debt / What to Do Differently
* **Redundancy across Variants:** The current repository structure heavily duplicates the core infrastructure files (`MISSION.md`, `OPERATING_RULES.md`) across the variant folders (Hermes, Opencode, Claude, etc.).
  * *Alternative Approach:* Create a single `/shared-core` directory for all common infrastructure rules, and have the variant-specific `MASTER_PROMPT.md` files point to this shared core. Trade-off: slightly more complex loading instructions for the LLM, but massively reduced maintenance overhead when updating the core framework rules.
