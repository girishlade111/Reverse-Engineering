# PROMPT_01.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_01: Repository Intake & Boundary Definition

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_01
- **Phase:** 1
- **Stage:** 1 of 10
- **Dependencies:** Stage 0 Engagement Manifest (orchestrator-provided); framework files per `MASTER_INDEX.md` § 2.
- **Estimated Tokens:** 9000–14000
- **Output Artifacts:** ART-01 (Manifest) — Repository Boundary Declaration.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Repository Boundary Declaration (ART-01) that enumerates every in-scope path, every excluded path with rationale, the resolved scope modifier, the detected repository topology, the primary language(s), the detected license, and a content fingerprint that subsequent prompts use to detect mutation.

---

## 3. When to Invoke

PROMPT_01 is dispatched when ALL of the following predicates hold:

- The engagement manifest exists at `<output_root>/engagement_manifest.json` and is non-empty.
- The engagement manifest declares a non-empty `subject_path` resolvable from the agent's working directory.
- The engagement manifest declares a `scope_modifier` equal to one of `SCOPE_FULL`, `SCOPE_CORE`, `SCOPE_TRIAGE`, `SCOPE_MODULE`.
- IF `scope_modifier = SCOPE_MODULE`, the manifest declares a non-empty `target_module`.
- The agent has confirmed the pre-flight checklist in `MASTER_PROMPT.md` § 9.
- No prior `PROMPT_01` completion record exists for this `engagement_id` with `status: SUCCESS`.

PROMPT_01 is the first prompt of every engagement; it has no upstream artifact dependency.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| `engagement_manifest.json` | Engagement file | Resolve `subject_path`, `scope_modifier`, `target_module`, `engagement_id`, `authorization`. |
| `MISSION.md` | Framework file | Confirm scope modifier semantics (`MISSION.md` § 6) and ethical boundaries (`MISSION.md` § 5). |
| `OPERATING_RULES.md` | Framework file | Bind R13 (read-only), R15 (fingerprint), R16 (binary/generated exclusion), R34 (license conflict escalation). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact naming, header schema, and directory layout (`OUTPUT_RULES.md` § 2–§ 4). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Manifest schema (`§ 4.2`) and type-specific bar (`§ 5`, aggregate ≥ 30). |

No prior ART-XX artifact is consumed.

---

## 5. Instructions to AI Agent

1. Read `engagement_manifest.json` and extract `subject_path`, `scope_modifier`, `target_module`, `engagement_id`, `authorization`, `output_root`.
2. Verify the `subject_path` is readable; IF not, emit `BLOCKED` with `INPUT_GAP` and halt.
3. Compute a content fingerprint of `subject_path` per § 6.1; record the algorithm name and digest in the working layer.
4. Apply the standard exclusion patterns per § 6.2 to produce a candidate in-scope set and a candidate excluded set.
5. Resolve the scope modifier per § 6.3: IF `SCOPE_MODULE`, restrict the in-scope set to `target_module` and its import closure (best-effort at this stage; refine in PROMPT_06).
6. Enumerate the top-level structure of `subject_path` (depth-1 directory listing plus root-level files).
7. Detect the repository topology per § 6.4 (single-package, monorepo, polyrepo).
8. Detect the primary language(s) per § 6.5 using file-extension frequency and manifest signals.
9. Detect the license per § 6.6; IF a license file exists and its terms prohibit reverse engineering, emit `BLOCKED` with `AUTH_FAIL` per R34.
10. Detect version-control metadata (`.git`, `.hg`, `.svn`) and record the system; do NOT read `.git` object store.
11. For every excluded path, record the matching exclusion rule and a one-line rationale.
12. For every in-scope path, record its relative path, kind (file or folder), byte size, and mtime.
13. Compute the coverage target: the fraction of intended paths that MUST appear in-scope or explicitly excluded.
14. Emit ART-01 per § 8 with full front-matter header, body sections, traceability index, and open questions.
15. Run the Quality Checks in § 9; record pass/fail in the working layer.
16. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

Each instruction is atomic; an agent that cannot complete an instruction MUST emit `BLOCKED` rather than skip it.

---

## 6. Analysis Procedures

### 6.1 Content Fingerprint Computation

Compute a deterministic fingerprint over every in-scope file. The procedure MUST be reproducible and order-independent.

1. Enumerate every regular file under `subject_path` excluding the patterns in § 6.2.
2. For each file, compute SHA-256 of the file's bytes.
3. Concatenate the hex digests in lexicographic order of the relative path, separated by `\n`.
4. Compute SHA-256 of the concatenated string; this is the `repository_fingerprint`.
5. Record `fingerprint_algorithm: "sha-256-over-sha-256-of-lexicographically-sorted-relative-paths"`.
6. Record the file count and total byte count; these are cross-checked at phase boundaries per R15.

The fingerprint is the engagement's mutation-detection anchor. A drift in this value at any subsequent phase boundary triggers `INTEGRITY_FAIL` per R15.

### 6.2 Standard Exclusion Patterns

The following glob patterns are excluded by default. The agent MUST record the matched rule for each excluded path so a reviewer can audit the exclusion.

| Rule ID | Pattern | Rationale |
|---------|---------|-----------|
| EXC-01 | `**/.git/**` | Version-control internals; not source. |
| EXC-02 | `**/.hg/**`, `**/.svn/**` | Legacy VCS internals. |
| EXC-03 | `**/node_modules/**` | Installed dependencies; managed by package manager. |
| EXC-04 | `**/dist/**`, `**/build/**`, `**/.next/**`, `**/.nuxt/**`, `**/out/**`, `**/target/**` | Build outputs; generated, not authored. |
| EXC-05 | `**/vendor/**`, `**/third_party/**` | Vendored dependencies; treated as external. |
| EXC-06 | `**/.cache/**`, `**/.parcel-cache/**`, `**/__pycache__/**`, `**/.mypy_cache/**` | Caches; transient. |
| EXC-07 | `**/coverage/**`, `**/.nyc_output/**` | Coverage artifacts; generated. |
| EXC-08 | `**/*.min.js`, `**/*.min.css`, `**/*.map` | Minified or source-mapped outputs. |
| EXC-09 | `**/.DS_Store`, `**/Thumbs.db` | OS metadata. |
| EXC-10 | `**/.env.local`, `**/.env.*.local` | Local-only secrets; excluded for safety. |

Excluded paths MUST still appear in ART-01's excluded-set listing with their matched rule. Exclusion is not erasure; it is a classification. Files larger than 5 MiB that are not source code (binary blobs, large datasets) MUST be classified `UNANALYZABLE_BINARY` and listed with a size annotation.

### 6.3 Scope Resolution

The `scope_modifier` governs the in-scope set.

- `SCOPE_FULL` — every path not matching an exclusion pattern is in-scope.
- `SCOPE_CORE` — identical to `SCOPE_FULL` for PROMPT_01; downstream prompts MAY skip optional steps.
- `SCOPE_TRIAGE` — restrict to source-tree roots (e.g., `src/`, `lib/`, `app/`, `cmd/`, `internal/`) and the root manifest files. Documentation, examples, and assets are excluded with rationale `TRIAGE_NON_SOURCE`.
- `SCOPE_MODULE(target)` — restrict to the directory or glob identifying `target_module`. IF `target_module` cannot be resolved to an existing path, emit `BLOCKED` with `INPUT_GAP`. Record the module's import closure as a `SCOPE_MODULE_CLOSURE` annotation to be refined by PROMPT_06.

The resolved scope is recorded in ART-01's `resolved_scope` block.

### 6.4 Repository Topology Detection

Detect whether the subject is a single package, a monorepo, or a polyrepo.

- **Monorepo markers** — presence of any of: `pnpm-workspace.yaml`, `lerna.json`, `nx.json`, `turbo.json`, `rush.json`, `package.json` with non-empty `workspaces` field, `Cargo.toml` with `[workspace]` section, `go.work`, `composer.json` with `merge-plugin`, `settings.gradle` with multiple `include` directives, `Pipfile` or `pyproject.toml` with monorepo tooling.
- **Polyrepo markers** — the subject path resolves to a parent directory containing multiple sibling repositories each with their own VCS root. Polyrepo subjects are rare in engagement practice; the agent records the topology but analyzes only the declared subject.
- **Single-package** — none of the above; one root manifest (or none).

For a monorepo, enumerate each workspace package, its manifest path, and its declared name. Record the workspace manager (npm, pnpm, yarn, lerna, nx, turbo, cargo, go-work, pipenv) since downstream prompts use this to drive dependency resolution.

### 6.5 Primary Language Detection

Compute the language distribution by file count and by byte count over the in-scope set. Use the following extension map (non-exhaustive):

| Extension(s) | Language |
|---------------|----------|
| `.ts`, `.tsx` | TypeScript |
| `.js`, `.jsx`, `.mjs`, `.cjs` | JavaScript |
| `.py` | Python |
| `.go` | Go |
| `.rs` | Rust |
| `.java` | Java |
| `.kt`, `.kts` | Kotlin |
| `.swift` | Swift |
| `.rb` | Ruby |
| `.php` | PHP |
| `.cs` | C# |
| `.cpp`, `.cc`, `.cxx`, `.hpp` | C++ |
| `.c`, `.h` | C |
| `.scala` | Scala |
| `.lua` | Lua |
| `.ex`, `.exs` | Elixir |
| `.erl` | Erlang |
| `.clj`, `.cljs`, `.cljc` | Clojure |

Declare the top three languages as `primary_languages`. Record the full distribution in ART-01's `language_distribution` block. Where a manifest declares the primary language explicitly (e.g., `package.json` `"type": "module"`, `pyproject.toml` `[tool.poetry]`), cross-check the declared language against the detected distribution; mismatches are flagged as `OPEN_QUESTION`.

### 6.6 License Detection

Search the repository root for license-bearing files: `LICENSE`, `LICENSE.md`, `LICENSE.txt`, `LICENCE`, `LICENCE.md`, `COPYING`, `COPYING.txt`, `NOTICE`, `UNLICENSE`. For each file found, read its content and classify the license by SPDX identifier when a confident match exists (MIT, Apache-2.0, BSD-2-Clause, BSD-3-Clause, ISC, MPL-2.0, LGPL-2.1, LGPL-3.0, GPL-2.0, GPL-3.0, AGPL-3.0, Unlicense, proprietary). For ambiguous cases, record `license_classification: UNVERIFIED` and emit an Open Question.

IF the detected license is one of `GPL-2.0`, `GPL-3.0`, `AGPL-3.0`, `SSPL`, `BUSL-1.1`, `proprietary`, or `UNVERIFIED`, the agent MUST evaluate the engagement authorization statement for explicit coverage. IF the authorization does not cover copyleft or proprietary analysis, emit `BLOCKED` with `AUTH_FAIL` per R34 and halt.

### 6.7 Version Control Metadata Detection

Detect VCS by presence of `.git`, `.hg`, `.svn`, `.bzr` directories. For git, the agent MAY read `.git/HEAD`, `.git/config`, and `.git/refs` to record the current branch and remote URL; the agent MUST NOT read the object store. Record `vcs_system`, `vcs_branch`, `vcs_remote_url` (when present). The remote URL is the engagement's authoritative subject identity for audit logs.

### 6.8 Coverage Target Computation

The coverage target is the fraction of intended paths that the agent classifies as either in-scope or explicitly-excluded. Define `intended_paths = all paths under subject_path excluding VCS internals`. The coverage fraction is `(classified_paths) / (intended_paths)` where `classified_paths = in_scope ∪ excluded_with_rationale`. The PROMPT_01 handoff target is `coverage_fraction ≥ 0.99`.

---

## 7. Required Outputs

### ART-01 — Repository Boundary Declaration (Manifest)

**Type:** Manifest.

**Acceptance Criteria:**

- AC-01.1: The artifact file exists at `<output_root>/artifacts/phase1/ART01_<engagement_id>_boundary-declaration.md`.
- AC-01.2: The YAML front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.2.
- AC-01.3: The header records `repository_fingerprint`, `fingerprint_algorithm`, `file_count`, `byte_count`.
- AC-01.4: The body contains: Executive Summary, Methodology, Topology, Primary Languages, License, VCS, In-Scope Paths, Excluded Paths, Coverage Statement, Traceability Index, Open Questions, Cross-References.
- AC-01.5: Every excluded path carries a `rule_id` from § 6.2 and a one-line rationale.
- AC-01.6: `coverage_fraction ≥ 0.99` over intended paths.
- AC-01.7: For monorepos, every workspace package is enumerated with manifest path and declared name.
- AC-01.8: All claims carry citations of the form `<file_path>:<line_range>` or, for absence claims, `<file_path>` with `symbol: N/A`.

---

## 8. Output Templates

### 8.1 ART-01 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-01
artifact_type: Manifest
producing_prompt: PROMPT_01
phase: 1
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score:
  coverage: <0..5>
  traceability: <0..5>
  accuracy: <0..5>
  depth: <0..5>
  coherence: <0..5>
  precision: <0..5>
  completeness: <0..5>
  readability: <0..5>
  aggregate: <0..40>
status: DRAFT
repository_fingerprint: <sha-256-hex>
fingerprint_algorithm: "sha-256-over-sha-256-of-lexicographically-sorted-relative-paths"
file_count: <int>
byte_count: <int>
topology: single-package | monorepo | polyrepo
workspace_manager: npm | pnpm | yarn | lerna | nx | turbo | cargo | go-work | pipenv | none
primary_languages:
  - language: <name>
    file_count: <int>
    byte_count: <int>
    fraction_by_file: <0..1>
license:
  spdx_id: <id> | UNVERIFIED
  file: <path> | none
  classification_conflict: true | false
vcs:
  system: git | hg | svn | bzr | none
  branch: <name>
  remote_url: <url> | none
resolved_scope:
  scope_modifier: SCOPE_FULL | SCOPE_CORE | SCOPE_TRIAGE | SCOPE_MODULE
  target_module: <name> | null
  in_scope_count: <int>
  excluded_count: <int>
  coverage_fraction: <0..1>
source_coverage:
  - path: <file_path>
    symbol_count: 0
    line_range: N/A
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <file_path>:<line_range>
    symbol: <name>
items:
  - id: F-000001
    name: <relative-path>
    type: file | folder
    location: <relative-path>
    attributes:
      classification: in-scope | excluded
      exclusion_rule: <rule_id> | null
      exclusion_rationale: <text> | null
      byte_size: <int>
      kind: source | config | test | doc | asset | generated | build-script | binary
---
```

### 8.2 ART-01 Body Skeleton

```markdown
# ART-01: Repository Boundary Declaration

## 1. Executive Summary
<3–5 sentences>

## 2. Methodology
<how the artifact was produced>

## 3. Topology
<single-package | monorepo | polyrepo + evidence>

## 4. Primary Languages
<distribution table>

## 5. License
<detection result and conflict evaluation>

## 6. Version Control
<vcs system, branch, remote>

## 7. In-Scope Paths
<enumeration or summary by directory>

## 8. Excluded Paths
<table: path, rule_id, rationale>

## 9. Coverage Statement
<coverage_fraction + reconciliation>

## 10. Traceability Index
<mirror of header traceability_index>

## 11. Open Questions
<mirror of header open_questions>

## 12. Cross-References
- Downstream: ART-02, ART-03, ART-04, ART-05, ART-06 (consumed by PROMPT_02–PROMPT_06).
```

---

## 9. Quality Checks

The agent evaluates the following predicates before emitting the Completion Record.

### Baseline Checks (per `QUALITY_STANDARDS.md` § 3)

- **Q1. Coverage Check** — `coverage_fraction ≥ 0.99`. On fail, emit `COVERAGE_GAP` with the list of unclassified paths.
- **Q2. Citation Check** — `cited_claims / total_claims ≥ 0.95`. Every topology, language, license, and VCS claim MUST cite the file or directory that evidences it.
- **Q3. Schema Conformance Check** — the front-matter validates against § 4.2 of `QUALITY_STANDARDS.md` (Manifest schema).
- **Q4. Non-Contradiction Check** — no claim contradicts the engagement manifest (e.g., declared `scope_modifier` matches `resolved_scope.scope_modifier`).
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` license classification has an Open Question entry.
- **Q6. Idempotence Spot-Check** — re-running § 6.1 over a 5% sample of files yields identical SHA-256 digests.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-01.A. Fingerprint Determinism** — the fingerprint algorithm name matches § 6.1 exactly; no ad-hoc hash.
- **Q-01.B. Exclusion Auditability** — every excluded path has a non-null `exclusion_rule` and a non-empty `exclusion_rationale`.
- **Q-01.C. License Conflict Escalation** — IF the license is copyleft or proprietary and the authorization does not cover it, the artifact status is `BLOCKED`, not `DRAFT`.
- **Q-01.D. Monorepo Enumeration** — IF `topology = monorepo`, every workspace package has a recorded `manifest_path` resolvable to an existing file.
- **Q-01.E. Read-Only Compliance** — the agent did not modify, create, or delete any file under `subject_path` (verified by re-running the fingerprint computation post-write; digest MUST be unchanged).

---

## 10. Common Pitfalls

- Do not exclude a directory merely because its name is unfamiliar; only the patterns in § 6.2 are auto-excluded. Unfamiliar directories are in-scope by default and are analyzed by downstream prompts.
- Always record the exclusion rule for every excluded path; an excluded path without a `rule_id` is a coverage gap in disguise.
- Do not infer the license from package manifest metadata alone (e.g., `package.json` `"license": "MIT"`); the agent MUST verify by reading the `LICENSE` file. Manifest fields are advisory; the license file is authoritative.
- Do not read the `.git` object store; it is binary, opaque, and excluded per EXC-01. Reading `.git/HEAD` and `.git/config` is permitted for branch and remote detection only.
- Do not declare a language as primary based solely on file count; cross-check with byte count and manifest declarations. A repository with 1000 tiny `.json` files and 50 large `.ts` files is TypeScript-primary.
- Do not collapse the workspace enumeration for monorepos; each workspace package is a distinct in-scope subtree that PROMPT_02 and PROMPT_06 will analyze separately.
- Do not compute the fingerprint over excluded paths; the fingerprint MUST reflect only the in-scope set so that downstream mutation detection is not perturbed by changes to `node_modules`.
- Always verify that the `subject_path` provided in the engagement manifest actually resolves before computing the fingerprint; a typo in the path produces a fingerprint over an empty set, which silently corrupts all downstream mutation checks.

---

## 11. Handoff Criteria

PROMPT_02, PROMPT_03, PROMPT_04, PROMPT_05, and PROMPT_06 consume ART-01. Handoff requires ALL of:

- HC-01.1: ART-01 status is `REVIEWED` or higher (or `DRAFT` with orchestrator waiver for fast triage).
- HC-01.2: `coverage_fraction ≥ 0.99`.
- HC-01.3: `repository_fingerprint` is non-empty and the algorithm name matches § 6.1.
- HC-01.4: `topology` is one of the three enumerated values.
- HC-01.5: `primary_languages` is non-empty.
- HC-01.6: License is resolved or, if `UNVERIFIED`, the Open Question is non-blocking (orchestrator accepts the gap).
- HC-01.7: For `SCOPE_MODULE`, the in-scope set is non-empty and contains the target module path.
- HC-01.8: No `BLOCKING` open questions remain.

A failure on any HC blocks dispatch of PROMPT_02.

---

## 12. Cross-References

- **Consumed by:** PROMPT_02 (Tech Stack), PROMPT_03 (Folder Cartography), PROMPT_04 (Build & Config), PROMPT_05 (Entry Points), PROMPT_06 (Module Architecture).
- **Governing rules:** `OPERATING_RULES.md` R13 (read-only), R15 (fingerprint/mutation), R16 (binary exclusion), R34 (license escalation).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1 (generic header), § 4.2 (Manifest schema), § 5 (Manifest bar: aggregate ≥ 30, Coverage ≥ 4, Traceability ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.2 (directory layout), § 3.1 (naming), § 4 (header), § 6 (citations).
- **Mission linkage:** `MISSION.md` § 2.1 (Objective A — Total Structural Comprehension), § 6 (scope modifiers), § 5 (ethical boundaries).
- **Forward reference:** PROMPT_30 will re-verify `repository_fingerprint` at engagement close to confirm subject integrity per R15.

*End of PROMPT_01. Orchestrator may dispatch PROMPT_02 upon satisfaction of § 11.*
