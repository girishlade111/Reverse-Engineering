# PROMPT_21: Technical Reference Documentation Generation

## Classification
- **Domain:** Documentation Generation
- **Phase:** 5 — Documentation Production
- **Prerequisites:** All Phase 1-4 prompts (01-19)
- **Dependencies:** Complete analysis context
- **Estimated Effort:** Very High

---

## Objective

Generate comprehensive technical reference documentation covering all APIs, data models, configuration options, error codes, and technical interfaces in the system.

---

## Input Requirements

### Required Context
- All analysis artifacts from Phase 1-4
- API endpoints and handlers from component analysis
- Data models and schemas
- Configuration specifications
- Error codes and error handling patterns

---

## Analysis Tasks

### Task 1: API Reference Documentation

**Purpose:** Generate complete API reference documentation.

**Instructions:**
1. Document every API endpoint with:
   - HTTP method and path
   - Request/response schemas
   - Authentication requirements
   - Rate limiting information
   - Error responses
   - Example requests/responses

**Output Format:**

```markdown
## API Reference

### Authentication
All API requests require Bearer token authentication except:
- POST /api/v1/auth/login
- POST /api/v1/auth/register

### Endpoints

#### POST /api/v1/users
Create a new user.

**Authentication:** Bearer token (admin only)
**Rate Limit:** 10 requests per minute

**Request Body:**
```json
{
    "email": "user@example.com",
    "password": "securePassword123",
    "name": "John Doe",
    "role": "customer"
}
```

**Response (201):**
```json
{
    "id": "uuid",
    "email": "user@example.com",
    "name": "John Doe",
    "role": "customer",
    "created_at": "2024-01-15T10:30:00Z"
}
```

**Error Responses:**
| Status | Code | Description |
|--------|------|-------------|
| 400 | VALIDATION_ERROR | Invalid input data |
| 401 | UNAUTHORIZED | Missing or invalid token |
| 403 | FORBIDDEN | Insufficient permissions |
| 409 | DUPLICATE_EMAIL | Email already exists |

**Example:**
```bash
curl -X POST https://api.example.com/api/v1/users \
  -H "Authorization: Bearer <token>" \
  -H "Content-Type: application/json" \
  -d '{"email":"user@example.com","password":"securePass123","name":"John Doe"}'
```
```

---

### Task 2: Data Model Reference

**Purpose:** Document all data models and schemas.

**Instructions:**
1. Document every data model with:
   - Entity name and description
   - All fields with types and constraints
   - Relationships to other entities
   - Indexes and constraints
   - Sample data

**Output Format:**

```markdown
## Data Model Reference

### Entity: User
| Field | Type | Constraints | Default | Description |
|-------|------|-------------|---------|-------------|
| id | UUID | PK, Auto-generated | - | Unique identifier |
| email | VARCHAR(255) | UNIQUE, NOT NULL | - | User email address |
| password_hash | VARCHAR(255) | NOT NULL | - | Bcrypt password hash |
| name | VARCHAR(100) | NOT NULL | - | User display name |
| role | ENUM('admin','customer') | NOT NULL | 'customer' | User role |
| is_active | BOOLEAN | NOT NULL | true | Account active status |
| created_at | TIMESTAMP | NOT NULL | NOW() | Creation timestamp |
| updated_at | TIMESTAMP | NOT NULL | NOW() | Last update timestamp |

**Relationships:**
- Has many: Orders (via user_id foreign key)
- Has many: Addresses (via user_id foreign key)

**Indexes:**
- PRIMARY KEY (id)
- UNIQUE INDEX idx_users_email (email)
- INDEX idx_users_role (role)
```

---

### Task 3: Configuration Reference

**Purpose:** Document every configuration option.

**Instructions:**
1. Document every configuration setting with:
   - Key name
   - Type and default value
   - Description and purpose
   - Valid values
   - Environment variable mapping

**Output Format:**

```markdown
## Configuration Reference

### Application Settings
| Key | Type | Default | Description | Valid Values | Env Variable |
|-----|------|---------|-------------|--------------|--------------|
| DEBUG | boolean | false | Enable debug mode | true, false | DEBUG |
| SECRET_KEY | string | - | JWT signing key | Minimum 32 chars | SECRET_KEY |
| DATABASE_URL | string | - | PostgreSQL connection | Valid connection string | DATABASE_URL |
| LOG_LEVEL | string | INFO | Logging verbosity | DEBUG, INFO, WARNING, ERROR | LOG_LEVEL |
```

---

### Task 4: Error Code Reference

**Purpose:** Document all error codes and their meanings.

**Instructions:**
1. Document every error code with:
   - Error code identifier
   - HTTP status code
   - Description
   - Possible causes
   - Resolution steps

**Output Format:**

```markdown
## Error Code Reference

| Error Code | HTTP Status | Description | Common Causes | Resolution |
|------------|-------------|-------------|---------------|------------|
| VALIDATION_ERROR | 400 | Input validation failed | Missing required fields, invalid format | Check request body against API spec |
| UNAUTHORIZED | 401 | Authentication failed | Missing/expired token | Obtain valid token via /auth/login |
| FORBIDDEN | 403 | Insufficient permissions | User role insufficient | Request elevation or use different account |
| NOT_FOUND | 404 | Resource not found | Invalid ID, deleted resource | Verify resource ID |
| RATE_LIMITED | 429 | Too many requests | Exceeded rate limit | Wait and retry with backoff |
```

---

## Output Requirements
### Required Deliverables
1. Complete API reference with all endpoints
2. Data model reference with all entities
3. Configuration reference with all settings
4. Error code reference

### Output Structure
```
DOCUMENTATION_TECHNICAL/
├── api_reference.md
├── data_models.md
├── configuration_reference.md
└── error_codes.md
```

---

## Cross-References
- **Previous Prompt:** PROMPT_20_DOCUMENTATION_ARCHITECTURE.md
- **Next Prompt:** PROMPT_22_DOCUMENTATION_DEVELOPER.md
- **Shared Context Key:** documentation.technical
