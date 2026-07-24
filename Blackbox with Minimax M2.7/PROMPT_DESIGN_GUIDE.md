# Prompt Design Guide

> **Document:** PROMPT_DESIGN_GUIDE.md  
> **Version:** 1.0.0  
> **Purpose:** Document the design philosophy, architecture, and engineering decisions behind this prompt framework

---

## 🏛️ DESIGN PHILOSOPHY

### 1.1 First Principles

This framework is built on five first principles:

| Principle | Description | Implication |
|-----------|-------------|-------------|
| **Understanding First** | Complete understanding must precede documentation | 8 analysis phases before 1 documentation phase |
| **No Blind Spots** | Every file, function, and concept must be analyzed | Phase 1 inventory + Phase 4 deep analysis |
| **Depth Over Breadth** | Deep understanding > broad coverage | Modules for deep dives, standards for depth |
| **Structure Mirrors Code** | Documentation structure mirrors codebase structure | Output directory mirrors analysis phases |
| **Quality is Non-Negotiable** | All output must meet enterprise standards | Quality standards, validation phase, scoring |

### 1.2 Design Goals

1. **Reusability:** Framework works for any repository regardless of size, language, or domain.
2. **Completeness:** No aspect of any repository remains undocumented.
3. **Scalability:** Works for 10-file projects and 10,000-file monorepos.
4. **Extensibility:** New phases, modules, and templates can be added without breaking existing structure.
5. **Maintainability:** The framework itself is well-documented and easy to update.

---

## 🧠 PROMPT ARCHITECTURE

### 2.1 The Hub-and-Spoke Model

```
                  ┌─────────────────┐
                  │  MASTER_PROMPT  │
                  │   (Orchestrator)│
                  └────────┬────────┘
          ┌────────────────┼────────────────┐
          │                │                │
          ▼                ▼                ▼
    ┌──────────┐    ┌──────────┐    ┌──────────┐
    │ Mission  │    │  Rules   │    │Standards │
    │   &      │    │  of      │    │    &     │
    │ Spec     │    │ Operation│    │ Output   │
    └──────────┘    └──────────┘    └──────────┘
          │                │                │
          └────────────────┼────────────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │   Phase 1-10    │
                  │  (Sequential)   │
                  └─────────────────┘
                           │
                    ┌──────┴──────┐
                    │             │
                    ▼             ▼
              ┌─────────┐  ┌──────────┐
              │ Modules │  │Templates │
              │(Optional)│  │ (Phase 9)│
              └─────────┘  └──────────┘
```

### 2.2 Why Hub-and-Spoke?

1. **Single Entry Point:** The AI agent always starts from `MASTER_PROMPT.md`.
2. **Modularity:** Each phase is independent but connected.
3. **Controlled Flow:** The orchestrator manages phase transitions.
4. **Extensibility:** Adding a new phase doesn't require changing other phases.

### 2.3 Why Sequential Phases?

1. **Cumulative Understanding:** Each phase builds on the previous.
2. **Manageable Scope:** Breaking the task into phases makes it achievable.
3. **Quality Gates:** Each phase has exit criteria before proceeding.
4. **Traceability:** Issues can be traced to the phase where they were introduced.

---

## 🔄 PHASE DESIGN PATTERNS

### 3.1 Each Phase Follows This Pattern

```markdown
## PHASE HEADER
- Phase number and name
- Entry criteria (what must be true before starting)
- Exit criteria (what must be true after completing)
- Estimated effort

## OBJECTIVES
- Clear, measurable objectives for this phase

## METHODOLOGY
- Step-by-step instructions for completing the phase

## TOOLS
- Which tools to use and how

## KNOWLEDGE BASE UPDATE
- What information to add to the working knowledge base

## DELIVERABLES
- What files or artifacts this phase produces

## QUALITY CHECK
- How to verify the phase was completed correctly

## PHASE COMPLETION GATE
- Final verification before proceeding
```

### 3.2 Why This Pattern?

1. **Predictability:** The AI agent knows what to expect from each phase.
2. **Completeness:** The structure ensures nothing is forgotten.
3. **Measurability:** Clear entry/exit criteria make completion verifiable.
4. **Consistency:** Same structure across all phases reduces cognitive load.

---

## 🧩 MODULE DESIGN PATTERNS

### 4.1 Module Invocation

Modules are invoked in two ways:
1. **Explicit:** The phase prompt says "Use Module X if applicable."
2. **Implicit:** The AI agent determines deeper analysis is needed.

### 4.2 Module Structure

Each module follows this pattern:

```markdown
## MODULE HEADER
- Module name and purpose
- When to use this module

## METHODOLOGY
- Detailed analysis methodology

## ANALYSIS FRAMEWORK
- Specific questions to answer
- Specific patterns to look for
- Specific artifacts to produce

## OUTPUT REQUIREMENTS
- What the module must produce

## CROSS-REFERENCES
- Related phases, templates, and standards
```

### 4.3 Design Rationale

- **Optionality:** Modules are not mandatory, reducing overhead for simple repos.
- **Depth:** Modules provide deep expertise when needed.
- **Reusability:** Modules are designed to be used across different phases.

---

## 📋 TEMPLATE DESIGN PATTERNS

### 5.1 Template Philosophy

Templates provide **structure without rigidity**. They define:
- **What** information to include
- **How** to organize it
- **What format** to use

But they do NOT dictate:
- **What** the content says (that's determined by analysis)
- **How much** detail to include (that's determined by the code)

### 5.2 Template Structure

```markdown
## TEMPLATE HEADER
- Template name and purpose
- When to use this template

## STRUCTURE
- The document structure with placeholders

## USAGE GUIDELINES
- How to fill in the template

## EXAMPLE
- Illustrative example (not from the actual repo)

## QUALITY CHECKLIST
- Verification items for template usage
```

---

## 🔗 CROSS-REFERENCE DESIGN

### 6.1 Why Cross-References?

Cross-references are the **connective tissue** of the documentation. They:
1. Show how components relate to each other.
2. Enable readers to navigate the documentation.
3. Reveal gaps (if a component has no references, it may be isolated).
4. Create a web of understanding that mirrors the codebase.

### 6.2 Cross-Reference Strategy

- **Bidirectional:** If A references B, B should reference A.
- **Meaningful:** References should be contextually relevant.
- **Hierarchical:** Parent → Child references for module relationships.
- **Horizontal:** Peer references for sibling components.
- **External:** References to external dependencies.

---

## 🧪 CONFIDENCE TRACKING DESIGN

### 7.1 Why Confidence Tracking?

1. **Honesty:** Not all findings have equal certainty.
2. **Risk Management:** Low-confidence findings are flagged for human review.
3. **Traceability:** If an error is found, the confidence level indicates how likely it was to be caught.
4. **Improvement:** Low confidence areas highlight where more analysis is needed.

### 7.2 Confidence Scale Design

The scale is deliberately coarser than percentages:
- **100% Confirmed:** Multiple independent evidence sources agree.
- **80% Likely:** Strong evidence from one source, no contradicting evidence.
- **60% Probable:** Reasonable inference, some supporting evidence.
- **40% Possible:** Speculative, limited evidence.
- **20% Uncertain:** Guess, needs verification.

The 60% threshold ensures that only reasonably confident findings make it into final documentation.

---

## 📊 QUALITY SCORING DESIGN

### 8.1 Why a Quality Score?

1. **Objective Measurement:** A score forces objective evaluation.
2. **Gatekeeping:** Prevents low-quality documentation from being delivered.
3. **Improvement Target:** Clear what needs to improve.
4. **Accountability:** The AI agent is responsible for the score.

### 8.2 Weighting Rationale

- **Completeness (25%):** The most important dimension. Missing content is worse than imperfect content.
- **Accuracy (25%):** Equal importance. Incorrect content is dangerous.
- **Clarity (15%):** Important but secondary to completeness and accuracy.
- **Consistency (10%):** Important for usability but can be improved in editing.
- **Depth (15%):** Important for usefulness but varies by component.
- **Usefulness (10%):** The ultimate goal but harder to measure objectively.

---

## 🚀 SCALABILITY DESIGN

### 9.1 Handling Large Repositories

For repositories with > 1,000 files:

1. **Phases remain the same** but execution is more systematic.
2. **Modules become essential** for managing depth.
3. **Batch processing:** Analyze files in groups by module.
4. **Prioritization:** Core modules first, peripheral modules second.
5. **Abstraction layers:** Document at module level first, then drill down.

### 9.2 Handling Small Repositories

For repositories with < 50 files:

1. **Phases can be condensed** but not skipped.
2. **Modules may not be needed** but should be available.
3. **More time per file** for deeper analysis.
4. **More cross-referencing** since files are fewer but may be dense.

### 9.3 Handling Multi-Language Repositories

1. **Phase 1 identifies all languages.**
2. **Phase 4 adapts analysis methodology per language.**
3. **Language-specific templates** can be added.
4. **Inter-language boundaries** are documented explicitly.

---

## ⚙️ EXTENSIBILITY MECHANISMS

### 10.1 Adding Capabilities

| Mechanism | How | Example |
|-----------|-----|---------|
| New Phase | Create PROMPT_NN.md, update MASTER_PROMPT.md | Add "Security Analysis" phase |
| New Module | Create modules/MODULE_NAME.md | Add "Performance Analysis" module |
| New Template | Create templates/TEMPLATE_NAME.md | Add "Database Schema" template |
| New Standard | Create standards/STANDARD_NAME.md | Add "Security Documentation" standard |

### 10.2 Customization Points

- **Phase Prompts:** Can be modified for domain-specific analysis.
- **Module Content:** Can be extended for technology-specific patterns.
- **Templates:** Can be customized for organization-specific documentation.
- **Standards:** Can be adjusted to meet organizational quality requirements.

---

## 🧭 NAVIGATION DESIGN

### 11.1 The MASTER_INDEX

The MASTER_INDEX.md serves multiple purposes:
1. **Table of Contents:** Shows all files in the framework.
2. **Navigation Guide:** Directs users to the right file.
3. **Execution Flow:** Shows the order of operations.
4. **Cross-Reference Map:** Links related concepts.
5. **Maturity Model:** Shows the levels of analysis depth.

### 11.2 Why This Navigation Design?

1. **Multiple Entry Points:** Users can find what they need from different starting points.
2. **Visual Structure:** The file tree gives an immediate sense of the framework.
3. **Relationships:** The cross-reference map shows how concepts connect.
4. **Progressive Disclosure:** Details are available but not overwhelming.

---

## 🎯 FINAL DESIGN NOTE

This framework is designed with **engineering rigor** at every level:

- **Macro:** The overall architecture (hub-and-spoke, sequential phases)
- **Meso:** The individual prompts (consistent structure, clear objectives)
- **Micro:** The documentation output (consistent formatting, cross-references)

Every design decision is documented and justified. Every component has a purpose. Nothing is arbitrary.

*This is not a prompt—it's a prompt engineering system.*

