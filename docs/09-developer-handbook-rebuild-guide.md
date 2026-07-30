# 09 - Developer Handbook / Rebuild Guide

This document provides actionable instructions for rebuilding the Enterprise Reverse Engineering Prompt Framework from scratch, porting it to a new LLM, or extending it with new phases.

---

## Rebuild Order

To create a new framework variant from scratch, execute the following steps in order:

### Step 1: Scaffold the Infrastructure Layer

Create the foundational constraint files that govern all subsequent prompt execution:

| File | Purpose |
|------|---------|
| `MISSION.md` | Defines the agent's overarching goal (complete repository reverse-engineering) |
| `OPERATING_RULES.md` | Establishes pacing rules, continuation protocol, and anti-skip constraints |
| `QUALITY_STANDARDS.md` | Enforces evidence-based documentation, `[UNVERIFIED]` tagging, and anti-hallucination rules |
| `OUTPUT_RULES.md` | Specifies formatting requirements (Mermaid for diagrams, tables for enumerable data, numbered prefixes) |

### Step 2: Build the MASTER_PROMPT Orchestrator

Create `MASTER_PROMPT.md` as the single entry point. This file must:

1. Reference and load all infrastructure files from Step 1
2. Define the phase execution sequence (Phases 1-10)
3. Specify conditional logic for optional phases (Phase 6 if AI code present, Phase 8 if APIs/DBs present)
4. Include the continuation mechanism trigger string

### Step 3: Create Phase Prompts Following the 7-Section Template

Each phase prompt should contain these sections:

1. **Objective** - What this phase produces
2. **Inputs** - What prior phase outputs are required
3. **Execution Instructions** - Step-by-step procedure for the LLM
4. **Output Format** - Exact structure of the deliverable
5. **Quality Gates** - Validation criteria before proceeding
6. **Conditional Logic** - When to skip or adapt
7. **Continuation Trigger** - How to handle output truncation

Build prompts in order:

| Phase | Prompt File | Deliverable |
|-------|-------------|-------------|
| 1 | `PROMPT_01_REPOSITORY_INTELLIGENCE.md` | Repository overview, variant map, tech stack identification |
| 2 | `PROMPT_02_FILE_FOLDER_ANALYSIS.md` | Per-file purpose table for every file in the repository |
| 3 | `PROMPT_03_FUNCTION_CLASS_DOCS.md` | Unit-level documentation of code/prompt functions |
| 4 | `PROMPT_04_ARCHITECTURE_RECONSTRUCTION.md` | System design, component map, working logic |
| 5 | `PROMPT_05_DIAGRAMS.md` | Mermaid sequence/component/flowchart diagrams |
| 6 | `PROMPT_06_AI_AGENT_WORKFLOW.md` | AI orchestration analysis (conditional) |
| 7 | `PROMPT_07_TECH_STACK.md` | Technology inventory with versions and roles |
| 8 | `PROMPT_08_CONDITIONAL_DOCS.md` | API/DB/Auth/Deploy/Env docs (conditional) |
| 9 | `PROMPT_09_DEVELOPER_HANDBOOK_REBUILD.md` | Rebuild guide and feature checklist |
| 10 | `PROMPT_10_VALIDATION_QA.md` | Self-audit and open questions |

### Step 4: Add Supporting Files

Create supplementary reference materials:

| File | Purpose |
|------|---------|
| `GLOSSARY.md` | Defines framework-specific terminology (variant, phase, infrastructure layer, etc.) |
| `DIAGRAM_TEMPLATES.md` | Provides reusable Mermaid diagram skeletons for common patterns |
| `VALIDATION_CHECKLISTS.md` | Pre-built quality gate checklists for each phase |

### Step 5: Validate Against Quality Gates

Run the following checks before finalizing:

- [ ] Every phase prompt references the infrastructure files
- [ ] Continuation mechanism is present in all prompts
- [ ] `[UNVERIFIED]` tagging rule is enforced in QUALITY_STANDARDS
- [ ] Output format specifications are unambiguous
- [ ] Conditional skip logic is explicitly defined for Phases 6 and 8

### Step 6: Organize into Variant Directory

Place all files into a named directory following the convention:

```
VariantName/
  MISSION.md
  OPERATING_RULES.md
  QUALITY_STANDARDS.md
  OUTPUT_RULES.md
  MASTER_PROMPT.md
  PROMPT_01_REPOSITORY_INTELLIGENCE.md
  PROMPT_02_FILE_FOLDER_ANALYSIS.md
  ...
  PROMPT_10_VALIDATION_QA.md
  GLOSSARY.md
  DIAGRAM_TEMPLATES.md
  VALIDATION_CHECKLISTS.md
```

---

## Feature Checklist

Every framework "feature" expressed as a rebuildable specification:

| Feature | Implementation Location | Description |
|---------|------------------------|-------------|
| Continuation Mechanism | `OPERATING_RULES.md` | If LLM output truncates mid-response, emit a specific trigger string and resume from the exact truncation point without re-greeting or summarizing |
| Evidence Tagging (`[UNVERIFIED]`) | `QUALITY_STANDARDS.md` | Any claim not directly verifiable from repository contents must be tagged `[UNVERIFIED - needs confirmation]` |
| Conditional Skipping | `MASTER_PROMPT.md`, Phase 6/8 prompts | Phases 6 and 8 include explicit "skip if not applicable" logic with required justification |
| Quality Gates | Each phase prompt, Section 5 | Per-phase validation criteria that must pass before proceeding to the next phase |
| Context Compression / Handoff | `OPERATING_RULES.md` | Between phases, compress prior output into a structured summary to preserve context window budget |
| Multi-Resolution Analysis | Phase 1 (high-level) through Phase 3 (unit-level) | Progressive zoom from repository overview to individual file/function documentation |
| Progressive Deepening Funnel | Phase sequence 1-10 | Each phase builds on the prior phase's output, narrowing from broad discovery to specific validation |

---

## Non-obvious Gotchas

### Context Window Overflow Mitigation

Large repositories (500+ files) will exceed most LLMs' context windows if all file contents are loaded simultaneously. The framework mitigates this by enforcing single-phase, depth-first execution. Never combine multiple phases into one prompt for repositories exceeding 100 files. The continuation mechanism handles within-phase overflow, but cross-phase overflow requires explicit context compression at handoff boundaries.

### LLM Laziness / Summarization Tendency

LLMs tend to summarize or skip repetitive content (e.g., "remaining files follow similar patterns"). The framework combats this with explicit anti-skip rules in `OPERATING_RULES.md` requiring one-line purpose documentation for every file without exception. When porting to a new LLM, test this constraint specifically - some models require stronger phrasing ("You MUST document EVERY file") to comply.

### Prompt Ordering Sensitivity

The sequence in which infrastructure files are referenced in `MASTER_PROMPT.md` affects LLM behavior. Place `QUALITY_STANDARDS.md` before `OUTPUT_RULES.md` to establish the "what" constraints before the "how" formatting. Placing output rules first can cause the LLM to prioritize formatting over accuracy.

### Cross-Variant Redundancy Management

When maintaining multiple variants, changes to shared infrastructure concepts (e.g., updating the continuation mechanism) must be manually propagated to all variant directories. There is currently no shared-core import mechanism. Track which variants have received which updates using a changelog or diff tool.

---

## Known Debt

| Issue | Impact | Suggested Resolution |
|-------|--------|---------------------|
| Duplicated infrastructure files across 10 variants with no shared-core directory | Updates to core rules require manual propagation to all variants; drift risk is high | Create a `/shared-core/` directory and have each variant's `MASTER_PROMPT.md` reference it via relative path |
| No automated diff verification between variants | Cannot detect unintentional divergence between variant copies of the same file | Implement a CI script that diffs common files across all variant directories and flags differences |
| Gemini variant has empty placeholder files | Incomplete variant may confuse users or produce partial output | Either complete the Gemini variant or add a `STATUS: INCOMPLETE` marker file |
| No version tracking between variant updates | Impossible to determine which variant has the latest infrastructure rules | Add a `VERSION.md` or changelog per variant tracking update dates |
| `.docx` file in GLM5.1 breaks Markdown-only principle | Cannot be rendered in standard Markdown viewers; not diffable in git | Convert to `.md` or extract text content into a Markdown equivalent |
