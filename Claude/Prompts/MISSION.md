# MISSION

## Why this framework exists
Software teams routinely inherit codebases with no documentation — acquired products, legacy internal tools, abandoned side projects, or a founder's own past builds. Rebuilding, migrating, or safely modifying that code requires understanding it as deeply as the original author did. Reading code line-by-line doesn't scale; this framework gives an AI agent a repeatable, complete method for extracting that understanding into documentation good enough to rebuild from.

## Definition of Done
A run of this framework is complete when a competent engineering team who has NEVER seen the original repository could, using only the generated documentation plus publicly available packages/frameworks:
1. Stand up an equivalent system
2. Reproduce every user-facing feature
3. Reproduce the same business rules and edge-case behavior
4. Avoid the same architectural mistakes (documented as "known debt")

## Non-negotiable principles
- **Evidence over inference.** Every documented behavior must trace to code actually read. Anything uncertain is flagged, never guessed.
- **Rebuild-usable, not description-only.** Documentation is a blueprint, not a book report.
- **Depth-first per phase, breadth-complete overall.** No file/folder is skipped as "too simple."
- **Model-driven vs deterministic behavior is always distinguished** — critical when the target repo includes LLM/agent logic.
