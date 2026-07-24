========================================================================
MASTER INDEX
========================================================================
Enterprise Reverse Engineering Prompt Framework
Version: 3.0
Status: FINAL

========================================================================
FRAMEWORK OVERVIEW
========================================================================

This is a modular, multi-phase prompt framework designed to guide
any AI agent through a complete, systematic reverse engineering
of any software repository.

The framework is organized into 10 Phases, with supporting
documentation and handbook files.

========================================================================
FILE INVENTORY
========================================================================

--- CORE FRAMEWORK FILES ---

MASTER_INDEX.md              - This file. Complete navigation index.
MASTER_PROMPT.md             - The master orchestration prompt that
                               coordinates all phases and sub-prompts.
PROJECT_SPECIFICATION.md     - Project scope, versioning, and
                               specification metadata.
PROMPT_DESIGN_GUIDE.md       - Design philosophy, prompt engineering
                               methodology, and design decisions.
MISSION.md                   - Core mission, vision, and principles.
OPERATING_RULES.md           - Operating rules, constraints, and
                               behavioral guidelines for the AI agent.
QUALITY_STANDARDS.md         - Quality standards for all generated
                               documentation and analysis artifacts.
OUTPUT_RULES.md              - Output formatting, structure, and
                               presentation rules.

--- PHASE PROMPTS ---

PROMPT_01_RECONNAISSANCE.md           - Phase 1: Initial repository
                                         reconnaissance and surface
                                         analysis.
PROMPT_02_ARCHITECTURE_DISCOVERY.md   - Phase 2: Architecture
                                         discovery and system design
                                         extraction.
PROMPT_03_MODULE_ANALYSIS.md          - Phase 3: Module and component
                                         deep-dive analysis.
PROMPT_04_DATA_FLOW.md                - Phase 4: Data flow, state
                                         management, and execution
                                         pipeline analysis.
PROMPT_05_BUSINESS_LOGIC.md           - Phase 5: Business logic,
                                         domain model, and algorithm
                                         extraction.
PROMPT_06_AI_WORKFLOWS.md             - Phase 6: AI, agent, prompt,
                                         and automation workflow
                                         analysis.
PROMPT_07_INTEGRATIONS.md             - Phase 7: Integration,
                                         dependency, and boundary
                                         analysis.
PROMPT_08_SECURITY_ERRORS.md          - Phase 8: Security, error
                                         handling, and resilience
                                         analysis.
PROMPT_09_DOCUMENTATION.md            - Phase 9: Comprehensive
                                         documentation generation.
PROMPT_10_VALIDATION.md               - Phase 10: Validation,
                                         cross-referencing, and
                                         quality assurance.

--- HANDBOOK FILES ---

HANDBOOK_ARCHITECTURE.md     - Architecture Handbook: Complete
                               system architecture reference.
HANDBOOK_DEVELOPER.md        - Developer Handbook: Engineering
                               guide for future developers.
HANDBOOK_REBUILD_GUIDE.md    - Rebuild Guide: How to rebuild the
                               system from scratch.
ENGINEERING_NOTES.md         - Engineering Notes: Deep technical
                               observations and analysis.
CROSS_REFERENCES.md          - Cross References: Multi-dimensional
                               cross-reference index.
VALIDATION_CHECKLIST.md      - Validation Checklist: Quality gates
                               and verification steps.

--- SUPPLEMENTARY PROMPTS ---

PROMPT_S1_MEMORY_ANALYSIS.md        - Memory and Caching Analysis
PROMPT_S2_TEST_ANALYSIS.md          - Testing and Quality Analysis
PROMPT_S3_EVENT_DRIVEN.md           - Event-Driven Architecture Analysis
PROMPT_S4_PERFORMANCE.md            - Performance and Scalability
                                      Analysis
PROMPT_S5_BUILD_CI.md               - Build, CI/CD, and DevOps Analysis
PROMPT_S6_I18N.md                   - Internationalization Analysis
PROMPT_S7_MONOREPO.md               - Monorepo and Multi-Package
                                      Analysis
PROMPT_S8_UI_COMPONENT.md           - UI Component Tree Analysis
PROMPT_S9_OBSERVABILITY.md          - Observability, Telemetry, and
                                      Monitoring Analysis
PROMPT_S10_DEPENDENCY_INJECTION.md  - IoC Containers and Dependency
                                      Injection Analysis
PROMPT_S11_SCHEDULED_TASKS.md       - Cron Jobs, Schedulers, and Timed
                                      Operation Analysis
PROMPT_S12_BACKGROUND_JOBS.md       - Background Workers and Async Job
                                      Processing Analysis
PROMPT_S13_SERIALIZATION.md         - Serialization, Marshaling, and
                                      Data Format Analysis
PROMPT_S14_DATA_MIGRATION.md        - Data Migration and ETL Pipeline
                                      Analysis

--- QUALITY ASSURANCE ---

SELF_IMPROVEMENT_REVIEW.md      - Self-review identifying gaps,
                                  strengths, and improvement actions

=======================================================================
NAVIGATION GUIDE
=======================================================================

For AI agents: Follow this order strictly unless otherwise instructed.

CORE PHASES:
STEP 1: Read MASTER_PROMPT.md for orchestration instructions.
STEP 2: Read MISSION.md and OPERATING_RULES.md for context.
STEP 3: Read PROJECT_SPECIFICATION.md for scope.
STEP 4: Execute phases in order from PROMPT_01 through PROMPT_10.
STEP 5: Generate handbook files after Phase 9.
STEP 6: Execute PROMPT_10_VALIDATION.md for final quality check.

SUPPLEMENTARY PROMPTS:
- Execute supplementary prompts S1-S14 as needed based on repository
  characteristics. Each supplementary prompt specifies when to use it
  and after which core phase it should be executed.

========================================================================
PROMPT DEPENDENCY GRAPH
========================================================================

PROMPT_01 (Reconnaissance)     --> PROMPT_02 (Architecture)
PROMPT_01 (Reconnaissance)     --> PROMPT_03 (Module Analysis)
PROMPT_02 (Architecture)       --> PROMPT_04 (Data Flow)
PROMPT_03 (Module Analysis)    --> PROMPT_04 (Data Flow)
PROMPT_04 (Data Flow)          --> PROMPT_05 (Business Logic)
PROMPT_05 (Business Logic)     --> PROMPT_06 (AI Workflows)
PROMPT_04 (Data Flow)          --> PROMPT_07 (Integrations)
PROMPT_05 (Business Logic)     --> PROMPT_08 (Security)
PROMPT_01-08 (All Prior)       --> PROMPT_09 (Documentation)
PROMPT_09 (Documentation)      --> PROMPT_10 (Validation)

SUPPLEMENTARY DEPENDENCY GRAPH:
PROMPT_S1 (Memory)             Execute after Phase 8
PROMPT_S2 (Testing)            Execute after Phase 9
PROMPT_S3 (Event-Driven)       Execute after Phase 4
PROMPT_S4 (Performance)        Execute after Phase 8
PROMPT_S5 (Build/CI)           Execute after Phase 1
PROMPT_S6 (I18N)               Execute after Phase 3
PROMPT_S7 (Monorepo)           Execute after Phase 1
PROMPT_S8 (UI Component)       Execute after Phase 3
PROMPT_S9 (Observability)      Execute after Phase 8
PROMPT_S10 (DI)                Execute after Phase 3
PROMPT_S11 (Scheduled Tasks)   Execute after Phase 4
PROMPT_S12 (Background Jobs)   Execute after Phase 4
PROMPT_S13 (Serialization)     Execute after Phase 4
PROMPT_S14 (Data Migration)    Execute after Phase 7

========================================================================
TERMINOLOGY
========================================================================

Repository / Codebase:      The software system being analyzed.
Target:                     The specific component under analysis.
Artifact:                   Any generated document, diagram, or file.
Phase:                      A major stage of the reverse engineering
                            process.
DeepScan:                   Complete line-by-line analysis of a file.
SurfaceScan:                High-level overview without deep analysis.
Entry Point:                Starting point of execution or analysis.
Call Graph:                 Map of function/method invocations.
Dependency Graph:           Map of module/package relationships.
Data Flow:                  Path of data through the system.
Control Flow:               Path of execution through the system.
State Machine:              Formal model of state transitions.
Boundary:                   Interface between components or systems.

========================================================================
END OF MASTER INDEX
========================================================================
