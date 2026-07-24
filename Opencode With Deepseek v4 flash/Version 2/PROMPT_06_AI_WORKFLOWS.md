========================================================================
PROMPT 06: AI WORKFLOW ANALYSIS
========================================================================
Phase 6: AI, Agent, Prompt, and Automation Workflow Analysis
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. Complete understanding of all AI/agent architectures in the system
2. All prompt architectures documented with full prompt text
3. Agent workflow and orchestration logic documented
4. Tool-calling and function-calling architectures mapped
5. RAG (Retrieval-Augmented Generation) pipelines documented
6. Memory and context management strategies understood
7. Planning and reasoning pipeline architectures documented
8. Streaming and real-time AI interaction patterns documented

========================================================================
INPUTS
========================================================================

- FEATURE_MAP.md (from Phase 5)
- WORKFLOW_CATALOG.md (from Phase 5)
- DOMAIN_MODEL.md (from Phase 5)
- DATA_FLOW_DIAGRAMS.md (from Phase 4)
- EVENT_CATALOG.md (from Phase 4)
- MODULE_CATALOG.md (from Phase 3)
- All repository files

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 6.1: AI/AGENT ARCHITECTURE IDENTIFICATION

6.1.1. Determine if the system contains AI/agent components:
    - LLM integration points
    - AI agent frameworks (LangChain, CrewAI, AutoGPT, etc.)
    - Custom agent implementations
    - Prompt-based decision making
    - AI-powered features and capabilities

6.1.2. If AI/agent components exist, document the overall
    AI architecture:
    - Agent hierarchy (single agent, multi-agent, supervisor)
    - Agent roles and responsibilities
    - Agent communication patterns
    - Agent lifecycle management
    - Agent state persistence
    - Human-in-the-loop patterns

6.1.3. Generate a Mermaid agent architecture diagram.

ACTIVITY 6.2: PROMPT ARCHITECTURE EXTRACTION

6.2.1. Find all prompt definitions in the system:
    - System prompts
    - User prompts templates
    - Few-shot examples
    - Instruction templates
    - Prompt templates (Handlebars, Mustache, f-strings, etc.)
    - Embedded prompts in code

6.2.2. For every distinct prompt, document:
    - Prompt purpose and trigger
    - Full prompt text (exact content)
    - Template variables and their sources
    - Prompt engineering techniques used
      (chain-of-thought, few-shot, role-playing, etc.)
    - Token count estimation
    - Model targeted (GPT-4, Claude, local model, etc.)
    - Temperature and other parameters
    - Output format instructions
    - Fallback/alternative prompts

6.2.3. Analyze prompt effectiveness:
    - Are there guardrails/constraints?
    - Is there output validation?
    - Are there retry/recovery mechanisms?
    - Is there prompt injection protection?
    - Are there prompt chaining patterns?

6.2.4. Generate prompt dependency map showing:
    - Which prompts call which other prompts
    - How prompt outputs feed into subsequent prompts
    - Decision points based on prompt outputs

ACTIVITY 6.3: AGENT WORKFLOW ORCHESTRATION

6.3.1. Document the agent workflow/orchestration:
    - Step sequencing and ordering
    - Conditional branching based on LLM outputs
    - Loops and iteration patterns
    - Parallel agent execution
    - Agent handoff patterns
    - Timeout and retry strategies
    - Error recovery flows

6.3.2. For each agent workflow step, document:
    - Step purpose
    - Input data
    - AI model/agent used
    - Tools/functions available
    - Expected output
    - Validation criteria
    - Fallback behavior on failure

6.3.3. Generate Mermaid sequence diagrams for agent workflows.

6.3.4. Generate Mermaid flowcharts for orchestration logic.

ACTIVITY 6.4: TOOL/FUNCTION CALLING ANALYSIS

6.4.1. Identify all tools/functions available to AI agents:
    - Function/tool definitions
    - Tool parameters and return types
    - Tool descriptions (how agents discover tools)
    - Tool registration mechanisms
    - Tool execution environment

6.4.2. For each tool/function, document:
    - Tool name and purpose
    - Tool definition (schema/signature)
    - Implementation location
    - When/how the tool is invoked
    - Authentication/authorization required
    - Rate limiting and throttling
    - Error handling
    - Logging/monitoring

6.4.3. Map tool usage patterns:
    - Which agents use which tools
    - Tool calling frequency (if determinable)
    - Tool composition (tools calling tools)
    - Tool output processing

ACTIVITY 6.5: RAG PIPELINE ANALYSIS

6.5.1. If the system implements RAG, document the pipeline:
    - Document ingestion pipeline
    - Chunking strategy and parameters
    - Embedding model and configuration
    - Vector database and index configuration
    - Retrieval strategy (semantic, hybrid, keyword)
    - Retrieval parameters (top-k, score threshold)
    - Reranking strategy
    - Context window management
    - Query transformation (rewriting, expansion)

6.5.2. For each RAG component, document:
    - Location in the codebase
    - Configuration options
    - Dependencies (libraries, models, services)
    - Performance characteristics

6.5.3. Generate Mermaid RAG pipeline diagram.

ACTIVITY 6.6: MEMORY AND CONTEXT MANAGEMENT

6.6.1. Identify all memory/context management:
    - Conversation history management
    - Sliding window strategies
    - Summarization-based compression
    - External memory stores (vector DB, key-value)
    - Ephemeral vs. persistent memory
    - Memory retrieval mechanisms

6.6.2. For each memory mechanism, document:
    - What is stored
    - Storage format
    - Storage location
    - Retrieval mechanism
    - Expiration/eviction policy
    - Context window budget allocation

ACTIVITY 6.7: PLANNING AND REASONING PIPELINES

6.7.1. Identify planning/reasoning architectures:
    - Chain-of-Thought (CoT) implementations
    - Tree-of-Thought (ToT) implementations
    - ReAct (Reasoning + Acting) patterns
    - Plan-and-Execute patterns
    - Reflection/self-critique patterns
    - Multi-step reasoning pipelines

6.7.2. For each reasoning architecture, document:
    - Implementation location
    - Step-by-step reasoning process
    - How intermediate results are stored
    - How the final answer is derived
    - Error recovery during reasoning
    - Maximum reasoning depth/iterations

6.7.3. Generate Mermaid reasoning flow diagrams.

ACTIVITY 6.8: STREAMING AND REAL-TIME AI PATTERNS

6.8.1. Identify streaming patterns:
    - Token-by-token streaming
    - Server-Sent Events (SSE)
    - WebSocket-based streaming
    - Chunked responses

6.8.2. For each streaming pattern, document:
    - Streaming protocol/mechanism
    - Client-side handling
    - Server-side implementation
    - Error handling during stream
    - Cancellation/abort patterns
    - Backpressure handling

ACTIVITY 6.9: AI SAFETY AND GUARDRAILS

6.9.1. Identify safety mechanisms:
    - Output validation and filtering
    - Content moderation
    - Prompt injection detection
    - Rate limiting per user/session
    - Cost management (token budgets)
    - Model fallback chains
    - Human approval gates

6.9.2. For each safety mechanism, document:
    - Implementation location
    - Trigger conditions
    - Action taken
    - Bypass mechanisms (if any)
    - Logging and alerting

========================================================================
ANALYSIS METHODOLOGY
========================================================================

This phase requires DeepScan methodology for all AI-related code.

AI systems often have complex, non-linear execution paths.
Use these strategies:

1. TRACE PROMPT EXECUTION: Find where prompts are constructed,
   sent to models, and how responses are processed.

2. FOLLOW THE CHAIN: For chain-based systems (LangChain, etc.),
   trace the entire chain from first prompt to final output.

3. MAP TOOL ACCESS: Document every tool available to every agent
   and how they are invoked.

4. RECONSTRUCT DECISION LOGIC: AI systems often use LLM outputs
   to make decisions. Document how these decisions are parsed
   and acted upon.

5. SEPARATE STATIC FROM DYNAMIC: Document which parts of prompts
   are static and which are dynamically generated.

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT 6.1: AI_ARCHITECTURE_OVERVIEW.md
- Agent architecture diagram (Mermaid)
- Agent role inventory
- Agent communication patterns

ARTIFACT 6.2: PROMPT_CATALOG.md
- Every prompt with full text
- Prompt engineering analysis
- Prompt dependency map
- Token budget documentation

ARTIFACT 6.3: AGENT_WORKFLOWS.md
- Orchestration documentation
- Mermaid sequence diagrams
- Mermaid flowcharts
- Error recovery patterns

ARTIFACT 6.4: TOOL_CATALOG.md
- Tool inventory with definitions
- Tool-agent mapping
- Tool usage patterns

ARTIFACT 6.5: RAG_PIPELINE.md (if applicable)
- Complete RAG pipeline documentation
- Chunking/embedding/retrieval details
- Mermaid RAG diagram

ARTIFACT 6.6: MEMORY_CONTEXT.md
- Memory mechanism inventory
- Context management strategy
- Storage and retrieval details

ARTIFACT 6.7: REASONING_PIPELINES.md
- Reasoning architecture documentation
- Mermaid reasoning flow diagrams
- Step-by-step process documentation

ARTIFACT 6.8: STREAMING_PATTERNS.md
- Streaming mechanism documentation
- Client/server implementation
- Error handling during streaming

ARTIFACT 6.9: SAFETY_GUARDRAILS.md
- Safety mechanism inventory
- Implementation documentation
- Gap analysis

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] All AI/agent components are identified.
[ ] Every prompt is extracted with full text.
[ ] Agent workflows have sequence diagrams.
[ ] All tools/functions are cataloged with definitions.
[ ] RAG pipeline is fully documented (if present).
[ ] Memory/context management is documented.
[ ] Reasoning pipelines are documented with diagrams.
[ ] Streaming patterns are documented.
[ ] Safety mechanisms are cataloged.
[ ] Artifacts meet quality standards (score >= 4.0).

========================================================================
OUTPUTS TO NEXT PHASE
========================================================================

Pass to Phase 7:
- AI_ARCHITECTURE_OVERVIEW.md
- TOOL_CATALOG.md

Pass to Phase 9:
- All artifacts from this phase

========================================================================
END OF PROMPT 06
========================================================================
