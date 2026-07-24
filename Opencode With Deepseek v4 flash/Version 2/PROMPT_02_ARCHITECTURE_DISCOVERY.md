========================================================================
PROMPT 02: ARCHITECTURE DISCOVERY
========================================================================
Phase 2: Architecture Discovery and System Design Extraction
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. A complete understanding of the system's architectural style
2. A component diagram showing all major system components
3. An understanding of component responsibilities and interactions
4. A layered architecture decomposition
5. Identification of architectural patterns in use
6. A dependency graph between components
7. An understanding of the system's boundaries and interfaces

========================================================================
INPUTS
========================================================================

- FILE_INVENTORY.md (from Phase 1)
- DIRECTORY_MAP.md (from Phase 1)
- TECH_STACK_SURVEY.md (from Phase 1)
- ENTRY_POINTS.md (from Phase 1)
- All files in the repository (for selective reading)

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 2.1: ARCHITECTURAL STYLE IDENTIFICATION

2.1.1. Determine the architectural style:
    - Monolithic vs. Microservices
    - Layered (n-tier)
    - Hexagonal (Ports and Adapters)
    - Clean Architecture
    - Event-Driven Architecture
    - CQRS/Event Sourcing
    - Pipeline Architecture
    - Plugin Architecture
    - Serverless
    - Peer-to-Peer
    - Client-Server
    - MVC/MVVM
    - Any hybrid or custom style

2.1.2. Cite specific evidence for your determination:
    - Directory structure patterns
    - Import/export patterns
    - Configuration patterns
    - Framework usage
    - Communication patterns

2.1.3. Document any architectural inconsistencies or anti-patterns.

ACTIVITY 2.2: COMPONENT IDENTIFICATION

2.2.1. Identify all major components of the system.

For each component, document:
- Name and purpose
- Location in the repository (directory path)
- Programming language and framework
- Entry points (APIs, handlers, listeners)
- Dependencies on other components
- Dependencies from other components
- Configuration requirements
- State owned (if any)
- Data persisted (if any)

2.2.2. Categorize components:
- Core/Critical: Essential to system function
- Supporting: Enhance core functionality
- Infrastructure: Cross-cutting concerns
- Integration: External system connections
- UI/Presentation: User-facing components
- API/Interface: System boundary components

ACTIVITY 2.3: LAYERED ARCHITECTURE ANALYSIS

2.3.1. Identify the layers present:
    - Presentation/UI layer
    - API/Controller layer
    - Service/Business Logic layer
    - Domain/Model layer
    - Data Access/Persistence layer
    - Infrastructure/Cross-cutting layer

2.3.2. For each layer, document:
    - Files and directories that comprise it
    - Responsibilities
    - Interfaces to adjacent layers
    - Dependencies on lower layers
    - Services provided to upper layers

2.3.3. Verify layer boundaries:
    - Are dependencies only inward?
    - Are there any circular dependencies?
    - Are there any layer violations?

ACTIVITY 2.4: COMPONENT INTERACTION ANALYSIS

2.4.1. Map how components interact:

For each interaction, document:
- Source component
- Target component
- Interaction type (sync/async)
- Communication mechanism (HTTP, message queue, event bus, etc.)
- Data format (JSON, Protobuf, XML, etc.)
- Protocol (REST, gRPC, WebSocket, etc.)
- Frequency/volume (if determinable)
- Error handling for this interaction

2.4.2. Identify interaction patterns:
- Request/Response
- Publish/Subscribe
- Command/Query
- Event/Notification
- Stream
- Batch

ACTIVITY 2.5: ARCHITECTURAL PATTERN INVENTORY

2.5.1. Identify all architectural patterns in use:
    - Dependency Injection
    - Inversion of Control
    - Factory
    - Singleton
    - Observer
    - Strategy
    - Decorator
    - Adapter
    - Proxy
    - Facade
    - Command
    - Chain of Responsibility
    - State
    - Template Method
    - Any domain-specific patterns

2.5.2. For each pattern found:
    - Cite the specific implementation location
    - Describe how it's used
    - Note any unusual adaptations

ACTIVITY 2.6: DEPENDENCY GRAPH CONSTRUCTION

2.6.1. For each major component/module, identify:
    - Direct dependencies (explicit imports/requires)
    - Indirect/transitive dependencies
    - External dependencies (third-party packages)
    - Internal dependencies (other components)

2.6.2. Build a dependency graph showing:
    - Components as nodes
    - Dependencies as directed edges
    - Dependency type labels
    - Any circular dependency clusters

2.6.3. Generate a Mermaid component diagram.

ACTIVITY 2.7: BOUNDARY AND INTERFACE ANALYSIS

2.7.1. Identify all system boundaries:
    - Public API surfaces
    - Internal module interfaces
    - External service integrations
    - File system interfaces
    - Network interfaces
    - Database interfaces

2.7.2. For each boundary, document:
    - What crosses the boundary
    - In what format
    - Under what protocol
    - What authentication/authorization exists
    - What validation occurs

ACTIVITY 2.8: INITIALIZATION AND LIFECYCLE

2.8.1. Document the system initialization sequence:
    - Bootstrap/startup process
    - Configuration loading order
    - Dependency initialization order
    - Component registration
    - Connection establishment
    - Health check registration

2.8.2. Document the shutdown sequence (if present).

2.8.3. Document any heartbeat/keepalive mechanisms.

========================================================================
ANALYSIS METHODOLOGY
========================================================================

This phase requires MediumScan depth:
- Read all files that define architecture (main entry points,
  module definitions, configuration, router definitions)
- Read representative files from each component
- Read interface/contract files completely
- Read initialization/bootstrap files completely
- Do NOT read every implementation file yet

Use evidence from imports, exports, and module definitions
rather than full implementation reading.

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT 2.1: ARCHITECTURAL_STYLE.md
- Architectural style determination with evidence
- Architecture rationale (why this style was chosen)
- Architecture constraints and trade-offs

ARTIFACT 2.2: COMPONENT_INVENTORY.md
- Complete component list
- Component categorization
- Component responsibility documentation
- Component location mapping

ARTIFACT 2.3: LAYER_ARCHITECTURE.md
- Layer identification and definition
- Layer responsibility documentation
- Layer dependency verification
- Layer violation report (if any)

ARTIFACT 2.4: COMPONENT_INTERACTIONS.md
- Interaction catalog with details
- Interaction pattern identification
- Communication protocol documentation

ARTIFACT 2.5: ARCHITECTURE_DIAGRAM.md
- Mermaid component diagram
- Mermaid layer diagram
- Mermaid deployment diagram (if applicable)
- Diagram legends and explanations

ARTIFACT 2.6: DEPENDENCY_GRAPH.md
- Component dependency graph
- Dependency type analysis
- Circular dependency report (if any)
- External dependency mapping

ARTIFACT 2.7: PATTERN_INVENTORY.md
- Architectural patterns used with citations
- Design patterns used with citations
- Custom/domain-specific patterns

ARTIFACT 2.8: SYSTEM_BOUNDARIES.md
- Boundary inventory
- Interface contract documentation
- Cross-boundary data format documentation

ARTIFACT 2.9: INITIALIZATION_LIFECYCLE.md
- Startup sequence
- Shutdown sequence
- Lifecycle hooks and events

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] Architectural style is identified with clear evidence.
[ ] Every component is documented.
[ ] Component interactions are mapped.
[ ] Architecture diagram is generated and accurate.
[ ] Dependency graph is complete.
[ ] All architectural patterns are identified.
[ ] System boundaries are documented.
[ ] Layer architecture is analyzed.
[ ] Initialization sequence is documented.
[ ] All findings cite specific file evidence.
[ ] Artifacts meet quality standards (score >= 4.0).

========================================================================
OUTPUTS TO NEXT PHASE
========================================================================

Pass to Phase 3:
- ARCHITECTURAL_STYLE.md
- COMPONENT_INVENTORY.md
- LAYER_ARCHITECTURE.md
- COMPONENT_INTERACTIONS.md
- ARCHITECTURE_DIAGRAM.md
- DEPENDENCY_GRAPH.md
- PATTERN_INVENTORY.md
- SYSTEM_BOUNDARIES.md
- INITIALIZATION_LIFECYCLE.md

========================================================================
END OF PROMPT 02
========================================================================
