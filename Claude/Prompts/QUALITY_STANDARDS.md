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
