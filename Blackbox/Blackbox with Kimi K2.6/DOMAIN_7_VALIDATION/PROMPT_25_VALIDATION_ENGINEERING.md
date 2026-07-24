# PROMPT_25: Engineering Validation & Accuracy Verification

## Classification
- **Domain:** Validation & Quality Assurance
- **Phase:** 6 — Final Validation
- **Prerequisites:** All Phase 1-5 prompts (01-24)
- **Dependencies:** Complete analysis and generated documentation
- **Estimated Effort:** High

---

## Objective

Perform a comprehensive engineering validation of all analysis findings and generated documentation against the actual source code, verifying technical accuracy, completeness, and correctness of every claim.

---

## Input Requirements

### Required Context
- All analysis artifacts from Phase 1-4
- All generated documentation from Phase 5
- Complete source code access for verification

---

## Pre-Analysis Checklist
- [ ] All Phase 1-5 prompts completed
- [ ] Documentation quality validated (PROMPT_24)
- [ ] Source code available for cross-verification

---

## Analysis Tasks

### Task 1: Structural Validation

**Purpose:** Verify structural claims against actual source code.

**Instructions:**
1. Validate directory structure claims:
   - Verify documented directories exist
   - Verify documented files exist at specified paths
   - Verify module boundaries match actual structure
2. Validate architectural claims:
   - Verify documented architecture layers exist
   - Verify component relationships
   - Verify dependency directions

**Output Format:**

```markdown
## Structural Validation

### Directory Structure Verification
| Claimed Path | Exists? | Matches Documentation? | Notes |
|-------------|---------|----------------------|-------|
| src/api/ | Yes | Yes | - |
| src/core/services/ | Yes | Yes | - |
| src/legacy/ | Yes | No | Not mentioned in architecture docs |

### Architecture Claims Verification
| Claim | Evidence | Status | Correction |
|-------|----------|--------|------------|
| Layered architecture with 4 layers | src/api/, src/core/, src/data/ | CONFIRMED | - |
| Repository pattern used | IRepository interface + implementations | CONFIRMED | - |
| Dependency Injection in use | DI container at src/di/ | CONFIRMED | - |
```

---

### Task 2: Behavioral Validation

**Purpose:** Verify behavioral claims against actual code execution.

**Instructions:**
1. Validate API endpoint claims:
   - Verify documented endpoints exist in route definitions
   - Verify HTTP methods match
   - Verify request/response schemas match
2. Validate data flow claims:
   - Trace documented flows through actual code
   - Verify transformation steps
   - Verify error handling at each step

**Output Format:**

```markdown
## Behavioral Validation

### API Endpoint Verification
| Endpoint | Method | Documented | Actual | Match? |
|----------|--------|------------|--------|--------|
| /api/v1/users | POST | Yes | src/api/routes/users.py:25 | Yes |
| /api/v1/users/{id} | GET | Yes | src/api/routes/users.py:45 | Yes |
| /api/v1/products | GET | Yes | - | NOT FOUND |

### Data Flow Verification
| Flow | Steps Documented | Steps Verified | Match? |
|------|-----------------|----------------|--------|
| User Registration | 7 | 7 | Yes |
| Order Placement | 8 | 8 | Yes |
| Payment Processing | 4 | 3 | No (missing error handling step) |
```

---

### Task 3: Documentation Accuracy Score

**Purpose:** Calculate overall documentation accuracy score.

**Instructions:**
1. Sample-check documentation claims (minimum 20% of claims)
2. Calculate accuracy metrics:
   - Claim accuracy rate
   - Completeness rate
   - Consistency score
3. Identify patterns in inaccuracies

**Output Format:**

```markdown
## Documentation Accuracy Score

### Accuracy Metrics
| Metric | Result | Target | Gap |
|--------|--------|--------|-----|
| Claim Accuracy | 94% | 100% | 6% |
| Completeness | 96% | 100% | 4% |
| Consistency | 92% | 100% | 8% |

### Error Pattern Analysis
| Pattern | Frequency | Root Cause | Recommendation |
|---------|-----------|------------|----------------|
| Outdated API paths | 3 occurrences | Recent refactoring | Regenerate API docs after refactors |
| Missing files | 2 occurrences | Documentation not updated | Add file verification step |
```

---

## Synthesis

**Output Format:**

```markdown
## Engineering Validation Report

### Summary
| Category | Verified | Failed | Score |
|----------|----------|--------|-------|
| Structural | 45 | 3 | 94% |
| Behavioral | 32 | 2 | 94% |
| Data Flow | 28 | 1 | 96% |

### Required Corrections
- [ ] Update architecture docs to include legacy module
- [ ] Fix missing products endpoint documentation
- [ ] Add missing error handling step to payment flow
```

---

## Output Requirements
### Required Deliverables
1. Structural validation report
2. Behavioral validation report
3. Documentation accuracy scoring

---

## Cross-References
- **Previous Prompt:** PROMPT_24_DOCUMENTATION_QUALITY.md
- **Next Prompt:** PROMPT_26_VALIDATION_COVERAGE.md
- **Shared Context Key:** validation.engineering
