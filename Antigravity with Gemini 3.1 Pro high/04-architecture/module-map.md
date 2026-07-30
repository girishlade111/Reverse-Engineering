# Module Map

Since the repository is a collection of Markdown prompts rather than executable source code, the dependency graph represents "Information Dependency" rather than "Import Dependency".

* `MASTER_PROMPT.md` depends on ALL Layer 1 files (`MISSION.md`, `OPERATING_RULES.md`, etc.).
* `PROMPT_09_DEVELOPER_HANDBOOK_REBUILD.md` depends on the output artifacts of `PROMPT_01` through `PROMPT_08`.
* `PROMPT_05_DIAGRAMS.md` depends heavily on the output of `PROMPT_03` and `PROMPT_04`.
* `PROMPT_10_VALIDATION_QA.md` depends on ALL generated output artifacts and `QUALITY_STANDARDS.md`.

*Circular Dependencies:* None detected. The pipeline is strictly unidirectional (Phases 1 → 10).
