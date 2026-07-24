Repository Analysis:



Repository Analysis:

* Structural Scan (PROMPT\_01)
* Dependency Mapping (PROMPT\_03)
* Architecture Discovery (PROMPT\_02)
* Code Structure Analysis (PROMPT\_04)



Execution Flow Strategy:

* Entry Point Identification
* Control Flow Mapping
* State Transition Analysis
* Data Flow Analysis



\---



\## ⚙️ \*\*OPERATING\_RULES.md\*\*



```markdown

\# OPERATING RULES: REVERSE ENGINEERING PROMPT FRAMEWORK



\## 1. CORE OPERATING PRINCIPLES



\### PRINCIPLE 1: SOURCE CODE IS TRUTH

\*\*Rule:\*\* All analysis must be based on source code. No assumptions from documentation, comments, or external sources can be trusted without verification.

\*\*Exception:\*\* None. This is absolute.



\### PRINCIPLE 2: COMPLETE BEFORE DOCUMENT

\*\*Rule:\*\* Complete understanding of the entire system is mandatory before any documentation can be written.

\*\*Verification:\*\* Use PROMPT\_10\_VALIDATION\_VERIFICATION.md



\### PRINCIPLE 3: VERIFY EVERY FACT

\*\*Rule:\*\* Every statement in documentation must be verifiable from source code.

\*\*Method:\*\* Include file paths, line numbers, and code references.



\### PRINCIPLE 4: NO OMISIONS

\*\*Rule:\*\* No component, relationship, or behavior can be omitted.



\### PRINCIPLE 5: PRESERVE COMPLEXITY

\*\*Rule:\*\* Complex logic must be documented as-is, not simplified.



\## 2. EXECUTION RULES



\*\*RULE 1: STRUCTURE BEFORE BEHAVIOR\*\*

Always analyze structure (files, folders, modules) before behavior.



\*\*RULE 2: DEPENDENCIES BEFORE IMPLEMENTATION\*\*

Always map dependencies before analyzing implementation.



\*\*RULE 3: ENTRY POINTS BEFORE INTERNAL LOGIC\*\*

Always identify and trace entry points before internal functions.



\*\*RULE 4: TOP-DOWN THEN BOTTOM-UP\*\*

Analyze high-level to low-level, then verify low-level to high-level.



\## 3. DOCUMENTATION RULES



\*\*RULE 5: DOCUMENT IN CONTEXT\*\*

Document each component in context of its relationships.



\*\*RULE 6: ALWAYS INCLUDE EXAMPLES\*\*

Include real code examples for all non-trivial explanations.



\*\*RULE 7: USE DIAGRAMS LIBERALLY\*\*

Use Mermaid diagrams for all relationships, flows, and structures.



\*\*RULE 8: CROSS-REFERENCE EVERYTHING\*\*

Every component must link to related components.



\*\*RULE 9: VERSION ALL DOCUMENTATION\*\*

Include version and timestamp metadata.



\## 4. TECHNICAL RULES



\*\*RULE 10: PARSE DON'T GUESS\*\*

Use AST parsing for code analysis.



\*\*RULE 11: FOLLOW ALL IMPORTS\*\*

Every import/require must be followed and analyzed.



\*\*RULE 12: TRACE ALL CALLS\*\*

Every function call must be traced to its implementation.



\*\*RULE 13: IDENTIFY ALL PATTERNS\*\*

Actively search for and document all patterns.



\## 5. VALIDATION RULES



\*\*RULE 14: VALIDATE BEFORE FINALIZING\*\*

Run PROMPT\_10 before considering documentation complete.



\*\*RULE 15: PEER REVIEW REQUIRED\*\*

All critical documentation must be reviewed.



\*\*RULE 16: CONTINUOUS IMPROVEMENT\*\*

Update documentation when code changes.



\## 6. ERROR HANDLING RULES



\*\*RULE 17: HANDLE SYNTAX ERRORS\*\*

Document error and continue with valid code.



\*\*RULE 18: HANDLE MISSING FILES\*\*

Document missing dependency and continue.



\*\*RULE 19: HANDLE CIRCULAR DEPENDENCIES\*\*

Detect and document all circular dependencies.



\*\*RULE 20: MARK UNCERTAIN INFORMATION\*\*

Mark as \[UNCERTAIN] with reason.

