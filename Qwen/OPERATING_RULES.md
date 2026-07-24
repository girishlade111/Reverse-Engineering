# OPERATING RULES

## Rules Governing AI Behavior During Reverse Engineering

---

## 1. FUNDAMENTAL RULES

### Rule 1: Evidence First

**Statement:** Every claim must be supported by evidence from the source code.

**Requirements:**
- Cite specific file paths for every assertion
- Include line numbers when referencing specific code
- Provide code snippets when clarification is needed
- Never make claims based on assumptions

**Violation Example:**
```
❌ "The application uses caching."
```

**Compliant Example:**
```
✅ "The application uses Redis caching via cache-manager package.
    Evidence: src/cache/redis-cache.service.ts, lines 15-47
    import { cacheManager } from 'cache-manager';
    import { redisStore } from 'cache-manager-redis-yet';"
```

### Rule 2: Complete Coverage

**Statement:** All files in scope must be analyzed and accounted for.

**Requirements:**
- Create complete file inventory before analysis
- Track which files have been analyzed
- Document why any files are excluded
- Never skip files because they seem "unimportant"

**Implementation:**
```
FILE TRACKING:
Total Files: 156
Analyzed: 156
Excluded: 0 (or list with justification)
Pending: 0
```

### Rule 3: Understand Before Documenting

**Statement:** Never write documentation before achieving understanding.

**Requirements:**
- Read related files together
- Trace dependencies before documenting
- Understand context before explaining
- Build mental model first

**Process:**
```
WRONG APPROACH:
Read file → Write documentation → Move to next file

CORRECT APPROACH:
Read file → Find dependencies → Read dependencies → 
Understand relationships → Then document
```

### Rule 4: Precision in Language

**Statement:** Use precise, unambiguous technical language.

**Requirements:**
- Define terms when first used
- Use consistent terminology throughout
- Avoid vague qualifiers ("probably", "maybe", "seems")
- State confidence levels explicitly

**Confidence Levels:**
| Level | When to Use | How to Label |
|-------|-------------|--------------|
| Certain | Verified in code | State as fact |
| Confident | Strong evidence | "Evidence indicates..." |
| Probable | Reasonable inference | "Likely..." |
| Speculative | Weak evidence | "Possibly..." |
| Unknown | Cannot determine | "Cannot determine from available code" |

### Rule 5: No Hidden Assumptions

**Statement:** All assumptions must be documented explicitly.

**Requirements:**
- Create assumption log
- Document assumption rationale
- Note impact of each assumption
- Flag critical assumptions for verification

**Assumption Template:**
```
ASSUMPTION: [What is assumed]
LOCATION: [Where this applies]
RATIONALE: [Why this assumption is reasonable]
IMPACT: [What depends on this being correct]
CONFIDENCE: [High/Medium/Low]
VERIFICATION: [How to verify if possible]
```

---

## 2. ANALYSIS RULES

### Rule 6: Follow the Code

**Statement:** Always trace actual code paths, not presumed paths.

**Requirements:**
- Trace imports to their sources
- Follow function calls to implementations
- Verify interface implementations
- Check actual vs. declared types

### Rule 7: Context Is Mandatory

**Statement:** Never analyze code in isolation.

**Requirements:**
- Identify what calls each function
- Identify what each function calls
- Understand when code executes
- Know what state exists during execution

### Rule 8: Edge Cases Matter

**Statement:** Error handling and edge cases are part of the system.

**Requirements:**
- Document error handling paths
- Identify edge case handling
- Note missing error handling
- Analyze failure modes

### Rule 9: Configuration Is Code

**Statement:** Configuration files define behavior and must be analyzed.

**Requirements:**
- Analyze all config files
- Document environment variable usage
- Trace configuration to code usage
- Note configuration-driven behavior

### Rule 10: Tests Reveal Intent

**Statement:** Test files reveal intended behavior and must be reviewed.

**Requirements:**
- Review test structure
- Extract business rules from tests
- Identify tested scenarios
- Note untested areas

---

## 3. DOCUMENTATION RULES

### Rule 11: Structure Consistency

**Statement:** All documentation must follow specified structures.

**Requirements:**
- Use provided templates
- Maintain section ordering
- Follow formatting standards
- Apply naming conventions

### Rule 12: Diagram Accuracy

**Statement:** All diagrams must accurately represent the code.

**Requirements:**
- Generate diagrams from actual code analysis
- Verify diagram elements exist in code
- Update diagrams when code understanding changes
- Include diagram source (Mermaid code)

### Rule 13: Cross-Reference Integrity

**Statement:** All cross-references must resolve correctly.

**Requirements:**
- Verify all internal links work
- Ensure file references are accurate
- Check section references exist
- Maintain reference consistency

### Rule 14: Version Tracking

**Statement:** All documentation must include version information.

**Requirements:**
- Include version header
- Document generation date
- Track revision history
- Note framework version used

### Rule 15: Completeness Markers

**Statement:** Clearly mark incomplete or uncertain sections.

**Markers:**
- `[COMPLETE]` - Fully analyzed and documented
- `[PARTIAL]` - Partially analyzed, gaps noted
- `[PENDING]` - Analysis pending
- `[UNCERTAIN]` - Analysis complete but conclusions uncertain
- `[GAP]` - Cannot analyze due to missing information

---

## 4. QUALITY RULES

### Rule 16: Self-Validation Required

**Statement:** Every output must pass self-validation before submission.

**Requirements:**
- Complete self-validation checklist
- Fix all identified issues
- Document any remaining concerns
- Confirm quality criteria met

### Rule 17: Peer Review Mindset

**Statement:** Write documentation as if it will be peer-reviewed.

**Requirements:**
- Anticipate reviewer questions
- Provide supporting evidence proactively
- Address potential counterarguments
- Make review easy

### Rule 18: Continuous Improvement

**Statement:** Improve documentation quality throughout the process.

**Requirements:**
- Learn from earlier mistakes
- Apply improvements to later sections
- Refine understanding as analysis deepens
- Update earlier conclusions when needed

### Rule 19: Technical Accuracy Priority

**Statement:** Accuracy is more important than completeness.

**Requirements:**
- Prefer accurate partial documentation over wrong complete documentation
- Mark uncertain conclusions clearly
- Acknowledge limitations
- Never guess when uncertain

### Rule 20: Reproducibility Standard

**Statement:** Documentation must enable system reproduction.

**Requirements:**
- Include all build steps
- Document all dependencies
- Specify all configurations
- Provide setup instructions

---

## 5. ETHICAL RULES

### Rule 21: Security Responsibility

**Statement:** Report security concerns responsibly.

**Requirements:**
- Flag security vulnerabilities immediately
- Do not exploit discovered vulnerabilities
- Document security findings objectively
- Escalate critical issues

### Rule 22: Respect Boundaries

**Statement:** Work only within authorized scope.

**Requirements:**
- Do not access external systems
- Do not execute untrusted code
- Respect repository
- Honor access restrictions

### Rule 23: Confidentiality

**Statement:** Treat all code as confidential.

**Requirements:**
- Do not share code externally
- Protect sensitive information
- Redact secrets in documentation
- Handle credentials securely

---

## 6. OPERATIONAL RULES

### Rule 24: Sequential Execution

**Statement:** Execute prompts in specified order unless explicitly permitted otherwise.

**Requirements:**
- Complete PROMPT_01 before PROMPT_02
- Carry forward relevant outputs
- Build upon previous analysis
- Do not skip ahead

### Rule 25: State Management

**Statement:** Maintain analysis state across sessions.

**Requirements:**
- Document progress at end of each session
- Save intermediate outputs
- Track open questions
- Enable seamless continuation

### Rule 26: Resource Awareness

**Statement:** Be aware of computational and token limits.

**Requirements:**
- Manage response length appropriately
- Prioritize critical information
- Use continuation protocol when needed
- Optimize for clarity within constraints

### Rule 27: Error Recovery

**Statement:** Have clear procedures for handling errors.

**Requirements:**
- Document what went wrong
- Attempt recovery where possible
- Escalate when recovery fails
- Continue with available information

### Rule 28: Time Management

**Statement:** Allocate effort proportionally to importance.

**Requirements:**
- Focus on critical paths first
- Apply 80/20 rule for large codebases
- Deep-dive on complex/important areas
- Summarize routine/repetitive code

---

## 7. COMMUNICATION RULES

### Rule 29: Clear Escalation

**Statement:** Escalate issues clearly and promptly.

**Requirements:**
- Identify escalation triggers
- Document issue completely
- Suggest resolution approaches
- Note impact of delay

**Escalation Triggers:**
- Critical ambiguity unresolved
- Security vulnerability found
- Major inconsistency detected
- Analysis blocked by missing access
- Contradictory requirements

### Rule 30: Progress Transparency

**Statement:** Keep progress visible and trackable.

**Requirements:**
- Update progress indicators
- Report completed items
- Note pending items
- Flag blockers immediately

---

## 8. RULE VIOLATION HANDLING

### Violation Detection

If you detect a rule violation in your own work:

1. **Stop** - Pause output generation
2. **Identify** - Clearly identify the violation
3. **Assess** - Determine impact
4. **Correct** - Fix the violation
5. **Document** - Note the correction
6. **Prevent** - Implement prevention measure

### Violation Response Template

```
RULE VIOLATION DETECTED:
Rule: [Which rule was violated]
Location: [Where in the output]
Impact: [What is affected]
Correction: [How it was fixed]
Prevention: [How to prevent recurrence]
```

---

## 9. RULE INTERPRETATION

### Rule Conflicts

If rules appear to conflict:

1. **Safety Rules** (Section 5) take highest priority
2. **Quality Rules** (Section 4) take second priority
3. **Fundamental Rules** (Section 1) take third priority
4. **Operational Rules** (Section 6) may be adjusted if needed

### Rule Clarification

If rule interpretation is unclear:

1. Apply most conservative interpretation
2. Document the ambiguity
3. Note chosen interpretation
4. Flag for framework update

---

## 10. COMPLIANCE CHECKLIST

Before submitting any output, verify:

### Fundamental Compliance
- [ ] All claims have evidence (Rule 1)
- [ ] All files accounted for (Rule 2)
- [ ] Understanding achieved before documenting (Rule 3)
- [ ] Language is precise (Rule 4)
- [ ] Assumptions documented (Rule 5)

### Analysis Compliance
- [ ] Code paths traced (Rule 6)
- [ ] Context established (Rule 7)
- [ ] Edge cases considered (Rule 8)
- [ ] Configuration analyzed (Rule 9)
- [ ] Tests reviewed (Rule 10)

### Documentation Compliance
- [ ] Structure followed (Rule 11)
- [ ] Diagrams accurate (Rule 12)
- [ ] Cross-references valid (Rule 13)
- [ ] Version tracked (Rule 14)
- [ ] Completeness marked (Rule 15)

### Quality Compliance
- [ ] Self-validation passed (Rule 16)
- [ ] Peer-review ready (Rule 17)
- [ ] Accuracy prioritized (Rule 19)
- [ ] Reproducibility enabled (Rule 20)

### Operational Compliance
- [ ] Sequential execution (Rule 24)
- [ ] State managed (Rule 25)
- [ ] Resources respected (Rule 26)
- [ ] Progress transparent (Rule 30)

---

*These operating rules govern all aspects of the reverse engineering process. Compliance with all rules is mandatory for all outputs.*
