# PHASE 5 — DIAGRAMS

## Objective
Visualize what Phase 4 documented in prose — diagrams must be derived from, not a substitute for, the architecture docs.

## Steps
Produce as many of the following as the codebase supports evidence for (skip with reason if genuinely not applicable):
1. Call Graph — function-level, for the 2–3 most important flows identified in Phase 4
2. Dependency Graph — module/package level, consistent with Phase 4's module map
3. Sequence Diagrams — one per major user flow / API request / background job
4. Component Diagram
5. State Diagram — for any state machine or status field with defined transitions
6. ER Diagram — only if a database is present
7. UML Class Diagrams — for core domain classes documented in Phase 3

Every diagram: valid Mermaid syntax in a fenced code block, with a 2–3 sentence caption explaining what it shows and why it matters for rebuilding.

## Required Outputs
- `05-diagrams.md` (or split per app/service if monorepo) with all applicable diagrams

## Validation Checklist
- [ ] Every diagram mentally re-parsed as valid Mermaid syntax
- [ ] Every diagram traces to a flow/relationship actually documented in Phase 3 or 4 — no decorative diagrams
- [ ] Skipped diagram types have an explicit one-line reason (e.g., "ER Diagram — N/A, no database in this repo")
