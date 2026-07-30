# 07-tech-stack

## Language Analysis
* **Primary Language:** Markdown (`.md`)
* **Rationale:** Markdown is universally understood by all LLMs and provides structured, plain-text formatting (headers, bolding, lists, code blocks) that maximizes LLM token efficiency and comprehension without requiring compilation.

## Framework Analysis
* **Framework:** Enterprise Reverse Engineering Prompt Framework (Custom/Internal)
* **Configuration:** This framework defines its own constraints via specialized markdown files like `QUALITY_STANDARDS.md` and `OPERATING_RULES.md`.

## Package / Dependency Analysis
This repository does not use traditional package managers (npm, pip, cargo). The "dependencies" are the capabilities required of the runtime environment:
1. **Large Language Model:** Must support large context windows (to absorb the entire framework + the target repo) and robust instruction-following capabilities.
2. **Execution Environment:** Must be an agentic IDE or CLI (e.g., Cursor, Claude Code, Opencode) capable of reading files from disk automatically based on prompt instructions.

## Tech Stack Summary Table

| Layer | Technology | Viable Alternatives for Rebuild | Trade-off of Alternative |
|-------|------------|---------------------------------|--------------------------|
| Documentation Standard | Markdown (GitHub Flavored) | ReStructuredText (.rst) | `.rst` is less universally supported by default LLM parsers. |
| Diagramming | Mermaid.js | PlantUML | Mermaid is natively rendered in GitHub and natively supported by most LLMs out-of-the-box. |
| Runtime Environment | Human-in-loop Agent (Cursor/Claude Code) | Fully Autonomous Script (LangChain/Python) | A python script would allow programmatic scaling, but removes the human architect from the loop, risking lower quality on ambiguous edge cases. |
