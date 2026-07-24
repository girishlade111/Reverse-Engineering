# Phase 4: Deep Code Analysis

> **Document:** PROMPT_04.md  
> **Phase:** 4 of 10  
> **Purpose:** Perform deep analysis of every significant file, algorithm, and code path  
> **Prerequisite:** Phase 3 complete; dependency structure understood

---

## 📋 PHASE INFORMATION

| Property | Value |
|----------|-------|
| **Phase** | 4 — Deep Code Analysis |
| **Entry Criteria** | Phase 3 complete; dependency graphs available; module structure understood |
| **Exit Criteria** | All code analyzed; algorithms documented; critical paths traced |
| **Estimated Effort** | Very High |

---

## 🎯 OBJECTIVES

1. **Analyze** every source file's logic completely.
2. **Document** every function's purpose, parameters, and behavior.
3. **Trace** algorithms and understand their logic.
4. **Identify** critical code paths and core logic.
5. **Map** error handling, edge cases, and failure modes.
6. **Build** comprehensive understanding of code-level behavior.

---

## 🔬 METHODOLOGY

### Step 1: File Prioritization

Prioritize files for analysis:

| Priority | Category | Description |
|----------|----------|-------------|
| **P0** | Entry Points | Main files, app bootstrap, server start |
| **P0** | Core Logic | Business logic, algorithm files |
| **P0** | Data Models | Data structures, entities, schemas |
| **P1** | Controllers/Handlers | API handlers, event handlers |
| **P1** | Services | Business service implementations |
| **P1** | Repositories/Data Access | Database access, API clients |
| **P2** | Utilities/Helpers | Shared utility functions |
| **P2** | Middleware | Request/response middleware |
| **P2** | Configuration | Configuration files, settings |
| **P3** | Tests | Test files (analyze for structure) |
| **P3** | Scripts | Build, deployment scripts |
| **P4** | Documentation | Existing documentation |

### Step 2: File-Level Analysis

For every source file, document:

```markdown
### [relative/path/to/file]
- **Purpose:** [What this file does]
- **Language:** [Language]
- **Lines of Code:** [LOC]
- **Complexity:** Low / Medium / High
- **Dependencies (internal):** [Files imported from this project]
- **Dependencies (external):** [External libraries used]
- **Exports:** [APIs, classes, functions exported]

#### Key Functions/Classes

##### `functionName(param1, param2)`
- **Line:** [line number]
- **Purpose:** [What this function does]
- **Parameters:**
  - `param1` (Type): Description
  - `param2` (Type): Description
- **Returns:** (Type): Description
- **Throws:** [Error types and conditions]
- **Algorithm:** [Brief algorithm description]
- **Complexity:** O(n) / O(log n) / etc.
- **Called By:** [List of callers]
- **Calls:** [List of callees]
- **Error Handling:** [Error handling logic]
- **Edge Cases:** [Edge cases handled]

##### `ClassName`
- **Line:** [line number]
- **Purpose:** [What this class does]
- **Properties:**
  - `property` (Type): Description
- **Methods:**
  - `methodName()`: Description
- **Relationships:** [Inheritance, composition, dependencies]
- **State Management:** [How state is managed]
- **Lifecycle:** [Creation, usage, destruction]
```

### Step 3: Algorithm Deep Analysis

For any non-trivial algorithm, provide:

```markdown
### Algorithm: [Algorithm Name]
- **File:** [File path]
- **Function:** [Function name]
- **Type:** [Sorting / Search / Optimization / ML / Crypto / Custom]

#### Step-by-Step Logic
1. [Step 1: What happens]
2. [Step 2: What happens]
3. [Step 3: Decision point]
   - If condition A: [path A]
   - If condition B: [path B]
4. [Step 4: What happens]
5. [Return result]

#### Complexity Analysis
- **Time Complexity:** O(n) — Linear
- **Space Complexity:** O(n) — Linear
- **Best Case:** [description]
- **Average Case:** [description]
- **Worst Case:** [description]

#### Pseudocode
```python
# Simplified representation of the algorithm
def algorithm_name(input):
    # Step 1
    intermediate = process(input)
    
    # Step 2
    if condition:
        result = branch_a(intermediate)
    else:
        result = branch_b(intermediate)
    
    # Step 3
    return finalize(result)
```

#### Edge Cases
- Input = None/Empty: [behavior]
- Input = Maximum: [behavior]
- Duplicates: [behavior]
- Race conditions: [if applicable]
```

### Step 4: Critical Code Path Tracing

Trace the most critical execution paths:

```
### Critical Path: [Path Name]
- **Trigger:** [What initiates this path]
- **End:** [Where this path terminates]
- **Importance:** [Why this path is critical]

#### Path Steps
1. [File:function:line] — [Description of step]
2. [File:function:line] — [Description of step]
3. [Decision point at File:function:line]
   - Path A: [leads to...]
   - Path B: [leads to...]
4. [File:function:line] — [Description of step]

#### Error Paths
- If [condition] fails: [error handling at File:line]
- If [condition] fails: [error handling at File:line]
```

### Step 5: Error Handling Analysis

For each module, document error handling:

```markdown
### Error Handling: [Module Name]

#### Error Categories
| Error Type | Source | Handling Strategy |
|------------|--------|-------------------|
| Validation | User input | Return validation error |
| Authentication | Auth middleware | Redirect / 401 |
| Not Found | Database | Return 404 |
| Timeout | External API | Retry (3x, exponential backoff) |
| System Error | Internal | Log + return 500 |

#### Retry Strategy
- **Retry Count:** [number]
- **Backoff:** [Linear / Exponential / Constant]
- **Timeout:** [duration]
- **Jitter:** [Yes / No]
- **Fallback:** [Fallback behavior]

#### Logging
- **Log Level Usage:** DEBUG / INFO / WARN / ERROR / FATAL
- **Log Format:** [JSON / Text / Structured]
- **Log Destinations:** [Console / File / External Service]
- **Error Tracking:** [Integration with error tracking service]
```

### Step 6: Knowledge Base Update

```json
{
  "file_analyses": { /* file-level analysis results */ },
  "algorithm_analyses": { /* algorithm deep dives */ },
  "critical_paths": { /* traced critical paths */ },
  "error_handling": { /* error handling analysis */ },
  "phase_4_notes": {
    "complex_components": [],
    "debt_or_technical_issues": [],
    "code_quality_observations": [],
    "open_questions": []
  }
}
```

---

## 🛠️ TOOLS

| Tool | Purpose | Usage |
|------|---------|-------|
| `read_file` | Read source files | Primary tool — read every file |
| `search_files` | Find patterns | Search for error handling, patterns |
| `execute_command` | Run analysis | Linters, complexity tools |

---

## 📚 KNOWLEDGE BASE UPDATE

Add to the working knowledge base:

1. **FileAnalyses:** Detailed analysis of every source file
2. **AlgorithmAnalyses:** Deep analysis of algorithms
3. **CriticalPaths:** Traced execution paths
4. **ErrorHandling:** Error handling patterns across the repository

---

## 📦 DELIVERABLES

Phase 4 produces:

1. `04_DEEP_ANALYSIS/FILE_ANALYSIS_INDEX.md` — Index of all analyzed files
2. `04_DEEP_ANALYSIS/[module-name]_ANALYSIS.md` — Per-module deep analysis
3. `04_DEEP_ANALYSIS/ALGORITHM_ANALYSIS.md` — Algorithm documentation
4. `04_DEEP_ANALYSIS/CRITICAL_CODE_PATHS.md` — Critical path documentation

---

## ✅ QUALITY CHECK

- [ ] Every source file has been read and analyzed?
- [ ] Every function documented (at least with purpose and signature)?
- [ ] Every algorithm traced and understood?
- [ ] Critical code paths identified and traced?
- [ ] Error handling documented?
- [ ] No file left unread (except generated/third-party)?

---

## 🚪 PHASE COMPLETION GATE

Before proceeding to Phase 5:

1. Confirm all P0 and P1 files are analyzed.
2. Confirm algorithms are understood.
3. Confirm critical paths are traced.
4. Confirm error handling is documented.
5. **If any file was not analyzed, flag it and document why.**

---

**PROCEED TO PHASE 5 → `PROMPT_05.md`**

