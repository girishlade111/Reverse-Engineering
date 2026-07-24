========================================================================
SUPPLEMENTARY PROMPT S12: BACKGROUND JOBS
========================================================================
Supplementary Analysis: Background Workers and Async Job Processing
Enterprise Reverse Engineering Prompt Framework

========================================================================
PURPOSE
========================================================================

This supplementary prompt analyzes background job processing,
worker pools, task queues, and asynchronous processing pipelines.

========================================================================
WHEN TO USE
========================================================================

Execute this prompt if:
- Background job libraries are used (Bull, Sidekiq, Celery, etc.)
- Worker processes exist
- Task/Job queues are configured
- Async processing is done outside request/response cycle
- Offline/background processing is significant

Execute after Phase 4.

========================================================================
ACTIVITIES
========================================================================

S12.1. JOB QUEUE INFRASTRUCTURE
- Document job queue library and configuration
- Document queue/connection configuration
- Document job serialization format
- Document job routing and queue topology
- Document job persistence and durability

S12.2. JOB TYPE CATALOG
For each job type:
- Job name and purpose
- Trigger/queue location
- Job data/payload schema
- Processing logic location
- Error handling and retry strategy
- Success/failure callbacks
- Estimated execution time
- Resource requirements

S12.3. WORKER INFRASTRUCTURE
- Document worker process model
- Document concurrency settings
- Document worker scaling strategy
- Document worker lifecycle management
- Document worker monitoring

S12.4. JOB LIFECYCLE AND RELIABILITY
- Document job creation flow
- Document job scheduling/delaying
- Document job prioritization
- Document job timeout handling
- Document dead letter handling
- Document job cleanup/purging

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT S12.1: JOB_QUEUE_INFRASTRUCTURE.md
ARTIFACT S12.2: JOB_TYPE_CATALOG.md
ARTIFACT S12.3: WORKER_INFRASTRUCTURE.md
ARTIFACT S12.4: JOB_LIFECYCLE.md

========================================================================
END OF PROMPT S12
========================================================================
