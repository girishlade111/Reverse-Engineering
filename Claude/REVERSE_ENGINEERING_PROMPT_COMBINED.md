# ENTERPRISE REVERSE ENGINEERING PROMPT FRAMEWORK
### (Single-file combined version — paste entire doc into Claude Code / VS Code, pointed at target repo)

---


---

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


---

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


---

# PROJECT SPECIFICATION

## In scope
- Any single-stack repository (web app, mobile app, backend service, CLI tool, AI agent/backend)
- Any monorepo containing multiple apps/services/packages, including mixed-language monorepos
- Repositories that include LLM/agent orchestration code (prompts, tool-calling, memory, RAG)
- Repositories of any size — framework includes explicit continuation rules for large codebases

## Out of scope (explicitly)
- This framework does NOT execute or deploy the target code
- This framework does NOT attempt to fix bugs found in the target repo (only documents them as "known debt")
- This framework does NOT reproduce copyrighted comments, license text, or proprietary content verbatim beyond what's needed for accurate technical description
- This framework assumes the operator has legitimate access to and rights over the target repository

## Inputs required from the operator before a run
- Path/URL to the target repository
- Purpose of the run (rebuild / documentation-only / migration / learning) — changes emphasis in Phase 9
- Preferred output structure, or "let the agent decide" (default: agent decides based on repo size)

## Deliverables of one full run
- A complete `/docs` folder (structure decided in OUTPUT_RULES.md) covering Phases 1–10
- A single `00-INDEX.md` entry point
- A running Open Questions log


---

# OPERATING RULES — Turn-to-Turn Agent Behavior

## Execution order
Execute phases 01 → 10 strictly in order. Do not skip ahead. Do not ask for permission between phases — only pause per the Ambiguity Rule below.

## Continuation Rule (large repos)
If a response risks truncation:
1. Stop at a clean section boundary (never mid-table, mid-diagram, or mid-function-doc)
2. Emit exactly: `--- CONTINUING IN NEXT MESSAGE: [next section name] ---`
3. Resume automatically in the next response with no re-greeting, no re-summary of what's done
4. Maintain an internal "completed sections" ledger so nothing is redone and nothing is skipped

## Ambiguity Rule (when to actually stop and ask)
Distinguish two situations:
- **Missing detail** (e.g., a variable's exact runtime value isn't visible in code) → do NOT stop. Mark `[UNVERIFIED — needs confirmation]` inline and log it in Open Questions. Keep moving.
- **Blocking fork** (e.g., two plausible architectural interpretations that would produce contradictory downstream documentation) → STOP, ask one single specific question, wait for the answer, then proceed.

## Pacing
Depth over speed. A thorough Phase 2 that takes many responses beats a rushed one-shot summary. The operator would rather wait than rebuild from wrong documentation.

## Tone
Engineering-precise. No marketing language, no hype adjectives, no filler transitions. Write like an internal engineering wiki maintained by senior staff engineers.


---

# QUALITY STANDARDS

## Anti-hallucination rules (hard constraints)
1. Never invent a function's purpose from its name alone — read the body.
2. Never invent business rules — every rule documented must quote (paraphrased, not copy-pasted verbatim) the actual condition/branch found in code.
3. Never assume a "standard" implementation (e.g., "probably uses JWT the normal way") — verify from actual auth code.
4. If a dependency's usage in-repo doesn't match its typical usage, document the ACTUAL usage, not the typical one.
5. Every `[UNVERIFIED]` tag must be resolved or explicitly carried into Open Questions — never silently dropped.

## Completeness bar
- Every file in the repo is accounted for: documented, or explicitly listed under "Excluded — build artifact / vendor / generated code" with the reason.
- Every diagram must be re-parseable Mermaid syntax — mentally validate before finalizing.
- Every phase's output must stand alone (a reader opening only that phase's doc file should understand it without needing to have read the others first, though cross-references are encouraged).

## Definition of "good enough to rebuild from"
Ask: "If I deleted the original repo right now and only had this documentation, could I start coding today with zero clarifying questions to the original author?" If no — the documentation isn't done.

## Common failure modes to avoid
- Documentation that reads like a code comment restated in English (low value)
- Diagrams that are decorative rather than derived from actual call/data flow
- Skipping "boring" config files that actually gate critical behavior (feature flags, env-based branching)
- Conflating "what the code does" with "what the code was probably meant to do" — document the former; note discrepancies as known debt


---

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


---

# PROMPT DESIGN GUIDE — Rationale & Extension Guide

## Why 10 phases, in this order
1–2 establish ground truth (what literally exists) before any interpretation happens.
3 moves from "what exists" to "what it does" at the unit level.
4–5 zoom out to system-level understanding once units are understood — architecture before diagrams, because diagrams should be *derived from* the architecture doc, not the other way around.
6 is conditional and inserted before general tech-stack docs because AI/agent logic often IS the core differentiator of the product being rebuilt.
7–8 capture the "plumbing" — stack, infra, conditional concerns — after the logic is understood, so these docs can reference real usage instead of generic descriptions.
9 is the payoff: synthesis into an actionable rebuild sequence. It can only be written well once 1–8 exist.
10 is the safety net — nothing ships without a self-audit.

## Why phases are separate files, not one giant prompt
- Keeps each phase's instructions loadable/referenceable independently
- Lets an operator swap out or extend a single phase without touching the rest
- Matches how the agent should actually work: phase-by-phase with clean boundaries, not one undifferentiated blob

## How to extend this framework
To add a new phase (e.g., "Security Audit"):
1. Create `PROMPT_11_SECURITY_AUDIT.md` following the same template shape as existing PROMPT_XX files (Objective → Steps → Required Outputs → Validation Checklist)
2. Add it to `MASTER_INDEX.md`'s file map and to `MASTER_PROMPT.md`'s phase list
3. Decide its position in the execution order and update `OPERATING_RULES.md` if it changes sequencing assumptions
4. Keep it conditional (skip-with-reason) if it doesn't apply to every repo type, same pattern as Phase 6 and Phase 8

## Known limitation
No framework can force perfect accuracy from an LLM reading unfamiliar code. This framework's real value is in forcing systematic coverage and explicit uncertainty-flagging — not in guaranteeing zero errors. Treat all output as a strong first draft requiring one human review pass before being trusted as the sole rebuild source.


---

# MASTER PROMPT
### (Paste this to the AI coding agent, pointed at the target repository, to start a full run)

You are a Senior Staff Software Architect performing a full reverse-engineering audit of the repository at the given path. Your job is to produce documentation complete enough that an engineering team with zero access to this repo could rebuild an equivalent system using only your docs plus public packages/frameworks.

Follow, in this exact order:
1. `MISSION.md` — internalize the definition of done
2. `OPERATING_RULES.md` — how you pace yourself, handle ambiguity, and continue across long responses
3. `QUALITY_STANDARDS.md` — your anti-hallucination and completeness bar
4. `OUTPUT_RULES.md` — how you structure and format everything you produce

Then execute these phases strictly in order, each phase's detailed instructions in its own file:

| Order | Phase file | Skip condition |
|---|---|---|
| 1 | PROMPT_01_REPOSITORY_INTELLIGENCE.md | never |
| 2 | PROMPT_02_FILE_FOLDER_ANALYSIS.md | never |
| 3 | PROMPT_03_FUNCTION_CLASS_DOCS.md | never |
| 4 | PROMPT_04_ARCHITECTURE_RECONSTRUCTION.md | never |
| 5 | PROMPT_05_DIAGRAMS.md | never |
| 6 | PROMPT_06_AI_AGENT_WORKFLOW.md | skip, state "N/A — no AI/agent logic found", if repo has no LLM/agent code |
| 7 | PROMPT_07_TECH_STACK.md | never |
| 8 | PROMPT_08_CONDITIONAL_DOCS.md | skip individual subsections that don't apply (no DB → skip DB doc, etc.) |
| 9 | PROMPT_09_DEVELOPER_HANDBOOK_REBUILD.md | never |
| 10 | PROMPT_10_VALIDATION_QA.md | never — final self-audit is mandatory |

Do not ask for permission between phases. Begin Phase 1 immediately. Apply the Continuation Rule automatically if any response risks truncation. Only pause for the single-question exception defined in `OPERATING_RULES.md`.

Start now.


---

# PHASE 1 — REPOSITORY INTELLIGENCE

## Objective
Establish top-level ground truth about the repository before any deep analysis begins.

## Steps
1. Produce a full folder tree, depth-limited to meaningful levels. Collapse `node_modules`, `vendor`, `dist`, `build`, `.git`, and other generated/dependency folders into single collapsed lines.
2. Detect monorepo structure: list every distinct app/package/service found. Identify workspace tooling in use (nx, turborepo, lerna, pnpm/yarn workspaces, Cargo workspaces, etc.) and how apps/packages relate (shared libs, internal package references).
3. Per distinct stack found, identify: language(s) + version (from tsconfig/pyproject/go.mod/etc.), framework(s) + version, package manager, runtime target (node version, browser targets, Python version).
4. Identify entry points for every app/service (main files, index files, server bootstrap, CLI entry).
5. Identify build & tooling setup: bundler, linter/formatter config, CI/CD config files if present, test runner.
6. Write a one-paragraph hypothesis of "what this system does" — explicitly labeled as a hypothesis to be refined, not a final claim.

## Required Outputs
- `01-repository-intelligence.md` containing all of the above, organized per app/service if monorepo

## Validation Checklist
- [ ] Every top-level folder is accounted for (documented or explicitly excluded with reason)
- [ ] Every distinct language/framework combination found is listed exactly once, not duplicated across sections
- [ ] Entry points are verified against actual package.json/pyproject/etc. scripts, not guessed from file names alone


---

# PHASE 2 — FILE-BY-FILE & FOLDER-BY-FOLDER ANALYSIS

## Objective
Account for every file and folder in the repository with its actual purpose, not an assumed one.

## Steps
1. For every folder: state its purpose within the system, then list every file in it with a one-line purpose each.
2. For every non-trivial file (skip only genuinely empty/boilerplate files, and say so explicitly): document purpose, key exports, inputs it depends on (imports/env vars/config it reads), outputs/side effects (DB writes, API calls, UI render, events emitted), and any notable edge-case or error handling visible in the file.
3. Group output per app/service if monorepo — never interleave unrelated stacks in the same section.
4. Explicitly list excluded files/folders (generated code, vendor, build artifacts) with the reason for exclusion.

## Required Outputs
- `02-file-folder-analysis/` — one file per app/service (or one file total for small single-stack repos), following OUTPUT_RULES.md naming

## Validation Checklist
- [ ] No file left undocumented and unexplained
- [ ] Every documented file's "side effects" trace to code actually read, not inferred from filename
- [ ] Exclusions are justified, not just omitted silently


---

# PHASE 3 — FUNCTION & CLASS DOCUMENTATION

## Objective
Document unit-level logic precisely enough to reimplement each unit independently.

## Steps
For every function/method of real significance (trivial getters/setters can be skipped unless they hide logic — say so if skipping a batch):
1. Signature — params with types, return type
2. Purpose in plain terms
3. Step-by-step internal logic, numbered
4. Side effects / mutations
5. Error/exception behavior
6. Called-by / calls-into relationships (these feed Phase 5's call graph)

For every class:
1. Single-responsibility statement
2. State it owns
3. Public API surface
4. Inheritance/composition relationships
5. Lifecycle — when constructed, when destroyed/cleaned up

## Required Outputs
- `03-function-class-docs/` — organized to mirror the source folder structure so a reader can find a unit's doc the same way they'd find its source file

## Validation Checklist
- [ ] Every documented function's logic steps were read from the actual function body
- [ ] Call relationships documented here are consistent with the call graph produced in Phase 5
- [ ] No function's purpose is inferred from its name without reading its body


---

# PHASE 4 — ARCHITECTURE RECONSTRUCTION

## Objective
Move from unit-level understanding to system-level understanding.

## Steps
1. **System Design Doc** — layered view (presentation / application / domain / infra or equivalent for the stack found), responsibilities per layer
2. **Component Map** — every major component/module and what it owns
3. **Module Map** — dependency direction between modules (who imports whom); flag any circular dependencies found
4. **Working Logic Documentation** — how a real request/action flows end-to-end through the system, in numbered prose (diagrams come in Phase 5, this is the prose backbone they'll be drawn from)
5. **Business Logic Documentation** — domain rules only: validation rules, permission rules, pricing/state-transition rules, anything encoding "how the business actually works" — kept clearly separate from technical/plumbing logic

## Required Outputs
- `04-architecture/` containing system-design.md, component-map.md, module-map.md, working-logic.md, business-logic.md

## Validation Checklist
- [ ] Every claimed business rule quotes (paraphrased) an actual code condition/branch
- [ ] Module map's dependency directions were verified from actual imports, not assumed from folder naming
- [ ] Working Logic doc traces at least the 2–3 most important user-facing flows end-to-end


---

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


---

# PHASE 6 — AI AGENT WORKFLOW ANALYSIS (conditional)

## Skip condition
If the repository contains no LLM/agent/orchestration code, write `06-ai-agent-workflow.md` containing only: "N/A — no AI/agent logic found in this repository." and move to Phase 7.

## Objective
Document the AI-specific behavior separately from general application logic, since this is often the core differentiator of the product.

## Steps
1. **Prompt Flow** — every system prompt/prompt template found, what variables it injects, where in the execution flow it fires
2. **Reasoning Flow** — how the agent decides its next action (ReAct loop, planner/executor split, state machine, fixed pipeline, etc.)
3. **Planning Flow** — task decomposition logic if present
4. **Tool Calling Flow** — full list of tools/functions exposed to the model, their schemas, and the dispatch/routing logic
5. **Memory Flow** — what gets persisted, where, retrieval triggers, eviction/compression/summarization logic
6. **RAG Flow** — embedding model, vector store, chunking strategy, retrieval-to-prompt injection path

Explicitly mark, for each behavior documented, whether it is **deterministic code** or **model-driven** (i.e., depends on the LLM's output at runtime rather than fixed logic).

## Required Outputs
- `06-ai-agent-workflow.md`

## Validation Checklist
- [ ] Every prompt template documented is quoted structurally (variables/placeholders) without reproducing large verbatim proprietary prompt text beyond what's needed for technical accuracy
- [ ] Deterministic vs model-driven labeling is present for every documented behavior
- [ ] Tool schemas documented match actual function signatures/schemas in code, not assumed conventions


---

# PHASE 7 — TECH STACK DOCUMENTATION

## Objective
Document the stack as actually configured and used in THIS repo — not generic framework documentation.

## Steps
1. **Language Analysis** — languages used, versions, inferred rationale (from config files, not guessed)
2. **Framework Analysis** — frameworks + versions + this repo's specific configuration of them (custom middleware, plugins, non-default settings)
3. **Package/Dependency Analysis** — every direct dependency: what it's used for in THIS repo specifically, and whether it's load-bearing (core to a feature) or replaceable (could swap without redesign)
4. **Tech Stack Summary Table** — layer → technology → viable alternative options for a rebuild, with one-line trade-off per alternative

## Required Outputs
- `07-tech-stack.md`

## Validation Checklist
- [ ] Every dependency's documented usage was verified against actual import/usage sites, not assumed from the package's typical purpose
- [ ] Version numbers pulled from actual lockfiles/manifests, not assumed "latest"
- [ ] Summary table's alternatives are realistic, not arbitrary


---

# PHASE 8 — CONDITIONAL DOCUMENTATION

## Objective
Cover infrastructure-adjacent concerns that only apply to some repos — skip individually with a stated reason where not applicable.

## Subsections
1. **API Documentation** (if any routes/endpoints exist) — every route, method, auth requirement, request/response shape, status codes, rate limits if visible in code/config
2. **Database Documentation** (if a database is present) — schema, relationships, indexes, migration history if tracked in-repo
3. **Authentication Documentation** (if auth exists) — strategy used (session/JWT/OAuth/etc.), token lifecycle, permission/role model as implemented
4. **Deployment Documentation** (if deploy config exists) — build/deploy process from repo's own Docker/CI/CD/hosting config, not generic platform docs
5. **Environment Documentation** (if env vars are used) — every env var found, what it controls, required vs optional, default values if visible

## Required Outputs
- `08-conditional-docs/` — one file per applicable subsection; explicitly state "N/A — not present in this repo" for any that don't apply

## Validation Checklist
- [ ] Every documented endpoint/schema/env var traces to actual code/config, not framework defaults assumed
- [ ] N/A subsections are explicitly stated, not silently omitted


---

# PHASE 9 — DEVELOPER HANDBOOK & COMPLETE REBUILD GUIDE

## Objective
Synthesize everything from Phases 1–8 into an actionable sequence for rebuilding the system from zero.

## Steps
1. **Rebuild Order** — step-by-step sequence (e.g., scaffold framework → set up DB schema → build auth → build core domain models → build API layer → build UI → wire AI/agent layer if applicable → deploy)
2. **Feature Checklist** — every user-facing feature found, described as a rebuildable spec: trigger, behavior, edge cases — described as behavior, not restated code
3. **Non-obvious Gotchas** — workarounds, hacks, or unusual config found in the actual code that a rebuilder MUST know about to avoid breaking something
4. **Known Debt / What to Do Differently** — architectural smells actually observed, each with a brief alternative approach and its trade-off

## Required Outputs
- `09-developer-handbook-rebuild-guide.md`

## Validation Checklist
- [ ] Rebuild Order alone, read without any other doc file, gives enough sequencing to start building
- [ ] Every feature in the checklist traces to code/UI actually found, not assumed from product-type conventions
- [ ] Known Debt items are genuinely observed issues, not generic "best practices" filler


---

# PHASE 10 — FINAL VALIDATION & QA (mandatory, always run)

## Objective
Self-audit the entire documentation set before declaring the run complete.

## Checklist to run against the full output
- [ ] Every file in the repository is accounted for (documented, or explicitly excluded with reason) — cross-check against Phase 1's folder tree
- [ ] Every diagram across all phases uses valid, re-parseable Mermaid syntax
- [ ] No invented business logic anywhere — spot-check a sample of claims against source
- [ ] The Rebuild Guide (Phase 9) is usable standalone
- [ ] All cross-references between doc files point to real section headers, not broken links
- [ ] Tone is engineering-precise throughout — no marketing language, no filler transitions
- [ ] Open Questions log in `00-INDEX.md` is complete and every `[UNVERIFIED]` tag used anywhere is represented in it

## Required Outputs
- Append a `## Final Validation Report` section to `00-INDEX.md` stating pass/fail per checklist item, with fixes applied for any failures before declaring the run complete

