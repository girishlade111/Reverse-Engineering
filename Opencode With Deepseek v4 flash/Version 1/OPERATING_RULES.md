# OPERATING RULES — Enterprise Reverse Engineering Framework

## RULE CLASSIFICATION: Immutable
## SCOPE: All phases, all outputs, all agent behavior

---

## RULE 1: UNDERSTAND BEFORE DOCUMENTING

You must NOT write any final documentation until you have performed the analysis that proves understanding.

**Enforcement**: Before writing a documentation section, you must produce:
- File:line references for every claim
- Evidence of reading the relevant code
- Analysis notes showing reasoning process

## RULE 2: NO SKIPPING PHASES

All 20 phases must be executed in order. No phase may be skipped.

**Exception**: If a phase analyzes a feature that does not exist (e.g., PROMPT_17_AI_WORKFLOWS for a repo with no AI), output a single file stating the feature is absent and proceed.

## RULE 3: EVIDENCE EVERY CLAIM

Every claim in every output must be backed by:
- File path
- Line number(s)
- Quoted source code or clear paraphrase

**Bad**: "The system uses JWT authentication."
**Good**: "The system uses JWT authentication implemented in `src/auth/jwt.ts:25-89` via the `jsonwebtoken` library, with token verification at `src/middleware/auth.ts:42-58`."

## RULE 4: VALIDATE EVERY PHASE

Before advancing from phase N to phase N+1:
- Run the validation checks specified in PROMPT_N
- Fix any failures
- Re-run validation
- Only then proceed

## RULE 5: DOCUMENT GAPS

When you encounter something you cannot fully analyze:
1. Document what you know
2. Document what you don't know
3. Document why you can't analyze it further
4. Flag it as a gap with a unique GAP-ID
5. Log it in the gap tracker

## RULE 6: MAINTAIN CONSISTENCY

Once you name a module, component, service, or concept, use that name consistently across all phases.

**Enforcement**: Maintain a glossary (`GLOSSARY.md`) that maps every named entity to:
- Canonical name
- Aliases (if any)
- First-seen location (file:line)
- Description

## RULE 7: HANDLE AMBIGUITY

When code is ambiguous, unclear, or contradictory:
1. Document both/all interpretations
2. State which interpretation is most likely and why
3. Flag the ambiguity in the output

## RULE 8: ONE PHASE AT A TIME

Focus on exactly one phase at a time. Do not jump ahead.

## RULE 9: READ BEFORE ANALYZING

Before analyzing any file:
- Read it completely
- Read its imports
- Read its dependents
- Understand the context

**Exception**: For files over 1000 lines, read the first 200 lines, the last 50 lines, and search for key patterns.

## RULE 10: TRACE EVERY IMPORT

For every import, require, include, or dependency reference:
- Resolve it to the actual file
- Document what is imported
- Document how it's used

## RULE 11: DOCUMENT EVERY ERROR HANDLER

Every catch block, error handler, error middleware, fallback, and recovery path must be documented.

## RULE 12: DOCUMENT EVERY CONFIGURATION POINT

Every configuration file, environment variable, setting, flag, and constant must be documented with:
- Name
- Purpose
- Default value
- Valid values
- Where it's consumed

## RULE 13: DOCUMENT EVERY ENTRY POINT

Every entry point to the system must be documented:
- CLI commands
- HTTP endpoints
- Event handlers
- Message queue listeners
- Scheduled jobs
- Worker processes
- Public API surfaces

## RULE 14: DOCUMENT EVERY EXIT POINT

Every exit point must be documented:
- HTTP responses
- Error responses
- Return values
- Side effects
- Log outputs
- Written files
- External calls

## RULE 15: TRACE ASYNC FLOWS

Every async operation, promise chain, callback, event emission, and message publication must be traced from trigger to completion.

## RULE 16: NO ASSUMPTIONS ABOUT AI CONTENT

If the repository contains prompts, AI configurations, or LLM-related code, analyze it with the same depth as any other code.

## RULE 17: CROSS-REFERENCE FORWARD AND BACKWARD

New findings must be cross-referenced against all previous phase outputs:
- Does this new finding contradict earlier analysis?
- Does this new finding enrich earlier analysis?
- Does this new finding reveal a gap in earlier analysis?

## RULE 18: OUTPUT TO STRUCTURED FILES

All outputs must be written to structured markdown files in the `re-docs/` directory.

## RULE 19: DIAGRAMS FOR COMPLEXITY

Any subsystem with more than 5 components, or any flow with more than 5 steps, must include a diagram.

## RULE 20: FINAL VALIDATION IS MANDATORY

Phase 20 validation is not optional. The framework is incomplete without it.

---

## VIOLATION HANDLING

If the agent detects it has violated any operating rule:
1. Immediately note the violation
2. Correct the output
3. Log the violation in `re-docs/VIOLATION_LOG.md`
4. If the violation affected downstream phases, flag affected phases for re-analysis
