# PHASE 2 — FILE-BY-FILE & FOLDER-BY-FOLDER ANALYSIS

## Objective
Account for every file and folder in the repository with its actual purpose, not an assumed one.

## Steps
1. For every folder: state its purpose within the system, then list every file in it with a one-line purpose each.
2. For every non-trivial file (skip only genuinely empty/boilerplate files, and say so explicitly): document purpose, key exports, inputs it depends on (imports/env vars/config it reads), outputs/side effects (DB writes, API calls, UI render, events emitted), and any notable edge-case or error handling visible in the file.
3. Group output per app/service if monorepo — never interleave unrelated stacks in the same section.
4. Explicitly list excluded files/folders (generated code, vendor, build artifacts) with the reason for exclusion.

## Required Outputs
- `02-file-folder-analysis/` — one file per app/service (or one file total for small single-stack repos), following OUTPUT_RULES.md naming

## Validation Checklist
- [ ] No file left undocumented and unexplained
- [ ] Every documented file's "side effects" trace to code actually read, not inferred from filename
- [ ] Exclusions are justified, not just omitted silently
