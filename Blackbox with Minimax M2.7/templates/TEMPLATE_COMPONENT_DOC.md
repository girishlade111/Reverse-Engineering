# Template: Component Documentation

> **Document:** templates/TEMPLATE_COMPONENT_DOC.md  
> **Version:** 1.0.0  
> **Purpose:** Template for documenting individual components  
> **When to Use:** During Phase 9 for component-level documentation

---

## 📋 STRUCTURE

```markdown
# Component: [Component Name]

> **Document:** [relative-path.md]
> **Phase:** Component Documentation
> **Last Updated:** [YYYY-MM-DD]
> **Purpose:** Document the [component name] component

---

## Overview

- **Module:** [Parent module]
- **Purpose:** [What this component does]
- **Criticality:** [Core / Important / Peripheral]
- **Complexity:** [Low / Medium / High]

## Files

| File | Purpose | Lines |
|------|---------|-------|
| [path/to/file] | [Purpose] | [LOC] |
| [path/to/file] | [Purpose] | [LOC] |

## Public API

### Functions

#### `functionName(param1, param2)`
- **File:** [file:line]
- **Purpose:** [Purpose]
- **Parameters:** [Description]
- **Returns:** [Description]
- **Throws:** [Error conditions]

### Classes

#### `ClassName`
- **File:** [file:line]
- **Purpose:** [Purpose]
- **Properties:** [Key properties]
- **Methods:** [Key methods]

## Dependencies

| Dependency | Type | Purpose |
|------------|------|---------|
| [Component/Module] | Internal | [Purpose] |
| [Library] | External | [Purpose] |

## State Management

[How this component manages state]

## Error Handling

| Error | Handling | Recovery |
|-------|----------|----------|
| [Error type] | [Handling] | [Recovery] |

## Usage Example

```[language]
[Code example showing how to use this component]
```

## Confidence Assessment

- **Component Confidence:** [High/Medium/Low]
- **Uncertainties:** [List]

## Cross-References

- [Related Document 1](path/to/doc.md)
- [Related Document 2](path/to/doc.md)
```

---

## 📝 USAGE GUIDELINES

1. **Scope:** Use for individual components (services, controllers, repositories, etc.).
2. **Audience:** Developers who need to understand or modify this component.
3. **Depth:** Include enough detail for a developer to use the component correctly.
4. **Code Examples:** Always include at least one usage example.
5. **Public API:** Document only the public API; internal details are in the code.

---

## ✅ QUALITY CHECKLIST

- [ ] Component purpose clearly stated
- [ ] All files listed
- [ ] Public API fully documented
- [ ] Dependencies listed
- [ ] State management described
- [ ] Error handling documented
- [ ] Usage example included
- [ ] Cross-references to related components

