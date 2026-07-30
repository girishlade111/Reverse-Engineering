# 05-diagrams

## Execution Sequence Diagram
This diagram illustrates the sequence of execution when an AI agent processes a target repository using the framework.

```mermaid
sequenceDiagram
    participant User
    participant LLM_Agent
    participant Framework
    participant Target_Repo

    User->>LLM_Agent: Paste MASTER_PROMPT.md
    LLM_Agent->>Framework: Read MISSION.md & Rules
    Framework-->>LLM_Agent: Load constraints into context
    LLM_Agent->>Framework: Read PROMPT_01
    LLM_Agent->>Target_Repo: Scan root & package files
    Target_Repo-->>LLM_Agent: Return folder tree & stack info
    LLM_Agent->>User: Output 01-repository-intelligence.md
    
    loop Phases 2 through 8
        LLM_Agent->>Framework: Read PROMPT_XX
        LLM_Agent->>Target_Repo: Analyze specific layer (Files/Arch/Tech)
        Target_Repo-->>LLM_Agent: Source code & config
        LLM_Agent->>User: Output XX-Phase-Docs.md
    end
    
    LLM_Agent->>Framework: Read PROMPT_09
    LLM_Agent->>User: Output 09-developer-handbook-rebuild-guide.md
    
    LLM_Agent->>Framework: Read PROMPT_10
    LLM_Agent->>User: Output Final Validation Report
```
*Caption: The sequence of interactions between the user, the LLM agent, the framework rules, and the target codebase being analyzed.*

## Framework Component Diagram
This diagram illustrates the logical separation of concerns within the prompt framework itself.

```mermaid
graph TD
    subgraph Layer_1_Infrastructure [Layer 1: Infrastructure]
        MISSION[MISSION.md]
        RULES[OPERATING_RULES.md]
        QUALITY[QUALITY_STANDARDS.md]
        INDEX[MASTER_INDEX.md]
    end

    subgraph Layer_2_Orchestration [Layer 2: Orchestration]
        MASTER[MASTER_PROMPT.md]
    end

    subgraph Layer_3_Execution [Layer 3: Execution Prompts]
        P1[PROMPT_01: Intelligence]
        P2[PROMPT_02: Files & Folders]
        P3[PROMPT_03: Unit Docs]
        P4[PROMPT_04: Architecture]
        PX[PROMPT_XX: ...]
    end
    
    MASTER --> |Loads into context| Layer_1_Infrastructure
    MASTER --> |Triggers| Layer_3_Execution
    P1 --> P2
    P2 --> P3
    P3 --> P4
    P4 --> PX
```
*Caption: The three-layer architecture of the prompt framework, showing how orchestration loads infrastructure rules before triggering the sequential execution pipeline.*

*Note: ER Diagram, Call Graph, and UML Class Diagrams are N/A as there is no traditional runtime source code or database in this repository.*
