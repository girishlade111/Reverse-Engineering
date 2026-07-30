# Phase 5: Diagrams

> Mermaid diagrams derived from the [Phase 4 Architecture Reconstruction](./04-architecture/system-design.md). Each diagram visualizes a distinct architectural concern that cannot be captured in prose alone.

---

## 1. Execution Flow Sequence Diagram

This sequence diagram traces the full lifecycle of a single framework invocation, from operator initialization through all 9 phases to final delivery. It shows how the Orchestrator mediates every interaction between infrastructure rules, execution prompts, and the target repository, compressing context at each phase boundary via the Context Summary mechanism.

```mermaid
sequenceDiagram
    participant Op as Operator
    participant L2 as MASTER_PROMPT (Orchestrator)
    participant L1 as Infrastructure Files
    participant L3 as Execution Prompts
    participant Repo as Target Repository
    participant Disk as File System (Disk)

    Op->>L2: Load MASTER_PROMPT.md + repository path
    L2->>L1: Load MISSION, OPERATING_RULES, QUALITY_STANDARDS
    L2->>L1: Load OUTPUT_RULES, PROMPT_DEPENDENCY_MAP, VALIDATION_CHECKLISTS
    L2->>L2: Internalize constraints (rules engine loaded)

    rect rgb(230, 245, 255)
    Note over L2,Repo: Phase 1 - Discovery (P01, P02, P03)
    L2->>L3: Load P01 (Repository Scan)
    L3->>Repo: Read root structure, config files
    L3->>Disk: Write 01_scan_notes.md
    L3-->>L2: Output + quality gate status
    L2->>L3: Load P02, P03 (parallel)
    L3->>Repo: File inventory + tech stack detection
    L3->>Disk: Write phase outputs
    L3-->>L2: Outputs + gate status
    L2->>L2: Generate Context Summary (Phase 1)
    end

    rect rgb(230, 255, 230)
    Note over L2,Repo: Phase 2 - Structural Analysis (P04, P05, P06)
    L2->>L3: Load P04
    L3->>Repo: Map folder architecture
    L2->>L3: Load P05, P06 (parallel)
    L3->>Repo: Module deps + entry points
    L3-->>L2: Outputs + gate status
    L2->>L2: Generate Context Summary (Phase 2)
    end

    rect rgb(255, 245, 230)
    Note over L2,Repo: Phase 3 - Architecture Reconstruction (P07-P10)
    L2->>L3: Load P07 then P08, P09, P10 (parallel)
    L3->>Repo: Architecture style, components, layers, patterns
    L3-->>L2: Outputs + gate status
    L2->>L2: Generate Context Summary (Phase 3)
    end

    rect rgb(255, 230, 230)
    Note over L2,Repo: Phase 4 - Deep Code Analysis (P11-P15)
    L2->>L3: Load P11 then P12, P13, then P14, P15
    L3->>Repo: Data flow, exec paths, state, errors, concurrency
    L3-->>L2: Outputs + gate status
    L2->>L2: Generate Context Summary (Phase 4)
    end

    alt AI patterns detected in Phase 3
        rect rgb(255, 255, 210)
        Note over L2,Repo: Phase 5 - AI/Automation Analysis (P16-P20)
        L2->>L3: Load P16, P17 (parallel)
        L3->>Repo: Prompt architecture + agent workflow
        L2->>L3: Load P18 then P19, P20
        L3->>Repo: Tools, planning, memory/RAG
        L3-->>L2: Outputs + gate status
        L2->>L2: Generate Context Summary (Phase 5)
        end
    else No AI patterns
        L2->>L2: Skip Phase 5, carry Phase 4 context forward
    end

    rect rgb(240, 230, 255)
    Note over L2,Repo: Phase 6 - Integration and Boundaries (P21-P24)
    L2->>L3: Load P21, P22 (parallel) then P23, P24
    L3->>Repo: APIs, external services, events, config
    L3-->>L2: Outputs + gate status
    L2->>L2: Generate Context Summary (Phase 6)
    end

    rect rgb(230, 255, 245)
    Note over L2,Repo: Phase 7 - Documentation Generation (P25-P30)
    L2->>L3: Load P25 then P26, P27, P28 (parallel) then P29, P30
    L3->>Disk: Write Architecture/Developer Handbooks, Rebuild Guide, Diagrams
    L3-->>L2: Outputs + gate status
    L2->>L2: Generate Context Summary (Phase 7)
    end

    rect rgb(245, 245, 245)
    Note over L2,Repo: Phase 8 - Validation and Quality (P31-P34)
    L2->>L3: Load P31, P32, P33 (parallel) then P34
    L3->>Disk: Read all prior outputs for cross-validation
    L3-->>L2: Final quality gate score
    L2->>L2: Generate Context Summary (Phase 8)
    end

    alt Rebuild requested AND Phase 7 passed
        rect rgb(210, 255, 255)
        Note over L2,Repo: Phase 9 - Rebuild Package (P35-P36)
        L2->>L3: Load P35 then P36
        L3->>Disk: Assemble rebuild artifacts, verify completeness
        L3-->>L2: Verification report
        end
    end

    L2->>Op: Deliver complete documentation set
```

The diagram reveals three key architectural properties: (1) the Orchestrator never accesses the target repository directly, always delegating to execution prompts; (2) context is compressed at every phase boundary, preventing window overflow; (3) conditional branches at Phase 5 and Phase 9 are the only non-linear control flow in the pipeline.

---

## 2. Three-Layer Architecture Component Diagram

This component diagram shows the structural separation between Infrastructure (configuration invariants), Orchestration (control plane), and Execution (data plane). Arrows indicate the direction of dependency and data flow, demonstrating that Layer 3 components never bypass Layer 2 to access Layer 1 directly at runtime.

```mermaid
graph TB
    subgraph L1["Layer 1: Infrastructure (Read-Only Configuration)"]
        direction LR
        MISSION["MISSION.md<br/>6 principles, 12 success criteria"]
        RULES["OPERATING_RULES.md<br/>12 binding rules"]
        QUALITY["QUALITY_STANDARDS.md<br/>Q1-Q10 metrics"]
        OUTPUT["OUTPUT_RULES.md<br/>7 format sections"]
        VALID["VALIDATION_CHECKLISTS.md<br/>9 phase checklists"]
        DEPMAP["PROMPT_DEPENDENCY_MAP.md<br/>DAG + parallelization"]
        GLOSSARY["GLOSSARY.md<br/>Term definitions"]
        DESIGN["FRAMEWORK_DESIGN_PHILOSOPHY.md<br/>Design rationale"]
        PGUIDE["PROMPT_DESIGN_GUIDE.md<br/>Authoring conventions"]
        TEMPLATES["DIAGRAM_TEMPLATES.md<br/>Mermaid templates"]
        INDEX["MASTER_INDEX.md<br/>File navigation"]
        SPEC["PROJECT_SPECIFICATION.md<br/>Target context"]
    end

    subgraph L2["Layer 2: Orchestration (Control Plane)"]
        MASTER["MASTER_PROMPT.md<br/>Sequencing + Context Management<br/>+ Quality Enforcement"]
    end

    subgraph L3["Layer 3: Execution (Data Plane - 36 Prompts)"]
        direction LR
        PH1["Phase 1<br/>P01-P03<br/>Discovery"]
        PH2["Phase 2<br/>P04-P06<br/>Structure"]
        PH3["Phase 3<br/>P07-P10<br/>Architecture"]
        PH4["Phase 4<br/>P11-P15<br/>Deep Analysis"]
        PH5["Phase 5<br/>P16-P20<br/>AI/Automation"]
        PH6["Phase 6<br/>P21-P24<br/>Integration"]
        PH7["Phase 7<br/>P25-P30<br/>Documentation"]
        PH8["Phase 8<br/>P31-P34<br/>Validation"]
        PH9["Phase 9<br/>P35-P36<br/>Rebuild"]
    end

    MASTER -->|"loads at init"| MISSION
    MASTER -->|"loads at init"| RULES
    MASTER -->|"loads at init"| QUALITY
    MASTER -->|"loads at init"| OUTPUT
    MASTER -->|"loads at init"| VALID
    MASTER -->|"loads at init"| DEPMAP

    MASTER -->|"sequences + enforces gates"| PH1
    MASTER -->|"sequences + enforces gates"| PH2
    MASTER -->|"sequences + enforces gates"| PH3
    MASTER -->|"sequences + enforces gates"| PH4
    MASTER -->|"sequences + enforces gates"| PH5
    MASTER -->|"sequences + enforces gates"| PH6
    MASTER -->|"sequences + enforces gates"| PH7
    MASTER -->|"sequences + enforces gates"| PH8
    MASTER -->|"sequences + enforces gates"| PH9

    PH1 -->|"Context Summary"| PH2
    PH2 -->|"Context Summary"| PH3
    PH3 -->|"Context Summary"| PH4
    PH4 -->|"Context Summary"| PH5
    PH4 -->|"Context Summary"| PH6
    PH5 -->|"Context Summary"| PH6
    PH6 -->|"Context Summary"| PH7
    PH7 -->|"Context Summary"| PH8
    PH8 -->|"Context Summary"| PH9
```

The 12 infrastructure files act as compile-time constants: loaded once, never mutated. The Orchestrator is the sole runtime authority that decides sequencing, gating, and conditional branching. Execution prompts consume context summaries (not raw prior outputs) to stay within token budgets.

---

## 3. Conditional Branching State Diagram

This state diagram models the two decision points that break the otherwise linear pipeline: the Phase 5 conditional (triggered by AI pattern detection) and the Phase 8 subsection skip logic (each validation prompt may produce a "no findings" result that causes downstream remediation to be skipped). These are the only points where the framework's execution path diverges.

```mermaid
stateDiagram-v2
    [*] --> Initialization
    Initialization --> Phase1: Load infrastructure files
    Phase1 --> Phase2: Context Summary generated
    Phase2 --> Phase3: Context Summary generated
    Phase3 --> Phase4: Context Summary generated

    Phase4 --> AIDetectionCheck: Phase 4 complete

    state AIDetectionCheck <<choice>>
    AIDetectionCheck --> Phase5: AI patterns found
    AIDetectionCheck --> Phase6: No AI patterns

    Phase5 --> Phase6: Context Summary generated

    Phase6 --> Phase7: Context Summary generated
    Phase7 --> Phase8: Context Summary generated

    state Phase8 {
        [*] --> P31_Accuracy
        [*] --> P32_Completeness
        [*] --> P33_Consistency

        P31_Accuracy --> P34_FinalGate: findings reported
        P32_Completeness --> P34_FinalGate: findings reported
        P33_Consistency --> P34_FinalGate: findings reported

        state P34_FinalGate <<choice>>
        P34_FinalGate --> GatePassed: All Q1-Q8 above threshold
        P34_FinalGate --> RemediationLoop: Below threshold
        RemediationLoop --> P34_FinalGate: Re-validate
        GatePassed --> [*]
    }

    Phase8 --> RebuildCheck: Validation complete

    state RebuildCheck <<choice>>
    RebuildCheck --> Phase9: Rebuild requested
    RebuildCheck --> Complete: Not requested

    Phase9 --> Complete: Rebuild verified
    Complete --> [*]
```

The state diagram makes explicit that Phase 5 is guarded by a runtime detection check (not a static configuration flag), while Phase 9 depends on both a user request and successful Phase 7 completion. The Phase 8 remediation loop can cycle indefinitely until thresholds are met or the gap is documented.

---

## 4. Framework Variant Comparison Diagram

This diagram compares the structural differences between the two framework variants present in the repository: the Hermes canonical variant (with DeepSeek) and the Antigravity variant (with Gemini). It highlights divergent prompt counts, phase numbering, and output structures while showing shared architectural principles.

```mermaid
graph LR
    subgraph Shared["Shared Architecture"]
        direction TB
        S1["Three-Layer Model"]
        S2["Context Summary Handoffs"]
        S3["Quality Gate Enforcement"]
        S4["Conditional Phase 5"]
        S5["DAG-Based Sequencing"]
    end

    subgraph Hermes["Hermes (DeepSeek v4 Flash)"]
        direction TB
        H1["36 execution prompts"]
        H2["9 phases"]
        H3["13 infrastructure files"]
        H4["Explicit PROMPT_DEPENDENCY_MAP"]
        H5["Phase 9: Rebuild Package"]
        H6["8 parallelization batches"]
    end

    subgraph Antigravity["Antigravity (Gemini 3.1 Pro)"]
        direction TB
        A1["35 execution prompts"]
        A2["9 phases"]
        A3["Fewer infrastructure files"]
        A4["Implicit dependency ordering"]
        A5["Phase 9: Rebuild Package"]
        A6["Similar parallelization"]
    end

    Shared --- Hermes
    Shared --- Antigravity

    Hermes ---|"Differences"| DiffH["More explicit infrastructure<br/>Formal DAG document<br/>36 vs 35 prompts<br/>Detailed validation checklists"]
    Antigravity ---|"Differences"| DiffA["Leaner infrastructure set<br/>Ordering via convention<br/>35 prompts<br/>Integrated validation"]
```

Both variants implement identical architectural principles (three-layer separation, context compression, conditional AI analysis) but differ in configuration granularity. Hermes favors explicit formal specification (13 infrastructure files, explicit DAG), while Antigravity favors convention over configuration with fewer governance documents.

---

## 5. Diagrams Not Applicable

### ER Diagram (Entity-Relationship)

**Status:** Not applicable.

**Reason:** This repository contains no database, no persistent data store, and no entity relationships. All data is ephemeral (held in LLM context) or serialized as Markdown files on disk. There are no tables, foreign keys, or relational constraints to diagram.

### UML Class Diagram

**Status:** Not applicable.

**Reason:** This repository contains no classes, no object-oriented inheritance hierarchies, and no typed interfaces. The "components" are Markdown files consumed as text by an LLM. There is no type system, no polymorphism, and no method dispatch to represent in UML class notation.

---

## Cross-References

- [System Design](./04-architecture/system-design.md) - Three-Layer Architecture prose description
- [Module Map](./04-architecture/module-map.md) - Dependency DAG in tabular form
- [Working Logic](./04-architecture/working-logic.md) - Execution trace with inline flowcharts
- [Business Logic](./04-architecture/business-logic.md) - Rules governing conditional branches
