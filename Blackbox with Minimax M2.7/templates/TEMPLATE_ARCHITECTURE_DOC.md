# Template: Architecture Documentation

> **Document:** templates/TEMPLATE_ARCHITECTURE_DOC.md  
> **Version:** 1.0.0  
> **Purpose:** Template for documenting system/component/module architecture  
> **When to Use:** During Phase 9 for architecture-related documentation

---

## 📋 STRUCTURE

```markdown
# [System/Component/Module] Architecture

> **Document:** [relative-path.md]
> **Phase:** Architecture Documentation
> **Last Updated:** [YYYY-MM-DD]
> **Purpose:** Describe the architecture of [entity]

---

## Overview

[2-3 paragraph overview of the architecture]

## Architectural Style

[Primary architectural style(s), with evidence]

## Architecture Diagram

```mermaid
graph TB
    [Architecture diagram]
```

## Layers

### [Layer 1]
- **Purpose:** [Purpose]
- **Components:** [Components in this layer]
- **Interfaces:** [How this layer communicates]

### [Layer 2]
- **Purpose:** [Purpose]
- **Components:** [Components in this layer]
- **Interfaces:** [How this layer communicates]

## Components

### [Component 1]
- **Module:** [Parent module]
- **Purpose:** [Purpose]
- **Files:** [Key files]
- **Dependencies:** [Dependencies]
- **Used By:** [Consumers]

### [Component 2]
- **Module:** [Parent module]
- **Purpose:** [Purpose]
- **Files:** [Key files]
- **Dependencies:** [Dependencies]
- **Used By:** [Consumers]

## Component Interactions

[Description of how components interact]

```mermaid
sequenceDiagram
    [Interaction diagram]
```

## Data Flow

[Description of data flow through the architecture]

```mermaid
graph LR
    [Data flow diagram]
```

## Communication Patterns

| Pattern | Where Used | Protocol |
|---------|------------|----------|
| [Pattern] | [Component] | [Protocol] |

## Key Design Decisions

| Decision | Rationale | Trade-offs |
|----------|-----------|------------|
| [Decision] | [Rationale] | [Trade-offs] |

## Confidence Assessment

- **Architecture Confidence:** [High/Medium/Low]
- **Uncertainties:** [List of uncertainties]
- **Open Questions:** [List of open questions]

## Cross-References

- [Related Document 1](path/to/doc.md) — Description
- [Related Document 2](path/to/doc.md) — Description
```

---

## 📝 USAGE GUIDELINES

1. **Scope:** Use for system-level, component-level, and module-level architecture documentation.
2. **Diagrams:** Always include a high-level architecture diagram. Add detail diagrams as needed.
3. **Audience:** Architects and senior developers. Assume technical background but not prior knowledge of this system.
4. **Depth:** Include enough detail for an architect to understand design decisions and trade-offs.
5. **Evidence:** All architectural claims must be traceable to code evidence.

---

## ✅ QUALITY CHECKLIST

- [ ] Architectural style(s) clearly stated
- [ ] Architecture diagram included and accurate
- [ ] All layers documented
- [ ] All components documented
- [ ] Component interactions described
- [ ] Data flow described
- [ ] Communication patterns identified
- [ ] Design decisions documented
- [ ] Cross-references to related documents
- [ ] Confidence assessment included

