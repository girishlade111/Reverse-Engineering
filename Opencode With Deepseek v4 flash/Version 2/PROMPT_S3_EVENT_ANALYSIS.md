========================================================================
SUPPLEMENTARY PROMPT S3: EVENT-DRIVEN ANALYSIS
========================================================================
Supplementary Analysis: Event-Driven Architecture Deep Dive
Enterprise Reverse Engineering Prompt Framework

========================================================================
PURPOSE
========================================================================

This supplementary prompt provides deep analysis of event-driven
patterns, message passing, pub/sub systems, and asynchronous
communication beyond what is covered in the standard phases.

========================================================================
WHEN TO USE
========================================================================

Execute this prompt if the system:
- Uses message queues or event buses
- Implements pub/sub patterns
- Has complex async communication patterns
- Uses event sourcing or CQRS
- Has distributed event-driven workflows

Execute after Phase 4 and before Phase 7.

========================================================================
ACTIVITIES
========================================================================

S3.1. EVENT INFRASTRUCTURE ANALYSIS
- Document broker/transport layer
- Document serialization format
- Document delivery guarantees
- Document ordering guarantees
- Document error handling (DLQ, retry)

S3.2. EVENT SCHEMA ANALYSIS
- Document every event type with full schema
- Document event versioning strategy
- Document schema registry (if any)
- Document backward/forward compatibility

S3.3. EVENT FLOW TRACING
- Trace complete event chains
- Document causal event relationships
- Document fan-out and aggregation patterns
- Document timing and latency expectations

S3.4. ASYNC PATTERN ANALYSIS
- Document fire-and-forget patterns
- Document request-response over events
- Document saga/choreography patterns
- Document timeout and compensation patterns

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT S3.1: EVENT_INFRASTRUCTURE.md
ARTIFACT S3.2: EVENT_SCHEMAS.md
ARTIFACT S3.3: EVENT_CHAINS.md
ARTIFACT S3.4: ASYNC_PATTERNS.md

========================================================================
END OF PROMPT S3
========================================================================
