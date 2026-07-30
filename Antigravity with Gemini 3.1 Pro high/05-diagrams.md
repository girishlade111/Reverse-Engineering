# 05-diagrams

## Context Management Lifecycle (Sequence Diagram)
This diagram illustrates the crucial 7-step loop dictated by `MASTER_PROMPT.md` to prevent context window overflow when orchestrating the 35 execution prompts.

```mermaid
sequenceDiagram
    participant Orchestrator as MASTER_PROMPT
    participant Agent as LLM Agent
    participant Target as Target Repo
    participant Output as File System

    Orchestrator->>Agent: Step 1: Load PROMPT_XX
    Orchestrator->>Agent: Step 2: Read constraints
    Agent->>Output: Step 3: Verify prerequisites (read previous Phase Output)
    
    Agent->>Target: Step 4: Execute analysis on Codebase
    Target-->>Agent: Code context
    
    Agent->>Agent: Step 5: Validate output against QUALITY_STANDARDS.md
    
    alt Fails Quality Gate
        Agent->>Target: Re-examine codebase
        Agent->>Agent: Attempt remediation
    end
    
    Agent->>Output: Write XX-Phase-Docs.md (Detailed Analysis)
    Agent->>Agent: Step 6: Generate Context Summary (Memory compression)
    
    Agent->>Orchestrator: Step 7: Proceed to PROMPT_XX+1 (carrying only Context Summary)
```
*Caption: The execution loop that compresses detailed analysis into a "Context Summary" before transitioning to the next phase. This is the core architectural innovation of the Hermes canonical framework that allows it to process large repositories without hallucinating.*

## Framework Component Diagram
This diagram illustrates the logical separation of concerns within the prompt framework itself.

```mermaid
graph TD
    subgraph Layer_1_Infrastructure [Layer 1: Infrastructure]
        MISSION[MISSION.md]
        RULES[OPERATING_RULES.md]
        QUALITY[QUALITY_STANDARDS.md]
        INDEX[MASTER_INDEX.md]
        DAG[PROMPT_DEPENDENCY_MAP.md]
        QA[VALIDATION_CHECKLISTS.md]
    end

    subgraph Layer_2_Orchestration [Layer 2: Orchestration]
        MASTER[MASTER_PROMPT.md]
    end

    subgraph Layer_3_Execution [Layer 3: 35 Execution Prompts]
        P1[Phase 1: Prompts 01-03]
        P2[Phase 2: Prompts 04-06]
        P3[Phase 3: Prompts 07-10]
        P4[Phase 4: Prompts 11-15]
        P5[Phase 5: Prompts 16-20]
        PX[Phases 6-9: Prompts 21-35]
    end
    
    MASTER --> |Loads into context| Layer_1_Infrastructure
    MASTER --> |Triggers Sequence| Layer_3_Execution
    P1 --> |Context Summary| P2
    P2 --> |Context Summary| P3
    P3 --> |Context Summary| P4
    P4 --> |Context Summary| P5
    P5 --> |Context Summary| PX
```
*Caption: The three-layer architecture of the prompt framework. Note how data is passed between the 35 execution prompts via the Context Summary mechanism.*
