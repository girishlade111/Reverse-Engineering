========================================================================
SUPPLEMENTARY PROMPT S4: PERFORMANCE ANALYSIS
========================================================================
Supplementary Analysis: Performance Characteristics Deep Dive
Enterprise Reverse Engineering Prompt Framework

========================================================================
PURPOSE
========================================================================

This supplementary prompt analyzes the performance characteristics
of the system from the source code, identifying bottlenecks,
optimization patterns, and resource usage profiles.

========================================================================
WHEN TO USE
========================================================================

Execute this prompt if:
- Performance is critical to the system
- The system has complex algorithms
- The system processes large data volumes
- Real-time/low-latency requirements exist
- Concurrency/parallelism patterns are significant

Execute after Phase 8 and before Phase 9.

========================================================================
ACTIVITIES
========================================================================

S4.1. ALGORITHMIC COMPLEXITY ANALYSIS
- Document Big-O complexity of key algorithms
- Identify potential performance bottlenecks
- Document any known performance limitations
- Identify N+1 query patterns in data access

S4.2. CONCURRENCY ANALYSIS
- Document thread/process models
- Document lock contention areas
- Document async/await patterns and thread pool usage
- Document deadlock/livelock prevention

S4.3. RESOURCE PROFILE ANALYSIS
- Document memory usage patterns
- Document CPU usage patterns
- Document I/O patterns (disk, network)
- Document connection pool usage

S4.4. CACHING EFFECTIVENESS
- Document cache hit/miss characteristics
- Document cache invalidation cost
- Document cache size vs. performance trade-offs
- Document cache warming strategies

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT S4.1: COMPLEXITY_ANALYSIS.md
ARTIFACT S4.2: CONCURRENCY_ANALYSIS.md
ARTIFACT S4.3: RESOURCE_PROFILE.md
ARTIFACT S4.4: CACHE_EFFECTIVENESS.md

========================================================================
END OF PROMPT S4
========================================================================
