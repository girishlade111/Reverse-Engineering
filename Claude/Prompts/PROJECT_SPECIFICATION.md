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
