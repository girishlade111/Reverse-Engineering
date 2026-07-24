# MASTER INDEX — Enterprise Reverse Engineering Prompt Framework

This project is a **modular, reusable prompt framework**. It does not document any repository itself — it is the set of instructions you hand to an AI coding agent (Claude Code, Cursor, etc.) so THAT agent can reverse-engineer any target repository to rebuild-grade fidelity.

## How to use this framework
1. Read `MISSION.md` and `PROJECT_SPECIFICATION.md` first — they set the contract.
2. Feed `MASTER_PROMPT.md` to the agent as the top-level instruction. It internally references every `PROMPT_XX_*.md` file as a phase.
3. The agent executes phases in order (01 → 10), following `OPERATING_RULES.md`, `QUALITY_STANDARDS.md`, and `OUTPUT_RULES.md` throughout.
4. `PROMPT_DESIGN_GUIDE.md` explains WHY the framework is structured this way — read it if you want to extend the framework with new phases later.

## File map

| File | Purpose |
|---|---|
| MISSION.md | Why this framework exists, what "done" means |
| PROJECT_SPECIFICATION.md | Scope, constraints, non-goals |
| OPERATING_RULES.md | How the agent should behave turn-to-turn (continuation, pacing, ambiguity handling) |
| QUALITY_STANDARDS.md | Bar for acceptable output; anti-hallucination rules |
| OUTPUT_RULES.md | File/folder naming, structure, cross-referencing conventions |
| PROMPT_DESIGN_GUIDE.md | Design rationale + how to extend the framework |
| MASTER_PROMPT.md | The single top-level prompt that kicks off the whole run |
| PROMPT_01_REPOSITORY_INTELLIGENCE.md | Phase 1 — top-level scan, stack detection |
| PROMPT_02_FILE_FOLDER_ANALYSIS.md | Phase 2 — file-by-file, folder-by-folder |
| PROMPT_03_FUNCTION_CLASS_DOCS.md | Phase 3 — function/class-level documentation |
| PROMPT_04_ARCHITECTURE_RECONSTRUCTION.md | Phase 4 — system/component/module architecture |
| PROMPT_05_DIAGRAMS.md | Phase 5 — all Mermaid/UML diagram generation |
| PROMPT_06_AI_AGENT_WORKFLOW.md | Phase 6 — LLM/agent-specific analysis (conditional) |
| PROMPT_07_TECH_STACK.md | Phase 7 — language/framework/package analysis |
| PROMPT_08_CONDITIONAL_DOCS.md | Phase 8 — API/DB/Auth/Deploy/Env (conditional) |
| PROMPT_09_DEVELOPER_HANDBOOK_REBUILD.md | Phase 9 — final synthesis: rebuild guide |
| PROMPT_10_VALIDATION_QA.md | Phase 10 — self-audit before declaring done |

## Open Questions Log
Maintained live by the executing agent during a real run — not pre-filled here since this is the framework, not an actual repo analysis.
