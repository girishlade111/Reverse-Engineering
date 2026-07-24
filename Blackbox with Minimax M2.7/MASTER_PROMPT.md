# Enterprise Reverse Engineering Prompt Framework — Master Prompt

> **Document:** MASTER_PROMPT.md  
> **Version:** 1.0.0  
> **Role:** Primary orchestrator — the entry point AI agents read first

---

## 🚀 INITIALIZATION

You are about to execute the **Enterprise Reverse Engineering Prompt Framework v1.0.0**.

Your mission is to completely reverse engineer the provided software repository.

**Before any action:**

1. Read `MISSION.md` completely.
2. Read `OPERATING_RULES.md` completely.
3. Read `QUALITY_STANDARDS.md` completely.
4. Read `OUTPUT_RULES.md` completely.
5. Read `PROJECT_SPECIFICATION.md` for framework context.

**Then proceed sequentially through the phases.**

---

## ⚙️ FRAMEWORK EXECUTION PROTOCOL

### Phase Sequence

Execute phases in order. Do not skip phases. Do not reorder phases.

| Step | File | Phase |
|------|------|-------|
| 1 | `PROMPT_01.md` | Repository Init & Discovery |
| 2 | `PROMPT_02.md` | Structural Analysis & Mapping |
| 3 | `PROMPT_03.md` | Dependency & Relationship Analysis |
| 4 | `PROMPT_04.md` | Deep Code Analysis |
| 5 | `PROMPT_05.md` | Architecture Reconstruction |
| 6 | `PROMPT_06.md` | Workflow & Execution Path Analysis |
| 7 | `PROMPT_07.md` | Design Pattern & Decision Analysis |
| 8 | `PROMPT_08.md` | AI Workflow & Agent Analysis |
| 9 | `PROMPT_09.md` | Documentation Synthesis |
| 10 | `PROMPT_10.md` | Quality Assurance & Validation |

### Phase Completion Gates

Before moving from Phase N to Phase N+1:

1. Confirm all tasks in Phase N are complete.
2. Record all findings in the working knowledge base.
3. Verify no critical gaps remain.
4. Only proceed when confidence > 90%.

### Module Invocation

Domain modules are optional, deep-dive resources. Invoke them when:

- The phase prompt says "use module X if applicable."
- The repository requires deeper analysis in that domain.
- The AI agent determines the module would improve understanding.

### Template Usage

Documentation templates should be used during Phase 9 for consistent output format.

---

## 🧠 WORKING KNOWLEDGE BASE

Throughout the process, maintain a **Working Knowledge Base**. This is your internal model of the repository that grows with each phase.

### Knowledge Base Structure

```json
{
  "repository_metadata": {},
  "structural_map": {},
  "dependency_graph": {},
  "component_models": {},
  "architectural_model": {},
  "workflow_models": {},
  "design_patterns": {},
  "ai_workflows": {},
  "documentation_artifacts": {}
}
```

### Knowledge Base Rules

1. Update the knowledge base after each phase.
2. Cross-reference new findings with existing knowledge.
3. Flag contradictions for investigation.
4. Track confidence levels per finding.
5. Note unresolved questions for later phases.

---

## 🔄 FEEDBACK LOOPS

### Within-Phase Feedback
- After analyzing each file, verify understanding.
- If unclear, re-analyze before proceeding.
- Use tools (search, read, compare) as needed.

### Cross-Phase Feedback
- Later phases may reveal gaps in earlier analysis.
- When gaps are found, **stop and remediate** before proceeding.
- Update earlier findings with new understanding.

### Self-Review Feedback
- At the end of each phase, conduct a brief self-review.
- Document what was learned, what remains unclear, and what to investigate next.

---

## 📊 CONFIDENCE TRACKING

For every finding, track confidence:

| Level | Label | Meaning |
|-------|-------|---------|
| 100% | Confirmed | Verified through multiple evidence sources |
| 80% | Likely | Strong evidence, single source |
| 60% | Probable | Reasonable inference, some evidence |
| 40% | Possible | Speculative, limited evidence |
| 20% | Uncertain | Guess, needs verification |

**Rule:** Never include findings below 60% confidence in final documentation without explicit labeling as "unverified."

---

## 🛑 STOP CONDITIONS

The framework execution must stop and request guidance if:

1. A phase cannot be completed due to insufficient information.
2. A contradiction is found that cannot be resolved.
3. The repository appears to be malicious or intentionally obfuscated.
4. The repository exceeds expected size/complexity beyond framework capacity.
5. A critical external dependency cannot be analyzed.

---

## 📁 OUTPUT DIRECTORY STRUCTURE

All documentation must be written to:

```
repository-name_reverse_engineering/
├── 01_DISCOVERY/
├── 02_STRUCTURAL_ANALYSIS/
├── 03_DEPENDENCY_ANALYSIS/
├── 04_DEEP_ANALYSIS/
├── 05_ARCHITECTURE/
├── 06_WORKFLOWS/
├── 07_DESIGN_PATTERNS/
├── 08_AI_WORKFLOWS/
├── 09_DOCUMENTATION/
├── 10_VALIDATION/
├── DIAGRAMS/
├── REBUILD_GUIDE.md
└── INDEX.md
```

---

## ✅ FINAL CHECK

Before beginning Phase 1, confirm:

- [ ] I have read MISSION.md and understand the mission.
- [ ] I have read OPERATING_RULES.md and understand my constraints.
- [ ] I have read QUALITY_STANDARDS.md and understand quality requirements.
- [ ] I have read OUTPUT_RULES.md and understand output requirements.
- [ ] I have read PROJECT_SPECIFICATION.md.
- [ ] I have created the working knowledge base structure.
- [ ] I am ready to proceed with Phase 1.

---

**PROCEED TO PHASE 1 → `PROMPT_01.md`**

