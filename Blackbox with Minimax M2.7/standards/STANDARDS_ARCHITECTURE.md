# Architecture Documentation Standards

> **Document:** standards/STANDARDS_ARCHITECTURE.md  
> **Version:** 1.0.0  
> **Purpose:** Define standards for documenting software architecture  
> **When to Use:** During Phase 5 and Phase 9 when documenting architecture

---

## 📐 STANDARD A1: ARCHITECTURAL STYLE IDENTIFICATION

### A1.1 Style Classification
Architecture must be classified using standardized terminology:

| Term | Definition |
|------|------------|
| Layered Architecture | Organized into horizontal layers (presentation, business, data) |
| Microservices | Independent, deployable services communicating via network |
| Event-Driven | Components communicate via events and event handlers |
| Microkernel | Core system with pluggable extension modules |
| Hexagonal | Core domain isolated from infrastructure via ports/adapters |
| Clean Architecture | Dependency inversion with use-case-centric organization |
| CQRS | Separated read and write models |
| Event Sourcing | State derived from event log |
| Serverless | Function-based, event-triggered, auto-scaling |
| Pipe-and-Filter | Processing pipeline with composable filters |

### A1.2 Evidence Requirements
For each identified style, provide:
1. At least 3 pieces of code evidence supporting the classification.
2. Code examples showing the pattern.
3. File paths where the pattern is visible.
4. Confidence level in the classification.

---

## 📐 STANDARD A2: DIAGRAM STANDARDS

### A2.1 Diagram Types

| Type | When Required | Standard |
|------|---------------|----------|
| System Context | Always | Show system boundaries and external actors |
| Container/Component | Always | Show major components and their interactions |
| Deployment | If distributed | Show physical/virtual deployment topology |
| Sequence | For key interactions | Show time-ordered message exchange |
| State | For stateful components | Show states, transitions, triggers |

### A2.2 Diagram Notation
All diagrams must use Mermaid.js syntax.

**Node naming conventions:**
- **Systems:** UPPERCASE (USER, DATABASE, EXTERNAL_API)
- **Containers:** Title Case (API Gateway, User Service)
- **Components:** Descriptive (UserController, OrderService)
- **Actors:** Title Case with role (User, Admin)

**Relationship labels:**
- Use verbs that describe the relationship
- Include protocol/format in brackets: [HTTP/REST], [gRPC], [async/AMQP]
- Include direction: creates, sends to, calls, triggers

### A2.3 Diagram Quality

Every diagram must have:
1. A clear title (H3 heading).
2. Consistent styling within the diagram set.
3. Appropriate level of detail.
4. No overlapping or confusing lines.
5. Labels on all relationships.

---

## 📐 STANDARD A3: LAYER DOCUMENTATION

### A3.1 Layer Definition
Each architectural layer must document:

```
- Name: [Standardized layer name]
- Responsibility: [Single sentence]
- Boundaries: [What belongs and what doesn't]
- Components: [List of components in this layer]
- Interfaces: [How the layer exposes functionality]
- Dependencies: [Which layers it depends on]
- Rules: [Architectural rules enforced]
- Violations: [Any detected violations with file references]
```

### A3.2 Layer Naming
Use standardized layer names:
- **Presentation Layer:** UI, API endpoints, controllers
- **Application Layer:** Use cases, services, orchestration
- **Domain Layer:** Business logic, entities, value objects
- **Infrastructure Layer:** Data access, external APIs, messaging

---

## 📐 STANDARD A4: COMPONENT DOCUMENTATION

### A4.1 Component Specification
Each component must document:

```
- Name: [Component name]
- Module: [Parent module]
- Stereotype: [<<controller>>, <<service>>, <<repository>>, etc.]
- Responsibility: [Single paragraph]
- Public API: [Exported functions/classes]
- Internal Structure: [Key internal elements]
- Dependencies: [Components it depends on]
- State: [State managed by this component]
- Lifecycle: [Creation, usage, destruction]
```

### A4.2 Component Relationships
Document all component relationships:
- **Direction:** Who depends on whom
- **Type:** Association, aggregation, composition, dependency
- **Cardinality:** One-to-one, one-to-many, many-to-many
- **Communication:** Synchronous, asynchronous, event-based

---

## 📐 STANDARD A5: ARCHITECTURAL DECISION RECORDS

### A5.1 ADR Format
Every architectural decision must follow this format:

```
# ADR-NNN: Title

## Status
[Proposed / Accepted / Deprecated / Superseded]

## Context
[Problem description, constraints, alternatives]

## Decision
[What was decided]

## Rationale
[Why this decision was made]

## Consequences
[Positive and negative consequences]

## Compliance
[How compliance is verified]

## Notes
[Additional information]
```

### A5.2 When to Create an ADR
Create an ADR for:
- Technology choices (database, framework, language)
- Architectural patterns (microservices, event-driven)
- Significant design decisions (schema design, API style)
- Cross-cutting concerns (security, performance, scalability)

---

## 📐 STANDARD A6: ARCHITECTURE EVALUATION

### A6.1 Quality Attribute Assessment

| Attribute | Assessment Criteria | Rating (1-5) |
|-----------|-------------------|--------------|
| Performance | Latency, throughput, resource usage | /5 |
| Scalability | Horizontal/vertical scaling capability | /5 |
| Availability | Uptime, fault tolerance, redundancy | /5 |
| Maintainability | Modularity, testability, change impact | /5 |
| Security | Authentication, authorization, data protection | /5 |
| Deployability | Deployment complexity, rollback capability | /5 |

### A6.2 Architecture Debt Assessment
Document:
- Deviations from intended architecture
- Layer violations with file references
- Circular dependencies
- God components (too many responsibilities)
- Missing abstractions

---

## ✅ COMPLIANCE CHECK

- [ ] Architectural style identified with evidence
- [ ] Diagrams follow Mermaid standards
- [ ] Layers documented with responsibilities and boundaries
- [ ] Components documented with relationships
- [ ] Key decisions follow ADR format
- [ ] Quality attributes assessed
- [ ] Architecture debt documented

