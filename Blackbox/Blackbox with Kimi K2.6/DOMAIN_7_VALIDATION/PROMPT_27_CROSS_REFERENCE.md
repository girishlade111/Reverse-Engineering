# PROMPT_27: Cross-Reference Validation & Consistency Check

## Classification
- **Domain:** Validation & Quality Assurance
- **Phase:** 6 — Final Validation
- **Prerequisites:** PROMPT_26 (Coverage Validation)
- **Dependencies:** Coverage validation results
- **Estimated Effort:** Medium

---

## Objective

Validate all cross-references across the entire analysis and documentation output, ensuring every reference points to a valid target, terminology is consistent, and there are no broken links or contradictory statements.

---

## Input Requirements

### Required Context
- All analysis artifacts from Phase 1-4
- All generated documentation from Phase 5
- Coverage validation results from PROMPT_26
- Engineering validation results from PROMPT_25

---

## Pre-Analysis Checklist
- [ ] PROMPT_25, 26 completed and corrections applied
- [ ] All documentation artifacts available for cross-reference scanning
- [ ] Shared context fully populated

---

## Analysis Tasks

### Task 1: Internal Cross-Reference Validation

**Purpose:** Validate all internal cross-references within documentation.

**Instructions:**
1. Scan all documentation for cross-references:
   - "See [Section X]" references
   - "As documented in [File Y]" references
   - "Refer to [Module Z]" references
   - "Cross-Reference: [Target]" markers
2. Verify each reference points to an existing target
3. Flag broken or ambiguous references

**Output Format:**

```markdown
## Internal Cross-Reference Validation

### Reference Inventory
| Source Document | Reference | Target | Status |
|----------------|-----------|--------|--------|
| architecture.md | See Auth Module | modules/auth.md | VALID |
| architecture.md | See data flow diagram | diagrams/data_flow.md | VALID |
| technical.md | Refer to error codes | error_codes.md | BROKEN (file not found) |
| developer.md | See setup guide | setup_guide.md | VALID |

### Broken References
| Reference | Source | Expected Target | Actual | Fix |
|-----------|--------|----------------|--------|-----|
| "See error codes" | technical.md | error_codes.md | error_reference.md | Update path |
| "As shown in diagram 3.2" | architecture.md | Figure 3.2 | No figure 3.2 | Renumber or remove |
```

---

### Task 2: Terminology Consistency Validation

**Purpose:** Validate terminology consistency across all documents.

**Instructions:**
1. Build a terminology map from all documents
2. Identify inconsistent usage:
   - Same concept with different names
   - Same name used for different concepts
   - Abbreviations used without definition
3. Flag terminology issues

**Output Format:**

```markdown
## Terminology Consistency Validation

### Terminology Map
| Concept | Preferred Term | Used In | Inconsistent Variants |
|---------|---------------|---------|----------------------|
| User authentication | "Authentication" | All docs | "Auth", "Login" (in developer.md) |
| Order processing | "OrderService" | Architecture, Technical | "Order Manager" (in developer.md) |
| Payment gateway | "PaymentGateway" | All docs | "Payment Provider" (in technical.md) |

### Inconsistencies Found
| Term | Document 1 | Document 2 | Recommended Fix |
|------|------------|------------|-----------------|
| "Auth" | architecture.md | - | Use "Authentication" consistently |
| "Order Manager" | developer.md | - | Use "OrderService" consistently |
| "Payment Provider" | technical.md | - | Use "PaymentGateway" consistently |

### Undefined Abbreviations
| Abbreviation | Used In | Definition Missing? |
|-------------|---------|---------------------|
| JWT | All docs | Defined in glossary |
| DI | architecture.md | NOT DEFINED |
| ORM | technical.md | NOT DEFINED |
```

---

### Task 3: Cross-Document Consistency Validation

**Purpose:** Validate consistency of facts across all documents.

**Instructions:**
1. Identify facts stated in multiple documents:
   - API endpoints and their behavior
   - Configuration defaults
   - Component responsibilities
   - Data model definitions
2. Compare statements across documents
3. Flag contradictions

**Output Format:**

```markdown
## Cross-Document Consistency Validation

### Fact Consistency Check
| Fact | Document 1 | Document 2 | Consistent? |
|------|------------|------------|-------------|
| POST /api/v1/users creates user | architecture.md: "Creates user" | technical.md: "Creates user" | Yes |
| Default LOG_LEVEL = INFO | technical.md: "INFO" | developer.md: "INFO" | Yes |
| Rate limit = 100/min | technical.md: "100/min" | architecture.md: "1000/min" | NO |
| MAX_RETRIES = 3 | technical.md: "3" | developer.md: "5" | NO |

### Contradictions Found
| Contradiction | Source 1 | Source 2 | Correct Value | Action |
|---------------|----------|----------|---------------|--------|
| Rate limit: 100 vs 1000 | technical.md | architecture.md | 100 (from code) | Fix architecture.md |
| MAX_RETRIES: 3 vs 5 | technical.md | developer.md | 3 (from config) | Fix developer.md |
```

---

### Task 4: External Reference Validation

**Purpose:** Validate all external references.

**Instructions:**
1. Identify external references:
   - URLs to external documentation
   - Links to external libraries
   - References to external standards
2. Verify references are valid (format check)
3. Flag potentially broken external references

**Output Format:**

```markdown
## External Reference Validation

### External References
| Reference | Type | Format Valid? | Notes |
|-----------|------|---------------|-------|
| https://stripe.com/docs/api | External API | Yes | - |
| https://fastapi.tiangolo.com/ | Framework | Yes | - |
| RFC 7519 (JWT) | Standard | Yes | - |
| https://example.com/docs | External | Yes | Verify still accessible |

### External Reference Issues
| Reference | Issue | Action |
|-----------|-------|--------|
| None found | - | - |
```

---

## Synthesis

**Output Format:**

```markdown
## Cross-Reference Validation Summary

### Overall Results
| Category | Total | Valid | Issues | Score |
|----------|-------|-------|--------|-------|
| Internal References | 47 | 45 | 2 | 95.7% |
| Terminology | 35 | 32 | 3 | 91.4% |
| Cross-Document Facts | 28 | 26 | 2 | 92.9% |
| External References | 12 | 12 | 0 | 100% |

### Required Corrections
- [ ] Fix 2 broken internal references
- [ ] Standardize 3 terminology inconsistencies
- [ ] Resolve 2 cross-document contradictions
- [ ] Define 2 undefined abbreviations (DI, ORM)
```

---

## Output Requirements
### Required Deliverables
1. Internal cross-reference validation report
2. Terminology consistency report
3. Cross-document consistency report
4. External reference validation report

---

## Cross-References
- **Previous Prompt:** PROMPT_26_VALIDATION_COVERAGE.md
- **Next Prompt:** PROMPT_28_FINAL_REVIEW.md
- **Shared Context Key:** validation.cross_reference
