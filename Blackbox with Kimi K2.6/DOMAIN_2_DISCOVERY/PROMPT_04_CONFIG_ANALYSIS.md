# PROMPT_04: Configuration & Environment Analysis

## Classification
- **Domain:** Discovery & Intake
- **Phase:** 1 — Initial Repository Analysis
- **Prerequisites:** PROMPT_01 (File Inventory), PROMPT_02 (Language Detection), PROMPT_03 (Dependency Mapping)
- **Dependencies:** File inventory, language detection, dependency map
- **Estimated Effort:** Medium (50-100 config files to analyze)

---

## Objective

Perform a comprehensive analysis of all configuration files, environment variables, build configurations, and deployment settings in the repository. Understand how the system is configured, what can be changed without code modification, and how the system adapts to different environments.

---

## Input Requirements

### Required Context
- Complete file inventory from PROMPT_01
- Language and framework catalog from PROMPT_02
- Dependency map from PROMPT_03
- Build system identification

### Required Files
- All configuration files (.json, .yaml, .toml, .ini, .cfg, .conf, .env, .properties)
- All environment variable files (.env, .env.example, .env.local)
- All build configuration files (webpack.config.js, tsconfig.json, pyproject.toml, Makefile)
- All deployment configuration files (Dockerfile, docker-compose.yml, Kubernetes manifests)
- All CI/CD configuration files (.github/workflows/, .gitlab-ci.yml, Jenkinsfile)

---

## Pre-Analysis Checklist

- [ ] PROMPT_01, PROMPT_02, PROMPT_03 completed and context loaded
- [ ] All configuration files identified from file inventory
- [ ] Build system identified from metadata
- [ ] Environment variable files located

---

## Analysis Tasks

### Task 1: Configuration File Inventory & Classification

**Purpose:** Create a complete inventory of all configuration files, classified by purpose and scope.

**Instructions:**
1. Locate all configuration files in the repository (excluding build artifacts)
2. Classify each configuration file:
   - **Application Config:** Settings that control application behavior
   - **Build Config:** Settings for build tools and compilers
   - **Environment Config:** Environment-specific settings (dev, staging, production)
   - **Deployment Config:** Settings for deployment infrastructure
   - **CI/CD Config:** Settings for continuous integration/deployment
   - **Tool Config:** Settings for development tools (linters, formatters, editors)
   - **Security Config:** Settings for authentication, authorization, encryption
3. For each file, document:
   - File path and name
   - Format (JSON, YAML, TOML, INI, etc.)
   - Purpose and scope
   - Environment specificity (dev only, all environments, etc.)
   - Whether it contains sensitive information

**Success Criteria:**
- Every configuration file is identified and classified
- Purpose and scope are documented for each file
- Sensitive configuration is flagged

**Output Format:**

```markdown
## Configuration File Inventory

### Classification Summary
| Category | Count | Example Files |
|----------|-------|---------------|
| Application Config | 8 | settings.py, config.yaml, app.config.json |
| Build Config | 5 | webpack.config.js, tsconfig.json, pyproject.toml |
| Environment Config | 4 | .env, .env.example, .env.production |
| Deployment Config | 3 | Dockerfile, docker-compose.yml, nginx.conf |
| CI/CD Config | 2 | test.yml, deploy.yml |
| Tool Config | 6 | .eslintrc.js, .prettierrc, .editorconfig |
| Security Config | 2 | .htaccess, cors.config.js |

### Detailed Configuration File List

#### Application Configuration
| File | Format | Purpose | Environment | Sensitive? |
|------|--------|---------|-------------|------------|
| src/config/settings.py | Python | Main application settings | All | No |
| src/config/database.py | Python | Database connection config | All | Yes (passwords) |
| config/app.config.json | JSON | Feature flags and toggles | All | No |
| config/logging.yaml | YAML | Logging configuration | All | No |

#### Environment Configuration
| File | Format | Purpose | Environment | Sensitive? |
|------|--------|---------|-------------|------------|
| .env.example | Text | Template for env vars | All | No |
| .env.development | Text | Dev environment vars | Development | Yes |
| .env.production | Text | Production environment vars | Production | Yes |
```

---

### Task 2: Application Configuration Deep Analysis

**Purpose:** Understand every configuration option that controls application behavior.

**Instructions:**
1. For each application configuration file, read and document:
   - Every configuration key/setting
   - Its default value (if any)
   - Its allowed values or type
   - Its effect on application behavior
   - Where it's used in the codebase
2. Identify configuration patterns:
   - Environment variable overrides
   - Configuration inheritance (base config + environment-specific overrides)
   - Feature flags and toggles
   - Runtime configuration vs. compile-time configuration
3. Map configuration to code:
   - Which code modules read which configuration
   - How configuration values flow through the system
   - What happens when configuration is invalid or missing

**Success Criteria:**
- Every configuration setting is documented with its effect
- Configuration-to-code mapping is complete
- Configuration patterns are identified
- Error handling for missing/invalid config is documented

**Output Format:**

```markdown
## Application Configuration Analysis

### Configuration Settings

#### Database Configuration (src/config/database.py)
| Setting | Type | Default | Environment Override | Effect | Used By |
|---------|------|---------|---------------------|--------|---------|
| DATABASE_URL | string | (required) | DATABASE_URL env | PostgreSQL connection string | All data access code |
| DATABASE_POOL_SIZE | integer | 10 | DATABASE_POOL_SIZE env | Connection pool size | SQLAlchemy engine |
| DATABASE_TIMEOUT | integer | 30 | DATABASE_TIMEOUT env | Query timeout in seconds | All queries |

#### Application Settings (src/config/settings.py)
| Setting | Type | Default | Environment Override | Effect | Used By |
|---------|------|---------|---------------------|--------|---------|
| DEBUG | boolean | False | DEBUG env | Enables debug mode | Error handling, logging |
| SECRET_KEY | string | (required) | SECRET_KEY env | JWT signing, encryption | Auth module |
| ALLOWED_HOSTS | list | ["*"] | ALLOWED_HOSTS env | CORS and host validation | API gateway |
| LOG_LEVEL | string | "INFO" | LOG_LEVEL env | Logging verbosity | All modules |

### Configuration Patterns

#### Environment Variable Override Pattern
```python
# Pattern: Config with env var override
DATABASE_URL = os.getenv("DATABASE_URL", "postgresql://localhost:5432/app")
DEBUG = os.getenv("DEBUG", "False").lower() == "true"
```

#### Feature Flags
| Flag | Location | Default | Effect | Code Reference |
|------|----------|---------|--------|----------------|
| ENABLE_NEW_PIPELINE | config/app.config.json | false | Enables new data pipeline | src/core/engine.py:45 |
| MAINTENANCE_MODE | config/app.config.json | false | Puts app in maintenance mode | src/middleware/maintenance.py |

### Configuration Error Handling
| Scenario | Behavior | Code Location |
|----------|----------|---------------|
| Missing DATABASE_URL | Application fails to start with clear error | src/config/database.py:10 |
| Invalid LOG_LEVEL | Falls back to INFO with warning | src/config/settings.py:25 |
| Missing SECRET_KEY | Application fails to start | src/config/settings.py:15 |
```

---

### Task 3: Environment Variable Mapping

**Purpose:** Create a complete map of all environment variables used by the application.

**Instructions:**
1. Scan all source files for environment variable access patterns:
   - `os.getenv()`, `os.environ.get()` (Python)
   - `process.env.*` (Node.js)
   - `env!()`, `var!()` (Rust)
   - `os.Getenv()`, `os.LookupEnv()` (Go)
2. For each environment variable found:
   - Document its name
   - Document where it's read
   - Document its purpose
   - Document its default value (if any)
   - Document whether it's required or optional
   - Document its type (string, number, boolean, JSON)
3. Cross-reference with .env files:
   - Which variables are defined in .env files?
   - Which variables are missing from .env.example?
   - Which variables are defined but never used?

**Success Criteria:**
- Every environment variable used in code is documented
- Cross-reference with .env files is complete
- Missing or unused variables are flagged

**Output Format:**

```markdown
## Environment Variable Map

### Complete Environment Variable Inventory
| Variable | Type | Required | Default | Used In | Purpose |
|----------|------|----------|---------|---------|---------|
| DATABASE_URL | string | Yes | - | src/config/database.py:12 | PostgreSQL connection |
| SECRET_KEY | string | Yes | - | src/config/settings.py:15 | JWT signing key |
| DEBUG | boolean | No | False | src/config/settings.py:20 | Debug mode toggle |
| LOG_LEVEL | string | No | INFO | src/config/settings.py:25 | Logging level |
| REDIS_URL | string | No | redis://localhost:6379 | src/config/cache.py:8 | Redis connection |
| AWS_ACCESS_KEY_ID | string | Conditional | - | src/services/s3.py:5 | AWS credentials |
| AWS_SECRET_ACCESS_KEY | string | Conditional | - | src/services/s3.py:6 | AWS credentials |
| SENDGRID_API_KEY | string | Conditional | - | src/services/email.py:10 | Email service |

### .env File Cross-Reference
| Variable | .env.example | .env.development | .env.production | Status |
|----------|-------------|------------------|-----------------|--------|
| DATABASE_URL | Yes | Yes | Yes | Complete |
| SECRET_KEY | Yes (placeholder) | Yes | Yes | Complete |
| DEBUG | Yes | Yes | No | Complete |
| LOG_LEVEL | Yes | Yes | Yes | Complete |
| REDIS_URL | No | Yes | Yes | Missing from example |
| NEW_FEATURE_FLAG | No | Yes | No | Missing from example |

### Issues Found
| Issue | Variable | Details |
|-------|----------|---------|
| Missing from .env.example | REDIS_URL | Used in production but not documented in template |
| Unused variable | OLD_DB_URL | Defined in .env files but never referenced in code |
| Hardcoded fallback | DATABASE_URL | Falls back to localhost in production code |
```

---

### Task 4: Build & Deployment Configuration Analysis

**Purpose:** Understand how the application is built, packaged, and deployed.

**Instructions:**
1. Analyze build configuration:
   - Build system and version
   - Build steps and their order
   - Build targets and outputs
   - Build-time optimizations
   - Build-time environment variables
2. Analyze deployment configuration:
   - Containerization (Dockerfile analysis)
   - Orchestration (docker-compose, Kubernetes)
   - Infrastructure as Code (Terraform, CloudFormation)
   - Deployment scripts and their steps
3. Analyze CI/CD pipeline:
   - Pipeline stages and their order
   - Test execution strategy
   - Build and publish steps
   - Deployment environments and strategies
   - Approval gates and manual steps

**Success Criteria:**
- Build process is fully documented with all steps
- Deployment configuration is analyzed
- CI/CD pipeline is documented with all stages

**Output Format:**

```markdown
## Build & Deployment Configuration

### Build Process

#### Build System: Poetry + Webpack
| Step | Tool | Command | Output | Duration (est.) |
|------|------|---------|--------|-----------------|
| 1. Install Python deps | Poetry | poetry install | .venv/ | 30s |
| 2. Install Node deps | npm | npm ci | node_modules/ | 60s |
| 3. TypeScript compile | tsc | npx tsc | dist/client/ | 20s |
| 4. Webpack bundle | webpack | npx webpack | dist/bundle.js | 30s |
| 5. Collect static | Python | python manage.py collectstatic | static/ | 10s |
| **Total** | | | | **~150s** |

#### Build Outputs
| Output | Path | Size | Description |
|--------|------|------|-------------|
| Backend package | dist/backend/ | 15MB | Python wheel |
| Frontend bundle | dist/frontend/ | 5MB | Static assets |
| Combined image | docker image | 200MB | Production container |

### Deployment Configuration

#### Docker Multi-Stage Build
| Stage | Base Image | Purpose | Key Steps |
|-------|------------|---------|-----------|
| builder-python | python:3.11-slim | Build Python deps | poetry install --no-dev |
| builder-node | node:18-alpine | Build frontend | npm ci, npm run build |
| production | python:3.11-slim | Runtime image | Copy artifacts, set entrypoint |

#### Docker Compose Services
| Service | Image | Ports | Dependencies | Health Check |
|---------|-------|-------|--------------|--------------|
| api | app:latest | 8000:8000 | db, cache, redis | /health |
| web | nginx:alpine | 80:80, 443:443 | api | - |
| db | postgres:15 | 5432:5432 | - | pg_isready |
| cache | redis:7-alpine | 6379:6379 | - | redis ping |

### CI/CD Pipeline (GitHub Actions)

#### Pipeline: Test & Deploy
| Stage | Job | Trigger | Steps | Approx Time |
|-------|-----|---------|-------|-------------|
| 1. Test | lint | Push, PR | ESLint, PyLint | 2min |
| 2. Test | unit-test | Push, PR | pytest, jest | 5min |
| 3. Test | integration-test | Push, PR | docker-compose up, test | 10min |
| 4. Build | build-image | Push to main | docker build, tag | 3min |
| 5. Deploy | deploy-staging | Push to main | Deploy to staging | 5min |
| 6. Deploy | deploy-production | Tag v* | Manual approval, deploy | 10min |

#### Deployment Strategy
| Environment | Strategy | Rollback | Approval |
|-------------|----------|----------|----------|
| Staging | Rolling update | Automatic on failure | Automatic |
| Production | Blue-green | Manual | Manual (2 approvals) |
```

---

### Task 5: Security Configuration Analysis

**Purpose:** Identify and analyze all security-related configuration.

**Instructions:**
1. Identify security configuration:
   - Authentication configuration (JWT secrets, OAuth settings)
   - Authorization configuration (RBAC, permissions)
   - CORS configuration
   - Rate limiting configuration
   - Encryption configuration
   - SSL/TLS configuration
2. Assess security configuration quality:
   - Are secrets stored securely (not hardcoded)?
   - Are there default/weak credentials?
   - Is HTTPS enforced?
   - Are security headers configured?
   - Is input validation configured?
3. Flag security concerns:
   - Hardcoded secrets
   - Weak default configurations
   - Missing security configurations
   - Overly permissive settings

**Success Criteria:**
- All security configuration is identified
- Security configuration quality is assessed
- Security concerns are flagged

**Output Format:**

```markdown
## Security Configuration Analysis

### Security Configuration Inventory
| Category | Configuration | Location | Current Value | Assessment |
|----------|--------------|----------|---------------|------------|
| Authentication | JWT_SECRET | src/config/settings.py | (env var) | Good (not hardcoded) |
| Authentication | JWT_EXPIRY | src/config/settings.py | 3600 (1 hour) | Good |
| CORS | ALLOWED_ORIGINS | src/config/settings.py | ["*"] | WARNING (too permissive) |
| Rate Limiting | RATE_LIMIT | src/middleware/rate_limit.py | 100/min | Good |
| SSL | SSL_ENABLED | src/config/settings.py | True | Good |

### Security Concerns Found
| Severity | Issue | Location | Recommendation |
|----------|-------|----------|----------------|
| HIGH | CORS allows all origins (*) | src/config/settings.py:30 | Restrict to specific domains |
| MEDIUM | Debug mode enabled in staging | .env.staging | Disable DEBUG in staging |
| LOW | No rate limiting on auth endpoints | src/api/handlers/auth.py | Add rate limiting |
| INFO | Default SECRET_KEY in .env.example | .env.example | Remove default value |

### Secrets Management
| Secret | Storage Method | Risk Level |
|--------|---------------|------------|
| DATABASE_URL | Environment variable | LOW |
| SECRET_KEY | Environment variable | LOW |
| AWS credentials | Environment variable | LOW |
| API keys | Environment variable | LOW |
| (No hardcoded secrets found) | | |
```

---

## Synthesis

**Purpose:** Create a unified configuration profile that documents how the entire system is configured.

**Instructions:**
1. Combine all task outputs into a unified configuration reference
2. Create a configuration dependency map (which configs affect which modules)
3. Identify configuration best practices and anti-patterns
4. Prepare context for PROMPT_05 (Architecture High-Level)

**Output Format:**

```markdown
## Unified Configuration Profile

### Configuration Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Environment Variables                      │
│  DATABASE_URL │ SECRET_KEY │ DEBUG │ LOG_LEVEL │ REDIS_URL  │
└────────────────────────┬────────────────────────────────────┘
                         │ Read by
┌────────────────────────▼────────────────────────────────────┐
│                  Application Settings                         │
│              src/config/settings.py                          │
│  ┌──────────────┬──────────────┬──────────────────────┐     │
│  │ Database     │ Application  │ External Services    │     │
│  │ Config       │ Config       │ Config               │     │
│  └──────────────┴──────────────┴──────────────────────┘     │
└────────────────────────┬────────────────────────────────────┘
                         │ Used by
┌────────────────────────▼────────────────────────────────────┐
│                    Application Modules                        │
│  API Layer │ Business Logic │ Data Access │ External APIs   │
└─────────────────────────────────────────────────────────────┘
```

### Configuration by Environment
| Setting | Development | Staging | Production |
|---------|-------------|---------|------------|
| DEBUG | True | False | False |
| DATABASE_URL | Local PostgreSQL | Staging RDS | Production RDS |
| LOG_LEVEL | DEBUG | INFO | WARNING |
| CORS Origins | * | *.staging.example.com | *.example.com |

### Configuration Anti-Patterns Found
| Anti-Pattern | Location | Impact |
|--------------|----------|--------|
| CORS allows all origins | src/config/settings.py | Security risk |
| Hardcoded fallback values | src/config/database.py | May mask missing config |
| Missing .env.example entries | .env.example | Poor developer experience |

### Context for Next Prompt
- Configuration architecture influences system architecture
- Environment-specific configs indicate deployment environments
- Feature flags indicate architectural extension points
```

---

## Output Requirements

### Required Deliverables
1. Configuration file inventory with classification
2. Application configuration deep analysis with setting-to-code mapping
3. Complete environment variable map with cross-reference
4. Build and deployment configuration analysis
5. Security configuration analysis with flagged concerns
6. Unified configuration profile

### Output Structure
```
CONFIG_ANALYSIS/
├── config_inventory.md
├── app_config_analysis.md
├── env_variable_map.md
├── build_deploy_config.md
├── security_config.md
└── unified_config_profile.md
```

---

## Quality Checks

- [ ] Every configuration file is identified and classified
- [ ] Every configuration setting is documented with its effect
- [ ] Every environment variable used in code is documented
- [ ] Cross-reference between code and .env files is complete
- [ ] Build and deployment process is fully documented
- [ ] Security configuration is assessed and concerns flagged
- [ ] Configuration anti-patterns are identified
- [ ] Context for PROMPT_05 is prepared

---

## Continuation Rules

If the repository has more than 30 configuration files:
1. Focus on application configuration and environment variables
2. Summarize build/deploy/CI-CD configuration at high level
3. Flag any configuration files requiring deeper analysis

---

## Cross-References

- **Previous Prompt:** `PROMPT_03_DEPENDENCY_MAPPING.md`
- **Next Prompt:** `PROMPT_05_ARCHITECTURE_HIGH_LEVEL.md`
- **Related Context:** Configuration architecture feeds into system architecture
- **Shared Context Key:** `config.files`, `config.env_vars`, `config.build`, `config.security`
