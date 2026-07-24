========================================================================
VALIDATION CHECKLIST
========================================================================
Template: Enterprise Reverse Engineering Prompt Framework
This file is a TEMPLATE. The executing AI must populate all sections.

========================================================================
INSTRUCTIONS
========================================================================

For each item, mark as:
- [PASS] - Requirement met
- [FAIL] - Requirement not met
- [N/A]  - Not applicable
- [FLAG] - Needs human review

========================================================================
SECTION A: COMPLETENESS
========================================================================

A.1. FILE COVERAGE
[ ] A.1.1. Every source file has a documented purpose.
[ ] A.1.2. Every module is documented with responsibilities.
[ ] A.1.3. Every entry point is identified and documented.
[ ] A.1.4. Every directory has a documented purpose.
[ ] A.1.5. Hidden files and directories are accounted for.

A.2. COMPONENT COVERAGE
[ ] A.2.1. Every component is identified.
[ ] A.2.2. Component responsibilities are documented.
[ ] A.2.3. Component interactions are mapped.
[ ] A.2.4. Component dependencies are documented.
[ ] A.2.5. Component diagrams are generated.

A.3. DATA COVERAGE
[ ] A.3.1. Every data source is identified.
[ ] A.3.2. Every data flow is traced.
[ ] A.3.3. Every transformation is documented.
[ ] A.3.4. State management is fully documented.
[ ] A.3.5. Event catalog is complete.

A.4. LOGIC COVERAGE
[ ] A.4.1. Every business rule is extracted.
[ ] A.4.2. Every algorithm is documented.
[ ] A.4.3. Every workflow has a sequence diagram.
[ ] A.4.4. Decision trees are mapped.
[ ] A.4.5. Business invariants are documented.

A.5. AI COVERAGE (if applicable)
[ ] A.5.1. Every prompt is extracted with full text.
[ ] A.5.2. Agent workflows are documented.
[ ] A.5.3. Tool definitions are cataloged.
[ ] A.5.4. RAG pipeline is documented.
[ ] A.5.5. Memory/context management is documented.

A.6. INTEGRATION COVERAGE
[ ] A.6.1. Every API endpoint is documented.
[ ] A.6.2. Database schema is documented.
[ ] A.6.3. Every third-party integration is documented.
[ ] A.6.4. Authentication/authorization is documented.
[ ] A.6.5. Every external dependency is cataloged.

A.7. ERROR COVERAGE
[ ] A.7.1. Error handling patterns are documented.
[ ] A.7.2. Error catalog is complete.
[ ] A.7.3. Retry/resilience patterns are documented.
[ ] A.7.4. Validation layers are audited.

A.8. DOCUMENTATION COVERAGE
[ ] A.8.1. Architecture Handbook is complete.
[ ] A.8.2. Developer Handbook is complete.
[ ] A.8.3. Rebuild Guide is complete.
[ ] A.8.4. Engineering Notes are complete.
[ ] A.8.5. Cross-References are complete.
[ ] A.8.6. Validation Checklist is complete.

========================================================================
SECTION B: ACCURACY
========================================================================

B.1. CLAIM VERIFICATION
[ ] B.1.1. Random spot-check of claims passes (10% sample).
[ ] B.1.2. All claims have code evidence citations.
[ ] B.1.3. No unlabeled speculation or assumptions.

B.2. DIAGRAM VERIFICATION
[ ] B.2.1. All nodes in diagrams exist in the code.
[ ] B.2.2. All connections in diagrams exist in the code.
[ ] B.2.3. Diagrams match actual code structure.
[ ] B.2.4. No elements missing from diagrams.

B.3. API DOCUMENTATION ACCURACY
[ ] B.3.1. Route paths match actual definitions.
[ ] B.3.2. Request schemas match actual handlers.
[ ] B.3.3. Response schemas match actual responses.
[ ] B.3.4. Authentication requirements match.

B.4. CONFIGURATION ACCURACY
[ ] B.4.1. Configuration keys match actual config files.
[ ] B.4.2. Default values are correct.
[ ] B.4.3. Environment variable names are correct.

========================================================================
SECTION C: QUALITY
========================================================================

C.1. QUALITY DIMENSION SCORES
[ ] C.1.1. Accuracy score >= 4.0/5.0
[ ] C.1.2. Completeness score >= 4.0/5.0
[ ] C.1.3. Clarity score >= 4.0/5.0
[ ] C.1.4. Depth score >= 4.0/5.0
[ ] C.1.5. Organization score >= 4.0/5.0
[ ] C.1.6. Consistency score >= 4.0/5.0
[ ] C.1.7. Usability score >= 4.0/5.0

C.2. OVERALL SCORE
[ ] C.2.1. Overall weighted score >= 4.0/5.0
[ ] C.2.2. No dimension below 3.0/5.0

========================================================================
SECTION D: CONSISTENCY
========================================================================

D.1. TERMINOLOGY CONSISTENCY
[ ] D.1.1. All domain terms are defined.
[ ] D.1.2. Terms are used consistently across all artifacts.
[ ] D.1.3. No undefined abbreviations or acronyms.

D.2. FORMATTING CONSISTENCY
[ ] D.2.1. All artifacts use consistent heading structure.
[ ] D.2.2. All diagrams follow consistent style.
[ ] D.2.3. All citations follow consistent format.
[ ] D.2.4. All tables use consistent formatting.

D.3. CROSS-DOCUMENT CONSISTENCY
[ ] D.3.1. Same concepts described consistently across docs.
[ ] D.3.2. Architectural descriptions are consistent.
[ ] D.3.3. No contradictory information across artifacts.

========================================================================
SECTION E: CROSS-REFERENCES
========================================================================

E.1. INTERNAL CROSS-REFERENCES
[ ] E.1.1. All internal links resolve correctly.
[ ] E.1.2. All section references are correct.
[ ] E.1.3. No broken cross-references.

E.2. TRACEABILITY
[ ] E.2.1. Every code citation resolves to a valid file.
[ ] E.2.2. Every file citation has a valid line number.
[ ] E.2.3. Claims match cited code.

========================================================================
SECTION F: USABILITY
========================================================================

F.1. ACTIONABILITY
[ ] F.1.1. Developer can begin working after reading handbook.
[ ] F.1.2. Rebuild process is reproducible from the guide.
[ ] F.1.3. Troubleshooting guide addresses common issues.

F.2. NAVIGABILITY
[ ] F.2.1. Table of contents is present and accurate.
[ ] F.2.2. Information is findable within 3 navigation steps.
[ ] F.2.3. Cross-references help navigation.

========================================================================
SECTION G: GAP ANALYSIS
========================================================================

G.1. CRITICAL GAPS
[ ] G.1.1. No critical gaps in understanding core functionality.
[ ] G.1.2. No critical gaps in documentation coverage.

G.2. MAJOR GAPS
[ ] G.2.1. No major gaps in feature documentation.
[ ] G.2.2. No major gaps in component understanding.

G.3. MINOR GAPS
[ ] Document minor gaps here:

G.4. COSMETIC ISSUES
[ ] Document cosmetic issues here:

========================================================================
SECTION H: FINAL VERDICT
========================================================================

[ ] PASS: All minimum thresholds met.
[ ] FAIL: Minimum thresholds not met. See improvement plan.

IMPROVEMENT PLAN (if FAIL):
1. [required improvement 1]
2. [required improvement 2]

========================================================================
END OF VALIDATION CHECKLIST
========================================================================
