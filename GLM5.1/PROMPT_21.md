# PROMPT_21.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_21: AI / LLM Workflow Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_21
- **Phase:** 3
- **Stage:** 1 of 5 (Phase 3 opener)
- **Dependencies:** ART-01 (PROMPT_01), ART-06 (PROMPT_06), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-11 (PROMPT_11), ART-16 (PROMPT_16).
- **Estimated Tokens:** 16000–25000
- **Output Artifacts:** ART-21 (Doc) — AI / LLM Workflow Report.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the AI / LLM Workflow Report artifact (ART-21) that enumerates every AI/LLM workflow in the subject repository, traces each workflow's full pipeline from input to output, extracts every prompt template verbatim (or marks it `REDACTED` with rationale), classifies each agent and reasoning architecture, reconstructs every planning, memory, RAG, and tool-calling pipeline, and records every model parameter that influences non-determinism — so that a downstream engineer can rebuild a behaviorally equivalent intelligence layer from the documentation alone.

---

## 3. When to Invoke

PROMPT_21 is dispatched when ALL of the following predicates hold:

- Phase 2 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.3 (PROMPT_20 emitted `status: SUCCESS` or `status: SKIPPED` under `SCOPE_FULL`).
- ART-01, ART-06, ART-08, ART-09, ART-10, ART-11, and ART-16 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- At least one AI/LLM marker is detected in the in-scope source per § 6.1; otherwise the prompt emits a `SKIPPED` completion record (per § 3.1) and ART-21 is recorded as `NOT_PRODUCED`. Downstream consumers (PROMPT_22, PROMPT_24, PROMPT_25) MUST degrade gracefully by treating ART-21 as `ABSENT`.
- ART-11 records at least one significant data type (`D-XX`) OR ART-09 records at least one function (`FN-XX`); absent either, the prompt emits `BLOCKED` with `INPUT_GAP`.

### 3.1 Skipped Behavior (No AI/LLM Detected)

If § 6.1's marker detection returns an empty set under `SCOPE_FULL`, the prompt emits a `SKIPPED` completion record:

```
COMPLETION_RECORD {
  prompt_id: PROMPT_21,
  status: "SKIPPED",
  artifacts_produced: [],
  quality_checks_passed: [],
  quality_checks_failed: [],
  open_questions: [],
  handoff_ready: true,
  notes: "No AI/LLM markers detected in scope. ART-21 not produced. Downstream consumers MUST treat ART-21 as ABSENT and not require it for handoff."
}
```

Under `SCOPE_CORE` or `SCOPE_MODULE`, the orchestrator skips dispatch entirely. Under `SCOPE_TRIAGE`, PROMPT_21 is never dispatched.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-06 | Map | Module boundaries; LLM workflows are aggregated to the module level for the master pipeline diagram. |
| ART-08 | Doc | Class catalog; agent classes, client-wrapper classes, memory classes, and tool-registry classes are extracted from here. |
| ART-09 | Doc | Function catalog; client-call functions, prompt-rendering functions, tool-dispatch functions, and retrieval functions are detected by name and call patterns. |
| ART-10 | Graph | Call graph; the topology along which an LLM workflow's pipeline is traced. Strongly connected components identify recursive agent loops. |
| ART-11 | Graph | Data-flow diagrams; flows whose sources or sinks are LLM calls identify the intelligence layer's input and output boundaries. Significant data types (`D-XX`) flowing into prompts are prompt-input candidates. |
| ART-16 | Doc | Middleware & pipeline map; LLM call sites often sit inside middleware-style chains (request → prompt-render → LLM-call → post-process → response). Middleware composition patterns inform agent-loop reconstruction. |
| `OPERATING_RULES.md` | Framework file | Bind R13 (read-only), R15 (fingerprint), R16 (binary exclusion), R17 (citation), R19 (no secondary citation), R21 (no invention), R22 (no behavior invention), R23 (UNVERIFIED), R33 (contradiction escalation). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, and Mermaid diagram conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (§ 4.5) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). HOOK-05 (Prompt Exposure) is enforced here. |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect every LLM-client integration per § 6.1 by scanning imports, call patterns, and configuration.
3. If no markers are detected and the scope modifier is `SCOPE_FULL`, emit `SKIPPED` per § 3.1 and halt.
4. Enumerate every AI/LLM workflow `W-XX` per § 6.2 by clustering LLM-client call sites into pipelines.
5. Extract every prompt template per § 6.3 (verbatim or `REDACTED` with rationale).
6. Classify every agent architecture per § 6.4 (ReAct, Plan-and-Execute, Reflexion, multi-agent, single-shot).
7. Reconstruct every planning pipeline per § 6.5 (task decomposition, plan revision, plan execution).
8. Identify every memory system per § 6.6 (short-term, long-term, episodic, semantic, vector store).
9. Reconstruct every RAG pipeline per § 6.7 (chunking, embedding, retrieval, reranking, generation).
10. Extract every tool-calling integration per § 6.8 (tool/function definitions, dispatch, result handling).
11. Identify every reasoning pattern per § 6.9 (chain-of-thought, tree-of-thought, reflection, self-consistency).
12. Extract every model parameter per § 6.10 (temperature, top_p, max_tokens, stop sequences, seed).
13. Identify streaming usage per § 6.11 (token streaming, partial-response handling) — forward reference to PROMPT_22 for full streaming analysis.
14. Emit Mermaid pipeline diagrams per § 6.12 with node-level and edge-level citations.
15. Cross-check the workflow catalog against ART-09's call graph and ART-11's LLM-boundary flows per § 6.13; unaccounted call sites are `CONTRADICTION` findings per R33.
16. Emit ART-21 per § 8 with full front-matter, per-workflow sections, prompt-template catalog, agent-pattern catalog, planning-pipeline catalog, memory-system catalog, RAG-pipeline catalog, tool-calling catalog, reasoning-pattern catalog, model-parameter catalog, traceability index, open questions.
17. Run the Quality Checks in § 9.
18. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 LLM-Client Detection

Detect every LLM-client integration by scanning imports, call patterns, and configuration. A client integration is any code site that issues a request to a hosted or local language model.

**Marker libraries by ecosystem:**

- **OpenAI** — `openai` (Python), `openai` (Node), `@azure/openai`, `@azure/openai-assistants`, `openai-java`, `OpenAI-DotNet`, `go-openai`.
- **Anthropic** — `anthropic` (Python), `@anthropic-ai/sdk` (Node), `anthropic-sdk-java`, `Anthropic.SDK` (.NET).
- **Google Gemini / Vertex** — `@google/generative-ai`, `@google-cloud/vertexai`, `google-generativeai` (Python), `@langchain/google-genai`, `cloud.google.com/go/ai/generativelanguage`.
- **Cohere** — `cohere` (Python), `cohere-ai` (Node).
- **Mistral** — `@mistralai/mistralai`, `mistralai` (Python).
- **AI21** — `ai21` (Python), `@ai21labs/client-schemas`.
- **Together / Anyscale / Replicate / Fireworks** — HTTP clients to `api.together.xyz`, `api.anyscale.com`, `api.replicate.com`, `api.fireworks.ai`.
- **Local models** — `llama.cpp` bindings (`llama-cpp-python`, `node-llama-cpp`), `ollama` (`ollama` Python/Node SDKs, `ollama.ai` HTTP), `vllm`, `text-generation-inference` (HuggingFace TGI), `llamafile`, `MLX` (Apple), `llamafile`, `ggml` directly, `ctransformers`, `exllama`, `gpt4all`.
- **Orchestration frameworks** — `langchain`, `langchain-core`, `langchain-community`, `langgraph` (`langchain-ai/langgraph`), `llama-index` (`llama_index`), `haystack` (`farm-haystack`), `dspy`, `semantic-kernel` (`Microsoft.SemanticKernel`), `promptflow`, ` instructor`, `marvin`, `outlines`, `guidance`, `autogen` (`microsoft/autogen`), `crewai`, `metaflow`, `chatlas`, `ell`, `magentic`, `bondai`, `letta` (`letta-ai/letta`, formerly MemGPT), `mem0`, `litellm` (proxy/unifier).
- **HuggingFace Transformers (inference)** — `transformers.AutoModelForCausalLM`, `pipeline("text-generation", ...)`, `from_pretrained` with a generative checkpoint.
- **Embedding clients** — `sentence-transformers`, `OpenAIEmbeddings`, `CohereEmbeddings`, `HuggingFaceEmbeddings`, `fastembed`, `Infinity` (embedding server).

Each client integration records `client_id` `CL-XX`, `vendor`, `library`, `library_version` (when statically declared), `instantiation_citation` (`file:line-range, symbol`), `config_source` (env var | config file | literal), `base_url` (when overridden — value `REDACTED` if it contains a secret), `api_key_env_var` (value `REDACTED`), `external: true`. Local-model clients record `external: false` with the model-file path.

### 6.2 AI/LLM Workflow Enumeration

Enumerate every AI/LLM workflow `W-XX` by clustering LLM-client call sites into coherent pipelines. A workflow is the maximal set of `FN-XX` and `K-XX` entities that cooperate to produce an LLM-mediated output from a given input. Two call sites belong to the same workflow when the call graph (ART-10) connects them through prompt-rendering, response-parsing, tool-dispatch, or memory-update edges without crossing a public-API boundary (`A-XX` from ART-15).

Each workflow records: `workflow_id` `W-XX`, `name` (derived from the enclosing class or function), `entry_point` `FN-XX`, `client_id` `CL-XX`, `kind` (single-shot | chat | agent | rag | planning | multi-agent | structured-extraction), `module_id` `M-XX`, `pipeline_summary` (one sentence), `claim_count`, `citation`. The workflow's full pipeline (input → pre-processing → prompt-render → LLM-call → response-parse → post-processing → output) is traced per § 6.2.1.

#### 6.2.1 Pipeline Tracing

Trace the pipeline as an ordered list of `stage` entries. Each stage records `stage_id`, `kind` (input-collection | pre-process | prompt-render | llm-call | response-parse | tool-call | tool-result-handling | memory-read | memory-write | retrieval | rerank | post-process | output-emit), `function_id` `FN-XX`, `citation`, `inputs` (list of `D-XX` or literal types), `outputs` (list of `D-XX`), `notes`. The trace is bounded by 50 call-hops and by ART-10's reachability from the entry point; cycles (agent loops) are detected and recorded as `LOOP_DETECTED` with the SCC ID.

### 6.3 Prompt-Template Extraction

Extract every prompt template — this is the most critical procedure because HOOK-05 (Prompt Exposure) is enforced here. A prompt template is any string literal, template file, or templating call whose rendered output is sent to an LLM as the system prompt, user prompt, assistant priming, or few-shot example.

**Extraction sources:**

- **Inline string literals** — multi-line strings assigned to variables named `system_prompt`, `user_prompt`, `SYSTEM_PROMPT`, `prompt_template`, `instruction`, `persona`, `preamble`. Extract the literal verbatim with citation.
- **Template files** — `*.prompt`, `*.tpl`, `*.j2`, `*.jinja`, `*.jinja2`, `*.handlebars`, `*.mustache`, `*.txt` in `prompts/`, `templates/`, `src/prompts/`, `resources/prompts/`. Extract the file content verbatim with `file:line-range` covering the entire file (or the template block).
- **Templating calls** — `PromptTemplate.from_template(...)` (LangChain), `ChatPromptTemplate.from_messages(...)`, `prompt.format(...)`, `f"..."` f-strings consumed by an LLM call, `string.Template(...).substitute(...)`, Jinja2 `Template(...).render(...)`, Mustache `render(template, data)`, `.replace("{{x}}", value)` patterns. Extract the template body verbatim before substitution; record the substitution variables.
- **Few-shot examples** — `examples=[("input1", "output1"), ...]` arrays passed to LLM calls, `FewShotPromptTemplate`, `ExampleSelector` instances. Extract each example verbatim.
- **Compiled prompts** — `dspy.Signature` declarations (the docstring is the prompt), `dspy.Predict`, `dspy.ChainOfThought`, `dspy.ReAct` — extract the signature's docstring and input/output field annotations.
- **System messages in chat histories** — `{"role": "system", "content": "..."}` literals or builder functions. Extract the content verbatim.
- **Tool descriptions** — the `description` field of every tool/function declared for tool-calling (§ 6.8) is itself a prompt consumed by the model's tool-selection logic. Extract each verbatim.

Each prompt template records: `prompt_id` `PR-XX`, `name`, `kind` (system | user | few-shot | tool-description | signature | assistant-priming), `format` (plain | f-string | jinja2 | handlebars | mustache | dspy | langchain-template | literal-block), `template_body_verbatim` (the exact string, or `REDACTED` per § 6.3.1), `substitution_variables` (list of variable names with their source `V-XX` or literal), `language` (the template's natural language), `citation` (`file:line-range, symbol`), `client_id` `CL-XX` (the client it is rendered for), `workflow_id` `W-XX`.

#### 6.3.1 REDACTION Policy

A prompt template is marked `REDACTED` (and its body is replaced with the literal string `REDACTED`) when ANY of the following conditions hold:

- The template contains a secret, API key, or credential literal (per ART-11's sensitive-data lexicon plus `sk-`, `Bearer `, `AKIA`, OAuth tokens).
- The template is loaded from a binary or encrypted file (`*.enc`, `*.bin`, `*.b64`) and the decrypted form is not present in the source.
- The template is fetched at runtime from a remote URL and is not present in the repository.
- The orchestrator's engagement manifest declares the template as `REDACTED` (e.g., proprietary prompts the consumer has chosen not to expose).
- The template contains PII that would identify real users (e.g., a few-shot example with a real email address).

When a template is `REDACTED`, the artifact records: `redaction_rationale` (one of the above categories), `redaction_evidence` (`file:line-range` of the literal that triggered redaction OR the orchestrator directive identifier), `redacted_length` (character count of the redacted body, to support size verification without exposing content).

Templates that contain placeholder variables (e.g., `Hello, {{user_name}}!`) are NOT redacted on the basis of containing a variable name that resembles PII; placeholders are template syntax, not actual PII.

### 6.4 Agent-Architecture Classification

Classify every workflow that issues more than one LLM call per invocation or that branches on the LLM's output as an agent. The classification taxonomy:

- **Single-shot** — one LLM call per invocation, no branching on output. Not an agent; recorded as `kind: single-shot`.
- **ReAct** — interleaved `Thought → Action → Observation` cycles. Detected by prompt templates containing the literals `Thought:`, `Action:`, `Observation:` (or equivalent), by LangChain's `ReActAgent`, `create_react_agent`, `AgentType.ZERO_SHOT_REACT_DESCRIPTION`, by LlamaIndex's `ReActAgent`, by DSPy `dspy.ReAct`, by Semantic Kernel `ReActPlugin`.
- **Plan-and-Execute** — a planner LLM call produces a list of steps; an executor LLM call (or series) carries them out. Detected by prompt templates mentioning `Plan:`, `Step 1:`, by LangGraph's `PlanExecute` pattern, by AutoGen's `PlannerAgent`, by CrewAI's hierarchical process with a `PlanningAgent`.
- **Reflexion** — the agent reflects on its prior output and revises. Detected by prompt templates mentioning `Reflection:`, `Critique:`, `Revision:`, by `Reflexion` DSPy module, by LangGraph self-reflection nodes.
- **Multi-agent** — multiple agents with distinct roles communicate. Detected by AutoGen's `GroupChat`, `AssistantAgent`, `UserProxyAgent`, by CrewAI's `Crew` with multiple `Agent`s, by LangGraph's multi-node graphs with `Agent` named nodes, by ChatDev-style role assignments.
- **Tool-use (function-calling)** — the model is given tool definitions and selects tools to call. Detected by OpenAI `tools=[...]` parameter, Anthropic `tools=[...]` parameter, Gemini `tools=[func(...)]`, LangChain `bind_tools(...)`, LlamaIndex `FunctionTool`, `ToolNode`. Recorded as `kind: tool-use`; if combined with ReAct, record both as `kind: react-with-tools`.
- **Tree-of-Thought (ToT)** — the agent explores multiple reasoning branches and selects the best. Detected by explicit ToT implementations, by prompt templates mentioning `Branch:`, `Evaluate:`, by LangChain's `TreeOfThoughts`.
- **Self-consistency** — the agent samples multiple completions and aggregates (majority vote, etc.). Detected by `n > 1` sampling followed by aggregation logic.
- **Chain-of-Thought (CoT)** — single-shot with a prompt that elicits step-by-step reasoning. Detected by prompt templates containing `Let's think step by step` or equivalent; recorded as `reasoning_pattern: cot` (§ 6.9) rather than as a distinct agent architecture when no loop is present.

Each agent records: `agent_id` `AG-XX`, `workflow_id` `W-XX`, `architecture` (one of the above), `loop_termination_condition` (max-iterations | goal-reached | error | token-budget | human-halt | none), `max_iterations` (when declared), `iteration_count_citation`, `agent_class_id` `K-XX` (when the agent is implemented as a class), `citation`.

### 6.5 Planning-Pipeline Reconstruction

Reconstruct every planning pipeline. A planning pipeline is any sequence of LLM calls that produces, revises, or executes a structured plan. Detection markers:

- **Task decomposition** — prompt templates mentioning `Decompose`, `Break down`, `Subtasks`, `Step 1`, `Plan:`. Outputs are JSON lists or numbered lists of steps.
- **Plan revision** — an LLM call that takes a prior plan and a failure observation and emits a revised plan. Detected by two-call sequences where the second call's input includes the first call's output and an error/observation string.
- **Plan execution** — a loop that iterates over plan steps and executes each (often via tool-calling or sub-agent dispatch).

Each planning pipeline records: `planning_id` `PL-XX`, `workflow_id` `W-XX`, `kind` (decompose | revise | execute | full), `planner_client_id` `CL-XX`, `executor_client_id` `CL-XX` (when distinct), `plan_schema` (the structured output schema — typically a JSON list of step objects), `plan_schema_citation`, `revision_trigger` (error | observation | none), `max_revisions` (when declared), `citation`.

### 6.6 Memory-System Identification

Identify every memory system. Memory is any persistent or semi-persistent store that the agent reads from or writes to during a workflow, distinct from the workflow's primary input and output. The taxonomy:

- **Short-term (working) memory** — the in-flight context: the current message history, the current scratchpad. Detected by `messages` arrays, `ConversationBufferMemory`, `ChatMessageHistory`, `WindowMemory`, in-process variables accumulating agent state.
- **Long-term memory (semantic)** — persistent factual knowledge the agent recalls across sessions. Detected by `VectorStoreRetrieverMemory`, `RedisMemory`, `PostgresMemory`, `SQLMemory`, Zep, Mem0 (`mem0` library), LangGraph `MemorySaver` checkpointer, Letta (`letta-ai`) blocks.
- **Long-term memory (episodic)** — records of past interactions. Detected by `EpisodicMemory`, `ConversationSummaryMemory`, `EntityMemory`, summaries persisted to a store.
- **Vector store** — the underlying embedding index backing retrieval. Detected by `FAISS`, `Chroma`, `Pinecone`, `Weaviate`, `Qdrant`, `Milvus`, `pgvector`, `LanceDB`, `redisvl`, `AstraDB`, `MongoDBAtlasVectorSearch`. Each vector store records `vector_store_id` `VS-XX`, `kind`, `embedding_client_id` `CL-XX`, `dimension` (when statically declared), `distance_metric` (cosine | l2 | ip), `citation`.
- **External memory services** — Zep, Mem0, Letta, A-MEM. Each records `external: true` and the service identifier.

Each memory system records: `memory_id` `MEM-XX`, `workflow_id` `W-XX`, `kind` (short-term | long-term-semantic | long-term-episodic | vector-store | external-service), `backend` (in-process | redis | postgres | sqlite | faiss | chroma | pinecone | weaviate | qdrant | milvus | pgvector | lancedb | zep | mem0 | letta | custom), `embedding_client_id` `CL-XX` (when applicable), `read_fn` `FN-XX`, `write_fn` `FN-XX`, `retention_policy` (when declared: ttl | window | summarization | permanent), `citation`.

### 6.7 RAG-Pipeline Reconstruction

Reconstruct every retrieval-augmented generation (RAG) pipeline. A RAG pipeline is any workflow that retrieves context from a corpus and injects it into a prompt before LLM generation. The pipeline stages:

- **Chunking** — splitting source documents into chunks. Detected by `RecursiveCharacterTextSplitter`, `TokenTextSplitter`, `SentenceSplitter`, LlamaIndex `SentenceSplitter`, `NodeParser`, custom chunking functions. Each chunker records `chunk_strategy` (fixed-size | recursive | sentence | paragraph | semantic | markdown-header | code-ast), `chunk_size`, `chunk_overlap`, `citation`.
- **Embedding** — generating vectors for chunks. Detected by `OpenAIEmbeddings`, `HuggingFaceEmbeddings`, `SentenceTransformerEmbeddings`, `cohere.embed`. Records the embedding client `CL-XX`, `dimension`, `citation`.
- **Indexing** — writing chunks and vectors to a vector store. Records the target `VS-XX`.
- **Retrieval** — at query time, embedding the query and fetching top-k chunks. Detected by `retriever.invoke(query)`, `vectorstore.similarity_search(query, k=5)`, `query_engine.query(...)`. Each retriever records `retrieval_id` `RT-XX`, `top_k`, `similarity_function`, `filter_expression` (when used), `citation`.
- **Reranking** — reordering retrieved chunks by a cross-encoder or LLM-based scorer. Detected by `CohereRerank`, `BgeReranker`, `FlashRerank`, `LLMRerank`, `RankGPrompt`, `colbert` reranking. Records `reranker_id` `RR-XX`, `kind` (cross-encoder | llm-based | learned | hybrid), `top_n_after_rerank`, `citation`.
- **Generation** — the LLM call that consumes the retrieved context. Records the consuming `W-XX` and the prompt template `PR-XX` into which context is injected.

Each RAG pipeline records: `rag_id` `RAG-XX`, `workflow_id` `W-XX`, `chunker`, `embedding_client_id`, `vector_store_id`, `retriever_id`, `reranker_id` (when present), `generation_workflow_id`, `citation`. RAG variants (HyDE, RAG-Fusion, FLARE, Self-RAG, Corrective RAG) are detected by their characteristic patterns and recorded in `variant` field.

### 6.8 Tool-Calling Extraction

Extract every tool-calling integration. A tool is a function the LLM can invoke by name with structured arguments. Detection markers:

- **OpenAI function/tool definitions** — `tools=[{"type": "function", "function": {"name": ..., "description": ..., "parameters": ...}}]` passed to `chat.completions.create()`; `@tool` decorator (LangChain); `FunctionTool.from_defaults(fn)` (LlamaIndex); `kernel.import_skill(...)` or `kernel.register_function(...)` (Semantic Kernel).
- **Anthropic tool definitions** — `tools=[{"name": ..., "description": ..., "input_schema": ...}]` passed to `messages.create()`.
- **Gemini tool definitions** — `tools=[func(...)]` with `FunctionDeclaration`.
- **Manual function-calling parsers** — code that parses the LLM's text output for `Action: tool_name\nAction Input: {...}` patterns and dispatches a Python/JS function. Common in ReAct implementations.
- **MCP (Model Context Protocol)** — `@mcp.tool()`, `mcp.Server`, `from mcp import ...`. Each MCP server records `server_id` `MCP-XX`, `transport` (stdio | sse | http), `tools_exposed` (list of `T-XX`), `citation`.

Each tool records: `tool_id` `T-XX`, `name`, `description` (verbatim from the tool definition; this is itself a `PR-XX` prompt), `parameters_schema` (JSON Schema), `dispatch_fn` `FN-XX`, `return_type` `D-XX`, `error_handling` (throws | returns-error-object | retries | none), `citation`. Tool-result handling records whether the result is fed back to the LLM as a tool message, parsed into a structured type, or both.

Each workflow's tool-calling loop records: `tool_loop_id` `TL-XX`, `workflow_id` `W-XX`, `max_iterations`, `parallel_tool_calls` (true | false), `tool_choice_strategy` (auto | required | specific-tool | none), `citation`.

### 6.9 Reasoning-Pattern Identification

Identify every reasoning pattern. A reasoning pattern is a prompt-engineering or sampling technique that influences how the model reasons. Patterns are not mutually exclusive; a workflow may exhibit several.

- **Chain-of-Thought (CoT)** — prompt elicits step-by-step reasoning. Markers: `Let's think step by step`, `Reasoning:`, `Thought:`, `ChainOfThought` (DSPy), `ChatPromptTemplate` with a `reasoning` placeholder.
- **Self-consistency** — multiple samples aggregated. Markers: `temperature > 0` with `n > 1` and aggregation logic (majority vote, longest common substring, etc.).
- **Tree-of-Thought (ToT)** — branching exploration. Markers: per § 6.4.
- **Reflection / Self-critique** — the model critiques and revises its own output. Markers: `Critique:`, `Reflection:`, two-stage prompts where the second stage's input is the first stage's output.
- **Decomposition** — the model breaks a problem into sub-problems (distinct from planning in that decomposition is single-shot; planning is multi-step). Markers: `Decompose the following`, `Sub-questions:`.
- **Verbalized confidence** — the model is asked to state its confidence. Markers: `Confidence:`, `On a scale of 1-10`.

Each reasoning pattern records: `reasoning_id` `RP-XX`, `workflow_id` `W-XX`, `pattern` (cot | self-consistency | tot | reflection | decomposition | verbalized-confidence), `evidence_citation`, `notes`.

### 6.10 Model-Parameter Extraction

Extract every model parameter that influences non-determinism or output shape. Parameters are extracted from the call site of every LLM invocation.

- `model` / `model_name` — the model identifier (e.g., `gpt-4o`, `claude-3-5-sonnet-20241022`, `gemini-1.5-pro`, `llama3-8b`). Required.
- `temperature` — sampling temperature. Recorded with value and citation.
- `top_p` — nucleus sampling. Recorded with value and citation.
- `top_k` — top-k sampling (local models, Gemini). Recorded with value and citation.
- `max_tokens` / `max_output_tokens` / `max_new_tokens` — output length cap. Recorded.
- `stop` / `stop_sequences` — stop strings. Recorded verbatim (each stop string is itself a behavioral artifact).
- `seed` — when set, enables deterministic sampling. Recorded.
- `presence_penalty`, `frequency_penalty` — OpenAI-specific. Recorded.
- `logit_bias` — token bias. Recorded.
- `response_format` / `json_mode` / `structured_outputs` — structured-output enforcement. Recorded with the schema reference.
- `n` — number of completions. Recorded.
- `stream` — streaming flag (forward reference to PROMPT_22).
- `tool_choice` — tool selection strategy (per § 6.8).
- Custom parameters — vendor-specific parameters (e.g., Anthropic `top_k`, Gemini `safety_settings`). Recorded under `custom_parameters`.

Each LLM call site records: `call_id` `LC-XX`, `workflow_id` `W-XX`, `client_id` `CL-XX`, `parameters` (the above fields, with `UNDECLARED` for omitted parameters), `citation` (`file:line-range, symbol`). When a parameter is set from a configuration variable (per ART-04), record the variable name and the resolved value (or `RESOLVED_AT_RUNTIME`).

### 6.11 Streaming-Usage Identification

Identify whether each LLM call uses streaming and how the stream is consumed. Detection markers:

- `stream=True` (OpenAI Python), `stream: true` (OpenAI Node), `messages.stream(...)` (Anthropic), `generateContentStream` (Gemini), `model.stream(...)`, `astream` (LangChain async streaming).
- Iteration patterns: `for chunk in response:`, `await for chunk in response:`, `.on('data', ...)`, `response.body.pipeTo(...)` (Web Streams API), reactive `subscribe(chunk => ...)`.

Each streaming call records: `streaming_id` `ST-XX`, `call_id` `LC-XX`, `chunk_kind` (token | message | character | sentence), `aggregation_strategy` (concatenate | yield-immediately | buffer-window | none), `partial_response_handling` (yield-partial | yield-only-final | reconstruct-on-done), `citation`. The full streaming analysis (producer/consumer/buffer/backpressure) is delegated to PROMPT_22; ART-21 records only the call-site facts that PROMPT_22 consumes.

### 6.12 Mermaid Pipeline-Diagram Emission

Emit Mermaid diagrams per `OUTPUT_RULES.md` § 7. Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file at `<output_root>/diagrams/<engagement_id>_ART21_D-XX.mmd`.

- **Per-workflow pipeline diagram** — one `flowchart TD` per workflow showing the pipeline stages (input → pre-process → prompt-render → llm-call → response-parse → output) with branches for agent loops, tool calls, and memory reads/writes. Nodes: `FN-XX` (functions), `PR-XX` (prompts), `CL-XX` (clients), `T-XX` (tools), `MEM-XX` (memory). Edges labeled with the data type `D-XX` and citation.
- **Agent-loop diagram** — for each agent workflow, a `stateDiagram-v2` showing the agent's states (e.g., `Plan`, `Execute`, `Observe`, `Reflect`) and transitions.
- **Master intelligence-layer diagram** — a `graph LR` showing every workflow `W-XX` and its client `CL-XX`, with edges for cross-workflow calls (one workflow invoking another as a sub-agent). Decomposed by module when > 30 nodes.

Edge styles: solid black for synchronous calls, dashed blue for streaming, dashed green for memory reads/writes, dashed purple for tool calls, dashed orange for retrieval, dashed red for prompt-rendering (to highlight HOOK-05-relevant edges).

### 6.13 Coverage Cross-Check

Cross-check the workflow catalog against ART-09's call graph and ART-11's LLM-boundary flows:

1. Compute `C_09` = set of `FN-XX` in ART-09 whose call patterns match § 6.1 markers (LLM-client calls).
2. Compute `W_21` = set of `FN-XX` appearing as a stage in any workflow in ART-21.
3. Expected: `W_21 ⊇ C_09` (every LLM-client call site appears in at least one workflow). Call sites in `C_09 \ W_21` are `COVERAGE_GAP` findings recorded as Open Questions.
4. Compute `B_11` = set of `D-XX` in ART-11 whose sources or sinks are LLM-client call sites.
5. Compute `D_21` = set of `D-XX` appearing as a stage input or output in any workflow in ART-21.
6. Expected: `D_21 ⊇ B_11`. Types in `B_11 \ D_21` are `COVERAGE_GAP` findings; types in `D_21 \ B_11` are `CONTRADICTION` findings per R33 (ART-21 references a type ART-11 did not classify as LLM-boundary).

---

## 7. Required Outputs

### ART-21 — AI / LLM Workflow Report

**Type:** Doc.

**Acceptance Criteria:**

- AC-21.1: The artifact file exists at `<output_root>/artifacts/phase3/ART21_<engagement_id>_ai-llm-workflows.md`.
- AC-21.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-21.3: The body contains: Executive Summary, Methodology, LLM-Client Integrations, Workflow Catalog (per-workflow sections with pipeline diagrams), Prompt-Template Catalog (with verbatim or REDACTED bodies), Agent-Architecture Catalog, Planning-Pipeline Catalog, Memory-System Catalog, RAG-Pipeline Catalog, Tool-Calling Catalog, Reasoning-Pattern Catalog, Model-Parameter Catalog, Streaming-Usage Catalog, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-21.4: Every prompt template is documented verbatim OR marked `REDACTED` with rationale (HOOK-05).
- AC-21.5: Every workflow's pipeline is traced from input to output with stage-level citations.
- AC-21.6: Every model parameter is recorded with value and citation; `UNDECLARED` is used for omitted parameters.
- AC-21.7: Every Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-21.8: A `.mmd` sidecar file exists for every Mermaid block under `<output_root>/diagrams/`.
- AC-21.9: Coverage cross-check is recorded with no unresolved contradictions.
- AC-21.10: Every REDACTED prompt records `redaction_rationale`, `redaction_evidence`, and `redacted_length`.

---

## 8. Output Templates

### 8.1 ART-21 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-21
artifact_type: Doc
producing_prompt: PROMPT_21
phase: 3
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
llm_clients:
  - id: CL-01
    vendor: openai | anthropic | google | cohere | mistral | ai21 | together | replicate | local | custom
    library: <name>
    library_version: <text> | UNVERIFIED
    instantiation_citation: <file>:<line-range>
    config_source: env | config-file | literal
    base_url: <url> | DEFAULT | REDACTED
    api_key_env_var: <name> | REDACTED
    external: true
workflows:
  - id: W-01
    name: <text>
    entry_point: FN-XX
    client_id: CL-XX
    kind: single-shot | chat | agent | rag | planning | multi-agent | structured-extraction
    module_id: M-XX
    pipeline_summary: <sentence>
    stages:
      - stage_id: ST-01
        kind: input-collection | pre-process | prompt-render | llm-call | response-parse | tool-call | tool-result-handling | memory-read | memory-write | retrieval | rerank | post-process | output-emit
        function_id: FN-XX
        citation: <file>:<line-range>
        inputs: [D-XX]
        outputs: [D-XX]
        notes: <text>
    loop_detected: true | false
    scc_id: <text> | NA
prompt_templates:
  - id: PR-01
    name: <text>
    kind: system | user | few-shot | tool-description | signature | assistant-priming
    format: plain | f-string | jinja2 | handlebars | mustache | dspy | langchain-template | literal-block
    template_body_verbatim: <verbatim-string> | REDACTED
    redaction_rationale: contains-secret | encrypted-source | remote-fetched | orchestrator-directive | contains-pii | NA
    redaction_evidence: <file>:<line-range> | <directive-id> | NA
    redacted_length: <int> | NA
    substitution_variables: [{ name: <text>, source: V-XX | literal }]
    language: <ietf-tag>
    citation: <file>:<line-range>
    client_id: CL-XX
    workflow_id: W-XX
agents:
  - id: AG-01
    workflow_id: W-XX
    architecture: react | plan-and-execute | reflexion | multi-agent | tool-use | react-with-tools | tot | self-consistency | single-shot
    loop_termination_condition: max-iterations | goal-reached | error | token-budget | human-halt | none
    max_iterations: <int> | UNDECLARED
    iteration_count_citation: <file>:<line-range>
    agent_class_id: K-XX | NA
    citation: <file>:<line-range>
planning_pipelines:
  - id: PL-01
    workflow_id: W-XX
    kind: decompose | revise | execute | full
    planner_client_id: CL-XX
    executor_client_id: CL-XX | NA
    plan_schema: <json-schema-text>
    plan_schema_citation: <file>:<line-range>
    revision_trigger: error | observation | none
    max_revisions: <int> | UNDECLARED
    citation: <file>:<line-range>
memory_systems:
  - id: MEM-01
    workflow_id: W-XX
    kind: short-term | long-term-semantic | long-term-episodic | vector-store | external-service
    backend: in-process | redis | postgres | sqlite | faiss | chroma | pinecone | weaviate | qdrant | milvus | pgvector | lancedb | zep | mem0 | letta | custom
    embedding_client_id: CL-XX | NA
    read_fn: FN-XX
    write_fn: FN-XX
    retention_policy: ttl | window | summarization | permanent | UNDECLARED
    citation: <file>:<line-range>
vector_stores:
  - id: VS-01
    kind: faiss | chroma | pinecone | weaviate | qdrant | milvus | pgvector | lancedb | redisvl | astradb | mongodb-atlas | custom
    embedding_client_id: CL-XX
    dimension: <int> | UNVERIFIED
    distance_metric: cosine | l2 | ip | UNDECLARED
    citation: <file>:<line-range>
rag_pipelines:
  - id: RAG-01
    workflow_id: W-XX
    chunker: { strategy: fixed-size | recursive | sentence | paragraph | semantic | markdown-header | code-ast, chunk_size: <int>, chunk_overlap: <int>, citation: <file>:<line-range> }
    embedding_client_id: CL-XX
    vector_store_id: VS-XX
    retriever: { id: RT-XX, top_k: <int>, similarity_function: cosine | l2 | ip | mmr, filter_expression: <text> | none, citation: <file>:<line-range> }
    reranker: { id: RR-XX, kind: cross-encoder | llm-based | learned | hybrid, top_n_after_rerank: <int>, citation: <file>:<line-range> } | NA
    generation_workflow_id: W-XX
    variant: naive | hyde | rag-fusion | flare | self-rag | corrective-rag | NA
    citation: <file>:<line-range>
tools:
  - id: T-01
    name: <text>
    description: <verbatim-string>
    description_prompt_id: PR-XX
    parameters_schema: <json-schema-text>
    dispatch_fn: FN-XX
    return_type: D-XX | UNDECLARED
    error_handling: throws | returns-error-object | retries | none
    citation: <file>:<line-range>
tool_loops:
  - id: TL-01
    workflow_id: W-XX
    max_iterations: <int> | UNDECLARED
    parallel_tool_calls: true | false
    tool_choice_strategy: auto | required | specific-tool | none
    citation: <file>:<line-range>
reasoning_patterns:
  - id: RP-01
    workflow_id: W-XX
    pattern: cot | self-consistency | tot | reflection | decomposition | verbalized-confidence
    evidence_citation: <file>:<line-range>
    notes: <text>
llm_calls:
  - id: LC-01
    workflow_id: W-XX
    client_id: CL-XX
    parameters:
      model: <text>
      temperature: <float> | UNDECLARED
      top_p: <float> | UNDECLARED
      top_k: <int> | UNDECLARED
      max_tokens: <int> | UNDECLARED
      stop: [<string>] | UNDECLARED
      seed: <int> | UNDECLARED
      presence_penalty: <float> | UNDECLARED
      frequency_penalty: <float> | UNDECLARED
      response_format: <text> | UNDECLARED
      n: <int> | UNDECLARED
      stream: true | false | UNDECLARED
      tool_choice: <text> | UNDECLARED
      custom_parameters: { <key>: <value> }
    citation: <file>:<line-range>
streaming_usage:
  - id: ST-01
    call_id: LC-XX
    chunk_kind: token | message | character | sentence
    aggregation_strategy: concatenate | yield-immediately | buffer-window | none
    partial_response_handling: yield-partial | yield-only-final | reconstruct-on-done
    citation: <file>:<line-range>
coverage_cross_check:
  llm_call_sites_in_art09: [FN-XX]
  llm_call_sites_in_art21: [FN-XX]
  unaccounted_call_sites: [FN-XX]
  llm_boundary_types_in_art11: [D-XX]
  llm_boundary_types_in_art21: [D-XX]
  unaccounted_types: [D-XX]
  contradictions: [{ kind: <text>, entity: <id>, detail: <text> }]
mermaid_sources:
  - diagram_id: D-01
    title: <text>
    sidecar_file: <relative-path>
    node_count: <int>
source_coverage:
  - path: <file_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <file_path>:<line-range>
    symbol: <name>
sections:
  - id: S-01
    title: <string>
    claims: [C-XX]
---
```

### 8.2 ART-21 Body Skeleton

```markdown
# ART-21: AI / LLM Workflow Report

## 1. Executive Summary
## 2. Methodology
## 3. LLM-Client Integrations
## 4. Workflow Catalog
   ### 4.1 W-01: <name>
   **Diagram D-01: W-01 Pipeline**
   ```mermaid
   flowchart TD
       FN01[FN-01: entryPoint] --> PR01[PR-01: systemPrompt]
       PR01 --> LC01[LC-01: openai.chat]
       LC01 --> FN02[FN-01: parseResponse]
       FN02 --> SNK01[SNK-XX: output]
       %% edge: src/agent/workflow.ts:42
   ```
   <ordered stage list>
## 5. Prompt-Template Catalog
   ### 5.1 PR-01: <name> (kind, format)
   <verbatim body or REDACTED with rationale>
## 6. Agent-Architecture Catalog
## 7. Planning-Pipeline Catalog
## 8. Memory-System Catalog
## 9. RAG-Pipeline Catalog
## 10. Tool-Calling Catalog
## 11. Reasoning-Pattern Catalog
## 12. Model-Parameter Catalog
## 13. Streaming-Usage Catalog
## 14. Coverage Cross-Check
## 15. Traceability Index
## 16. Open Questions
## 17. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every LLM-client call site has a workflow or is recorded `UNACCOUNTED` with rationale. Threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of claims cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no workflow assertion contradicts ART-10's call graph or ART-11's data flows.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED`, `UNDECLARED`, `REDACTED`, and `LOOP_DETECTED` entry has a corresponding Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.2 on a 5% sample of workflows yields the same stage list.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-21.A. Prompt Exposure (HOOK-05)** — every prompt template is recorded with `template_body_verbatim` populated OR `template_body_verbatim: REDACTED` with `redaction_rationale`, `redaction_evidence`, and `redacted_length` all populated. A template with an empty body and no REDACTED marker is a `BLOCKING` finding.
- **Q-21.B. Pipeline Completeness** — every workflow has at least one `llm-call` stage and at least one `prompt-render` stage (the latter MAY be implicit when the prompt is a literal passed directly to the call; in that case the `prompt-render` stage's `function_id` is the same as the `llm-call` stage's `function_id`).
- **Q-21.C. Model-Parameter Recording** — every `LC-XX` records `model` (required) and at minimum `temperature`, `max_tokens`, `stream`, and `tool_choice` (each as a value or `UNDECLARED`).
- **Q-21.D. Agent-Loop Termination** — every agent (`architecture != single-shot`) records `loop_termination_condition`. `max_iterations` is required when the condition is `max-iterations`.
- **Q-21.E. Tool-Description Prompt Linkage** — every `T-XX` with a non-empty `description` has `description_prompt_id` populated and the referenced `PR-XX` exists in `prompt_templates`.
- **Q-21.F. RAG-Pipeline Stage Coverage** — every `RAG-XX` records a chunker, an embedding client, a vector store, and a retriever. Absent any stage, the stage is `NA` with an Open Question.
- **Q-21.G. Memory-System Backend Specificity** — every `MEM-XX` records a `backend` value from the enumerated set; `custom` requires an Open Question describing the custom backend.
- **Q-21.H. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment per `OUTPUT_RULES.md` § 7.5.
- **Q-21.I. Sidecar Files** — every Mermaid block has a corresponding `.mmd` file under `<output_root>/diagrams/`.
- **Q-21.J. Cross-Check Closure** — `unaccounted_call_sites` and `unaccounted_types` are either empty or each entry has a corresponding Open Question.

---

## 10. Common Pitfalls

- Do not paraphrase prompt templates; HOOK-05 requires verbatim reproduction or REDACTED. Paraphrasing is a `BLOCKING` finding per Q-21.A.
- Always record `UNDECLARED` for omitted model parameters rather than inferring defaults; defaults vary across vendors and versions, and inferring them violates R22.
- Do not collapse two LLM calls into one workflow because they share a client; workflows are bounded by call-graph connectivity through prompt-render, response-parse, and tool-dispatch edges.
- Always distinguish ReAct from tool-use; a workflow that uses OpenAI function-calling without `Thought:`/`Action:`/`Observation:` prompts is `tool-use`, not `react-with-tools`. The distinction informs PROMPT_24's engineering-decision analysis.
- Do not record a few-shot example as a single `PR-XX`; each example is its own `PR-XX` with `kind: few-shot`, because HOOK-05 verifies exposure per-template.
- Always trace the pipeline to its actual entry point per ART-10; starting the trace at an arbitrary intermediate function produces a partial workflow and inflates the workflow count.
- Do not omit local-model clients (`ollama`, `llama.cpp`, `vllm`); they are first-class LLM clients and their omission is a `COVERAGE_GAP`.
- Always record `redacted_length` for REDACTED templates; the length is the only verifiable property of a redacted body and PROMPT_28 uses it for consistency checks.
- Do not record `stream: true` calls without recording the corresponding `ST-XX` entry; the streaming flag and the streaming-usage record MUST be linked.
- Always cross-check the workflow's stages against ART-11's LLM-boundary flows; a stage that produces a type ART-11 did not classify as LLM-boundary is a real contradiction per R33.
- Do not include the embedding model as an `LC-XX` unless it is also used for generation; embedding-only clients are recorded under `vector_stores[].embedding_client_id` and are not LLM calls in the workflow-pipeline sense.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders the diagrams from the sidecar source.

---

## 11. Handoff Criteria

PROMPT_22, PROMPT_24, and PROMPT_25 consume ART-21. Handoff requires ALL of:

- HC-21.1: ART-21 status is `REVIEWED` or `DRAFT` with orchestrator waiver, OR `SKIPPED` per § 3.1 with downstream degradation declared.
- HC-21.2: Every LLM-client call site appears in at least one workflow OR is recorded `UNACCOUNTED` with an Open Question.
- HC-21.3: Every prompt template is verbatim or REDACTED with rationale (HOOK-05).
- HC-21.4: Every model parameter is recorded as a value or `UNDECLARED`.
- HC-21.5: Every agent records its `loop_termination_condition`.
- HC-21.6: Mermaid diagrams are emitted with `.mmd` sidecar files; diagrams > 30 nodes are decomposed.
- HC-21.7: Coverage cross-check is recorded with no unresolved contradictions.
- HC-21.8: `repository_fingerprint_recheck` matches ART-01.
- HC-21.9: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_22 (Streaming Workflow — consumes ART-21's `streaming_usage` to anchor the streaming-workflow producer/consumer analysis), PROMPT_24 (Engineering Decisions — consumes ART-21's agent architectures, RAG variants, and model choices as engineering-decision inputs), PROMPT_25 (Diagram Generation — re-renders the Mermaid sources at higher visual fidelity and embeds them in the architecture handbook), PROMPT_26 (Rebuild Guide — consumes ART-21's intelligence-layer reconstruction), PROMPT_28 (Cross-Reference Checklists — verifies HOOK-05 over ART-21's prompt templates).
- **Depends on:** ART-01 (PROMPT_01), ART-06 (PROMPT_06), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-11 (PROMPT_11), ART-16 (PROMPT_16).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33 (contradiction escalation between ART-21's workflow stages and ART-09/ART-11).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4 (sidecar files), § 3.1, § 4, § 6, § 7 (Mermaid conventions, edge citations, ≤ 30 nodes, decomposition).
- **HOOK enforcement:** HOOK-05 (Prompt Exposure) is enforced by Q-21.A; PROMPT_28 verifies HOOK-05 over ART-21 in the final cross-reference pass and PROMPT_30 re-verifies in the terminal QA pass.
- **Forward reference:** PROMPT_30 (Self-Review & QA) verifies that every `PR-XX` referenced by ART-26 (Rebuild Guide) resolves to an entry in ART-21, and that every `W-XX` referenced by ART-25 (Diagrams) resolves to an entry in ART-21.

*End of PROMPT_21. Orchestrator may dispatch PROMPT_22 upon satisfaction of § 11.*
