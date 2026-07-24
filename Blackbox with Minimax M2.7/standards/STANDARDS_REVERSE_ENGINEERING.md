# Reverse Engineering Methodology Standards

> **Document:** standards/STANDARDS_REVERSE_ENGINEERING.md  
> **Version:** 1.0.0  
> **Purpose:** Define the methodology standards for the reverse engineering process  
> **When to Use:** Throughout all phases as a reference for methodology

---

## 📐 STANDARD R1: ANALYSIS METHODOLOGY

### R1.1 Top-Down Approach

The reverse engineering process follows a top-down approach:

```
Repository Level → Module Level → File Level → Function Level → Algorithm Level
```

1. **Repository Level:** Understand the overall structure and purpose.
2. **Module Level:** Understand the module boundaries and responsibilities.
3. **File Level:** Understand each file's purpose and organization.
4. **Function Level:** Understand each function's logic and behavior.
5. **Algorithm Level:** Understand complex algorithms in detail.

**Rule:** Do not jump to function-level analysis before understanding the module structure.

### R1.2 Breadth-First Before Depth-First

Within each level, use breadth-first exploration:

1. Scan all items at the current level before diving deep.
2. Build a mental map of the entire level.
3. Then deep-dive into items that require deeper analysis.

**Rule:** Do not deep-dive into one module while ignoring others at the same level.

### R1.3 Hypothesis-Driven Analysis

Form hypotheses and verify them:

1. **Hypothesize:** Based on file names, comments, or conventions, hypothesize what a component does.
2. **Verify:** Read the code to verify or refute the hypothesis.
3. **Refine:** Update understanding based on verification.
4. **Document:** Record findings with confidence level.

**Example:**
> **Hypothesis:** The `paymentService.js` file handles payment processing.
> **Verification:** Contains `processPayment()`, `refundPayment()`, `validatePayment()` functions.
> **Refined Understanding:** Confirmed — handles payment lifecycle (process, refund, validate).
> **Confidence:** 100%

---

## 📐 STANDARD R2: EVIDENCE-BASED ANALYSIS

### R2.1 Evidence Hierarchy

When making claims about the code, use the following evidence hierarchy:

| Level | Evidence Type | Examples |
|-------|--------------|----------|
| 1 | Direct Code Evidence | Function implementation, variable declaration |
| 2 | Structural Evidence | File organization, module structure |
| 3 | Convention Evidence | Naming conventions, code patterns |
| 4 | Documentation Evidence | Comments, README, existing docs |
| 5 | Inferred Evidence | Logical deduction from available evidence |

**Rule:** Level 1 evidence is always preferred. Inferred evidence must be labeled.

### R2.2 Evidence Documentation

For each key finding, document:
1. **Claim:** What you found.
2. **Evidence:** Where the evidence is located (file:line).
3. **Evidence Type:** Level 1-5 from the hierarchy.
4. **Confidence:** Based on evidence strength.

### R2.3 Contradiction Resolution

When evidence contradicts:
1. **Identify** the contradiction explicitly.
2. **Investigate** both sources of evidence.
3. **Determine** which is correct (newer code, more specific code, etc.).
4. **Document** the resolution and reasoning.
5. **Flag** if resolution is uncertain.

---

## 📐 STANDARD R3: COMPREHENSIVENESS

### R3.1 100% Coverage Rule

**Every file must be accounted for.** No file can be ignored without documented reason.

Valid reasons to skip:
- Binary files that cannot be parsed
- Auto-generated files (clearly marked)
- Third-party/vendor files in standard locations
- Build artifacts and cache directories

### R3.2 Multi-Perspective Analysis

Every significant component must be analyzed from these perspectives:

| Perspective | Questions |
|-------------|-----------|
| **Structural** | What is it? How is it organized? |
| **Behavioral** | What does it do? How does it behave? |
| **Relational** | What does it depend on? What depends on it? |
| **Temporal** | When is it created? What is its lifecycle? |
| **Qualitative** | How well is it implemented? Any issues? |

### R3.3 Error Path Coverage

Error paths must be documented with the same detail as happy paths:
- Every error condition
- Error handling logic
- Error recovery/fallback
- Error logging
- Error propagation

---

## 📐 STANDARD R4: CONFIDENCE MANAGEMENT

### R4.1 Confidence Levels

| Level | Label | Definition | Action |
|-------|-------|------------|--------|
| 100% | Confirmed | Multiple independent evidence sources | Include in docs without caveat |
| 80% | Likely | Strong evidence, single source | Include, note as "likely" |
| 60% | Probable | Reasonable inference | Include with confidence note |
| 40% | Possible | Speculative, limited evidence | Only include if critical, mark as "unverified" |
| 20% | Uncertain | Guess | Do not include in final docs |

### R4.2 Confidence Escalation

When confidence is < 80%:
1. Attempt to find additional evidence.
2. If additional evidence found, update confidence.
3. If no additional evidence, mark as uncertain.
4. Flag for human review.

### R4.3 Confidence Documentation

In the documentation:
- Include a "Confidence Assessment" section in each document.
- For individual findings, note confidence inline: `(Confidence: 80%)`
- For the overall document, provide an aggregate confidence.
- Document what would improve confidence.

---

## 📐 STANDARD R5: CROSS-VALIDATION

### R5.1 Internal Cross-Validation

Cross-validate findings from different phases:
- Phase 2 module structure vs. Phase 4 file analysis.
- Phase 3 dependency analysis vs. Phase 5 architecture.
- Phase 6 workflow analysis vs. Phase 4 function analysis.

**Rule:** Contradictions between phases must be resolved before documentation.

### R5.2 External Cross-Validation

When available, cross-validate against:
- Existing documentation (README, wiki, API docs)
- Code comments
- Issue tracker / commit messages
- Test files

### R5.3 The "Telephone" Rule

Information degrades when passed through multiple intermediaries. Therefore:
- Prefer primary sources (code) over secondary sources (comments).
- Prefer secondary sources (comments) over tertiary sources (external docs).
- Document the evidence chain: "Claim X is based on code evidence Y in file Z."

---

## 📐 STANDARD R6: DOCUMENTATION INTEGRITY

### R6.1 No Speculation in Final Output

**Final documentation must not contain speculation.**
- If something is unclear, document that it's unclear.
- If something is inferred, label it as inferred.
- If something is unknown, mark it as unknown.
- Never present speculation as fact.

### R6.2 Traceability

Every significant claim in documentation must be traceable to:
- Source code (preferred), or
- Configuration evidence, or
- Clear logical inference from the above

**Traceability format:** `[Claim] → [Evidence at file:line]`

### R6.3 Completeness Over Polish

In the analysis phases, completeness is more important than polish:
- Raw analysis notes are acceptable during phases 1-8.
- Polish is applied during Phase 9 documentation synthesis.
- Do not sacrifice completeness for presentation during analysis.

---

## 📐 STANDARD R7: ITERATIVE DEEPENING

### R7.1 Progressive Analysis

Understanding deepens with each phase:
- Phase 1: Surface understanding (file names, sizes)
- Phase 2: Structural understanding (module organization)
- Phase 3: Relational understanding (dependencies)
- Phase 4: Deep understanding (code logic)
- Phases 5-8: Architectural and behavioral understanding

**Rule:** Earlier phases may be revisited as understanding deepens.

### R7.2 The "5 Whys" Technique

For complex components, use the "5 Whys" technique:
1. Why does this component exist? → To handle X.
2. Why is X handled this way? → Because of Y constraint.
3. Why does Y constraint exist? → Because of Z requirement.
4. etc.

This reveals the design reasoning behind the code.

### R7.3 Unknown Unknowns

Be aware of "unknown unknowns" — things you don't know that you don't know.
- When exploring a new area, ask: "What might I be missing?"
- Look for: files in unusual locations, unconventional patterns, comments referencing removed code.
- Document areas where the analysis might be incomplete.

---

## ✅ COMPLIANCE CHECK

- [ ] Top-down approach followed
- [ ] Breadth-first before depth-first
- [ ] Hypothesis-driven analysis used
- [ ] Evidence base for all claims
- [ ] 100% file coverage (or documented exceptions)
- [ ] Multi-perspective analysis done
- [ ] Error paths documented
- [ ] Confidence levels assigned
- [ ] Cross-validation performed
- [ ] No speculation in final output
- [ ] Traceability maintained
- [ ] Iterative deepening applied

