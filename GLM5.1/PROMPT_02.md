# PROMPT_02.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_02: Technology Stack & Dependency Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_02
- **Phase:** 1
- **Stage:** 2 of 10
- **Dependencies:** ART-01 (PROMPT_01).
- **Estimated Tokens:** 10000–16000
- **Output Artifacts:** ART-02 (Manifest) — Technology Stack & Dependency Inventory.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Technology Stack & Dependency Inventory (ART-02) that records every declared programming language, runtime version, framework, library, package manager, build tool, and infrastructure-as-code artifact, classifies each dependency as direct or transitive and as production or development, and detects framework-specific markers used by downstream prompts.

---

## 3. When to Invoke

PROMPT_02 is dispatched when ALL of the following predicates hold:

- PROMPT_01 has emitted a Completion Record with `status: SUCCESS` or `status: PARTIAL` whose partial gaps do not include the in-scope path set.
- ART-01 exists at `<output_root>/artifacts/phase1/ART01_<engagement_id>_boundary-declaration.md` and is non-empty.
- ART-01 `resolved_scope.coverage_fraction ≥ 0.99`.
- ART-01 `topology` is populated and either `single-package` or `monorepo` (polyrepo subjects are reduced to the single declared subject).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | Resolve the in-scope path set, topology, workspace manager, and primary languages; read `repository_fingerprint` for mutation re-check. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R16 (generated lockfiles are read but not analyzed for behavior), R19 (cite the manifest line, not the lockfile line, for declared dependencies). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, and citation conventions. |
| `QUALITY_STANDARDS.md` | Framework file | Apply Manifest schema and type-specific bar. |

---

## 5. Instructions to AI Agent

1. Re-verify the `repository_fingerprint` recorded in ART-01 against a fresh computation; IF mismatched, emit `BLOCKED` with `INTEGRITY_FAIL` per R15.
2. Read ART-01's `primary_languages`, `topology`, and `workspace_manager` to seed the ecosystem set.
3. For each ecosystem detected (per § 6.1), locate the corresponding manifest files within the in-scope set.
4. Parse each manifest per § 6.2; extract declared dependencies with name, version specifier, and dependency-class.
5. Resolve transitive dependencies per § 6.3 where a lockfile is present; record resolution depth and source lockfile.
6. Classify each dependency as `production`, `development`, `optional`, or `peer` per § 6.4.
7. Detect framework markers per § 6.5 and emit a `frameworks` block.
8. Detect infrastructure-as-code artifacts per § 6.6 and emit an `infrastructure` block.
9. Detect runtime version constraints per § 6.7.
10. Detect package manager and version per § 6.8.
11. Cross-check declared primary languages (ART-01) against ecosystem manifests; flag mismatches as Open Questions.
12. Emit ART-02 per § 8 with full front-matter, body sections, traceability index, open questions.
13. Run the Quality Checks in § 9.
14. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Ecosystem Detection

Detect ecosystems by the presence of their canonical manifest files inside the in-scope set. Each ecosystem yields a `DEP-XX` namespace per `PROJECT_SPECIFICATION.md` § 4.1.

| Ecosystem | Manifest Files | Lockfile (transitive source) |
|-----------|----------------|------------------------------|
| Node.js | `package.json` | `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml` |
| Python | `requirements.txt`, `pyproject.toml`, `setup.py`, `Pipfile` | `poetry.lock`, `Pipfile.lock`, `requirements.txt` (pinned) |
| Go | `go.mod` | `go.sum` |
| Rust | `Cargo.toml` | `Cargo.lock` |
| Java (Maven) | `pom.xml` | (none; resolved at build) |
| Java (Gradle) | `build.gradle`, `build.gradle.kts` | `gradle.lockfile` |
| Ruby | `Gemfile` | `Gemfile.lock` |
| PHP | `composer.json` | `composer.lock` |
| .NET | `*.csproj`, `*.fsproj`, `*.vbproj`, `Directory.Packages.props` | `packages.lock.json` |
| Swift | `Package.swift` | `Package.resolved` |
| Elixir | `mix.exs` | `mix.lock` |
| Erlang | `rebar.config` | `rebar.lock` |
| Clojure | `project.clj`, `deps.edn` | `deps.lock` (rare) |

For each ecosystem detected, record `ecosystem_id`, `manifest_path`, `lockfile_path` (or `none`), and the manifest's declared package name and version. For monorepos, every workspace package is a separate manifest entry.

### 6.2 Manifest Parsing

Parse each manifest using the rules of its ecosystem. The agent MUST record the manifest line range for every declared dependency so the citation points to the declaration, not the resolved version.

**Node.js `package.json`** — read `dependencies`, `devDependencies`, `peerDependencies`, `optionalDependencies`, `bundleDependencies`. The `engines` field yields runtime version constraints. The `packageManager` field yields the package manager and version (e.g., `pnpm@9.0.0`).

**Python `pyproject.toml`** — read `[tool.poetry.dependencies]`, `[tool.poetry.dev-dependencies]`, `[project.dependencies]`, `[project.optional-dependencies]`. The `[tool.poetry]` `name` and `version` identify the project. The `requires-python` field yields the runtime constraint.

**Python `requirements.txt`** — every non-comment, non-`-r` line is a direct dependency. Lines beginning with `#` are comments. Lines beginning with `-e` or `git+` are VCS or editable dependencies and are recorded with their source URL.

**Go `go.mod`** — the `require` block lists direct dependencies. The `replace` and `exclude` blocks modify resolution and MUST be recorded. The `go` directive yields the Go runtime version. The `toolchain` directive (Go 1.21+) yields the toolchain constraint.

**Rust `Cargo.toml`** — read `[dependencies]`, `[dev-dependencies]`, `[build-dependencies]`, `[target.*.dependencies]`. The `[package]` `edition` field yields the Rust edition. The `[workspace]` section in a workspace root yields workspace members.

**Maven `pom.xml`** — read `<dependencies>`, `<dependencyManagement>`, `<properties>` (for version properties). The `<parent>` element yields the parent POM (often a Spring Boot starter). The `<java.version>` property yields the Java version.

**Gradle `build.gradle`** — read the `dependencies { ... }` block; classify by configuration (`implementation`, `api`, `compileOnly`, `runtimeOnly`, `testImplementation`). The `sourceCompatibility` and `targetCompatibility` yield Java version. Kotlin script variants (`build.gradle.kts`) parse identically.

**Ruby `Gemfile`** — read `gem` declarations. The `ruby` declaration yields the Ruby version. The `source` declarations yield gem sources.

**PHP `composer.json`** — read `require`, `require-dev`. The `config.platform.php` yields the PHP version constraint.

### 6.3 Transitive Dependency Resolution

Where a lockfile exists, parse it to recover the transitive set. The agent MUST NOT execute the package manager; only static parsing of the lockfile is permitted per R14.

- **`package-lock.json`** — every entry under `packages` is a resolved package; `dev` flag distinguishes dev from prod.
- **`yarn.lock`** — entries are keyed by name@version-range; resolution yields the locked version.
- **`pnpm-lock.yaml`** — `dependencies` and `devDependencies` blocks list importers; `packages` lists resolved snapshots.
- **`poetry.lock`** — `[[package]]` entries list resolved packages with dependencies.
- **`go.sum`** — every line is a hash entry; cross-reference `go.mod` for direct vs transitive (transitive = present in `go.sum` but not in `go.mod` require block).
- **`Cargo.lock`** — `[[package]]` entries; transitive = not declared in any `Cargo.toml` `[dependencies]`.
- **`Gemfile.lock`** — the `specs` block lists resolved gems; the dependency graph within the lockfile yields transitivity.
- **`composer.lock`** — `packages` and `packages-dev` arrays list resolved packages with `require` edges.

For each transitive dependency, record `declared_by` (the direct dependency that brought it in) where recoverable from the lockfile. Where the lockfile does not encode the introducer, mark `introducer: UNVERIFIED`.

### 6.4 Dependency Classification

Every dependency entry carries a `dependency_class`:

- `production` — required at runtime.
- `development` — required only at build/test/lint time.
- `optional` — declared optional; absence does not break build.
- `peer` — declared as a peer dependency (consumer must provide a compatible version).
- `build` — required only at build time (Rust `build-dependencies`, Gradle `compileOnly`).

Classification rules per ecosystem:

- Node: `dependencies` → production; `devDependencies` → development; `peerDependencies` → peer; `optionalDependencies` → optional.
- Python `pyproject.toml`: `[project.dependencies]` → production; `[project.optional-dependencies]` → optional; `[tool.poetry.dev-dependencies]` → development.
- Go: every `require` entry is production unless path contains `/test/` or suffix `_test`.
- Rust: `[dependencies]` → production; `[dev-dependencies]` → development; `[build-dependencies]` → build.
- Maven: `<scope>compile</scope>` (default) → production; `<scope>test</scope>` → development; `<scope>provided</scope>` → build; `<scope>runtime</scope>` → production.
- Gradle: `implementation`, `api`, `runtimeOnly` → production; `testImplementation`, `testRuntimeOnly` → development; `compileOnly` → build.
- Ruby: gems outside the `group :development` and `group :test` blocks → production.
- PHP: `require` → production; `require-dev` → development.

### 6.5 Framework Marker Detection

Detect frameworks by manifest declarations and by code markers. Record each detected framework with `framework_id`, `name`, `version`, `evidence_files`.

**Frontend frameworks:**
- React — dependency `react` in `package.json` OR file content marker `import React` or `from 'react'`.
- Vue — dependency `vue` OR file marker `from 'vue'` or `.vue` files present.
- Svelte — dependency `svelte` OR `.svelte` files present.
- Angular — `@angular/core` dependency OR `angular.json` present.
- Next.js — `next` dependency OR `next.config.js` present.
- Nuxt — `nuxt` dependency OR `nuxt.config.ts` present.
- Remix — `@remix-run/react` dependency.
- Astro — `astro` dependency OR `.astro` files.
- Solid — `solid-js` dependency.

**Backend frameworks:**
- Express — `express` dependency.
- Fastify — `fastify` dependency.
- Koa — `koa` dependency.
- NestJS — `@nestjs/core` dependency.
- Django — `django` in `requirements.txt` or `pyproject.toml`.
- Flask — `flask` in `requirements.txt` or `pyproject.toml`.
- FastAPI — `fastapi` in `requirements.txt` or `pyproject.toml`.
- Spring Boot — `org.springframework.boot:spring-boot-starter` in `pom.xml` or `org.springframework.boot` plugin in Gradle.
- Quarkus — `io.quarkus` in `pom.xml` or Gradle.
- Micronaut — `io.micronaut` in `pom.xml` or Gradle.
- Gin — `github.com/gin-gonic/gin` in `go.mod`.
- Echo — `github.com/labstack/echo` in `go.mod`.
- Actix — `actix-web` in `Cargo.toml`.
- Axum — `axum` in `Cargo.toml`.
- Rocket — `rocket` in `Cargo.toml`.
- Rails — `rails` in `Gemfile` OR `config/application.rb` references `Rails::Application`.
- Sinatra — `sinatra` in `Gemfile`.
- Laravel — `laravel/laravel` in `composer.json` OR `artisan` present.
- Symfony — `symfony/symfony` in `composer.json`.
- ASP.NET Core — `Microsoft.AspNetCore.*` in `*.csproj`.

### 6.6 Infrastructure-as-Code Detection

Scan the in-scope set for IaC artifacts and record each with its type and file path.

- **Docker** — `Dockerfile`, `Dockerfile.*`, `docker-compose.yml`, `docker-compose.*.yml`, `.dockerignore`.
- **Terraform** — `*.tf`, `*.tf.json`, `.terraform.lock.hcl`.
- **Kubernetes** — manifests matching `*.yaml` with `apiVersion` and `kind` fields; `kustomization.yaml`; Helm `Chart.yaml` and `templates/*.yaml`.
- **Pulumi** — `Pulumi.yaml`, `Pulumi.*.yaml`.
- **CloudFormation** — `*.template.json`, `*.template.yaml`, `serverless.template`.
- **Ansible** — `*.yml` under `playbooks/` or with `hosts:` key; `ansible.cfg`.
- **Serverless Framework** — `serverless.yml`, `serverless.ts`.

### 6.7 Runtime Version Constraints

For each ecosystem, extract the runtime version constraint:

- Node: `package.json` `engines.node`; `.nvmrc`; `.node-version`.
- Python: `pyproject.toml` `requires-python`; `runtime.txt`; `.python-version`; `Pipfile` `[requires] python_version`.
- Go: `go.mod` `go` directive and `toolchain` directive.
- Rust: `rust-toolchain.toml` or `rust-toolchain` file; `Cargo.toml` `edition`.
- Java: `pom.xml` `<maven.compiler.source>` and `<maven.compiler.target>`; `<java.version>` property; Gradle `sourceCompatibility`.
- Ruby: `Gemfile` `ruby` declaration; `.ruby-version`.
- PHP: `composer.json` `config.platform.php`.

### 6.8 Package Manager Detection

The package manager is detected from lockfile presence and manifest declarations:

- npm — `package-lock.json` present.
- yarn (classic) — `yarn.lock` with format version 1.
- yarn (berry) — `yarn.lock` with format version ≥ 4; `.yarnrc.yml` present.
- pnpm — `pnpm-lock.yaml` present; `package.json` `packageManager` field starts with `pnpm@`.
- poetry — `poetry.lock` present; `pyproject.toml` `[tool.poetry]`.
- pip — `requirements.txt` present, no poetry/Pipfile markers.
- pipenv — `Pipfile.lock` present.
- uv — `uv.lock` present.
- go modules — `go.mod` present (no alternative in modern Go).
- cargo — `Cargo.toml` present.
- maven — `pom.xml` present.
- gradle — `build.gradle` or `build.gradle.kts` present.
- bundler — `Gemfile.lock` present.
- composer — `composer.lock` present.
- nuget — `packages.lock.json` or `Directory.Packages.props` present.

---

## 7. Required Outputs

### ART-02 — Technology Stack & Dependency Inventory (Manifest)

**Type:** Manifest.

**Acceptance Criteria:**

- AC-02.1: The artifact file exists at `<output_root>/artifacts/phase1/ART02_<engagement_id>_tech-stack.md`.
- AC-02.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.2.
- AC-02.3: The body contains: Executive Summary, Methodology, Ecosystems, Languages & Runtimes, Frameworks, Package Managers, Infrastructure-as-Code, Dependency Inventory, Classification Summary, Traceability Index, Open Questions, Cross-References.
- AC-02.4: Every declared dependency has `name`, `version_specifier`, `dependency_class`, `manifest_path`, `manifest_line_range`.
- AC-02.5: Every direct dependency is recorded; transitive dependencies are recorded where a lockfile exists; absence of a lockfile is documented as an Open Question.
- AC-02.6: Every detected framework carries `name`, `version`, and at least one `evidence_file`.
- AC-02.7: Every IaC artifact carries `type` and `path`.
- AC-02.8: All claims cite `<manifest_path>:<line_range>` or, for code markers, `<file_path>:<line_range>`.
- AC-02.9: For monorepos, every workspace package's dependencies are enumerated separately under the package's `DEP-XX` namespace.

---

## 8. Output Templates

### 8.1 ART-02 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-02
artifact_type: Manifest
producing_prompt: PROMPT_02
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
repository_fingerprint_recheck: <sha-256-hex>   # MUST match ART-01's repository_fingerprint
ecosystems:
  - ecosystem_id: EC-01
    name: node | python | go | rust | maven | gradle | ruby | php | nuget | swift | elixir | erlang | clojure
    manifest_path: <path>
    lockfile_path: <path> | none
    package_manager: <name>
    package_manager_version: <version> | UNVERIFIED
    workspace_package: <name> | null
languages_and_runtimes:
  - language: <name>
    runtime_version_constraint: <specifier> | UNVERIFIED
    evidence: <path>:<line>
frameworks:
  - framework_id: FW-01
    name: <name>
    version: <version> | UNVERIFIED
    kind: frontend | backend | fullstack | mobile | desktop
    evidence_files:
      - <path>:<line>
infrastructure:
  - id: IAC-01
    type: docker | terraform | kubernetes | pulumi | cloudformation | ansible | serverless
    path: <path>
    notes: <text> | ""
source_coverage:
  - path: <manifest_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <path>:<line-range>
    symbol: <name>
items:
  - id: DEP-000001
    name: <dependency-name>
    type: dependency
    location: <manifest_path>:<line>
    attributes:
      ecosystem: <name>
      version_specifier: <specifier>
      resolved_version: <version> | UNVERIFIED
      dependency_class: production | development | optional | peer | build
      direct: true | false
      introducer: <DEP-XX> | null
      manifest_path: <path>
      manifest_line_range: <start-end>
      lockfile_path: <path> | none
---
```

### 8.2 ART-02 Body Skeleton

```markdown
# ART-02: Technology Stack & Dependency Inventory

## 1. Executive Summary
## 2. Methodology
## 3. Ecosystems
## 4. Languages & Runtimes
## 5. Frameworks
## 6. Package Managers
## 7. Infrastructure-as-Code
## 8. Dependency Inventory
   ### 8.1 Direct Dependencies
   ### 8.2 Transitive Dependencies
## 9. Classification Summary
## 10. Traceability Index
## 11. Open Questions
## 12. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks (per `QUALITY_STANDARDS.md` § 3)

- **Q1. Coverage Check** — every in-scope manifest file declared in ART-01 is parsed and represented. Threshold ≥ 0.95 (some manifests may be empty by design; such emptiness is documented, not penalized).
- **Q2. Citation Check** — every dependency entry cites `<manifest_path>:<line_range>`; ≥ 0.95 cited.
- **Q3. Schema Conformance Check** — front-matter validates against § 4.2.
- **Q4. Non-Contradiction Check** — no framework or dependency claim contradicts ART-01's `primary_languages` or `topology` declarations.
- **Q5. UNVERIFIED Accounting** — every `introducer: UNVERIFIED` has an Open Question.
- **Q6. Idempotence Spot-Check** — re-parsing a 5% sample of manifests yields the same dependency set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-02.A. Lockfile Cross-Check** — IF a lockfile exists, the count of locked packages is recorded; the count of direct dependencies is ≤ the count of locked packages.
- **Q-02.B. Framework Evidence** — every framework entry has at least one `evidence_file` that exists in the in-scope set.
- **Q-02.C. Classification Completeness** — every dependency has a non-null `dependency_class` and a non-null `direct` boolean.
- **Q-02.D. Monorepo Per-Package Enumeration** — IF `topology = monorepo`, every workspace package declared in ART-01 has a corresponding ecosystem entry with at least one direct dependency (or an explicit `empty-manifest` note).
- **Q-02.E. Runtime Constraint Citation** — every `runtime_version_constraint` cites the manifest line that declares it.

---

## 10. Common Pitfalls

- Do not cite the lockfile as the source of a declared dependency; cite the manifest line that declares it per R19. The lockfile is consulted only for resolved versions and transitive closure.
- Do not infer a framework's presence from a single import statement without verifying the dependency is declared; test files and examples may import frameworks that are not production dependencies.
- Do not collapse monorepo packages into a single dependency list; each workspace package has its own dependency closure and its own `DEP-XX` namespace.
- Do not classify `peerDependencies` as `production`; peer dependencies are the consumer's responsibility and require separate classification.
- Always distinguish direct from transitive; a direct dependency is one declared in a manifest, a transitive dependency is one resolved via the lockfile but not declared directly.
- Do not infer transitive dependencies by reading `node_modules` or `vendor/`; these directories are excluded per ART-01 EXC-03/EXC-05 and may be absent.
- Do not declare a runtime version constraint from a comment or README; cite the manifest or `.<lang>-version` file that the toolchain actually reads.
- Always record the package manager version when declared via `packageManager` field; an unversioned package manager is a build-reproducibility risk that PROMPT_04 will flag.
- Do not skip infrastructure-as-code detection because the files are in a subdirectory like `deploy/` or `infra/`; scan the entire in-scope set, not just the repository root.

---

## 11. Handoff Criteria

PROMPT_04, PROMPT_23, PROMPT_24, and PROMPT_26 consume ART-02. Handoff requires ALL of:

- HC-02.1: ART-02 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-02.2: Every in-scope manifest file is parsed (coverage ≥ 0.95).
- HC-02.3: Every direct dependency has `dependency_class` and `direct` populated.
- HC-02.4: Framework detection has run for every detected ecosystem.
- HC-02.5: IaC detection has run over the entire in-scope set.
- HC-02.6: Runtime version constraints are recorded or marked `UNVERIFIED` with Open Question.
- HC-02.7: `repository_fingerprint_recheck` matches ART-01's `repository_fingerprint`.
- HC-02.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_04 (Build & Config — uses package managers and frameworks to drive build detection), PROMPT_23 (Design Patterns — uses framework set to scope pattern detection), PROMPT_24 (Engineering Decisions — uses framework choices as decision evidence), PROMPT_26 (Rebuild Guide — uses stack to write the rebuild instructions).
- **Depends on:** ART-01 (PROMPT_01) for in-scope set and topology.
- **Governing rules:** `OPERATING_RULES.md` R13, R14, R15, R16, R19.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.2; Manifest bar (aggregate ≥ 30, Coverage ≥ 4, Traceability ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 3.1, § 4, § 6.
- **Forward reference:** PROMPT_30 re-verifies that every dependency declared in a manifest appears in ART-02 (engagement-wide coverage sweep).

*End of PROMPT_02. Orchestrator may dispatch PROMPT_03 upon satisfaction of § 11.*
