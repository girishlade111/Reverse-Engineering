# 06-ai-agent-workflow

This repository encodes an AI agent workflow directly via sequential prompting, rather than using an executable orchestration backend (like LangChain or AutoGen). The LLM itself acts as the runtime environment.

## Prompt Flow
* **System Prompt / Meta-Prompt:** The `MASTER_PROMPT.md` acts as the meta-prompt. It establishes the persona ("Senior Staff Software Architect") and injects constraints (`QUALITY_STANDARDS.md`, `OPERATING_RULES.md`).
* **Execution Flow:** Prompts are fed into the LLM sequentially. The output of one phase (e.g., the folder tree from Phase 1) implicitly becomes context for the subsequent phases, assuming the LLM maintains a continuous chat context.

## Reasoning Flow
* **Framework:** The reasoning flow loosely follows a Chain-of-Thought approach combined with strict constraints. 
* **State Machine:** The workflow enforces a linear state machine (Phases 1 → 10). The LLM is instructed *not* to skip ahead.
* **Ambiguity Handling:** The LLM is instructed to guess nothing (evidence over inference) and use a specific tag (`[UNVERIFIED]`) or pause and ask a clarifying question if completely blocked.

## Planning Flow
* The framework acts as a pre-computed plan. The AI agent does not generate its own task decomposition for the reverse engineering; instead, it executes the decomposition provided by the framework's phases.

## Tool Calling Flow
* **Explicit Tools:** None defined in the framework itself. However, the framework is designed to be copy-pasted into environments like Claude Code, Cursor, or Opencode, which *do* have tools (e.g., file reading, bash execution). 
* **Implicit Expectation:** The framework assumes the AI has read access to the local filesystem to fulfill the deep code analysis.

## Memory Flow
* **Persistence:** All memory is persisted by instructing the LLM to write Markdown files (`01-repository-intelligence.md`, etc.). These files serve as the "long-term memory" that Phase 9 will later synthesize.
* **Context Management:** The "Continuation Rule" handles context window limits by forcing the LLM to stop cleanly and emit a resumption token before truncation occurs.

*All behavior described above is **Model-Driven**, meaning the success of the workflow depends entirely on the LLM's adherence to the text instructions at runtime, rather than deterministic compiled code.*
