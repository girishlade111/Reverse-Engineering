# PROMPT DESIGN GUIDE — Enterprise Reverse Engineering Framework

## DOCUMENT CLASS: Meta-Design Document
## PURPOSE: Explain the design philosophy, architecture decisions, and engineering rationale behind this prompt framework

---

## 1. DESIGN PHILOSOPHY

### 1.1 The Understanding-First Principle

The foundational design decision of this framework is **Understanding Before Documenting**.

Most reverse engineering efforts fail because the agent tries to document while still exploring. This leads to:
- Incomplete documentation
- Incorrect architectural claims
- Missed dependencies
- Shallow analysis

This framework enforces a strict separation between analysis phases (1-18) and documentation phases (19-20). The agent must build complete understanding before writing final documentation.

### 1.2 The Progressive Depth Principle

The framework starts at 50,000 feet and progressively descends:

```
Phase 01-02: Aerial survey (folder structure, file map)
Phase 03-05: Infrastructure survey (build, deps, tech stack)
Phase 06-07: Street-level walkthrough (modules, files)
Phase 08-12: Building inspection (architecture, flows, algorithms)
Phase 13-18: Systems inspection (APIs, state, errors, AI, config)
Phase 19-20: Final blueprints (documentation, validation)
```

Each level builds on the previous. You cannot understand the architecture until you understand the modules. You cannot understand the modules until you understand the file structure.

### 1.3 The Evidence Chain Principle

Every conclusion must be traceable back to source code evidence. This creates an **evidence chain**:

```
Claim → File:Line Reference → Quoted Source Code → Raw File
```

If any link in this chain is missing, the claim is unsupported.

## 2. ARCHITECTURAL DECISIONS

### ADR-1: Modular File Structure

**Decision**: Split the framework into 32+ files instead of one monolithic prompt.

**Rationale**:
- Each phase can be loaded independently for targeted analysis
- Reduced context window usage per phase
- Easier to update individual phases
- Parallel analysis possible for very large repos
- Clear separation of concerns

**Trade-off**: Slightly more complex navigation. Mitigated by MASTER_INDEX.md.

### ADR-2: Phase-Gated Progression

**Decision**: Each phase has validation gates that must be passed before proceeding.

**Rationale**:
- Prevents compounding errors across phases
- Ensures each phase is complete before the next begins
- Creates natural checkpoint for quality assurance
- Enables targeted re-work of individual phases

### ADR-3: Output Directory Structure

**Decision**: All outputs go to `re-docs/` with phase-numbered subdirectories.

**Rationale**:
- Outputs are organized and findable
- No mixing of analysis output with source code
- Easy to archive or share the full analysis
- Supports incremental analysis across multiple sessions

### ADR-4: Accuracy Tiers

**Decision**: Claims are labeled A (Verified), B (Inferred), C (Uncertain), D (Unknown).

**Rationale**:
- Explicit communication of confidence levels
- Prevents false certainty
- Enables readers to gauge reliability
- Supports quality auditing

### ADR-5: Language-Agnostic Design

**Decision**: All prompts avoid language-specific terminology.

**Rationale**:
- Single framework for all repositories
- No need for multiple language-specific versions
- Agent adapts to the detected language automatically

### ADR-6: Diagram Requirements

**Decision**: Complex subsystems require diagrams.

**Rationale**:
- Diagrams communicate relationships more effectively than prose
- Mermaid.js is universally supported by AI agents
- Forces deeper understanding (you can't diagram what you don't understand)

## 3. ENGINEERING GUIDELINES FOR PROMPT CONSTRUCTION

### 3.1 Prompt Structure

Each phase prompt follows this exact structure:

```
# PROMPT_N — Phase Name

## OBJECTIVE
## PREREQUISITES
## INPUTS
## ANALYSIS STEPS
## OUTPUT SPECIFICATION
## REQUIRED DIAGRAMS
## VALIDATION CHECKS
## COMPLETION CHECKLIST
```

### 3.2 Instruction Clarity

All instructions use:
- Imperative mood ("Read the file", "Document the function")
- Explicit scope ("All files in src/api/")
- Measurable outcomes ("List every endpoint with method, path, and handler")
- No ambiguous qualifiers ("Analyze thoroughly" is forbidden; "For each file, document: [list]" is required)

### 3.3 Validation Design

Each validation check must be:
- **Automatically checkable** by the AI agent
- **Binary** (pass/fail, not subjective)
- **Specific** ("Did you document all 12 endpoints?" not "Did you document the API?")

### 3.4 Checklist Design

All checklists use:
- Checkboxes (`- [ ]`)
- Specific items, not categories
- Items that can be verified independently

## 4. COGNITIVE LOAD MANAGEMENT

The framework manages cognitive load through:

1. **Single focus per phase**: The agent works on one aspect at a time
2. **Progressive complexity**: Simple analysis before complex analysis
3. **Structured output**: Templates reduce formatting decisions
4. **Validation gates**: Natural breakpoints for review
5. **Forward references**: Don't analyze what you haven't reached yet

## 5. ERROR TOLERANCE

The framework handles errors through:

1. **Gap flags**: Unknown information is flagged, not fabricated
2. **Recovery procedures**: Each phase has error recovery steps
3. **Continuation**: Gaps don't block the entire process
4. **Final validation**: Gaps are collected and reported for human intervention

## 6. SCALABILITY DESIGN

For very large repositories (5000+ files):

1. Phase 01 identifies module boundaries
2. Phases 06-07 are executed per-module, in parallel
3. Phase 08 synthesizes module-level analyses into system architecture
4. Each module's outputs are stored independently
5. The framework dynamically allocates more analysis budget to critical modules

---

*This design guide explains why the framework works the way it does. Understanding the design helps the executing agent make better decisions within the framework.*
