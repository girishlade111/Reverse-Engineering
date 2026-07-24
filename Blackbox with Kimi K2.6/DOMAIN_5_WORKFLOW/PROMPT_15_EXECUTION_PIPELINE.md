# PROMPT_15: Execution Pipeline Reconstruction

## Classification
- **Domain:** Workflow & Execution Analysis
- **Phase:** 4 — Workflow Analysis
- **Prerequisites:** All Phase 3 prompts (10-14)
- **Dependencies:** Class analysis, algorithms, patterns, errors, state management
- **Estimated Effort:** High

---

## Objective

Reconstruct every execution pipeline in the system, from entry points through all processing stages to completion. Document the sequence of operations, data transformations, conditional branching, and parallel execution paths.

---

## Input Requirements

### Required Context
- Data flow mappings from PROMPT_08
- Class/function catalog from PROMPT_10
- Algorithm documentation from PROMPT_11
- State management from PROMPT_14

---

## Pre-Analysis Checklist
- [ ] All Phase 3 prompts completed
- [ ] Entry points identified
- [ ] Processing stages identified

---

## Analysis Tasks

### Task 1: Pipeline Identification

**Purpose:** Identify all execution pipelines in the system.

**Instructions:**
1. Identify execution pipelines:
   - **Request pipelines:** HTTP request -> middleware -> handler -> service -> response
   - **Data pipelines:** Data ingestion -> transformation -> validation -> storage
   - **Batch pipelines:** Scheduled jobs -> processing -> output generation
   - **Event pipelines:** Event emission -> propagation -> handling -> side effects
2. For each pipeline, document:
   - Entry point and trigger
   - Processing stages in order
   - Stage inputs and outputs
   - Stage dependencies
   - Error handling at each stage

**Output Format:**

```markdown
## Execution Pipelines

### Pipeline: HTTP Request Processing
```mermaid
graph LR
    A[HTTP Request] --> B[Rate Limiter]
    B --> C[Auth Middleware]
    C --> D[Request Validator]
    D --> E[Route Handler]
    E --> F[Service Layer]
    F --> G[Data Access]
    G --> H[Response Formatter]
    H --> I[HTTP Response]
    
    style A fill:#4CAF50
    style I fill:#2196F3
```

### Pipeline Stages
| Stage | Component | Input | Output | Error Handling |
|-------|-----------|-------|--------|----------------|
| 1. Rate Limiter | RateLimitMiddleware | HTTP Request | Request or 429 | Returns 429 if exceeded |
| 2. Auth | AuthMiddleware | Request + Token | AuthContext or 401 | Returns 401 if invalid |
| 3. Validator | RequestValidator | Request body | ValidatedData | Returns 400 with errors |
| 4. Handler | Route Handler | ValidatedData | ServiceInput | Returns 500 on error |
| 5. Service | Service Layer | ServiceInput | ServiceOutput | Returns 500 on error |
| 6. Data | Data Access | Query | Result | Returns 404 if not found |
| 7. Response | ResponseFormatter | ServiceOutput | HTTP Response | Always succeeds |
```

---

### Task 2: Pipeline Stage Deep Analysis

**Purpose:** Document each pipeline stage with complete detail.

**Instructions:**
1. For each pipeline stage, document:
   - Implementation details
   - Configuration options
   - Performance characteristics
   - Resource requirements
   - Error states and recovery

**Output Format:**

```markdown
## Stage Analysis: Rate Limiter

### Implementation
| Aspect | Detail |
|--------|--------|
| **Location** | src/middleware/rate_limiter.py |
| **Algorithm** | Sliding window counter |
| **Storage** | Redis sorted sets |
| **Window** | 60 seconds |

### Configuration
| Setting | Default | Effect |
|---------|---------|--------|
| RATE_LIMIT_REQUESTS | 100 | Max requests per window |
| RATE_LIMIT_WINDOW | 60 | Window duration in seconds |

### Performance
- **Overhead:** < 5ms per request
- **Redis Calls:** 2 per request (ZADD + ZREMRANGEBYSCORE)
- **Memory:** ~100 bytes per request entry

### Error States
| State | Behavior | Recovery |
|-------|----------|----------|
| Redis unavailable | Allow request, log warning | Auto-recover when Redis available |
| Rate exceeded | Return 429 with Retry-After header | Wait until window resets |
```

---

### Task 3: Pipeline Optimization & Bottleneck Analysis

**Purpose:** Identify bottlenecks and optimization opportunities.

**Instructions:**
1. For each pipeline, identify:
   - Sequential dependencies (must run in order)
   - Parallelization opportunities
   - Caching opportunities
   - Redundant operations
   - Performance bottlenecks

**Output Format:**

```markdown
## Pipeline Optimization Analysis

### Bottlenecks
| Pipeline | Bottleneck | Location | Impact | Optimization |
|----------|------------|----------|--------|--------------|
| HTTP Request | Auth token validation | AuthMiddleware | +50ms | Cache valid tokens |
| Data Pipeline | Data transformation | Transformer | +200ms | Parallelize chunks |
| Report Generation | PDF generation | ReportService | +5s | Async generation |

### Parallelization Opportunities
| Pipeline | Stage 1 | Stage 2 | Can Parallelize? | Savings |
|----------|---------|---------|------------------|---------|
| Order Processing | Payment | Inventory | Yes | 30% |
| User Registration | Email validation | Username check | Yes | 20% |
```

---

## Synthesis
**Purpose:** Create a comprehensive pipeline reference.

**Output Format:**

```markdown
## Execution Pipeline Summary

| Pipeline | Entry Point | Stages | Critical Path | Est. Duration | Bottleneck |
|----------|-------------|--------|---------------|---------------|------------|
| HTTP Request | FastAPI router | 7 | Limiter->Auth->Handler | 200ms | Auth |
| Data Ingestion | File watcher | 5 | Parse->Validate->Store | 1s | Validation |
| Batch Report | Cron job | 4 | Query->Transform->Generate | 30s | Generation |
```

---

## Output Requirements
### Required Deliverables
1. Pipeline identification and stage documentation
2. Stage deep analysis
3. Bottleneck and optimization analysis

---

## Cross-References
- **Previous Prompt:** PROMPT_14_STATE_MANAGEMENT.md
- **Next Prompt:** PROMPT_16_EVENT_WORKFLOW.md
- **Shared Context Key:** pipelines.identified, pipelines.stages, pipelines.optimization
