# PROMPT DESIGN GUIDE

## Guidelines for Prompt Structure and Execution

---

## 1. PROMPT PHILOSOPHY

### 1.1 Core Principles

**Principle 1: Clarity Over Cleverness**
- Prompts must be unambiguous
- Avoid clever phrasing that could be misinterpreted
- Use direct, imperative language

**Principle 2: Completeness Over Brevity**
- Include all necessary context
- Provide sufficient examples
- Never sacrifice clarity for brevity

**Principle 3: Structure Over Freedom**
- Provide clear structure for responses
- Define expected output formats
- Guide the AI toward consistent outputs

**Principle 4: Verification Over Assumption**
- Require evidence for all claims
- Mandate cross-referencing
- Build in self-validation

### 1.2 Prompt Design Goals

| Goal | Description |
|------|-------------|
| Precision | Every instruction has one interpretation |
| Completeness | All necessary information is provided |
| Actionability | AI knows exactly what to do |
| Verifiability | Outputs can be validated |
| Reusability | Prompts work across repositories |
| Scalability | Prompts work for any codebase size |

---

## 2. PROMPT STRUCTURE

### 2.1 Standard Prompt Template

Every execution prompt follows this structure:

```markdown
# PROMPT XX: [DESCRIPTIVE TITLE]

## Context Setting
[Brief reminder of where we are in the process]

## Objective
[Clear statement of what this prompt achieves]

## Scope
[What is included and excluded from this analysis]

## Instructions
[Step-by-step instructions for execution]

## Required Analysis
[Detailed list of what must be analyzed]

## Required Outputs
[Specific deliverables with format specifications]

## Output Format
[Template or structure for the response]

## Quality Criteria
[How to know the output is acceptable]

## Evidence Requirements
[What evidence must be provided]

## Common Pitfalls
[Warnings about common mistakes]

## Examples
[Examples of good outputs when helpful]

## Continuation Guidance
[How to handle large analyses]

## Self-Validation Checklist
[Checklist for AI to verify its own work]
```

### 2.2 Section Specifications

#### Context Setting (Required)
Purpose: Orient the AI to current position in workflow

```markdown
## Context Setting

You have completed:
- PROMPT_01: Repository Discovery
- PROMPT_02: Technology Stack Analysis

You are now executing:
- PROMPT_03: Architecture Extraction

This prompt builds upon your repository inventory and 
technology identification to extract architectural patterns.
```

#### Objective (Required)
Purpose: Clear single-sentence goal

```markdown
## Objective

Extract and document the complete system architecture, 
including architectural patterns, component organization, 
and high-level design decisions.
```

#### Scope (Required)
Purpose: Define boundaries

```markdown
## Scope

INCLUDED:
- System-level architecture patterns
- Component boundaries and responsibilities
- Inter-component communication
- Architectural decision records

EXCLUDED:
- Detailed implementation logic (covered in later prompts)
- Individual function documentation
- Third-party architecture (document via dependencies)
```

#### Instructions (Required)
Purpose: Step-by-step execution guide

```markdown
## Instructions

1. BEGIN by reviewing all files identified in PROMPT_01
2. IDENTIFY architectural entry points (main files, app roots)
3. TRACE imports and dependencies to find components
4. MAP component relationships
5. IDENTIFY architectural patterns in use
6. DOCUMENT findings using the specified format
7. VALIDATE against quality criteria before completing
```

#### Required Analysis (Required)
Purpose: Specific items to analyze

```markdown
## Required Analysis

You MUST analyze:
- All files matching patterns: *app*, *main*, *index*, *entry*
- All configuration files defining structure
- All module/package definition files
- All files at root of src/ directories
- Any file importing more than 5 other modules
```

#### Required Outputs (Required)
Purpose: Deliverable specification

```markdown
## Required Outputs

1. **Architecture Overview** (300-500 words)
2. **Component Diagram** (Mermaid)
3. **Component Responsibility Table**
4. **Communication Pattern Documentation**
5. **Architectural Decision Log**
6. **Evidence References** (file paths with line numbers)
```

#### Output Format (Required)
Purpose: Response structure template

```markdown
## Output Format

Structure your response as follows:

### 1. Architecture Overview
[Text description]

### 2. Component Diagram
```mermaid
[Diagram code]
```

### 3. Component Responsibilities
| Component | Responsibility | Files |
|-----------|---------------|-------|
| ... | ... | ... |

### 4. Communication Patterns
[Description with evidence]

### 5. Architectural Decisions
| Decision | Evidence | Rationale |
|----------|----------|-----------|
| ... | ... | ... |

### 6. Evidence References
- File: path/to/file.ts, lines 10-25
- File: path/to/file.py, lines 5-15
```

#### Quality Criteria (Required)
Purpose: Acceptance standards

```markdown
## Quality Criteria

Your output is acceptable only if:
- [ ] Every component is traced to actual files
- [ ] Diagram accurately reflects code structure
- [ ] All claims have file references
- [ ] No component is undocumented
- [ ] Communication patterns are evidenced
```

#### Evidence Requirements (Required)
Purpose: Proof standards

```markdown
## Evidence Requirements

For each claim, provide:
- File path
- Line numbers (when specific)
- Code snippet (when clarifying)
- Import/export statements (for dependencies)

Example:
CLAIM: "AuthService depends on TokenService"
EVIDENCE: src/auth/auth.service.ts, line 5
         import { TokenService } from './token.service';
```

#### Common Pitfalls (Required)
Purpose: Error prevention

```markdown
## Common Pitfalls

AVOID:
❌ Assuming architecture from folder names alone
❌ Missing implicit dependencies
❌ Confusing runtime architecture with file structure
❌ Overlooking configuration-driven architecture
❌ Ignoring dynamic imports

INSTEAD:
✅ Verify architecture through import analysis
✅ Trace all dependency paths
✅ Distinguish between physical and logical structure
✅ Check configuration files for architecture hints
✅ Search for dynamic import patterns
```

#### Examples (Optional)
Purpose: Illustrate expected quality

```markdown
## Example: Good Component Documentation

GOOD:
```
Component: AuthenticationModule
Responsibility: Handle all authentication operations
Files: 
  - src/auth/auth.module.ts (main module)
  - src/auth/auth.service.ts (business logic)
  - src/auth/auth.controller.ts (API endpoints)
Dependencies:
  - TokenService (internal)
  - UserService (internal)
  - @nestjs/jwt (external)
Evidence: src/auth/auth.module.ts imports TokenService at line 3
```
```

#### Continuation Guidance (Required)
Purpose: Handle large analyses

```markdown
## Continuation Guidance

If analysis exceeds response limits:

1. COMPLETE all sections partially rather than skipping any
2. MARK continuation points clearly: [CONTINUES IN NEXT RESPONSE]
3. MAINTAIN section numbering across continuations
4. BEGIN continuation with: [CONTINUING PROMPT_XX - SECTION Y]
5. END with summary of what remains

Priority order if truncation unavoidable:
1. Required diagrams
2. Component inventories
3. Relationship mappings
4. Detailed explanations
```

#### Self-Validation Checklist (Required)
Purpose: Quality assurance

```markdown
## Self-Validation Checklist

Before submitting your response, verify:

CONTENT:
- [ ] All required sections are present
- [ ] All components are documented
- [ ] All relationships are mapped

EVIDENCE:
- [ ] Every claim has a file reference
- [ ] Diagram matches code structure
- [ ] No unsupported assertions

QUALITY:
- [ ] Writing is clear and precise
- [ ] Terminology is consistent
- [ ] Formatting is correct

COMPLETENESS:
- [ ] Nothing marked TODO remains
- [ ] All open questions are logged
- [ ] Analysis scope is fully covered
```

---

## 3. PROMPT EXECUTION METHODOLOGY

### 3.1 Pre-Execution Checklist

Before executing any prompt:

```markdown
PRE-EXECUTION CHECKLIST:
- [ ] Previous prompt outputs reviewed
- [ ] Context understood
- [ ] Required inputs available
- [ ] Output templates ready
- [ ] Evidence collection method prepared
```

### 3.2 Execution Protocol

**Step 1: Context Review**
- Re-read previous prompt outputs
- Identify carry-forward items
- Note open questions from previous analysis

**Step 2: Scope Confirmation**
- Confirm understanding of scope
- Identify boundary cases
- Plan approach for edge cases

**Step 3: Systematic Analysis**
- Follow instructions precisely
- Document as you analyze
- Collect evidence continuously

**Step 4: Output Generation**
- Use specified templates
- Include all required sections
- Format according to specifications

**Step 5: Self-Validation**
- Run through validation checklist
- Fix any identified issues
- Confirm quality criteria met

### 3.3 Post-Execution Protocol

After completing each prompt:

```markdown
POST-EXECUTION CHECKLIST:
- [ ] All required outputs generated
- [ ] Self-validation passed
- [ ] Evidence properly referenced
- [ ] Open questions logged
- [ ] Ready to proceed to next prompt
```

---

## 4. PROMPT CHAINING STRATEGY

### 4.1 Sequential Dependencies

Prompts are designed to build upon each other:

```
PROMPT_01 → Provides file inventory for all subsequent prompts
PROMPT_02 → Provides tech context for architecture analysis
PROMPT_03 → Provides architecture for code structure context
PROMPT_04 → Provides code structure for dependency mapping
PROMPT_05 → Provides dependencies for flow analysis
PROMPT_06 → Provides data flows for control flow context
PROMPT_07 → Provides control flows for API analysis
PROMPT_08 → Provides APIs for business logic context
PROMPT_09 → Provides business logic for specialized analysis
PROMPT_10-13 → Provide specialized analysis for synthesis
PROMPT_14 → Synthesizes all previous analysis
```

### 4.2 Cross-Reference Requirements

Each prompt must reference relevant previous outputs:

```markdown
CROSS-REFERENCE EXAMPLE:

As identified in PROMPT_01, the repository contains 47 TypeScript files.
Of these, PROMPT_02 identified 12 as framework configuration files.
This analysis focuses on the remaining 35 application files.
```

### 4.3 Iterative Refinement

Later prompts may refine earlier conclusions:

```markdown
REFINEMENT PROTOCOL:

If later analysis contradicts earlier findings:

1. DOCUMENT the contradiction explicitly
2. ANALYZE which conclusion is correct
3. UPDATE the earlier conclusion with note
4. EXPLAIN the refinement in current output

Example:
"Initial analysis in PROMPT_03 suggested MVC pattern.
Deeper analysis in PROMPT_09 reveals hexagonal architecture.
This refinement is based on business logic isolation patterns."
```

---

## 5. ADAPTIVE PROMPT STRATEGIES

### 5.1 Repository Size Adaptation

**Small Repositories (< 50 files):**
- Analyze every file in detail
- Document all functions
- Complete call graphs

**Medium Repositories (50-500 files):**
- Prioritize by importance
- Sample representative files per category
- Focus on public interfaces

**Large Repositories (500+ files):**
- Architecture-first approach
- Deep-dive on critical paths only
- Summarize repetitive patterns

### 5.2 Technology-Specific Adaptation

**Frontend Projects:**
- Emphasize component hierarchies
- Document state management
- Map UI flows

**Backend Projects:**
- Emphasize API structures
- Document data models
- Map service layers

**AI/ML Projects:**
- Emphasize model architectures
- Document prompt structures
- Map reasoning pipelines

**Library Projects:**
- Emphasize public APIs
- Document extension points
- Map usage patterns

### 5.3 Complexity-Based Adaptation

**Low Complexity:**
- Straightforward documentation
- Minimal diagramming
- Direct explanations

**High Complexity:**
- Layered documentation
- Extensive diagramming
- Multiple abstraction levels
- Detailed cross-references

---

## 6. QUALITY ASSURANCE IN PROMPTS

### 6.1 Built-In Validation

Every prompt includes:

1. **Self-Check Questions**
   - Embedded throughout instructions
   - Force reflection before proceeding

2. **Evidence Requirements**
   - Prevent unsupported claims
   - Enable verification

3. **Completeness Checklists**
   - Ensure nothing missed
   - Provide completion criteria

### 6.2 Error Prevention

Prompts prevent errors through:

1. **Explicit Warnings**
   - Common pitfalls documented
   - Anti-patterns highlighted

2. **Structured Templates**
   - Reduce formatting errors
   - Ensure consistency

3. **Progressive Disclosure**
   - Complex tasks broken down
   - Cognitive load managed

### 6.3 Recovery Mechanisms

When things go wrong:

1. **Ambiguity Handling**
   - Document ambiguity explicitly
   - Provide multiple interpretations
   - Flag for human review

2. **Gap Management**
   - Acknowledge what cannot be determined
   - Log gaps systematically
   - Suggest verification methods

3. **Correction Protocol**
   - Allow revision of earlier conclusions
   - Document rationale for changes
   - Maintain audit trail

---

## 7. PROMPT MAINTENANCE

### 7.1 Version Control

- Each prompt versioned independently
- Change log maintained
- Backward compatibility considered

### 7.2 Feedback Integration

- Track execution issues
- Collect improvement suggestions
- Update prompts iteratively

### 7.3 Testing Protocol

Before deploying prompt changes:

1. Test on small repository
2. Test on medium repository
3. Test on large repository
4. Verify output quality
5. Confirm no regressions

---

*This guide defines how all prompts in this framework should be structured, executed, and maintained. All prompt authors must follow these guidelines.*
