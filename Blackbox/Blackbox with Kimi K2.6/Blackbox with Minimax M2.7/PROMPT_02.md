# Phase 2: Structural Analysis & Mapping

> **Document:** PROMPT_02.md  
> **Phase:** 2 of 10  
> **Purpose:** Analyze the repository's organizational structure, module boundaries, and file responsibilities  
> **Prerequisite:** Phase 1 complete; file inventory and tech stack known

---

## 📋 PHASE INFORMATION

| Property | Value |
|----------|-------|
| **Phase** | 2 — Structural Analysis & Mapping |
| **Entry Criteria** | Phase 1 complete; file inventory available; tech stack identified |
| **Exit Criteria** | Module map built; folder responsibilities documented; naming conventions identified |
| **Estimated Effort** | Medium |

---

## 🎯 OBJECTIVES

1. **Map** the module structure of the repository.
2. **Document** the responsibility of every folder.
3. **Identify** naming conventions and organizational patterns.
4. **Analyze** file organization strategies.
5. **Build** a structural model of the repository.
6. **Identify** entry points and application boundaries.

---

## 🔬 METHODOLOGY

### Step 1: Module Boundary Identification

Identify all major modules in the repository:

```
- Look for top-level directories that represent logical groupings.
- Look for package declarations (Java, Python, Go).
- Look for module definitions (package.json workspaces, Cargo workspace).
- Look for namespace declarations (C#, C++).
- Look for directory naming patterns that reveal module boundaries.
```

**Document each module:**

```markdown
### Module: [module-name]
- **Path:** relative/path/
- **Purpose:** [What this module does]
- **Files:** [Number of files]
- **Sub-modules:** [List of sub-modules]
- **Dependencies (internal):** [Modules this depends on]
- **Entry Point:** [Entry file, if applicable]
- **Classification:** [Core / Feature / Utility / Infrastructure / Test]
```

### Step 2: Folder Responsibility Analysis

For every folder (recursively), document:

```
- Folder Path:
- Parent Module:
- Responsibility: (What files in this folder do)
- Files: (Summary of file types and purposes)
- Relationship to Siblings: (How this folder relates to peers)
- Classification: (Source, Test, Config, Asset, Documentation, Build)
```

**Prioritize folders in this order:**
1. Top-level source directories (`src/`, `lib/`, `app/`)
2. Feature/module directories
3. Configuration directories (`config/`, `settings/`)
4. Test directories (`tests/`, `spec/`)
5. Asset directories (`static/`, `assets/`, `public/`)
6. Script directories (`scripts/`, `bin/`)
7. Documentation directories (`docs/`, `doc/`)

### Step 3: Naming Convention Analysis

Identify and document all naming conventions:

```
- Files: (kebab-case, snake_case, camelCase, PascalCase)
- Classes: (PascalCase, CamelCase)
- Functions: (camelCase, snake_case)
- Variables: (camelCase, snake_case, UPPER_CASE for constants)
- Folders: (kebab-case, snake_case)
- Database tables: (singular, plural, snake_case)
- API endpoints: (kebab-case, camelCase)
- Test files: (*_test, *.spec, test_*, *_test.go)
- Configuration keys: (SNAKE_CASE, camelCase, dot.notation)
```

**Also identify:**
- Whether conventions are enforced by linters/formatters.
- Whether there are convention violations (inconsistent naming).
- Whether the conventions follow language/framework standards.

### Step 4: File Organization Patterns

Identify how files are organized within modules:

```
- Pattern: (Feature-based, Layer-based, Type-based, Hybrid)
- Description:
- Example:
- Consistency: (Consistent / Inconsistent across modules)
```

**Common patterns:**
| Pattern | Description | Example |
|---------|-------------|---------|
| Feature-based | Files grouped by feature/domain | `users/`, `payments/`, `notifications/` |
| Layer-based | Files grouped by architectural layer | `controllers/`, `services/`, `repositories/` |
| Type-based | Files grouped by type | `components/`, `hooks/`, `utils/` |
| Hybrid | Combination of patterns | `features/users/components/` |

### Step 5: Entry Point Identification

Identify all application entry points:

```
- Primary entry point: (main.py, index.js, App.java, main.go)
- Worker entry points: (worker.js, consumer.py)
- CLI entry points: (cli.py, bin/ scripts)
- Test entry points: (jest.config, pytest.ini)
- API entry points: (app.py, server.js, routes/)
- Event handlers: (handler.py, lambda_function.py)
- Cron jobs / scheduled tasks
```

### Step 6: Structural Knowledge Base Update

Update the knowledge base with Phase 2 findings:

```json
{
  "modules": { /* module map from Step 1 */ },
  "folder_responsibilities": { /* from Step 2 */ },
  "naming_conventions": { /* from Step 3 */ },
  "file_organization": { /* from Step 4 */ },
  "entry_points": { /* from Step 5 */ },
  "phase_2_notes": {
    "structural_anomalies": [],
    "organization_insights": [],
    "open_questions": []
  }
}
```

---

## 🛠️ TOOLS

| Tool | Purpose | Usage |
|------|---------|-------|
| `list_files` | View folder contents | Examine module structure |
| `read_file` | Examine module entry files | package.json, __init__.py, mod.rs |
| `search_files` | Find import/require patterns | Module dependency patterns |
| `execute_command` | Run tree command | `tree -L 3` for visual structure |

---

## 📚 KNOWLEDGE BASE UPDATE

Add to the working knowledge base:

1. **ModuleMap:** All modules with boundaries and responsibilities
2. **FolderResponsibilities:** Every folder's purpose
3. **NamingConventions:** All conventions with examples
4. **FileOrganizationPatterns:** How code is organized
5. **EntryPoints:** All application entry points

---

## 📦 DELIVERABLES

Phase 2 produces:

1. `02_STRUCTURAL_ANALYSIS/MODULE_MAP.md` — Complete module mapping
2. `02_STRUCTURAL_ANALYSIS/FOLDER_RESPONSIBILITIES.md` — Folder analysis
3. `02_STRUCTURAL_ANALYSIS/NAMING_CONVENTIONS.md` — Convention documentation
4. `02_STRUCTURAL_ANALYSIS/FILE_ORGANIZATION.md` — Organization patterns

---

## ✅ QUALITY CHECK

- [ ] Every folder has a documented responsibility?
- [ ] Module boundaries are clearly identified?
- [ ] Naming conventions are documented with examples?
- [ ] File organization pattern(s) identified?
- [ ] All entry points documented?
- [ ] No modules were missed?

---

## 🚪 PHASE COMPLETION GATE

Before proceeding to Phase 3:

1. Confirm the module map is complete.
2. Confirm all folder responsibilities are documented.
3. Confirm naming conventions are identified.
4. Confirm entry points are documented.
5. **If the repository uses an unconventional structure, flag it for special attention in later phases.**

---

**PROCEED TO PHASE 3 → `PROMPT_03.md`**

---

> **💡 Module Available:** Use `modules/MODULE_ARCHITECTURE.md` if the repository has a complex or unconventional structure.

