# PROMPT_18.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_18: Caching & Performance Strategy

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_18
- **Phase:** 2
- **Stage:** 8 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-11 (PROMPT_11), ART-13 (PROMPT_13).
- **Estimated Tokens:** 10000–16000
- **Output Artifacts:** ART-18 (Doc) — Caching & Performance Report.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Caching & Performance Report artifact (ART-18) that identifies every cache in the subject repository (in-memory caches, distributed caches, HTTP caches, CDN, query caches, computed-value caches / memoization), records for each cache its key, value, TTL, invalidation strategy, and eviction policy, identifies every performance-critical path, detects every N+1 query pattern, detects every batched operation, classifies every load as lazy or eager, and detects every prefetching mechanism.

---

## 3. When to Invoke

PROMPT_18 is dispatched when ALL of the following predicates hold:

- Phase 1 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.2.
- ART-01, ART-09, ART-10, ART-11, and ART-13 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-13 records at least one stateful unit with `kind: cache` OR ART-09 records at least one function with side-effect kind `cache-write` OR the in-scope source contains at least one cache-library usage pattern (else `NO_CACHES` and the prompt emits a minimal report with that finding).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-09 | Doc | Function side-effect records; functions with `cache-write`/`cache-read` side effects are cache-operation candidates. Function complexity and call frequency (inferred from ART-10's `in_degree`) identify performance-critical paths. |
| ART-10 | Graph | Call graph and critical paths; the critical paths from ART-10 seed the performance-critical path identification in § 6.7. |
| ART-11 | Graph | Data-flow diagrams; cached data types are identified from ART-11's `significant_data_types`. Cache invalidation flows are traced from ART-11's mutation points. |
| ART-13 | Doc | Stateful-unit catalog; units with `kind: cache` are the cache candidates. Mutation points on cached variables are invalidation events. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid graph conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect caches per § 6.1 by category (in-memory, distributed, HTTP, CDN, query, memoization).
3. For every cache, extract key, value, TTL, invalidation strategy, and eviction policy per § 6.2.
4. Infer invalidation strategies per § 6.3 (TTL-based, write-through, write-around, write-back, explicit-invalidate, event-driven).
5. Identify eviction policies per § 6.4 (LRU, LFU, FIFO, random, TTL-only, size-based).
6. Detect N+1 query patterns per § 6.5 by scanning loops containing persistence calls.
7. Detect batched operations per § 6.6 by scanning for batch APIs (`findByIdIn`, `bulkCreate`, `WHERE IN`).
8. Classify lazy vs eager loading per § 6.7 from access patterns and initialization sites.
9. Detect prefetching per § 6.8 from explicit prefetch calls and warming routines.
10. Identify performance-critical paths per § 6.9 by combining ART-10's critical paths with cache-miss branches.
11. Identify performance hotspots per § 6.10 from high-complexity functions on critical paths.
12. Emit Mermaid diagrams per § 6.11 with cache-flow citations.
13. Cross-check the cache catalog against ART-13's `kind: cache` units per § 6.12; unaccounted caches are `CONTRADICTION` findings per R33.
14. Emit ART-18 per § 8 with full front-matter, per-cache sections, N+1 and batch catalog, lazy/eager classification, performance-hotspot register, traceability index, open questions.
15. Run the Quality Checks in § 9.
16. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Cache Detection by Category

Detect caches by category, using ART-13's `kind: cache` units as the seed.

**In-memory caches** — caches stored in process memory. Detected by: `lru-cache` (Node.js), `node-cache`, `memory-cache`, `quick-lru`, `Map`/`WeakMap` used as caches (detected by `set`/`get` patterns and bounded size), `functools.lru_cache`/`@cache` (Python), `cachetools` (Python), `concurrent.HashMap` with TTL (Java), `Caffeine` (Java), `Guava Cache` (Java), `MemoryCache`/`IMemoryCache` (.NET), `LazyCache` (.NET), `sync.Map` with TTL patterns (Go), `ristretto`/`bigcache`/`fastcache` (Go), `moka`/`cached`/`lru` (Rust), `ThreadSafe::Cache` (Ruby), `LRUCache` (Ruby stdlib).

**Distributed caches** — caches stored in a shared cache service. Detected by: Redis client usage (`ioredis`, `node-redis`, `redis-py`, `jedis`, `lettuce`, `go-redis`, `redis-rs`, `redis-rb`, `StackExchange.Redis`) with `SET`/`GET` patterns, Memcached client usage (`memcached`, `memcache-client`, `spymemcached`, `gomemcached`), Hazelcast, Apache Ignite, Infinispan, AWS ElastiCache, Google Memorystore, Azure Cache for Redis.

**HTTP caches** — caches at the HTTP layer. Detected by: `Cache-Control` headers (`public`, `private`, `max-age`, `s-maxage`, `no-cache`, `no-store`), `ETag` headers, `Last-Modified`/`If-Modified-Since`, `Vary` headers, HTTP-cache middleware (`apicache`, `express-cache-controller`, `cache-control` middleware), CDN-managed cache headers, `Cache-Control: stale-while-revalidate`, `Cache-Control: immutable`.

**CDN caches** — caches at the content delivery network. Detected by: CDN configuration in IaC (CloudFront, Cloudflare, Fastly, Akamai), `cdn.com` asset URLs, CDN-specific headers (`X-Cache: HIT`, `CF-Cache-Status`, `X-Amz-Cf-Id`), edge function caching (`Cloudflare Workers`, `Lambda@Edge`, `Fastly Compute@Edge`).

**Query caches** — caches at the database/ORM layer. Detected by: Hibernate second-level cache (`@Cache`), EF Core query cache (`MemoryCache` integration), Prisma's client-side cache extension, Django's `cache_page` decorator, Rails's `Rails.cache` integration with query results, MyBatis second-level cache, MySQL query cache (deprecated, detected by config).

**Memoization (computed-value caches)** — caches of function return values. Detected by: `@functools.lru_cache`, `@functools.cache`, `memoize`/`memoizee` (Node.js), `lodash.memoize`, `memoize-one`, `reselect` (Redux selectors), `useMemo` (React), `computed` (Vue), `derive` (Svelte), `@Memoized` (Micronaut), `@Cacheable` (Spring), `Memoize` attribute (C# via PostSharp), `once_cell::sync::Lazy` (Rust), `OnceLock` (Rust), `lazy_static!` (Rust), `Memoize` (Ruby `memoist`).

Each cache is assigned a `CACHE-XX` identifier and records `cache_id`, `kind` (in-memory | distributed | http | cdn | query | memoization), `library`, `client_instantiation_citation`, `external: false|true` (for managed services).

### 6.2 Cache-Property Extraction

For every cache, extract its properties:

- **Key** — the cache key expression. For key-value caches: the key string (`user:${userId}`). For HTTP caches: the cache key derivation (`method + path + Accept-Encoding`). For memoization: the function's argument tuple. Records `key_expression`, `key_citation`.
- **Value** — the cached value's type. Cross-reference ART-11's `significant_data_types` when the value type is in-scope. Records `value_type`, `value_serialization` (JSON | binary | protobuf | NA for in-memory).
- **TTL** — the time-to-live. Detected by `ttl:` config, `EX`/`PX` Redis arguments, `expireAfterWrite` (Caffeine/Guava), `absoluteExpirationRelativeToNow` (.NET), `expires` config. Records `ttl_duration`, `ttl_unit`, `ttl_citation`, `default_ttl` (when per-key override is possible).
- **Max size** — for bounded caches. Detected by `max:` config, `maxSize`, `maxEntries`, `maximumSize` (Caffeine), `size:` (LRU libs). Records `max_size`, `size_unit` (entries | bytes | kilobytes).
- **Scope** — `process` (in-memory, single-instance), `distributed` (shared across instances), `request` (per-request), `session` (per-session). Records `scope`.

### 6.3 Invalidation-Strategy Inference

Infer the invalidation strategy for every cache:

- **TTL-based** — entries expire after a duration; no explicit invalidation. Detected by non-zero TTL and absence of explicit delete calls.
- **Write-through** — writes update both the cache and the underlying store synchronously. Detected by `cache.set(key, value); db.save(value)` patterns in the same function.
- **Write-around** — writes bypass the cache and go directly to the store; the cache is populated on the next read. Detected by absence of cache writes during mutation and presence of cache-fill-on-miss.
- **Write-back (write-behind)** — writes update the cache and asynchronously update the store. Detected by `cache.set(key, value); queue.publish('persist', value)` patterns or by `@CachePut` with async configuration.
- **Explicit invalidate** — code explicitly deletes cache entries on mutations. Detected by `cache.del(key)`, `cache.invalidate(key)`, `@CacheEvict`, `redis.del(key)` calls following state mutations. Records `invalidate_on` (the mutation event or function that triggers invalidation), `invalidate_citation`.
- **Event-driven invalidation** — cache entries are invalidated by event subscriptions (e.g., a Redis pub/sub message triggers invalidation across instances). Detected by event handlers that call `cache.del` (cross-reference ART-14).
- **Tag-based invalidation** — cache entries are tagged and invalidated by tag. Detected by `cache.invalidateTag('users')`, `@CacheEvict(allEntries=true, cacheNames="users")`, Cloudflare cache-tag headers (`Cache-Tag`).

Each cache records `invalidation_strategy` (one of the above or `mixed`), `invalidation_details`, `citation`.

### 6.4 Eviction-Policy Identification

Identify the eviction policy for bounded caches:

- **LRU (Least Recently Used)** — `lru-cache`, `functools.lru_cache`, `Caffeine` default, `MemoryCache` with `SizeLimit`. Detected by library defaults and explicit config.
- **LFU (Least Frequently Used)** — `lfu-cache`, `Caffeine` with `.evictionPolicy(LFU)`, `WeightedGainCache`. Detected by explicit config.
- **FIFO (First In First Out)** — `node-cache` default, simple maps with push/shift. Detected by library defaults.
- **TTL-only** — entries are not evicted by size; they expire by TTL only. Detected by absence of `maxSize` config and presence of TTL.
- **Random** — random eviction. Detected by `random-eviction` config in some libs.
- **Size-based (no policy)** — entries evicted when size exceeds a threshold; policy is unspecified or framework-default.

Each cache records `eviction_policy`, `eviction_citation`.

### 6.5 N+1 Query Pattern Detection

Detect every N+1 query pattern — a loop that issues a separate query per iteration:

1. Scan every loop (`LP-XX` from ART-12) for body calls to persistence APIs (per § 6.2 of PROMPT_11: `findUnique`, `findById`, `SELECT`, `Model.objects.get`, `User.find(...)`, `repo.findById`).
2. For each such loop, check whether the loop variable is used as the query key (e.g., `for (const userId of userIds) { const user = await User.findById(userId); ... }`). This is the N+1 pattern: 1 query to fetch the list, then N queries to fetch each item.
3. Record each N+1 with `nplusone_id` `NQ-XX`, `loop_id` `LP-XX`, `query_fn_id` `FN-XX`, `query_call_citation`, `loop_iterations_estimated` (`UNVERIFIED` or the size expression), `suggested_batch_alternative` (the batch-API equivalent if the persistence layer supports one: `findByIdIn`, `WHERE id IN (...)`, `Model.objects.filter(id__in=ids)`).

N+1 patterns are flagged with severity `MAJOR` when the loop iterations are unbounded (e.g., iterating over user input) and `MINOR` when bounded to a small constant.

### 6.6 Batched-Operation Detection

Detect every batched operation — a single API call that processes multiple items:

1. Scan for batch-API call patterns: `findByIdIn`, `findMany`, `bulkCreate`, `bulkUpdate`, `INSERT INTO ... VALUES (...), (...), (...)`, `UNNEST` arrays, `WHERE id IN (?, ?, ?)`, `Promise.all(items.map(asyncFn))` (parallel batch).
2. Scan for ORM batch APIs: Prisma `createMany`, TypeORM `bulk`/`insert`, Sequelize `bulkCreate`, Django `bulk_create`, Rails `insert_all`, SQLAlchemy `bulk_insert_mappings`, EF Core `AddRange`/`SaveChanges`, Hibernate `Session.save` in a loop flushed once.
3. Scan for queue batch APIs: Kafka producer `send` with multiple records per `flush`, RabbitMQ `publish` batching, SQS `SendMessageBatch`.

Each batched operation records `batch_id` `BT-XX`, `kind` (db-batch | queue-batch | parallel-batch | pipeline-batch), `function_id` `FN-XX`, `batch_size` (constant or expression), `batch_call_citation`.

### 6.7 Lazy vs Eager Loading Classification

Classify every load operation as lazy or eager:

- **Lazy loading** — the load is deferred until the value is first accessed. Detected by: getter functions that fetch on demand (`get user() { return this._user ?? this.fetchUser(); }`), `useLazyQuery` (Apollo), `Lazy<T>` (.NET), `Lazy` (Java), `once_cell::sync::Lazy` (Rust), ORM lazy-loading (`@ManyToOne(fetch = LAZY)`, SQLAlchemy `lazy='select'` default, Prisma's lazy relations, Sequelize `lazy` associations), `Suspense` with lazy components (React `React.lazy`).
- **Eager loading** — the load happens upfront at object construction or relation access. Detected by: `@ManyToOne(fetch = EAGER)`, Prisma `include` clause, Sequelize `include`, Rails `includes(...)`, Django `select_related`/`prefetch_related`, SQLAlchemy `joinedload`/`selectinload`, EF Core `Include(...)`.

Each load records `load_id` `LD-XX`, `kind` (lazy | eager), `loaded_entity` (the `D-XX` from ART-11), `load_fn_id` `FN-XX`, `trigger` (the access pattern that triggers the load for lazy; the construction/initialization site for eager), `citation`.

### 6.8 Prefetching Detection

Detect every prefetching mechanism — operations that load data before it is requested:

- **Link prefetching** — HTML `<link rel="prefetch">`, Next.js `next/link` prefetch behavior, Gatsby `prefetch`.
- **Route prefetching** — Next.js router `prefetch`, Ember `link-to` prefetch, SvelteKit `prefetch`, Angular resolver preloads.
- **Query prefetching** — React Query `prefetchQuery`, SWR `preload`, Apollo `prefetchQueries`, RTK Query `prefetch`.
- **Background refresh** — stale-while-revalidate patterns (SWR), refresh-ahead caches, scheduled refresh jobs.

Each prefetch records `prefetch_id` `PF-XX`, `kind`, `target` (the prefetched entity/route/query), `trigger` (user-hover | route-change | timer | idle), `function_id` `FN-XX`, `citation`.

### 6.9 Performance-Critical-Path Identification

Identify performance-critical paths by combining ART-10's critical paths with cache-miss branches:

1. Take ART-10's `critical_paths` (entry-to-leaf chains through high-traffic hubs).
2. For each path, identify cache-miss branches — branches in ART-12 whose `taken_targets` include cache-miss handlers (functions that read from a cache, miss, and then load from the underlying store).
3. Score each path by `score = betweenness_sum + 0.7 * cache_miss_count + 0.3 * N+1_count`. The cache-miss count contributes because cache misses are expensive; N+1 patterns multiply the cost.
4. Rank the top-N (N = min(15, total paths)) by score.
5. Each critical path records `pcp_id` `PCP-XX`, `entry_id`, `path_nodes`, `betweenness_sum`, `cache_miss_count`, `nplusone_count`, `score`, `citations`.

### 6.10 Performance-Hotspot Identification

Identify performance hotspots — functions on critical paths whose cost is disproportionate:

1. From ART-09's cyclomatic complexity and the call graph's `in_degree`, compute a heuristic cost score: `cost = complexity * in_degree`.
2. Identify functions on performance-critical paths (per § 6.9) whose cost score is in the top 10%.
3. Flag functions that perform synchronous I/O on critical paths (detected by sync I/O calls — `fs.readFileSync`, `requests.get` without async, `db.Query` without goroutine — on a critical-path node).
4. Flag functions that perform unbounded loops on critical paths (loop with `max_iterations_estimated: UNVERIFIED`).
5. Flag functions that call external APIs without caching on critical paths.

Each hotspot records `hotspot_id` `HS-XX`, `function_id` `FN-XX`, `critical_path_id` `PCP-XX`, `cost_score`, `flagged_reasons` (list), `citation`.

### 6.11 Mermaid Diagram Emission

Emit Mermaid diagrams per `OUTPUT_RULES.md` § 7:

- **Cache-catalog overview** — `graph LR` showing every `CACHE-XX` cache and the data types it caches (cross-reference ART-11).
- **Per-cache flow diagram** — for each cache with `≥ 3` access points, a `flowchart TD` showing write sites, read sites, invalidation sites, and eviction triggers. Edges labeled with the operation (`SET`, `GET`, `DEL`, `EXPIRE`) and the citation.
- **Performance-critical-path overlay** — `graph LR` showing the top critical paths with cache-miss branches highlighted in red and N+1 loops highlighted in orange.
- **Hotspot diagram** — `graph LR` showing every hotspot and its position on the critical path.

Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

### 6.12 Coverage Cross-Check

Cross-check the cache catalog against ART-13's `kind: cache` units:

1. Compute `C_13` = set of `S-XX` stateful units with `kind: cache` from ART-13.
2. Compute `C_18` = set of `CACHE-XX` caches from ART-18.
3. Expected: `C_18 ⊇ C_13` (every cache stateful unit is cataloged as a `CACHE-XX`). Caches in `C_13 \ C_18` are `COVERAGE_GAP` findings.
4. Caches in `C_18 \ C_13` are caches cataloged in ART-18 that ART-13 did not record as stateful units; these are typically HTTP caches and CDN caches (which ART-13 may not classify as stateful). These are flagged for review and may indicate that ART-13 should be amended.

---

## 7. Required Outputs

### ART-18 — Caching & Performance Report

**Type:** Doc.

**Acceptance Criteria:**

- AC-18.1: The artifact file exists at `<output_root>/artifacts/phase2/ART18_<engagement_id>_caching-perf.md`.
- AC-18.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-18.3: The body contains: Executive Summary, Methodology, Cache Catalog, Per-Cache Properties, Invalidation Strategies, Eviction Policies, N+1 Patterns, Batched Operations, Lazy/Eager Classification, Prefetching, Performance-Critical Paths, Performance Hotspots, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-18.4: Every cache records its key, value, TTL, invalidation strategy, and eviction policy.
- AC-18.5: Every Mermaid diagram is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-18.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-18.7: Every N+1 pattern and batched operation is flagged.
- AC-18.8: Performance-critical paths and hotspots are enumerated.
- AC-18.9: Coverage cross-check is recorded with no unresolved contradictions.

---

## 8. Output Templates

### 8.1 ART-18 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-18
artifact_type: Doc
producing_prompt: PROMPT_18
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
caches:
  - id: CACHE-01
    kind: in-memory | distributed | http | cdn | query | memoization
    library: <name>
    client_instantiation_citation: <file>:<line-range>
    external: false
    key_expression: <text>
    key_citation: <file>:<line-range>
    value_type: D-XX | <type>
    value_serialization: JSON | binary | protobuf | NA
    ttl_duration: <int> | NA
    ttl_unit: seconds | minutes | hours | days | NA
    ttl_citation: <file>:<line-range>
    default_ttl: <text> | NA
    max_size: <int> | NA
    size_unit: entries | bytes | kilobytes
    scope: process | distributed | request | session
    invalidation_strategy: ttl-based | write-through | write-around | write-back | explicit-invalidate | event-driven | tag-based | mixed
    invalidation_details: <text>
    invalidation_citation: <file>:<line-range>
    eviction_policy: LRU | LFU | FIFO | TTL-only | random | size-based
    eviction_citation: <file>:<line-range>
nplusone_patterns:
  - id: NQ-01
    loop_id: LP-XX
    query_fn_id: FN-XX
    query_call_citation: <file>:<line-range>
    loop_iterations_estimated: <int> | UNVERIFIED
    suggested_batch_alternative: <text>
    severity: MAJOR | MINOR
batched_operations:
  - id: BT-01
    kind: db-batch | queue-batch | parallel-batch | pipeline-batch
    function_id: FN-XX
    batch_size: <int> | <expression>
    batch_call_citation: <file>:<line-range>
lazy_eager_loads:
  - id: LD-01
    kind: lazy | eager
    loaded_entity: D-XX
    load_fn_id: FN-XX
    trigger: <text>
    citation: <file>:<line-range>
prefetches:
  - id: PF-01
    kind: link | route | query | background-refresh
    target: <text>
    trigger: user-hover | route-change | timer | idle
    function_id: FN-XX
    citation: <file>:<line-range>
performance_critical_paths:
  - id: PCP-01
    entry_id: EP-XX
    path_nodes: [FN-XX]
    betweenness_sum: <float>
    cache_miss_count: <int>
    nplusone_count: <int>
    score: <float>
    citations: [<file>:<line-range>]
performance_hotspots:
  - id: HS-01
    function_id: FN-XX
    critical_path_id: PCP-XX
    cost_score: <float>
    flagged_reasons: [<text>]
    citation: <file>:<line-range>
coverage_cross_check:
  caches_from_art13: [S-XX]
  caches_cataloged: [CACHE-XX]
  coverage_gaps: [S-XX]
  catalog_only: [CACHE-XX]
mermaid_sources:
  - diagram_id: D-01
    title: <text>
    sidecar_file: <relative-path>
    node_count: <int>
source_coverage:
  - path: <file_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <file_path>:<line-range>
    symbol: <name>
sections:
  - id: S-01
    title: <string>
    claims: [C-XX]
---
```

### 8.2 ART-18 Body Skeleton

```markdown
# ART-18: Caching & Performance Report

## 1. Executive Summary
## 2. Methodology
## 3. Cache Catalog
   ### 3.1 In-Memory Caches
   #### CACHE-01: <name>
   - Kind: <kind>
   - Key: <key_expression>
   - Value: <value_type>
   - TTL: <ttl>
   - Invalidation: <strategy>
   - Eviction: <policy>
   **Diagram D-01: CACHE-01 Flow**
   ### 3.2 Distributed Caches
   ### 3.3 HTTP Caches
   ### 3.4 CDN Caches
   ### 3.5 Query Caches
   ### 3.6 Memoization
## 4. N+1 Patterns
## 5. Batched Operations
## 6. Lazy/Eager Classification
## 7. Prefetching
## 8. Performance-Critical Paths
   **Diagram D-02: Top Critical Path**
## 9. Performance Hotspots
   **Diagram D-03: Hotspot Map**
## 10. Coverage Cross-Check
## 11. Traceability Index
## 12. Open Questions
## 13. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every `S-XX` cache from ART-13 is cataloged; threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of caches, N+1 patterns, batches, loads, prefetches, critical paths, and hotspots cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no cache in ART-18 contradicts ART-13's `kind: cache` units or ART-11's `significant_data_types`.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` TTL, `UNVERIFIED` eviction policy, and `UNVERIFIED` loop iterations has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.1 on a 5% sample of files yields the same cache set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-18.A. Cache-Property Completeness** — every cache has `key_expression`, `value_type`, `ttl_duration` (or `NA`), `invalidation_strategy`, and `eviction_policy` populated.
- **Q-18.B. N+1 Detection Coverage** — every loop in ART-12 with `body_calls` containing a persistence API call is checked for N+1; matches are recorded.
- **Q-18.C. Invalidation-Strategy Evidence** — every cache's `invalidation_strategy` is backed by an `invalidation_citation` that evidences the strategy (TTL config, explicit-invalidate call, event handler, etc.).
- **Q-18.D. Critical-Path Hotspot Link** — every `HS-XX` hotspot references a `PCP-XX` critical path.
- **Q-18.E. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment.
- **Q-18.F. Sidecar Files** — every Mermaid block has a `.mmd` sidecar file.
- **Q-18.G. Lazy/Eager Coherence** — every load classified as `lazy` has a `trigger` that is an access pattern (getter, relation access, `useEffect`); every `eager` load has a `trigger` that is a construction or initialization site.

---

## 10. Common Pitfalls

- Do not infer cache properties from library defaults alone; verify the actual configuration per R22.
- Always record the cache key expression; an unspecified key makes the cache opaque to downstream analysis.
- Do not conflate write-through with write-back; the distinction determines durability guarantees.
- Always cite the invalidation strategy; an unspecified strategy leaves the cache-coherence model underspecified.
- Do not infer N+1 patterns from the presence of a loop and a query; verify that the loop variable is the query key.
- Always record the suggested batch alternative for N+1 patterns; the suggestion is actionable for the End Consumer.
- Do not conflate lazy loading with deferred execution (e.g., generators); lazy loading loads from a backing store, while deferred execution computes locally.
- Always cross-check caches against ART-13's `kind: cache` units; a missing cache is a `COVERAGE_GAP`.
- Do not infer prefetching from caching; prefetching loads before access, while caching stores after access.
- Always cap performance-critical path scoring at top-N to avoid combinatorial explosion.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders diagrams from the sidecar source.

---

## 11. Handoff Criteria

PROMPT_24 and PROMPT_26 consume ART-18. Handoff requires ALL of:

- HC-18.1: ART-18 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-18.2: Every cache records its key, value, TTL, invalidation strategy, and eviction policy.
- HC-18.3: N+1 patterns, batched operations, lazy/eager loads, and prefetches are cataloged.
- HC-18.4: Performance-critical paths and hotspots are enumerated.
- HC-18.5: Mermaid diagrams are emitted with `.mmd` sidecar files.
- HC-18.6: Coverage cross-check is recorded with no unresolved contradictions.
- HC-18.7: `repository_fingerprint_recheck` matches ART-01.
- HC-18.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_24 (Engineering Decisions & Trade-offs — uses caching and performance strategy as evidence of trade-offs), PROMPT_26 (Rebuild Guide — uses caching catalog as required content for the operational runbook).
- **Depends on:** ART-01 (PROMPT_01), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-11 (PROMPT_11), ART-13 (PROMPT_13).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 7.
- **Forward reference:** PROMPT_30 verifies that every cache referenced by ART-24 or ART-26 resolves to an entry in ART-18 and that every `MAJOR` N+1 pattern is recorded as a finding in the QA report.

*End of PROMPT_18. Orchestrator may dispatch PROMPT_19 (if auth code is detected) or PROMPT_20 (if persistence code is detected) upon satisfaction of § 11.*
