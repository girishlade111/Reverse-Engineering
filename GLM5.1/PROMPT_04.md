# PROMPT_04.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_04: Build System & Configuration Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_04
- **Phase:** 1
- **Stage:** 4 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-03 (PROMPT_03).
- **Estimated Tokens:** 11000–17000
- **Output Artifacts:** ART-04 (Spec) — Build & Configuration Map.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Build & Configuration Map (ART-04) that records every build tool, build configuration file, declared script, environment variable, feature flag, multi-environment configuration, build output location, and CI/CD pipeline step, with each entry traceable to the source file and line that declares it.

---

## 3. When to Invoke

PROMPT_04 is dispatched when ALL of the following predicates hold:

- PROMPT_01 has emitted a Completion Record with `status: SUCCESS` or non-blocking `PARTIAL`.
- ART-01, ART-02, and ART-03 exist and pass their respective Handoff Criteria.
- ART-02 records at least one ecosystem with a manifest and (optionally) a lockfile.

PROMPT_04 MAY be dispatched in parallel with PROMPT_05 if the orchestrator authorizes a parallel batch under R12; the two have no inter-dependency per `MASTER_INDEX.md` § 6.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | Resolve the in-scope path set; re-verify `repository_fingerprint`. |
| ART-02 | Manifest | Resolve package managers, frameworks, ecosystems; drive build-tool detection (e.g., Next.js presence implies Next.js build pipeline). |
| ART-03 | Map | Resolve `config` and `build-script` classified files as candidate build configuration sources. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R16 (generated config not documented as authored), R17 (citation format), R19 (cite the script definition, not the call site). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, and citation conventions. |
| `QUALITY_STANDARDS.md` | Framework file | Apply Spec schema (`§ 4` is implicit; Spec extends Doc with the same header) and type-specific bar (aggregate ≥ 32, Accuracy ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect build tools per § 6.1 using ART-02's ecosystems and ART-03's `build-script` files.
3. For every detected build tool, locate and parse its configuration file per § 6.2.
4. Enumerate every declared script per § 6.3 (npm scripts, Makefile targets, justfile recipes, package.json `scripts`, Gradle tasks, Rake tasks, Cargo aliases).
5. Extract every environment variable referenced per § 6.4 (from config files, code `process.env.*` / `os.environ` / `os.Getenv` reads, and `.env.example`).
6. Identify multi-environment configuration per § 6.5 (dev/staging/prod configs, `.env.development`, `config/production.json`).
7. Identify feature flags per § 6.6.
8. Identify build outputs and their declared location per § 6.7.
9. Identify CI/CD pipelines per § 6.8 and enumerate every pipeline step.
10. Identify configuration layering per § 6.9 (env override files, default + override precedence).
11. Emit ART-04 per § 8 with full front-matter, body sections, traceability index, open questions.
12. Run the Quality Checks in § 9.
13. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Build Tool Detection

A build tool is any tool that transforms source into a runnable or deployable artifact. Detect build tools by combining ART-02's ecosystems and ART-03's `build-script` files.

| Build Tool | Detection Marker |
|------------|------------------|
| webpack | `webpack` in `devDependencies`; `webpack.config.*` present |
| Rollup | `rollup` in `devDependencies`; `rollup.config.*` present |
| Vite | `vite` in `devDependencies`; `vite.config.*` present |
| esbuild | `esbuild` in `devDependencies`; build script invokes `esbuild` |
| Parcel | `parcel` in `devDependencies`; `.parcelrc` present |
| Turbopack | `next` ≥ 13 with `turbo: true` or `--turbo` flag in scripts |
| SWC | `@swc/core` in `devDependencies`; `.swcrc` present |
| Babel | `@babel/core` in `devDependencies`; `babel.config.*` or `.babelrc*` present |
| TypeScript (tsc) | `typescript` in `devDependencies`; `tsconfig.json` present; script invokes `tsc` |
| Next.js build | `next` in `dependencies`; script `next build` |
| Nuxt build | `nuxt` in `dependencies`; script `nuxt build` |
| tsc + bundler hybrid | script invokes both `tsc` and a bundler |
| Make | `Makefile`/`makefile`/`GNUmakefile` present |
| Just | `justfile`/`Justfile` present |
| CMake | `CMakeLists.txt` present |
| Gradle | `build.gradle`/`build.gradle.kts`/`settings.gradle` present |
| Maven | `pom.xml` present |
| Cargo | `Cargo.toml` present |
| pip / setuptools | `setup.py`/`setup.cfg`/`pyproject.toml` with `[build-system]` |
| Poetry | `pyproject.toml` with `[tool.poetry]` |
| hatch | `pyproject.toml` with `[tool.hatch]` |
| Rake | `Rakefile` present |
| Bundler | `Gemfile` + `Gemfile.lock` (bundler is the implicit build tool for Ruby gems) |
| Go build | `go.mod` present (the `go` toolchain is the implicit builder) |
| dotnet | `*.csproj`/`*.fsproj`/`*.sln` present |
| Swift Package Manager | `Package.swift` present |
| Mix | `mix.exs` present |
| rebar3 | `rebar.config` present |
| Docker build | `Dockerfile` present |
| Bazel | `WORKSPACE`/`MODULE.bazel`/`BUILD`/`BUILD.bazel` present |
| Buck | `BUCK`/`BUCK2` present |
| Pants | `pants.toml`/`pants.ci.toml` present |
| Nx | `nx.json` present |
| Turbo (monorepo) | `turbo.json` present |

Record each detected tool with `tool_id`, `name`, `version` (from ART-02 where available), `config_files`, and `evidence_line`.

### 6.2 Build Configuration Parsing

Parse each detected tool's configuration file. The agent MUST record the file path and the line range of every parsed directive. Configurations are parsed as follows.

**`tsconfig.json`** — read `compilerOptions.target`, `module`, `moduleResolution`, `jsx`, `strict`, `outDir`, `rootDir`, `baseUrl`, `paths` (path aliases — used by PROMPT_06), `references` (project references — used by PROMPT_06). Record `extends` chains (resolved against the in-scope set).

**`vite.config.{ts,js,mjs}`** — read the `define` block (compile-time constants), `build.outDir`, `build.rollupOptions.input`, `resolve.alias`, `plugins` array (each plugin name is recorded with its options shape — not the options values, which may be runtime-dependent).

**`webpack.config.{js,ts,cjs,mjs}`** — read `entry` (single or map), `output.path`, `output.filename`, `resolve.alias`, `module.rules` count (rules themselves are summarized by loader name, not full content), `plugins` array (plugin names).

**`rollup.config.{js,ts,mjs}`** — read `input`, `output.dir`/`output.file`, `output.format`, `plugins` array, `external` array.

**`next.config.{js,ts,mjs}`** — read `experimental` flags, `rewrites`, `redirects`, `headers`, `env`, `images.domains`, `i18n` config, `webpack` customization presence.

**`Makefile`/`makefile`** — every line matching `^<target>: <prereqs>` is a target; record target name, prerequisites, and the recipe lines (lines beginning with TAB).

**`justfile`/`Justfile`** — every line matching `^<recipe>:` is a recipe; record recipe name and body.

**`CMakeLists.txt`** — record `project()` name and version, `cmake_minimum_required`, `add_executable()`, `add_library()`, `target_link_libraries()`, `set()` variables.

**`build.gradle`/`build.gradle.kts`** — record `plugins {}` block, `dependencies {}` block (already in ART-02), `tasks.register()`/`tasks.create()`, the `java { toolchain }` block, the `application { mainClass }` block.

**`pom.xml`** — record `<build><plugins>` (each `<plugin>` with `groupId`, `artifactId`, `version`), `<profiles>` (each profile id and activation), `<properties>` (build properties).

**`Cargo.toml`** — record `[[bin]]` targets, `[lib]` target, `[profile.*]` sections, `[features]` map (feature flags), `[build-dependencies]`.

**`pyproject.toml`** — record `[tool.poetry.scripts]` (entry points), `[tool.poetry.plugins]`, `[project.scripts]`, `[build-system]` backend, any tool-specific tables (`[tool.black]`, `[tool.isort]`, `[tool.mypy]`, `[tool.pytest.ini_options]`).

**`setup.py`/`setup.cfg`** — record `entry_points`, `install_requires` (cross-checked against ART-02), `extras_require`, `options`.

**`.csproj`/`.fsproj`** — record `<PropertyGroup>` (TargetFramework, OutputType, Nullable, LangVersion), `<ItemGroup>` (PackageReference, ProjectReference, Compile items), `<Target>` custom MSBuild targets.

**`Dockerfile`** — record every `FROM`, `RUN`, `COPY`, `ADD`, `CMD`, `ENTRYPOINT`, `ENV`, `ARG`, `EXPOSE`, `WORKDIR` instruction with its argument string (secrets redacted).

### 6.3 Script Enumeration

Enumerate every declared script:

- **npm/yarn/pnpm scripts** — every key in `package.json` `scripts`. Record the script name and the command string. Detect cross-scripts (`npm run build && npm run test`) and record the dependency.
- **Makefile targets** — every target per § 6.2.
- **Justfile recipes** — every recipe per § 6.2.
- **Rake tasks** — every `task :name` and `desc "..." task :name` declaration in `Rakefile` and `lib/tasks/*.rake`.
- **Gradle tasks** — every `tasks.register("name")` and `tasks.create("name")`; default tasks (build, test, assemble, check, clean) are recorded even if not declared.
- **Cargo aliases** — `[alias]` table in `.cargo/config.toml`.
- **Mix tasks** — `defp aliases` in `mix.exs` and any `defmodule Mix.Tasks.*`.
- **Go: no scripts** — record build commands inferred from `go.mod` (`go build ./...`, `go test ./...`).
- **Package script `pre*`/`post*` hooks** — `pretest`, `posttest`, `prepublish`, `postpublish`, `preinstall`, `postinstall` are recorded as lifecycle hooks with their triggering script.

### 6.4 Environment Variable Extraction

Enumerate every environment variable the system reads. The agent MUST scan both configuration files and source code for env-var reads.

- **Node.js** — `process.env.<NAME>` patterns; `process.env['NAME']` patterns; `process.env.NAME` patterns. The `define` block in `vite.config`/`webpack.config`/`next.config` declares compile-time constants that may shadow env vars.
- **Python** — `os.environ['NAME']`, `os.environ.get('NAME')`, `os.getenv('NAME')`; `pydantic` BaseSettings classes (each field is an env var by default).
- **Go** — `os.Getenv("NAME")`, `os.LookupEnv("NAME")`, `os.ExpandEnv`; `flag` package declarations; `envconfig` library struct tags.
- **Rust** — `std::env::var("NAME")`, `envy::from_env`; `envy` and `config` crate struct tags.
- **Java** — `System.getenv("NAME")`, `System.getProperty("name")`; Spring `@Value("${NAME}")`, `@ConfigurationProperties`.
- **Ruby** — `ENV['NAME']`, `ENV.fetch('NAME')`.
- **PHP** — `getenv('NAME')`, `$_ENV['NAME']`, `$_SERVER['NAME']`.
- **.NET** — `Environment.GetEnvironmentVariable("NAME")`, `IConfiguration` indexers.

For each env var, record `name`, `purpose` (inferred from the variable name and surrounding code per R21 — no invention), `default_value` (if declared in config), `required` (true|false|UNVERIFIED), `config_file` (where declared if at all), `read_sites` (list of `file:line` locations).

Cross-check `.env.example`, `.env.template`, `.env.sample` for declared env vars; these are the canonical "expected env vars" list. Any env var read in code but absent from the example is flagged `MISSING_FROM_EXAMPLE`.

### 6.5 Multi-Environment Configuration

Detect environment-specific configuration:

- **Node** — `.env.development`, `.env.staging`, `.env.production`, `.env.test`, `next.config.js` `env` block, `config/<env>.json`.
- **Python** — `settings/dev.py`, `settings/prod.py`, `config/<env>.yml`, `django-environ` patterns.
- **Spring** — `application.yml`, `application-<profile>.yml`, `application.properties`, `application-<profile>.properties`; `spring.profiles.active` declaration.
- **Rails** — `config/environments/*.rb` (development.rb, test.rb, production.rb).
- **Laravel** — `config/` directory; `.env` per environment.
- **Go** — `config/<env>.yaml` patterns; viper multi-file configs.
- **Rust** — `config/default.toml`, `config/<env>.toml` (config crate convention).

Record each environment variant with its file path and the variables it overrides relative to the default.

### 6.6 Feature Flag Detection

Detect feature flag systems by dependency and by code patterns.

- **Dependencies** — `launchdarkly`, `flagsmith`, `unleash`, `growthbook`, `@openshift/dynamic-plugin-sdk`, `posthog`, `statsig`.
- **Code patterns** — variables named `enable_*`, `ENABLE_*`, `FEATURE_*`, `ff_*`; `if (config.features.<name>)`; `@FeatureFlag` annotations; `Flags.<name>` enum references.
- **Config patterns** — `features:` block in YAML configs, `featureFlags` block in JSON configs.

Each flag is recorded with `name`, `default_value`, `config_source`, and `read_sites`. PROMPT_18 (Caching & Performance) uses feature-flag detection to identify gating logic.

### 6.7 Build Output Identification

Identify where the build writes its output:

- **TypeScript** — `tsconfig.json` `outDir` (default: `./dist` or `./build`).
- **Vite** — `vite.config` `build.outDir` (default: `dist`).
- **webpack** — `webpack.config` `output.path` (default: `dist`).
- **Next.js** — `.next/` (always).
- **Nuxt** — `.nuxt/` and `.output/`.
- **Rollup** — `rollup.config` `output.dir` or `output.file`.
- **Cargo** — `target/` (always; per `CARGO_TARGET_DIR` override).
- **Go** — current directory or explicit `-o` flag in build script.
- **Maven** — `target/` (always).
- **Gradle** — `build/libs/` for JARs, `build/distributions/` for distributions.
- **Python** — `dist/` (wheels), `build/` (intermediate); Poetry and hatch write to `dist/`.
- **.NET** — `bin/<Configuration>/<TargetFramework>/`.
- **Swift** — `.build/`.
- **Mix** — `_build/`.
- **Docker** — image tagged per `--tag` in the build script.

Each output location is recorded as a `CFG-XX` entry. If the output location is excluded per ART-01 EXC-04, the exclusion is cross-referenced.

### 6.8 CI/CD Pipeline Extraction

Detect CI/CD configurations and enumerate every pipeline step.

| CI System | File Marker |
|-----------|-------------|
| GitHub Actions | `.github/workflows/*.yml` |
| GitLab CI | `.gitlab-ci.yml` |
| CircleCI | `.circleci/config.yml` |
| Jenkins | `Jenkinsfile`, `jenkins/` |
| Travis CI | `.travis.yml` |
| Buildkite | `.buildkite/pipeline.yml`, `.buildkite/pipeline.sh` |
| Drone | `.drone.yml` |
| Azure Pipelines | `azure-pipelines.yml`, `pipelines/*.yml` |
| Bitbucket Pipelines | `bitbucket-pipelines.yml` |
| TeamCity | `.teamcity/` |
| Woodpecker | `.woodpecker.yml`, `.woodpecker/*.yml` |
| Argo Workflows | `.argo/` or `argoproj.io/v1alpha1 Workflow` manifests |
| Tekton | `.tekton/` |

For each pipeline file, record:

- `triggers` — push, pull_request, schedule, manual, tag.
- `jobs` (or `stages`) — each with `name`, `runs_on` (GitHub Actions `runs-on`), `steps` array.
- For each step: `name`, `uses` (action), `run` (inline shell), `env` (step-level env vars), `with` (action inputs).

The agent records every step's `name` and `run` command verbatim. Secrets referenced via `${{ secrets.X }}` are recorded with the secret name redacted as `secrets.<REDACTED>` per `OPERATING_RULES.md` and `MISSION.md` § 5.

### 6.9 Configuration Layering Analysis

Document the configuration precedence. For each ecosystem, record the precedence from lowest to highest:

- **Node (dotenv)** — process env > `.env.<env>.local` > `.env.<env>` > `.env.local` > `.env`.
- **Next.js** — process env > `.env.production.local` > `.env.production` > `.env.local` > `.env`.
- **Vite** — process env > `.env.<mode>.local` > `.env.<mode>` > `.env.local` > `.env`.
- **Spring Boot** — command-line args > environment variables > `application-<profile>.yml` > `application.yml` > defaults.
- **Rails** — `ENV` > `config/environments/<env>.rb` > `config/application.rb` > `config/initializers/*`.
- **Django** — `DJANGO_SETTINGS_MODULE` env var > settings module > defaults.
- **Python (pydantic-settings)** — init args > environment variables > `.env` file > defaults.
- **Go (viper)** — flags > env vars > config file > defaults.

Each layering chain is documented with a citation to the framework or library documentation that establishes it (marked `EXTERNAL` per `OUTPUT_RULES.md` § 6.4 where the citation is to a library doc).

---

## 7. Required Outputs

### ART-04 — Build & Configuration Map (Spec)

**Type:** Spec.

**Acceptance Criteria:**

- AC-04.1: The artifact file exists at `<output_root>/artifacts/phase1/ART04_<engagement_id>_build-config.md`.
- AC-04.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1.
- AC-04.3: The body contains: Executive Summary, Methodology, Build Tools, Build Configuration, Scripts, Environment Variables, Multi-Environment Configuration, Feature Flags, Build Outputs, CI/CD Pipelines, Configuration Layering, Traceability Index, Open Questions, Cross-References.
- AC-04.4: Every build tool has `name`, `version`, `config_files`, and `evidence_line`.
- AC-04.5: Every script has `name`, `command`, `manifest_path`, and `manifest_line_range`.
- AC-04.6: Every env var has `name`, `purpose`, `required`, `config_file`, and at least one `read_site`.
- AC-04.7: Every CI/CD pipeline has `system`, `file`, `triggers`, and `steps` array.
- AC-04.8: Every build output has `path` and `producing_tool`.
- AC-04.9: All claims cite `<file_path>:<line_range>`; library-documentation citations are marked `EXTERNAL`.

---

## 8. Output Templates

### 8.1 ART-04 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-04
artifact_type: Spec
producing_prompt: PROMPT_04
phase: 1
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
build_tools:
  - tool_id: BT-01
    name: <name>
    version: <version> | UNVERIFIED
    config_files: [<path>]
    evidence: <file_path>:<line-range>
scripts:
  - script_id: SCR-01
    name: <name>
    command: <string>
    manifest_path: <path>
    manifest_line_range: <start-end>
    lifecycle_hook: pre | post | none
    depends_on: [SCR-XX]
environment_variables:
  - id: ENV-01
    name: <NAME>
    purpose: <text>
    default_value: <value> | none | UNVERIFIED
    required: true | false | UNVERIFIED
    config_file: <path> | none
    read_sites:
      - <file_path>:<line-range>
    missing_from_example: true | false
feature_flags:
  - id: FF-01
    name: <name>
    default_value: <value>
    config_source: <path>
    read_sites: [<file_path>:<line-range>]
build_outputs:
  - id: OUT-01
    path: <relative-path>
    producing_tool: <BT-XX>
    excluded_per: EXC-04 | in-scope
ci_cd_pipelines:
  - pipeline_id: CICD-01
    system: <name>
    file: <path>
    triggers: [push | pull_request | schedule | manual | tag]
    jobs:
      - name: <job-name>
        runs_on: <runner>
        steps:
          - name: <step-name>
            uses: <action> | null
            run: <command> | null
            env: [<ENV-XX>]
configuration_layering:
  - ecosystem: <name>
    precedence: [lowest-to-highest: <layer>]
    evidence: EXTERNAL | <file_path>:<line-range>
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
items:
  - id: CFG-000001
    name: <config-item-name>
    type: build-tool | script | env-var | feature-flag | build-output | pipeline-step | layering-rule
    location: <file_path>:<line-range>
    attributes: { ... }
---
```

### 8.2 ART-04 Body Skeleton

```markdown
# ART-04: Build & Configuration Map

## 1. Executive Summary
## 2. Methodology
## 3. Build Tools
## 4. Build Configuration
   ### 4.1 <Tool 1> Configuration
   ### 4.2 <Tool 2> Configuration
## 5. Scripts
## 6. Environment Variables
## 7. Multi-Environment Configuration
## 8. Feature Flags
## 9. Build Outputs
## 10. CI/CD Pipelines
   ### 10.1 <Pipeline 1>
   ### 10.2 <Pipeline 2>
## 11. Configuration Layering
## 12. Traceability Index
## 13. Open Questions
## 14. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every `build-script` and `config` file in ART-03 is parsed; threshold ≥ 0.95.
- **Q2. Citation Check** — ≥ 0.95 of claims cited.
- **Q3. Schema Conformance Check** — front-matter validates.
- **Q4. Non-Contradiction Check** — build tools do not contradict ART-02's ecosystem declarations (e.g., `webpack` recorded here implies `webpack` in `devDependencies` of ART-02).
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` env-var `required` field has an Open Question.
- **Q6. Idempotence Spot-Check** — re-parsing a 5% sample of config files yields the same script/env-var set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-04.A. Script Enumeration Completeness** — every key in every `package.json` `scripts` block is recorded.
- **Q-04.B. Env-Var Source Coverage** — every `process.env.X` / `os.Getenv` / `System.getenv` read in the in-scope source set is captured as a `read_site`.
- **Q-04.C. CI/CD Step Verbatim** — every pipeline step's `run` command is recorded verbatim (no paraphrasing).
- **Q-04.D. Build Output Cross-Reference** — every `build_outputs` entry is either excluded per ART-01 EXC-04 or marked `in-scope` with rationale.
- **Q-04.E. Layering Citation** — every layering chain cites either the framework's documentation (`EXTERNAL`) or a config file that establishes the chain.
- **Q-04.F. Secret Redaction** — no `${{ secrets.X }}` value appears unredacted; the redaction marker `secrets.<REDACTED>` is used.

---

## 10. Common Pitfalls

- Do not document the resolved value of a secret; redact per `MISSION.md` § 5 and `OPERATING_RULES.md` R21.
- Do not paraphrase CI/CD step commands; record them verbatim so the pipeline can be reproduced.
- Always distinguish a declared script from a built-in tool command; `tsc` invoked in a `package.json` script is recorded as a script entry, but `tsc` itself is recorded as a build tool.
- Do not invent env-var purposes; if the variable name does not reveal the purpose and no code comment or `.env.example` clarifies, mark `purpose: UNVERIFIED` and emit an Open Question.
- Always record the `pre*`/`post*` lifecycle hook relationship; downstream prompts (PROMPT_05, PROMPT_12) use these to understand initialization ordering.
- Do not skip `Makefile` targets that begin with `.` (e.g., `.PHONY`); these are special targets and are recorded with `kind: special-target`.
- Always cross-check feature flags between config and code; a flag declared in config but never read in code is `UNUSED_FLAG`; a flag read in code but absent from config is `MISSING_DEFAULT`.
- Do not infer Docker build arguments from the `Dockerfile` alone; check the build script (e.g., `docker buildx build --build-arg X=Y`) for the values actually passed at build time.
- Always record the precedence order for multi-environment configs; an env var overridden by `.env.production` is a different value at production than at development, and PROMPT_17 (Error Handling & Resilience) needs this distinction.
- Do not collapse multi-environment configs into a single "config" entry; each environment is a distinct `CFG-XX` with its overrides listed.

---

## 11. Handoff Criteria

PROMPT_05 and PROMPT_26 consume ART-04. Handoff requires ALL of:

- HC-04.1: ART-04 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-04.2: Every build tool detected has a configuration file parsed (or `none` recorded with rationale).
- HC-04.3: Every script in every `package.json`/`Makefile`/`justfile`/`Rakefile`/`pom.xml`/`build.gradle` is recorded.
- HC-04.4: Every env var read in the in-scope source set is recorded.
- HC-04.5: Every CI/CD pipeline file in the in-scope set is parsed with every step enumerated.
- HC-04.6: Build output locations are recorded and cross-referenced against ART-01 exclusions.
- HC-04.7: `repository_fingerprint_recheck` matches ART-01.
- HC-04.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_05 (Entry Points — uses scripts and build outputs to identify entry-point candidates and bootstrap artifacts), PROMPT_26 (Rebuild Guide — uses the entire ART-04 to write the rebuild instructions).
- **Depends on:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-03 (PROMPT_03).
- **Governing rules:** `OPERATING_RULES.md` R13, R16, R17, R19, R21 (no invented env-var purposes).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1; Spec bar (aggregate ≥ 32, Accuracy ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 3.1, § 4, § 6.
- **Forward reference:** PROMPT_30 will spot-check that every `process.env.*` read in the source set appears in ART-04's env-var inventory.

*End of PROMPT_04. Orchestrator may dispatch PROMPT_05 upon satisfaction of § 11.*
