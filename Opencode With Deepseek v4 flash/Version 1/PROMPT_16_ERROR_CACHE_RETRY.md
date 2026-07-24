# PROMPT_16 — Phase 15: Error Handling, Caching & Retry Strategy Analysis

## PHASE CLASS: Reliability Analysis
## DEPENDENCIES: PROMPT_15 (State & Events) — complete
## OUTPUT DIRECTORY: `re-docs/15-error-cache-retry/`

---

## OBJECTIVE

Document every error handling strategy, caching mechanism, retry policy, and reliability pattern in the system. Assess the robustness and fault tolerance of the application.

## PREREQUISITES

- [ ] PROMPT_15 completed
- [ ] Code is deeply read
- [ ] Data flows are traced
- [ ] Control flow is understood

## INPUTS

- `re-docs/06-deep-read/03-function-catalog.md`
- `re-docs/09-call-graph/04-error-propagation.md`
- `re-docs/13-api-boundaries/07-middleware-catalog.md`
- Full source code

## ANALYSIS STEPS

### Step 1: Error Handling Strategy Documentation

Document how errors are handled throughout the system:

```markdown
## Error Handling Strategy

### Global Error Handler
- **Location**: src/middleware/errorHandler.ts
- **Catchment**: All unhandled errors from route handlers
- **Behavior**: 
  1. Log error with stack trace (production: log only, dev: console.error)
  2. Map error type to HTTP status code
  3. Sanitize error for client response (strip internal details)
  4. Return formatted error response

### Error Types
| Error Class | HTTP Status | Description | Location |
|------------|-------------|-------------|----------|
| ValidationError | 400 | Input validation failed | src/errors/validation.ts |
| AuthenticationError | 401 | Authentication failed | src/errors/auth.ts |
| AuthorizationError | 403 | Insufficient permissions | src/errors/auth.ts |
| NotFoundError | 404 | Resource not found | src/errors/http.ts |
| ConflictError | 409 | Resource conflict | src/errors/http.ts |
| RateLimitError | 429 | Rate limit exceeded | src/errors/http.ts |
| InternalError | 500 | Unexpected error | src/errors/http.ts |
```

### Step 2: Retry Strategy Analysis

Document all retry mechanisms:

```markdown
## Retry Strategy: External API Calls

### Location: src/infrastructure/retry.ts

### Strategy: Exponential Backoff with Jitter

```typescript
async function withRetry<T>(
  fn: () => Promise<T>,
  options: RetryOptions = defaultOptions
): Promise<T> {
  let lastError: Error;
  for (let attempt = 1; attempt <= options.maxRetries; attempt++) {
    try {
      return await fn();
    } catch (error) {
      lastError = error;
      if (!isRetryable(error)) throw error;
      if (attempt === options.maxRetries) throw error;
      await delay(calculateBackoff(attempt, options));
    }
  }
  throw lastError;
}
```

### Configuration
| Parameter | Default | Description |
|-----------|---------|-------------|
| maxRetries | 3 | Maximum retry attempts |
| baseDelay | 1000ms | Base delay between retries |
| maxDelay | 30000ms | Maximum delay cap |
| multiplier | 2 | Exponential multiplier |
| jitter | 0.1 | Random jitter factor (±10%) |

### Retryable Errors
- Network timeout (ECONNABORTED)
- Connection refused (ECONNREFUSED)
- Rate limited (429)
- Server error (5xx)

### Non-Retryable Errors
- Client error (4xx except 429)
- Validation error
- Authentication error
```

### Step 3: Caching Strategy Analysis

Document all caching mechanisms:

```markdown
## Cache Layer: Multi-Level Cache

### Level 1: In-Memory Cache

- **Location**: src/cache/memory.ts
- **Technology**: NodeCache
- **TTL**: 5 minutes (configurable)
- **Max Entries**: 1000
- **Eviction**: LRU (Least Recently Used)
- **Usage**: Frequently accessed user data, configuration

### Level 2: Redis Cache

- **Location**: src/cache/redis.ts
- **Technology**: Redis 7
- **TTL**: 1 hour (configurable)
- **Usage**: Session data, rate limiter state, job queue

### Cache-Aside Pattern
```typescript
async function getUser(id: string): Promise<User> {
  // Try cache first
  const cached = await cache.get(`user:${id}`);
  if (cached) return JSON.parse(cached);
  
  // Cache miss - get from database
  const user = await db.query('SELECT * FROM users WHERE id = $1', [id]);
  
  // Store in cache
  await cache.set(`user:${id}`, JSON.stringify(user), 'EX', 300);
  
  return user;
}
```

### Cache Invalidation
| Trigger | Cache Key Pattern | Strategy |
|---------|------------------|----------|
| User update | user:* | Delete (passive invalidation) |
| Config change | config:* | Delete (manual trigger) |
| Session expiry | session:* | TTL expiration |
```

### Step 4: Timeout and Deadline Analysis

Document all timeout configurations:

```markdown
## Timeout Configuration

| Operation | Timeout | Configuration Location |
|-----------|---------|----------------------|
| HTTP request | 30s | src/config/app.ts:15 |
| Database query | 5s | src/config/database.ts:20 |
| External API call | 10s | src/config/external.ts:10 |
| Session | 24h | src/config/auth.ts:30 |
| Idle connection | 60s | src/config/database.ts:25 |
```

### Step 5: Circuit Breaker Analysis (if applicable)

```markdown
## Circuit Breaker: External Payment API

### Location: src/infrastructure/circuitBreaker.ts

### States
- CLOSED: Normal operation, requests pass through
- OPEN: Failures threshold exceeded, requests fail fast
- HALF_OPEN: Testing if service recovered

### Configuration
- Failure threshold: 5 failures in 60 seconds
- Reset timeout: 30 seconds (time before HALF_OPEN)
- Half-open max requests: 3

### Behavior on OPEN
- Immediate failure (no network call)
- Error: "Service temporarily unavailable"
- Client receives 503
```

### Step 6: Fallback Strategy Analysis

Document all fallback mechanisms:

```markdown
## Fallback: Email Service

### Primary: SendGrid API
### Fallback: SMTP direct send
### Trigger: SendGrid API failure after 3 retries
### Implementation: src/email/service.ts:50-80
```

### Step 7: Graceful Degradation

Document degradation behavior:

```markdown
## Graceful Degradation

| Service Failure | Degraded Behavior |
|-----------------|-------------------|
| Database down | Read-only mode (cached data only) |
| Cache down | Bypass cache, direct DB access |
| Email down | Queue emails for later delivery |
| Payment down | Disable checkout, show maintenance message |
```

## OUTPUT SPECIFICATION

### File 1: `01-error-handling.md`

Complete error handling documentation.

### File 2: `02-retry-strategies.md`

All retry strategies with configuration.

### File 3: `03-caching-strategy.md`

Complete caching documentation.

### File 4: `04-timeouts-and-deadlines.md`

Timeout configuration documentation.

### File 5: `05-circuit-breakers.md` (if applicable)

Circuit breaker documentation.

### File 6: `06-fallbacks.md`

Fallback mechanism documentation.

### File 7: `07-reliability-summary.md`

Summary including:
- Error handling coverage assessment
- Retry strategy effectiveness
- Cache hit rate estimation
- Fault tolerance assessment
- Reliability recommendations

## VALIDATION CHECKS

- [ ] Global error handler is documented
- [ ] All error types/classes are documented
- [ ] Retry strategies are documented with configuration
- [ ] All caching layers are documented
- [ ] Timeout configurations are documented
- [ ] Fallback mechanisms are documented
- [ ] Graceful degradation behavior is documented

## COMPLETION CHECKLIST

- [ ] All 7 output files generated
- [ ] Error handling documented
- [ ] Retry strategies documented
- [ ] Caching documented
- [ ] Timeouts documented
- [ ] Circuit breakers documented
- [ ] Fallbacks documented
- [ ] Reliability assessed
- [ ] All outputs saved to `re-docs/15-error-cache-retry/`
- [ ] Phase validation checks passed

---

*Proceed to PROMPT_17_AI_WORKFLOWS.md only after all checklist items are complete.*
