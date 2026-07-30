# 03-prompt-template-docs

In this framework, Prompts serve the role of classes and functions in a traditional codebase. The **Hermes Canonical Variant** utilizes 35 tightly coupled prompt functions.

## Class Equivalent: Core Configuration Documents
These documents define the state, constraints, and lifecycle of the agent's operation.

### `QUALITY_STANDARDS.md`
* **Purpose:** The global validation middleware. Enforces anti-hallucination rules (e.g., "never invent a function's purpose from its name alone").
* **State:** Immutable rule set.
* **Lifecycle:** Loaded into the LLM context at `Step 1` and referenced during `Step 5` (Validation) of every single phase execution.

### `PROMPT_DEPENDENCY_MAP.md`
* **Purpose:** The DAG (Directed Acyclic Graph) router.
* **Public API:** Provides the exact `Next()` function logic for the orchestrator, ensuring `PROMPT_11` is never executed before `PROMPT_04`.

## Function Equivalent: Phase Execution Prompts
Each execution prompt is a highly constrained function designed to extract specific intelligence without polluting the context window with irrelevant data.

### `MASTER_PROMPT.md`
* **Signature:** `execute_pipeline(target_repo_path) -> void`
* **Logic loop:**
  ```text
  For each Phase (1 to 9):
    1. Load specific Phase Prompt.
    2. Verify prerequisites from previous phase outputs.
    3. Execute the Phase System Prompt against the repo.
    4. Run Quality Gate (VALIDATION_CHECKLISTS.md).
    5. Generate a "Context Summary" for the next phase.
    6. Yield to the next Phase Prompt.
  ```
* **Side effects:** Modifies the AI's internal state (via the Context Summary) and generates markdown output files.

### `PROMPT_01` through `PROMPT_03` (Discovery Module)
* **Signature:** `discover(folder_tree, package_files) -> Inventory`
* **Purpose:** Establishes absolute ground truth before any deep analysis. Prevents the AI from hallucinating files that don't exist.

### `PROMPT_11` through `PROMPT_15` (Deep Code Analysis Module)
* **Signature:** `analyze_code(source_files, component_map) -> Flow_Diagrams`
* **Constraints:** Must use 10% spot-checks of claims against the source code. Cannot assume standard implementations (e.g., standard JWT).
* **Exception handling:** If logic is obscured or missing, the prompt forces the LLM to throw an `[UNVERIFIED - needs confirmation]` tag rather than guessing.

### `PROMPT_16` through `PROMPT_20` (AI Workflow Module)
* **Signature:** `analyze_ai(prompts, agent_code, tool_schemas) -> AI_Architecture`
* **Conditional Logic:** The `MASTER_PROMPT` checks the output of Phase 3. If no AI patterns were detected, `PROMPT_16-20` are skipped entirely to save compute and context space.
* **Outputs:** Tool catalogs, planning pipelines, memory/RAG schemas.
