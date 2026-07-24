# Operating Rules for AI Agents

> **Document:** OPERATING_RULES.md  
> **Version:** 1.0.0  
> **Purpose:** Define mandatory operating rules for the AI agent during reverse engineering

---

## 🟥 RULE 1: NO SURFACE-LEVEL ANALYSIS

**Rule:** The AI agent must go beyond surface-level understanding for every component.

**Enforcement:**
- For every file, document not just WHAT it does, but HOW and WHY.
- For every function, trace the logic, not just the signature.
- For every class, understand the state management and behavior.
- Surface-level summaries are rejected during quality validation.

**Rationale:** Surface analysis produces documentation that is technically incorrect or dangerously misleading.

---

## 🟥 RULE 2: COMPLETE BEFORE WRITING

**Rule:** The AI agent must achieve complete understanding of the repository BEFORE writing any documentation.

**Enforcement:**
- No documentation is generated during Phases 1-8.
- Phase 9 is the ONLY phase where documentation is written.
- Earlier phases produce analysis notes, internal models, and diagrams (in draft form).
- Final documentation is a synthesis of all analysis, not a running log.

**Rationale:** Documentation written during analysis is always incomplete and often incorrect as understanding evolves.

---

## 🟥 RULE 3: VERIFY EVERY ASSUMPTION

**Rule:** Every assumption about the repository must be verified.

**Enforcement:**
- If you assume a file's purpose, verify by reading it.
- If you assume a module's responsibility, verify by examining its files.
- If you assume a design pattern, verify by checking the code structure.
- If you assume a dependency, verify by tracing imports/requires.

**Rationale:** Unverified assumptions are the #1 source of documentation errors.

---

## 🟥 RULE 4: DOCUMENT ALL FINDINGS WITH CONFIDENCE

**Rule:** Every finding must have an associated confidence level.

**Enforcement:**
- Use the confidence tracking system defined in MASTER_PROMPT.md.
- Findings below 60% confidence are marked as "unverified."
- Findings below 40% confidence are not included in final documentation.
- When confidence improves later, update the finding.

**Rationale:** Confidence tracking prevents the spread of speculative information.

---

## 🟥 RULE 5: NO SKIPPING FILES

**Rule:** Every file in the repository must be analyzed.

**Enforcement:**
- Build a complete file inventory in Phase 1.
- Track which files have been analyzed in Phase 4.
- Any file not analyzed is flagged as a gap.
- Gaps must be resolved before Phase 9.

**Exceptions:**
- Binary files that cannot be read (document their existence and purpose).
- Generated files that are clearly marked as auto-generated.
- Dependency files in `node_modules`, `vendor`, etc. (but document their presence).

**Rationale:** Skipped files create blind spots that propagate through the entire documentation.

---

## 🟥 RULE 6: CROSS-REFERENCE EVERYTHING

**Rule:** All documentation must include cross-references.

**Enforcement:**
- Every component documented must reference related components.
- Every module documented must reference its parent and child modules.
- Every workflow documented must reference the components it touches.
- Cross-references must be bidirectional where possible.

**Rationale:** Cross-references create a web of understanding that mirrors the actual codebase relationships.

---

## 🟥 RULE 7: MAINTAIN CONTEXT ACROSS PHASES

**Rule:** The AI agent must carry forward context from each phase to the next.

**Enforcement:**
- The working knowledge base is the primary context mechanism.
- Before starting each phase, review the accumulated knowledge.
- When new understanding contradicts earlier findings, update the knowledge base.
- Never start a phase with a "clean slate" mindset.

**Rationale:** Each phase builds on the previous. Context loss results in inconsistent documentation.

---

## 🟥 RULE 8: HANDLE AMBIGUITY EXPLICITLY

**Rule:** When the code is ambiguous, the AI must document the ambiguity.

**Enforcement:**
- Identify the ambiguous section.
- Document the possible interpretations.
- Note which interpretation is most likely and why.
- Flag for human review in the final documentation.

**Rationale:** Pretending ambiguity doesn't exist leads to incorrect documentation.

---

## 🟥 RULE 9: NO INFERRING EXTERNAL CONTEXT

**Rule:** Do not infer information about external systems, business requirements, or user needs unless it is evident from the code itself.

**Enforcement:**
- If the code doesn't mention a business requirement, don't invent one.
- If a comment explains the purpose, use it. If not, infer cautiously.
- Document the code as it IS, not as you think it SHOULD BE.
- Distinguish between "code intent" (what the developer intended) and "code behavior" (what the code actually does).

**Rationale:** Inventing external context introduces fiction into documentation.

---

## 🟥 RULE 10: MAINTAIN SEPARATION OF CONCERNS

**Rule:** Keep analysis artifacts separate from final documentation.

**Enforcement:**
- Analysis notes, internal models, and draft diagrams are for internal use.
- Final documentation is polished, structured, and professional.
- Do not include "thinking" or "analysis process" in final documentation.
- Do include reasoning traces where they aid understanding.

**Rationale:** Final documentation should be useful to readers, not a log of the analysis process.

---

## 🟥 RULE 11: RESPECT EXECUTION ORDER

**Rule:** Execute phases in the specified order. Do not jump ahead.

**Enforcement:**
- Phase 1 must be completed before Phase 2.
- Phase 2 must be completed before Phase 3.
- (Sequence continues through Phase 10.)
- If later analysis reveals a gap in earlier phases, pause and remediate.

**Rationale:** The phase sequence is designed to build understanding cumulatively. Breaking order creates gaps.

---

## 🟥 RULE 12: CONTINUOUS SELF-IMPROVEMENT

**Rule:** At the end of each phase, conduct a self-review.

**Enforcement:**
- Identify what was learned.
- Identify what remains unclear.
- Identify what to investigate next.
- Note any improvements to the process.
- Document lessons learned for future phases.

**Rationale:** Self-review catches gaps early and improves process quality.

---

## 🟥 RULE 13: TOOL USAGE DISCIPLINE

**Rule:** Use all available tools thoroughly and appropriately.

**Enforcement:**
- Use `list_files` to discover repository structure.
- Use `search_files` to find patterns, references, and usages.
- Use `read_file` to examine file contents completely.
- Use `execute_command` to run builds, tests, or analysis tools.
- Do not rely on a single tool for understanding.

**Rationale:** Different tools reveal different aspects of the codebase.

---

## 🟥 RULE 14: REPORT UNABLE TO COMPLETE

**Rule:** If the AI agent cannot complete a phase or task, it must clearly report:
1. What was attempted.
2. What was accomplished.
3. What could not be completed.
4. Why it could not be completed.
5. What would be needed to complete it.

**Enforcement:**
- Do not pretend to complete incomplete work.
- Do not generate placeholder content.
- Flag incomplete work for human review.

**Rationale:** Transparency about limitations allows humans to fill gaps.

---

## 🟥 RULE 15: MAINTAIN PROFESSIONAL DISCOURSE

**Rule:** All output must be professional, technically precise, and actionable.

**Enforcement:**
- Use precise technical terminology.
- Avoid vague language ("seems to," "might be," "probably").
- Use active voice.
- Be specific about file paths, function names, and line numbers.
- Structure documentation for readability.

**Rationale:** Professional documentation is taken seriously; vague documentation is ignored.

---

## ✅ COMPLIANCE CHECK

Before each phase, read these rules. After each phase, verify compliance.

**If you cannot comply with all rules, do not proceed. Flag the issue.**

