# PROMPT_06.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_06: Module Architecture Extraction

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_06
- **Phase:** 1
- **Stage:** 6 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-03 (PROMPT_03), ART-05 (PROMPT_05).
- **Estimated Tokens:** 13000–19000
- **Output Artifacts:** ART-06 (Map) — Module Map.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Module Map (ART-06) that identifies every logical module in the subject repository by analyzing import/export graphs and barrel files, assigns each module a set of files, builds the inter-module dependency graph, constructs the module hierarchy, and flags circular module dependencies.

---

## 3. When to Invoke

PROMPT_06 is dispatched when ALL of the following predicates hold:

- PROMPT_05 has emitted a Completion Record satisfying its Handoff Criteria.
- ART-01, ART-03, and ART-05 are present and non-empty.
- ART-03 has `coverage_fraction ≥ 0.99` and has flagged every barrel file (`is_barrel: true`).

PROMPT_06 MAY be dispatched in parallel with PROMPT_07 only if the orchestrator authorizes a parallel batch under R12; the two share no input dependency but both consume ART-03.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; fingerprint re-verification; scope modifier (affects module closure under `SCOPE_MODULE`). |
| ART-03 | Map | File nodes, file roles, barrel file detection, responsibility statements — the seed for module boundary inference. |
| ART-05 | Map | Entry-point catalog — module hierarchy is rooted at the entry-point modules; bootstrap traces reveal the implicit module load order. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R16 (generated modules flagged), R17, R19 (cite the import statement, not the call), R21. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, and Mermaid graph conventions. |
| `QUALITY_STANDARDS.md` | Framework file | Apply Map schema (`§ 4.3`) and type-specific bar (aggregate ≥ 30, Coverage ≥ 4, Precision ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Construct the import graph per § 6.1 by parsing every `source` file's import statements.
3. Detect module boundaries per § 6.2 using the import graph, ART-03's barrel files, and folder conventions.
4. Assign each in-scope file to exactly one module per § 6.3.
5. Build the module dependency graph per § 6.4 from the file-level import graph aggregated to modules.
6. Construct the module hierarchy per § 6.5.
7. Detect circular module dependencies per § 6.6.
8. Compute module metrics per § 6.7 (size, fan-in, fan-out, stability).
9. Emit ART-06 per § 8 with full front-matter, module catalog, dependency graph (Mermaid), hierarchy, circular dependencies, metrics, traceability index, open questions.
10. Run the Quality Checks in § 9.
11. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Import Graph Construction

For every `source` file in ART-03, parse its import statements and produce a directed edge `(importer, imported)` per `PROJECT_SPECIFICATION.md` § 4.2 (`IMPORTS` / `IMPORTED_BY`). The agent MUST normalize the import specifier to a resolved file path within the in-scope set; unresolved imports are marked `EXTERNAL`.

**Normalization procedure:**

1. Parse every import statement per language (see below).
2. For each specifier, resolve relative to the importing file's directory (for relative specifiers like `./foo`, `../bar`).
3. For non-relative specifiers (e.g., `react`, `lodash`), check the path-alias map from `tsconfig.json` `paths` (per ART-04), then check the package's `exports` map (per ART-02), then check `node_modules` resolution — but the agent MUST NOT descend into `node_modules` (excluded per EXC-03); instead, mark `EXTERNAL`.
4. For each resolved path, add the edge `(importer.file_id, imported.file_id)` with the citation `<importing_file>:<line_range>`.
5. For Go, parse `import` blocks; resolve against the module path declared in `go.mod`; cross-module imports are `EXTERNAL`.
6. For Rust, parse `use` declarations; resolve against the crate root and `mod` declarations; cross-crate imports (from `extern crate` or `use <crate>::...`) are `EXTERNAL`.
7. For Python, parse `import x` and `from x import y`; resolve against the in-scope set's `__init__.py` files; cross-package imports are `EXTERNAL`.
8. For Java/Kotlin, parse `import` statements; resolve against the source root (`src/main/java`, `src/main/kotlin`); cross-package imports outside the source root are `EXTERNAL`.

**Per-language import parsers:**

- **TypeScript/JavaScript (ESM)** — `import x from 'spec'`, `import { a } from 'spec'`, `import * as x from 'spec'`, `export ... from 'spec'` (re-exports create edges too).
- **TypeScript/JavaScript (CommonJS)** — `require('spec')`, `module.exports = require('spec')`.
- **Python** — `import x`, `import x.y`, `from x import y`, `from . import y`, `from .x import y`.
- **Go** — `import ("path")`, `import ( ... )` blocks, single and grouped.
- **Rust** — `use crate::x`, `use self::x`, `use super::x`, `use <crate>::x`, `use x::{a, b}`.
- **Java/Kotlin** — `import x.y.Z;`, `import static x.y.Z.method;`.
- **Ruby** — `require 'x'`, `require_relative 'x'`, `include X`, `extend X`.
- **PHP** — `use X\Y\Z;`, `require 'x.php'`, `require_once 'x.php'`, `include 'x.php'`.
- **C#** — `using X.Y.Z;`, `using static X.Y.Z.Method;`.
- **Swift** — `import X`.
- **Elixir** — `alias X.Y`, `import X`, `use X`, `require X`.
- **C/C++** — `#include "x.h"`, `#include <x.h>`.

For every edge, record `from_file` (`F-XX`), `to_file` (`F-XX` or `EXTERNAL`), `specifier` (the raw string), `resolution` (the resolved path or `EXTERNAL`), `kind` (esm-import | cjs-require | re-export | namespace-import | side-effect-import), `citation` (`<file>:<line>`).

### 6.2 Module Boundary Inference

A module is a cohesive unit of related functionality. Modules are inferred by combining three signals:

1. **Folder boundaries** — every top-level directory under `src/` (or `lib/`, `app/`, `internal/`, `pkg/`, `cmd/`) that contains source files is a candidate module. Sub-directories are sub-modules.
2. **Barrel files** — every barrel file (per ART-03) defines a public API surface; the barrel's directory is a module, and the files the barrel re-exports are the module's public exports.
3. **Namespace markers** — Go `package` clauses, Rust `mod` declarations, Java/Kotlin `package` declarations, C# `namespace` declarations, Python `__init__.py`-defined packages, Elixir `defmodule X.Y` declarations define explicit module boundaries.

**Inference procedure:**

1. Start with folder-based candidate modules: every directory containing at least one `source` file is a candidate.
2. IF the directory contains a barrel file (per § 6.2.1 of PROMPT_03), the barrel is the module's public entry; the directory is a module.
3. IF the directory contains a namespace declaration that differs from the parent's namespace, the directory is a module.
4. IF the directory contains only test files, it is NOT a module (test files are part of the module under test).
5. IF the directory contains only generated files, it is NOT a module (generated files defer to their source generator).
6. Merge candidates whose import graph is dense (>70% intra-candidate import density) and whose external imports are sparse (<30%) into a single module.
7. Split candidates whose import graph is sparse (<40% intra-candidate density) into sub-modules by sub-directory.
8. For monorepos (per ART-01 `topology = monorepo`), each workspace package is a top-level module; the package's internal structure is decomposed per steps 1–7.

Each module is assigned an `M-XX` ID per `PROJECT_SPECIFICATION.md` § 4.1.

### 6.3 File-to-Module Assignment

Every in-scope `source` file MUST be assigned to exactly one module. The assignment procedure:

1. IF the file is a barrel, it belongs to the module defined by its containing directory.
2. ELSE IF the file's namespace declaration matches a module's namespace, the file belongs to that module.
3. ELSE IF the file's directory is a module, the file belongs to that module.
4. ELSE IF the file's directory's nearest-ancestor module exists, the file belongs to that ancestor.
5. ELSE the file is `ORPHAN_FILE` and is flagged with an Open Question; the agent MUST NOT invent a module for it.

Test files are assigned to the module under test (the module of the file with the same basename minus the test suffix). Migration files are assigned to a synthetic `M-migrations` module. Build-script files are assigned to a synthetic `M-build` module.

### 6.4 Module Dependency Graph

Aggregate the file-level import graph to the module level. For every file-level edge `(F_a, F_b)` where `F_a ∈ M_x` and `F_b ∈ M_y` and `M_x ≠ M_y`, add a module-level edge `(M_x, M_y)` of type `IMPORTS` (or `DEPENDS_ON` if the import is a runtime dependency rather than a type-only import; the distinction is made by the import's `kind` field — `type` imports in TypeScript (`import type {X}`) and Java `import` (which is type-only) are `IMPORTS`, not `DEPENDS_ON`).

Each module-level edge records:

- `from_module` (`M-XX`).
- `to_module` (`M-XX`).
- `relationship` — `IMPORTS` or `DEPENDS_ON`.
- `evidence_files` — the list of file-level edges that compose this module-level edge.
- `weight` — the count of file-level edges.
- `external` — `false` if `to_module` is in-scope, `true` if the import targets an external package (in which case `to_module` is the external package name, recorded as `DEP-XX` per ART-02).

External dependencies are recorded as a separate `EXTERNAL_DEPENDENCY` edge list, not as in-scope modules.

### 6.5 Module Hierarchy Construction

Construct the module hierarchy:

1. Top-level modules — modules whose containing directory is a direct child of a source root (`src/`, `lib/`, `app/`, `internal/`, `pkg/`, `cmd/`, or the repository root for Go/Rust).
2. Sub-modules — modules whose containing directory is a descendant of another module's directory. The relationship is `PART_OF` / `HAS_PART` per `PROJECT_SPECIFICATION.md` § 4.2.
3. For monorepos, every workspace package (per ART-01) is a top-level module; sub-modules are nested within.

The hierarchy is recorded as a forest (multiple roots) with `root_id` and `parent_id` fields. Roots are entry-point modules (per ART-05) or, in their absence, the topmost directory modules.

### 6.6 Circular Dependency Detection

Detect circular module dependencies using Tarjan's strongly connected components (SCC) algorithm on the module dependency graph. Every SCC with more than one module is a circular dependency; every SCC of size 1 with a self-edge is a self-import (a module that imports itself, typically through re-exports).

For every circular dependency:

- Record the SCC's modules in topological order (impossible for a true cycle; record the cycle's nodes in the order they appear in the cycle).
- Record the edges that form the cycle.
- Classify the cycle: `BUILD_TIME_CYCLE` (only type-only imports), `RUNTIME_CYCLE` (at least one runtime import), `RE_EXPORT_CYCLE` (only re-export edges).
- Severity: `RUNTIME_CYCLE` is `MAJOR`; `BUILD_TIME_CYCLE` is `MINOR`; `RE_EXPORT_CYCLE` is `INFO`.
- Mark every module in the cycle with `in_circular_dependency: true`.

### 6.7 Module Metrics

Compute for every module:

- `file_count` — number of source files in the module.
- `line_count` — total source lines.
- `fan_in` — number of in-scope modules that import this module.
- `fan_out` — number of in-scope modules this module imports.
- `external_fan_out` — number of external packages this module imports.
- `instability` — `fan_out / (fan_in + fan_out)` (per Robert C. Martin's stability metric; 0 = maximally stable, 1 = maximally unstable).
- `abstractness` — `abstract_symbol_count / total_symbol_count` where abstract symbols are interfaces, abstract classes, traits, protocols. Abstractness is computed in PROMPT_08 and cross-referenced here as `UNVERIFIED` if PROMPT_08 has not yet run.
- `public_api_surface` — count of symbols exported via the module's barrel file or its public namespace.

Metrics are recorded as `module_metrics` blocks per module.

---

## 7. Required Outputs

### ART-06 — Module Map

**Type:** Map.

**Acceptance Criteria:**

- AC-06.1: The artifact file exists at `<output_root>/artifacts/phase1/ART06_<engagement_id>_module-map.md`.
- AC-06.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.3.
- AC-06.3: The body contains: Executive Summary, Methodology, Module Catalog, Module Hierarchy, Inter-Module Dependency Graph (Mermaid), Circular Dependencies, Module Metrics, External Dependencies, Traceability Index, Open Questions, Cross-References.
- AC-06.4: Every in-scope `source` file is assigned to exactly one module.
- AC-06.5: Every module has `id`, `name`, `root_path`, `file_count`, `metrics`, and a citation to the barrel file or namespace declaration that establishes it.
- AC-06.6: Every inter-module edge has `from_module`, `to_module`, `relationship`, `weight`, and at least one `evidence_file` citation.
- AC-06.7: Every circular dependency is enumerated with its member modules and the cycle's edges.
- AC-06.8: The Mermaid graph is emitted per `OUTPUT_RULES.md` § 7 with edge-level citations (`edge: file:line`).
- AC-06.9: Graphs larger than 30 modules are decomposed into sub-graphs with a master index diagram.

---

## 8. Output Templates

### 8.1 ART-06 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-06
artifact_type: Map
producing_prompt: PROMPT_06
phase: 1
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
modules:
  - module_id: M-01
    name: <name>
    root_path: <relative-path>
    namespace: <name> | UNVERIFIED
    parent_module: M-XX | null
    kind: top-level | sub-module | workspace-package | synthetic
    files: [F-XXXXXX]
    public_api_symbols: [FN-XX | K-XX | I-XX | V-XX]
    barrel_file: F-XXXXXX | null
    metrics:
      file_count: <int>
      line_count: <int>
      fan_in: <int>
      fan_out: <int>
      external_fan_out: <int>
      instability: <0..1>
      abstractness: <0..1> | UNVERIFIED
      public_api_surface: <int>
    in_circular_dependency: true | false
module_edges:
  - from: M-01
    to: M-02
    relationship: IMPORTS | DEPENDS_ON
    weight: <int>
    evidence_files:
      - <file_path>:<line-range>
    external: false
  - from: M-01
    to: DEP-000123        # external dependency reference
    relationship: IMPORTS
    weight: <int>
    evidence_files:
      - <file_path>:<line-range>
    external: true
circular_dependencies:
  - cycle_id: CYC-01
    members: [M-XX, ...]
    edges:
      - { from: M-XX, to: M-XX, evidence: <file>:<line> }
    kind: BUILD_TIME_CYCLE | RUNTIME_CYCLE | RE_EXPORT_CYCLE
    severity: MAJOR | MINOR | INFO
module_hierarchy:
  - module_id: M-01
    parent_module: null
    depth: 0
    children: [M-XX, ...]
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
  - id: M-01
    label: <name>
    kind: module
    parent_id: null
    depth: 0
edges:
  - from: M-01
    to: M-02
    relationship: IMPORTS
    evidence: <file_path>:<line-range>
---
```

### 8.2 ART-06 Body Skeleton

```markdown
# ART-06: Module Map

## 1. Executive Summary
## 2. Methodology
## 3. Module Catalog
## 4. Module Hierarchy
## 5. Inter-Module Dependency Graph
   ### 5.1 Master Index Diagram
   **Diagram D-01: Module Dependency Graph (Overview)**
   ```mermaid
   graph TD
       M01[M-01: auth] --> M02[M-02: users]
       M01 --> M03[M-03: tokens]
       %% edge: src/auth/index.ts:5
   ```
   ### 5.2 Sub-graph: <Cluster>
## 6. Circular Dependencies
## 7. Module Metrics
## 8. External Dependencies
## 9. Traceability Index
## 10. Open Questions
## 11. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every in-scope `source` file is assigned to a module; threshold ≥ 0.99.
- **Q2. Citation Check** — ≥ 0.95 of module edges and assignments cited.
- **Q3. Schema Conformance Check** — validates against § 4.3.
- **Q4. Non-Contradiction Check** — no module assignment contradicts ART-03's barrel detection (a barrel file's directory MUST be a module).
- **Q5. UNVERIFIED Accounting** — every `ORPHAN_FILE`, `UNVERIFIED namespace`, and `UNVERIFIED abstractness` has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.2 on a 5% sample of files yields the same module assignment.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-06.A. Single Assignment** — every source file appears in exactly one module's `files` list.
- **Q-06.B. Edge Bidirectionality** — for every edge `(M_a, M_b)` of type `IMPORTS`, the reverse edge `(M_b, M_a)` exists implicitly as `IMPORTED_BY` (recorded in the graph but not in `module_edges` to avoid duplication).
- **Q-06.C. Cycle Detection** — Tarjan's algorithm runs over the in-scope module graph; every SCC of size > 1 is reported.
- **Q-06.D. Mermaid Edge Citation** — every edge in the Mermaid graph cites at least one `evidence_file`.
- **Q-06.E. External Edge Classification** — edges to packages outside the in-scope set are marked `external: true` and reference `DEP-XX` from ART-02.
- **Q-06.F. Hierarchy Acyclic** — the `PART_OF` hierarchy is acyclic (verified by a topological sort on `parent_module` relationships).

---

## 10. Common Pitfalls

- Do not treat every directory as a module; empty directories, asset-only directories, and test-only directories are not modules.
- Always normalize import specifiers before adding edges; an unresolved relative path produces a false `EXTERNAL` edge and a false circular-dependency report.
- Do not collapse two modules because they share a name; namespacing collisions across monorepo packages are common and must be preserved as distinct modules.
- Always distinguish `IMPORTS` (type-only) from `DEPENDS_ON` (runtime); a type-only import does not create a runtime coupling and should not be flagged as a circular runtime dependency.
- Do not infer module boundaries from README descriptions or comments; the import graph is the authoritative signal per R22.
- Always record the barrel file as the module's public entry; a module without a barrel file has its public API inferred from its non-test, non-internal source files.
- Do not skip generated-file modules entirely; record them as `M-XX` with `kind: synthetic` and a `generator_hint` per ART-03, even though their internals are not documented.
- Always cross-check the module hierarchy against ART-05's entry points; an entry point that does not belong to a top-level module is `ORPHAN_ENTRY` and is flagged.
- Do not run cycle detection only on the module graph; run it also on the file-level import graph (a module-level cycle may be a false positive caused by a single file-level cycle through a barrel).
- Always cite the import statement that evidences a module edge, not the call site that uses the imported symbol (per R19).

---

## 11. Handoff Criteria

PROMPT_07, PROMPT_10, and PROMPT_11 consume ART-06. Handoff requires ALL of:

- HC-06.1: ART-06 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-06.2: Every in-scope `source` file is assigned to a module (coverage ≥ 0.99).
- HC-06.3: Every module has metrics populated (fan_in, fan_out, instability).
- HC-06.4: Inter-module dependency graph has at least one Mermaid diagram with edge-level citations.
- HC-06.5: Circular dependencies are enumerated with severity classification.
- HC-06.6: External dependencies are listed and cross-referenced to ART-02's `DEP-XX`.
- HC-06.7: `repository_fingerprint_recheck` matches ART-01.
- HC-06.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_07 (Component Architecture — uses module map to scope component detection; components are typically within a single module), PROMPT_10 (Call Graph & Dependency Graph — uses module edges as the dependency graph's seed), PROMPT_11 (Data Flow — uses module boundaries to scope data-flow tracing).
- **Depends on:** ART-01 (PROMPT_01), ART-03 (PROMPT_03), ART-05 (PROMPT_05).
- **Governing rules:** `OPERATING_RULES.md` R13, R16, R17, R19, R21.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.3; Map bar (aggregate ≥ 30, Coverage ≥ 4, Precision ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 3.1, § 4, § 6, § 7 (Mermaid graphs with edge citations).
- **Forward reference:** PROMPT_30 will verify that every module in ART-06 has a corresponding entry in ART-10's dependency graph.

*End of PROMPT_06. Orchestrator may dispatch PROMPT_07 upon satisfaction of § 11.*
