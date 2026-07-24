========================================================================
PROMPT 07: INTEGRATION AND DEPENDENCY ANALYSIS
========================================================================
Phase 7: Integration, Dependency, and Boundary Analysis
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. Complete map of all external integrations and their contracts
2. API endpoint documentation (REST, GraphQL, gRPC, etc.)
3. Database schema and query documentation
4. Third-party service integration documentation
5. Authentication and authorization architecture
6. File system and storage integration documentation
7. Network communication patterns
8. Protocol and contract specifications

========================================================================
INPUTS
========================================================================

- DATA_FLOW_DIAGRAMS.md (from Phase 4)
- EVENT_CATALOG.md (from Phase 4)
- AI_ARCHITECTURE_OVERVIEW.md (from Phase 6)
- TOOL_CATALOG.md (from Phase 6)
- SYSTEM_BOUNDARIES.md (from Phase 2)
- MODULE_CATALOG.md (from Phase 3)
- All repository files

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 7.1: API ENDPOINT AUDIT

7.1.1. Find all API definitions:
    - REST endpoints (routes, controllers, handlers)
    - GraphQL schemas, resolvers, mutations, queries
    - gRPC service definitions and methods
    - WebSocket event handlers
    - RPC endpoints
    - CLI command definitions
    - Internal API boundaries between modules

7.1.2. For every API endpoint, document:
    - HTTP method (GET, POST, PUT, DELETE, PATCH, etc.)
    - URL path or route pattern
    - Request parameters (path, query, headers, body)
    - Request body schema
    - Response body schema (success + error)
    - HTTP status codes returned
    - Authentication required
    - Authorization required (roles, permissions)
    - Rate limiting
    - Input validation rules
    - Implementation location
    - Error responses and their formats
    - Versioning strategy

7.1.3. For GraphQL, additionally document:
    - Schema types and relationships
    - Query fields and arguments
    - Mutation fields and input types
    - Subscription events
    - Resolver implementation locations
    - DataLoader batch patterns
    - N+1 query prevention

7.1.4. For gRPC, additionally document:
    - Proto file locations
    - Service definitions
    - RPC method signatures
    - Message types
    - Streaming types (unary, server stream, client stream, bidirectional)

7.1.5. Generate Mermaid API route maps.

7.1.6. API VERSIONING ANALYSIS
7.1.6.1. Document API versioning strategy:
    - URL-based versioning (/v1/, /v2/)
    - Header-based versioning (Accept: application/vnd.api.v2+json)
    - Query parameter versioning
    - Contract-based versioning (Protobuf)

7.1.6.2. Document version lifecycle:
    - Active versions and their status
    - Deprecation policy and timelines
    - Sunset/decommission process
    - Migration guides between versions
    - Backward compatibility guarantees

7.1.6.3. Document version negotiation:
    - Default version behavior
    - Version resolution priority
    - Unversioned request handling
    - Error responses for invalid/missing versions

7.1.6.4. Document versioning in practice:
    - When new versions are created vs. backward-compatible changes
    - Breaking change policy
    - Internal API versioning vs. public API versioning
    - Client version tracking (if implemented)

ACTIVITY 7.2: DATABASE INTEGRATION ANALYSIS

7.2.1. Identify all database systems used:
    - Relational databases (PostgreSQL, MySQL, SQLite, etc.)
    - NoSQL databases (MongoDB, Cassandra, etc.)
    - In-memory databases (Redis, etc.)
    - Time-series databases
    - Graph databases
    - Vector databases

7.2.2. For each database, document:
    - Database type and version
    - Connection configuration
    - Connection pooling strategy
    - ORM/ODM used and version
    - Migration strategy and tools
    - Backup strategy (if documented)
    - Replication/sharding strategy

7.2.3. Document the database schema:
    - All tables/collections with columns/fields
    - Data types and constraints
    - Indexes (defined or implicit)
    - Relationships and foreign keys
    - Views and materialized views
    - Stored procedures and functions
    - Triggers

7.2.4. Analyze query patterns:
    - Read queries (frequency, complexity)
    - Write queries (frequency, complexity)
    - Transactions and isolation levels
    - N+1 query patterns
    - Query performance indicators

7.2.5. For schema/migration files, document:
    - Migration file locations
    - Migration framework
    - Schema evolution history
    - Seed data scripts

ACTIVITY 7.3: THIRD-PARTY SERVICE INTEGRATIONS

7.3.1. Identify all third-party service integrations:
    - External APIs called
    - SaaS platform integrations
    - Cloud service integrations
    - Payment processors
    - Email/SMS services
    - File storage services
    - Authentication providers
    - Monitoring/observability services
    - AI/ML model APIs
    - CDN services

7.3.2. For each third-party integration, document:
    - Service name and provider
    - Integration type (API, SDK, webhook, etc.)
    - API version used
    - Authentication method (API key, OAuth, etc.)
    - Client library/SDK used
    - Wrapper/abstraction layer (if any)
    - Configuration (endpoints, keys, timeouts, etc.)
    - Rate limiting and throttling
    - Error handling and retry logic
    - Circuit breaker implementation (if any)
    - Caching of responses
    - Cost implications (per-call pricing)
    - Fallback behavior on service failure

7.3.3. For webhook integrations:
    - Direction (incoming, outgoing, both)
    - Event types received/sent
    - Payload schema
    - Verification/authentication mechanism
    - Retry and delivery guarantees
    - Idempotency handling

ACTIVITY 7.4: AUTHENTICATION AND AUTHORIZATION

7.4.1. Document the authentication architecture:
    - Authentication methods (JWT, OAuth, SAML, API keys, etc.)
    - Identity provider (Auth0, Firebase, custom, etc.)
    - User registration and management
    - Password policies
    - Session management
    - MFA/2FA implementation
    - SSO implementation
    - Token lifecycle (issuance, refresh, revocation)

7.4.2. Document the authorization architecture:
    - Authorization model (RBAC, ABAC, ReBAC, etc.)
    - Role/permission definitions
    - Permission checking locations
    - Policy enforcement points
    - Scope/tenant isolation
    - Audit logging of authorization decisions

7.4.3. For each authentication/authorization component:
    - Implementation location
    - Configuration
    - Testing approach
    - Known limitations

ACTIVITY 7.5: FILE AND STORAGE INTEGRATIONS

7.5.1. Identify all file/storage systems:
    - Local file system usage
    - Cloud storage (S3, GCS, Azure Blob, etc.)
    - CDN integration
    - Static asset management
    - Temporary file management
    - File upload/download handling

7.5.2. For each storage integration:
    - Storage provider and configuration
    - Data stored (what types of files)
    - Access patterns (read-heavy, write-heavy)
    - Retention policies
    - Backup and redundancy
    - Access control
    - Cost structure

ACTIVITY 7.6: MESSAGE QUEUE AND STREAM INTEGRATION

7.6.1. Identify messaging/streaming systems:
    - Message queues (RabbitMQ, SQS, etc.)
    - Stream processing (Kafka, Kinesis, etc.)
    - Event buses
    - Pub/Sub systems

7.6.2. For each messaging system:
    - Queue/topic names and purposes
    - Producer components
    - Consumer components
    - Message format/schema
    - Delivery semantics
    - Dead letter queue handling
    - Consumer group/partition strategy

ACTIVITY 7.7: EXTERNAL DEPENDENCY MAP

7.7.1. Create a complete external dependency map showing:
    - All external systems the repository depends on
    - The nature of each dependency (hard, soft, optional)
    - The impact if the dependency fails
    - The failover/redundancy mechanism (if any)
    - The contract/interface between the system and the dependency

========================================================================
ANALYSIS METHODOLOGY
========================================================================

This phase requires ContractScan methodology:

Focus on the contracts between the system and external entities:
- API specifications (OpenAPI, GraphQL schema, proto files)
- Database schemas
- Interface definitions
- Integration wrappers and adapters
- Configuration for external services

Read integration tests for deeper understanding of
how integrations are expected to behave.

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT 7.1: API_CATALOG.md
- Complete API endpoint documentation
- Request/response schemas
- Authentication requirements per endpoint
- Mermaid API route map

ARTIFACT 7.2: DATABASE_SCHEMA.md
- Database schema documentation
- Entity-relationship diagrams (Mermaid)
- Query pattern analysis
- Migration history

ARTIFACT 7.3: THIRD_PARTY_INTEGRATIONS.md
- Third-party service inventory
- Integration details per service
- Fallback and error handling
- Webhook documentation

ARTIFACT 7.4: AUTH_ARCHITECTURE.md
- Authentication flow documentation
- Authorization model documentation
- Mermaid auth flow diagrams
- Permission matrix

ARTIFACT 7.5: STORAGE_INTEGRATIONS.md
- Storage system inventory
- Configuration and access patterns
- Data lifecycle documentation

ARTIFACT 7.6: MESSAGING_INTEGRATIONS.md
- Message queue/stream inventory
- Producer/Consumer mapping
- Message format documentation

ARTIFACT 7.7: EXTERNAL_DEPENDENCY_MAP.md
- Complete external dependency map
- Dependency criticality assessment
- Failover analysis

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] Every API endpoint is documented with full schema.
[ ] Database schema is completely documented.
[ ] All third-party integrations are identified and documented.
[ ] Authentication/authorization architecture is documented.
[ ] All storage integrations are documented.
[ ] All messaging integrations are documented.
[ ] External dependency map is complete.
[ ] Error handling per integration is documented.
[ ] All findings cite specific file:line evidence.
[ ] Artifacts meet quality standards (score >= 4.0).

========================================================================
OUTPUTS TO NEXT PHASE
========================================================================

Pass to Phase 8:
- API_CATALOG.md
- THIRD_PARTY_INTEGRATIONS.md
- AUTH_ARCHITECTURE.md

Pass to Phase 9:
- All artifacts from this phase

========================================================================
END OF PROMPT 07
========================================================================
