# PROMPT_02: Language, Framework & Version Detection

## Classification
- **Domain:** Discovery & Intake
- **Phase:** 1 — Initial Repository Analysis
- **Prerequisites:** PROMPT_01 (File Inventory, Tech Stack Initial)
- **Dependencies:** File inventory from PROMPT_01
- **Estimated Effort:** Medium (50-200 files to deep-scan)

---

## Objective

Perform definitive identification of all programming languages, frameworks, libraries, runtime environments, and their versions used in the repository. Move from initial hypotheses to confirmed identifications.

---

## Input Requirements

### Required Context
- Complete file inventory from PROMPT_01
- Initial technology stack hypotheses
- Repository metadata (package files, config files)

### Required Files
- All package/dependency manifest files
- All build configuration files
- All language-specific configuration files
- Sample source files from each language type

---

## Pre-Analysis Checklist

- [ ] PROMPT_01 completed and context loaded
- [ ] File inventory available with extension analysis
- [ ] All manifest files identified (package.json, requirements.txt, etc.)
- [ ] Build config files identified

---

## Analysis Tasks

### Task 1: Language Confirmation via Deep Scanning

**Purpose:** Confirm identified languages and detect any languages missed during initial scanning.

**Instructions:**
1. For each file extension found in PROMPT_01:
   - Read 3-5 representative files of that extension
   - Verify the language based on syntax patterns, not just extension
   - Note files where extension doesn't match content (e.g., .js file containing TypeScript)
2. Detect language features:
   - Static vs dynamic typing
   - Object-oriented vs functional patterns
   - Compiled vs interpreted
   - Garbage collected vs manual memory management
3. Identify language versions:
   - Check shebang lines (`#!/usr/bin/env python3`)
   - Check version pragmas (`// @ts-check`, `# -*- coding: utf-8 -*-`)
   - Check target version in build configs
4. Detect preprocessor languages:
   - TypeScript -> JavaScript
   - SCSS/SASS -> CSS
   - JSX/TSX -> JavaScript/TypeScript
   - Pug/Jade -> HTML

**Success Criteria:**
- Every language used in the repository is identified
- Language versions are determined where possible
- Preprocessor relationships are mapped
- Any misidentified languages are corrected

**Output Format:**

```markdown
## Language Analysis

### Primary Languages
| Language | Version | Usage % | Total Files | Total Lines | Transpiles To | Confidence |
|----------|---------|---------|-------------|-------------|---------------|------------|
| Python | 3.11+ | 45% | 150 | 45,000 | - | CONFIRMED |
| TypeScript | 5.0+ | 25% | 80 | 24,000 | JavaScript | CONFIRMED |

### Secondary Languages
| Language | Version | Usage % | Total Files | Purpose | Confidence |
|----------|---------|---------|-------------|---------|------------|
| JavaScript | ES2022 | 10% | 45 | Legacy code | CONFIRMED |
| SQL | - | 5% | 20 | Database queries | CONFIRMED |

### Preprocessor Chain
| Source Language | Output Language | Compiler/Transpiler | Configuration |
|----------------|----------------|-------------------|---------------|
| TypeScript (.ts) | JavaScript (.js) | tsc | tsconfig.json |
| SCSS (.scss) | CSS (.css) | sass | - |

### Language Feature Summary
- **Typing:** Static (TypeScript), Dynamic (Python)
- **Paradigm:** Multi-paradigm (OOP, Functional)
- **Concurrency:** Async/await (Python, JS), Threading (Python)
- **Memory:** Garbage collected (all)

### Edge Cases Found
- `data/migrations/` contains SQL embedded in Python strings
- `legacy/` contains Python 2.7 code (flagged for migration)
```

---

### Task 2: Framework & Library Deep Detection

**Purpose:** Identify all frameworks, libraries, and their specific versions with high confidence.

**Instructions:**
1. For each identified language, scan for framework-specific patterns:
   - Import/require/include statements
   - Class inheritance from framework base classes
   - Decorator/annotation patterns
   - Configuration objects
   - Convention-based file organization
2. Cross-reference with manifest files:
   - package.json -> npm packages and versions
   - requirements.txt/Pipfile -> PyPI packages
   - Cargo.toml -> Rust crates
   - pom.xml/build.gradle -> Maven/Gradle dependencies
   - go.mod -> Go modules
3. Identify framework versions:
   - Check lock files (package-lock.json, yarn.lock, poetry.lock)
   - Check version constraints in manifest files
   - Import framework modules and check version attributes
4. Identify major and minor frameworks:
   - Major: Full application frameworks (Django, Spring, React)
   - Minor: Utility libraries (lodash, moment, requests)
   - Micro: Single-purpose libraries
5. Detect framework usage patterns:
   - How much of the framework's feature set is used?
   - Are there custom extensions or wrappers?
   - Is the framework used directly or through abstractions?

**Success Criteria:**
- All frameworks are identified with version numbers
- Framework usage patterns are documented
- Direct vs. transitive dependencies are distinguished
- Framework version compatibility is assessed

**Output Format:**

```markdown
## Framework & Library Analysis

### Major Frameworks
| Framework | Language | Version | Usage Type | Files Affected | Confidence |
|-----------|----------|---------|------------|----------------|------------|
| FastAPI | Python | 0.104+ | Full | 35 | CONFIRMED |
| React | TypeScript | 18.2+ | Full | 40 | CONFIRMED |
| Express | JavaScript | 4.18+ | Full | 15 | CONFIRMED |

### Minor Libraries
| Library | Language | Version | Purpose | Files Affected | Confidence |
|---------|----------|---------|---------|----------------|------------|
| SQLAlchemy | Python | 2.0+ | ORM | 20 | CONFIRMED |
| lodash | TypeScript | 4.17+ | Utilities | 10 | CONFIRMED |
| pytest | Python | 7.4+ | Testing | 25 | CONFIRMED |

### Framework Usage Depth
| Framework | Features Used | Features Available | Usage Ratio |
|-----------|---------------|-------------------|-------------|
| FastAPI | Routing, validation, dependencies, middleware | Full framework | 60% |
| React | Components, hooks, context, router | Full framework | 45% |

### Custom Framework Extensions
| Extension | Base Framework | Purpose | Location |
|-----------|---------------|---------|----------|
| CustomRouter | Express | Adds auth middleware | src/middleware/auth.py |
| BaseModelExt | SQLAlchemy | Adds timestamp fields | src/models/base.py |

### Version Compatibility Notes
- FastAPI 0.104 requires Python 3.8+ (satisfied)
- React 18.2 requires Node 14+ (confirmed in .nvmrc)
- Two libraries have deprecated versions (flagged)
```

---

### Task 3: Runtime & Environment Detection

**Purpose:** Identify all runtime environments, interpreters, and execution contexts.

**Instructions:**
1. Detect runtime environments:
   - Node.js version (.nvmrc, .node-version, engines in package.json)
   - Python version (.python-version, runtime.txt, pyproject.toml)
   - Java version (pom.xml, build.gradle)
   - Go version (go.mod)
   - Rust version (rust-toolchain.toml)
2. Detect execution environments:
   - Browser (client-side JavaScript detection)
   - Server (Node.js, Python WSGI/ASGI)
   - CLI (command-line tools)
   - Mobile (React Native, Flutter)
   - Desktop (Electron, Tauri)
3. Detect containerization:
   - Dockerfile
   - docker-compose.yml
   - .dockerignore
4. Detect CI/CD platforms:
   - GitHub Actions (.github/workflows/)
   - GitLab CI (.gitlab-ci.yml)
   - Jenkins (Jenkinsfile)
   - Circle CI (.circleci/config.yml)

**Success Criteria:**
- All runtime environments are identified with versions
- Execution context (browser, server, CLI) is determined
- Container and CI/CD configurations are documented

**Output Format:**

```markdown
## Runtime & Environment Analysis

### Runtime Environments
| Runtime | Version | Source | Used By | Confidence |
|---------|---------|--------|---------|------------|
| Node.js | 18.x | .nvmrc | Frontend, API server | CONFIRMED |
| Python | 3.11.x | .python-version | Backend, scripts | CONFIRMED |
| PostgreSQL | 15 | docker-compose.yml | Database | CONFIRMED |

### Execution Contexts
| Context | Description | Entry Points |
|---------|-------------|--------------|
| Web Server | FastAPI ASGI server | src/main.py |
| Web Client | React SPA | src/client/index.tsx |
| CLI Tools | Python CLI scripts | scripts/ |
| Background Jobs | Celery workers | src/workers/ |

### Containerization
| File | Purpose | Base Image |
|------|---------|------------|
| Dockerfile.api | API server | python:3.11-slim |
| Dockerfile.web | Web client | node:18-alpine |
| docker-compose.yml | Multi-service orchestration | - |

### CI/CD Configuration
| Platform | Configuration Location | Triggers |
|----------|----------------------|----------|
| GitHub Actions | .github/workflows/ | Push, PR, Tag |
| - CI: test.yml | Tests | Push, PR |
| - CD: deploy.yml | Deployment | Tag v* |
```

---

### Task 4: Version Compatibility & Constraint Analysis

**Purpose:** Analyze version constraints and compatibility across the technology stack.

**Instructions:**
1. Build a version compatibility matrix:
   - Language runtime vs framework requirements
   - Framework vs library dependencies
   - Transitive dependency version conflicts
2. Identify version constraint issues:
   - Mismatched versions between manifest and usage
   - Deprecated packages or versions
   - Security vulnerabilities (from known databases)
   - Breaking changes in upstream dependencies
3. Check lockfile integrity:
   - Does package-lock.json match package.json?
   - Are there any lockfile conflicts?
4. Assess upgrade paths:
   - Identify major version gaps
   - Note any blocking issues for upgrades

**Success Criteria:**
- Version compatibility matrix is complete
- All constraints and conflicts are documented
- Upgrade paths are assessed for major components

**Output Format:**

```markdown
## Version Compatibility Analysis

### Compatibility Matrix
| Component | Version | Requires | Status |
|-----------|---------|----------|--------|
| Python | 3.11.5 | FastAPI >= 0.100 | Compatible |
| FastAPI | 0.104.0 | Python >= 3.8 | Compatible |
| SQLAlchemy | 2.0.23 | Python >= 3.7 | Compatible |
| Node.js | 18.18.0 | React >= 17 | Compatible |
| React | 18.2.0 | Node >= 14 | Compatible |

### Known Conflicts
| Conflict | Components | Severity | Resolution |
|----------|------------|----------|------------|
| None found | - | - | - |

### Deprecated Packages
| Package | Current Version | Latest Version | Age | Risk |
|---------|----------------|----------------|-----|------|
| moment | 2.29.4 | 2.30.1 | 1yr | LOW |
| requests | 2.28.0 | 2.31.0 | 8mo | LOW |

### Security Vulnerabilities
| Package | Version | CVE | Severity | Fix Available |
|---------|---------|-----|----------|---------------|
| None found (based on manifest analysis) |

### Upgrade Assessment
| Component | Current | Latest Stable | Upgrade Effort | Breaking Changes |
|-----------|---------|---------------|----------------|------------------|
| FastAPI | 0.104.0 | 0.109.0 | LOW | Minor API changes |
| React | 18.2.0 | 18.2.0 | NONE | - |
| Python | 3.11.5 | 3.12.1 | MEDIUM | EOL 3.12 changes |
```

---

## Synthesis

**Purpose:** Create a definitive technology stack profile for the repository.

**Instructions:**
1. Combine all task outputs into a unified technology profile
2. Create a visual technology stack diagram
3. Identify the most impactful technologies for architecture decisions
4. Prepare context for PROMPT_03 (Dependency Mapping)

**Output Format:**

```markdown
## Technology Stack Profile

### Stack Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    FRONTEND (React 18)                    │
│  TypeScript 5 │ React Router │ Redux │ Axios │ SCSS     │
└────────────────────┬────────────────────────────────────┘
                     │ HTTP/REST
┌────────────────────▼────────────────────────────────────┐
│                     API GATEWAY                          │
│                  Express 4 (Node 18)                     │
└────────────────────┬────────────────────────────────────┘
                     │ Internal RPC
┌────────────────────▼────────────────────────────────────┐
│                   BACKEND SERVICES                       │
│    FastAPI 0.104 (Python 3.11) │ Celery │ Redis         │
└────────────────────┬────────────────────────────────────┘
                     │ SQLAlchemy ORM
┌────────────────────▼────────────────────────────────────┐
│                     DATA LAYER                           │
│     PostgreSQL 15 │ Redis 7 │ SQLAlchemy 2.0            │
└─────────────────────────────────────────────────────────┘
```

### Confirmed Technology Stack
| Layer | Technology | Version | Confidence |
|-------|------------|---------|------------|
| Frontend Framework | React | 18.2.0 | CONFIRMED |
| Frontend Language | TypeScript | 5.0+ | CONFIRMED |
| API Gateway | Express | 4.18.0 | CONFIRMED |
| Backend Framework | FastAPI | 0.104.0 | CONFIRMED |
| Backend Language | Python | 3.11+ | CONFIRMED |
| ORM | SQLAlchemy | 2.0+ | CONFIRMED |
| Database | PostgreSQL | 15 | CONFIRMED |
| Cache | Redis | 7+ | CONFIRMED |
| Task Queue | Celery | 5.3+ | CONFIRMED |
| Testing | pytest | 7.4+ | CONFIRMED |

### Context for Next Prompt
- Package managers: npm, poetry, pip
- Number of direct dependencies: 45 (npm), 25 (pip)
- Dependency graph complexity: MEDIUM
- Key dependencies to analyze: FastAPI, React, SQLAlchemy
```

---

## Output Requirements

### Required Deliverables
1. Definitive language identification report
2. Framework and library catalog with versions
3. Runtime environment specification
4. Version compatibility and constraint analysis
5. Technology stack profile with diagram

### Output Structure
```
LANGUAGE_ANALYSIS/
├── language_confirmed.md
├── framework_catalog.md
├── runtime_spec.md
├── compatibility_analysis.md
└── tech_stack_profile.md
```

---

## Quality Checks

- [ ] Every language is confirmed by reading representative files, not just extensions
- [ ] Framework versions are extracted from lock files or version attributes
- [ ] Runtime environments are identified with version pins
- [ ] Version compatibility matrix is complete and accurate
- [ ] All deprecated or vulnerable packages are flagged
- [ ] Context for PROMPT_03 is prepared
- [ ] No unverified language claims remain

---

## Continuation Rules

If the repository uses more than 10 languages:
1. Focus on languages with >1% file count contribution
2. Document rare languages in a separate "Minor Languages" section
3. Ensure all languages are at least listed, even if not deeply analyzed

---

## Cross-References

- **Previous Prompt:** `PROMPT_01_REPO_DISCOVERY.md`
- **Next Prompt:** `PROMPT_03_DEPENDENCY_MAPPING.md`
- **Related Context:** Language list feeds dependency scanning scope
- **Shared Context Key:** `discovery.languages`, `discovery.frameworks`
