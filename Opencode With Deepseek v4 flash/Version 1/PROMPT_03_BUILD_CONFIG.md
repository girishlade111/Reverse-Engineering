# PROMPT_03 — Phase 02: Build & Configuration Analysis

## PHASE CLASS: Infrastructure Survey
## DEPENDENCIES: PROMPT_02 (Structure) — complete
## OUTPUT DIRECTORY: `re-docs/02-build-config/`

---

## OBJECTIVE

Analyze every build system, package manager, task runner, and configuration system in the repository. Understand how the project is built, tested, linted, formatted, and deployed from a configuration perspective.

## PREREQUISITES

- [ ] PROMPT_02 completed
- [ ] Folder structure is understood
- [ ] `re-docs/00-scouting/05-config-files.md` exists

## INPUTS

- All build files (package.json, build.gradle, Cargo.toml, CMakeLists.txt, Makefile, pyproject.toml, etc.)
- All config files (tsconfig.json, .eslintrc, .prettierrc, etc.)
- All Docker/docker-compose files
- CI/CD configuration files (.github/workflows/, .gitlab-ci.yml, Jenkinsfile, etc.)

## ANALYSIS STEPS

### Step 1: Build System Identification

Identify the primary build system(s):

| Ecosystem | Build File | Purpose |
|-----------|-----------|---------|
| Node.js | package.json, tsconfig.json | Dependencies, scripts, TypeScript config |
| Python | pyproject.toml, setup.py, requirements.txt | Dependencies, build config |
| Rust | Cargo.toml | Dependencies, build config |
| Go | go.mod | Dependencies, module config |
| Java | build.gradle, pom.xml | Dependencies, build config |
| .NET | .csproj, .sln | Dependencies, build config |
| Ruby | Gemfile | Dependencies |
| C/C++ | CMakeLists.txt, Makefile | Build config |
| Multi | Makefile, taskfile | Task orchestration |

For each build system found, document:
- Build file location
- Build tool name and version
- Purpose (building, testing, packaging, etc.)
- All defined scripts/targets
- Build dependencies (dev dependencies vs runtime)

### Step 2: Build Scripts Deep Dive

For each build file, extract and document every script/target:

For package.json (Node.js):
```json
{
  "scripts": {
    "dev": "next dev",         // Development server
    "build": "next build",     // Production build
    "start": "next start",    // Production start
    "test": "jest",           // Test runner
    "lint": "eslint ."        // Linter
  }
}
```

For each script, document:
- Script name
- Full command
- Purpose
- When it's run (development, CI, production)
- Dependencies it relies on (other scripts)

### Step 3: TypeScript/Type System Configuration

If TypeScript or similar typed language:
- Read tsconfig.json or equivalent
- Document: target, module, strict mode, paths, aliases, includes/excludes
- Assess strictness level

### Step 4: Linting & Formatting Configuration

Analyze all linting and formatting configurations:
- ESLint (.eslintrc)
- Prettier (.prettierrc)
- Ruff (pyproject.toml)
- rustfmt (rustfmt.toml)
- Other

Document:
- Tool name
- Rule set (recommended, custom)
- Key rules (non-default rules)
- Severity levels

### Step 5: Test Configuration

Analyze test configuration:
- Test framework (Jest, Vitest, pytest, cargo test, etc.)
- Test file patterns
- Coverage configuration
- Test environment setup

### Step 6: Docker Configuration (if present)

If Docker files exist:
- Read all Dockerfile(s)
- Read docker-compose.yml (if exists)
- Document:
  - Base images used
  - Multi-stage build structure
  - Build arguments
  - Service definitions
  - Network configuration
  - Volume mounts
  - Environment variables

### Step 7: CI/CD Configuration

Analyze all CI/CD configurations:

- **GitHub Actions**: `.github/workflows/*.yml`
- **GitLab CI**: `.gitlab-ci.yml`
- **Jenkins**: `Jenkinsfile`
- **CircleCI**: `.circleci/config.yml`
- **Other**: custom

For each CI/CD pipeline, document:
- Trigger events (push, PR, schedule)
- Jobs and their dependencies
- Steps within each job
- Caching strategy
- Artifact management
- Deployment targets
- Secrets/environment variables used

### Step 8: Task Runners (if present)

- Makefile targets
- Justfile recipes
- Taskfile.yml tasks
- Grunt/Gulp configurations

## OUTPUT SPECIFICATION

### File 1: `01-build-system.md`

Complete build system documentation.

### File 2: `02-scripts-and-targets.md`

Complete list of all scripts/targets with descriptions.

### File 3: `03-lint-and-format.md`

Linting and formatting configuration documentation.

### File 4: `04-test-configuration.md`

Test configuration documentation.

### File 5: `05-docker-config.md` (if applicable)

Docker configuration documentation.

### File 6: `06-cicd-pipelines.md`

CI/CD pipeline documentation.

### File 7: `07-build-config-summary.md`

Summary of the build system, including:
- Build commands for common tasks
- Test commands
- Lint commands
- Build time estimates
- Configuration complexity assessment

## CHEAT SHEET OUTPUT

Generate a build cheat sheet:

```markdown
## Build Cheat Sheet

| Task | Command |
|------|---------|
| Install dependencies | `npm install` |
| Development server | `npm run dev` |
| Build | `npm run build` |
| Test | `npm run test` |
| Lint | `npm run lint` |
| Format | `npm run format` |
| Docker build | `docker build -t app .` |
```

## VALIDATION CHECKS

- [ ] Build system identified for every language in the project
- [ ] Every script/target in every build file documented
- [ ] Linting and formatting configuration documented
- [ ] Test configuration documented
- [ ] Docker configuration documented (if present)
- [ ] CI/CD pipeline documented (if present)
- [ ] Build commands are known and documented

## COMPLETION CHECKLIST

- [ ] All output files generated
- [ ] Build cheat sheet generated
- [ ] Build system complexity assessed
- [ ] All outputs saved to `re-docs/02-build-config/`
- [ ] Phase validation checks passed

---

*Proceed to PROMPT_04_DEPENDENCIES.md only after all checklist items are complete.*
