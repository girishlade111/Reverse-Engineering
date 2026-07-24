# PROMPT_22: Developer Handbook & Rebuild Guide Generation

## Classification
- **Domain:** Documentation Generation
- **Phase:** 5 — Documentation Production
- **Prerequisites:** All Phase 1-4 prompts (01-19)
- **Dependencies:** Complete analysis context
- **Estimated Effort:** High

---

## Objective

Generate comprehensive developer documentation including setup guide, development workflow, coding conventions, testing strategy, deployment process, and a complete rebuild guide that enables a developer to recreate the system from scratch.

---

## Input Requirements

### Required Context
- Build and deployment configuration from PROMPT_04
- Development environment setup from metadata
- Testing structure and patterns
- Coding conventions and style guides

---

## Analysis Tasks

### Task 1: Development Setup Guide
**Purpose:** Document how to set up the development environment.

**Instructions:**
1. Document step-by-step setup process:
   - Prerequisites and system requirements
   - Repository cloning and branch strategy
   - Local environment configuration
   - Dependency installation
   - Database setup and migration
   - Running the application locally
   - Running tests

**Output Format:**

```markdown
## Development Setup Guide

### Prerequisites
- Python 3.11+
- Node.js 18+
- PostgreSQL 15
- Redis 7
- Poetry (Python packaging)
- npm (Node.js packaging)

### Step 1: Clone Repository
```bash
git clone https://github.com/company/repo.git
cd repo
```

### Step 2: Configure Environment
```bash
cp .env.example .env
# Edit .env with your local configuration
```

### Step 3: Install Dependencies
```bash
# Python dependencies
poetry install

# Node.js dependencies
npm install
```

### Step 4: Database Setup
```bash
# Create database
createdb app_development

# Run migrations
poetry run alembic upgrade head

# Seed data (optional)
poetry run python scripts/seed.py
```

### Step 5: Run Application
```bash
# Terminal 1: Backend
poetry run uvicorn src.main:app --reload

# Terminal 2: Frontend
npm run dev
```

### Step 6: Run Tests
```bash
# All tests
poetry run pytest

# Specific test file
poetry run pytest tests/test_users.py

# Frontend tests
npm test
```
```

---

### Task 2: Development Workflow Guide
**Purpose:** Document the standard development workflow.

**Instructions:**
1. Document development workflows:
   - Feature development workflow
   - Bug fix workflow
   - Code review process
   - Release process
   - Hotfix process

**Output Format:**

```markdown
## Development Workflow

### Feature Development
1. Create branch from `main`: `git checkout -b feature/description`
2. Implement changes following coding conventions
3. Write/update tests
4. Run tests locally: `poetry run pytest`
5. Create pull request
6. Address review feedback
7. Merge to `main`

### Code Review Checklist
- [ ] Code follows project conventions
- [ ] Tests cover new functionality
- [ ] No breaking changes without deprecation notice
- [ ] Documentation updated
- [ ] Error handling is appropriate
- [ ] Logging added for key operations
```
---

### Task 3: Rebuild Guide
**Purpose:** Generate a complete guide to rebuild the system from scratch.

**Instructions:**
1. Document the complete rebuild process:
   - Infrastructure requirements
   - Service dependencies
   - Build process for each component
   - Database creation and migration
   - Deployment steps
   - Verification steps

**Output Format:**

```markdown
## Rebuild Guide

### System Requirements
| Resource | Requirement |
|----------|-------------|
| CPU | 4 cores |
| RAM | 8GB minimum |
| Storage | 20GB available |
| OS | Ubuntu 22.04 / macOS 14+ |

### Step-by-Step Rebuild
[Complete rebuild instructions covering all components]
```

---

## Output Requirements
### Required Deliverables
1. Development setup guide
2. Development workflow documentation
3. Complete rebuild guide

### Output Structure
```
DOCUMENTATION_DEVELOPER/
├── setup_guide.md
├── development_workflow.md
└── rebuild_guide.md
```

---

## Cross-References
- **Previous Prompt:** PROMPT_21_DOCUMENTATION_TECHNICAL.md
- **Next Prompt:** PROMPT_23_DOCUMENTATION_DIAGRAMS.md
- **Shared Context Key:** documentation.developer
