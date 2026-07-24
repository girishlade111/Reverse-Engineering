========================================================================
SUPPLEMENTARY PROMPT S1: MEMORY ANALYSIS
========================================================================
Supplementary Analysis: Memory, Persistence, and Data Storage Deep Dive
Enterprise Reverse Engineering Prompt Framework

========================================================================
PURPOSE
========================================================================

This supplementary prompt provides deeper analysis of memory
management, data persistence, and storage patterns beyond what
is covered in the standard phases. Use when the system has
complex data management requirements.

========================================================================
WHEN TO USE
========================================================================

Execute this prompt if the system:
- Manages significant in-memory state
- Uses multiple storage backends
- Implements caching with complex invalidation
- Has custom memory management
- Uses database query optimization patterns
- Implements data versioning or migration

Execute after Phase 4 and before Phase 9.

========================================================================
ACTIVITIES
========================================================================

S1.1. MEMORY PROFILE ANALYSIS
- Document memory allocation patterns
- Identify object lifecycles and garbage collection behavior
- Document memory pools, object reuse patterns
- Identify memory leak risks

S1.2. PERSISTENCE LAYER DEEP DIVE
- Document ORM/ODM layer and its abstractions
- Document query patterns (lazy vs eager loading)
- Document transaction management
- Document connection pooling
- Document migration strategy

S1.3. DATA LIFECYCLE MANAGEMENT
- Document data creation, storage, archival, deletion lifecycle
- Document retention policies
- Document backup and recovery procedures
- Document data purging/cleanup processes

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT S1.1: MEMORY_ANALYSIS.md
ARTIFACT S1.2: PERSISTENCE_ANALYSIS.md
ARTIFACT S1.3: DATA_LIFECYCLE.md

========================================================================
END OF PROMPT S1
========================================================================
