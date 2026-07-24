========================================================================
PROMPT DESIGN GUIDE
========================================================================
Enterprise Reverse Engineering Prompt Framework
Version: 3.0

========================================================================
DESIGN PHILOSOPHY
========================================================================

This framework is built on the following design principles:

1. FIRST PRINCIPLES DECOMPOSITION
   Every system can be understood by decomposing it into its
   fundamental components and understanding their interactions.
   The framework guides the AI to perform this decomposition
   systematically.

2. EVIDENCE-BASED ANALYSIS
   Every claim about the repository must be supported by evidence
   from the code. Speculation is explicitly labeled as such.
   The framework requires the AI to cite specific files, lines,
   and patterns as evidence.

3. PROGRESSIVE DEEPENING
   Analysis proceeds from surface-level to deep understanding.
   Each phase builds on the previous, creating a layered
   comprehension of the system.

4. MODULAR ORCHESTRATION
   Each phase is an independent prompt module with clear inputs,
   instructions, and outputs. Phases are orchestrated by the
   master prompt but can be executed independently if needed.

5. UNIVERSAL APPLICABILITY
   The framework is language-agnostic and architecture-agnostic.
   It works for any software system regardless of tech stack,
   size, or domain.

6. QUALITY-EMBEDDED
   Quality standards are embedded within each phase, not applied
   as an afterthought. Every phase includes specific quality gates.

7. DOCUMENTATION-DRIVEN
   Understanding is not complete until it is documented.
   The framework treats documentation as the primary output of
   understanding, not an optional extra.

========================================================================
PROMPT ENGINEERING METHODOLOGY
========================================================================

This framework uses a multi-layered prompting strategy:

LAYER 1: SYSTEM PROMPT (MASTER_PROMPT.md)
- Defines the AI's role, mindset, and capabilities
- Provides orchestration instructions for all phases
- Sets quality standards and context management rules
- Longest, most comprehensive layer

LAYER 2: CONTEXT PROMPTS (MISSION.md, OPERATING_RULES.md, etc.)
- Provide background context and behavioral constraints
- Define the "personality" and approach of the analysis
- Ensure consistency across all phases

LAYER 3: PHASE PROMPTS (PROMPT_01 through PROMPT_10)
- Each phase has specific, actionable instructions
- Phases produce concrete artifacts
- Each phase references previous phases' outputs
- Designed to be executed sequentially

LAYER 4: HAND-BOOK PROMPTS (Embedded in PROMPT_09)
- Templates for each handbook document
- Specific formatting and content requirements
- Cross-reference requirements

This layered approach allows:
- Clear separation of concerns
- Reusability across different repositories
- Easy adaptation and extension
- Consistent output quality

========================================================================
PROMPT STRUCTURE PATTERN
========================================================================

Each phase prompt follows this structure:

HEADER:       Phase number, title, purpose
OBJECTIVES:   Measurable goals for the phase
INPUTS:       Required artifacts from previous phases
ACTIVITIES:   Step-by-step analysis instructions
ANALYSIS:     Deep analysis methodology
ARTIFACTS:    Documents to produce during this phase
QUALITY GATES: Verification steps before completing the phase
OUTPUTS:      Concrete deliverables to pass to the next phase
EXAMPLES:     Illustrative patterns (NOT prescriptive)

========================================================================
DESIGN DECISIONS
========================================================================

DECISION 1: Phase-based vs. monolithic approach
CHOSEN: Phase-based
RATIONALE: Allows the AI to manage context effectively, build
knowledge incrementally, and produce intermediate artifacts that
can be validated independently.

DECISION 2: Separate handbook files vs. single document
CHOSEN: Separate handbook files
RATIONALE: Different stakeholders need different views of the
system. Separating handbooks allows targeted consumption and
easier maintenance.

DECISION 3: Mermaid diagrams as primary visualization
CHOSEN: Mermaid
RATIONALE: Mermaid is text-based, version-controllable, and
renderable by most markdown viewers. It does not require external
tools or binaries.

DECISION 4: Evidence-based analysis with citation requirements
CHOSEN: Required citations
RATIONALE: Prevents hallucination and ensures every claim can be
verified against the actual codebase.

DECISION 5: Progressive deepening (surface to deep)
CHOSEN: Progressive
RATIONALE: Mirrors how human engineers naturally understand
systems. Prevents analysis paralysis on large codebases.

========================================================================
END OF PROMPT DESIGN GUIDE
========================================================================
