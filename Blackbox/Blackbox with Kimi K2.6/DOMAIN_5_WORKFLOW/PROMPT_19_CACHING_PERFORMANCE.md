# PROMPT_19: Caching & Performance Analysis

## Classification
- **Domain:** Workflow & Execution Analysis
- **Phase:** 4 — Workflow Analysis
- **Prerequisites:** PROMPT_15, PROMPT_18
- **Dependencies:** Pipeline analysis, tool integration analysis
- **Estimated Effort:** Medium

---

## Objective

Identify and document every caching mechanism, performance optimization pattern, caching strategy, cache invalidation policy, and performance-critical code path in the repository.

---

## Input Requirements

### Required Context
- Execution pipelines from PROMPT_15
- Tool integrations from PROMPT_18
- Data flow mappings from PROMPT_08
- Configuration from PROMPT_04

---

## Pre-Analysis Checklist
- [ ] PROMPT_15, PROMPT_18 completed
- [ ] All cache-related code identified
- [ ] All performance-sensitive paths identified

---

## Analysis Tasks

### Task 1: Caching Architecture Analysis

**Purpose:** Document the complete caching architecture.

**Instructions:**
1. Identify caching layers:
   - **Application cache:** In-memory caching (dict, LRU cache)
   - **Distributed cache:** Redis, Memcached
   - **HTTP cache:** Browser cache, CDN
   - **Database cache:** Query cache, materialized views
2. For each cache layer, document:
   - Cache technology and configuration
   - Cache key structure
   - Cache value format and serialization
   - TTL and eviction policy
   - Cache invalidation strategy
   - Cache hit/miss behavior

**Output Format:**

```markdown
## Caching Architecture

### Cache Layers
| Layer | Technology | Location | Capacity | Eviction | Serialization |
|-------|------------|----------|----------|----------|---------------|
| L1: Application | Python dict (LRU) | In-process | 1000 items | LRU | Native objects |
| L2: Distributed | Redis 7 | Separate server | 1GB | allkeys-lru | JSON |
| L3: HTTP | CDN (CloudFront) | Edge network | Unlimited | TTL-based | N/A |
| L4: Database | PostgreSQL query cache | Database | Automatic | Query-based | N/A |

### Cache Key Structure
| Cache | Key Pattern | Example |
|-------|-------------|---------|
| Application | {module}:{entity}:{id} | user:profile:12345 |
| Redis | app:{env}:{type}:{key} | app:prod:user:12345 |
| HTTP | URL path | /api/v1/users/12345 |

### TTL & Eviction
| Cache | Default TTL | Max TTL | Eviction Policy | Invalidation Trigger |
|-------|-------------|---------|-----------------|---------------------|
| Application | 5 minutes | 30 minutes | LRU | Write-through |
| Redis | 1 hour | 24 hours | allkeys-lru | Explicit delete |
| CDN | 1 hour | 24 hours | TTL-based | Cache purge API |
```

---

### Task 2: Cache Strategy Documentation

**Purpose:** Document each caching strategy used.

**Instructions:**
1. Identify caching strategies:
   - **Cache-aside:** Application reads cache, fills on miss
   - **Read-through:** Cache layer handles reads
   - **Write-through:** Writes go to cache and DB simultaneously
   - **Write-behind:** Writes go to cache, async write to DB
   - **Refresh-ahead:** Cache refreshes before expiry
2. For each strategy, document:
   - Where it's used
   - Implementation details
   - Consistency guarantees
   - Failure modes

**Output Format:**

```markdown
## Caching Strategies

### Strategy 1: Cache-Aside (Lazy Loading)
| Aspect | Detail |
|--------|--------|
| **Usage** | User profiles, product catalog |
| **Location** | src/cache/user_cache.py, src/cache/product_cache.py |
| **Consistency** | Eventual (data may be stale until next read) |
| **Failure Mode** | Cache miss -> DB read -> populate cache (degraded but functional) |

#### Implementation
```python
async def get_user(user_id: UUID) -> User:
    # Try cache first
    cached = await redis.get(f"user:{user_id}")
    if cached:
        return deserialize_user(cached)
    
    # Cache miss - read from DB
    user = await user_repository.find(user_id)
    
    # Populate cache
    await redis.setex(f"user:{user_id}", 3600, serialize_user(user))
    
    return user
```

### Strategy 2: Write-Through
| Aspect | Detail |
|--------|--------|
| **Usage** | Session data, configuration |
| **Location** | src/cache/session_cache.py |
| **Consistency** | Strong (DB and cache updated together) |
| **Failure Mode** | Cache write fails -> DB write still succeeds (cache stale) |
```

---

### Task 3: Performance Bottleneck Analysis

**Purpose:** Identify and document performance bottlenecks.

**Instructions:**
1. Identify performance-critical paths:
   - High-traffic endpoints
   - Data-intensive operations
   - Compute-heavy algorithms
   - External API calls
2. For each bottleneck, document:
   - Current performance (if measurable from code)
   - Root cause
   - Optimization opportunities
   - Expected improvement

**Output Format:**

```markdown
## Performance Bottleneck Analysis

### Top Bottlenecks
| Bottleneck | Location | Impact | Root Cause | Optimization | Est. Improvement |
|------------|----------|--------|------------|--------------|------------------|
| Auth token validation | AuthMiddleware | +50ms/req | JWT decode + DB lookup | Cache valid tokens | 80% reduction |
| Product search | ProductService | +200ms/req | Full DB scan | Add search index | 95% reduction |
| Report generation | ReportService | +30s/report | Synchronous PDF generation | Async generation | 100% (non-blocking) |

### Performance Metrics
| Metric | Current | Target | Gap |
|--------|---------|--------|-----|
| P95 response time | 500ms | 200ms | 300ms |
| P99 response time | 2s | 500ms | 1.5s |
| Cache hit rate | 75% | 95% | 20% |
| DB query time (avg) | 50ms | 20ms | 30ms |
```

---

### Task 4: Performance Optimization Recommendations

**Purpose:** Provide actionable performance optimization recommendations.

**Instructions:**
1. For each identified bottleneck, provide:
   - Specific optimization
   - Implementation effort estimate
   - Expected performance improvement
   - Risk assessment
2. Prioritize optimizations by impact/effort ratio

**Output Format:**

```markdown
## Performance Optimization Recommendations

### Priority Matrix
| Priority | Optimization | Effort | Impact | Risk |
|----------|--------------|--------|--------|------|
| P0 | Add Redis caching for auth tokens | 2 days | High | Low |
| P0 | Add database index for product search | 1 day | High | Low |
| P1 | Implement async report generation | 5 days | Medium | Medium |
| P1 | Add connection pooling for database | 0.5 day | Medium | Low |
| P2 | Implement query optimization for dashboard | 3 days | Low | Medium |
```

---

## Synthesis
**Purpose:** Create a comprehensive caching and performance reference.

**Output Format:**

```markdown
## Caching & Performance Summary

| Layer | Technology | Hit Rate | Miss Penalty | Strategy |
|-------|------------|----------|--------------|----------|
| L1 App Cache | Python dict | 60% | 5ms | Cache-aside |
| L2 Distributed | Redis | 85% | 1ms | Cache-aside + Write-through |
| L3 HTTP/CDN | CloudFront | 90% | 50ms | TTL-based |
| L4 Database | PostgreSQL | 70% | 20ms | Query cache |

### Key Metrics
- **Current P95:** 500ms (Target: 200ms)
- **Cache Hit Rate:** 75% (Target: 95%)
- **Top Bottleneck:** Auth token validation
- **Quick Wins:** 3 optimizations (P0 priority)
```

---

## Output Requirements
### Required Deliverables
1. Caching architecture documentation
2. Cache strategy documentation with implementation details
3. Performance bottleneck analysis
4. Performance optimization recommendations

---

## Cross-References
- **Previous Prompt:** PROMPT_18_TOOL_INTEGRATION.md
- **Next Prompt:** PROMPT_20_DOCUMENTATION_ARCHITECTURE.md
- **Shared Context Key:** caching.architecture, caching.strategies, performance.bottlenecks
