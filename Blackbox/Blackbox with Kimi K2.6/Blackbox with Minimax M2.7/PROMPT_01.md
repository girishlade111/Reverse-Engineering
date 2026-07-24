# Phase 1: Repository Init & Discovery

> **Document:** PROMPT_01.md  
> **Phase:** 1 of 10  
> **Purpose:** Initialize the reverse engineering process by discovering and cataloging the repository  
> **Prerequisite:** MASTER_PROMPT.md, MISSION.md, OPERATING_RULES.md, QUALITY_STANDARDS.md, OUTPUT_RULES.md read and understood

---

## 📋 PHASE INFORMATION

| Property | Value |
|----------|-------|
| **Phase** | 1 — Repository Init & Discovery |
| **Entry Criteria** | All prerequisite documents read; working knowledge base initialized |
| **Exit Criteria** | Complete repository inventory; metadata collected; tech stack identified |
| **Estimated Effort** | Variable (proportional to repository size) |

---

## 🎯 OBJECTIVES

1. **Discover** the complete repository structure.
2. **Catalog** every file, folder, and artifact.
3. **Identify** languages, frameworks, and technologies.
4. **Document** the build system and configuration.
5. **Initialize** the working knowledge base.
6. **Establish** the baseline for all subsequent phases.

---

## 🔬 METHODOLOGY

### Step 1: Repository Metadata

Collect the following metadata:

```
- Repository Name:
- Repository Path:
- Total Files:
- Total Folders:
- Repository Size (MB):
- Primary Language:
- Secondary Languages:
- Build System:
- Package Manager:
- License (if identifiable):
- Last Commit/Modification Date (if available):
```

### Step 2: Full File Inventory

Use the `list_files` tool with `recursive=true` to generate a complete file listing.

**Organize the inventory into categories:**

| Category | Description | Examples |
|----------|-------------|----------|
| Source Code | Application code files | .py, .js, .ts, .java, .go, .rs, .cpp |
| Configuration | Config files | .json, .yaml, .toml, .ini, .cfg |
| Build Files | Build system files | Dockerfile, Makefile, pom.xml, build.gradle |
| Documentation | Documentation files | .md, .rst, .txt, .pdf |
| Tests | Test files | test_*, *_test, *_spec, __tests__ |
| Assets | Static assets | .png, .svg, .css, .html |
| Data | Data files | .csv, .sql, .db, .sqlite |
| Generated | Auto-generated files | dist/, build/, generated/ |
| Third-Party | Vendor/external code | node_modules/, vendor/ |
| Scripts | Utility scripts | .sh, .bat, .ps1 |

### Step 3: Language & Tech Stack Identification

For each detected language and technology, document:

```
- Language/Framework:
- Version (if detectable):
- Files using this language:
- Key libraries/packages:
- Framework patterns detected:
- Package manager files:
```

**Check these files for dependency information:**

| File Type | Files to Check |
|-----------|----------------|
| Node.js | package.json, package-lock.json, yarn.lock |
| Python | requirements.txt, pyproject.toml, Pipfile, setup.py |
| Java | pom.xml, build.gradle, build.gradle.kts |
| Rust | Cargo.toml, Cargo.lock |
| Go | go.mod, go.sum |
| Ruby | Gemfile, Gemfile.lock |
| C/C++ | CMakeLists.txt, Makefile, conanfile.txt |
| Docker | Dockerfile, docker-compose.yml |
| Generic | .gitignore, .env.example, .editorconfig |

### Step 4: Build & Configuration Analysis

Examine the build system and configuration:

```
- Build system type:
- Build commands:
- Test commands:
- Lint/format commands:
- CI/CD configuration (if present):
- Environment variable requirements:
- External service dependencies:
- Database dependencies:
```

### Step 5: Repository Structure Overview

Generate a visual tree of the top 3-4 levels of the repository:

```
repository/
├── src/
│   ├── main/
│   │   ├── java/
│   │   └── resources/
│   └── test/
├── config/
├── docs/
└── scripts/
```

### Step 6: Working Knowledge Base Initialization

Initialize the knowledge base with Phase 1 findings:

```json
{
  "repository_metadata": { /* from Step 1 */ },
  "file_inventory": { /* from Step 2 */ },
  "tech_stack": { /* from Step 3 */ },
  "build_config": { /* from Step 4 */ },
  "structure_overview": { /* from Step 5 */ },
  "phase_1_notes": {
    "unusual_findings": [],
    "initial_assumptions": [],
    "open_questions": []
  }
}
```

---

## 🛠️ TOOLS

| Tool | Purpose | Usage |
|------|---------|-------|
| `list_files` (recursive) | Generate complete file tree | Primary tool for Phase 1 |
| `read_file` | Examine key config files | package.json, Dockerfile, etc. |
| `execute_command` | Run language version checks | `node --version`, `python --version` |
| `search_files` | Find specific patterns | Search for TODO, FIXME, HACK |

---

## 📚 KNOWLEDGE BASE UPDATE

Add to the working knowledge base:

1. **RepositoryMetadata:** Name, path, size, language breakdown
2. **FileInventory:** Complete listing with categories
3. **TechStack:** Languages, frameworks, versions, patterns
4. **BuildConfig:** Build system, commands, CI/CD configuration
5. **StructureOverview:** Top-level tree view
6. **PhaseNotes:** Initial observations, assumptions, questions

---

## 📦 DELIVERABLES

Phase 1 produces (in the output directory):

1. `01_DISCOVERY/REPOSITORY_OVERVIEW.md` — High-level overview
2. `01_DISCOVERY/FILE_INVENTORY.md` — Complete file listing with categories
3. `01_DISCOVERY/LANGUAGE_AND_TECH_STACK.md` — Technology analysis
4. `01_DISCOVERY/BUILD_AND_CONFIGURATION.md` — Build system analysis

---

## ✅ QUALITY CHECK

- [ ] All files in the repository have been discovered and categorized?
- [ ] All languages and frameworks identified?
- [ ] Build system and configuration understood?
- [ ] No file categories were missed?
- [ ] Repository metadata is complete?
- [ ] Working knowledge base has been initialized?

---

## 🚪 PHASE COMPLETION GATE

Before proceeding to Phase 2:

1. Confirm the file inventory is complete.
2. Confirm the tech stack is fully identified.
3. Confirm the build system is documented.
4. Confirm the knowledge base is initialized.
5. **No gaps should remain** — if gaps exist, resolve them before proceeding.

---

**PROCEED TO PHASE 2 → `PROMPT_02.md`**

