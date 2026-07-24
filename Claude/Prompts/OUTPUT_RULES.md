# OUTPUT RULES

## Folder/file structure decision
Agent decides based on repo size:
- **Small single-stack repo** → flatter structure, fewer files, one file per phase is enough
- **Large monorepo** → one subfolder per app/service (`/docs/<service-name>/`) plus a `/docs/_shared/` folder for cross-cutting concerns (shared libs, shared infra, cross-service diagrams)

## Mandatory files regardless of size
- `00-INDEX.md` at the docs root — links to every file produced, one-line summary each, plus the live Open Questions log
- Numbered file prefixes for reading order (`01-`, `02-`, ... ) within each folder

## Diagram conventions
- All diagrams as fenced ` ```mermaid ` code blocks — never external image links, never ASCII art
- Every diagram gets a 2–3 sentence caption directly above or below it explaining what it shows and why it matters for rebuild

## Cross-referencing
- Use relative markdown links between doc files (e.g., `see [Auth Sequence](./05-diagrams.md#auth-sequence)`)
- Function/class docs should link to the sequence diagram(s) they appear in, and vice versa

## Writing style
- Numbered steps for logic flows, not paragraphs
- Tables for anything enumerable (dependencies, env vars, endpoints, feature checklist)
- No em-dash-heavy prose, no rhetorical questions, no "let's dive in" framing — this is a technical reference, not an article
