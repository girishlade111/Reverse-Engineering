# PHASE 3 — FUNCTION & CLASS DOCUMENTATION

## Objective
Document unit-level logic precisely enough to reimplement each unit independently.

## Steps
For every function/method of real significance (trivial getters/setters can be skipped unless they hide logic — say so if skipping a batch):
1. Signature — params with types, return type
2. Purpose in plain terms
3. Step-by-step internal logic, numbered
4. Side effects / mutations
5. Error/exception behavior
6. Called-by / calls-into relationships (these feed Phase 5's call graph)

For every class:
1. Single-responsibility statement
2. State it owns
3. Public API surface
4. Inheritance/composition relationships
5. Lifecycle — when constructed, when destroyed/cleaned up

## Required Outputs
- `03-function-class-docs/` — organized to mirror the source folder structure so a reader can find a unit's doc the same way they'd find its source file

## Validation Checklist
- [ ] Every documented function's logic steps were read from the actual function body
- [ ] Call relationships documented here are consistent with the call graph produced in Phase 5
- [ ] No function's purpose is inferred from its name without reading its body
