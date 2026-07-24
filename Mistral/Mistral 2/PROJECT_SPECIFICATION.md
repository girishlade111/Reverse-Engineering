\# PROJECT SPECIFICATION: REVERSE ENGINEERING PROMPT FRAMEWORK



\## 1. OVERVIEW



\### 1.1 Purpose

This specification defines the complete requirements for a world-class Reverse Engineering Prompt Framework capable of guiding any AI coding agent to fully reverse engineer any software repository.



\### 1.2 Scope

The framework must handle:

\- Any programming language

\- Any framework or library

\- Any architecture pattern

\- Any repository size (1 file to 100,000+ files)

\- Any complexity level

\- Any technology stack



\### 1.3 Audience

\- AI coding agents performing reverse engineering

\- Human engineers reviewing AI-generated documentation

\- Technical leads validating system understanding

\- Architects reconstructing system designs



\## 2. CORE REQUIREMENTS



\### 2.1 Completeness Requirements

The framework MUST ensure:

\- Every file is analyzed

\- Every folder structure is understood

\- Every class is documented

\- Every function is explained

\- Every interface is defined

\- Every type is described

\- Every dependency is mapped

\- Every relationship is identified

\- Every execution path is traced

\- Every state transition is captured

\- Every error handling path is documented

\- Every configuration option is explained

\- Every design pattern is identified

\- Every engineering decision is rationale-explained



\### 2.2 Accuracy Requirements

The framework MUST enforce:

\- Source code is the single source of truth

\- No assumptions without verification

\- No undocumented behavior

\- No missed edge cases

\- No incorrect relationships

\- No misrepresented logic

\- No omitted dependencies



\### 2.3 Quality Requirements

The framework MUST produce:

\- Production-grade documentation

\- Architecturally sound diagrams

\- Technically precise descriptions

\- Consistently formatted outputs

\- Maintainable documentation structure

\- Scalable analysis approach



\## 3. TECHNICAL REQUIREMENTS



\### 3.1 Analysis Depth (10 Levels)

1\. \*\*Repository Level\*\*: Overall structure, metadata, statistics

2\. \*\*Folder Level\*\*: Directory architecture, organization patterns

3\. \*\*File Level\*\*: File purposes, relationships, contents

4\. \*\*Module Level\*\*: Module boundaries, responsibilities, interactions

5\. \*\*Class Level\*\*: Class hierarchies, responsibilities, collaborations

6\. \*\*Function Level\*\*: Function logic, parameters, return values, side effects

7\. \*\*Code Block Level\*\*: Logic blocks, control flow, data transformations

8\. \*\*Line Level\*\*: Individual line explanations (when critical)

9\. \*\*Dependency Level\*\*: External and internal dependency analysis

10\. \*\*Integration Level\*\*: System integration points and workflows



\### 3.2 Documentation Types Required



\*\*Architecture Documentation:\*\*

\- System Architecture Diagram

\- Component Architecture Diagram

\- Module Architecture Diagram

\- Folder Architecture Diagram

\- Deployment Architecture Diagram

\- Data Flow Architecture Diagram



\*\*Structural Documentation:\*\*

\- Complete Folder Tree

\- Module Map

\- Feature Map

\- Component Graph

\- Dependency Graph

\- Call Graph

\- Import/Export Graph



\*\*Behavioral Documentation:\*\*

\- Execution Flow Diagrams

\- Sequence Diagrams

\- State Machine Diagrams

\- Event Flow Diagrams

\- Error Handling Flows

\- Retry Strategy Documentation

\- Caching Strategy Documentation



\*\*Code Documentation:\*\*

\- File Responsibility Documentation

\- Class Documentation (with UML)

\- Interface Documentation

\- Function Documentation

\- Type Documentation

\- Constant Documentation

\- Utility Documentation



\*\*Integration Documentation:\*\*

\- API Documentation

\- Service Documentation

\- Middleware Documentation

\- Plugin Documentation

\- Third-Party Integration Documentation



\*\*AI-Specific Documentation:\*\*

\- Prompt Architecture

\- Agent Architecture

\- Reasoning Process Documentation

\- Planning Pipeline Documentation

\- Tool Integration Documentation

\- Memory Workflow Documentation

\- RAG Workflow Documentation

\- Search Workflow Documentation



\*\*Operational Documentation:\*\*

\- Build System Documentation

\- Package Analysis

\- Configuration Documentation

\- Environment Variables Documentation

\- Deployment Documentation

\- Development Setup Guide



\*\*Reference Documentation:\*\*

\- Developer Handbook

\- Architecture Handbook

\- Rebuild Guide

\- Engineering Notes

\- Cross References

\- Validation Checklists



\## 4. NON-FUNCTIONAL REQUIREMENTS



\- \*\*Performance\*\*: Analysis must complete within reasonable time for repository size

\- \*\*Scalability\*\*: Must handle repositories with 100,000+ files

\- \*\*Maintainability\*\*: Framework must be version-controlled, modular, extensible

\- \*\*Compatibility\*\*: Must work with any AI model capable of code analysis



\## 5. ACCEPTANCE CRITERIA



The framework is ACCEPTED when:

\- It can successfully reverse engineer 3 different repositories of varying complexity

\- All generated documentation is technically accurate

\- All diagrams are correct and complete

\- All relationships are properly mapped

\- All execution paths can be traced

\- The documentation can be used to rebuild the system

\- Human engineers can understand the system from the documentation alone

