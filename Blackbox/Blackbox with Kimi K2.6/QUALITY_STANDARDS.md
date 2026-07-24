# Quality Standards

## Enterprise Reverse Engineering Prompt Framework

---

## 1. Quality Framework Overview

### 1.1 Quality Dimensions

| Dimension | Definition | Measurement |
|-----------|------------|-------------|
| **Accuracy** | Factual correctness of all claims | Verified against source code |
| **Completeness** | Coverage of all repository elements | % of files/modules analyzed |
| **Traceability** | Ability to trace claims to source | Source references per claim |
| **Consistency** | Internal logical coherence | Contradiction count |
| **Clarity** | Readability and understandability | Readability score |
| **Depth** | Level of analytical detail | Depth level achieved |
| **Structure** | Organization and navigation | Navigation efficiency |

### 1.2 Quality Levels

```
Level 0: UNACCEPTABLE
├── Contains factual errors
├── Missing critical components
├── No source traceability
└── Cannot be used

Level 1: MINIMUM
├── No factual errors
├── All major components identified
├── Basic traceability
└── Usable for overview

Level 2: STANDARD
├── All claims verified
├── 90%+ file coverage
├── Full traceability
└── Usable for development

Level 3: ADVANCED
├── Deep analysis of all components
├── 100% file coverage
├── Cross-references verified
└── Usable for architecture decisions

Level 4: PRODUCTION
├── Production-ready quality
├── Rebuild-ready documentation
├── All gaps resolved
└── Usable for mission-critical decisions
```

---

## 2. Accuracy Standards

### 2.1 Factual Accuracy Requirements

| Category | Requirement | Verification Method |
|----------|-------------|-------------------|
| File paths | 100% match actual paths | Cross-check with file system |
| Function signatures | 100% match source code | Compare with source |
| Class hierarchies | 100% match inheritance | Verify in source |
| API endpoints | 100% match route definitions | Check route registrations |
| Dependencies | 100% match import statements | Verify in source files |
| Configuration | 100% match config files | Compare with actual config |
| Data types | 100% match type annotations | Verify in source |
| Return values | 100% match return statements | Check function bodies |

### 2.2 Prohibited Accuracy Violations

- Claiming functionality that doesn't exist in code
- Describing parameters not present in function signatures
- Asserting error handling where none exists
- Assuming design patterns not implemented
- Inventing configuration options not in config files
- Speculating about runtime behavior without evidence

---

## 3. Completeness Standards

### 3.1 Coverage Requirements

| Artifact | Required Coverage | Verification |
|----------|------------------|--------------|
| Source files | 100% | File count match |
| Modules | 100% | Module map completeness |
| Public APIs | 100% | API surface enumeration |
| Data models | 100% | Model definition scan |
| Routes/endpoints | 100% | Route registration scan |
| Configuration | 100% | Config file scan |
| Dependencies | 100% | Import/require scan |
| Error handlers | 100% | Error handling scan |

### 3.2 Completeness Checklist

- [ ] Every file in repository has documented purpose
- [ ] Every module has documented responsibility
- [ ] Every public function has documented signature
- [ ] Every class has documented role and relationships
- [ ] Every API endpoint has documented request/response
- [ ] Every configuration option has documented effect
- [ ] Every dependency has documented usage
- [ ] Every error path has documented behavior
- [ ] Every state transition has documented trigger
- [ ] Every event has documented handler

---

## 4. Traceability Standards

### 4.1 Source Reference Format

Every claim must include a source reference in this format:

```markdown
**Source:** `path/to/file.py` lines 42-58
**Evidence:** [Direct quote or paraphrase of relevant code]
**Confidence:** CONFIRMED | INFERRED | UNKNOWN
```

### 4.2 Traceability Requirements

| Claim Type | Required Reference | Minimum Confidence |
|------------|-------------------|-------------------|
| Architecture structure | File/module location | CONFIRMED |
| Function behavior | Function definition | CONFIRMED |
| Data flow | Data movement code | CONFIRMED |
| Design pattern | Pattern implementation | CONFIRMED |
| Error handling | Error handling code | CONFIRMED |
| Dependency relationship | Import/require statement | CONFIRMED |
| Configuration effect | Config usage code | CONFIRMED |
| Performance characteristic | Benchmark/measurement | INFERRED |
| Design rationale | Comments/patterns | INFERRED |
| Future intent | TODO/FIXME comments | INFERRED |

---

## 5. Consistency Standards

### 5.1 Internal Consistency

- No contradictory statements about the same component
- Consistent terminology across all documents
- Consistent formatting across all documents
- Consistent depth level for similar components
- Consistent reference style for source citations

### 5.2 Cross-Document Consistency

- Same component described identically in all documents
- Same terminology used across all prompts
- Same architectural model maintained throughout
- Same dependency graph referenced consistently
- Same state model used in all relevant documents

### 5.3 Consistency Verification

```markdown
## Consistency Check Results
- [ ] No internal contradictions found
- [ ] Terminology consistent across all documents
- [ ] Formatting consistent across all documents
- [ ] Cross-references verified and consistent
- [ ] Architectural model consistent across all views
```

---

## 6. Clarity Standards

### 6.1 Readability Requirements

| Document Type | Target Audience | Required Clarity Level |
|---------------|-----------------|----------------------|
| Architecture docs | Technical leads, architects | Professional technical writing |
| Developer handbook | Developers | Clear, instructional |
| API documentation | Integrators | Precise, unambiguous |
| Rebuild guide | Engineers | Step-by-step, executable |
| Error reference | Operators | Actionable, diagnostic |

### 6.2 Clarity Guidelines

- Use active voice: "The function processes data" not "Data is processed by the function"
- Keep paragraphs under 5 sentences
- Use bullet points for enumerations
- Use tables for comparisons
- Include diagrams for complex relationships
- Define acronyms on first use
- Use consistent naming conventions

---

## 7. Depth Standards

### 7.1 Depth Level Definitions

| Level | Description | Analysis Required |
|-------|-------------|-------------------|
| **D1: Surface** | File purpose and basic structure | File header, exports, public API |
| **D2: Standard** | Module behavior and key functions | All public functions, key algorithms |
| **D3: Detailed** | Complete internal logic | All functions, all code paths |
| **D4: Comprehensive** | Full analysis with edge cases | All code paths, error handling, state |
| **D5: Exhaustive** | Complete system understanding | Everything including cross-cutting concerns |

### 7.2 Depth Assignment Rules

| Component Type | Minimum Depth | Standard Depth |
|----------------|---------------|----------------|
| Core architecture | D4 | D5 |
| Business logic | D3 | D4 |
| Data access layer | D3 | D4 |
| API layer | D3 | D4 |
| Utility modules | D2 | D3 |
| Configuration | D2 | D2 |
| Third-party wrappers | D2 | D3 |
| Build/deploy scripts | D1 | D2 |
| Documentation | D1 | D1 |

---

## 8. Quality Gate Process

### 8.1 Gate Entry Criteria

Before entering the quality gate:
- [ ] All analysis prompts completed
- [ ] All required data collected
- [ ] Shared context fully populated
- [ ] No pending continuation points

### 8.2 Gate Checks

```markdown
## QUALITY GATE CHECKLIST

### Accuracy Checks
- [ ] All claims verified against source code
- [ ] No unverified assumptions presented as fact
- [ ] All source references point to existing code
- [ ] No hallucinated APIs or behaviors

### Completeness Checks
- [ ] All files in repository covered
- [ ] All modules analyzed
- [ ] All public APIs documented
- [ ] All dependencies mapped
- [ ] All error paths identified

### Traceability Checks
- [ ] Every claim has source reference
- [ ] Source references include file and line numbers
- [ ] Confidence levels assigned to all claims
- [ ] Gaps explicitly documented

### Consistency Checks
- [ ] No internal contradictions
- [ ] Terminology consistent
- [ ] Cross-references verified
- [ ] Architectural model consistent

### Clarity Checks
- [ ] Documentation is readable by target audience
- [ ] Technical terms defined
- [ ] Diagrams included for complex concepts
- [ ] Examples provided where helpful

### Depth Checks
- [ ] Core components analyzed at required depth
- [ ] Standard components at minimum depth
- [ ] No component below minimum depth
```

### 8.3 Gate Exit Criteria

Pass the quality gate only when:
- All applicable checks pass
- All failures have been corrected
- All gaps have been documented
- Final sign-off has been recorded

---

## 9. Continuous Quality Improvement

### 9.1 Quality Metrics Tracking

Track these metrics across all reverse engineering projects:
- Accuracy rate: % of claims verified as correct
- Completeness rate: % of files/modules covered
- Gap rate: % of components with unresolved gaps
- Consistency score: % of cross-references verified
- Clarity score: Readability assessment

### 9.2 Quality Improvement Process

1. Collect quality metrics from each project
2. Identify patterns in quality failures
3. Update framework prompts to address common failures
4. Add new quality checks for identified failure modes
5. Review and update quality standards quarterly
