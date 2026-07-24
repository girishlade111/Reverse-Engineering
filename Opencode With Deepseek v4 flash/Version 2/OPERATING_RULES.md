========================================================================
OPERATING RULES
========================================================================
Enterprise Reverse Engineering Prompt Framework
Version: 3.0

========================================================================
RULE 1: NO MODIFICATION
========================================================================

You must NEVER modify the repository being analyzed.
This is a read-only reverse engineering process.

You may:
- Read any file in the repository
- Copy content for documentation purposes
- Generate new files in the output workspace
- Reference file paths and line numbers

You may NOT:
- Edit, delete, or rename files in the repository
- Create new files inside the repository
- Execute build or test commands
- Install dependencies
- Modify configuration

========================================================================
RULE 2: EVIDENCE REQUIREMENT
========================================================================

Every claim about the repository must include a citation to
specific evidence in the code.

Correct: "The `authenticate` function in `src/auth/login.ts:45`
validates JWT tokens using the `jsonwebtoken` library."

Incorrect: "The system uses JWT authentication."

If you are speculating or inferring, state it explicitly:
"Based on the directory structure and naming conventions, it
appears that `src/services/` contains business logic services,
but this has not been verified by reading each file."

========================================================================
RULE 3: NO ASSUMPTION WITHOUT VERIFICATION
========================================================================

Do not assume:
- File purposes based solely on names
- Design patterns based solely on imports
- Architecture based solely on directory structure
- Functionality based solely on function names
- Security based solely on apparent patterns

Always verify by reading the actual code.

========================================================================
RULE 4: HANDLE AMBIGUITY
========================================================================

When the code is ambiguous:

1. Document all possible interpretations.
2. Cite the evidence for each interpretation.
3. Note which interpretation you consider most likely and why.
4. Flag the ambiguity for human review.

========================================================================
RULE 5: RESPECT SCOPE
========================================================================

Stay within the defined scope of each phase.

Phase 1 is for reconnaissance only. Do not perform deep module
analysis during Phase 1.

If you discover something that belongs to a later phase, document
it briefly in your current notes and ensure it is covered in the
appropriate phase.

========================================================================
RULE 6: MAINTAIN CONTEXT
========================================================================

Maintain a running mental model of the repository across phases.

- Reference findings from previous phases.
- Note when new findings contradict earlier conclusions.
- Update earlier conclusions when new evidence emerges.
- Keep track of what has been analyzed and what remains.

========================================================================
RULE 7: HANDLE SCALE PROPORTIONATELY
========================================================================

For large repositories, use these strategies:

SAMPLING: Analyze representative samples from each category of
file. Document your sampling methodology.

PATTERN IDENTIFICATION: Identify patterns early. Once a pattern
is understood, document it and apply it to similar files without
re-analyzing every instance.

PRIORITIZATION: Prioritize analysis of:
1. Entry points and core logic
2. Public APIs and interfaces
3. Data models and schemas
4. Configuration and initialization
5. Error handling and edge cases
6. Tests (as specifications)
7. Build and deployment

DEPTH TIERS:
- Tier 1 (DeepScan): Every line, every branch
- Tier 2 (StandardScan): Every function signature, key logic
- Tier 3 (SurfaceScan): Structure and purpose only
- Tier 4 (Skip): Document as out of scope

========================================================================
RULE 8: DOCUMENTATION MUST BE READABLE
========================================================================

All generated documentation must be:

- Well-structured with clear headings
- Written in professional technical English
- Free of vague or ambiguous language
- Consistent in terminology and formatting
- Accessible to the target audience

========================================================================
RULE 9: DIAGRAMS ARE MANDATORY
========================================================================

Wherever a diagram would aid understanding, one must be included.

Preferred diagram types (in order):
1. Mermaid sequence diagrams (for workflows)
2. Mermaid class diagrams (for structures)
3. Mermaid flowcharts (for processes)
4. Mermaid state diagrams (for state machines)
5. Mermaid component diagrams (for architecture)
6. Mermaid entity-relationship diagrams (for data models)
7. ASCII art (when Mermaid is impractical)

========================================================================
RULE 10: TRACEABILITY
========================================================================

Every artifact must be traceable back to the source code that
informed it.

Maintain a traceability matrix that maps:
- Artifact section --> Source file(s) and line numbers
- Source file --> Artifact sections that reference it

========================================================================
RULE 11: ERROR RESILIENCE
========================================================================

If a file cannot be read:
- Document the error
- Note the file path and suspected purpose
- Flag it for manual review
- Continue analysis

If a pattern is unrecognizable:
- Document what it appears to do
- Note the uncertainty
- Flag it for expert review

========================================================================
RULE 12: FINALITY
========================================================================

Do not stop analysis until:
- Every file in scope has been examined (at least SurfaceScan)
- Every required artifact has been generated
- Every quality gate has been passed
- The validation checklist is complete
- All inconsistencies have been resolved or documented

========================================================================
END OF OPERATING RULES
========================================================================
