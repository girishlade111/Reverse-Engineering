# 06-ai-agent-workflow

This repository encodes a highly sophisticated AI agent workflow via structural prompt engineering, bypassing the need for a Python/Node orchestration backend (like LangChain) by leveraging the LLM's own chat context as the runtime environment.

## Prompt Flow (The 35-Prompt Chain)
* **Meta-Orchestrator:** `MASTER_PROMPT.md` acts as the overarching controller. It does not do the analysis itself; rather, it instructs the LLM to load `PROMPT_01` through `PROMPT_35` sequentially.
* **Dynamic Branching:** The workflow includes conditional execution. Based on findings in Phase 3 (Architecture Reconstruction), the `MASTER_PROMPT.md` dictates whether the LLM should execute Phase 5 (Prompts 16-20) or skip them entirely if no AI/Agent logic exists in the target repo.

## Execution Model & State Management
* **State Machine:** The `PROMPT_DEPENDENCY_MAP.md` enforces a Directed Acyclic Graph (DAG) state machine.
* **Context Compression:** The primary challenge for any AI agent reading an entire codebase is Context Window Overflow. This framework solves this by enforcing a **Context Summary** output at the end of every phase.
  * Instead of maintaining the raw text of a 500-file analysis in memory, the LLM writes the detailed analysis to disk (e.g., `_analysis/` files) and only carries forward the highly compressed Context Summary into the next phase.

## Validation Pipeline (Self-Healing)
* **Quality Gates:** Before transitioning states (from Phase N to Phase N+1), the LLM must execute a self-audit using `VALIDATION_CHECKLISTS.md`.
* **Remediation Loop:** If the output fails the checklist (e.g., a function's purpose was inferred rather than read from code), the LLM is instructed to pause, re-examine the specific codebase files, and attempt to remediate the hallucination before proceeding.

## Tool Calling & Memory
* **Implicit Tools:** The framework assumes execution within an environment (like Cursor, Claude Code, or Opencode) that provides implicit filesystem tools (Read File, List Dir, Semantic Search).
* **Memory Architecture:**
  * *Short-term memory:* The current Phase Prompt + Context Summary.
  * *Long-term memory:* The generated markdown files on disk. 
  * *Rules engine:* The Layer 1 infrastructure files (`QUALITY_STANDARDS.md`, `OPERATING_RULES.md`) remain pinned in the context.
