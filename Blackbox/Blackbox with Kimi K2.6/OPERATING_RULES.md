# Operating Rules

## Enterprise Reverse Engineering Prompt Framework

---

## ⚖️ Rule 1: The Primacy of Understanding

**Rule:** The AI agent MUST achieve complete understanding of the repository BEFORE generating any documentation output.

**Enforcement:**
- No documentation generation is permitted during the analysis phase
- The agent must explicitly state "UNDERSTANDING COMPLETE" before transitioning to documentation
- If understanding is incomplete for any module, the agent must identify the gap and request additional context
- Documentation generated without complete understanding is considered a framework violation

---

## ⚖️ Rule 2: Evidence-Based Analysis

**Rule:** Every claim in the analysis and documentation MUST be traceable to specific source code artifacts.

**Enforcement:**
- Each documented element must include a source reference (file path, line numbers)
- Claims without source references are flagged as "UNVERIFIED"
- The agent must distinguish between:
  - `[CONFIRMED]` — Directly observed in source code
  - `[INFERRED]` — Logically derived from confirmed evidence
  - `[UNKNOWN]` — Cannot be determined from available code
- No `[UNKNOWN]` may be presented as fact

---

## ⚖️ Rule 3: Systematic Decomposition

**Rule:** Analysis must proceed from highest abstraction to lowest, never skipping levels.

**Enforcement Sequence:**
1. Repository-level structure and metadata
2. Directory/module-level organization
3. File-level responsibilities
4. Class/component-level interfaces
5. Function/method-level behavior
6. Statement-level algorithms
7. Cross-cutting concerns (error handling, state, events)

**Violation:** Jumping to function-level analysis before establishing architectural context.

---

## ⚖️ Rule 4: Modular Analysis Isolation

**Rule:** Each module must be analyzed independently before analyzing its interactions.

**Enforcement:**
- Define module boundaries first
- Analyze internal structure and behavior
- Map external interfaces and dependencies
- Document interactions only after internal understanding is complete
- This prevents cross-module assumption contamination

---

## ⚖️ Rule 5: Completeness Over Speed

**Rule:** No analysis step may be skipped or abbreviated due to time, length, or complexity constraints.

**Enforcement:**
- If analysis would exceed response limits, the agent must:
  1. Complete the current analysis phase
  2. Note the continuation point
  3. Request continuation in the next response
- Quality must never be sacrificed for brevity
- "Good enough" is never acceptable

---

## ⚖️ Rule 6: Self-Review Before Output

**Rule:** Before generating any final documentation, the agent must perform a structured self-review.

**Review Checklist:**
- [ ] Are all architectural layers identified?
- [ ] Are all execution paths traced?
- [ ] Are all data flows mapped?
- [ ] Are all dependencies documented?
- [ ] Are all design patterns identified?
- [ ] Are all error handling paths documented?
- [ ] Are all state transitions mapped?
- [ ] Are all external interfaces documented?
- [ ] Are all claims traceable to source code?
- [ ] Are all gaps explicitly noted?

---

## ⚖️ Rule 7: Gap Transparency

**Rule:** All gaps in understanding must be explicitly documented, never hidden or glossed over.

**Gap Documentation Format:**
```markdown
## [GAP] Description of what is unknown
- **Location:** File/Module where gap exists
- **Impact:** What analyses depend on this information
- **Severity:** CRITICAL | MAJOR | MINOR
- **Resolution Needed:** What additional information would close this gap
```

---

## ⚖️ Rule 8: Terminology Consistency

**Rule:** All terminology used in analysis and documentation must be consistent across the entire framework output.

**Enforcement:**
- Maintain a running glossary of terms discovered in the codebase
- Use the same term for the same concept throughout all documents
- When the codebase uses inconsistent terminology, document both and note the inconsistency
- Never introduce new terminology without defining it

---

## ⚖️ Rule 9: Depth Proportionality

**Rule:** Analysis depth must be proportional to the complexity and criticality of the component.

**Guidelines:**
- Core architecture components: Maximum depth (every function, every path)
- Utility modules: Standard depth (public API, key internals)
- Configuration/boilerplate: Surface depth (purpose, inputs, outputs)
- Third-party dependencies: Interface depth (how they're used, not internal implementation)

---

## ⚖️ Rule 10: No Hallucination

**Rule:** The agent must NEVER fabricate or assume code behavior that cannot be verified.

**Prohibited Actions:**
- Inventing API endpoints not found in code
- Assuming error handling where none exists
- Claiming design patterns that aren't implemented
- Describing functionality not present in source
- Generating example code that doesn't exist in the repository

**Required Action:** When encountering unclear behavior, document the ambiguity and mark it as `[UNVERIFIED]`.

---

## ⚖️ Rule 11: Cross-Reference Integrity

**Rule:** All cross-references between components, modules, and documents must be verified for accuracy.

**Enforcement:**
- Every cross-reference must point to an existing file, function, class, or module
- Dead references (pointing to non-existent entities) are prohibited
- The agent must verify cross-references during the self-review phase

---

## ⚖️ Rule 12: Framework Adherence

**Rule:** The agent must follow the framework's defined prompt sequence and not skip prompts.

**Enforcement:**
- Process prompts in the defined order
- Complete all analysis tasks in each prompt before moving to the next
- If a prompt's analysis is not applicable, document why and move on
- Never combine multiple prompts into one unless explicitly permitted

---

## ⚖️ Rule 13: Output Formatting Compliance

**Rule:** All output must comply with the formatting rules defined in `OUTPUT_RULES.md`.

**Enforcement:**
- Use specified heading levels
- Follow defined template structures
- Include required metadata blocks
- Use consistent code formatting
- Follow diagram syntax requirements

---

## ⚖️ Rule 14: Continuation Protocol

**Rule:** When analysis exceeds response limits, follow the defined continuation protocol.

**Protocol:**
1. Complete the current logical section
2. Add a continuation marker: `[CONTINUATION_POINT: {section_name}]`
3. Summarize what was completed and what remains
4. Request continuation in the next response
5. In the continuation, start by restating context from the previous response

---

## ⚖️ Rule 15: Quality Gate

**Rule:** No output may be delivered without passing through the quality gate defined in `QUALITY_STANDARDS.md`.

**Quality Gate Process:**
1. Run all applicable quality checks
2. Document check results
3. Fix any failures
4. Re-run checks until all pass
5. Only then deliver the output

---

## ⚖️ Rule 16: Repository Context Preservation

**Rule:** All analysis must be performed within the context of the entire repository, not in isolation.

**Enforcement:**
- When analyzing a file, consider its relationship to all other files
- When documenting a function, consider its callers and callees
- When describing a module, consider its consumers and dependencies
- Never analyze a component as if it exists in isolation

---

## ⚖️ Rule 17: Audit Trail

**Rule:** Maintain a complete audit trail of the analysis process.

**Audit Trail Includes:**
- Files examined (with timestamps)
- Analysis decisions made
- Gaps identified and their resolution status
- Cross-references discovered
- Assumptions and their verification status
- Quality check results

---

## ⚖️ Rule 18: Final Sign-Off

**Rule:** All documentation must receive a final sign-off confirming it meets framework standards.

**Sign-Off Requirements:**
- All quality checks passed
- All gaps documented or resolved
- All cross-references verified
- All claims traceable to source code
- All formatting rules followed
- Self-review completed
