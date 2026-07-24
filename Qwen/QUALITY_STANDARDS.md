# QUALITY STANDARDS

## Quality Metrics and Standards for Reverse Engineering Output

---

## 1. QUALITY DIMENSIONS

### 1.1 Accuracy

**Definition:** The degree to which documentation correctly represents the actual code.

**Metrics:**

| Metric | Target | Measurement |
|--------|--------|-------------|
| Claim Accuracy | 100% | All claims verifiable against code |
| Diagram Accuracy | 100% | All diagram elements exist in code |
| Reference Accuracy | 100% | All file references resolve correctly |
| Technical Accuracy | 100% | No technical errors or misconceptions |

**Quality Indicators:**
- ✅ Every statement traceable to specific code
- ✅ All file paths are valid
- ✅ All line numbers are accurate
- ✅ All code snippets match source
- ✅ All technical terms used correctly

**Anti-Indicators:**
- ❌ Unsupported assertions
- ❌ Broken file references
- ❌ Incorrect line numbers
- ❌ Misquoted code
- ❌ Misused terminology

### 1.2 Completeness

**Definition:** The degree to which all required elements are documented.

**Metrics:**

| Metric | Target | Measurement |
|--------|--------|-------------|
| File Coverage | 100% | All non-generated files analyzed |
| Component Coverage | 100% | All components documented |
| Interface Coverage | 100% | All public APIs documented |
| Flow Coverage | 95%+ | All major execution paths covered |

**Quality Indicators:**
- ✅ Complete file inventory
- ✅ All components documented
- ✅ All entry points identified
- ✅ All dependencies mapped
- ✅ All configurations documented

**Anti-Indicators:**
- ❌ Undocumented files
- ❌ Missing components
- ❌ Unidentified entry points
- ❌ Unmapped dependencies
- ❌ Undocumented configurations

### 1.3 Clarity

**Definition:** The degree to which documentation is easily understood.

**Metrics:**

| Metric | Target | Measurement |
|--------|--------|-------------|
| Readability Score | 50+ Flesch | Technical audience appropriate |
| Terminology Consistency | 100% | Same term = same meaning |
| Structural Clarity | High | Clear hierarchy and organization |
| Explanation Quality | High | Concepts explained appropriately |

**Quality Indicators:**
- ✅ Clear, concise writing
- ✅ Consistent terminology
- ✅ Logical structure
- ✅ Appropriate detail level
- ✅ Helpful examples

**Anti-Indicators:**
- ❌ Ambiguous language
- ❌ Inconsistent terminology
- ❌ Poor organization
- ❌ Too much/too little detail
- ❌ Missing examples

### 1.4 Usefulness

**Definition:** The degree to which documentation enables its intended purposes.

**Purposes:**
1. Enable new developers to understand the system
2. Support maintenance and debugging
3. Facilitate architectural decisions
4. Enable system reconstruction
5. Support knowledge transfer

**Quality Indicators:**
- ✅ New developer can navigate codebase
- ✅ Maintainer can find relevant code
- ✅ Architect can understand decisions
- ✅ System could be rebuilt from docs
- ✅ Knowledge is captured effectively

**Validation Questions:**
- Could a new senior developer understand this system in 2 days using only this documentation?
- Could the system be rebuilt from scratch using this documentation?
- Can any file's purpose be determined from the documentation?
- Can any function's behavior be predicted from the documentation?

---

## 2. DOCUMENT QUALITY STANDARDS

### 2.1 Architecture Documentation

**Required Elements:**

| Element | Quality Standard |
|---------|------------------|
| Architecture Overview | Clear description of overall architecture pattern |
| Component Diagram | Accurate representation of components and relationships |
| Component Descriptions | Each component has clear responsibility definition |
| Communication Patterns | All inter-component communication documented |
| Design Decisions | Key decisions documented with rationale |
| Trade-offs | Alternatives considered and why rejected |

**Quality Checklist:**
- [ ] Architecture pattern clearly identified
- [ ] All major components shown in diagram
- [ ] Component boundaries are clear
- [ ] Communication mechanisms specified
- [ ] Design decisions have evidence
- [ ] Trade-offs are documented

### 2.2 Code Documentation

**Required Elements:**

| Element | Quality Standard |
|---------|------------------|
| File Inventory | Complete list with responsibilities |
| Class Documentation | All classes with methods and properties |
| Function Documentation | Purpose, parameters, return, side effects |
| Type Definitions | All types with usage examples |
| Import/Export Maps | All module boundaries clear |

**Quality Checklist:**
- [ ] Every file has documented purpose
- [ ] All classes have API documentation
- [ ] Complex functions are explained
- [ ] Types are defined and exemplified
- [ ] Module boundaries are clear

### 2.3 Flow Documentation

**Required Elements:**

| Element | Quality Standard |
|---------|------------------|
| Data Flow Diagram | Shows all data movement |
| Control Flow Diagram | Shows execution paths |
| Sequence Diagrams | Complex interactions shown |
| State Transitions | State changes documented |
| Error Flows | Error handling paths shown |

**Quality Checklist:**
- [ ] Entry points identified
- [ ] Main flows documented
- [ ] Alternative flows shown
- [ ] Error paths included
- [ ] State changes traced

### 2.4 Dependency Documentation

**Required Elements:**

| Element | Quality Standard |
|---------|------------------|
| External Dependencies | Complete list with versions and purposes |
| Internal Dependencies | Dependency graph between modules |
| Circular Dependencies | Identified and explained |
| Dependency Health | Outdated/problematic deps flagged |

**Quality Checklist:**
- [ ] All external dependencies listed
- [ ] All internal dependencies mapped
- [ ] Circular dependencies identified
- [ ] Problem dependencies flagged

---

## 3. DIAGRAM QUALITY STANDARDS

### 3.1 General Diagram Standards

**All diagrams must:**

- Accurately represent the code
- Use standard notation (UML, Mermaid)
- Include legend if non-standard symbols used
- Be readable at standard zoom
- Have descriptive titles
- Include creation/update date

**Diagram Validation:**
```
For each element in diagram:
  1. Verify element exists in code
  2. Verify relationship exists in code
  3. Verify direction/orientation is correct
  4. Verify labels match code names
```

### 3.2 Component Diagram Standards

**Required Information:**
- All major components
- Component boundaries
- Inter-component connections
- Connection types (sync/async, direct/message)
- External systems

**Quality Checks:**
- [ ] No orphaned components (unless truly isolated)
- [ ] All connections bidirectionally verified
- [ ] Component names match code
- [ ] Connection types accurate

### 3.3 Sequence Diagram Standards

**Required Information:**
- All participants (objects/services)
- Message flow in time order
- Synchronous vs asynchronous calls
- Return values
- Error scenarios

**Quality Checks:**
- [ ] Time ordering is correct
- [ ] All messages exist in code
- [ ] Sync/async distinction accurate
- [ ] Error scenarios included

### 3.4 Data Flow Diagram Standards

**Required Information:**
- Data sources
- Data destinations
- Data transformations
- Data stores
- Data formats

**Quality Checks:**
- [ ] All data sources identified
- [ ] All transformations documented
- [ ] Data formats specified
- [ ] Flow directions correct

---

## 4. EVIDENCE QUALITY STANDARDS

### 4.1 Evidence Requirements

**Every claim requires:**

| Claim Type | Required Evidence |
|------------|-------------------|
| Architecture | Multiple file references showing pattern |
| Component | File(s) implementing component |
| Dependency | Import statements or usage |
| Behavior | Code showing implementation |
| Configuration | Config file excerpts |
| API | Route definitions or interface declarations |

### 4.2 Evidence Format

**Standard Evidence Format:**
```
CLAIM: [What is being claimed]
EVIDENCE_TYPE: [File content / Import / Pattern / etc.]
LOCATION: path/to/file.ext, lines X-Y
EXCERPT: [Relevant code snippet if clarifying]
CONFIDENCE: [Certain/Confident/Probable/Speculative]
```

**Example:**
```
CLAIM: UserService depends on DatabaseService
EVIDENCE_TYPE: Import statement
LOCATION: src/services/user.service.ts, line 5
EXCERPT: import { DatabaseService } from './database.service';
CONFIDENCE: Certain
```

### 4.3 Evidence Quality Checks

- [ ] Evidence directly supports claim
- [ ] Location is precise and accurate
- [ ] Excerpt is not misleading
- [ ] Confidence level is appropriate
- [ ] No cherry-picking (show representative evidence)

---

## 5. WRITING QUALITY STANDARDS

### 5.1 Technical Writing Standards

**Clarity:**
- Use active voice
- Prefer simple sentences
- Define acronyms on first use
- Avoid ambiguous qualifiers

**Precision:**
- Use specific numbers, not "several", "many"
- Name specific files, not "various files"
- Cite exact locations, not "somewhere in"
- Distinguish fact from inference

**Consistency:**
- Use same term for same concept
- Follow established naming
- Maintain formatting consistency
- Apply style guide uniformly

### 5.2 Tone Standards

**Appropriate Tone:**
- Professional and objective
- Confident but not arrogant
- Acknowledge uncertainty when present
- Focus on facts over opinions

**Inappropriate Tone:**
- Casual or conversational
- Speculative without labeling
- Opinion presented as fact
- Emotional language

### 5.3 Structure Standards

**Document Structure:**
- Clear hierarchical headings
- Logical flow between sections
- Consistent section patterns
- Helpful cross-references

**Paragraph Structure:**
- One main idea per paragraph
- Topic sentence first
- Supporting details follow
- Transition to next paragraph

---

## 6. VALIDATION PROCEDURES

### 6.1 Self-Validation

**Before submitting any output:**

```markdown
SELF-VALIDATION CHECKLIST:

ACCURACY:
- [ ] Every claim verified against code
- [ ] All file references checked
- [ ] All diagrams match code structure
- [ ] No unsupported assertions

COMPLETENESS:
- [ ] All required sections present
- [ ] All files in scope covered
- [ ] All questions answered
- [ ] Nothing marked TODO

CLARITY:
- [ ] Writing is clear and precise
- [ ] Terminology is consistent
- [ ] Structure is logical
- [ ] Examples are helpful

EVIDENCE:
- [ ] All claims have evidence
- [ ] Evidence is properly formatted
- [ ] Evidence locations are accurate
- [ ] Confidence levels are appropriate
```

### 6.2 Peer Review Simulation

**Before finalizing, ask:**

1. Would a skeptical reviewer accept this claim with this evidence?
2. Is there any way this could be misinterpreted?
3. What questions would a reviewer ask?
4. Have I preemptively answered those questions?
5. Is the documentation better than what I would expect to receive?

### 6.3 Quality Scoring

**Rate each dimension 1-5:**

| Dimension | Score | Criteria |
|-----------|-------|----------|
| Accuracy | _/5 | 5 = 100% accurate, 1 = multiple errors |
| Completeness | _/5 | 5 = nothing missing, 1 = major gaps |
| Clarity | _/5 | 5 = crystal clear, 1 = confusing |
| Usefulness | _/5 | 5 = highly useful, 1 = not useful |
| Evidence | _/5 | 5 = all claims evidenced, 1 = unsupported |

**Minimum Acceptable Scores:**
- Accuracy: 5 (non-negotiable)
- Completeness: 4
- Clarity: 4
- Usefulness: 4
- Evidence: 5 (non-negotiable)

**If any score below minimum:**
1. Identify specific deficiencies
2. Create improvement plan
3. Revise output
4. Re-score

---

## 7. CONTINUOUS IMPROVEMENT

### 7.1 Learning Loop

**After each prompt:**
1. Review what went well
2. Identify what could be improved
3. Document lessons learned
4. Apply improvements to next prompt

### 7.2 Pattern Recognition

**Track patterns of:**
- Common mistakes to avoid
- Effective approaches to repeat
- Efficient techniques to reuse
- Quality issues to watch for

### 7.3 Framework Feedback

**Report framework issues:**
- Unclear instructions
- Missing guidance
- Contradictory requirements
- Improvement suggestions

---

## 8. QUALITY ESCALATION

### 8.1 When Quality Cannot Be Achieved

**If quality standards cannot be met:**

1. **Document the limitation**
   - What standard cannot be met
   - Why it cannot be met
   - What impact this has

2. **Propose alternatives**
   - Best achievable alternative
   - Trade-offs involved
   - Recommendation

3. **Flag for review**
   - Mark section clearly
   - Explain situation
   - Request human review if critical

### 8.2 Quality Issue Template

```
QUALITY ISSUE IDENTIFIED:
Standard: [Which standard cannot be met]
Reason: [Why it cannot be met]
Impact: [What this means for documentation]
Alternative: [Best alternative approach]
Recommendation: [Suggested action]
```

---

## 9. QUALITY CERTIFICATION

### 9.1 Final Quality Certification

**Before marking task complete:**

```
QUALITY CERTIFICATION:

I certify that this reverse engineering documentation:

ACCURACY:
- [ ] Contains no known factual errors
- [ ] All claims are evidenced
- [ ] All references are valid

COMPLETENESS:
- [ ] Covers all required elements
- [ ] Addresses all prompt requirements
- [ ] Includes all necessary diagrams

CLARITY:
- [ ] Is written clearly and precisely
- [ ] Uses consistent terminology
- [ ] Has logical structure

USEFULNESS:
- [ ] Enables system understanding
- [ ] Supports maintenance activities
- [ ] Captures key knowledge

EVIDENCE:
- [ ] Provides evidence for all claims
- [ ] Evidence is properly formatted
- [ ] Evidence locations are accurate

CERTIFIER: [AI Agent ID]
DATE: [Certification Date]
FRAMEWORK_VERSION: [Version Used]
```

---

*These quality standards define the expected quality level for all reverse engineering outputs. All documentation must meet or exceed these standards.*
