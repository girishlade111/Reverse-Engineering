# PROMPT_03.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_03: Folder & File System Cartography

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_03
- **Phase:** 1
- **Stage:** 3 of 10
- **Dependencies:** ART-01 (PROMPT_01).
- **Estimated Tokens:** 12000–18000
- **Output Artifacts:** ART-03 (Map) — Folder & File Tree with Responsibilities.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Folder & File Tree map (ART-03) that enumerates every in-scope file and folder, assigns each file a one-line responsibility statement derived from its name, location, header, and exported symbols, classifies each file by role, detects naming conventions, and flags generated or convention-violating files.

---

## 3. When to Invoke

PROMPT_03 is dispatched when ALL of the following predicates hold:

- PROMPT_01 has emitted a Completion Record with `status: SUCCESS` or `status: PARTIAL` whose partial gaps do not include the in-scope path set.
- ART-01 exists and `resolved_scope.coverage_fraction ≥ 0.99`.
- ART-01 `topology` is populated.

PROMPT_03 MAY be dispatched in parallel with PROMPT_02 if the orchestrator authorizes a parallel batch under R12.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | Resolve the in-scope path set; classify paths by ART-01's exclusion rationale. |
| `OPERATING_RULES.md` | Framework file | Bind R13 (read-only), R16 (binary/generated handling), R17 (citation format), R21 (no invention). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, and citation conventions. |
| `QUALITY_STANDARDS.md` | Framework file | Apply Map schema (`§ 4.3`) and type-specific bar (aggregate ≥ 30, Coverage ≥ 4, Precision ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Enumerate the in-scope tree per § 6.1 to a depth bounded by the in-scope set; produce a node list of folders (`D-XX`) and files (`F-XX`).
3. For every file, apply the role classification per § 6.2.
4. For every file, derive a one-line responsibility statement per § 6.3.
5. For every folder, derive a one-line purpose statement per § 6.4.
6. Detect naming conventions per § 6.5; record the convention set and any violating files.
7. Detect generated files per § 6.6; flag them with `generated: true` and a generator hint.
8. Detect barrel files (`index.ts`, `index.js`, `__init__.py`, `mod.rs`, `lib.rs`) per § 6.7.
9. Detect documentation files (`README.md`, `CHANGELOG.md`, `CONTRIBUTING.md`, `LICENSE`, `*.rst`, `docs/**/*.md`) and mark them `kind: doc`.
10. Detect asset files (images, fonts, audio, video) and mark them `kind: asset` with `UNANALYZABLE_BINARY` where appropriate.
11. Emit ART-03 per § 8 with full front-matter, tree visualization, classification tables, traceability index, open questions.
12. Run the Quality Checks in § 9.
13. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Tree Enumeration

Walk the in-scope path set from ART-01's items list. Produce a node for every directory and every regular file. Symbolic links are recorded with `kind: symlink` and the link target; the agent does not follow symlinks (prevents infinite recursion and respects ART-01's boundary). Empty directories are recorded with `kind: empty-folder` because they may carry semantic intent (e.g., placeholder for runtime-generated content).

Each node records:

- `id` — `F-XX` for files, `D-XX` for folders, zero-padded six-digit sequence assigned in lexicographic path order for deterministic re-runs.
- `path` — repository-relative path.
- `parent_id` — the `D-XX` of the containing folder, or `null` for the repository root.
- `kind` — `file | folder | symlink | empty-folder`.
- `depth` — distance from the repository root (root = 0).
- `byte_size` (files only) — file size in bytes.
- `line_count` (text files only) — number of LF-terminated lines.
- `extension` (files only) — lowercase extension without the dot, or `none`.

### 6.2 File Role Classification

Classify each file into exactly one role. The classification is deterministic and based on path and content heuristics.

| Role | Detection Rule |
|------|----------------|
| `source` | Extension matches a source language (per ART-01's primary languages) AND content not generated (per § 6.6). |
| `config` | Name matches `*.config.*`, `.*rc`, `*.yml`/`*.yaml` under root or `config/`, `.env*`, `tsconfig.json`, `jsconfig.json`, `pyproject.toml` (tooling sections), `setup.cfg`, `.editorconfig`, `.prettierrc*`, `.eslintrc*`. |
| `test` | Path contains `test/`, `tests/`, `__tests__/`, `spec/`, OR name matches `*.test.*`, `*.spec.*`, `*_test.go`, `test_*.py`, `*-spec.js`, `*Test.java`, `*Tests.java`, `*Tests.cs`, `*_spec.rb`. |
| `doc` | Extension `.md`, `.rst`, `.adoc`, `.txt` (under `docs/` or named `README`, `CHANGELOG`, `CONTRIBUTING`, `LICENSE`, `AUTHORS`, `CODE_OF_CONDUCT`). |
| `asset` | Extension `.png`, `.jpg`, `.jpeg`, `.gif`, `.svg`, `.webp`, `.ico`, `.bmp`, `.pdf`, `.mp3`, `.mp4`, `.wav`, `.ogg`, `.webm`, `.ttf`, `.otf`, `.woff`, `.woff2`, `.eot`. |
| `generated` | Per § 6.6. |
| `build-script` | Name `Makefile`, `makefile`, `GNUmakefile`, `justfile`, `Justfile`, `CMakeLists.txt`, `Brewfile`, `Vagrantfile`, `Rakefile`, `Jakefile`; or path under `scripts/` with executable bit set and a shebang. |
| `migration` | Path under `migrations/`, `db/migrate/`, `flyway/`, `liquibase/`; or name matches `V*__*.sql` (Flyway) or `*.up.sql` + `*.down.sql` (golang-migrate). |
| `i18n` | Extension `.json`/`.yaml`/`.po`/`.pot`/`.mo` under `locales/`, `i18n/`, `translations/`, `lang/`. |

Each file's role is recorded under `attributes.role`. Files that match multiple rules are assigned by precedence (in table order, top wins). The precedence order is intentional: a generated test file is `generated`, not `test`, because its source is the generator.

### 6.3 File Responsibility Inference

For every `source` file, derive a one-line responsibility statement (≤ 120 characters) using the following procedure:

1. Read the file's first comment block (shebang and language-specific header comment) per § 6.3.1; IF a descriptive comment exists, extract the first descriptive sentence.
2. ELSE IF the file is a module manifest (e.g., `package.json`, `Cargo.toml`), read the `description` field.
3. ELSE IF the file has a single top-level export (class, function, or constant), the responsibility is `<export_kind> <export_name>` (e.g., "Function `dispatchRequest()` — routes HTTP requests by method").
4. ELSE IF the file has multiple top-level exports, the responsibility is "Exports <N> symbols: <first-three>, ..." with the count and first three exported names.
5. ELSE IF the file is a barrel (per § 6.7), the responsibility is "Barrel file re-exporting <N> modules from <dir>."
6. ELSE the responsibility is "Module with <line_count> lines; no top-level exports detected" with an `UNVERIFIED` annotation.

The responsibility statement MUST cite the source line that evidences it (the comment line, the export statement, or the manifest field). Responsibility statements that paraphrase beyond the evidence are forbidden per R21.

#### 6.3.1 Header Comment Detection

- JavaScript/TypeScript — first `/* ... */` block or first run of `//` lines starting at line 1.
- Python — module docstring (first `"""..."""` after the optional shebang and `# -*- coding -*-` line).
- Go — first `//` comment block immediately preceding the `package` clause.
- Rust — first `//` or `//!` (inner doc) comment block at file top.
- Java/Kotlin — first `/** ... */` Javadoc/KDoc block.
- Ruby — first `=begin ... =end` block or a `#` comment run at file top.
- PHP — first `/* ... */` block after `<?php`.

### 6.4 Folder Purpose Inference

For every folder, derive a one-line purpose statement. Procedure:

1. IF the folder name matches a convention name (per § 6.5.1), the purpose is the convention's semantic (e.g., `src/` → "Primary source tree"; `tests/` → "Test suite"; `docs/` → "Documentation").
2. ELSE IF the folder contains a `README.md`, the purpose is the first sentence of that README.
3. ELSE IF the folder is dominated (>70%) by a single file role, the purpose reflects that role (e.g., a folder >70% `test` files → "Test suite for <parent>").
4. ELSE the purpose is "Folder containing <N> files of roles <role-list>."

### 6.5 Naming Convention Detection

#### 6.5.1 Convention Folder Names

Detect standard folder-name conventions and record their presence:

| Folder Name | Semantic |
|-------------|----------|
| `src/`, `lib/`, `app/` | Primary source tree |
| `tests/`, `test/`, `__tests__/`, `spec/` | Test suite |
| `docs/`, `doc/` | Documentation |
| `examples/`, `examples/` | Usage examples |
| `scripts/` | Build/utility scripts |
| `config/`, `configs/`, `conf/` | Configuration |
| `migrations/`, `db/` | Database migrations |
| `public/`, `static/`, `assets/` | Static assets served verbatim |
| `dist/`, `build/`, `out/` | Build output (should be excluded per EXC-04 but recorded if in-scope) |
| `vendor/`, `third_party/` | Vendored dependencies (should be excluded per EXC-05) |
| `locales/`, `i18n/`, `lang/` | Internationalization catalogs |
| `cmd/` (Go) | CLI entry points |
| `internal/` (Go) | Private packages |
| `pkg/` (Go) | Public packages |
| `api/` | API definitions |
| `services/` | Service components |
| `components/`, `ui/` | UI components |
| `hooks/`, `composables/` | UI hooks |
| `pages/`, `routes/`, `views/` | Page-level UI |
| `utils/`, `helpers/`, `common/` | Shared utilities |
| `types/` | Type definitions |
| `constants/` | Constants |
| `models/` | Data models |
| `controllers/` | Controllers |
| `middleware/` | Middleware |

#### 6.5.2 File Naming Conventions

Detect the dominant naming convention per file type by sampling filenames and matching against:

- `kebab-case` — `^[a-z0-9]+(-[a-z0-9]+)+\.[a-z]+$`
- `camelCase` — `^[a-z][a-zA-Z0-9]*\.[a-z]+$`
- `PascalCase` — `^[A-Z][a-zA-Z0-9]*\.[a-z]+$`
- `snake_case` — `^[a-z0-9]+(_[a-z0-9]+)+\.[a-z]+$`
- `SCREAMING_SNAKE_CASE` — `^[A-Z0-9]+(_[A-Z0-9]+)+\.[a-z]+$`

Record the dominant convention per directory and per file type. Files that violate the dominant convention are flagged with `convention_violation: true` and an annotation of the expected convention.

### 6.6 Generated File Detection

Detect generated files by content markers and path patterns. A file is `generated` if ANY of:

- The file begins with a comment matching `/^.*generated.*do not edit/i` (language-normalized: `//`, `#`, `/*`, `<!--`).
- The file path matches `**/dist/**`, `**/build/**`, `**/.next/**`, `**/out/**`, `**/target/**` (these should already be excluded per EXC-04 but are flagged if in-scope).
- The file path matches `**/*.min.js`, `**/*.min.css`, `**/*.map`.
- The file is `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`, `poetry.lock`, `Cargo.lock`, `go.sum`, `Gemfile.lock`, `composer.lock` (lockfiles are generated; downstream prompts read them but do not document their internal structure).
- The file is `*.g.dart` (Dart codegen), `*.pb.go` (protobuf Go), `*.pb.cc`/`*.pb.h` (protobuf C++), `*_generated.go` (Go codegen), `*_gen.py` (Python codegen), `*Generated.java` (Java codegen), `*.designer.cs` (WinForms designer), `*.graphql.ts` (GraphQL codegen).
- The file path matches `**/__generated__/**`.

Generated files are marked `generated: true` with a `generator_hint` naming the likely generator (e.g., `protoc`, `openapi-generator`, `graphql-codegen`, `build_runner`, `tsc`). The agent MUST NOT document the internals of generated files; downstream prompts trace to the source generator per R16.

### 6.7 Barrel File Detection

Detect barrel files (re-export aggregators):

- `index.ts`, `index.js`, `index.mjs`, `index.cjs` whose content is predominantly `export ... from` or `module.exports = require(...)` statements.
- `__init__.py` whose content is predominantly `from .x import y` statements or empty.
- `mod.rs` whose content is predominantly `pub use ...` or `pub mod ...` statements.
- `lib.rs` whose content is predominantly `pub mod ...` statements.
- `index.vue` in some Vue design systems.

Barrel files are marked `is_barrel: true` with the count of re-exported modules. PROMPT_06 uses barrel detection to drive module boundary inference.

---

## 7. Required Outputs

### ART-03 — Folder & File Tree Map

**Type:** Map.

**Acceptance Criteria:**

- AC-03.1: The artifact file exists at `<output_root>/artifacts/phase1/ART03_<engagement_id>_folder-tree.md`.
- AC-03.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.3 (Map schema).
- AC-03.3: The body contains: Executive Summary, Methodology, Tree Visualization, Folder Purposes, File Responsibilities, Role Distribution, Naming Conventions, Generated Files, Barrel Files, Convention Violations, Traceability Index, Open Questions, Cross-References.
- AC-03.4: Every in-scope file appears as a `F-XX` node with `role`, `responsibility`, and citation.
- AC-03.5: Every in-scope folder appears as a `D-XX` node with `purpose` and citation.
- AC-03.6: Every generated file has `generated: true` and a `generator_hint`.
- AC-03.7: Every barrel file has `is_barrel: true` and a re-export count.
- AC-03.8: Convention violations list the violating file, the expected convention, and the actual convention.
- AC-03.9: Coverage ≥ 99% of in-scope files have a non-empty `responsibility`.

---

## 8. Output Templates

### 8.1 ART-03 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-03
artifact_type: Map
producing_prompt: PROMPT_03
phase: 1
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
tree_stats:
  file_count: <int>
  folder_count: <int>
  max_depth: <int>
  role_distribution:
    source: <int>
    config: <int>
    test: <int>
    doc: <int>
    asset: <int>
    generated: <int>
    build-script: <int>
    migration: <int>
    i18n: <int>
naming_conventions:
  - scope: directory | file-type
    target: <path-or-ext>
    dominant: kebab-case | camelCase | PascalCase | snake_case | SCREAMING_SNAKE_CASE | mixed
    violation_count: <int>
source_coverage:
  - path: <file_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <file_path>:<line-range>
    symbol: <name>
nodes:
  - id: F-000001
    label: <relative-path>
    kind: file
    parent_id: D-000001
    depth: <int>
    role: source | config | test | doc | asset | generated | build-script | migration | i18n
    responsibility: <one-line>
    responsibility_source: <file_path>:<line-range>
    extension: <ext>
    byte_size: <int>
    line_count: <int>
    is_barrel: true | false
    barrel_reexport_count: <int> | null
    generated: true | false
    generator_hint: <name> | null
    convention_violation: true | false
    expected_convention: <name> | null
    actual_convention: <name> | null
  - id: D-000001
    label: <relative-path>
    kind: folder
    parent_id: D-000000 | null
    depth: <int>
    purpose: <one-line>
    purpose_source: <file_path>:<line-range>
    convention_folder_name: <name> | null
edges:
  - from: D-000001
    to: F-000001
    relationship: CONTAINS
    evidence: <file_path>:<line>
---
```

### 8.2 ART-03 Body Skeleton

```markdown
# ART-03: Folder & File Tree Map

## 1. Executive Summary
## 2. Methodology
## 3. Tree Visualization
   ### 3.1 Top-Level Structure
   ### 3.2 Subtree Detail (per top-level folder)
## 4. Folder Purposes
## 5. File Responsibilities
## 6. Role Distribution
## 7. Naming Conventions
## 8. Generated Files
## 9. Barrel Files
## 10. Convention Violations
## 11. Traceability Index
## 12. Open Questions
## 13. Cross-References
```

The Tree Visualization section uses an ASCII tree representation followed by Mermaid `graph TD` diagrams per subtree (≤ 30 nodes each) for navigation. Subtrees larger than 30 nodes are decomposed.

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — `coverage_fraction ≥ 0.99` of in-scope files have a non-empty `responsibility`.
- **Q2. Citation Check** — every `responsibility` and `purpose` cites its source line; ≥ 0.95 cited.
- **Q3. Schema Conformance Check** — validates against § 4.3.
- **Q4. Non-Contradiction Check** — no role assignment contradicts ART-01's exclusion rationale (e.g., an excluded-by-EXC-04 path is not classified as `source` in-scope).
- **Q5. UNVERIFIED Accounting** — every `responsibility` marked `UNVERIFIED` has an Open Question.
- **Q6. Idempotence Spot-Check** — re-classifying a 5% sample of files yields the same role and responsibility.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-03.A. Generated File Coverage** — every file matching a § 6.6 marker is flagged `generated: true`.
- **Q-03.B. Barrel Detection** — every `index.ts`/`__init__.py`/`mod.rs`/`lib.rs` is examined for barrel content; non-barrel instances are marked `is_barrel: false` with rationale.
- **Q-03.C. Convention Detection** — at least one naming convention entry exists per non-empty directory.
- **Q-03.D. Folder Purpose Source** — every folder's `purpose_source` resolves to an existing file or to the folder's own path with `symbol: N/A`.
- **Q-03.E. Migration Detection** — every `.sql` file under a `migrations/` directory is classified `migration`, not `source`.

---

## 10. Common Pitfalls

- Do not infer a file's responsibility from its filename alone; always read the header comment or export statement per § 6.3. Filename inference is the last resort, not the first.
- Do not classify a file as `source` based on extension if it is generated; check § 6.6 markers first.
- Always cite the line that evidences a responsibility statement; an uncited responsibility is non-conformant per R17.
- Do not collapse barrel files into a single "re-export" responsibility; record the count and the directory the barrel aggregates.
- Do not skip empty directories; they may carry semantic intent (placeholder for runtime output, contract for plugin discovery).
- Always normalize extensions to lowercase for classification; `.TS` and `.ts` are the same language.
- Do not flag a file as a convention violation when its naming differs for a documented reason (e.g., Go test files are `*_test.go` by language mandate, not a `kebab-case` violation).
- Do not document the internals of generated files; record `generated: true` and the generator hint, then defer to the source generator per R16.
- Always walk the tree deterministically (lexicographic order) so that re-runs produce identical node IDs and the idempotence spot-check passes.
- Do not classify a symlink's target as in-scope; symlinks are recorded as `kind: symlink` and the target is analyzed only if it is independently in-scope per ART-01.

---

## 11. Handoff Criteria

PROMPT_06, PROMPT_07, PROMPT_08, PROMPT_09, and PROMPT_28 consume ART-03. Handoff requires ALL of:

- HC-03.1: ART-03 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-03.2: `coverage_fraction ≥ 0.99`.
- HC-03.3: Every in-scope file has a `role`, a `responsibility`, and a citation.
- HC-03.4: Every in-scope folder has a `purpose` and a citation.
- HC-03.5: Generated files are flagged; lockfiles and build outputs are not classified as `source`.
- HC-03.6: Barrel files are flagged with re-export counts.
- HC-03.7: `repository_fingerprint_recheck` matches ART-01.
- HC-03.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_06 (Module Architecture — uses file roles and barrel detection to infer module boundaries), PROMPT_07 (Component Architecture — uses `components/`, `ui/`, `pages/` folders to seed component detection), PROMPT_08 (Class & Interface — uses `source` files as the analysis set), PROMPT_09 (Function-Level — uses `source` and `build-script` files as the analysis set), PROMPT_28 (Cross-Reference Checklists — uses ART-03 as the file coverage ground truth).
- **Depends on:** ART-01 (PROMPT_01).
- **Governing rules:** `OPERATING_RULES.md` R13, R16, R17, R21.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.3; Map bar (aggregate ≥ 30, Coverage ≥ 4, Precision ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 3.1, § 4, § 6, § 7 (Mermaid diagrams per subtree).
- **Forward reference:** PROMPT_30 will spot-check that every `source` file in ART-03 appears in at least one downstream artifact (ART-06, ART-08, or ART-09).

*End of PROMPT_03. Orchestrator may dispatch PROMPT_04 upon satisfaction of § 11.*
