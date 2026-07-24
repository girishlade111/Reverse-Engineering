========================================================================
PROMPT 09: DOCUMENTATION GENERATION
========================================================================
Phase 9: Comprehensive Documentation Generation
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. A complete set of documentation covering all aspects of the system
2. Architecture Handbook for system architects and decision-makers
3. Developer Handbook for engineers working on the codebase
4. Rebuild Guide for recreating the system from scratch
5. Engineering Notes with deep technical observations
6. Cross-Reference Index for navigating documentation
7. Validation Checklist for verifying documentation accuracy

========================================================================
INPUTS
========================================================================

All artifacts from Phases 1 through 8.

This phase is a synthesis phase. It does not read new code.
It compiles and organizes all findings from prior phases into
coherent, audience-targeted documentation.

========================================================================
SYNTHESIS PRINCIPLES
========================================================================

PRINCIPLE 1: AUDIENCE-APPROPRIATE
Each handbook serves a different audience:
- Architecture Handbook: Architects, tech leads, decision-makers
- Developer Handbook: Developers working on the codebase
- Rebuild Guide: Engineers rebuilding the system
- Engineering Notes: Engineers seeking deep understanding

PRINCIPLE 2: NON-REDUNDANT
Each handbook should reference rather than duplicate content.
Use cross-references liberally.

PRINCIPLE 3: COMPLETE
While each handbook targets its audience, collectively the
documentation must cover every aspect of the system.

PRINCIPLE 4: ACTIONABLE
Documentation should enable action, not just describe.
A developer should be able to fix a bug after reading the
relevant section.

PRINCIPLE 5: VERIFIABLE
Every claim in the documentation must be traceable to evidence
in the code or to analysis artifacts from earlier phases.

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 9.1: ARCHITECTURE HANDBOOK GENERATION

Generate HANDBOOK_ARCHITECTURE.md covering:

1. SYSTEM OVERVIEW
   - One-paragraph system description
   - Primary purpose and capabilities
   - Key stakeholders and users
   - Major subsystems overview

2. ARCHITECTURAL STYLE
   - Style determination with rationale
   - Architectural constraints
   - Key architectural decisions and trade-offs
   - Architecture evaluation (strengths, weaknesses)

3. COMPONENT ARCHITECTURE
   - Component diagram (Mermaid)
   - Component responsibility documentation
   - Component interaction patterns
   - Data flow between components

4. LAYERED ARCHITECTURE
   - Layer diagram (Mermaid)
   - Layer responsibility documentation
   - Layer dependency rules
   - Layer violation analysis

5. DEPLOYMENT ARCHITECTURE
   - Deployment diagram (Mermaid)
   - Infrastructure components
   - Network topology
   - Scaling strategy

6. TECHNOLOGY DECISIONS
   - Why each major technology was chosen
   - Alternatives considered (if documented)
   - Technology constraints and trade-offs
   - Version compatibility notes

7. ARCHITECTURAL PATTERNS
   - Patterns used with locations
   - Pattern purpose and implementation
   - Custom pattern documentation

8. SYSTEM BOUNDARIES
   - External interfaces
   - Integration points
   - Boundary protocols and formats

9. ARCHITECTURAL EVOLUTION
   - Historical architecture (if discernible)
   - Current architecture
   - Planned/future architecture directions
   - Migration paths

ACTIVITY 9.2: DEVELOPER HANDBOOK GENERATION

Generate HANDBOOK_DEVELOPER.md covering:

1. REPOSITORY OVERVIEW
   - Repository structure and navigation
   - Tech stack summary
   - Development environment setup
   - Build and run instructions

2. CODE ORGANIZATION
   - Directory structure with purposes
   - Module organization
   - File naming conventions
   - Code organization patterns

3. CODING CONVENTIONS
   - Language-specific conventions
   - Framework conventions
   - Naming conventions
   - File structure conventions
   - Import/export conventions
   - Comment/documentation conventions

4. KEY WORKFLOWS
   - Development workflow
   - Testing workflow
   - CI/CD workflow
   - Release workflow
   - Debugging workflow

5. MODULE GUIDE
   - For each module, provide:
     - Purpose and responsibility
     - Key files and their purposes
     - Public API summary
     - Dependencies
     - Configuration
     - Common tasks and patterns

6. DATABASE GUIDE (if applicable)
   - Schema overview
   - Common queries
   - Migration guide
   - Connection configuration

7. API REFERENCE
   - Endpoint summary
   - Authentication instructions
   - Common request/response patterns
   - Error response format
   - SDK/client library usage

8. TESTING GUIDE
   - Test organization
   - Running tests
   - Writing tests
   - Test patterns and fixtures
   - Coverage goals

9. TROUBLESHOOTING GUIDE
   - Common issues and solutions
   - Debugging techniques
   - Logging and monitoring access
   - Support contacts/resources

ACTIVITY 9.3: REBUILD GUIDE GENERATION

Generate HANDBOOK_REBUILD_GUIDE.md covering:

1. PREREQUISITES
   - Required tools and versions
   - Required accounts and access
   - Required knowledge

2. BUILD PROCESS
   - Step-by-step build instructions
   - Configuration for each environment
   - Build variants (dev, staging, production)
   - Troubleshooting build issues

3. DATABASE SETUP (if applicable)
   - Database creation
   - Schema migration
   - Seed data
   - Connection configuration

4. DEPLOYMENT PROCESS
   - Infrastructure provisioning
   - Deployment steps per environment
   - Rollback procedures
   - Post-deployment verification

5. INTEGRATION SETUP
   - Third-party service configuration
   - API key setup
   - Webhook configuration
   - Integration testing

6. VERIFICATION PROCESS
   - Smoke tests
   - Health check verification
   - Integration test execution
   - Performance validation

7. OPERATIONS GUIDE
   - Monitoring setup
   - Logging configuration
   - Alert configuration
   - Backup and recovery
   - Scaling procedures

ACTIVITY 9.4: ENGINEERING NOTES GENERATION

Generate ENGINEERING_NOTES.md covering:

1. DEEP TECHNICAL OBSERVATIONS
   - Notable design decisions and their rationale
   - Complex algorithms explained
   - Performance optimizations and their impact
   - Concurrency and parallelism patterns
   - Memory management strategies
   - State management approaches

2. TRADE-OFF ANALYSIS
   - Architectural trade-offs made
   - Technology trade-offs
   - Performance vs. correctness trade-offs
   - Complexity vs. maintainability trade-offs

3. TECHNICAL DEBT IDENTIFICATION
   - Areas of technical debt
   - Deprecated code and patterns
   - Workarounds and their reasons
   - Migration/cleanup opportunities

4. UNIQUE PATTERNS AND INNOVATIONS
   - Domain-specific innovations
   - Custom framework extensions
   - Novel approaches to common problems

5. INTERESTING CODE SECTIONS
   - Particularly elegant solutions
   - Complex but necessary code
   - Code that deserves special attention
   - Code that may be confusing to newcomers

6. PERFORMANCE CHARACTERISTICS
   - Bottlenecks identified
   - Scaling limits
   - Resource usage patterns
   - Optimization opportunities

ACTIVITY 9.5: CROSS-REFERENCE INDEX GENERATION

Generate CROSS_REFERENCES.md covering:

1. MODULE-FILE CROSS REFERENCE
   - Which files belong to which modules

2. FEATURE-FILE CROSS REFERENCE
   - Which features are implemented in which files

3. API-FILE CROSS REFERENCE
   - Which API endpoints are implemented in which files

4. DATABASE-FILE CROSS REFERENCE
   - Which files access which database tables/collections

5. ERROR-FILE CROSS REFERENCE
   - Which error types originate from which files

6. DEPENDENCY-FILE CROSS REFERENCE
   - Which external dependencies are used in which files

7. PATTERN-FILE CROSS REFERENCE
   - Where each design pattern is implemented

8. DOCUMENTATION-FILE CROSS REFERENCE
   - Cross-reference between documentation and source files

ACTIVITY 9.6: VALIDATION CHECKLIST GENERATION

Generate the initial VALIDATION_CHECKLIST.md containing:

1. COMPLETENESS CHECKS
   - [ ] Every source file has documented purpose
   - [ ] Every module is documented
   - [ ] Every entry point is documented
   - [ ] Every API endpoint is documented
   - [ ] Every external integration is documented
   - [ ] Every error type is documented

2. ACCURACY CHECKS
   - [ ] Every claim cites evidence
   - [ ] Diagrams match actual code structure
   - [ ] API documentation matches implementation
   - [ ] Configuration documentation is accurate
   - [ ] Dependency versions are correct

3. QUALITY CHECKS
   - [ ] All quality dimensions meet minimum scores
   - [ ] No undefined terminology
   - [ ] Consistent formatting across all documents
   - [ ] Cross-references are valid
   - [ ] No broken links

========================================================================
ANALYSIS METHODOLOGY
========================================================================

This phase uses Synthesis methodology:

1. COMPILE: Gather all artifacts from prior phases.
2. ORGANIZE: Structure content for each target audience.
3. SYNTHESIZE: Combine findings into coherent narratives.
4. REFERENCE: Cross-link between documents.
5. REVIEW: Verify against quality standards.

Do NOT read new code in this phase unless necessary to
clarify an ambiguous finding from prior phases.

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT 9.1: HANDBOOK_ARCHITECTURE.md
ARTIFACT 9.2: HANDBOOK_DEVELOPER.md
ARTIFACT 9.3: HANDBOOK_REBUILD_GUIDE.md
ARTIFACT 9.4: ENGINEERING_NOTES.md
ARTIFACT 9.5: CROSS_REFERENCES.md
ARTIFACT 9.6: VALIDATION_CHECKLIST.md (initial draft)

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] Architecture Handbook serves its intended audience.
[ ] Developer Handbook enables productive development.
[ ] Rebuild Guide is sufficient to recreate the system.
[ ] Engineering Notes capture deep technical insights.
[ ] Cross-Reference index is complete and accurate.
[ ] Validation Checklist covers all quality dimensions.
[ ] All handbooks are internally consistent.
[ ] Cross-references between handbooks are accurate.
[ ] No new code was read unnecessarily.
[ ] Artifacts meet quality standards (score >= 4.0).

========================================================================
OUTPUTS TO NEXT PHASE
========================================================================

Pass to Phase 10:
- HANDBOOK_ARCHITECTURE.md
- HANDBOOK_DEVELOPER.md
- HANDBOOK_REBUILD_GUIDE.md
- ENGINEERING_NOTES.md
- CROSS_REFERENCES.md
- VALIDATION_CHECKLIST.md

========================================================================
END OF PROMPT 09
========================================================================
