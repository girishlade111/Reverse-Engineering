# PROJECT SPECIFICATION — Enterprise Reverse Engineering Prompt Framework

## PROJECT ID: RE-PROMPT-FRAMEWORK
## VERSION: 1.0
## AUTHOR: Enterprise Prompt Engineering Division
## STATUS: Production Ready

---

## 1. ABSTRACT

This project defines a comprehensive, reusable, modular prompt framework for AI-powered reverse engineering of software repositories. The framework consists of 32+ interconnected Markdown files that together define a complete methodology for understanding, analyzing, and documenting any software system from source code alone.

## 2. PROBLEM STATEMENT

AI coding agents lack a standardized methodology for reverse engineering software repositories. Without a structured framework, agents:

- Produce incomplete or shallow analysis
- Miss critical architectural insights
- Generate inconsistent documentation
- Fail to trace dependencies and data flows
- Cannot validate their own understanding
- Produce non-reusable outputs

This framework solves these problems by providing a rigorous, phase-gated methodology.

## 3. DESIGN OBJECTIVES

| OBJECTIVE | DESCRIPTION | MEASUREMENT |
|-----------|-------------|-------------|
| Completeness | Every aspect of the repository is analyzed | 200+ item checklist |
| Accuracy | Every claim is evidence-backed | Accuracy tier system (A-D) |
| Scalability | Works for repos of any size | Modular, phase-gated approach |
| Reusability | Framework works for any tech stack | Language-agnostic design |
| Maintainability | Outputs are easy to update | Structured directory, cross-references |
| Verifiability | Analysis can be validated | Validation gates at every phase |

## 4. SCOPE

### In Scope
- Source code analysis (all languages)
- Build system analysis
- Dependency analysis
- Architecture reconstruction
- Data flow analysis
- Control flow analysis
- API documentation
- Design pattern identification
- AI workflow analysis (prompts, agents, RAG)
- Configuration analysis
- Documentation generation
- Validation and quality assurance

### Out of Scope
- Runtime analysis (requires execution)
- Network traffic analysis
- Binary decompilation
- Database content analysis (schema only)
- User behavior analysis
- Performance benchmarking

## 5. FRAMEWORK ARCHITECTURE

```
┌─────────────────────────────────────────────────────────────┐
│                    MASTER PROMPT                             │
│         (Orchestration & Entry Point)                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              CORE FRAMEWORK FILES                    │    │
│  │  Mission │ Rules │ Quality │ Output │ Spec │ Design  │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              PHASE PROMPTS (01-20)                   │    │
│  │  Scouting → Structure → Build → Deps → Tech → ...  │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │             SUPPORTING ARTIFACTS                     │    │
│  │  Checklist │ Templates │ Diagram Guide │ Glossary   │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## 6. PHASE ARCHITECTURE

Each phase follows a consistent internal architecture:

```
┌──────────────────────────────┐
│   1. OBJECTIVE               │  What this phase achieves
├──────────────────────────────┤
│   2. PREREQUISITES           │  What must be completed before
├──────────────────────────────┤
│   3. INPUTS                  │  Files and data this phase consumes
├──────────────────────────────┤
│   4. ANALYSIS STEPS          │  Step-by-step instructions
├──────────────────────────────┤
│   5. OUTPUT SPECIFICATION    │  Exact output format
├──────────────────────────────┤
│   6. DIAGRAMS                │  Required diagrams (if any)
├──────────────────────────────┤
│   7. VALIDATION CHECKS       │  Self-validation before proceeding
├──────────────────────────────┤
│   8. CHECKLIST               │  Completion checklist
└──────────────────────────────┘
```

## 7. TARGET AUDIENCE

This framework is designed for:

- **AI Coding Agents**: Primary executor of the framework
- **Software Engineers**: Consumers of the generated documentation
- **Architects**: Reviewers of reconstructed architecture
- **Technical Writers**: Editors of generated documentation
- **Engineering Managers**: Oversight of reverse engineering projects
- **Security Auditors**: Users of analyzed code structure
- **AI/ML Engineers**: Understanding AI workflows in analyzed repos

## 8. SUCCESS CRITERIA

The framework is successful if:

1. An AI agent following the framework produces complete documentation for any repository
2. The documentation enables a developer to rebuild the system without reading source code
3. The framework works for repositories of any size (100 to 100,000+ files)
4. The framework works for any technology stack
5. The framework produces consistent results across different AI agent implementations
6. All generated documentation passes the quality audit standards

## 9. LIMITATIONS

- The framework cannot analyze runtime behavior (only static code)
- The framework cannot execute tests or observe runtime output
- The framework's accuracy depends on the AI agent's reading comprehension
- For obfuscated or minified code, analysis depth is reduced
- For generated code, analysis quality depends on source map availability

---

*This specification defines the contract for the Enterprise Reverse Engineering Prompt Framework.*
