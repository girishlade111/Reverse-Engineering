========================================================================
PROMPT 10: VALIDATION
========================================================================
Phase 10: Validation, Cross-Referencing, and Quality Assurance
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. All artifacts verified for completeness and accuracy
2. Cross-references validated for correctness
3. Quality scores calculated for all dimensions
4. Gap analysis documenting missing or incomplete coverage
5. Final quality scorecard for the entire reverse engineering effort
6. List of any issues, discrepancies, or items needing human review

========================================================================
INPUTS
========================================================================

All artifacts from Phases 1 through 9.

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 10.1: COMPLETENESS VERIFICATION

10.1.1. Verify that every file in the repository is covered:
    - Cross-reference the file inventory from Phase 1 against
      all documentation artifacts.
    - For each file not mentioned in any artifact, add it to
      the gap list.

10.1.2. Verify that every artifact section is populated:
    - Check each artifact for missing or incomplete sections.
    - Flag any sections marked [PENDING] or [PARTIAL].

10.1.3. Verify required artifact checklist:
    Use the artifact checklist from MASTER_PROMPT.md.

10.1.4. Generate a completeness report:
    - Total files in repository
    - Files documented (with purpose)
    - Files analyzed (deep)
    - Files not covered (gaps)
    - Artifact completion percentage per document

ACTIVITY 10.2: ACCURACY VERIFICATION

10.2.1. Spot-check claims against source code:
    - Randomly select 10% of documented claims.
    - Verify each claim by reading the cited source code.
    - Document any inaccuracies found.

10.2.2. Verify diagram accuracy:
    - For each diagram, verify that it accurately represents
      the actual code structure.
    - Check that all nodes in the diagram exist in the code.
    - Check that all connections/edges exist in the code.
    - Check that no elements are missing from the diagram.

10.2.3. Verify API documentation accuracy:
    - For each documented API endpoint, verify against the
      actual route definition and handler.
    - Check that request/response schemas match.
    - Check that authentication requirements match.

10.2.4. Verify configuration documentation:
    - For each documented configuration option, verify against
      the actual configuration file or schema.
    - Check that defaults are correctly documented.
    - Check that environment variable names are correct.

ACTIVITY 10.3: CROSS-REFERENCE VALIDATION

10.3.1. Verify all internal cross-references:
    - For every cross-reference in every artifact, verify that
      the target section exists and is correctly named.
    - Fix any broken or incorrect cross-references.

10.3.2. Verify traceability:
    - For every claim with a code citation, verify the citation
      is correct (file exists, line number is valid).
    - For every file referenced, verify the claim made about it.

10.3.3. Verify consistency across artifacts:
    - Check that the same concept is described consistently
      across all artifacts.
    - Check that terminology is consistent.
    - Check that architectural descriptions are consistent.

ACTIVITY 10.4: QUALITY SCORING

10.4.1. Score each major artifact against all quality dimensions:
    Dimensions:
    - Accuracy (weight: 25%)
    - Completeness (weight: 20%)
    - Clarity (weight: 15%)
    - Depth (weight: 15%)
    - Organization (weight: 10%)
    - Consistency (weight: 10%)
    - Usability (weight: 5%)

10.4.2. Calculate weighted scores:
    - Per-artifact weighted score
    - Overall project weighted score
    - Per-dimension average score

10.4.3. Determine if minimum quality thresholds are met:
    - Overall weighted score >= 4.0
    - No dimension below 3.0
    - Accuracy dimension >= 4.0

10.4.4. Document quality findings:
    - Strengths: dimensions that scored highest
    - Weaknesses: dimensions that need improvement
    - Recommendations for improvement

ACTIVITY 10.5: GAP ANALYSIS

10.5.1. Identify coverage gaps:
    - Files with no documented purpose
    - Modules with incomplete analysis
    - Features not covered in documentation
    - APIs not documented
    - Error types not cataloged
    - Dependencies not analyzed

10.5.2. Identify depth gaps:
    - Modules that need deeper analysis
    - Functions that need more detailed documentation
    - Workflows that need sequence diagrams
    - Algorithms that need complexity analysis

10.5.3. Classify gaps:
    - Critical: Impact understanding of core functionality
    - Major: Impact understanding of significant features
    - Minor: Impact understanding of peripheral features
    - Cosmetic: Impact documentation quality but not understanding

ACTIVITY 10.6: FINAL QUALITY SCORECARD

10.6.1. Generate the final quality scorecard including:

PROJECT SUMMARY
- Repository: [name]
- Total files: [count]
- Total LOC: [count]
- Languages: [list]
- Artifacts generated: [count]

QUALITY SCORES
- Overall Score: [X.X/5.0]
- Accuracy: [X.X/5.0]
- Completeness: [X.X/5.0]
- Clarity: [X.X/5.0]
- Depth: [X.X/5.0]
- Organization: [X.X/5.0]
- Consistency: [X.X/5.0]
- Usability: [X.X/5.0]

COVERAGE
- Files documented: [count/percentage]
- Modules documented: [count/percentage]
- APIs documented: [count/percentage]
- Errors cataloged: [count/percentage]
- Dependencies analyzed: [count/percentage]

GAPS
- Critical: [count]
- Major: [count]
- Minor: [count]
- Cosmetic: [count]

VERDICT
- PASS / FAIL (based on minimum thresholds)
- If FAIL: Required improvements

ACTIVITY 10.7: ISSUES AND FLAGS

10.7.1. Document all issues found during validation:
    - Inaccurate claims (with corrections needed)
    - Missing information (with what's needed)
    - Ambiguous descriptions (with clarification needed)
    - Contradictory information (with resolution needed)
    - Code issues discovered during analysis (with recommendations)

10.7.2. For each issue:
    - Severity (critical, major, minor, cosmetic)
    - Affected artifact and section
    - Description of the issue
    - Recommended action
    - Priority (immediate, next phase, future)

ACTIVITY 10.8: FINAL REPORT GENERATION

10.8.1. Generate a final executive summary including:
    - What was analyzed
    - Key findings
    - Architecture overview (2-3 sentences)
    - Quality assessment
    - Critical issues (if any)
    - Recommendations

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT 10.1: COMPLETENESS_REPORT.md
- File coverage analysis
- Artifact completion analysis
- Missing elements report

ARTIFACT 10.2: ACCURACY_REPORT.md
- Spot-check results
- Diagram accuracy results
- API documentation accuracy results
- Configuration documentation accuracy results

ARTIFACT 10.3: CROSS_REFERENCE_VALIDATION.md
- Cross-reference verification results
- Traceability verification results
- Consistency verification results

ARTIFACT 10.4: QUALITY_SCORECARD.md
- Per-artifact scores
- Overall scores
- Strengths and weaknesses
- Improvement recommendations

ARTIFACT 10.5: GAP_ANALYSIS.md
- Coverage gaps with classification
- Depth gaps with classification
- Remediation recommendations

ARTIFACT 10.6: FINAL_REPORT.md
- Executive summary
- Key findings
- Quality verdict
- Recommendations
- Next steps

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] Completeness verification is complete.
[ ] Accuracy spot-checks have been performed.
[ ] Cross-references are validated.
[ ] Quality scores have been calculated.
[ ] Gap analysis is complete.
[ ] Final scorecard is generated.
[ ] All issues are documented.
[ ] Final report is generated.
[ ] The entire framework has been executed to completion.
[ ] All quality thresholds are met or improvement plan exists.

========================================================================
OUTPUTS
========================================================================

This is the final phase.

Outputs are the complete set of all artifacts generated
throughout the process, plus the Phase 10 validation artifacts.

========================================================================
END OF PROMPT 10
========================================================================
