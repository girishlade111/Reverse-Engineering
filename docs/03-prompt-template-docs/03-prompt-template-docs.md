# 03-prompt-template-docs

In this repository, "Prompts" and "Templates" serve the role of classes and functions in a traditional codebase. They are the modular, single-responsibility units that instruct an LLM to execute a specific task.

## Class Equivalent: Core Configuration Documents
These documents define the state, constraints, and lifecycle of the agent's operation.

### `MISSION.md`
* **Purpose:** Defines the overarching goal and the definition of "Done" for the agent.
* **State it owns:** The terminal condition of the reverse-engineering task.
* **Public API surface:** Read by the agent at initialization.

### `QUALITY_STANDARDS.md`
* **Purpose:** Enforces anti-hallucination rules and sets the baseline for output acceptable quality.
* **Inheritance/Composition:** Inherited/composited into all downstream execution phases to govern how they generate output.
* **Lifecycle:** Active throughout the entire execution lifetime.

### `OPERATING_RULES.md`
* **Purpose:** Governs agent pacing, context continuation rules, and ambiguity handling (when to pause and ask the user).

## Function Equivalent: Phase Execution Prompts
These files take inputs, perform logic, and produce outputs (side effects).

### `MASTER_PROMPT.md`
* **Signature:** `execute(target_repo, framework_files) -> void`
* **Purpose:** The orchestrator function. It loads the configuration and iteratively calls each Phase Prompt.
* **Step-by-step logic:**
  1. Load Mission, Operating Rules, Quality Standards.
  2. Start iterating over Phase 1 through Phase 9 (or 10).
  3. Pause for user feedback only if an ambiguity is unresolvable.
* **Side effects:** Triggers the execution of all other prompts.
* **Called-by:** The human operator (User).
* **Calls-into:** `PROMPT_01` through `PROMPT_10`.

### `PROMPT_01_REPOSITORY_INTELLIGENCE.md` (and equivalents)
* **Signature:** `analyze_root(directory_tree) -> 01-repository-intelligence.md`
* **Purpose:** Extracts top-level ground truth, stacks, and hypotheses.
* **Outputs / Side effects:** Generates the `01-repository-intelligence.md` artifact.

### `PROMPT_02` through `PROMPT_08`
* **Signature:** `analyze_domain(codebase_files) -> [02-08]-*.md`
* **Purpose:** Depth-first unit analysis for files, architectures, diagrams, and AI workflows.
* **Error/Exception behavior:** If data is missing, tags as `[UNVERIFIED - needs confirmation]` and logs in Open Questions rather than failing or guessing.

### `PROMPT_09_DEVELOPER_HANDBOOK_REBUILD.md`
* **Signature:** `synthesize(all_previous_docs) -> 09-developer-handbook-rebuild-guide.md`
* **Purpose:** Compiles all prior phase artifacts into a sequential rebuild guide.
* **Calls-into:** Reads from outputs of Phases 1-8.

### `PROMPT_10_VALIDATION_QA.md`
* **Signature:** `audit(all_docs) -> final_validation_report`
* **Purpose:** Self-audits the generated docs against the `QUALITY_STANDARDS.md` and generates a final report appended to `00-INDEX.md`.
