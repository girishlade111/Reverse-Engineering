# PROMPT_24: Documentation Quality Validation

## Classification
- **Domain:** Documentation Generation
- **Phase:** 5 — Documentation Production
- **Prerequisites:** PROMPT_20, PROMPT_21, PROMPT_22, PROMPT_23
- **Dependencies:** All generated documentation
- **Estimated Effort:** Medium

---

## Objective

Validate the quality, completeness, consistency, and accuracy of all generated documentation against the framework's quality standards defined in QUALITY_STANDARDS.md.

---

## Input Requirements

### Required Context
- All generated documentation from PROMPT_20-23
- Quality standards from QUALITY_STANDARDS.md
- Output rules from OUTPUT_RULES.md
- All analysis artifacts for verification

---

## Pre-Analysis Checklist

- [ ] PROMPT_20, 21, 22, 23 completed
- [ ] All documentation drafts generated
- [ ] Quality standards loaded

---

## Analysis Tasks

### Task 1: Completeness Validation
**Purpose:** Verify all required documentation is complete.

**Instructions:**
1. Check each required documentation artifact:
   - Architecture documentation (PROMPT_20)
   - Technical reference (PROMPT_21)
   - Developer handbook (PROMPT_22)
   - Diagrams (PROMPT_23)
2. Verify:
   - No empty sections or TODOs
   - All required sections present
   - No missing deliverables

**Output Format:**

```
markdown
## Completeness Validation

### Documentation Coverage
| Document | Required | Generated | Complete | Missing |
|----------|----------|-----------|----------|---------|
| Architecture Overview | Yes | Yes | Yes | - |
| Module Reference | Yes | Yes | Partial | 3 modules missing |
| API Reference | Yes | Yes | Yes | - |
| Setup Guide | Yes | Yes | Yes | - |
| Rebuild Guide | Yes | Yes | Yes | - |

### Gaps Found
| Gap | Document | Severity | Action Required |
|-----|----------|----------|-----------------|
| Missing module documentation | Module Reference | MEDIUM | Add documentation for 3 modules |
| Incomplete error codes | Technical Reference | LOW | Document remaining error codes |
```

---

### Task 2: Accuracy Validation
**Purpose:** Verify documentation accuracy against source code.

**Instructions:**
1. Sample-check documentation claims against source code:
   - API endpoints match route definitions
   - Function signatures match implementation
   - Configuration defaults match actual code
   - Error codes match exception definitions
2. Flag any inaccuracies

**Output Format:**

```markdown
## Accuracy Validation

### Verified Claims
| Claim | Source | Status |
|-------|--------|--------|
| POST /api/v1/users creates user | src/api/routes/users.py:25 | CORRECT |
| User model has email field | src/data/models/user.py:15 | CORRECT |
| Default LOG_LEVEL is INFO | src/config/settings.py:20 | CORRECT |

### Inaccuracies Found
| Inaccuracy | Document | Source | Correction |
|-------------|----------|--------|------------|
| Wrong endpoint path | API Reference | src/api/routes/products.py:10 | Update to /api/v1/products |
| Incorrect default value | Configuration Reference | src/config/settings.py:25 | MAX_RETRIES default is 3, not 5 |
```

---

### Task 3: Consistency Validation
**Purpose:** Verify consistency across all documentation.

**Instructions:**
1. Check for consistency:
   - Terminology used consistently across documents
   - Same component described identically in all documents
   - Cross-references point to valid targets
   - No contradictory statements

**Output Format:**

```markdown
## Consistency Validation

### Terminology Check
| Term | Used In | Consistent? |
|------|---------|-------------|
| "OrderService" | Architecture, Technical, Developer | Yes |
| "authentication" | All documents | Yes |
| "JWT token" | API, Technical | Yes (consistent) |

### Cross-Reference Validation
| Reference | Source Document | Target | Valid? |
|-----------|----------------|--------|--------|
| See Auth Module | Architecture | modules/auth.md | Yes |
| See error codes | Technical | error_codes.md | Yes |
| See setup guide | Developer | setup_guide.md | Broken link |

### Contradictions Found
| Contradiction | Document 1 | Document 2 | Resolution |
|---------------|------------|------------|------------|
| Rate limit: 100/min | API Reference | Architecture | Both say 100/min - OK |
```

---

## Synthesis
**Purpose:** Generate quality validation report.

**Output Format:**

```markdown
## Documentation Quality Report

### Overall Assessment
| Dimension | Score | Issues |
|-----------|-------|--------|
| Completeness | 8/10 | 3 missing module docs |
| Accuracy | 9/10 | 2 inaccuracies found |
| Consistency | 9/10 | 1 broken cross-reference |
| **Overall** | **8.7/10** | **6 issues to resolve** |

### Required Fixes
- [ ] Add documentation for 3 missing modules
- [ ] Fix API endpoint path in API Reference
- [ ] Fix default value in Configuration Reference
- [ ] Fix broken cross-reference in Developer docs
```

---

## Output Requirements
### Required Deliverables
1. Completeness validation report
2. Accuracy validation report with corrections
3. Consistency validation report
4. Documentation quality score

---

## Cross-References
- **Previous Prompt:** PROMPT_23_DOCUMENTATION_DIAGRAMS.md
- **Next Prompt:** PROMPT_25_VALIDATION_ENGINEERING.md
- **Shared Context Key:** documentation.quality
