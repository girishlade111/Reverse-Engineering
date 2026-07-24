# PROJECT SPECIFICATION

## Detailed Specifications for Reverse Engineering Project

---

## 1. PROJECT DEFINITION

### 1.1 Project Type

**Classification:** Software Reverse Engineering and Documentation Generation

**Scope:** Complete analysis and documentation of any software repository

**Output:** Comprehensive technical documentation package

### 1.2 Target Systems

This framework applies to:

- Web applications (frontend, backend, full-stack)
- Mobile applications (iOS, Android, cross-platform)
- Desktop applications
- CLI tools and utilities
- Libraries and SDKs
- APIs and microservices
- AI/ML systems and agents
- Infrastructure as code
- Mixed technology stacks

### 1.3 Repository Sizes

| Size Category | File Count | Approach |
|---------------|------------|----------|
| Small | 1-50 files | Complete file-by-file analysis |
| Medium | 51-500 files | Prioritized deep-dive with sampling |
| Large | 501-5000 files | Architecture-first with targeted analysis |
| Enterprise | 5000+ files | Layered analysis with abstraction levels |

---

## 2. TECHNICAL SPECIFICATIONS

### 2.1 Analysis Depth Requirements

#### Level 1: Repository Level (Mandatory)
- Complete file inventory
- Directory structure
- Configuration files
- Build artifacts
- Documentation files

#### Level 2: Module Level (Mandatory)
- Module boundaries
- Inter-module communication
- Module responsibilities
- Module dependencies

#### Level 3: File Level (Mandatory)
- File purpose and responsibility
- Imports and exports
- Key functions/classes
- Relationships to other files

#### Level 4: Component Level (Mandatory)
- Class definitions
- Function implementations
- Interface contracts
- Type definitions

#### Level 5: Logic Level (Mandatory for Critical Paths)
- Algorithm details
- Decision logic
- State transitions
- Error handling

### 2.2 Documentation Granularity

| Element | Required Detail Level |
|---------|----------------------|
| System Architecture | High-level with component interactions |
| Module Architecture | Mid-level with interfaces |
| Classes | Full API with method signatures |
| Functions | Purpose, parameters, return, side effects |
| Complex Algorithms | Step-by-step explanation |
| Configuration | All options with defaults and effects |
| Dependencies | Version, purpose, usage location |

### 2.3 Diagram Requirements

**Mandatory Diagrams:**

1. System Context Diagram
2. Container/Component Diagram
3. Directory Structure Tree
4. Dependency Graph (high-level)
5. Main Data Flow Diagram
6. Primary Execution Flow

**Conditional Diagrams (when applicable):**

- Sequence Diagrams (for complex interactions)
- State Machine Diagrams (for stateful systems)
- Entity Relationship Diagrams (for data-heavy systems)
- Deployment Diagrams (for distributed systems)
- Class Diagrams (for OOP-heavy systems)
- Activity Diagrams (for complex workflows)

---

## 3. DELIVERABLE SPECIFICATIONS

### 3.1 Core Documentation Package

#### ARCHITECTURE.md
```
- Executive Summary
- System Overview
- Architecture Patterns
- Component Architecture
- Module Breakdown
- Technology Stack
- Architecture Diagrams
- Design Decisions
- Trade-offs
```

#### CODEBASE.md
```
- Directory Structure
- File Organization
- Naming Conventions
- Code Style
- Key Files Inventory
- File Responsibility Matrix
```

#### DEPENDENCIES.md
```
- External Dependencies Table
- Internal Dependencies Graph
- Dependency Categories
- Version Information
- Usage Locations
- Security Considerations
```

#### DATA_FLOW.md
```
- Data Entry Points
- Data Transformations
- Data Storage
- Data Exit Points
- Data Flow Diagrams
- State Management
```

#### EXECUTION_FLOW.md
```
- Entry Points
- Main Execution Paths
- Control Flow Diagrams
- Event Handling
- Async Operations
- Error Propagation
```

#### API_REFERENCE.md
```
- Public API Documentation
- Internal Interfaces
- API Contracts
- Request/Response Formats
- Authentication Requirements
- Rate Limiting
```

#### BUSINESS_LOGIC.md
```
- Domain Models
- Business Rules
- Workflow Descriptions
- Decision Logic
- Validation Rules
- Calculation Methods
```

#### OPERATIONS.md
```
- Build Instructions
- Development Setup
- Testing Procedures
- Deployment Guide
- Configuration Reference
- Environment Variables
```

### 3.2 Supplementary Materials

#### DIAGRAMS/
- All mermaid diagram source files
- Exported PNG/SVG versions (if capable)
- Diagram index and descriptions

#### ANALYSIS/
- Detailed analysis notes per prompt
- Evidence logs
- Investigation trails
- Open questions log

#### CHECKLISTS/
- Completeness checklists
- Quality verification forms
- Cross-reference indexes
- Validation reports

### 3.3 Format Specifications

**File Formats:**
- Primary: Markdown (.md)
- Diagrams: Mermaid (embedded in .md)
- Data: JSON (.json) for structured outputs
- Lists: Plain text or Markdown tables

**Naming Conventions:**
- Files: UPPER_CASE.md for framework, PascalCase.md for outputs
- Directories: lowercase-with-dashes
- Diagrams: diagram-type-description.mmd

**Version Control:**
- Include version header in each document
- Track revision history
- Document last update timestamp

---

## 4. QUALITY SPECIFICATIONS

### 4.1 Accuracy Requirements

| Requirement | Threshold |
|-------------|-----------|
| File Coverage | 100% of non-generated files |
| Claim Verification | 100% of claims must have evidence |
| Diagram Accuracy | Must match actual code behavior |
| Cross-reference Validity | 100% of links must resolve |
| Technical Precision | No ambiguous terminology |

### 4.2 Completeness Requirements

**Must Document:**
- Every source code file
- Every configuration file
- Every build script
- Every test file (summary level acceptable)
- Every public interface
- Every entry point
- Every dependency

**May Summarize:**
- Boilerplate code
- Generated files
- Third-party code
- Trivial getters/setters
- Standard CRUD operations (unless complex)

### 4.3 Readability Requirements

- Flesch Reading Ease: 50+ (technical audience)
- Consistent terminology throughout
- Clear section hierarchy
- Logical flow between sections
- Minimal jargon without explanation

---

## 5. PROCESS SPECIFICATIONS

### 5.1 Phase Gates

Each phase requires completion criteria before proceeding:

**Phase 1 Gate:**
- [ ] Complete file inventory generated
- [ ] Technology stack identified
- [ ] Initial architecture understood
- [ ] Repository mapped

**Phase 2 Gate:**
- [ ] All dependencies mapped
- [ ] Data flows documented
- [ ] Control flows documented
- [ ] Key algorithms identified

**Phase 3 Gate:**
- [ ] Specialized analyses complete
- [ ] Domain-specific patterns documented
- [ ] Security considerations addressed
- [ ] Testing landscape understood

**Phase 4 Gate:**
- [ ] All documentation synthesized
- [ ] Quality checks passed
- [ ] Final review complete
- [ ] Deliverables packaged

### 5.2 Review Checkpoints

**Checkpoint 1:** After PROMPT_04 (Code Structure)
- Verify understanding matches reality
- Adjust approach if needed

**Checkpoint 2:** After PROMPT_09 (Business Logic)
- Validate business understanding
- Confirm workflow accuracy

**Checkpoint 3:** After PROMPT_13 (Build/Deploy)
- Ensure operational completeness
- Verify deployment understanding

**Final Checkpoint:** After PROMPT_14 (Synthesis)
- Complete quality audit
- Final validation

### 5.3 Escalation Criteria

Escalate to human review when:

- Critical ambiguity cannot be resolved
- Security vulnerability discovered
- Major architectural inconsistency found
- Analysis reveals potential data loss risk
- Repository contains undocumented critical systems

---

## 6. CONSTRAINTS AND LIMITATIONS

### 6.1 Analysis Constraints

**Time Constraints:**
- Prioritize critical paths over edge cases
- Focus on 80/20 rule for large codebases
- Document what cannot be fully analyzed

**Access Constraints:**
- Work only with provided repository contents
- Do not access external resources unless specified
- Respect .gitignore and exclusion patterns

**Capability Constraints:**
- Cannot execute code (static analysis only)
- Cannot access runtime behavior
- Cannot query external APIs
- Cannot access databases directly

### 6.2 Assumption Management

**Document All Assumptions:**
```
ASSUMPTION: [Description]
LOCATION: [Where this assumption applies]
IMPACT: [What depends on this assumption]
CONFIDENCE: [High/Medium/Low]
VERIFICATION: [How to verify if possible]
```

**Assumption Categories:**
- Language/Framework conventions
- Standard library behaviors
- Common design patterns
- Industry-standard practices

### 6.3 Uncertainty Handling

**Uncertainty Levels:**

| Level | Description | Action |
|-------|-------------|--------|
| Certain | Verified against code | Document as fact |
| Confident | Strong evidence, multiple sources | Document with confidence note |
| Probable | Reasonable inference | Document as likely behavior |
| Speculative | Weak evidence, single source | Document as possibility |
| Unknown | No evidence available | Document as gap |

---

## 7. INTEGRATION SPECIFICATIONS

### 7.1 Tool Integration

**Expected Tool Capabilities:**
- File system access (read-only)
- Text search and pattern matching
- AST parsing (if available)
- Diagram rendering
- Markdown processing

**Optional Tool Integrations:**
- Git history analysis
- Issue tracker integration
- CI/CD system access
- Package registry queries

### 7.2 Output Integration

**Compatible Systems:**
- Static site generators (Docusaurus, MkDocs)
- Documentation platforms (GitBook, Notion)
- Wiki systems
- Knowledge bases

**Export Formats:**
- Markdown (primary)
- HTML (via conversion)
- PDF (via conversion)
- JSON (structured data)

---

## 8. MAINTENANCE SPECIFICATIONS

### 8.1 Documentation Updates

**Trigger Conditions:**
- Repository changes detected
- New features added
- Architecture modified
- Dependencies updated

**Update Process:**
1. Identify affected sections
2. Re-run relevant prompts
3. Update documentation
4. Version increment
5. Change log entry

### 8.2 Framework Updates

**Feedback Loop:**
- Track analysis gaps
- Record common issues
- Collect improvement suggestions
- Update prompts iteratively

**Version Management:**
- Semantic versioning for framework
- Backward compatibility where possible
- Migration guides for breaking changes

---

## 9. ACCEPTANCE CRITERIA

### 9.1 Functional Acceptance

The reverse engineering is acceptable when:

- [ ] A new developer can understand the system
- [ ] Architecture decisions are clear
- [ ] Code navigation is possible via documentation
- [ ] Build and deployment are reproducible
- [ ] APIs are fully documented
- [ ] Data flows are traceable
- [ ] All checklists pass

### 9.2 Quality Acceptance

Quality is acceptable when:

- [ ] No factual errors detected
- [ ] All diagrams render correctly
- [ ] Cross-references all work
- [ ] Terminology is consistent
- [ ] Formatting is professional
- [ ] Writing is clear and precise

### 9.3 Completeness Acceptance

Completeness is achieved when:

- [ ] All mandatory sections complete
- [ ] All required diagrams present
- [ ] All files accounted for
- [ ] All dependencies documented
- [ ] All entry points identified
- [ ] All tests catalogued

---

*This specification document defines the detailed requirements for the reverse engineering project. All outputs must comply with these specifications.*
