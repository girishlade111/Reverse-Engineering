# GLOSSARY OF TERMS

## FRAMEWORK: Enterprise Reverse Engineering Prompt Framework

---

## A

**Accuracy Tier**
Classification for documentation claims: A (direct code evidence), B (strong inference), C (weak inference), D (speculation/unknown).

**ADR (Architecture Decision Record)**
A document that captures an important architectural decision made during system development, including context, decision, consequences, and alternatives.

**Anti-Pattern**
A commonly used but ineffective or counterproductive solution to a recurring problem. Documented during Phase 12.

**API Boundary**
The interface surface where one service or module communicates with another. Documented in Phase 13.

**Architectural Style**
The overall structural approach of the system (layered, hexagonal, microservices, event-driven, etc.). Identified in Phase 07.

**AST (Abstract Syntax Tree)**
A tree representation of source code structure used for programmatic analysis.

**Async Chain**
A sequence of asynchronous operations linked by promises, callbacks, or async/await. Traced in Phase 09.

## B

**Blast Radius**
The scope of impact when a particular module or component fails or needs modification.

**Build System**
The tooling used to compile, bundle, or prepare the code for execution (e.g., Webpack, esbuild, Gradle, Make). Analyzed in Phase 02.

**Bounded Context**
In Domain-Driven Design, a logical boundary around a domain model. Used in Phase 07 for architecture description.

**Business Rule**
A rule that encodes business policy or logic, extracted from code during Phase 11.

## C

**Call Graph**
A directed graph showing which functions call which other functions. Built in Phase 09.

**CI/CD (Continuous Integration / Continuous Deployment)**
Automated pipelines for testing and deploying code changes. Analyzed in Phase 02.

**Circuit Breaker**
A reliability pattern that prevents cascading failures by detecting when a downstream service is unhealthy and stopping requests to it. Documented in Phase 15.

**Cohesion**
A measure of how strongly related the responsibilities of a module are (high cohesion = good). Assessed in Phase 05.

**Component**
A distinct part of the system with a specific responsibility. Cataloged in Phase 07.

**Configuration Schema**
The complete definition of all configurable parameters for the system. Documented in Phase 17.

**Coupling**
A measure of how dependent one module is on another (loose coupling = good). Assessed in Phase 05.

**Cross-Reference**
A mapping between related artifacts across different phases (e.g., feature X → files implementing X). Produced in Phase 18.

## D

**Data Flow**
The path data takes from entry point through transformations to exit point. Traced in Phase 08.

**Dead Code**
Code that is never executed. Identified during Phase 06 and Phase 09.

**Dependency Graph**
A graph showing how modules, components, or packages depend on each other. Built in Phase 03 and Phase 05.

**Design Pattern**
A reusable solution to a commonly occurring problem within a given context. Identified in Phase 12.

## E

**Entry Point**
A location where the system begins execution (e.g., main function, route handler, event handler, cron job). Identified in Phase 00.

**Error Propagation**
The path an error follows from its origin to its handler. Mapped in Phase 09.

**Error Tier**
Classification of error handling: G (global handler) / L (local handler) / N (not handled). Used in Phase 15.

**Event Bus**
A system for publishing and subscribing to events within an application. Documented in Phase 14.

## F

**Feature Flag**
A configuration mechanism that enables or disables functionality at runtime without code deployment. Documented in Phase 10 and Phase 17.

**Feature Map**
A diagram showing the relationship between features and their implementing files. Generated in Phase 10.

**File:Line Reference**
A precise reference to source code using format `path/to/file.ts:line_number`.

## G

**GAP-ID**
A unique identifier for gaps in understanding or coverage during the reverse engineering process. Format: `GAP-###`.

**Gap Analysis**
A systematic identification of missing information or incomplete understanding. Produced in Phase 19.

**Graceful Degradation**
A system's ability to maintain partial functionality when some components fail. Documented in Phase 15.

## H

**Hot Path**
A code path that executes frequently and is critical for performance.

**Hot Function**
A function that is called very frequently or has high computational cost.

## I

**ICP (Ideal Customer Profile)**
In product context, the description of the target user or customer for the software.

**Integration Points**
Locations where the system connects to external systems or services. Cataloged in Phase 13.

**Isomorphic Pattern**
A pattern that appears in multiple locations of the codebase with similar structure but different implementations.

## L

**Layer**
A horizontal slice of the architecture with a specific level of abstraction. Identified in Phase 07.

## M

**Magic Value**
A hardcoded numeric or string constant with unclear meaning. Flagged in Phase 17.

**Middleware**
Code that intercepts requests or responses to perform cross-cutting concerns (auth, logging, rate limiting). Documented in Phase 09 and Phase 13.

**Module**
A logical grouping of related code with a well-defined boundary. Cataloged in Phase 05.

**Monorepo**
A repository containing multiple independently-deployable projects. Identified in Phase 01.

## O

**ORM (Object-Relational Mapper)**
A library that maps database tables to programming language objects.

## P

**Phase**
One of 20 sequential stages in the reverse engineering framework, each with specific goals and outputs.

**Priority (File)**
Classification of files by analysis depth: Priority 1 (full read), Priority 2 (key sections), Priority 3 (scan only).

**Pseudocode**
A plain language description of an algorithm's logic, generated during Phase 11.

## Q

**Quality Score**
A numeric rating (0-100%) for a phase's output, calculated from completeness, accuracy, clarity, and consistency metrics.

## R

**Rebuild Guide**
Documentation that contains sufficient information to rebuild the system from scratch. Produced in Phase 18.

**Retry Strategy**
The policy for retrying failed operations, including max attempts, backoff algorithm, and retryable error classification. Documented in Phase 15.

## S

**SBOM (Software Bill of Materials)**
A complete inventory of software components and dependencies. Produced in Phase 03.

**Side Effect**
A function's interaction with state outside its scope (e.g., modifying a global variable, writing to a database).

**State Machine**
A model of computation consisting of states, transitions, and actions. Documented in Phase 14.

## T

**Tech Stack**
The complete set of technologies used by the system: languages, frameworks, databases, services, etc. Documented in Phase 04.

**Template (T-number)**
A standardized document format for consistent reverse engineering output. Referenced as T1, T2, etc. See TEMPLATES.md.

**Third-Party Integration**
A connection to an external service or API not owned by the system. Documented in Phase 13.

## V

**Validation Layer**
Code that checks data correctness before processing. Identified in Phase 08.

**Violation Log**
A record of output rules that were violated during the reverse engineering process. Generated during each phase.

## W

**Worker**
A background process that performs work asynchronously, often triggered by events or scheduled intervals.

---

*This glossary should be extended with domain-specific terms found in the target repository during reverse engineering.*
