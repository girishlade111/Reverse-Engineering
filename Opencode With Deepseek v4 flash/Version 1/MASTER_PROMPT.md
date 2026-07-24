# MASTER PROMPT — Enterprise Reverse Engineering Framework

## PROMPT CLASS: Orchestration / Entry Point
## TARGET: AI Coding Agent (Claude, GPT, Gemini, Codex, etc.)
## EXECUTION MODE: Sequential multi-phase
## FRAMEWORK VERSION: 1.0

---

## ACTIVATION

You are now operating inside the **Enterprise Reverse Engineering Prompt Framework**.

Your mission is to completely reverse engineer the target software repository.

You must follow every instruction in this MASTER PROMPT and every file it references.

---

## IMMEDIATE ACTIONS

1. Read `MISSION.md` — internalize the mission.
2. Read `OPERATING_RULES.md` — internalize all operating rules.
3. Read `QUALITY_STANDARDS.md` — internalize all quality standards.
4. Read `OUTPUT_RULES.md` — internalize all output formatting rules.
5. Read `PROJECT_SPECIFICATION.md` — understand the framework itself.
6. Read `PROMPT_DESIGN_GUIDE.md` — understand why this framework is designed this way.

After reading all six files above, confirm you have internalized them before proceeding.

---

## TARGET REPOSITORY

The target repository is: **[PROVIDE REPO PATH / URL]**

The agent must first clone or access the repository and perform an initial survey before starting any phase.

---

## EXECUTION WORKFLOW

### Phase Sequence (Strict Order)

Execute the following phases in order. Do not skip. Do not reorder.

| ORDER | PROMPT FILE | PHASE NAME | MINIMUM EVIDENCE TO PROCEED |
|-------|-------------|------------|------------------------------|
| 1 | PROMPT_01_SCOUTING.md | Project Scouting | Repository cloned, language identified, top-level structure captured |
| 2 | PROMPT_02_STRUCTURE.md | Structure Analysis | Full folder tree with file counts, naming conventions documented |
| 3 | PROMPT_03_BUILD_CONFIG.md | Build & Config | Build files parsed, config files documented, package manager identified |
| 4 | PROMPT_04_DEPENDENCIES.md | Dependency Graph | All dependencies listed with versions, dependency tree documented |
| 5 | PROMPT_05_TECH_STACK.md | Tech Stack | Complete tech stack documented with versions and roles |
| 6 | PROMPT_06_MODULES.md | Module Analysis | Each module's responsibility and interface documented |
| 7 | PROMPT_07_DEEP_READ.md | Deep Code Reading | Every significant file analyzed class-by-class, function-by-function |
| 8 | PROMPT_08_ARCHITECTURE.md | Architecture Reconstruction | Architecture diagrams, layer documentation, ADRs reconstructed |
| 9 | PROMPT_09_DATA_FLOW.md | Data Flow Analysis | Data pipelines, transformations, flow diagrams documented |
| 10 | PROMPT_10_CALL_GRAPH.md | Call Graph & Control Flow | Call graphs generated, execution paths traced |
| 11 | PROMPT_11_FEATURES.md | Feature Mapping | Complete feature inventory with boundaries and interactions |
| 12 | PROMPT_12_ALGORITHMS.md | Algorithm Extraction | Core algorithms extracted with pseudocode and complexity analysis |
| 13 | PROMPT_13_DESIGN_PATTERNS.md | Design Patterns | All design patterns identified with code evidence |
| 14 | PROMPT_14_API_BOUNDARIES.md | API & Service Boundaries | All APIs documented with contracts, endpoints, middleware |
| 15 | PROMPT_15_STATE_EVENTS.md | State & Events | State machines and event systems fully documented |
| 16 | PROMPT_16_ERROR_CACHE_RETRY.md | Error Handling & Reliability | Error strategies, retry logic, caching documented |
| 17 | PROMPT_17_AI_WORKFLOWS.md | AI Workflow Analysis | Prompts, agents, RAG, LLM integration documented (if applicable) |
| 18 | PROMPT_18_CONFIG_ENV.md | Configuration & Environment | All config, env vars, secrets, feature flags documented |
| 19 | PROMPT_19_DOCUMENTATION.md | Documentation Generation | Final documentation assembled, handbooks created |
| 20 | PROMPT_20_VALIDATION.md | Cross-Reference & Validation | Complete validation audit performed |

### Per-Phase Workflow

Within each phase, follow this exact workflow:

```
1. LOAD   → Read the phase prompt file
2. EXECUTE → Perform the analysis described in the prompt
3. OUTPUT  → Write structured output to re-docs/<phase-folder>/
4. VALIDATE → Run the phase's self-validation checks
5. CROSS-CHECK → Verify against previous phase outputs for consistency
6. COMMIT  → Save all outputs before advancing
```

### Validation Gates

Each phase prompt contains validation checks. Run these checks before advancing.

If validation fails, do not proceed. Identify the gap, fix it, re-validate.

---

## OUTPUT ARCHITECTURE

### Directory Structure

All outputs must be written to a `re-docs/` directory at the root of the target repository.

```
re-docs/
├── 00-scouting/           → PROMPT_01 outputs
├── 01-structure/          → PROMPT_02 outputs
├── 02-build-config/       → PROMPT_03 outputs
├── 03-dependencies/       → PROMPT_04 outputs
├── 04-tech-stack/         → PROMPT_05 outputs
├── 05-modules/            → PROMPT_06 outputs
├── 06-deep-read/          → PROMPT_07 outputs
├── 07-architecture/       → PROMPT_08 outputs
├── 08-data-flow/          → PROMPT_09 outputs
├── 09-call-graph/         → PROMPT_10 outputs
├── 10-features/           → PROMPT_11 outputs
├── 11-algorithms/         → PROMPT_12 outputs
├── 12-design-patterns/    → PROMPT_13 outputs
├── 13-api-boundaries/     → PROMPT_14 outputs
├── 14-state-events/       → PROMPT_15 outputs
├── 15-error-cache-retry/  → PROMPT_16 outputs
├── 16-ai-workflows/       → PROMPT_17 outputs
├── 17-config-env/         → PROMPT_18 outputs
├── 18-documentation/      → PROMPT_19 outputs
└── 19-validation/         → PROMPT_20 outputs
```

### Naming Convention

- Phase outputs: `<phase-number>-<phase-name>.md`
- Diagrams: `<type>-<subject>.md` (stored in diagrams/ subfolder)
- Evidence files: `<file-path-sanitized>.evidence.md`

---

## CROSS-PHASE INTEGRITY RULES

1. **No contradiction**: Output in phase N must not contradict output in phase N-1. If contradiction is found, flag it and resolve.
2. **Forward references**: When phase N references something not yet analyzed, flag as `[FORWARD-REF: phase M, subject]`.
3. **Evidence chain**: Every architectural claim must be traceable to file:line evidence.
4. **Progressive enrichment**: Phase N may enrich phase N-1 outputs with deeper analysis, but must never invalidate them.
5. **Consistent naming**: Use consistent names for modules, components, services across all phases.
6. **Dependency tracking**: If analysis reveals a dependency on an unanalyzed file, log it in the dependency tracker.

---

## ERROR RECOVERY

If the agent encounters an error during any phase:

1. Log the error in `re-docs/ERROR_LOG.md` with:
   - Phase number
   - File/operation that failed
   - Error description
   - Impact on downstream phases
2. Attempt recovery: retry, alternative approach, or partial analysis.
3. If recovery impossible, flag the gap and continue. Document the gap.
4. All gaps are collected during PROMPT_20 and reported in the validation report.

---

## COMPLETION CRITERIA

The framework is considered complete when:

- [ ] All 20 phases have been executed
- [ ] All phase outputs are saved to `re-docs/`
- [ ] All validation gates have passed
- [ ] No contradictions remain between phases
- [ ] All forward references have been resolved
- [ ] Complete documentation has been generated in `re-docs/18-documentation/`
- [ ] Validation report exists in `re-docs/19-validation/`
- [ ] Summary file `REVERSE_ENGINEERING_SUMMARY.md` exists

---

## START COMMAND

Begin by reading MISSION.md and the other core framework files as instructed in IMMEDIATE ACTIONS.

Then load PROMPT_01_SCOUTING.md and begin Phase 00.
