# MASTER REVERSE ENGINEERING PROMPT

### (Paste this into Claude Code / VS Code, pointed at any repo, to fully reconstruct it for rebuild-from-scratch)

---

## 0. MISSION

You are a Senior Staff Software Architect performing a **full reverse-engineering audit** of this repository. Your job is NOT to summarize the code. Your job is to reconstruct a documentation set so complete and precise that an experienced engineering team could **rebuild an equivalent product from scratch**, using only your documentation + publicly available packages/frameworks, without ever seeing the original source again.

Treat this as a real engineering deliverable, not a casual walkthrough. Be exhaustive, be structured, be precise. Do not skip files because they "look simple" — simple files often carry critical config or business rules.

**Ground rules:**

- Never fabricate logic you haven't actually read in the code. If something is unclear, mark it `[UNVERIFIED — needs confirmation]` and add it to the Follow-up Questions list at the end. Guessing wrong logic is worse than admitting a gap.
- Every claim about "why" (design intent) must be inferable from code comments, naming, structure, or config — not invented.
- This is a **rebuild guide**, so bias toward _actionable_ documentation over prose description.

---

## 1. PHASE 0 — REPOSITORY INTELLIGENCE

Before diving deep, produce a top-level intelligence report:

- Full folder tree (depth-limited to meaningful levels, collapse `node_modules`/`vendor`/build artifacts)
- Monorepo detection: list every distinct app/package/service found, and how they relate (shared libs, workspace tooling — nx/turborepo/lerna/pnpm workspaces/etc.)
- Per-stack breakdown: language(s), framework(s), runtime versions (from lockfiles/config), package manager
- Entry points for each app/service (main files, index files, server bootstrap files)
- Build & tooling setup (bundlers, linters, CI config if present)
- A one-paragraph "what does this system do" hypothesis — to be refined later, not treated as final

---

## 2. PHASE 1 — FILE-BY-FILE & FOLDER-BY-FOLDER ANALYSIS

For every folder:

- Purpose of the folder within the system
- List of files with one-line purpose each

For every non-trivial file:

- Purpose
- Key exports (functions/classes/components/routes)
- Inputs it depends on (imports, env vars, config)
- Outputs/side effects (writes to DB, calls API, renders UI, emits events)
- Notable edge cases or error handling patterns

Group this output logically per app/package if monorepo (don't interleave unrelated stacks).

---

## 3. PHASE 2 — FUNCTION & CLASS DOCUMENTATION

For every function/method of real significance (skip pure trivial getters/setters unless they hide logic):

- Signature (params + types + return type)
- Purpose in plain terms
- Step-by-step internal logic (numbered)
- Side effects / mutations
- Error/exception behavior
- Called by / calls into (link to call graph)

For every class:

- Responsibility (single-responsibility statement)
- State it owns
- Public API surface
- Inheritance/composition relationships
- Lifecycle (constructed when, destroyed/cleaned up when)

---

## 4. PHASE 3 — ARCHITECTURE RECONSTRUCTION

Produce:

- **System Design Doc**: layered view (presentation / application / domain / infra), with responsibilities per layer
- **Component Map**: every major component/module and what it owns
- **Module Map**: dependency direction between modules (who imports whom)
- **Working Logic Documentation**: how a request/action actually flows through the system end-to-end, in prose + diagram
- **Business Logic Documentation**: the _domain rules_ — validation rules, pricing/permission/state-transition rules, anything that encodes "how the business actually works," separated clearly from technical plumbing

---

## 5. PHASE 4 — DIAGRAMS (Mermaid, all diagrams must be valid Mermaid syntax)

Produce as many of these as the codebase supports evidence for:

- **Call Graph** (function-level, for the 2–3 most important flows)
- **Dependency Graph** (module/package level)
- **Sequence Diagrams** (per major user flow / API request / background job)
- **Component Diagram**
- **State Diagram** (for any state machine / status field with transitions)
- **ER Diagram** (if a database is present)
- **UML Class Diagrams** (for core domain classes)

Each diagram must have a 2–3 sentence caption explaining what it shows and why it matters for rebuilding.

---

## 6. PHASE 5 — AI AGENT WORKFLOW ANALYSIS (only if the repo contains an LLM/agent system — skip section entirely otherwise)

If any AI/agent/LLM orchestration code exists, document:

- **Prompt Flow**: every system prompt / prompt template found, what variables it injects, and where in the flow it fires
- **Reasoning Flow**: how the agent decides what to do next (planner loop, ReAct pattern, state machine, etc.)
- **Planning Flow**: task decomposition logic if present
- **Tool Calling Flow**: full list of tools/functions exposed to the model, their schemas, and the dispatch logic
- **Memory Flow**: what gets persisted, where, retrieval triggers, eviction/compression logic
- **RAG Flow**: embedding model, vector store, chunking strategy, retrieval-to-prompt injection path

Mark clearly which parts are deterministic code vs. model-driven behavior.

---

## 7. PHASE 6 — TECH STACK DOCUMENTATION

- **Language Analysis**: languages used, versions, why (inferred from tsconfig/pyproject/etc.)
- **Framework Analysis**: frameworks + version + how they're configured (not default docs — this repo's specific config)
- **Package/Dependency Analysis**: every direct dependency, what it's used for in THIS repo (not generic description), and whether it's load-bearing or replaceable
- **Tech Stack Summary Table**: layer → technology → alternative options for rebuild

---

## 8. PHASE 7 — CONDITIONAL DOCS (include only if present in repo, state "N/A — not present" otherwise)

- **API Documentation**: every route/endpoint, method, auth requirement, request/response shape, status codes, rate limits if visible
- **Database Documentation**: schema, relationships, indexes, migrations history if present
- **Authentication Documentation**: auth strategy (session/JWT/OAuth), token lifecycle, permission/role model
- **Deployment Documentation**: how it's built/deployed (Docker, CI/CD, hosting config found in repo)
- **Environment Documentation**: every env var found, what it controls, required vs optional

---

## 9. PHASE 8 — DEVELOPER HANDBOOK & COMPLETE REBUILD GUIDE

This is the final synthesis deliverable. It must let someone rebuild the product from zero:

1. **Rebuild Order**: step-by-step sequence (e.g., "1. scaffold X framework, 2. set up DB schema, 3. build auth, 4. build core domain models, 5. build API layer, 6. build UI, 7. wire AI/agent layer, 8. deploy")
2. **Feature Checklist**: every user-facing feature found, described as a rebuildable spec (not code) — trigger, behavior, edge cases
3. **Non-obvious gotchas**: anything found in code (workarounds, hacks, weird config) a rebuilder MUST know to avoid breaking things
4. **What to do differently**: if you (the documenting AI) spot clear architectural smells, note them as "known debt — consider rebuilding this part differently" with a brief alternative

---

## 10. OUTPUT STRUCTURE RULES

- Decide the best file/folder structure for the docs yourself based on repo size and monorepo complexity (small repo → fewer files; large monorepo → one folder per app/service + shared cross-cutting docs). Always include:
  - `00-INDEX.md` — table of contents with links to every doc file produced, plus a one-line summary of each
  - Clear, consistent file naming (numbered prefixes for reading order)
- Every doc file must have cross-references (links) to related files where relevant (e.g., a function doc referencing the sequence diagram it appears in)
- Use Mermaid code blocks (` ```mermaid `) for every diagram — no ASCII art, no external image tools

---

## 11. AUTOMATIC CONTINUATION RULES

- If the repo is large enough that output would be truncated, STOP at a clean section boundary, state clearly: `--- CONTINUING IN NEXT MESSAGE: [next section name] ---`, and continue automatically in your next reply without waiting for a "continue" prompt.
- Never restart already-completed sections. Keep a running mental index of what's done.
- If you hit genuine ambiguity that blocks progress (not just missing detail, but a fork where guessing wrong corrupts the doc), pause and ask a **single, specific** follow-up question rather than guessing.

---

## 12. FOLLOW-UP QUESTIONS (you generate this list live as you work)

Maintain a running `## Open Questions` section appended to `00-INDEX.md`, logging every `[UNVERIFIED]` item and any design-intent ambiguity you couldn't resolve from code alone.

---

## 13. QUALITY RULES & VALIDATION CHECKLIST (self-check before declaring any phase done)

Before marking a phase complete, verify:

- [ ] Every file in the repo has been accounted for (either documented or explicitly marked "excluded — build artifact/vendor code")
- [ ] Every diagram uses valid Mermaid syntax (mentally re-parse it)
- [ ] No invented business logic — everything traces to actual code
- [ ] Rebuild Guide alone (without reading other docs) gives enough sequencing to start building
- [ ] Cross-references actually point to real section headers
- [ ] Tone is engineering-precise, not marketing/prose-heavy

---

## 14. START INSTRUCTION

Begin with **Phase 0 — Repository Intelligence** now. Do not ask permission to proceed between phases — move through them in order, applying the Continuation Rules if truncation risk appears. Only pause for the single-question exception described in Section 11.
