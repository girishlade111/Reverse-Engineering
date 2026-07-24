========================================================================
PROMPT 03: MODULE ANALYSIS
========================================================================
Phase 3: Module and Component Deep-Dive Analysis
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. A complete understanding of every module in the system
2. A responsibility matrix mapping modules to features
3. Detailed knowledge of key classes, functions, and interfaces
4. An understanding of module-internal organization patterns
5. Complete call graphs for core modules
6. Identification of all public APIs and internal APIs
7. Understanding of module configuration and lifecycle

========================================================================
INPUTS
========================================================================

- COMPONENT_INVENTORY.md (from Phase 2)
- LAYER_ARCHITECTURE.md (from Phase 2)
- ARCHITECTURE_DIAGRAM.md (from Phase 2)
- DEPENDENCY_GRAPH.md (from Phase 2)
- SYSTEM_BOUNDARIES.md (from Phase 2)
- PATTERN_INVENTORY.md (from Phase 2)
- All files in the repository

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 3.1: MODULE IDENTIFICATION AND MAPPING

3.1.1. Identify all modules within the system.
    A module is a cohesive unit of functionality, typically
    represented by a directory or file group.

3.1.2. For each module, document:
    - Module name and purpose
    - Directory location
    - All files in the module
    - Main export/entry file
    - Programming language
    - Framework dependencies
    - Internal organization (how files relate within the module)
    - Public API (what is exposed to other modules)
    - Internal API (what is used within the module only)
    - Configuration requirements
    - State managed (files, variables, caches)
    - Data persisted (database collections, files, etc.)
    - Module lifecycle (init, run, shutdown hooks)
    - Error handling strategy
    - Testing approach

ACTIVITY 3.2: FILE-LEVEL ANALYSIS

3.2.1. For every file in the repository, document:
    - File path
    - Purpose and responsibility in one sentence
    - Exports (functions, classes, constants, types)
    - Imports (dependencies on other files/modules)
    - Size (lines of code)
    - Complexity indicators (number of functions, branches)
    - Whether it has associated tests
    - Whether it has associated documentation

3.2.2. For very large files (>500 lines), flag for special analysis.

3.2.3. Categorize each file:
    - Implementation: Contains business logic
    - Interface: Contains type/contract definitions
    - Configuration: Contains configuration
    - Utility: Contains helper/utility functions
    - Test: Contains tests
    - Script: Contains automation/utility scripts
    - Documentation: Contains documentation
    - Asset: Contains static assets
    - Build: Contains build configuration
    - Infrastructure: Contains deployment/infra config

ACTIVITY 3.3: CLASS ANALYSIS (Object-Oriented Code)

3.3.1. For every class, document:
    - Class name and file location
    - Purpose and responsibility
    - Inheritance hierarchy (parent class, interfaces)
    - Properties with types and purposes
    - Methods with signatures and purposes
    - Constructor/destructor behavior
    - Static members and their purposes
    - Access modifiers (public, protected, private)
    - Dependencies (what other classes it uses)
    - Instantiations (where it is created)
    - Singleton status (if applicable)
    - Thread-safety considerations

3.3.2. Generate a Mermaid class diagram for each module.

ACTIVITY 3.4: FUNCTION ANALYSIS

3.4.1. For every function/method, document:
    - Function name and location
    - Signature (parameters with types, return type)
    - Purpose and behavior
    - Algorithm or logic summary
    - Side effects (I/O, state changes, network calls)
    - Error conditions and how they are handled
    - Preconditions and postconditions
    - Performance characteristics (if determinable)
    - Recursion depth (if applicable)
    - Async behavior (promises, callbacks, async/await)

3.4.2. For complex functions (>50 lines):
    - Break down into logical steps
    - Document the control flow
    - Document all branches and conditions
    - Document loop logic and invariants

ACTIVITY 3.5: INTERFACE ANALYSIS

3.5.1. For every interface/type/contract:
    - Interface name and location
    - Purpose and contract description
    - All properties/methods defined
    - Implementations (which classes implement it)
    - Usage locations (where it is used as a type)
    - Generic/type parameters and constraints
    - Extension (if it extends other interfaces)

ACTIVITY 3.6: CALL GRAPH CONSTRUCTION

3.6.1. For critical execution paths:
    - Trace from entry point through the call chain
    - Document each function call in sequence
    - Note branching points and conditions
    - Document error handling paths
    - Document return values and their significance

3.6.2. Build call graphs for:
    - Main application flow
    - Key API endpoints
    - Critical background processes
    - Error handling paths
    - Initialization sequence

3.6.3. Generate Mermaid sequence diagrams for key workflows.

ACTIVITY 3.7: MODULE INTERNAL ORGANIZATION PATTERNS

3.7.1. Identify the organizational pattern within each module:
    - Feature-based (files grouped by feature)
    - Type-based (files grouped by type: controllers, services, etc.)
    - Layer-based (files grouped by architectural layer)
    - Hybrid approach

3.7.2. Document naming conventions:
    - File naming patterns
    - Class naming conventions
    - Function naming conventions
    - Variable naming conventions
    - Constant naming conventions

3.7.3. Document file organization conventions:
    - How are files ordered within directories?
    - How are imports organized?
    - What is the export pattern (named, default, barrel)?

ACTIVITY 3.8: MODULE RESPONSIBILITY MATRIX

3.8.1. Create a matrix mapping:
    - Modules to Features (which module implements which feature)
    - Modules to Files (which files belong to which module)
    - Modules to Tests (which tests cover which module)
    - Modules to Dependencies (which external libs each module uses)

========================================================================
ANALYSIS METHODOLOGY
========================================================================

This phase requires DeepScan depth for core modules and
StandardScan for auxiliary modules.

Core modules are those that:
- Contain primary business logic
- Are entry points or entry-adjacent
- Are heavily depended upon by other modules
- Implement critical system features

Auxiliary modules are those that:
- Provide utility/support functions
- Are configuration or setup files
- Are wrappers for external services

For auxiliary modules, document structure and purpose but not
every function/class in detail.

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT 3.1: MODULE_CATALOG.md
- Complete module inventory
- Module responsibility documentation
- Module organization patterns

ARTIFACT 3.2: FILE_RESPONSIBILITY_MATRIX.md
- Every file with its responsibility and category
- File-to-module mapping
- Export/import summary

ARTIFACT 3.3: CLASS_CATALOG.md
- Every class with full documentation
- Inheritance hierarchy
- Mermaid class diagrams

ARTIFACT 3.4: FUNCTION_CATALOG.md
- Key functions with documentation
- Complex function breakdowns
- async/sync classification

ARTIFACT 3.5: INTERFACE_CATALOG.md
- Every interface/type with documentation
- Implementation mapping
- Usage mapping

ARTIFACT 3.6: CALL_GRAPHS.md
- Mermaid call graphs for core paths
- Sequence diagrams for key workflows
- Control flow documentation

ARTIFACT 3.7: MODULE_RESPONSIBILITY_MATRIX.md
- Module-Feature mapping
- Module-Test mapping
- Module-Dependency mapping

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] Every module is identified and documented.
[ ] Every file has a documented responsibility.
[ ] Every class has been analyzed.
[ ] Every function in core modules has been analyzed.
[ ] Every interface has been documented.
[ ] Call graphs are complete for critical paths.
[ ] Module responsibility matrix is complete.
[ ] All diagrams accurately represent the code.
[ ] All findings cite specific file:line evidence.
[ ] Artifacts meet quality standards (score >= 4.0).

========================================================================
OUTPUTS TO NEXT PHASE
========================================================================

Pass to Phase 4:
- MODULE_CATALOG.md
- FILE_RESPONSIBILITY_MATRIX.md
- CLASS_CATALOG.md
- FUNCTION_CATALOG.md
- INTERFACE_CATALOG.md
- CALL_GRAPHS.md
- MODULE_RESPONSIBILITY_MATRIX.md

========================================================================
END OF PROMPT 03
========================================================================
