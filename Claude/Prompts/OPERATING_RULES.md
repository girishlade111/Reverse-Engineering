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
