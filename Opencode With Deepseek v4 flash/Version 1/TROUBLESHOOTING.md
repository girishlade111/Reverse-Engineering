# REVERSE ENGINEERING TROUBLESHOOTING GUIDE

## FRAMEWORK: Enterprise Reverse Engineering Prompt Framework
## PURPOSE: Diagnose and resolve common failures during the reverse engineering process

---

## T1 — FILE PARSING FAILURES

### Symptom: Cannot parse a source file
**Common causes**: Minified code, generated code, binary files, unusual encoding, corrupted files

**Resolution steps**:
1. Check file extension — binary files (.exe, .dll, .so, .class) are expected and should be skipped
2. Check file size — files >10MB may be truncated or minified
3. Check encoding — try UTF-8, UTF-16, Latin-1
4. For minified code: note that it exists, document its purpose from imports/exports only, do not spend time on full analysis
5. For generated code: check for header comments identifying the generator; analyze the generator's output structure, not the content
6. Log at `re-docs/ERROR_LOG.md` with file:line and reason

### Symptom: Language detection fails
**Common causes**: Polyglot files, template languages, custom DSLs, unknown extensions

**Resolution steps**:
1. Check file content for shebang lines (`#!/usr/bin/env node`)
2. Check for package manager files (package.json -> JS/TS, Cargo.toml -> Rust, etc.)
3. For unknown extensions: read first 50 lines to determine syntax
4. For template languages: treat as the host language with template extensions
5. Log the ambiguity with accuracy tier B or C

---

## T2 — IMPORT/DEPENDENCY RESOLUTION FAILURES

### Symptom: Import refers to a file that doesn't exist
**Common causes**: Dynamic imports, generated files, deleted files, build-time resolution, path aliases not yet resolved

**Resolution steps**:
1. Check for path aliases in tsconfig, webpack, vite, or similar config files
2. Check for barrel exports (index files re-exporting from submodules)
3. Check if the import is dynamic (e.g., `import(variable)`): document intent, cannot resolve statically
4. Check if the import is to a generated file: identify the generator
5. If unresolvable: note as GAP with accuracy C or D

### Symptom: Circular dependencies detected
**Common causes**: Design issues, barrel imports, cross-referencing types

**Resolution steps**:
1. Identify the cycle path (A → B → C → A)
2. Determine if it's type-only (safe, TypeScript/Flow) or runtime (problematic)
3. Document severity: type-only cycles are acceptable; runtime cycles indicate design issues
4. Log in engineering notes as technical debt

---

## T3 — REPOSITORY SIZE MANAGEMENT

### Symptom: Repository too large for a single session (>5000 files)
**Resolution steps**:
1. Use multi-session mode (see MASTER_INDEX.md)
2. Prioritize Phase 00 (Scouting) to identify the most important modules
3. Analyze each major module independently in separate sessions
4. Each session should cover complete phases 05-12 for its module
5. Assemble all module analyses in Phase 18 (Documentation Generation)
6. Cross-reference between modules in Phase 19 (Validation)

### Symptom: Individual file too large (>2000 lines)
**Resolution steps**:
1. Read the file in sections: exports first, then key functions, then the rest
2. Focus on public API surface and complex functions
3. Utility functions, getters, and simple methods can be summarized
4. Mark as Priority 2 (partial read) in file analysis

### Symptom: AI agent context window is full
**Resolution steps**:
1. Complete the current phase and commit all outputs
2. Start a new session with explicit reference to the completed phase directory
3. Use the phase dependency chain in MASTER_PROMPT.md to signal where to resume
4. For large outputs, split into sub-files per module within the phase directory

---

## T4 — AMBIGUOUS CODE

### Symptom: Function or module purpose is unclear
**Common causes**: Poor naming, missing documentation, over-abstracted code, dead code

**Resolution steps**:
1. Check all callers — how the function is used often reveals its purpose
2. Check test files — tests document expected behavior
3. Check for comments, JSDoc, or type annotations
4. Check git history for commit messages or PR descriptions
5. If still unclear: document as GAP with tier C or D

### Symptom: Conflicting interpretations of architecture
**Common causes**: Multiple architectural styles mixed, layered but inconsistent, evolution over time

**Resolution steps**:
1. Identify the dominant architectural pattern (what most code conforms to)
2. Identify deviations and when they were introduced (check git history)
3. Document both the intended architecture and the actual architecture
4. Note architectural drift in engineering notes as technical debt

---

## T5 — AI WORKFLOW ANALYSIS FAILURES

### Symptom: Cannot trace a prompt through to the code that executes it
**Common causes**: Dynamic prompt construction, prompts stored in databases, external prompt management systems

**Resolution steps**:
1. Search for string fragments of the prompt in the codebase
2. Search for variable names used in the prompt template
3. Check for external prompt management tools (LangSmith, custom databases)
4. Check if prompts are loaded from files or a file system
5. If external: document the external system and its API

### Symptom: Agent architecture cannot be reconstructed
**Common causes**: Custom agent implementations, heavily modified frameworks, no clear boundary

**Resolution steps**:
1. Identify the agent loop structure (what continues the conversation)
2. Identify tool definitions and how they're registered
3. Identify the model provider and how it's called
4. Document what can be determined, flag the rest as GAP

---

## T6 — OUTPUT VIOLATIONS

### Symptom: Violation log entry generated (OUTPUT_RULES violation)
**Resolution steps**:
1. Read the specific violation from `re-docs/VIOLATION_LOG.md`
2. Check which OUTPUT_RULE was violated
3. Fix the output immediately
4. Update the violation log with resolution status
5. If the rule cannot be satisfied: escalate to framework design note

### Symptom: Accuracy tier cannot be determined
**Resolution steps**:
1. If you have direct file:line evidence: use tier A
2. If you have strong indirect evidence (type signatures, imports, callers): use tier B
3. If you have weak indirect evidence (file naming, folder location, variable naming): use tier C
4. If you have no evidence: use tier D
5. Never omit the accuracy tier

---

## T7 — CROSS-REFERENCE MISMATCHES

### Symptom: Two phase outputs contradict each other
**Resolution steps**:
1. Identify the contradiction with exact file:line references from both phases
2. Re-read the relevant source code to determine which version is correct
3. Update the incorrect phase output
4. Document the correction in engineering notes
5. If both interpretations are plausible: note the ambiguity and use tier B

### Symptom: Cross-reference points to non-existent file
**Resolution steps**:
1. Update the cross-reference to the correct file path
2. If the file was moved/renamed: trace to new location via git history
3. If the file was deleted: update to reflect current state

---

## T8 — PHASE EXECUTION FAILURES

### Symptom: Phase validation fails (cannot proceed to next phase)
**Resolution steps**:
1. Read the validation section at the end of the current phase prompt
2. Identify which validation step failed
3. Fix the output to satisfy the validation
4. Re-run the validation
5. Only proceed to the next phase when validation passes

### Symptom: A phase produces no meaningful output
**Common causes**: Phase not applicable to the repository, empty source directory, feature not present

**Resolution steps**:
1. Confirm the phase truly does not apply (e.g., Phase 16 AI Workflows for a non-AI project)
2. Generate a minimal output with header: "NOT APPLICABLE: [reason]"
3. Log the reason in engineering notes
4. Set quality score to N/A for that phase

---

## T9 — TOOL/ENVIRONMENT FAILURES

### Symptom: Required tools not available (git, compilers, etc.)
**Resolution steps**:
1. Check if equivalent information can be extracted from source files directly
2. For git history: use git log if available, otherwise skip
3. For compilers: parse config files instead
4. Log the limitation

### Symptom: Network access required but unavailable
**Resolution steps**:
1. Skip external dependency license lookups
2. Skip online API documentation lookups
3. Document as network-limited analysis
4. Use only information available locally

---

## ERROR LOGGING PROTOCOL

When any error occurs during the reverse engineering process:

```markdown
### ERR-###: [Error Description]
- Phase: [Phase Number]
- Step: [Step where error occurred]
- File: `relative/path/to/file:line`
- Error Type: [Parsing | Resolution | Ambiguity | Tool | Other]
- Description: [detailed description]
- Resolution: [how the error was resolved or worked around]
- Status: [Resolved | Unresolved | Mitigated]
```

Errors are logged in `re-docs/ERROR_LOG.md`.

---

*When in doubt, log it. An unresolved error with documentation is better than an ignored error with no trace.*
