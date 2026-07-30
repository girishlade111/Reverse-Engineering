# Working Logic

## End-to-End Execution Flow
1. **Initialization:** The human operator opens the LLM interface (e.g., Claude Code, Cursor) and pastes the contents of `MASTER_PROMPT.md`.
2. **Context Loading:** The LLM reads `MISSION.md`, `OPERATING_RULES.md`, `QUALITY_STANDARDS.md`, and `OUTPUT_RULES.md` into its context window.
3. **Sequential Execution:** The LLM begins executing `PROMPT_01_REPOSITORY_INTELLIGENCE.md`. It scans the target repository's root, identifies the stack, and outputs `01-repository-intelligence.md`.
4. **Continuation:** Following the rules in `OPERATING_RULES.md`, the LLM automatically moves to the next phase without asking for permission, stopping only if it hits a token limit (emitting a specific continuation token) or an unresolvable ambiguity.
5. **Synthesis:** After completing phases 1 through 8, the LLM runs Phase 9, consuming all generated docs to write the `09-developer-handbook-rebuild-guide.md`.
6. **Validation:** The LLM runs Phase 10 to audit its own output against `QUALITY_STANDARDS.md` and generates a final report.
