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
