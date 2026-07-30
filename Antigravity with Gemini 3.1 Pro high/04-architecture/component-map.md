# Component Map

* **Infrastructure Component:** Owns the rules, guidelines, and definitions (e.g., `MISSION.md`, `QUALITY_STANDARDS.md`). Ensures the LLM stays within bounds.
* **Orchestrator Component:** Owns the sequence of execution. Loads rules into context and fires off the phase prompts in order (`MASTER_PROMPT.md`).
* **Phase Components:** Owns the specific extraction logic for each segment of reverse engineering.
  * *Discovery Phase:* `PROMPT_01` (Repository Intelligence)
  * *Structural Phase:* `PROMPT_02` (File & Folder Analysis), `PROMPT_03` (Unit/Class/Function Docs)
  * *Architectural Phase:* `PROMPT_04` (Architecture), `PROMPT_05` (Diagrams)
  * *Systems & Workflows Phase:* `PROMPT_06` (AI Workflows), `PROMPT_07` (Tech Stack), `PROMPT_08` (Conditional Docs)
  * *Synthesis & Validation Phase:* `PROMPT_09` (Rebuild Guide), `PROMPT_10` (Final QA)
