# 02 - File and Folder Analysis

This document provides a complete accounting of every file in the repository (303 files total), grouped by variant directory. Each file is documented with its purpose and key content based on actual file reading.

## Table of Contents

1. [Root Level](#1-root-level)
2. [Hermes With Deepseek v4 flash (Canonical)](#2-hermes-with-deepseek-v4-flash)
3. [Opencode With Deepseek v4 flash - Version 1](#3-opencode-with-deepseek-v4-flash---version-1)
4. [Opencode With Deepseek v4 flash - Version 2](#4-opencode-with-deepseek-v4-flash---version-2)
5. [Claude](#5-claude)
6. [GLM 5.1](#6-glm-51)
7. [Gemini With Gemini 3.1 Pro](#7-gemini-with-gemini-31-pro)
8. [Qwen](#8-qwen)
9. [Mistral](#9-mistral)
10. [Blackbox with Kimi K2.6](#10-blackbox-with-kimi-k26)
11. [Blackbox with Minimax M2.7](#11-blackbox-with-minimax-m27)
12. [Antigravity with Gemini 3.1 Pro high (Example Output)](#12-antigravity-with-gemini-31-pro-high)
13. [Excluded Items](#13-excluded-items)
14. [File Count Verification](#14-file-count-verification)

## 1. Root Level

**File count:** 1

| File | Purpose | Key Content |
|---|---|---|
| `README.md` | Global entry point and framework documentation | Framework overview, 9-phase pipeline description, variant comparison table, three-layer architecture diagram, usage instructions, file inventory, dependency map, glossary, contribution guidelines |

## 2. Hermes With Deepseek v4 flash

**File count:** 49 | **Status:** Canonical reference implementation | **Structure:** 13 infrastructure files + 36 prompts in 9 phase subfolders

### Infrastructure Layer (13 files)

| File | Purpose | Key Content |
|---|---|---|
| `MASTER_PROMPT.md` | Primary orchestrator entry point | Sequences all 36 sub-prompts; defines load instructions, execution model, phase management, context handoff protocol |
| `MASTER_INDEX.md` | Framework map and navigation index | Table of contents for all 49 files; framework architecture diagram; phase overview with prompt counts |
| `MISSION.md` | Core mission definition | Mission statement, rationale (6 objectives), scope boundaries (covered vs. not covered), success criteria |
| `PROJECT_SPECIFICATION.md` | Formal framework specification | Architecture contracts, component interfaces, behavioral rules, framework identification attributes |
| `OPERATING_RULES.md` | Binding agent behavior rules | Sequential discipline, quality gate enforcement, no-guessing policy, context preservation requirements |
| `OUTPUT_RULES.md` | Documentation formatting standards | Document structure rules, header conventions, diagram standards, table formatting, file naming |
| `QUALITY_STANDARDS.md` | Quality bar definitions (Q1-Q10) | 10 quality standards: accuracy, completeness, traceability, structural quality, diagram quality, consistency, clarity, verifiability, gate compliance, continuous improvement |
| `PROMPT_DESIGN_GUIDE.md` | Design rationale and extensibility guide | Modularity principles, language adaptation, common pitfalls, prompt template structure |
| `FRAMEWORK_DESIGN_PHILOSOPHY.md` | Theoretical underpinning | Failure mode analysis (5 modes), design thinking rationale, progressive deepening architecture |
| `PROMPT_DEPENDENCY_MAP.md` | Execution DAG | Directed dependency graph showing prompt ordering; parallelization opportunities; context handoff table |
| `GLOSSARY.md` | Standardized terminology (40+ terms) | Alphabetized definitions for framework-specific vocabulary (Agent, Anchor, Anti-pattern, etc.) |
| `DIAGRAM_TEMPLATES.md` | Reusable Mermaid diagram templates | 13 diagram templates: system context, component, sequence, data flow, state machine, deployment, etc. |
| `VALIDATION_CHECKLISTS.md` | Phase-level quality checklists | Per-phase pass/fail criteria for sign-off; file inventory validation; architecture completeness checks |

### Phase 1 - Discovery (3 files)

| File | Purpose | Key Content |
|---|---|---|
| `Phase 1/PROMPT_01_Repository_Scan.md` | Initial repository scan | First-contact analysis; repo type classification, language breakdown, build system identification |
| `Phase 1/PROMPT_02_File_Inventory.md` | Comprehensive file listing | File categorization by type, language, and purpose; complete inventory with classification |
| `Phase 1/PROMPT_03_Technology_Stack_Detection.md` | Technology stack detection | Framework/library/tool identification; version detection from lockfiles; tech stack matrix |

### Phase 2 - Structural Analysis (3 files)

| File | Purpose | Key Content |
|---|---|---|
| `Phase 2/PROMPT_04_Folder_Architecture.md` | Directory structure analysis | Naming conventions, organizational patterns, folder architecture diagram |
| `Phase 2/PROMPT_05_Module_Dependency_Graph.md` | Module dependency mapping | Import/require analysis; dependency graph construction between modules/packages |
| `Phase 2/PROMPT_06_Entry_Point_Analysis.md` | Entry point identification | CLI, API, constructor, main function, and event handler registry with invocation signatures |

### Phase 3 - Architecture Reconstruction (4 files)

| File | Purpose | Key Content |
|---|---|---|
| `Phase 3/PROMPT_07_System_Architecture.md` | High-level architecture reconstruction | Component relationships; system architecture diagram from code structure |
| `Phase 3/PROMPT_08_Component_Decomposition.md` | Component identification and classification | Component catalog with responsibilities and interfaces |
| `Phase 3/PROMPT_09_Layer_Analysis.md` | Architectural layer identification | Layer diagram; data flow between layers; layer interaction rules |
| `Phase 3/PROMPT_10_Design_Pattern_Recognition.md` | Design pattern detection | Pattern catalog with code locations and rationale for each detected pattern |

### Phase 4 - Deep Code Analysis (5 files)

| File | Purpose | Key Content |
|---|---|---|
| `Phase 4/PROMPT_11_Data_Flow_Analysis.md` | Data flow tracing | Input-to-output data tracing; transformation pipeline documentation |
| `Phase 4/PROMPT_12_Execution_Path_Reconstruction.md` | Execution path mapping | Control flow diagrams; primary execution paths with branching logic |
| `Phase 4/PROMPT_13_State_Management_Analysis.md` | State management analysis | State machine diagrams; state transition tables |
| `Phase 4/PROMPT_14_Error_Handling_Retry_Strategy.md` | Error handling documentation | Error classification, handling matrix, recovery flows, retry strategies |
| `Phase 4/PROMPT_15_Concurrency_Performance_Analysis.md` | Concurrency and performance analysis | Concurrency model diagrams; bottleneck analysis; threading/async patterns |

### Phase 5 - AI and Automation Analysis (5 files)

| File | Purpose | Key Content |
|---|---|---|
| `Phase 5/PROMPT_16_Prompt_Architecture_Analysis.md` | Prompt architecture analysis | Prompt file mapping; template analysis; system prompt dependency chain |
| `Phase 5/PROMPT_17_Agent_Workflow_Reconstruction.md` | Agent workflow decomposition | Agent workflow diagrams; decision trees; handoff points |
| `Phase 5/PROMPT_18_Tool_Integration_Analysis.md` | Tool integration mapping | Tool definitions, MCP servers, external integration registry |
| `Phase 5/PROMPT_19_Planning_Reasoning_Pipeline.md` | Planning/reasoning system analysis | ReAct, CoT pipeline decomposition; reasoning step documentation |
| `Phase 5/PROMPT_20_Memory_RAG_Workflow_Analysis.md` | Memory and RAG workflow analysis | Memory architecture; vector store topology; retrieval pipeline documentation |

### Phase 6 - Integration and Boundary Analysis (4 files)

| File | Purpose | Key Content |
|---|---|---|
| `Phase 6/PROMPT_21_Internal_API_Contract_Analysis.md` | Internal API documentation | API contract catalog; request/response schemas; interface definitions |
| `Phase 6/PROMPT_22_External_Service_Integration.md` | External dependency mapping | External dependency matrix; integration patterns; third-party service analysis |
| `Phase 6/PROMPT_23_Event_Stream_Workflow.md` | Event-driven communication analysis | Event catalog; producer/consumer maps; async workflow documentation |
| `Phase 6/PROMPT_24_Configuration_Environment_Analysis.md` | Configuration system documentation | Configuration schema; environment variable registry; config management patterns |

### Phase 7 - Documentation Generation (6 files)

| File | Purpose | Key Content |
|---|---|---|
| `Phase 7/PROMPT_25_Architecture_Handbook_Generation.md` | Architecture handbook synthesis | Comprehensive architecture reference with all diagrams |
| `Phase 7/PROMPT_26_Developer_Handbook_Generation.md` | Developer handbook creation | Onboarding guide; code conventions; practical development reference |
| `Phase 7/PROMPT_27_Rebuild_Guide_Generation.md` | Rebuild guide creation | Step-by-step rebuild instructions from scratch |
| `Phase 7/PROMPT_28_API_Reference_Class_Catalog.md` | API and class documentation | Complete API reference; class catalog; endpoint documentation |
| `Phase 7/PROMPT_29_Engineering_Notes_Cross_References.md` | Engineering notes capture | Edge cases, gotchas, cross-module references, institutional wisdom |
| `Phase 7/PROMPT_30_Validation_Handover_Protocol.md` | Handoff documentation | Handover summary; known gaps; assumptions; final delivery protocol |

### Phase 8 - Validation and Quality (4 files)

| File | Purpose | Key Content |
|---|---|---|
| `Phase 8/PROMPT_31_Cross_Phase_Accuracy_Validation.md` | Cross-phase accuracy validation | Factual accuracy verification across all phases; corrected findings report |
| `Phase 8/PROMPT_32_Completeness_Deep_Audit.md` | Completeness auditing | Gap analysis; coverage verification; missing documentation identification |
| `Phase 8/PROMPT_33_Consistency_Contradiction_Verification.md` | Consistency verification | Contradiction detection between phases; resolution documentation |
| `Phase 8/PROMPT_34_Final_Quality_Gate_Signoff.md` | Final quality gate sign-off | Quality score; sign-off certificate; final pass/fail determination |

### Phase 9 - Rebuild Package (2 files)

| File | Purpose | Key Content |
|---|---|---|
| `Phase 9/PROMPT_35_Rebuild_Package_Assembly.md` | Rebuild package assembly | Packaging all rebuild artifacts; dependency list; build order |
| `Phase 9/PROMPT_36_Rebuild_Verification_Protocol.md` | Rebuild verification protocol | Verification checklist; acceptance criteria for rebuilt system |

## 3. Opencode With Deepseek v4 flash - Version 1

**File count:** 37 | **Status:** Complete | **Structure:** Flat directory with 9 infrastructure + 23 prompts + 5 supporting files

### Infrastructure Files (9 files)

| File | Purpose | Key Content |
|---|---|---|
| `MASTER_PROMPT.md` | Primary orchestrator | Workflow sequencing for 23 prompts; load order; execution protocol |
| `MASTER_INDEX.md` | Framework navigation map | Layer breakdown (8 core, 23 prompts, 4 supporting, 2 operational) |
| `MISSION.md` | Mission definition | Core objectives for AI-powered repository reverse engineering |
| `PROJECT_SPECIFICATION.md` | Scope and constraints | Framework specification; language-agnostic design; scaling parameters |
| `OPERATING_RULES.md` | Agent behavior constraints | Sequential execution rules; continuation protocol; ambiguity handling |
| `OUTPUT_RULES.md` | Output formatting conventions | File/folder naming; cross-referencing conventions; structure requirements |
| `QUALITY_STANDARDS.md` | Quality metrics | Anti-hallucination rules; completeness bar; verification criteria |
| `PROMPT_DESIGN_GUIDE.md` | Design rationale | Prompt architecture decisions; extensibility patterns |
| `README.md` | Variant-specific documentation | Quick start guide; layer breakdown; phase execution instructions |

### Execution Prompts (23 files)

| File | Purpose | Key Content |
|---|---|---|
| `PROMPT_01_SCOUTING.md` | Phase 00: Project scouting | Initial survey; repository profiling; basic metrics gathering |
| `PROMPT_02_STRUCTURE.md` | Directory structure analysis | Folder hierarchy mapping; naming convention detection |
| `PROMPT_03_BUILD_CONFIG.md` | Build configuration analysis | Build system identification; config file parsing |
| `PROMPT_04_DEPENDENCIES.md` | Dependency analysis | Package/library dependency extraction and mapping |
| `PROMPT_05_TECH_STACK.md` | Tech stack identification | Language, framework, and tool detection with versions |
| `PROMPT_06_MODULES.md` | Module analysis | Module boundary detection; responsibility mapping |
| `PROMPT_07_DEEP_READ.md` | Deep code reading | Line-by-line analysis of critical files |
| `PROMPT_08_ARCHITECTURE.md` | Architecture reconstruction | System architecture extraction from code |
| `PROMPT_09_DATA_FLOW.md` | Data flow analysis | Data movement tracing through system layers |
| `PROMPT_10_CALL_GRAPH.md` | Call graph construction | Function/method invocation chain mapping |
| `PROMPT_11_FEATURES.md` | Feature extraction | User-facing feature identification and documentation |
| `PROMPT_12_ALGORITHMS.md` | Algorithm extraction | Core algorithm identification and documentation |
| `PROMPT_13_DESIGN_PATTERNS.md` | Design pattern detection | Pattern recognition with code location references |
| `PROMPT_14_API_BOUNDARIES.md` | API boundary analysis | Interface contract documentation; endpoint mapping |
| `PROMPT_15_STATE_EVENTS.md` | State and event analysis | State management patterns; event-driven architecture |
| `PROMPT_16_ERROR_CACHE_RETRY.md` | Error handling and caching | Error patterns; caching strategies; retry mechanisms |
| `PROMPT_17_AI_WORKFLOWS.md` | AI workflow analysis | Agent architectures; prompt pipelines; LLM integration |
| `PROMPT_18_CONFIG_ENV.md` | Configuration/environment | Config management; environment variable analysis |
| `PROMPT_19_DOCUMENTATION.md` | Documentation generation | Final documentation synthesis and assembly |
| `PROMPT_20_VALIDATION.md` | Validation and QA | Cross-phase validation; accuracy verification |
| `PROMPT_21_STREAMING.md` | Streaming analysis | Real-time data streaming patterns and protocols |
| `PROMPT_22_AUTH_ARCHITECTURE.md` | Authentication architecture | Auth/authz pattern analysis; security flow documentation |
| `PROMPT_23_DEPLOYMENT.md` | Deployment analysis | Deployment topology; infrastructure patterns |

### Supporting Files (5 files)

| File | Purpose | Key Content |
|---|---|---|
| `CHECKLIST.md` | Comprehensive QA checklist | 250+ check items across all phases; pass/fail criteria |
| `DIAGRAM_GUIDE.md` | Diagram generation standards | Requirements for when/how to generate Mermaid diagrams |
| `GLOSSARY.md` | Terminology definitions | Shared vocabulary for consistent documentation |
| `TEMPLATES.md` | Documentation templates | Reusable templates for file headers, modules, components |
| `TROUBLESHOOTING.md` | Failure diagnosis guide | Common failure patterns; file parsing issues; resolution steps |

## 4. Opencode With Deepseek v4 flash - Version 2

**File count:** 39 | **Status:** Complete (v3.0) | **Structure:** Flat directory with 11 infrastructure + 10 core prompts + 14 supplementary prompts + 4 handbook templates

### Infrastructure Files (11 files)

| File | Purpose | Key Content |
|---|---|---|
| `MASTER_PROMPT.md` | Primary orchestrator | Execution sequencing for core and supplementary prompts |
| `MASTER_INDEX.md` | Framework overview (v3.0) | Modular multi-phase framework index; status FINAL |
| `MISSION.md` | Mission definition | Core mission for systematic repository reverse engineering |
| `PROJECT_SPECIFICATION.md` | Scope and specification | Framework contracts and scope boundaries |
| `OPERATING_RULES.md` | Agent behavior rules | Execution discipline; context management; pacing rules |
| `OUTPUT_RULES.md` | Output formatting | Document structure; naming conventions; formatting standards |
| `QUALITY_STANDARDS.md` | Quality metrics | Verification criteria; anti-hallucination measures |
| `PROMPT_DESIGN_GUIDE.md` | Design guide | Prompt engineering rationale and extensibility |
| `VALIDATION_CHECKLIST.md` | Validation template | Template for AI to populate with pass/fail results per section |
| `SELF_IMPROVEMENT_REVIEW.md` | Framework self-review | Strengths, weaknesses, gaps, and improvement opportunities |
| `CROSS_REFERENCES.md` | Cross-reference template | Template for module-file cross-reference mapping |

### Core Prompts (10 files)

| File | Purpose | Key Content |
|---|---|---|
| `PROMPT_01_RECONNAISSANCE.md` | Phase 1: Initial reconnaissance | Repository surface analysis; first-contact profiling |
| `PROMPT_02_ARCHITECTURE_DISCOVERY.md` | Architecture discovery | High-level architecture extraction |
| `PROMPT_03_MODULE_ANALYSIS.md` | Module analysis | Module boundary and responsibility mapping |
| `PROMPT_04_DATA_FLOW.md` | Data flow analysis | Data movement and transformation tracing |
| `PROMPT_05_BUSINESS_LOGIC.md` | Business logic extraction | Domain model and business rule documentation |
| `PROMPT_06_AI_WORKFLOWS.md` | AI workflow analysis | Agent and LLM integration pattern analysis |
| `PROMPT_07_INTEGRATIONS.md` | Integration analysis | External/internal integration mapping |
| `PROMPT_08_SECURITY_ERRORS.md` | Security and error analysis | Security patterns; error handling; resilience |
| `PROMPT_09_DOCUMENTATION.md` | Documentation synthesis | Final documentation assembly |
| `PROMPT_10_VALIDATION.md` | Validation | Cross-phase accuracy and completeness checks |

### Supplementary Prompts (14 files)

| File | Purpose | Key Content |
|---|---|---|
| `PROMPT_S1_MEMORY_ANALYSIS.md` | Memory/persistence deep dive | Memory, persistence, and data storage patterns |
| `PROMPT_S2_TEST_ANALYSIS.md` | Test analysis | Test structure; coverage assessment; test strategy |
| `PROMPT_S3_EVENT_ANALYSIS.md` | Event analysis | Event-driven patterns; pub/sub; message queues |
| `PROMPT_S4_PERFORMANCE_ANALYSIS.md` | Performance analysis | Performance architecture; optimization patterns |
| `PROMPT_S5_BUILD_CI_ANALYSIS.md` | Build/CI analysis | Build pipeline; CI/CD configuration; automation |
| `PROMPT_S6_I18N_LOCALIZATION.md` | Internationalization | i18n/l10n patterns and implementation |
| `PROMPT_S7_MONOREPO_ANALYSIS.md` | Monorepo analysis | Workspace structure; package relationships |
| `PROMPT_S8_UI_COMPONENT_ANALYSIS.md` | UI component analysis | Frontend component architecture; design system |
| `PROMPT_S9_OBSERVABILITY_ANALYSIS.md` | Observability analysis | Logging, tracing, monitoring, alerting |
| `PROMPT_S10_DEPENDENCY_INJECTION.md` | Dependency injection | IoC patterns; DI container analysis |
| `PROMPT_S11_SCHEDULED_TASKS.md` | Scheduled tasks | Cron jobs; background schedulers; periodic tasks |
| `PROMPT_S12_BACKGROUND_JOBS.md` | Background jobs | Async job processing; worker patterns; queues |
| `PROMPT_S13_SERIALIZATION.md` | Serialization | Data serialization formats; encoding patterns |
| `PROMPT_S14_DATA_MIGRATION.md` | Data migration | Database migrations; schema evolution; data transforms |

### Handbook Templates (4 files)

| File | Purpose | Key Content |
|---|---|---|
| `HANDBOOK_ARCHITECTURE.md` | Architecture handbook template | Template for system overview; to be populated by AI execution |
| `HANDBOOK_DEVELOPER.md` | Developer handbook template | Template for repository overview and developer guidance |
| `HANDBOOK_REBUILD_GUIDE.md` | Rebuild guide template | Template for prerequisites and rebuild steps |
| `ENGINEERING_NOTES.md` | Engineering notes template | Template for deep technical observations |

## 5. Claude

**File count:** 20 | **Status:** Complete | **Structure:** 2 root files + 18 files in Prompts/ subfolder

### Root Files (2 files)

| File | Purpose | Key Content |
|---|---|---|
| `reverse-engineering-master-prompt.md` | Standalone master prompt | Single-paste prompt for Claude Code/VS Code; defines 10 phases; ground rules (no fabrication, rebuild-guide bias); designed for single-session execution |
| `REVERSE_ENGINEERING_PROMPT_COMBINED.md` | Single-file combined version | Complete framework in one file; master index, file map table, all phase instructions combined for direct paste into Claude |

### Prompts/ Subfolder (18 files)

| File | Purpose | Key Content |
|---|---|---|
| `Prompts/MASTER_INDEX.md` | Framework navigation | File map with purpose table; usage instructions (4-step process) |
| `Prompts/MASTER_PROMPT.md` | Orchestrator prompt | Top-level instruction that sequences PROMPT_01 through PROMPT_10 |
| `Prompts/MISSION.md` | Mission definition | Framework purpose; definition of done |
| `Prompts/PROJECT_SPECIFICATION.md` | Scope and constraints | Framework boundaries and non-goals |
| `Prompts/OPERATING_RULES.md` | Agent behavior rules | Continuation, pacing, ambiguity handling |
| `Prompts/OUTPUT_RULES.md` | Output format rules | File/folder naming; structure; cross-referencing |
| `Prompts/QUALITY_STANDARDS.md` | Quality bar | Anti-hallucination rules; completeness criteria |
| `Prompts/PROMPT_DESIGN_GUIDE.md` | Design rationale | Framework structure explanation; extension guidance |
| `Prompts/PROMPT_01_REPOSITORY_INTELLIGENCE.md` | Phase 1: Repository intelligence | Top-level scan; stack detection; folder tree; entry points |
| `Prompts/PROMPT_02_FILE_FOLDER_ANALYSIS.md` | Phase 2: File/folder analysis | Per-file purpose documentation; complete accounting |
| `Prompts/PROMPT_03_FUNCTION_CLASS_DOCS.md` | Phase 3: Function/class docs | Code-level documentation; signatures; logic |
| `Prompts/PROMPT_04_ARCHITECTURE_RECONSTRUCTION.md` | Phase 4: Architecture | Component map; system design; patterns |
| `Prompts/PROMPT_05_DIAGRAMS.md` | Phase 5: Diagrams | Mermaid diagram generation; visual documentation |
| `Prompts/PROMPT_06_AI_AGENT_WORKFLOW.md` | Phase 6: AI agent workflow | Agent architecture; prompt chains; tool usage |
| `Prompts/PROMPT_07_TECH_STACK.md` | Phase 7: Tech stack | Technology inventory; versions; dependencies |
| `Prompts/PROMPT_08_CONDITIONAL_DOCS.md` | Phase 8: Conditional docs | Domain-specific documentation (when applicable) |
| `Prompts/PROMPT_09_DEVELOPER_HANDBOOK_REBUILD.md` | Phase 9: Developer handbook | Rebuild guide; developer onboarding; handbook synthesis |
| `Prompts/PROMPT_10_VALIDATION_QA.md` | Phase 10: Validation/QA | Cross-phase verification; final quality gate |

## 6. GLM 5.1

**File count:** 39 | **Status:** Complete | **Structure:** Flat directory with 8 infrastructure + 30 prompts + 1 .docx file

### Infrastructure Files (8 files)

| File | Purpose | Key Content |
|---|---|---|
| `MASTER_PROMPT.md` | System prompt / runtime bootstrap | Agent identity (EREPF-Agent); load instructions; dispatch protocol; governance binding |
| `MASTER_INDEX.md` | Framework map | 9-phase structure; 30 prompt references; infrastructure file listing |
| `MISSION.md` | Mission definition | Core objectives; scope; success criteria |
| `PROJECT_SPECIFICATION.md` | Framework specification | Scope, constraints, architecture contracts |
| `OPERATING_RULES.md` | Agent rules | Execution discipline; sequential requirements |
| `OUTPUT_RULES.md` | Output format | Documentation conventions; formatting standards |
| `QUALITY_STANDARDS.md` | Quality metrics | Accuracy, completeness, traceability requirements |
| `PROMPT_DESIGN_GUIDE.md` | Design guide | Prompt engineering rationale; extensibility |

### Execution Prompts (30 files)

| File | Purpose | Key Content |
|---|---|---|
| `PROMPT_01.md` | Repository intake and boundary definition | Initial repository profiling; scope establishment |
| `PROMPT_02.md` | Technology stack and dependency analysis | Language/framework/tool detection with versions |
| `PROMPT_03.md` | Folder and file system cartography | Directory structure mapping and documentation |
| `PROMPT_04.md` | Build system and configuration analysis | Build tool identification; config file parsing |
| `PROMPT_05.md` | Entry points and bootstrap analysis | Main files; initialization sequences; bootstrap order |
| `PROMPT_06.md` | Module architecture extraction | Module boundaries; responsibilities; interfaces |
| `PROMPT_07.md` | Component architecture analysis | Component identification; relationships; contracts |
| `PROMPT_08.md` | Class and interface documentation | Class hierarchies; interface contracts; type systems |
| `PROMPT_09.md` | Function-level reverse engineering | Function signatures; logic flows; side effects |
| `PROMPT_10.md` | Call graph and dependency graph construction | Invocation chains; dependency visualization |
| `PROMPT_11.md` | Data flow analysis | Data movement tracing; transformation pipelines |
| `PROMPT_12.md` | Control flow and execution path analysis | Branching logic; execution sequences |
| `PROMPT_13.md` | State management analysis | State machines; transition tables; state patterns |
| `PROMPT_14.md` | Event workflow analysis | Event-driven patterns; pub/sub; handlers |
| `PROMPT_15.md` | API and interface documentation | Endpoint mapping; request/response schemas |
| `PROMPT_16.md` | Middleware and pipeline analysis | Middleware chains; processing pipelines |
| `PROMPT_17.md` | Error handling and resilience analysis | Error patterns; recovery strategies; circuit breakers |
| `PROMPT_18.md` | Caching and performance strategy | Cache layers; performance optimization patterns |
| `PROMPT_19.md` | Authentication and authorization analysis (optional) | Auth flows; permission models; security architecture |
| `PROMPT_20.md` | Database and persistence layer analysis (optional) | Schema analysis; query patterns; ORM mapping |
| `PROMPT_21.md` | AI/LLM workflow analysis | Agent architectures; prompt chains; model integration |
| `PROMPT_22.md` | Streaming workflow analysis | Real-time data streams; WebSocket; SSE patterns |
| `PROMPT_23.md` | Design pattern identification | Pattern catalog; code locations; usage rationale |
| `PROMPT_24.md` | Engineering decisions and trade-off reconstruction | Architectural decisions; trade-off documentation |
| `PROMPT_25.md` | Diagram generation (Mermaid, UML, Sequence, Flowchart) | Visual documentation; diagram synthesis |
| `PROMPT_26.md` | Rebuild guide and architecture handbook | System rebuild specification; architecture reference |
| `PROMPT_27.md` | Developer handbook | Onboarding guide; code conventions; workflows |
| `PROMPT_28.md` | Cross-reference and validation checklists | Inter-module references; validation criteria |
| `PROMPT_29.md` | Final documentation assembly | Documentation consolidation; output packaging |
| `PROMPT_30.md` | Self-review and quality assurance | Final QA pass; accuracy verification; sign-off |

### Binary File (1 file)

| File | Purpose | Key Content |
|---|---|---|
| `Enterprise_Reverse_Engineering_Prompt_Framework_TechnicalDoc_2026-07-24.docx` | Technical documentation export | Word document version of the framework (binary, not Markdown) |

## 7. Gemini With Gemini 3.1 Pro

**File count:** 3 | **Status:** Foundational (minimal) | **Structure:** 3 flat files

| File | Purpose | Key Content |
|---|---|---|
| `Master Index.md` | Entry point and execution instruction | Start instruction directing agent to begin Phase 0; continuation rules; single-question exception protocol |
| `Mission Directive.md` | Mission directive | Empty/placeholder file (0 bytes); reserved for future expansion |
| `Operating Rules.md` | Operating rules | Empty/placeholder file (0 bytes); reserved for future expansion |

**Note:** This variant is intentionally minimal. It provides only a master index with execution instructions, relying on Gemini 3.1 Pro extended context window to operate with less explicit infrastructure than other variants.

## 8. Qwen

**File count:** 15 | **Status:** Complete | **Structure:** 10 root files + 3 prompts + 2 templates

### Infrastructure Files (10 files)

| File | Purpose | Key Content |
|---|---|---|
| `MASTER_PROMPT.md` | Entry point and role definition | Expert AI architect role; task overview; ERE-PF execution instructions |
| `MASTER_INDEX.md` | Framework navigation | File listing; phase structure; execution order |
| `MISSION.md` | Mission statement | Core objectives; success criteria |
| `PROJECT_SPECIFICATION.md` | Framework specification | Scope, architecture, constraints |
| `OPERATING_RULES.md` | Agent rules | Behavior constraints; execution discipline |
| `OUTPUT_RULES.md` | Output format | Documentation standards; naming conventions |
| `QUALITY_STANDARDS.md` | Quality bar | Verification criteria; accuracy requirements |
| `PROMPT_DESIGN_GUIDE.md` | Design rationale | Architecture decisions; extensibility |
| `GLOSSARY.md` | Terminology (A-Z) | Accuracy, Architecture, Component definitions and more |
| `README.md` | Variant documentation | Overview; capabilities list; usage instructions; quick start |

### Prompts (3 files)

| File | Purpose | Key Content |
|---|---|---|
| `prompts/PROMPT_01_REPOSITORY_DISCOVERY.md` | Repository discovery | Initial repository scan; structure mapping |
| `prompts/PROMPT_02_TECH_STACK_ANALYSIS.md` | Tech stack analysis | Language/framework/tool detection |
| `prompts/PROMPT_03_ARCHITECTURE_EXTRACTION.md` | Architecture extraction | System architecture reconstruction |

### Templates (2 files)

| File | Purpose | Key Content |
|---|---|---|
| `templates/ARCHITECTURE_TEMPLATE.md` | Architecture documentation template | Standard template for system architecture documentation |
| `templates/COMPONENT_TEMPLATE.md` | Component documentation template | Standard template for individual component documentation |

## 9. Mistral

**File count:** 13 | **Status:** Complete | **Structure:** 1 root file (Version 1) + 12 files in Mistral 2/ subfolder

### Root File (1 file)

| File | Purpose | Key Content |
|---|---|---|
| `Version 1.md` | Complete single-file framework (v1) | Full master index; file manifest (7 core + 48 prompts planned); phase-by-phase listing; designed as a monolithic reference document |

### Mistral 2/ Subfolder (12 files)

#### Infrastructure (10 files)

| File | Purpose | Key Content |
|---|---|---|
| `Mistral 2/MASTER_PROMPT.md` | Master prompt orchestrator | Enterprise-grade RE agent mission; 5 imperatives (comprehend, reconstruct, document, maintain, ensure); escaped Markdown formatting |
| `Mistral 2/MASTER_INDEX.md` | Framework index | File map; component listing; phase structure |
| `Mistral 2/MISSION.md` | Mission definition | Core objective; comprehensive RE with maximum accuracy |
| `Mistral 2/PROJECT_SPECIFICATION.md` | Specification | Technical specifications; scope boundaries |
| `Mistral 2/OUTPUT_RULES.md` | Output formatting | Documentation structure; naming; conventions |
| `Mistral 2/QUALITY_STANDARDS.md` | Quality standards | Validation gates; accuracy requirements |
| `Mistral 2/PROMPT_DESIGN_GUIDE.md` | Design guide | Prompt architecture; design patterns |
| `Mistral 2/ANALYSIS STRATEGIES.md` | Analysis strategies | Repository analysis approach; execution flow (structural scan, dependency mapping, architecture discovery, code structure) |
| `Mistral 2/EXECUTION WORKFLOW.md` | Execution workflow | Phase 0 (preparation) through Phase 1 (discovery); step-by-step execution protocol |
| `Mistral 2/SUCCESS CRITERIA.md` | Success criteria | Completion conditions (100% documentation, accuracy, diagrams, validation, rebuild capability) |

#### Prompt Modules (1 file)

| File | Purpose | Key Content |
|---|---|---|
| `Mistral 2/PROMPT MODULES.md` | Prompt module definitions | 11 total prompt modules defined in a single file |

#### Combined Version (1 file)

| File | Purpose | Key Content |
|---|---|---|
| `Mistral 2/All in one version/Enterprise Reverse Engineering Framework Design.md` | Single-file combined framework | Complete framework (v1.0.0, production-ready) in one document; all instructions consolidated |

## 10. Blackbox with Kimi K2.6

**File count:** 36 | **Status:** Complete | **Structure:** 8 infrastructure files + 28 prompts across 6 domain subfolders

### Infrastructure Files (8 files)

| File | Purpose | Key Content |
|---|---|---|
| `MASTER_PROMPT.md` | Activation instruction and orchestrator | Senior RE AI Agent role binding; 6-file load sequence; shared context YAML initialization |
| `MASTER_INDEX.md` | Framework map (v1.0.0) | 34 modular prompt files in 7 core domains; enterprise-grade classification |
| `MISSION.md` | Mission statement | Uncompromising modular prompt framework; industry standard vision for AI-powered RE |
| `OPERATING_RULES.md` | Operating rules | Rule 1: Primacy of Understanding (complete understanding BEFORE documentation); framework violation definitions |
| `OUTPUT_RULES.md` | Output rules | Output classification; type definitions; formatting standards |
| `PROJECT_SPECIFICATION.md` | Project specification | Framework identification; architecture; interfaces |
| `PROMPT_DESIGN_GUIDE.md` | Prompt design guide | Core design principles; prompt architecture patterns |
| `QUALITY_STANDARDS.md` | Quality standards | Quality framework overview; quality dimensions |

### Domain 2 - Discovery (4 files)

| File | Purpose | Key Content |
|---|---|---|
| `DOMAIN_2_DISCOVERY/PROMPT_01_REPO_DISCOVERY.md` | Repository discovery and structure mapping | Phase 1 entry point; file system analysis; proportional effort (100-500 files) |
| `DOMAIN_2_DISCOVERY/PROMPT_02_LANGUAGE_DETECTION.md` | Language detection | Programming language identification and distribution |
| `DOMAIN_2_DISCOVERY/PROMPT_03_DEPENDENCY_MAPPING.md` | Dependency mapping | Package and library dependency extraction |
| `DOMAIN_2_DISCOVERY/PROMPT_04_CONFIG_ANALYSIS.md` | Configuration analysis | Config file parsing; environment setup documentation |

### Domain 3 - Architecture (5 files)

| File | Purpose | Key Content |
|---|---|---|
| `DOMAIN_3_ARCHITECTURE/PROMPT_05_ARCHITECTURE_HIGH_LEVEL.md` | High-level architecture | System-level architecture reconstruction |
| `DOMAIN_3_ARCHITECTURE/PROMPT_06_MODULE_DECOMPOSITION.md` | Module decomposition | Module boundary identification; responsibility mapping |
| `DOMAIN_3_ARCHITECTURE/PROMPT_07_COMPONENT_ANALYSIS.md` | Component analysis | Component catalog; interfaces; contracts |
| `DOMAIN_3_ARCHITECTURE/PROMPT_08_DATA_FLOW_MAPPING.md` | Data flow mapping | Data movement tracing; transformation documentation |
| `DOMAIN_3_ARCHITECTURE/PROMPT_09_DEPENDENCY_GRAPH.md` | Dependency graph | Directed dependency visualization; module relationships |

### Domain 4 - Deep Intelligence (5 files)

| File | Purpose | Key Content |
|---|---|---|
| `DOMAIN_4_DEEP_INTELLIGENCE/PROMPT_10_CLASS_FUNCTION_ANALYSIS.md` | Class/function analysis | Code-level documentation; signatures; logic flows |
| `DOMAIN_4_DEEP_INTELLIGENCE/PROMPT_11_ALGORITHM_EXTRACTION.md` | Algorithm extraction | Core algorithm identification; complexity analysis |
| `DOMAIN_4_DEEP_INTELLIGENCE/PROMPT_12_DESIGN_PATTERN_DETECTION.md` | Design pattern detection | Pattern recognition; code location mapping |
| `DOMAIN_4_DEEP_INTELLIGENCE/PROMPT_13_ERROR_HANDLING_ANALYSIS.md` | Error handling analysis | Error patterns; exception flows; recovery |
| `DOMAIN_4_DEEP_INTELLIGENCE/PROMPT_14_STATE_MANAGEMENT.md` | State management | State machines; transitions; persistence patterns |

### Domain 5 - Workflow (5 files)

| File | Purpose | Key Content |
|---|---|---|
| `DOMAIN_5_WORKFLOW/PROMPT_15_EXECUTION_PIPELINE.md` | Execution pipeline | Primary execution paths; control flow |
| `DOMAIN_5_WORKFLOW/PROMPT_16_EVENT_WORKFLOW.md` | Event workflow | Event-driven patterns; handler chains |
| `DOMAIN_5_WORKFLOW/PROMPT_17_AI_WORKFLOW_ANALYSIS.md` | AI workflow analysis | Agent architectures; LLM integration; prompt pipelines |
| `DOMAIN_5_WORKFLOW/PROMPT_18_TOOL_INTEGRATION.md` | Tool integration | External tool mapping; MCP; API integrations |
| `DOMAIN_5_WORKFLOW/PROMPT_19_CACHING_PERFORMANCE.md` | Caching and performance | Cache strategies; performance optimization |

### Domain 6 - Documentation (5 files)

| File | Purpose | Key Content |
|---|---|---|
| `DOMAIN_6_DOCUMENTATION/PROMPT_20_DOCUMENTATION_ARCHITECTURE.md` | Architecture documentation | Architecture handbook generation |
| `DOMAIN_6_DOCUMENTATION/PROMPT_21_DOCUMENTATION_TECHNICAL.md` | Technical documentation | API reference; class catalog; technical specs |
| `DOMAIN_6_DOCUMENTATION/PROMPT_22_DOCUMENTATION_DEVELOPER.md` | Developer documentation | Developer handbook; onboarding guide |
| `DOMAIN_6_DOCUMENTATION/PROMPT_23_DOCUMENTATION_DIAGRAMS.md` | Diagram documentation | Mermaid diagram generation; visual documentation |
| `DOMAIN_6_DOCUMENTATION/PROMPT_24_DOCUMENTATION_QUALITY.md` | Documentation quality | Quality review of generated documentation |

### Domain 7 - Validation (4 files)

| File | Purpose | Key Content |
|---|---|---|
| `DOMAIN_7_VALIDATION/PROMPT_25_VALIDATION_ENGINEERING.md` | Engineering validation | Technical accuracy verification |
| `DOMAIN_7_VALIDATION/PROMPT_26_VALIDATION_COVERAGE.md` | Coverage validation | Completeness audit; gap analysis |
| `DOMAIN_7_VALIDATION/PROMPT_27_CROSS_REFERENCE.md` | Cross-reference validation | Inter-phase consistency checking |
| `DOMAIN_7_VALIDATION/PROMPT_28_FINAL_REVIEW.md` | Final review | Sign-off; quality score; delivery |

## 11. Blackbox with Minimax M2.7

**File count:** 37 | **Status:** Complete | **Structure:** 18 root files + 7 modules + 6 templates + 3 standards + 3 checklists

### Infrastructure Files (8 files)

| File | Purpose | Key Content |
|---|---|---|
| `MASTER_PROMPT.md` | Primary orchestrator (v1.0.0) | Initialization sequence; 5-file load order; phase execution protocol |
| `MASTER_INDEX.md` | Framework index | File listing; structural organization |
| `MISSION.md` | Mission statement (v1.0.0) | Enable any AI agent to completely reverse engineer any repository |
| `OPERATING_RULES.md` | Agent behavior rules | Execution constraints; sequential discipline |
| `OUTPUT_RULES.md` | Output formatting | Documentation structure standards |
| `PROJECT_SPECIFICATION.md` | Specification | Framework scope; technical boundaries |
| `PROMPT_DESIGN_GUIDE.md` | Design philosophy (v1.0.0) | Prompt architecture; engineering decisions |
| `QUALITY_STANDARDS.md` | Quality metrics | Validation gates; quality dimensions |

### Execution Prompts (10 files)

| File | Purpose | Key Content |
|---|---|---|
| `PROMPT_01.md` | Phase 1: Repository init and discovery | Repository cataloging; initial profiling |
| `PROMPT_02.md` | Phase 2: Structural analysis and mapping | Organizational structure; module boundaries; file responsibilities |
| `PROMPT_03.md` | Phase 3: Dependency and relationship analysis | Internal/external dependencies; component relationships |
| `PROMPT_04.md` | Phase 4: Deep code analysis | File-level analysis; algorithms; code paths |
| `PROMPT_05.md` | Phase 5: Architecture reconstruction | Complete system architecture from analyzed code |
| `PROMPT_06.md` | Phase 6: Workflow and execution path analysis | Workflows; state transitions; event flows |
| `PROMPT_07.md` | Phase 7: Design pattern and decision analysis | Pattern identification; engineering decisions; code quality |
| `PROMPT_08.md` | Phase 8: AI workflow and agent analysis | AI-specific workflows; agent architectures; reasoning systems |
| `PROMPT_09.md` | Phase 9: Documentation synthesis | Comprehensive documentation assembly from all findings |
| `PROMPT_10.md` | Phase 10: Quality assurance and validation | Final validation against quality standards; completeness check |

### Modules (7 files)

| File | Purpose | Key Content |
|---|---|---|
| `modules/MODULE_ARCHITECTURE.md` | Architecture deep analysis module | Deep-dive for complex/unconventional architectures; multiple architectural styles |
| `modules/MODULE_AI_WORKFLOW.md` | AI workflow deep analysis module | Complex AI/agent system analysis; multi-agent patterns |
| `modules/MODULE_DATA_FLOW.md` | Data flow deep analysis module | Data flow and state management deep dive |
| `modules/MODULE_DEPENDENCY_GRAPH.md` | Dependency graph module | Advanced dependency graph construction and analysis |
| `modules/MODULE_DOCUMENTATION_GENERATION.md` | Documentation generation module | Advanced strategies for high-quality documentation |
| `modules/MODULE_QUALITY_VALIDATION.md` | Quality validation module | Advanced validation procedures for documentation output |
| `modules/MODULE_WORKFLOW_ANALYSIS.md` | Workflow deep analysis module | Complex workflow pattern analysis |

### Templates (6 files)

| File | Purpose | Key Content |
|---|---|---|
| `templates/TEMPLATE_API_DOC.md` | API documentation template | REST, GraphQL, gRPC documentation structure; Phase 9 usage |
| `templates/TEMPLATE_ARCHITECTURE_DOC.md` | Architecture documentation template | System architecture documentation structure |
| `templates/TEMPLATE_COMPONENT_DOC.md` | Component documentation template | Individual component documentation structure |
| `templates/TEMPLATE_REBUILD_GUIDE.md` | Rebuild guide template | Step-by-step rebuild instruction structure |
| `templates/TEMPLATE_SEQUENCE_DIAGRAM.md` | Sequence diagram template | Mermaid sequence diagram structure |
| `templates/TEMPLATE_WORKFLOW_DOC.md` | Workflow documentation template | Workflow and process documentation structure |

### Standards (3 files)

| File | Purpose | Key Content |
|---|---|---|
| `standards/STANDARDS_ARCHITECTURE.md` | Architecture documentation standards | Standard A1: architectural style identification; Phase 5/9 usage |
| `standards/STANDARDS_DOCUMENTATION.md` | Documentation standards | Standards for all generated documentation output |
| `standards/STANDARDS_REVERSE_ENGINEERING.md` | Reverse engineering methodology standards | Methodology standards for the RE process itself |

### Checklists (3 files)

| File | Purpose | Key Content |
|---|---|---|
| `checklists/CHECKLIST_ANALYSIS.md` | Analysis completeness checklist | Phase-by-phase (1-8) verification items; completeness criteria |
| `checklists/CHECKLIST_DOCUMENTATION.md` | Documentation completeness checklist | Verification of all generated documentation sections |
| `checklists/CHECKLIST_VALIDATION.md` | Validation and sign-off checklist | Final validation; sign-off criteria; delivery readiness |

## 12. Antigravity with Gemini 3.1 Pro high

**File count:** 14 | **Status:** Complete (example output) | **Structure:** This is NOT a framework variant. It is the output produced by running the framework against the Antigravity project using Gemini 3.1 Pro.

| File | Purpose | Key Content |
|---|---|---|
| `00-INDEX.md` | Documentation index | Table of contents for the complete analysis output |
| `01-repository-intelligence.md` | Phase 1 output: Repository intelligence | Folder tree; variant detection; tech stack; entry points; hypothesis |
| `02-file-folder-analysis/02-file-folder-analysis.md` | Phase 2 output: File/folder analysis | Per-layer file documentation (infrastructure, orchestration, execution) |
| `03-prompt-template-docs/03-prompt-template-docs.md` | Phase 3 output: Prompt template docs | Prompt-level documentation (analogous to function/class docs for code repos) |
| `04-architecture/system-design.md` | Phase 4 output: System design | Overall system architecture documentation |
| `04-architecture/component-map.md` | Phase 4 output: Component map | Component identification and relationship mapping |
| `04-architecture/module-map.md` | Phase 4 output: Module map | Module boundary and dependency documentation |
| `04-architecture/business-logic.md` | Phase 4 output: Business logic | Domain logic and business rules documentation |
| `04-architecture/working-logic.md` | Phase 4 output: Working logic | Operational logic and execution flow documentation |
| `05-diagrams.md` | Phase 5 output: Diagrams | Mermaid diagrams (architecture, flow, sequence) |
| `06-ai-agent-workflow.md` | Phase 6 output: AI agent workflow | Agent workflow analysis; execution pipeline documentation |
| `07-tech-stack.md` | Phase 7 output: Tech stack | Technology inventory and version documentation |
| `08-conditional-docs/08-conditional-docs.md` | Phase 8 output: Conditional docs | Domain-specific supplementary documentation |
| `09-developer-handbook-rebuild-guide.md` | Phase 9 output: Developer handbook | Rebuild guide; developer onboarding; reference handbook |

## 13. Excluded Items

| Item | Type | Reason for Exclusion |
|---|---|---|
| `.git/` | Directory | Git version control internals; not part of framework content |
| `.agents/` | Directory | Task management metadata; generated by automation tooling; not part of repository content |

## 14. File Count Verification

| Directory | File Count |
|---|---|
| Root level | 1 |
| Hermes With Deepseek v4 flash | 49 |
| Opencode With Deepseek v4 flash/Version 1 | 37 |
| Opencode With Deepseek v4 flash/Version 2 | 39 |
| Claude | 20 |
| GLM5.1 | 39 |
| Gemini With Gemini 3.1 Pro | 3 |
| Qwen | 15 |
| Mistral | 13 |
| Blackbox with Kimi K2.6 | 36 |
| Blackbox with Minimax M2.7 | 37 |
| Antigravity with Gemini 3.1 Pro high | 14 |
| **Total** | **303** |

All 303 files in the repository are accounted for in this document. No files have been silently omitted.
