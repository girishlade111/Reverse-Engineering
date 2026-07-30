# 00-INDEX

## Enterprise Reverse Engineering Prompt Framework - Documentation Index

Complete generated documentation for the [Reverse-Engineering](https://github.com/girishlade111/Reverse-Engineering) repository. This index links to all produced documentation phases with one-line descriptions.

---

## Documentation Files

### Phase 1: Repository Intelligence

- [01-repository-intelligence.md](./01-repository-intelligence.md) - Top-level repository scan identifying the 10-variant monorepo structure, tech stack (Markdown/LLM prompts), and framework purpose.

### Phase 2: File/Folder Analysis

- [02-file-folder-analysis.md](./02-file-folder-analysis/02-file-folder-analysis.md) - Structural enumeration of all 303 files across variant directories with per-file purpose descriptions.

### Phase 3: Prompt Template Documentation

- [03-prompt-template-docs.md](./03-prompt-template-docs/03-prompt-template-docs.md) - Unit-level documentation of individual prompts treated as functional code units (inputs, outputs, constraints, side effects).

### Phase 4: Architecture Reconstruction

- [system-design.md](./04-architecture/system-design.md) - Three-layer architecture (Infrastructure, Orchestration, Execution) and inter-layer relationships.
- [component-map.md](./04-architecture/component-map.md) - Component inventory mapping infrastructure files, orchestrator, and phase prompts with their responsibilities.
- [module-map.md](./04-architecture/module-map.md) - Dependency graph showing prompt-to-prompt and prompt-to-infrastructure relationships.
- [working-logic.md](./04-architecture/working-logic.md) - End-to-end execution flow from human operator input through LLM processing to documentation output.
- [business-logic.md](./04-architecture/business-logic.md) - Constraint rules governing LLM behavior: pacing, anti-skip, evidence requirements, continuation protocol.

### Phase 5: Diagrams

- [05-diagrams.md](./05-diagrams.md) - Mermaid sequence diagrams, component diagrams, and flowcharts visualizing framework execution and architecture.

### Phase 6: AI Agent Workflow

- [06-ai-agent-workflow.md](./06-ai-agent-workflow.md) - Analysis of the LLM execution environment, prompt-driven orchestration model, and human-in-the-loop interaction pattern.

### Phase 7: Tech Stack

- [07-tech-stack.md](./07-tech-stack.md) - Technology inventory: Markdown as the sole implementation language, target LLM platforms, and filesystem access requirements.

### Phase 8: Conditional Documentation

- [08-conditional-docs.md](./08-conditional-docs/08-conditional-docs.md) - N/A declarations for API, Database, Authentication, Deployment, and Environment documentation with explicit justifications.

### Phase 9: Developer Handbook / Rebuild Guide

- [09-developer-handbook-rebuild-guide.md](./09-developer-handbook-rebuild-guide.md) - Step-by-step instructions for rebuilding the framework from scratch, feature checklist, non-obvious gotchas, and known technical debt.

### Phase 10: Validation QA

- [10-validation-qa.md](./10-validation-qa.md) - Self-audit checklist verifying documentation completeness, accuracy, and cross-reference integrity.

---

## Open Questions Log

| # | Question | Status | Source Phase |
|---|----------|--------|--------------|
| 1 | Are infrastructure files (`MISSION.md`, `OPERATING_RULES.md`, etc.) byte-identical across all 10 variants, or do they contain LLM-specific adaptations? A full diff analysis across 200+ files is needed to confirm. | `[UNVERIFIED]` | Phase 2 |
| 2 | The Gemini variant contains empty/placeholder files. Is this intentionally incomplete or a work-in-progress? | `[UNVERIFIED]` | Phase 2 |
| 3 | The `.docx` file in `GLM5.1/` - is this an intentional format choice or an oversight that breaks the Markdown-only principle? | `[UNVERIFIED]` | Phase 2 |
| 4 | Does the continuation mechanism behave consistently across all target LLMs, or do some models require variant-specific trigger strings? | `[UNVERIFIED]` | Phase 9 |
| 5 | What is the intended update cadence for variant synchronization? Is there a primary/canonical variant that others should track? | `[UNVERIFIED]` | Phase 9 |
