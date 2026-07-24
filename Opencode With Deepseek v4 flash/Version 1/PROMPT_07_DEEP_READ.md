# PROMPT_07 — Phase 06: Deep Code Reading

## PHASE CLASS: Code-Level Analysis
## DEPENDENCIES: PROMPT_06 (Modules) — complete
## OUTPUT DIRECTORY: `re-docs/06-deep-read/`

---

## OBJECTIVE

Systematically read and analyze every significant file in the repository. Document every class, function, interface, type, constant, and significant code block. This is the most labor-intensive phase and produces the most detailed outputs.

## PREREQUISITES

- [ ] PROMPT_06 completed
- [ ] Module boundaries are known
- [ ] Public interfaces are identified

## INPUTS

- `re-docs/05-modules/02-module-interfaces.md` (for scoping)
- `re-docs/05-modules/03-module-internals.md` (for file lists)
- All source code files

## ANALYSIS STEPS

### Step 1: File Prioritization

Not all files are equal. Prioritize files for deep reading:

**Priority 1 (Must Read — Every Line)**:
- Entry points (server.ts, app.ts, main.py, etc.)
- Core service files
- Controller files
- Route/handler files
- Middleware files
- Configuration files
- Core domain logic files

**Priority 2 (Read Key Sections)**:
- Utility files (read exports and key functions)
- Model/entity files (read class/interface definitions)
- Type/definition files (read all definitions)
- Test files (read test structure and key test cases)

**Priority 3 (Scan)**:
- Auto-generated files
- Migration files
- Vendor files
- Configuration template files
- Third-party wrapper files

### Step 2: Per-File Analysis

For each high-priority file, perform this analysis:

#### 2.1 File Header
```
File: src/services/auth.service.ts
Path: relative/path
Module: Auth
Priority: 1
Lines: 245
Dependencies imported: 8
```

#### 2.2 Imports Analysis
- List every import
- Resolve each import to its actual file
- Categorize: internal module, external library, type-only

#### 2.3 Class Analysis (if applicable)
For each class:

- **Class name**
- **Extends/Implements**
- **Generic parameters**
- **Constructor parameters**
- **Properties** (with types and visibility)
- **Methods** (name, signature, visibility, description)
- **Static members**
- **Lifecycle hooks** (if any)
- **Usage locations**

#### 2.4 Function/Method Analysis
For each function/method:

- **Name**
- **Signature** (parameters with types, return type)
- **Description** (what does it do?)
- **Algorithm** (how does it work, briefly?)
- **Side effects** (does it modify state, write files, call external services?)
- **Error conditions** (when does it throw/return error?)
- **Complexity** (Big-O if apparent)
- **Dependencies** (other functions it calls)
- **Callers** (who calls this function?)
- **Async behavior** (is it async? What does it await?)

#### 2.5 Interface/Type Analysis
For each interface/type:

- **Name**
- **Properties/Members**
- **Generic parameters**
- **Extends**
- **Implementations** (which classes implement this?)

#### 2.6 Constants and Configuration
- All exported constants
- Configuration values
- Magic strings/numbers (FLAG these)

#### 2.7 Enums
- All enum definitions
- Enum members
- Usage locations

#### 2.8 Decorators/Annotations
- All decorators used
- Their purposes
- Configuration values

### Step 3: Pattern Recognition

While reading, identify:

- **Boilerplate patterns**: Code that follows a template pattern
- **Copy-paste patterns**: Code that appears to be duplicated
- **Inconsistencies**: Code that breaks the pattern
- **Complex code**: Code that requires special attention
- **Dead code**: Code that appears unused

### Step 4: Cross-File Tracing

For complex operations that span multiple files:

- Identify the entry point
- Trace through each file involved
- Document the complete flow
- Identify the exit/output

### Step 5: Code Quality Notes

For each file, note:
- Readability (easy, moderate, difficult)
- Comments (well-commented, under-commented, over-commented)
- Complexity (low, moderate, high)
- Test coverage (appears tested, appears untested)
- Code smells (if any)

## OUTPUT SPECIFICATION

### File: `01-auth-service.md` (one per key file)

Detailed analysis of each significant file.

### File: `02-index.md` — Complete index of all analyzed files

| File | Module | Priority | Key Contents |
|------|--------|----------|-------------|
| src/services/auth.ts | Auth | 1 | AuthService class, login/logout/refresh |

### File: `03-function-catalog.md`

Alphabetical catalog of every significant function:

| Function | File | Module | Purpose | Async | Complexity |
|----------|------|--------|---------|-------|------------|
| authenticate() | src/services/auth.ts | Auth | Verify credentials | No | O(n) |
| generateToken() | src/services/auth.ts | Auth | Create JWT | No | O(1) |

### File: `04-class-catalog.md`

Alphabetical catalog of every significant class:

| Class | File | Module | Purpose | Methods |
|-------|------|--------|---------|---------|
| AuthService | src/services/auth.ts | Auth | Auth business logic | 8 |

### File: `05-code-quality-notes.md`

Aggregated code quality observations.

### File: `06-deep-read-summary.md`

Summary:
- Total files analyzed
- Total functions/methods documented
- Total classes documented
- Total interfaces documented
- Code quality assessment
- Key areas of complexity
- Notable patterns and anti-patterns

## REQUIRED DIAGRAMS

### Diagram: Complex Function Flows

For any function with more than 10 steps or multiple branching paths:

```mermaid
flowchart TD
    A[Start: authenticate()] --> B{Valid credentials?}
    B -->|Yes| C[Generate access token]
    B -->|No| D[Return 401]
    C --> E[Generate refresh token]
    E --> F[Store refresh token]
    F --> G[Return tokens]
```

## VALIDATION CHECKS

- [ ] All Priority 1 files have complete analysis
- [ ] All Priority 2 files have key section analysis
- [ ] Every function in Priority 1 files is documented
- [ ] Every class in Priority 1 files is documented
- [ ] Every interface/type is documented
- [ ] Imports are traced for all analyzed files
- [ ] Function catalog is complete
- [ ] Class catalog is complete

## COMPLETION CHECKLIST

- [ ] All analyzed file outputs generated
- [ ] Index file created
- [ ] Function catalog compiled
- [ ] Class catalog compiled
- [ ] Code quality notes documented
- [ ] Deep read summary written
- [ ] All outputs saved to `re-docs/06-deep-read/`
- [ ] Phase validation checks passed

---

*Proceed to PROMPT_08_ARCHITECTURE.md only after all checklist items are complete.*
