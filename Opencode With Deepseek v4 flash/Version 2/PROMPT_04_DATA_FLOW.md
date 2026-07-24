========================================================================
PROMPT 04: DATA FLOW ANALYSIS
========================================================================
Phase 4: Data Flow, State Management, and Execution Pipeline Analysis
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. Complete understanding of all data flows through the system
2. State management patterns and strategies documented
3. Execution pipeline diagrams for all major workflows
4. Event system architecture and event catalog
5. Data transformation and mapping documentation
6. Caching strategy and cache invalidation patterns
7. Streaming and batch processing pipelines
8. Error propagation through data flows

========================================================================
INPUTS
========================================================================

- MODULE_CATALOG.md (from Phase 3)
- FILE_RESPONSIBILITY_MATRIX.md (from Phase 3)
- CALL_GRAPHS.md (from Phase 3)
- SYSTEM_BOUNDARIES.md (from Phase 2)
- COMPONENT_INTERACTIONS.md (from Phase 2)
- All repository files

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 4.1: DATA FLOW MAPPING

4.1.1. Identify all data sources:
    - User input (HTTP requests, CLI args, forms, etc.)
    - Database queries
    - File reads
    - External API responses
    - Message queue messages
    - WebSocket messages
    - System events
    - Sensor/hardware input
    - Internal timers/schedules

4.1.2. For each data source, document:
    - Source type and origin
    - Data format and schema
    - Frequency/volume
    - Trigger mechanism
    - Authentication/authorization applied
    - Validation and sanitization

4.1.3. Trace each data source through the system:
    - Entry point for the data
    - Validation/transformation steps
    - Business logic processing
    - Storage/persistence
    - Output/response generation
    - Error handling at each step

4.1.4. Generate Mermaid data flow diagrams for each major flow.

ACTIVITY 4.2: DATA TRANSFORMATION PIPELINE

4.2.1. Identify all data transformations:
    - Parsing (JSON, XML, CSV, binary, etc.)
    - Serialization/Deserialization
    - Validation and normalization
    - Enrichment (adding derived data)
    - Filtering and aggregation
    - Mapping and conversion
    - Encryption/Decryption
    - Encoding/Decoding
    - Compression/Decompression

4.2.2. For each transformation, document:
    - Input format/schema
    - Output format/schema
    - Transformation logic (with code citations)
    - Location in the codebase
    - Performance characteristics
    - Error conditions

4.2.3. Document the complete transformation chain for core flows:
    Input -> Step1 -> Step2 -> ... -> Output

ACTIVITY 4.3: STATE MANAGEMENT ANALYSIS

4.3.1. Identify all types of state:
    - In-memory state (variables, caches, singletons)
    - Persistent state (databases, files)
    - Session state (user sessions)
    - Application state (global configuration)
    - Distributed state (shared caches, distributed stores)

4.3.2. For each state type, document:
    - What data is stored
    - Where it is stored (variable, file, DB table, cache key)
    - Who writes to it
    - Who reads from it
    - Lifecycle (when created, when destroyed)
    - Consistency guarantees
    - Concurrency control (locks, transactions, atomic ops)
    - Recovery mechanism (if state is lost)

4.3.3. State machine analysis:
    - Identify any state machines in the system
    - Document states, transitions, events, and actions
    - Generate Mermaid state diagrams

ACTIVITY 4.4: EVENT SYSTEM ANALYSIS

4.4.1. Identify the event infrastructure:
    - Event bus or message broker
    - Event types and their schemas
    - Publishers and subscribers
    - Event routing mechanism
    - Delivery guarantees (at-most-once, at-least-once, exactly-once)
    - Event persistence and replay

4.4.2. Create an event catalog:
    - Event name
    - Trigger condition
    - Producer component
    - Consumer components
    - Payload schema
    - Synchronous or asynchronous
    - Error handling

4.4.3. Generate Mermaid event flow diagrams.

ACTIVITY 4.5: EXECUTION PIPELINE ANALYSIS

4.5.1. Identify all execution pipelines:
    - Request processing pipelines
    - Data processing pipelines
    - CI/CD pipelines
    - Deployment pipelines
    - Test execution pipelines
    - Build pipelines

4.5.2. For each pipeline, document:
    - Pipeline stages in order
    - What each stage does
    - Stage inputs and outputs
    - Stage ordering and dependencies
    - Conditional branching within the pipeline
    - Error handling and retry at each stage
    - Parallelism and concurrency
    - Resource requirements

4.5.3. Generate Mermaid flowchart for each pipeline.

ACTIVITY 4.6: CACHING ANALYSIS

4.6.1. Identify all caching mechanisms:
    - In-memory caches
    - Distributed caches (Redis, Memcached)
    - HTTP caching (ETags, Cache-Control)
    - CDN caching
    - Database query caching
    - File system caching
    - Browser caching

4.6.2. For each cache, document:
    - Cache key structure
    - Cached data format
    - Cache expiration/TTL policy
    - Cache invalidation strategy
    - Cache hit ratio indicators
    - Cache consistency model
    - Warmup/preloading strategy

ACTIVITY 4.7: DATA VALIDATION AND SANITIZATION

4.7.1. Identify all validation layers:
    - Input validation (at API boundaries)
    - Schema validation
    - Business rule validation
    - Sanitization (XSS, SQL injection, etc.)

4.7.2. For each validation, document:
    - Location in code
    - What is validated
    - Validation rules
    - Error handling on validation failure
    - Bypass mechanisms (if any)

ACTIVITY 4.8: STREAMING AND BATCH PROCESSING

4.8.1. If applicable, analyze:
    - Stream processing (continuous data flow)
    - Batch processing (scheduled/triggered bulk processing)
    - Hybrid approaches

4.8.2. For each, document:
    - Stream/batch source
    - Processing logic
    - Windowing/batching strategy
    - Checkpoint/savepoint mechanism
    - Failure recovery
    - Backpressure handling

========================================================================
ANALYSIS METHODOLOGY
========================================================================

This phase requires TracingScan methodology:

For each major data flow:
1. START at the data source/entry point.
2. FOLLOW the data through each transformation.
3. DOCUMENT each step with file:line evidence.
4. NOTE branching points and alternative paths.
5. TRACE error paths as well as happy paths.
6. VERIFY the flow traces by reading downstream code.

Use the call graphs from Phase 3 as a starting roadmap.

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT 4.1: DATA_FLOW_DIAGRAMS.md
- Mermaid data flow diagrams for each major flow
- Narrative descriptions for each diagram
- Data source inventory

ARTIFACT 4.2: TRANSFORMATION_PIPELINES.md
- Complete transformation chains
- Input/output schemas per transformation
- Performance notes

ARTIFACT 4.3: STATE_MANAGEMENT.md
- State inventory
- State lifecycle documentation
- Concurrency control documentation
- Mermaid state machine diagrams

ARTIFACT 4.4: EVENT_CATALOG.md
- Complete event catalog with schemas
- Publisher/Subscriber mapping
- Mermaid event flow diagrams

ARTIFACT 4.5: EXECUTION_PIPELINES.md
- Pipeline documentation
- Mermaid pipeline flowcharts
- Stage dependency documentation

ARTIFACT 4.6: CACHING_STRATEGY.md
- Cache inventory
- Cache key/invalidation documentation
- TTL and consistency documentation

ARTIFACT 4.7: VALIDATION_MATRIX.md
- Validation layer inventory
- Validation rules documentation
- Error handling on validation failure

ARTIFACT 4.8: STREAMING_BATCH.md (if applicable)
- Stream/batch processing documentation
- Checkpoint and recovery documentation

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] Every data source is identified and traced.
[ ] Every transformation is documented with input/output schemas.
[ ] All state types are identified and documented.
[ ] Event catalog is complete with publisher/subscriber mapping.
[ ] Execution pipelines have complete flowcharts.
[ ] Caching strategy is fully documented.
[ ] Validation layers are mapped and documented.
[ ] All diagrams accurately trace actual code paths.
[ ] Error paths are documented alongside happy paths.
[ ] Artifacts meet quality standards (score >= 4.0).

========================================================================
OUTPUTS TO NEXT PHASE
========================================================================

Pass to Phase 5:
- DATA_FLOW_DIAGRAMS.md
- TRANSFORMATION_PIPELINES.md
- STATE_MANAGEMENT.md
- EVENT_CATALOG.md
- EXECUTION_PIPELINES.md
- CACHING_STRATEGY.md
- VALIDATION_MATRIX.md
- STREAMING_BATCH.md (if applicable)

Pass to Phase 7:
- DATA_FLOW_DIAGRAMS.md
- EVENT_CATALOG.md

========================================================================
END OF PROMPT 04
========================================================================
