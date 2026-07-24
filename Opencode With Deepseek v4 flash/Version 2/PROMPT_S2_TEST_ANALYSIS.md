========================================================================
SUPPLEMENTARY PROMPT S2: TEST ANALYSIS
========================================================================
Supplementary Analysis: Test Suite Deep Analysis
Enterprise Reverse Engineering Prompt Framework

========================================================================
PURPOSE
========================================================================

This supplementary prompt provides a deep analysis of the test
suite, using tests as executable specifications to validate
understanding and uncover undocumented behavior.

========================================================================
WHEN TO USE
========================================================================

Execute this prompt if:
- Tests are comprehensive and well-organized
- Understanding of edge cases from tests is needed
- Test coverage analysis is desired
- Tests serve as primary documentation
- Behavior-Driven Development (BDD) tests exist

Execute after Phase 5 and before Phase 9.

========================================================================
ACTIVITIES
========================================================================

S2.1. TEST ORGANIZATION ANALYSIS
- Document test directory structure
- Identify test type (unit, integration, e2e, snapshot)
- Document test naming conventions
- Document test fixture organization

S2.2. TEST COVERAGE ANALYSIS
- Map tests to modules/features
- Identify untested code paths
- Identify over-tested code paths
- Document coverage gaps

S2.3. TEST PATTERN EXTRACTION
- Extract test patterns used
- Document mock/stub usage
- Document test data strategies
- Document assertion patterns

S2.4. TEST AS SPECIFICATION
- Extract behavioral specifications from BDD tests
- Extract edge cases from tests
- Extract expected error conditions from tests
- Extract integration contracts from tests

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT S2.1: TEST_ORGANIZATION.md
ARTIFACT S2.2: TEST_COVERAGE.md
ARTIFACT S2.3: TEST_PATTERNS.md
ARTIFACT S2.4: TEST_SPECIFICATIONS.md

========================================================================
END OF PROMPT S2
========================================================================
