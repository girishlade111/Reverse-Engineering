# Template: API Documentation

> **Document:** templates/TEMPLATE_API_DOC.md  
> **Version:** 1.0.0  
> **Purpose:** Template for documenting APIs (REST, GraphQL, gRPC, etc.)  
> **When to Use:** During Phase 9 when the repository exposes APIs

---

## 📋 STRUCTURE

```markdown
# API Reference: [API Name]

> **Document:** [relative-path.md]
> **Phase:** API Documentation
> **Last Updated:** [YYYY-MM-DD]
> **Purpose:** Document the [API name] API

---

## Overview

- **API Style:** REST / GraphQL / gRPC / WebSocket / SOAP
- **Base URL:** [Base URL]
- **Authentication:** [Auth method]
- **Versioning:** [Versioning strategy]
- **Content Type:** [Request/Response format]

## Authentication

[Description of authentication mechanism]

```[language]
// Example authentication
```

## Endpoints

### [HTTP Method] [path]

**Purpose:** [What this endpoint does]

**Request:**
```[language]
{
    "param1": "type - description",
    "param2": "type - description"
}
```

**Response (200):**
```[language]
{
    "field1": "type - description",
    "field2": "type - description"
}
```

**Error Responses:**
| Status Code | Condition | Response |
|-------------|-----------|----------|
| 400 | Invalid input | [Error response] |
| 401 | Unauthenticated | [Error response] |
| 403 | Unauthorized | [Error response] |
| 404 | Not found | [Error response] |
| 500 | Server error | [Error response] |

**Rate Limiting:** [Rate limit info]

**Implementation:**
- **File:** [file:line]
- **Handler:** [function name]
- **Service:** [service function]
- **Validation:** [validation rules]

### [HTTP Method] [path]

[Same structure for each endpoint]

## Data Models

### [Model Name]
```[language]
{
    "field1": "type - description",
    "field2": "type - description"
}
```

## Implementation Details

- **Controller/Handler:** [path/to/handler]
- **Service Layer:** [path/to/service]
- **Validation:** [path/to/validator]
- **Serialization:** [path/to/serializer]

## Confidence Assessment

- **API Confidence:** [High/Medium/Low]
- **Uncertainties:** [List]

## Cross-References

- [Architecture Document](path/to/architecture.md)
- [Component Document](path/to/component.md)
```

---

## 📝 USAGE GUIDELINES

1. **Scope:** Use for each distinct API (REST service, GraphQL schema, gRPC service).
2. **Completeness:** Document every endpoint with request/response formats.
3. **Error Handling:** Always include error response documentation.
4. **Examples:** Include at least one example request/response per endpoint.
5. **Implementation Reference:** Link from API endpoints to implementation files.

---

## ✅ QUALITY CHECKLIST

- [ ] API style and base URL documented
- [ ] Authentication mechanism documented
- [ ] All endpoints documented
- [ ] Request/response formats documented
- [ ] Error responses documented
- [ ] Rate limiting documented (if applicable)
- [ ] Data models documented
- [ ] Implementation files referenced

