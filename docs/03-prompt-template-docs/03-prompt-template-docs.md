# 03 - Prompt Template Documentation

> Prompt templates in this framework serve the role that functions and classes serve in traditional codebases. Each prompt is a self-contained unit of analytical work with defined inputs, execution logic, outputs, and quality gates.

---

## Canonical Prompt Template Structure

The Hermes With Deepseek v4 flash variant defines the reference template structure. Every execution prompt (P01 through P36) follows a seven-section format:

| Section | Role | Analogy (Traditional Code) |
|---------|------|---------------------------|
| 1. MISSION | Declares the prompt's single responsibility | Function docstring / purpose comment |
| 2. PREREQUISITES | Lists required inputs and prior outputs | Function parameters / preconditions |
| 3. SYSTEM PROMPT | Provides the AI persona and step-by-step instructions | Function body / implementation |
| 4. EXECUTION INSTRUCTIONS | Operational constraints for how to perform the work | Runtime configuration / compiler flags |
| 5. OUTPUT SPECIFICATION | Defines the exact structure of the deliverable | Return type / output schema |
| 6. QUALITY GATE | Pass/fail checklist before handoff | Unit test assertions / postconditions |
| 7. HANDOFF | Context items transferred to downstream prompts | Return value passed to caller |

---

## Section-by-Section Analysis

### Section 1: MISSION

Every prompt opens with a one-paragraph mission statement that establishes a single, non-overlapping analytical responsibility.

**Pattern observed across all 36 prompts:**

```
You are the [Role Name]. Your mission is to [single verb phrase describing the analytical objective].
```

**Examples from the Hermes variant:**

| Prompt | Role Name | Mission Verb Phrase |
|--------|-----------|-------------------|
| P01 | Discovery Scanner | perform the initial scan of the target repository |
| P07 | System Architect Analyst | reconstruct the system's architecture from structural evidence |
| P16 | Prompt Architecture Analyst | reverse engineer every AI prompt in the system |
| P31 | Accuracy Validation Officer | perform cross-phase accuracy validation of the entire output |

The mission section enforces the Single Responsibility Principle: each prompt does exactly one thing. No prompt attempts discovery and architecture simultaneously.

---

### Section 2: PREREQUISITES

Prerequisites function as a dependency contract. They use a checklist format:

```markdown
- [ ] PROMPT_NN completed - [specific output available]
- [ ] [Resource] is accessible
```

**Dependency types observed:**

| Type | Example | Source |
|------|---------|--------|
| Output dependency | "PROMPT_04 completed - folder architecture documented" | P07 prerequisites |
| Resource dependency | "Target repository path is known" | P01 prerequisites |
| Conditional dependency | "AI prompt patterns detected in Phase 3" | P16 prerequisites |
| Aggregate dependency | "ALL Phase 1-7 output files accessible" | P31 prerequisites |

The prerequisites section directly mirrors the edges in `PROMPT_DEPENDENCY_MAP.md`. If prompt B lists prompt A in its prerequisites, the dependency map shows an edge from A to B.

---

### Section 3: SYSTEM PROMPT

The system prompt section is the largest and most complex. It defines:

1. **AI persona assignment** - a specialization statement
2. **Step-by-step instructions** - numbered analytical steps (typically 3 to 6)
3. **Classification tables** - enumerable categories for the agent to select from
4. **Output templates** - structured formats for intermediate findings

**Structural pattern (consistent across all prompts):**

```markdown
## 3. SYSTEM PROMPT

You are an AI specializing in [domain expertise]. You [capability statement].

### 3.1 Instructions

**Step 1: [Action]**
[Detailed instructions with sub-bullets]

**Step 2: [Action]**
[Detailed instructions with tables or templates]

...
```

**Example from P07 (System Architecture):**

Step 1 provides a classification table with 14 architecture styles and their key indicators. The agent must select from this enumerated set, preventing open-ended hallucination. Each style requires three evidence fields: Evidence, Purity, and Deviations.

Step 2 defines a component documentation template with 10 required fields (Responsibility, Type, Files, Interfaces Provided, Interfaces Consumed, Dependencies internal, Dependencies external, Key Data Owned, Architectural Significance).

**Example from P01 (Repository Scan):**

Steps 1 through 5 define a progressive scan strategy: Root Analysis, Directory Structure, Initial Technology Detection, Large File Detection, Pattern Detection. Each step includes an explicit enumeration of what to look for, preventing the agent from inventing its own categories.

---

### Section 4: EXECUTION INSTRUCTIONS

This section constrains how the agent performs work rather than what work to perform. Common patterns:

| Constraint Type | Example | Purpose |
|----------------|---------|---------|
| Depth control | "This is a scan, not analysis. A function's purpose matters; its implementation details do not (yet)." | Prevents premature deep-dives |
| Synthesis directive | "Synthesize, don't repeat. Do not re-list files or re-catalog dependencies." | Prevents redundant output |
| Honesty requirement | "Be honest about uncertainty. If the architecture style is unclear, say so." | Anti-hallucination |
| Evidence binding | "Component boundaries must be evidence-based." | Traceability enforcement |
| Scope awareness | "Do NOT go deep into code logic yet." | Phase boundary enforcement |

Execution instructions typically contain 3 to 5 numbered directives. They are imperative ("DO X", "Do NOT Y") and unambiguous.

---

### Section 5: OUTPUT SPECIFICATION

Defines the exact deliverable. Every output spec includes:

1. **Target filename** - e.g., `07_system_architecture.md`
2. **Section headings** - the exact H2/H3 structure the output must follow
3. **Table schemas** - column headers and expected content per row
4. **Diagram templates** - Mermaid code blocks showing expected diagram structure

**Example output structure from P07:**

```
5.1 Architecture Overview       [2-3 paragraph summary]
5.2 Architecture Style          [Style name + evidence table + purity + deviations]
5.3 System Context Diagram      [Mermaid graph TD]
5.4 Component Catalog           [Table: Component | Responsibility | Type | Files | Depends On]
5.5 Component Relationship Diagram [Mermaid graph TD]
5.6 Architectural Decisions     [Table: Decision | Evidence | Tradeoffs Visible]
5.7 Architecture Quality Notes  [Prose: strengths, issues, debt indicators]
```

The output specification removes ambiguity about deliverable format. The agent cannot invent its own structure.

---

### Section 6: QUALITY GATE

A binary checklist that must pass before the prompt's output is considered complete:

```markdown
## 6. QUALITY GATE

- [ ] [Specific verifiable criterion]
- [ ] [Specific verifiable criterion]
...
```

**Typical gate sizes:**

| Phase Category | Prompts | Average Gate Items |
|---------------|---------|-------------------|
| Discovery (P01-P03) | 3 | 8 items |
| Structural (P04-P06) | 3 | 6 items |
| Architecture (P07-P10) | 4 | 7 items |
| Deep Code (P11-P15) | 5 | 6 items |
| AI Analysis (P16-P20) | 5 | 5 items |
| Integration (P21-P24) | 4 | 6 items |
| Documentation (P25-P30) | 6 | 7 items |
| Validation (P31-P34) | 4 | 8 items |
| Rebuild (P35-P36) | 2 | 5 items |

Quality gates are binary: all items must pass. There is no partial credit. If any item fails, the agent must remediate before proceeding.

---

### Section 7: HANDOFF

Defines exactly what context items transfer to downstream prompts:

```markdown
## 7. HANDOFF

Pass to PROMPT_NN, PROMPT_NN:
- [Context item 1]
- [Context item 2]
```

**Example from P07:**

> Pass to PROMPT_08, PROMPT_09, and PROMPT_10:
> - Architecture style and purity assessment
> - Component catalog (for decomposition in PROMPT_08)
> - System context (for integration analysis in Phase 6)
> - Architectural decisions (for design pattern analysis in PROMPT_10)

The handoff section prevents context window overflow by specifying only the minimum context needed by downstream prompts, rather than forwarding all raw analysis.

---

## Prompt Categories by Phase

### Phase 1: Discovery (P01-P03)

| Prompt | Name | Mission | Key Output |
|--------|------|---------|-----------|
| P01 | Repository Scan | First-contact reconnaissance | Scan report with stats, tree, and patterns |
| P02 | File Inventory | Complete file categorization | Categorized inventory with exclusion list |
| P03 | Technology Stack Detection | Identify all technologies | Language, framework, library catalog with versions |

**Characteristic pattern:** These prompts have minimal prerequisites (P01 has none) and produce foundational data consumed by all later phases.

---

### Phase 2: Structural Analysis (P04-P06)

| Prompt | Name | Mission | Key Output |
|--------|------|---------|-----------|
| P04 | Folder Architecture | Map directory structure and conventions | Directory structure map, naming conventions |
| P05 | Module Dependency Graph | Trace all import relationships | Dependency graph (internal + external), circular warnings |
| P06 | Entry Point Analysis | Identify all system entry points | Entry point catalog with signatures |

**Characteristic pattern:** P05 and P06 can run in parallel (both depend only on P04). This is the first parallelization opportunity in the pipeline.

---

### Phase 3: Architecture Reconstruction (P07-P10)

| Prompt | Name | Mission | Key Output |
|--------|------|---------|-----------|
| P07 | System Architecture | Reconstruct overall architecture | Architecture style, component map, decisions |
| P08 | Component Decomposition | Decompose components internally | Component internals, interfaces, structure |
| P09 | Layer Analysis | Identify architectural layers | Layer definitions, responsibilities, violations |
| P10 | Design Pattern Recognition | Catalog design patterns | Pattern catalog with code locations |

**Characteristic pattern:** P07 synthesizes all Phase 2 outputs. P08, P09, P10 can run in parallel after P07 completes.

---

### Phase 4: Deep Code Analysis (P11-P15)

| Prompt | Name | Mission | Key Output |
|--------|------|---------|-----------|
| P11 | Data Flow Analysis | Trace all data paths | Data flow maps, transformation pipelines |
| P12 | Execution Path Reconstruction | Map all execution paths | Execution path diagrams, branch conditions |
| P13 | State Management | Document all state machines | State machines, stores, transitions |
| P14 | Error Handling and Retry | Catalog error handling | Error catalog, retry strategy, fallbacks |
| P15 | Concurrency and Performance | Document concurrency model | Concurrency patterns, synchronization, performance |

**Characteristic pattern:** P11, P12, P13 can partially parallelize. P14 requires all three. P15 gates the transition to Phase 5.

---

### Phase 5: AI and Automation Analysis (P16-P20) [CONDITIONAL]

| Prompt | Name | Mission | Key Output |
|--------|------|---------|-----------|
| P16 | Prompt Architecture | Reverse engineer all AI prompts | Prompt inventory, dependency graph, quality assessment |
| P17 | Agent Workflow | Reconstruct agent behaviors | Agent inventory, communication, orchestration |
| P18 | Tool Integration | Catalog all agent tools | Tool catalog, interfaces, security assessment |
| P19 | Planning/Reasoning Pipeline | Map planning logic | Planning pipeline, decision framework |
| P20 | Memory/RAG Workflow | Analyze memory systems | Memory architecture, RAG pipeline, vector stores |

**Characteristic pattern:** This entire phase is conditional. The `MASTER_PROMPT.md` checks Phase 3 output for AI patterns. If none found, execution skips directly to Phase 6. P16 and P17 can run in parallel.

---

### Phase 6: Integration and Boundary Analysis (P21-P24)

| Prompt | Name | Mission | Key Output |
|--------|------|---------|-----------|
| P21 | Internal API Contracts | Document internal APIs | API contracts, service interfaces |
| P22 | External Service Integration | Catalog external services | External service catalog, endpoints, auth |
| P23 | Event Stream Workflow | Map event systems | Event types, producers, consumers, guarantees |
| P24 | Configuration and Environment | Map all configuration | Configuration map, env var catalog |

**Characteristic pattern:** P21 and P22 can run in parallel. P24 aggregates all integration knowledge.

---

### Phase 7: Documentation Generation (P25-P30)

| Prompt | Name | Mission | Key Output |
|--------|------|---------|-----------|
| P25 | Architecture Handbook | Generate architecture documentation | Complete architecture handbook |
| P26 | Developer Handbook | Generate developer guide | Development setup, common tasks |
| P27 | Rebuild Guide | Generate rebuild instructions | Step-by-step rebuild guide |
| P28 | API Reference/Class Catalog | Generate complete reference | API reference documentation |
| P29 | Engineering Notes/Cross-References | Generate cross-reference index | Feature map, cross-reference catalog |
| P30 | Validation Handover Protocol | Prepare for validation phase | Validation-ready artifact package |

**Characteristic pattern:** P26, P27, P28 can run in parallel after P25. This phase transforms raw analysis into polished documentation.

---

### Phase 8: Validation and Quality (P31-P34)

| Prompt | Name | Mission | Key Output |
|--------|------|---------|-----------|
| P31 | Cross-Phase Accuracy Validation | Verify accuracy across phases | Discrepancy report, correction map |
| P32 | Completeness Deep Audit | Verify nothing is missing | Coverage report, gap analysis |
| P33 | Consistency/Contradiction Verification | Check internal consistency | Contradiction catalog, resolution plan |
| P34 | Final Quality Gate Signoff | Overall quality assessment | Quality scores, signoff status |

**Characteristic pattern:** P31, P32, P33 can run in parallel. P34 aggregates all validation results for final signoff.

---

### Phase 9: Rebuild Package (P35-P36) [OPTIONAL]

| Prompt | Name | Mission | Key Output |
|--------|------|---------|-----------|
| P35 | Rebuild Package Assembly | Assemble rebuild artifacts | Build instructions, dependency lists |
| P36 | Rebuild Verification Protocol | Verify rebuild is possible | Verification test results |

**Characteristic pattern:** This phase is optional. It executes only when the rebuild guide (P27) was completed and explicitly requested.

---

## Variant Adaptations

The canonical seven-section structure is adapted across the 10 repository variants:

| Variant | Model Target | Key Adaptations |
|---------|-------------|----------------|
| Hermes With Deepseek v4 flash | DeepSeek V4 | Full canonical structure (36 prompts, 13 infrastructure files) |
| Claude with compact prompts | Claude 3.x | Condensed prompt bodies; fewer enumerated examples |
| Opencode V1/V2 | General | Simplified dependency tracking; fewer phases |
| GLM5.1 | GLM-5.1 | Adapted instruction style for Chinese-English bilingual |
| Qwen | Qwen models | Token-optimized prompts with shorter system sections |
| Mistral | Mistral models | Reduced classification tables; streamlined gates |
| Blackbox+Kimi / Blackbox+Minimax | Kimi/Minimax | Hybrid structure; shorter pipeline (fewer prompts per phase) |
| Gemini 3.1 Pro high | Gemini | 10-prompt simplified pipeline mapping to 9 logical phases |

**Common adaptation patterns:**

1. **Prompt count reduction** - Variants targeting smaller context windows merge multiple prompts into single files
2. **Table simplification** - Classification tables shrink from 14 rows (Hermes) to 5-8 rows in compact variants
3. **Quality gate compression** - Gate items reduce from 7-8 per prompt to 3-5 in compact variants
4. **Handoff elimination** - Some variants rely on implicit context carry-over rather than explicit handoff sections

---

## Cross-References

- [Repository Intelligence](../01-repository-intelligence.md) - file counts and variant inventory
- [File/Folder Analysis](../02-file-folder-analysis/02-file-folder-analysis.md) - directory structure of prompt phases
- [System Design](../04-architecture/system-design.md) - three-layer architecture placing prompts in execution layer
- [Module Map](../04-architecture/module-map.md) - dependency DAG between prompts
