# QUALITY STANDARDS — Enterprise Reverse Engineering Framework

## STANDARD CLASSIFICATION: Mandatory
## APPLIES TO: All outputs across all phases

---

## STANDARD 1: COMPLETENESS

Every output must be evaluated against the **Completeness Checklist**:

- [ ] All files in scope have been analyzed
- [ ] All functions have been documented
- [ ] All classes have been documented
- [ ] All interfaces have been documented
- [ ] All exports have been documented
- [ ] All imports have been traced
- [ ] All dependencies have been cataloged
- [ ] All configuration points have been captured
- [ ] All entry points have been identified
- [ ] All exit points have been identified
- [ ] All error handlers have been documented
- [ ] All state transitions have been mapped
- [ ] All events have been cataloged
- [ ] All API endpoints have been documented
- [ ] All data flows have been traced
- [ ] All edge cases have been considered

## STANDARD 2: ACCURACY

Every claim must be verifiable against the source code.

**Accuracy Tiers**:

| TIER | LABEL | CRITERIA |
|------|-------|----------|
| A | Verified | Confirmed by reading source code at specified file:line |
| B | Inferred | Logical conclusion from verified evidence, clearly stated as inference |
| C | Uncertain | Multiple interpretations exist; all are documented |
| D | Unknown | Cannot be determined from available evidence |

All claims must be labeled with their accuracy tier.

## STANDARD 3: DEPTH

Analysis depth must match the complexity of the subject:

| SUBJECT COMPLEXITY | REQUIRED DEPTH |
|-------------------|----------------|
| Single-purpose utility function | Name, signature, purpose, parameters, return value, dependencies |
| Class | Responsibility, public API, private methods, state, relationships, lifecycle |
| Module | Boundary, public interface, internal structure, dependencies, responsibility |
| Service | API contract, protocols, deployment, scaling, failure modes, dependencies |
| System | Architecture, layers, communication, data flow, deployment, trade-offs |

## STANDARD 4: CLARITY

All outputs must be:
- Written in clear, professional English
- Organized with hierarchical headings (H1 → H2 → H3 → H4)
- Free of vague language ("thing", "stuff", "some", "maybe")
- Explicit about what is known vs. unknown
- Navigable with a table of contents for files over 200 lines

## STANDARD 5: CONSISTENCY

Across all phases:
- Terminology must be consistent (use GLOSSARY.md)
- Naming conventions must match the source code
- Architectural descriptions must not contradict each other
- File references must use consistent paths

## STANDARD 6: TRACEABILITY

Every significant output section must include a **Traceability Section**:

```markdown
## Traceability

| Claim | Evidence | Tier |
|-------|----------|------|
| This module handles authentication | src/auth/index.ts:1-150 | A |
| JWT tokens expire after 24h | src/auth/config.ts:12 | A |
| Rate limiting is applied globally | src/middleware/rate-limit.ts:5-30 | B |
```

## STANDARD 7: DIAGRAM QUALITY

All diagrams must follow these standards:

- Mermaid.js syntax for all diagrams
- Consistent styling across all diagrams
- Meaningful labels on all nodes and edges
- Legend for any non-obvious notation
- Appropriate diagram type for the concept:
  - `graph TD` for component relationships
  - `sequenceDiagram` for interaction flows
  - `flowchart` for algorithms and workflows
  - `classDiagram` for class hierarchies
  - `stateDiagram` for state machines
  - `gantt` for project timelines

## STANDARD 8: ERROR DOCUMENTATION QUALITY

Error handlers must be documented with:

- Trigger condition (what causes this error path?)
- Recovery behavior (what happens after the error?)
- User-visible impact (what does the user see?)
- Logging behavior (what is logged?)
- Fallback behavior (what alternative path is taken?)

## STANDARD 9: READABILITY SCORE

All documentation must target a readability that allows:
- A new team member to understand the system in one reading
- An experienced engineer to find any specific detail in under 30 seconds
- A reviewer to verify accuracy against the source code

## STANDARD 10: MAINTAINABILITY

Documentation must be structured so that:
- Individual sections can be updated independently
- Cross-references are explicit (not "see above" but "see [Section X.Y.Z]")
- File paths in documentation can be verified against the current repo state
- Outdated sections are easy to identify

---

## QUALITY AUDIT

Before finalizing any phase output, run this self-audit:

```markdown
## Phase N Quality Audit

### Completeness
- [ ] All files in scope analyzed
- [ ] All functions/classes documented
- [ ] All imports traced
- [ ] All exports documented

### Accuracy
- [ ] All claims labeled with accuracy tier
- [ ] All tier-A claims have file:line references
- [ ] No unlabeled inferences

### Clarity
- [ ] Table of contents present (if >200 lines)
- [ ] No vague language
- [ ] Professional English throughout

### Consistency
- [ ] Terminology matches GLOSSARY.md
- [ ] No contradictions with previous phases

### Traceability
- [ ] Traceability section present
- [ ] Every claim traceable to evidence
```

## FAILURE THRESHOLDS

If any quality audit reveals:
- More than 3 untraced claims
- Any contradiction with previous phases
- Any file in scope that was not analyzed

...the phase output is REJECTED. Fix all issues before proceeding.

---

*Quality is not negotiable. Every standard must be met.*
