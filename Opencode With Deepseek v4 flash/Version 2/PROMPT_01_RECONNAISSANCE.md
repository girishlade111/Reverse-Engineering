========================================================================
PROMPT 01: RECONNAISSANCE
========================================================================
Phase 1: Initial Repository Reconnaissance and Surface Analysis
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. A complete inventory of all files and directories
2. An understanding of the repository's purpose and domain
3. Identification of all programming languages and technologies used
4. A map of the top-level directory structure
5. Identification of entry points and key configuration files
6. An initial understanding of the build system and dependencies
7. Identification of any existing documentation

========================================================================
INPUTS
========================================================================

- The target repository path
- MASTER_PROMPT.md (for context and orchestration)
- MISSION.md (for core principles)
- OPERATING_RULES.md (for behavioral constraints)
- QUALITY_STANDARDS.md (for quality expectations)

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 1.1: REPOSITORY SURVEY

1.1.1. Run a complete file listing of the repository.
1.1.2. Count total files, directories, and lines of code.
1.1.3. Identify file extensions present and their frequencies.
1.1.4. Identify any hidden files and directories (.git, .env, etc.).
1.1.5. Note any symlinks, submodules, or external references.

1.1.6. Document the complete folder tree at depth 2-3.
1.1.7. For each directory at depth 1-2, summarize its purpose.
1.1.8. Identify any unconventional directory structures.

ACTIVITY 1.2: LANGUAGE AND TECHNOLOGY IDENTIFICATION

1.2.1. Identify all programming languages used.
1.2.2. Identify all frameworks and major libraries.
1.2.3. Identify build tools and build configuration.
1.2.4. Identify package managers and dependency files.
1.2.5. Identify testing frameworks and test configuration.
1.2.6. Identify linting and formatting tools.
1.2.7. Identify CI/CD configuration files.
1.2.8. Identify containerization files (Docker, etc.).
1.2.9. Identify infrastructure-as-code files.
1.2.10. Identify database schemas and migration files.

ACTIVITY 1.3: ENTRY POINT IDENTIFICATION

1.3.1. Find all entry points:
    - Main application entry points (main(), index.js, etc.)
    - API route definitions
    - CLI command definitions
    - Worker/queue entry points
    - Cron job definitions
    - Event handler registrations
    - Test entry points

1.3.2. For each entry point, document:
    - File path and line number
    - Type (HTTP, CLI, Worker, etc.)
    - What triggers it
    - Brief description of its purpose

ACTIVITY 1.4: CONFIGURATION ANALYSIS

1.4.1. Find and read all configuration files:
    - Environment configuration (.env, config/, etc.)
    - Application configuration (JSON, YAML, TOML)
    - Build configuration (webpack, vite, tsconfig, etc.)
    - Package configuration (package.json, Cargo.toml, etc.)
    - Editor configuration (.editorconfig, .vscode/, etc.)
    - Linting configuration (.eslintrc, .prettierrc, etc.)

1.4.2. Document the configuration schema for each.
1.4.3. Identify all environment variables and their purposes.
1.4.4. Identify all configuration defaults.

ACTIVITY 1.5: DEPENDENCY ANALYSIS

1.5.1. Read all dependency manifest files.
1.5.2. Categorize each dependency:
    - Runtime dependency (required for production)
    - Development dependency (testing, building, linting)
    - Optional dependency
    - Peer dependency

1.5.3. For each runtime dependency, document:
    - Package name and version
    - Purpose in the system
    - Which modules use it
    - Criticality (essential, important, optional)

1.5.4. Identify the dependency resolution strategy.
1.5.5. Check for lock files (package-lock.json, yarn.lock, etc.).

ACTIVITY 1.6: EXISTING DOCUMENTATION SURVEY

1.6.1. Find and read all existing documentation:
    - README files
    - CONTRIBUTING files
    - CHANGELOG files
    - LICENSE files
    - API documentation
    - Architecture Decision Records (ADRs)
    - Wiki files
    - Any other .md files

1.6.2. Assess the quality and completeness of existing docs.
1.6.3. Identify gaps in existing documentation.
1.6.4. Document the LICENSE terms.

ACTIVITY 1.7: CODE QUALITY SURFACE SCAN

1.7.1. Check for:
    - Test files and their organization
    - Test coverage configuration
    - Linting rules and severity
    - Type checking configuration
    - Code formatting standards
    - Pre-commit hooks configuration

1.7.2. Read sample test files (at least 2-3) to understand test style.

ACTIVITY 1.8: REPOSITORY METADATA

1.8.1. Read .gitignore to understand what's intentionally excluded.
1.8.2. Check git history (if available) for:
    - Number of commits
    - Branch structure
    - Recent activity
    - Release tags

1.8.3. Note the repository's overall maturity indicators.

========================================================================
ANALYSIS METHODOLOGY
========================================================================

For this phase, use SurfaceScan depth:
- Read directory structures, not file contents (for most files)
- Read configuration files completely
- Read entry points completely
- Read existing documentation completely
- Read dependency manifests completely

Do NOT perform deep code analysis in this phase.
Save deep reading for Phases 3-5.

========================================================================
REQUIRED ARTIFACTS
========================================================================

Generate the following artifacts for this phase:

ARTIFACT 1.1: FILE_INVENTORY.md
- Complete file list grouped by directory
- File counts and statistics
- Language distribution
- Hidden file summary

ARTIFACT 1.2: DIRECTORY_MAP.md
- Directory tree at depth 3
- Purpose description for each directory

ARTIFACT 1.3: TECH_STACK_SURVEY.md
- Languages identified
- Frameworks identified
- Build tools identified
- Testing tools identified
- CI/CD tools identified
- Infrastructure tools identified

ARTIFACT 1.4: ENTRY_POINTS.md
- Complete list of entry points
- Type classification
- Trigger mechanism
- Brief purpose description

ARTIFACT 1.5: CONFIGURATION_ANALYSIS.md
- All configuration files found
- Configuration schema per file
- Environment variable inventory
- Default values

ARTIFACT 1.6: DEPENDENCY_ANALYSIS.md
- Complete dependency list
- Categorization (runtime vs dev)
- Purpose analysis
- Criticality assessment

ARTIFACT 1.7: EXISTING_DOCS_REVIEW.md
- Documentation inventory
- Quality assessment
- Gap analysis

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] Every file in the repository has been accounted for.
[ ] Every directory's purpose is documented.
[ ] Every entry point has been identified.
[ ] Every configuration file has been read.
[ ] Every dependency has been categorized.
[ ] All existing documentation has been reviewed.
[ ] Tech stack is fully identified.
[ ] No assumptions without evidence.
[ ] All artifacts are generated.
[ ] All artifacts meet quality standards (score >= 4.0).

========================================================================
OUTPUTS TO NEXT PHASE
========================================================================

Pass to Phase 2:
- FILE_INVENTORY.md
- DIRECTORY_MAP.md
- TECH_STACK_SURVEY.md
- ENTRY_POINTS.md
- CONFIGURATION_ANALYSIS.md
- DEPENDENCY_ANALYSIS.md
- EXISTING_DOCS_REVIEW.md

========================================================================
END OF PROMPT 01
========================================================================
