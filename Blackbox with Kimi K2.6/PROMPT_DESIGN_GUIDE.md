# Prompt Design Guide

## Enterprise Reverse Engineering Prompt Framework

---

## 1. Design Philosophy

### 1.1 Core Principles

**Principle 1: Structured Decomposition**
Every complex analysis task must be broken into manageable, sequential subtasks. Each subtask produces output that feeds the next.

**Principle 2: Explicit Instructions**
Leave nothing to interpretation. Every instruction must be unambiguous, with clear success criteria and output expectations.

**Principle 3: Context Preservation**
Each prompt must carry forward all relevant context from previous prompts. No prompt operates in isolation.

**Principle 4: Verifiability**
Every prompt must include mechanisms to verify that its instructions were followed correctly.

**Principle 5: Error Containment**
When errors occur, they must be contained to the current prompt and not corrupt the analysis of other prompts.

---

## 2. Prompt Template Structure

Every prompt in this framework follows this exact template:

```markdown
# PROMPT_XX: [Prompt Name]

## Classification
- **Domain:** [Domain Name]
- **Phase:** [Phase Number]
- **Prerequisites:** PXX, PXX
- **Dependencies:** [External dependencies]
- **Estimated Effort:** [Time/file estimate]

## Objective
[One-paragraph description of what this prompt achieves]

## Input Requirements
### Required Context
- [Context item 1]
- [Context item 2]

### Required Files
- [File or data needed]

## Pre-Analysis Checklist
- [ ] [Checklist item 1]
- [ ] [Checklist item 2]

## Analysis Tasks

### Task 1: [Task Name]
**Purpose:** [Why this task exists]

**Instructions:**
[Detailed step-by-step instructions]

**Success Criteria:**
- [Criterion 1]
- [Criterion 2]

**Output Format:**
```markdown
## [Task Output Header]
[Specific output structure]
```

### Task 2: [Task Name]
[Repeat structure as needed]

## Synthesis
**Purpose:** Combine all task outputs into a unified analysis

**Instructions:**
[How to synthesize findings]

## Output Requirements
### Required Deliverables
- [Deliverable 1]
- [Deliverable 2]

### Output Structure
[Specified structure for the final output]

## Quality Checks
- [ ] Check 1
- [ ] Check 2

## Continuation Rules
[How to handle overflow or partial completion]

## Cross-References
- Links to related prompts
- Links to shared context
```

---

## 3. Prompt Design Patterns

### Pattern 1: Progressive Disclosure
Start with broad analysis, progressively narrow to specifics. Used in architecture reconstruction.

```
Scan all files → Identify major modules → Analyze each module → Deep dive into key files
```

### Pattern 2: Divide and Conquer
Split large analysis into parallel subtasks, then merge results. Used in large repositories.

```
Split repository into domains → Analyze each domain independently → Cross-reference findings → Merge into unified view
```

### Pattern 3: Verification Loops
After each analysis step, verify before proceeding. Used in critical path analysis.

```
Analyze → Cross-verify with related files → Correct errors → Proceed
```

### Pattern 4: Context Accumulation
Each step adds to a growing context document. Used throughout the framework.

```
Initialize context → Step 1 adds to context → Step 2 builds on context → ...
```

### Pattern 5: Depth Modulation
Adjust analysis depth based on component criticality. Used in all phases.

```
Classify component → If core: maximum depth → If utility: standard depth → If config: surface depth
```

---

## 4. Instruction Writing Standards

### 4.1 Clarity Rules
- Use imperative mood: "Scan the repository", not "The repository should be scanned"
- Be specific: "Read all files with .py extension" not "Read Python files"
- Include examples: "For example: `function_name(param1, param2)`"
- Define all technical terms used

### 4.2 Completeness Rules
- Specify what to do AND what not to do
- Include edge case handling
- Define fallback behaviors
- Specify when to stop

### 4.3 Consistency Rules
- Use consistent terminology throughout
- Maintain consistent heading levels
- Use consistent formatting for code, output, and metadata
- Follow consistent numbering schemes

---

## 5. Output Design Standards

### 5.1 Structure
- Use hierarchical sections (H1 → H2 → H3 → H4)
- Include table of contents for long outputs
- Use consistent metadata headers
- Include source references

### 5.2 Formatting
- Code blocks with language specification
- Tables with aligned columns
- Lists for enumerable items
- Blockquotes for important notes
- Horizontal rules for section separation

### 5.3 Diagrams
- Use Mermaid.js for diagrams
- Include alt text for accessibility
- Keep diagrams focused on one concept
- Provide text summary alongside diagrams

---

## 6. Error Handling in Prompts

### 6.1 File Not Found
```markdown
If [file] is not found:
1. Search for alternative locations
2. Document the absence
3. Flag as [GAP: FILE_NOT_FOUND]
4. Proceed with available information
```

### 6.2 Language Unsupported
```markdown
If [language] is not in expected set:
1. Classify based on file extension
2. Use generic code analysis patterns
3. Document as [UNSUPPORTED_LANGUAGE]
4. Limit analysis to structural elements
```

### 6.3 Analysis Exceeds Limits
```markdown
If analysis exceeds response limits:
1. Complete current logical section
2. Add [CONTINUATION_POINT] marker
3. Summarize completed and remaining work
4. Request continuation
```

---

## 7. Cross-Prompt Communication

### 7.1 Shared Context Format
All prompts must use this format for shared context:

```
yaml
shared_context:
  repository:
    name: <string>
    path: <string>
    language: <string>
    framework: <string>
    total_files: <integer>
    total_lines: <integer>
  analysis_state:
    phase: <integer>
    completed_prompts: <list>
    current_prompt: <string>
    gaps_found: <list>
  architecture:
    modules: <list>
    components: <list>
    data_flows: <list>
    dependencies: <list>
```

### 7.2 Gap Propagation
When a gap is identified in one prompt, it must be:
1. Documented in the prompt's output
2. Added to the shared context
3. Flagged for dependent prompts
4. Addressed in validation phase

---

## 8. Prompt Testing Criteria

Every prompt must pass:
1. **Clarity Test**: Can another human understand what to do?
2. **Completeness Test**: Are all scenarios covered?
3. **Sequencing Test**: Does the order make logical sense?
4. **Dependency Test**: Are all prerequisites specified?
5. **Output Test**: Can the expected output be produced?
6. **Edge Case Test**: Are unusual scenarios handled?
7. **Recovery Test**: Can errors be recovered from?
