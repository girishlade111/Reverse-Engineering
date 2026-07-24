# PROMPT_18 — Phase 17: Configuration & Environment Analysis

## PHASE CLASS: Operations Analysis
## DEPENDENCIES: PROMPT_17 (AI Workflows) — complete
## OUTPUT DIRECTORY: `re-docs/17-config-env/`

---

## OBJECTIVE

Document every configuration mechanism, environment variable, secret management approach, feature flag, and environment-specific setting in the system.

## PREREQUISITES

- [ ] PROMPT_17 completed
- [ ] Build system is understood
- [ ] Deployment is understood (if applicable)

## INPUTS

- `re-docs/02-build-config/01-build-system.md`
- `re-docs/02-build-config/06-cicd-pipelines.md`
- All configuration files
- All environment variable templates (.env.example, etc.)

## ANALYSIS STEPS

### Step 1: Configuration Architecture

Document how the configuration system works:

```markdown
## Configuration Architecture

### Approach: Environment-based configuration with validation

### Configuration Loading Order
1. Default values (hardcoded in config files)
2. .env file (local development)
3. Environment variables (production/CI)
4. Secret manager (sensitive values)

### Configuration Structure
src/config/
├── index.ts          → Configuration loader
├── app.ts            → Application config
├── database.ts       → Database config
├── auth.ts           → Auth config (JWT secrets, etc.)
├── external.ts       → External API config
└── constants.ts      → System constants
```

### Step 2: Environment Variable Inventory

Document every environment variable:

```markdown
## Environment Variables

### Database
| Variable | Required | Default | Description | Used In |
|----------|----------|---------|-------------|---------|
| DATABASE_URL | Yes | — | PostgreSQL connection string | src/config/database.ts:10 |
| DB_POOL_MIN | No | 2 | Min pool connections | src/config/database.ts:15 |
| DB_POOL_MAX | No | 10 | Max pool connections | src/config/database.ts:16 |

### Authentication
| Variable | Required | Default | Description | Used In |
|----------|----------|---------|-------------|---------|
| JWT_SECRET | Yes | — | JWT signing secret | src/config/auth.ts:10 |
| JWT_EXPIRES_IN | No | 15m | Access token expiry | src/config/auth.ts:12 |
| REFRESH_TOKEN_EXPIRES_IN | No | 7d | Refresh token expiry | src/config/auth.ts:14 |

### External Services
| Variable | Required | Default | Description | Used In |
|----------|----------|---------|-------------|---------|
| SENDGRID_API_KEY | Yes | — | SendGrid API key | src/config/external.ts:10 |
| STRIPE_API_KEY | Yes | — | Stripe secret key | src/config/external.ts:15 |
| OPENAI_API_KEY | No | — | OpenAI API key | src/config/external.ts:20 |
```

### Step 3: Configuration Schema

Document the configuration schema:

```typescript
// src/config/index.ts:15-45
interface AppConfig {
  env: 'development' | 'staging' | 'production';
  port: number;
  host: string;
  database: DatabaseConfig;
  auth: AuthConfig;
  external: ExternalServiceConfig;
}

interface DatabaseConfig {
  url: string;
  poolMin: number;
  poolMax: number;
  ssl: boolean;
}

interface AuthConfig {
  jwtSecret: string;
  jwtExpiresIn: string;
  refreshTokenExpiresIn: string;
  bcryptRounds: number;
}

interface ExternalServiceConfig {
  sendgrid?: { apiKey: string; fromEmail: string };
  stripe?: { apiKey: string; webhookSecret: string };
  openai?: { apiKey: string; model: string };
}
```

### Step 4: Environment-Specific Configuration

Document configuration differences per environment:

```markdown
## Environment-Specific Configuration

### Development
- .env file: .env.development
- Database: Local PostgreSQL
- Log level: debug
- Mock external services: true
- Hot reload: enabled

### Staging
- .env file: .env.staging
- Database: Staging PostgreSQL
- Log level: info
- Feature flags: all enabled
- External services: sandbox mode

### Production
- .env file: .env.production (via CI/CD secrets)
- Database: Production PostgreSQL
- Log level: warn
- Feature flags: controlled
- External services: live mode
```

### Step 5: Secret Management

Document how secrets are managed:

```markdown
## Secret Management

### Development
- .env file (gitignored)
- .env.example (committed, with placeholder values)

### CI/CD
- GitHub Actions Secrets
- Accessed via ${{ secrets.SECRET_NAME }}

### Production
- AWS Secrets Manager (or equivalent)
- Injected as environment variables
- Rotated every 90 days

### Secrets Inventory
| Secret | Environment | Rotation | Storage |
|--------|-------------|----------|---------|
| JWT_SECRET | All | 90 days | .env / Secrets Manager |
| DATABASE_URL | All | On credential change | .env / Secrets Manager |
| SENDGRID_API_KEY | All | 90 days | .env / Secrets Manager |
| STRIPE_API_KEY | All | 90 days | .env / Secrets Manager |
```

### Step 6: Feature Flag Analysis

If feature flags exist:

```markdown
## Feature Flags

### Mechanism
- Environment variables: FEATURE_*
- Evaluated at startup (not runtime)
- Located in: src/config/features.ts

### Flags
| Flag | Default | Description | Target Removal |
|------|---------|-------------|----------------|
| FEATURE_NEW_CHECKOUT | false | New checkout flow | Q3 2026 |
| FEATURE_AI_SUGGESTIONS | true | AI-powered suggestions | GA |
| FEATURE_DARK_MODE | true | Dark mode toggle | GA |
| FEATURE_BETA_API | false | Beta API v2 endpoints | Q4 2026 |
```

### Step 7: Configuration Validation

Document how configuration is validated:

```markdown
## Configuration Validation

### Tool: Zod / env-var / custom validation

### Location: src/config/index.ts:50-80

### Validation Rules
- DATABASE_URL: Must be a valid PostgreSQL connection string
- JWT_SECRET: Must be at least 32 characters
- PORT: Must be between 1024 and 65535
- NODE_ENV: Must be development, staging, or production

### Startup Behavior
- If validation fails: log error and exit process
- If validation passes: freeze config object (Object.freeze)
```

## OUTPUT SPECIFICATION

### File 1: `01-config-architecture.md`

Configuration architecture documentation.

### File 2: `02-environment-variables.md`

Complete environment variable inventory.

### File 3: `03-config-schema.md`

Configuration schema documentation.

### File 4: `04-environment-specific.md`

Environment-specific configuration differences.

### File 5: `05-secret-management.md`

Secret management documentation.

### File 6: `06-feature-flags.md` (if applicable)

Feature flag documentation.

### File 7: `07-config-constants.md`

System constants and magic values.

### File 8: `08-config-summary.md`

Summary including:
- Configuration complexity assessment
- Security assessment (secrets exposure risk)
- Configuration best practices compliance
- Recommendations

## VALIDATION CHECKS

- [ ] All environment variables are documented
- [ ] Each variable has required/default/description
- [ ] Configuration schema is documented
- [ ] Environment differences are documented
- [ ] Secret management is documented
- [ ] Feature flags are documented (if applicable)
- [ ] Configuration validation is documented

## COMPLETION CHECKLIST

- [ ] All 8 output files generated
- [ ] Environment variables inventoried
- [ ] Configuration schema documented
- [ ] Environment differences captured
- [ ] Secret management documented
- [ ] Feature flags cataloged (if applicable)
- [ ] Constants documented
- [ ] All outputs saved to `re-docs/17-config-env/`
- [ ] Phase validation checks passed

---

*Proceed to PROMPT_19_DOCUMENTATION.md only after all checklist items are complete.*
