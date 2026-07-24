# PHASE 6 — AI AGENT WORKFLOW ANALYSIS (conditional)

## Skip condition
If the repository contains no LLM/agent/orchestration code, write `06-ai-agent-workflow.md` containing only: "N/A — no AI/agent logic found in this repository." and move to Phase 7.

## Objective
Document the AI-specific behavior separately from general application logic, since this is often the core differentiator of the product.

## Steps
1. **Prompt Flow** — every system prompt/prompt template found, what variables it injects, where in the execution flow it fires
2. **Reasoning Flow** — how the agent decides its next action (ReAct loop, planner/executor split, state machine, fixed pipeline, etc.)
3. **Planning Flow** — task decomposition logic if present
4. **Tool Calling Flow** — full list of tools/functions exposed to the model, their schemas, and the dispatch/routing logic
5. **Memory Flow** — what gets persisted, where, retrieval triggers, eviction/compression/summarization logic
6. **RAG Flow** — embedding model, vector store, chunking strategy, retrieval-to-prompt injection path

Explicitly mark, for each behavior documented, whether it is **deterministic code** or **model-driven** (i.e., depends on the LLM's output at runtime rather than fixed logic).

## Required Outputs
- `06-ai-agent-workflow.md`

## Validation Checklist
- [ ] Every prompt template documented is quoted structurally (variables/placeholders) without reproducing large verbatim proprietary prompt text beyond what's needed for technical accuracy
- [ ] Deterministic vs model-driven labeling is present for every documented behavior
- [ ] Tool schemas documented match actual function signatures/schemas in code, not assumed conventions
