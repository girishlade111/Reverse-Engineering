# PROMPT_26: Coverage Validation & Completeness Verification

## Classification
- **Domain:** Validation & Quality Assurance
- **Phase:** 6 — Final Validation
- **Prerequisites:** PROMPT_25 (Engineering Validation)
- **Dependencies:** Engineering validation results
- **Estimated Effort:** Medium

---

## Objective

Validate that the analysis and documentation achieve complete coverage of the repository — every file, every module, every function, every endpoint, every configuration option, and every dependency is documented with no gaps.

---

## Input Requirements

### Required Context
- Engineering validation results from PROMPT_25
- File inventory from PROMPT_01
- Module decomposition from PROMPT_06
- Component analysis from PROMPT_07
- Generated documentation from Phase 5

---

## Pre-Analysis Checklist
- [ ] PROMPT_25 completed and corrections applied
- [ ] File inventory available for comparison
- [ ] Module/component lists available for comparison

---

## Analysis Tasks

### Task 1: File-Level Coverage Validation

**Purpose:** Verify every source file is covered by documentation.

**Instructions:**
1. Compare file inventory against documented files:
   - Every `.py`, `.js`, `.ts`, `.java` file should be referenced in documentation
   - Every configuration file should be documented
   - Every test file scope should be noted
2. Flag undocumented files
3. Categorize files by coverage status

**Output Format:**

```markdown
## File-Level Coverage Validation

### Coverage Summary
| Category | Total Files | Documented | Undocumented | Coverage % |
|----------|-------------|------------|--------------|------------|
| Source (.py) | 150 | 148 | 2 | 98.7% |
| Source (.ts/.js) | 125 | 125 | 0 | 100% |
| Configuration | 45 | 43 | 2 | 95.6% |
| Tests | 80 | 75 | 5 | 93.8% |
| Documentation | 30 | 30 | 0 | 100% |
| **Total** | **430** | **421** | **9** | **97.9%** |

### Undocumented Files
| File | Type | Reason | Action |
|------|------|--------|--------|
| src/legacy/migration_v1.py | Source | Marked as legacy | Add brief documentation |
| config/experimental.yaml | Config | Newly added | Document experimental features |
| tests/integration/test_new_feature.py | Test | Recently added | Note in test documentation |
```

---

### Task 2: Module & Component Coverage Validation

**Purpose:** Verify every module and component is documented.

**Instructions:**
1. Compare module/component inventory against documented modules
2. Verify each module has:
   - Responsibility documentation
   - Interface documentation
   - Dependency documentation
3. Flag undocumented or poorly documented modules

**Output Format:**

```markdown
## Module & Component Coverage Validation

### Module Coverage
| Module | Responsibility | Interface | Dependencies | Overall Coverage |
|--------|---------------|-----------|--------------|------------------|
| Auth | Complete | Complete | Complete | 100% |
| Users | Complete | Complete | Complete | 100% |
| Orders | Complete | Partial | Complete | 90% (missing interface for helper) |
| Payments | Complete | Complete | Complete | 100% |
| Legacy | Missing | Missing | Missing | 0% (intentionally excluded) |
```

---

### Task 3: Feature & Function Coverage Validation

**Purpose:** Verify all features and functions are documented.

**Instructions:**
1. Compare feature list against documentation
2. Verify key functions are documented with:
   - Signature
   - Purpose
   - Parameters
   - Return values
   - Error handling

**Output Format:**

```markdown
## Function Coverage Validation

### Key Function Coverage
| Function | Documented | Signature Match | Error Handling | Overall |
|----------|------------|-----------------|----------------|---------|
| create_user() | Yes | Yes | Yes | 100% |
| process_order() | Yes | Yes | Partial | 85% |
| refund_payment() | Yes | Yes | Yes | 100% |
| validate_email() | No | - | - | 0% |

### Coverage Gaps
| Function | Location | Priority | Action Required |
|----------|----------|----------|-----------------|
| validate_email() | src/validators/email.py | LOW | Add utility function documentation |
| format_report() | src/reports/formatter.py | MEDIUM | Document report generation |
```

---

### Task 4: Coverage Gap Remediation

**Purpose:** Provide remediation plan for coverage gaps.

**Instructions:**
1. Prioritize gaps by impact
2. Provide specific remediation steps for each gap
3. Estimate effort for remediation

**Output Format:**

```markdown
## Coverage Gap Remediation Plan

| Priority | Gap | Type | Effort | Action |
|----------|-----|------|--------|--------|
| HIGH | Missing legacy module docs | Module | 2 hours | Add brief overview of legacy module |
| MEDIUM | 5 undocumented test files | File | 1 hour | Add test scope notes |
| LOW | 2 undocumented utility functions | Function | 30 min | Add simple documentation |
```

---

## Synthesis

**Output Format:**

```markdown
## Coverage Validation Summary

### Overall Coverage
| Dimension | Coverage | Target | Status |
|-----------|----------|--------|--------|
| File coverage | 97.9% | 100% | NEEDS IMPROVEMENT |
| Module coverage | 96% | 100% | NEEDS IMPROVEMENT |
| Function coverage | 95% | 100% | NEEDS IMPROVEMENT |
| API coverage | 100% | 100% | PASSED |

### Required Remediation
- HIGH priority: 2 items (1 day)
- MEDIUM priority: 3 items (2 days)
- LOW priority: 5 items (1 day)
- **Total estimated effort:** 4 days
```

---

## Output Requirements
### Required Deliverables
1. File-level coverage report
2. Module and component coverage report
3. Function coverage report
4. Coverage gap remediation plan

---

## Cross-References
- **Previous Prompt:** PROMPT_25_VALIDATION_ENGINEERING.md
- **Next Prompt:** PROMPT_27_CROSS_REFERENCE.md
- **Shared Context Key:** validation.coverage
