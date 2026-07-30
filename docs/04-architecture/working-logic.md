# Working Logic

## End-to-End Orchestration Loop
The framework operates as a precise state-machine to prevent the LLM from losing context during long reverse-engineering sessions.

1. **Initialization (Step 1-2):** The LLM loads `MASTER_PROMPT.md` and the core infrastructure files (`MISSION.md`, `OPERATING_RULES.md`, `QUALITY_STANDARDS.md`).
2. **Prerequisite Verification (Step 3):** Before running any Phase Prompt (e.g., `PROMPT_11` for Deep Code Analysis), the LLM verifies that the prerequisite outputs from the previous phase (e.g., Component Map from Phase 3) exist and are valid.
3. **Execution (Step 4):** The LLM runs the specific instructions in the active Phase Prompt against the target repository files.
4. **Validation (Step 5):** The LLM runs a self-audit against `VALIDATION_CHECKLISTS.md`. If the generated output fails the quality gate, the LLM attempts to remediate by re-examining the code. If remediation fails, it flags the issue.
5. **Context Management (Step 6):** **[CRITICAL]** To prevent the context window from overflowing, the LLM generates a **Context Summary**. This summary captures key findings, important file paths, and unresolved ambiguities from the just-completed phase. 
6. **Transition (Step 7):** The LLM yields to the next Phase Prompt, carrying *only* the Context Summary forward, rather than the raw text of the previous phase's output.

## Dynamic Adaptation Rules
The `MASTER_PROMPT.md` instructs the LLM to adapt dynamically:
* If $< 50$ files: Accelerate (read every file directly).
* If $> 500$ files: Plan strategic sampling.
* If no AI/agent code: Skip `PROMPT_16` through `PROMPT_20` entirely.
