# Output Rules

> **Document:** OUTPUT_RULES.md  
> **Version:** 1.0.0  
> **Purpose:** Define the rules for generating and structuring all documentation output

---

## 📄 OUTPUT STRUCTURE

### 1.1 Output Directory

All reverse engineering documentation must be written to:

```
[repository-name]_reverse_engineering/
```

### 1.2 Directory Structure

```
[repository-name]_reverse_engineering/
│
├── INDEX.md                               ← Master index for the documentation
│
├── 01_DISCOVERY/
│   ├── REPOSITORY_OVERVIEW.md
│   ├── FILE_INVENTORY.md
│   ├── LANGUAGE_AND_TECH_STACK.md
│   └── BUILD_AND_CONFIGURATION.md
│
├── 02_STRUCTURAL_ANALYSIS/
│   ├── MODULE_MAP.md
│   ├── FOLDER_RESPONSIBILITIES.md
│   ├── NAMING_CONVENTIONS.md
│   └── FILE_ORGANIZATION.md
│
├── 03_DEPENDENCY_ANALYSIS/
│   ├── INTERNAL_DEPENDENCIES.md
│   ├── EXTERNAL_DEPENDENCIES.md
│   ├── DEPENDENCY_GRAPH.md
│   └── IMPORT_ANALYSIS.md
│
├── 04_DEEP_ANALYSIS/
│   ├── FILE_ANALYSIS_INDEX.md
│   ├── [module-name]_ANALYSIS.md        ← One per major module
│   ├── ALGORITHM_ANALYSIS.md
│   └── CRITICAL_CODE_PATHS.md
│
├── 05_ARCHITECTURE/
│   ├── SYSTEM_ARCHITECTURE.md
│   ├── COMPONENT_ARCHITECTURE.md
│   ├── MODULE_ARCHITECTURE.md
│   ├── LAYER_DIAGRAM.md
│   ├── DATA_ARCHITECTURE.md
│   └── ARCHITECTURE_DECISIONS.md
│
├── 06_WORKFLOWS/
│   ├── WORKFLOW_INDEX.md
│   ├── [workflow-name]_WORKFLOW.md      ← One per workflow
│   ├── EXECUTION_PATHS.md
│   ├── STATE_TRANSITIONS.md
│   ├── EVENT_FLOW.md
│   └── ERROR_HANDLING_WORKFLOWS.md
│
├── 07_DESIGN_PATTERNS/
│   ├── PATTERN_CATALOG.md
│   ├── [pattern-name]_PATTERN.md        ← One per identified pattern
│   └── ENGINEERING_DECISIONS.md
│
├── 08_AI_WORKFLOWS/                     ← (If applicable)
│   ├── PROMPT_ARCHITECTURE.md
│   ├── AGENT_WORKFLOWS.md
│   ├── REASONING_PIPELINES.md
│   ├── PLANNING_PIPELINES.md
│   ├── TOOL_INTEGRATION.md
│   └── AI_SYSTEM_BOUNDARIES.md
│
├── 09_DOCUMENTATION/
│   ├── API_REFERENCE.md
│   ├── COMPONENT_REFERENCE.md
│   ├── FUNCTION_REFERENCE.md
│   ├── CLASS_REFERENCE.md
│   ├── CONFIGURATION_REFERENCE.md
│   └── ENVIRONMENT_VARIABLES.md
│
├── 10_VALIDATION/
│   ├── QUALITY_REPORT.md
│   ├── VALIDATION_CHECKLIST.md
│   ├── GAP_ANALYSIS.md
│   └── CONFIDENCE_ASSESSMENT.md
│
├── DIAGRAMS/
│   ├── SYSTEM_ARCHITECTURE.md
│   ├── COMPONENT_DIAGRAMS.md
│   ├── SEQUENCE_DIAGRAMS.md
│   ├── DEPENDENCY_GRAPHS.md
│   ├── WORKFLOW_DIAGRAMS.md
│   └── STATE_DIAGRAMS.md
│
├── REBUILD_GUIDE.md
└── ENGINEERING_NOTES.md
```

---

## 📝 DOCUMENT STRUCTURE RULES

### 2.1 Required Front Matter

Every documentation file MUST begin with:

```markdown
# [Document Title]

> **Document:** [filename.md]  
> **Phase:** [Phase Name]  
> **Last Updated:** [YYYY-MM-DD]  
> **Purpose:** [Brief description of this document's purpose]

---
```

### 2.2 Required Sections

Each document must contain, in order:

1. **Title** — Clear, descriptive title
2. **Metadata** — Front matter (above)
3. **Purpose** — What this document covers
4. **Content** — The main body, structured with headings
5. **Cross-References** — Links to related documents
6. **Confidence Assessment** — Confidence in the information

### 2.3 Optional Sections

- **Changelog** — If the document is updated
- **Notes** — Additional context or caveats
- **Open Questions** — Things that remain unclear
- **Visual Diagram** — Embedded Mermaid diagram

---

## ✍️ WRITING STYLE RULES

### 3.1 Voice and Tone
- Use **active voice** consistently.
- Be **precise and technical**.
- Be **objective**—describe what the code does.
- Avoid **marketing language** ("elegant," "beautiful," "clever").
- Avoid **hedging language** ("seems to," "appears to," "sort of").

### 3.2 Technical Precision
- Use **exact file paths** when referencing files.
- Use **exact function names** when referencing functions.
- Use **exact line numbers** when referencing specific code.
- Use **exact version numbers** when referencing dependencies.
- Use **standard terminology** for design patterns, algorithms, etc.

### 3.3 Code Examples
- Always specify the language in code blocks: ```python
- Include relevant imports when showing code examples.
- Show context (surrounding code) when relevant.
- Never truncate code examples with "..."
- Verify code examples against the source.

### 3.4 Formatting
- Use `code` for file names, function names, variable names.
- Use **bold** for key concepts on first mention.
- Use `> blockquotes` for warnings, notes, and important callouts.
- Use --- horizontal rules to separate major sections.
- Use consistent heading hierarchy (no skipping levels).

---

## 📊 DIAGRAM RULES

### 4.1 Diagram Format
All diagrams must use **Mermaid.js** syntax.

### 4.2 Diagram Standards
- Every diagram must have a clear title.
- Every diagram must have consistent styling.
- Node labels must be descriptive, not cryptic.
- Line labels must describe the relationship.
- Colors may be used but must have a legend.

### 4.3 Required Diagrams
| Diagram Type | When Required |
|-------------|---------------|
| System Architecture | Always |
| Component Diagram | When multiple components exist |
| Dependency Graph | Always (as separate or embedded) |
| Sequence Diagrams | When workflows exist |
| State Diagrams | When state machines exist |
| Class Diagrams | When OOP is used |
| Data Flow Diagrams | When data pipelines exist |

---

## 🔗 CROSS-REFERENCE RULES

### 5.1 Cross-Reference Format
```markdown
**See Also:**
- [Document Name](path/to/document.md) — Brief description
- [Related Concept](../05_ARCHITECTURE/SYSTEM_ARCHITECTURE.md)
```

### 5.2 Cross-Reference Requirements
- Every document must reference related documents.
- Every component must reference its parent module.
- Every workflow must reference the components it touches.
- Every dependency must reference where it's used.
- Cross-references must be meaningful, not mechanical.

---

## 🏷️ METADATA RULES

### 6.1 File Metadata
Every generated file must include:

```markdown
> **File:** path/to/file.extension  
> **Purpose:** What this file does  
> **Analyzed:** Yes/No (analysis status)  
> **Dependencies:** [List of dependencies]  
> **Referenced By:** [List of files that reference this]  
```

### 6.2 Function/Class Metadata
Every documented function or class must include:

```markdown
### `functionName(param1, param2)`
- **File:** path/to/file.extension
- **Line:** 42
- **Purpose:** What this function does
- **Parameters:** param1 (type) - description, param2 (type) - description
- **Returns:** type - description
- **Throws:** ErrorType - condition
- **Called By:** [list of callers]
- **Calls:** [list of callees]
```

---

## ✅ OUTPUT COMPLIANCE CHECKLIST

Before writing any documentation file:

- [ ] Does the file follow the required structure?
- [ ] Does the file have proper front matter?
- [ ] Are all cross-references accurate?
- [ ] Are all code examples verified?
- [ ] Are all file paths correct?
- [ ] Are diagrams in Mermaid syntax?
- [ ] Is terminology consistent with other files?
- [ ] Is confidence level documented?
- [ ] Is the file useful to the target audience?
- [ ] Does the file meet quality standards?

**Never output a file that fails these checks.**

