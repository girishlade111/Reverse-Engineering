# PROMPT_01: Repository Discovery & Structure Mapping

## Classification
- **Domain:** Discovery & Intake
- **Phase:** 1 — Initial Repository Analysis
- **Prerequisites:** None (Entry Point)
- **Dependencies:** File system access to the target repository
- **Estimated Effort:** Proportional to repository size (avg: 100-500 files analyzed)

---

## Objective

Perform a comprehensive initial scan of the target repository to establish a complete inventory of all files, directories, and structural artifacts. This forms the foundation for all subsequent analysis phases.

---

## Input Requirements

### Required Context
- Path to the target repository root directory
- File system read access to all files in the repository
- No prior knowledge of the repository is required

### Required Files
- None — this is the entry point prompt

---

## Pre-Analysis Checklist

- [ ] Repository path is accessible
- [ ] File system permissions allow reading all files
- [ ] No symlinks or external mounts that might cause infinite recursion
- [ ] Working directory is the repository root

---

## Analysis Tasks

### Task 1: Recursive File Inventory

**Purpose:** Establish a complete, ordered inventory of every file in the repository.

**Instructions:**
1. Recursively list all files in the repository root directory
2. For each file, capture:
   - Full relative path from repository root
   - File name
   - File extension
   - File size in bytes
   - Last modified date
   - File type classification (source, config, documentation, binary, data, media, etc.)
3. Organize the inventory hierarchically by directory structure
4. Exclude: `.git/`, `node_modules/`, `__pycache__/`, `.venv/`, `venv/`, `dist/`, `build/`, `.next/`, and other standard build/generated directories
5. Count total files, total lines (estimated), and total size

**Success Criteria:**
- Every file in the repository (excluding build artifacts) is listed
- Files are organized hierarchically
- Metadata is complete for every file
- Totals are calculated and reported

**Output Format:**

```markdown
## File Inventory

### Repository Overview
- **Root Path:** /path/to/repo
- **Total Files:** 1,234
- **Total Directories:** 56
- **Total Size:** 45.2 MB
- **Estimated Lines:** 125,000

### Directory Structure

```
repo-root/
├── src/
│   ├── main.py
│   ├── utils/
│   │   ├── helpers.py
│   │   └── validators.py
│   └── ...
├── tests/
│   ├── test_main.py
│   └── ...
├── docs/
│   ├── README.md
│   └── ...
└── config/
    ├── settings.json
    └── ...
```

### File Classification Summary

| Category | Count | Total Size | Example Extensions |
|----------|-------|------------|-------------------|
| Source Code | 450 | 15 MB | .py, .js, .ts, .java |
| Configuration | 45 | 2 MB | .json, .yaml, .toml |
| Documentation | 30 | 5 MB | .md, .rst, .txt |
| Data/Assets | 200 | 20 MB | .csv, .json, .xml |
| Test Files | 80 | 1.5 MB | test_*.py, *.spec.ts |
| Build/Scripts | 25 | 500 KB | Dockerfile, Makefile |
| Other | 4 | 200 KB | .gitignore, .env |

### Detailed File Inventory (by directory)

#### `/src/` (Source Code)
| # | File | Ext | Size | Lines | Type | Last Modified |
|---|------|-----|------|-------|------|---------------|
| 1 | src/main.py | .py | 12KB | 350 | Source | 2024-01-15 |
| 2 | src/utils/helpers.py | .py | 8KB | 220 | Source | 2024-01-14 |
| ... | ... | ... | ... | ... | ... | ... |

#### `/tests/` (Test Files)
[...]

---

### Task 2: Repository Metadata Extraction

**Purpose:** Extract all repository-level metadata files and their contents.

**Instructions:**
1. Locate and read all repository root metadata files:
   - `README.md`, `CONTRIBUTING.md`, `LICENSE`, `CHANGELOG.md`
   - `.gitignore`, `.gitattributes`, `.editorconfig`
   - `package.json`, `setup.py`, `Cargo.toml`, `pom.xml`, etc.
   - Dockerfile, docker-compose.yml
   - Makefile, Justfile, Taskfile
2. Extract key information from each:
   - Project name, description, version
   - Author/maintainer information
   - License type
   - Build instructions
   - Development setup instructions
   - Architecture overview (if provided in README)
3. Note any discrepancies between metadata and actual repository contents

**Success Criteria:**
- All metadata files are identified and read
- Key information is extracted and structured
- Discrepancies between metadata and reality are noted

**Output Format:**

```markdown
## Repository Metadata

### Project Identity
- **Name:** [From metadata]
- **Description:** [From README]
- **Version:** [From package.json, setup.py, etc.]
- **License:** [From LICENSE file]
- **Author:** [From metadata]

### Build & Setup
- **Build System:** [e.g., npm, pip, cargo, maven]
- **Build Command:** [e.g., npm run build, make build]
- **Test Command:** [e.g., npm test, pytest]
- **Run Command:** [e.g., npm start, python main.py]

### Key Files Found
| File | Exists | Key Content |
|------|--------|-------------|
| README.md | Yes | [Brief summary of README content] |
| LICENSE | Yes | MIT License |
| package.json | Yes | React 18, Express 4 |
| Dockerfile | No | - |

### Discrepancies Noted
- README mentions a `config.yaml` file that doesn't exist
- package.json version (2.0.0) differs from git tag (1.9.0)
```

---

### Task 3: Technology Stack Identification (Initial)

**Purpose:** Perform an initial identification of the technology stack based on file extensions and metadata.

**Instructions:**
1. Aggregate all file extensions found in the repository
2. Count frequency of each extension
3. Map extensions to likely programming languages
4. Identify potential frameworks from:
   - Configuration files (package.json -> React, Vue, Angular)
   - Import patterns (common imports suggest frameworks)
   - Directory structure conventions (src/ -> typical; app/ -> typical)
5. Identify build tools from metadata (webpack, vite, poetry, etc.)
6. Note any unusual or rare file extensions

**Success Criteria:**
- All file extensions are cataloged
- Languages are identified with confidence levels
- Frameworks are hypothesized (to be confirmed in PROMPT_02)

**Output Format:**

```markdown
## Initial Technology Stack

### Language Distribution

| Language | Extension | File Count | Lines Estimate | Confidence |
|----------|-----------|------------|----------------|------------|
| Python | .py | 150 | 45,000 | CONFIRMED |
| TypeScript | .ts | 80 | 24,000 | CONFIRMED |
| JavaScript | .js | 45 | 12,000 | CONFIRMED |
| HTML | .html | 20 | 5,000 | CONFIRMED |
| CSS | .css | 15 | 3,000 | CONFIRMED |
| YAML | .yml, .yaml | 12 | 500 | CONFIRMED |

### Framework Hypotheses
| Framework | Evidence | Confidence |
|-----------|----------|------------|
| React | package.json: react, react-dom | CONFIRMED |
| Express | package.json: express | CONFIRMED |
| FastAPI | requirements.txt: fastapi | INFERRED |

### Build Tools
| Tool | Evidence | Purpose |
|------|----------|---------|
| webpack | webpack.config.js | Module bundling |
| poetry | pyproject.toml | Python packaging |

### Notable Observations
- 15 files with `.generated` extension (auto-generated code)
- 3 files with `.proto` extension (Protocol Buffers)
```

---

### Task 4: Repository Health Assessment

**Purpose:** Perform a quick assessment of the repository's health and maintainability.

**Instructions:**
1. Check for presence of essential files:
   - README, LICENSE, CONTRIBUTING, CHANGELOG
   - .gitignore, .editorconfig
   - CI/CD configuration (.github/workflows, .gitlab-ci.yml, Jenkinsfile)
   - Test directory presence
   - Linting configuration (.eslintrc, .pylintrc, etc.)
2. Assess documentation coverage:
   - Are there inline comments?
   - Are there docstrings/JSDoc?
   - Is there API documentation?
3. Check code organization:
   - Is there a consistent directory structure?
   - Are naming conventions followed?
   - Are there overly large files (>1000 lines)?
4. Identify potential risk areas:
   - Deprecated dependencies
   - Large untested modules
   - Missing error handling patterns
   - Hardcoded values

**Success Criteria:**
- Health assessment covers all categories
- Risk areas are identified and documented
- Assessment is objective (based on evidence, not opinion)

**Output Format:**

```markdown
## Repository Health Assessment

### Essential Files
| File | Status | Notes |
|------|--------|-------|
| README.md | Present | Well-maintained |
| LICENSE | Present | MIT |
| CONTRIBUTING.md | Missing | - |
| .gitignore | Present | Standard |
| CI/CD | Present | GitHub Actions |

### Documentation Health
- **Code Comments:** Good (consistent docstrings)
- **API Documentation:** Partial (some endpoints documented)
- **Architecture Documentation:** Minimal
- **Setup Instructions:** Complete

### Code Organization
- **Structure:** Clean (well-organized modules)
- **Naming:** Consistent (snake_case for Python)
- **Large Files:** 3 files > 1000 lines (flagged for review)
- **Nesting Depth:** Average 3 levels (acceptable)

### Risk Areas
| Area | Risk Level | Details |
|------|------------|---------|
| Deprecated packages | LOW | `requests@2.28.0` (latest: 2.31.0) |
| Untested modules | MEDIUM | `src/legacy/` has no tests |
| Hardcoded values | MEDIUM | API keys in `config/local.py` |
| Large functions | LOW | 5 functions > 100 lines |
```

---

## Synthesis

**Purpose:** Combine all task outputs into a unified repository profile.

**Instructions:**
1. Create a summary profile of the repository
2. Identify the most important directories and files for deeper analysis
3. Note any immediate concerns or areas requiring special attention
4. Prepare context for PROMPT_02 (Language Detection)

**Synthesis Output:**

```markdown
## Repository Profile

### Profile Summary
- **Repository:** [Name] v[Version]
- **Primary Language:** [Language] (CONFIRMED)
- **Secondary Languages:** [List]
- **Primary Framework:** [Framework] (CONFIRMED)
- **Repository Size:** [Files/Dirs/Size]
- **Estimated Complexity:** [Low/Medium/High]
- **Code Quality:** [Excellent/Good/Fair/Poor]
- **Documentation Quality:** [Excellent/Good/Fair/Poor]

### Key Areas for Deep Analysis
1. `/src/core/` — Main business logic (largest module)
2. `/src/api/` — API layer (external interface)
3. `/src/data/` — Data access layer (critical path)

### Immediate Concerns
- [ ] Missing CONTRIBUTING.md
- [ ] 3 files exceed 1000 lines
- [ ] Untested legacy module

### Context for Next Prompt
- Language candidates: Python (primary), TypeScript (secondary)
- Frameworks to confirm: FastAPI, React
- Package managers: npm, poetry
```

---

## Output Requirements

### Required Deliverables
1. Complete file inventory with hierarchical structure
2. Extracted repository metadata
3. Initial technology stack identification
4. Repository health assessment
5. Repository profile summary

### Output Structure
```
REPO_DISCOVERY/
├── file_inventory.md
├── repository_metadata.md
├── tech_stack_initial.md
├── health_assessment.md
└── profile_summary.md
```

---

## Quality Checks

- [ ] All files (excluding build artifacts) are listed in inventory
- [ ] File metadata is complete for every entry
- [ ] All metadata files have been read and analyzed
- [ ] Language identification is based on evidence
- [ ] Health assessment is objective and evidence-based
- [ ] Gaps are documented with [GAP] markers
- [ ] Context for PROMPT_02 is prepared

---

## Continuation Rules

If the repository is very large (>5000 files):
1. Complete Task 1 (File Inventory) at directory level (not per-file)
2. Complete Task 2 (Metadata) fully
3. Complete Task 3 (Tech Stack) at directory level
4. Complete Task 4 (Health) at high level
5. Note that deep per-file analysis will be done in subsequent prompts

---

## Cross-References

- **Next Prompt:** `PROMPT_02_LANGUAGE_DETECTION.md`
- **Related Context:** File inventory feeds into language detection
- **Shared Context Key:** `discovery.file_inventory`, `discovery.tech_stack_initial`
