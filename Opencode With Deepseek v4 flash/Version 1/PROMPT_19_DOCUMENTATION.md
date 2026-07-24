# PROMPT_19 — Phase 18: Documentation Generation

## PHASE CLASS: Synthesis & Publication
## DEPENDENCIES: All Phases 00-17 complete
## OUTPUT DIRECTORY: `re-docs/18-documentation/`

---

## OBJECTIVE

Synthesize all analysis from phases 00-17 into comprehensive, production-quality documentation. Generate the architecture guide, developer handbook, rebuild guide, engineering notes, and cross-reference database.

## PREREQUISITES

- [ ] All phases 00-17 completed
- [ ] All phase outputs saved to `re-docs/`
- [ ] Validation checks passed for all previous phases

## INPUTS

- All files in `re-docs/00-scouting/` through `re-docs/17-config-env/`
- `re-docs/CHECKS.md` (if exists)
- All diagrams from previous phases

## GENERATION STEPS

### Step 1: Architecture Guide (`architecture-guide.md`)

Synthesize all architectural understanding into a cohesive guide:

```markdown
# Architecture Guide — [Project Name]

## Table of Contents
1. System Overview
2. Architecture Style & Principles
3. Layer Architecture
4. Component Architecture
5. Module Map
6. Data Architecture
7. Integration Architecture
8. Deployment Architecture
9. Security Architecture
10. Quality Attributes
11. Architecture Decision Records
12. Glossary
```

**Sources**: PROMPT_08 (Architecture), PROMPT_06 (Modules), PROMPT_05 (Tech Stack)

### Step 2: Developer Handbook (`developer-handbook.md`)

Create a comprehensive developer onboarding guide:

```markdown
# Developer Handbook — [Project Name]

## Table of Contents
1. Getting Started
2. Development Setup
3. Project Structure
4. Coding Conventions
5. Common Workflows
6. Testing Guide
7. Debugging Guide
8. Deployment Guide
9. Troubleshooting
10. FAQ
```

**Sources**: 
- Setup instructions → PROMPT_01 (README)
- Build commands → PROMPT_03 (Build Config)
- Project structure → PROMPT_02 (Structure)
- Workflows → PROMPT_10 (Call Graph)
- Dependencies → PROMPT_04 (Dependencies)
- Coding conventions → PROMPT_02 (Naming)

### Step 3: Rebuild Guide (`rebuild-guide.md`)

Create a guide for rebuilding the system from scratch:

```markdown
# Rebuild Guide — [Project Name]

## Purpose
This guide contains everything needed to rebuild this system from scratch.

## Technology Stack
[List all technologies with versions]

## Architecture Blueprint
[High-level architecture diagram and description]

## Module Specifications
[For each module: responsibility, interface, dependencies]

## Data Model
[Complete data model documentation]

## API Specifications
[All API contracts]

## Key Algorithms
[All core algorithms with pseudocode]

## Configuration
[All configuration needed]
```

**Sources**: All previous phases.

### Step 4: Engineering Notes (`engineering-notes.md`)

Create detailed engineering documentation:

```markdown
# Engineering Notes — [Project Name]

## Architecture Decisions
[Recovered ADRs with context and rationale]

## Design Patterns
[All design patterns with implementation notes]

## Performance Considerations
[Performance characteristics, bottlenecks, optimizations]

## Security Notes
[Security architecture, known concerns, recommendations]

## Technical Debt
[Known technical debt with priorities]

## Future Considerations
[Observations for future development]
```

**Sources**: 
- ADRs → PROMPT_08
- Patterns → PROMPT_13
- Performance → PROMPT_12 (Complexity)
- Security → PROMPT_16 (Error handling), PROMPT_18 (Secrets)
- Technical debt → All phases

### Step 5: Cross-Reference Database (`cross-references.md`)

Create a comprehensive cross-reference:

```markdown
# Cross-Reference Database — [Project Name]

## File → Feature Mapping
| File | Feature(s) |
|------|-----------|
| src/auth/service.ts | Authentication |
| src/orders/service.ts | Order Management |

## Feature → File Mapping
| Feature | Files |
|---------|-------|
| Authentication | src/auth/**, src/api/routes/auth.ts |

## Function → Callers
| Function | Callers |
|----------|---------|
| login() | loginController, mobileAuth, adminImpersonate |

## Component → Dependencies
| Component | Dependencies |
|-----------|-------------|
| AuthService | UserRepository, TokenService, EmailService |
```

**Sources**: All analysis phases.

### Step 6: Complete Diagram Set

Generate and organize all diagrams:

```markdown
# Complete Diagram Set — [Project Name]

## Architecture Diagrams
[All architecture diagrams]

## Data Flow Diagrams
[All data flow diagrams]

## Sequence Diagrams
[All sequence diagrams]

## Component Diagrams
[All component diagrams]

## State Diagrams
[All state machine diagrams]

## Deployment Diagrams
[All deployment diagrams]
```

### Step 7: Summary Document (`REVERSE_ENGINEERING_SUMMARY.md`)

Create the final summary document at `re-docs/` root:

```markdown
# Reverse Engineering Summary — [Project Name]

## Overview
- Repository: [name]
- Purpose: [description]
- Size: [files, LOC]
- Tech Stack: [summary]

## Architecture
[One-paragraph architecture summary]

## Key Findings
- [Finding 1]
- [Finding 2]
- [Finding 3]

## Quality Assessment
| Dimension | Score (1-10) | Notes |
|-----------|-------------|-------|
| Code Quality | 8 | Well-structured, good tests |
| Architecture | 7 | Clean layers, some erosion |
| Documentation | 6 | Good README, sparse inline |
| Testing | 8 | 85% coverage |
| Security | 7 | Basic practices, some gaps |

## Critical Gaps
- GAP-001: [description]
- GAP-002: [description]

## Recommendations
1. [Recommendation 1]
2. [Recommendation 2]
3. [Recommendation 3]
```

## OUTPUT SPECIFICATION

### File 1: `architecture-guide.md`

Complete architecture guide (20-100 pages equivalent).

### File 2: `developer-handbook.md`

Complete developer handbook (15-50 pages equivalent).

### File 3: `rebuild-guide.md`

Complete rebuild guide (10-30 pages equivalent).

### File 4: `engineering-notes.md`

Complete engineering notes (10-30 pages equivalent).

### File 5: `cross-references.md`

Complete cross-reference database.

### File 6: Complete diagrams in `re-docs/diagrams/`

### File 7: `REVERSE_ENGINEERING_SUMMARY.md` (at `re-docs/` root)

## QUALITY REQUIREMENTS

Each documentation file must be:

- **Accurate**: Every claim traceable to source code evidence
- **Navigable**: Table of contents, consistent headings, cross-references
- **Readable**: Clear language, appropriate detail level
- **Complete**: No missing sections, no placeholder content
- **Cohesive**: Consistent terminology, no contradictions
- **Self-contained**: Can be understood without reading source code

## VALIDATION CHECKS

- [ ] Architecture guide covers all architectural aspects
- [ ] Developer handbook enables complete onboarding
- [ ] Rebuild guide enables complete reimplementation
- [ ] Engineering notes capture all important details
- [ ] Cross-references cover all files and features
- [ ] All diagrams are present and correctly formatted
- [ ] Summary document is complete
- [ ] No claim contradicts earlier phase outputs
- [ ] All gaps from previous phases are documented

## COMPLETION CHECKLIST

- [ ] Architecture guide generated
- [ ] Developer handbook generated
- [ ] Rebuild guide generated
- [ ] Engineering notes generated
- [ ] Cross-reference database generated
- [ ] All diagrams generated
- [ ] Summary document generated
- [ ] All outputs saved to `re-docs/18-documentation/`
- [ ] Phase validation checks passed

---

*Proceed to PROMPT_20_VALIDATION.md only after all checklist items are complete.*
