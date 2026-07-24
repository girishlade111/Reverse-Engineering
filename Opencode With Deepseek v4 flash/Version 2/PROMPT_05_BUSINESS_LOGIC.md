========================================================================
PROMPT 05: BUSINESS LOGIC ANALYSIS
========================================================================
Phase 5: Business Logic, Domain Model, and Algorithm Extraction
Enterprise Reverse Engineering Prompt Framework

========================================================================
OBJECTIVES
========================================================================

After completing this phase, you will have:
1. Complete understanding of the domain model and business entities
2. All business rules extracted and documented
3. All algorithms, formulas, and calculations documented
4. Decision trees and branching logic fully mapped
5. Workflow/process flows documented
6. Business invariants and constraints documented
7. Policy and configuration-driven behavior documented
8. Feature map with complete feature descriptions

========================================================================
INPUTS
========================================================================

- DATA_FLOW_DIAGRAMS.md (from Phase 4)
- TRANSFORMATION_PIPELINES.md (from Phase 4)
- STATE_MANAGEMENT.md (from Phase 4)
- EVENT_CATALOG.md (from Phase 4)
- CALL_GRAPHS.md (from Phase 3)
- FUNCTION_CATALOG.md (from Phase 3)
- MODULE_CATALOG.md (from Phase 3)
- All repository files

========================================================================
ACTIVITIES
========================================================================

ACTIVITY 5.1: DOMAIN MODEL EXTRACTION

5.1.1. Identify all domain entities:
    - Core domain models (the primary business objects)
    - Value objects (immutable, equality-based)
    - Aggregates (clusters of domain objects treated as a unit)
    - Domain events (significant business occurrences)

5.1.2. For each domain entity, document:
    - Entity name and location
    - Attributes with types and constraints
    - Relationships to other entities
    - Business meaning (not just technical structure)
    - Lifecycle (creation, modification, deletion)
    - Identity/Key structure
    - Validation rules
    - Business invariants it enforces

5.1.3. Generate Mermaid entity-relationship diagrams.

5.1.4. Generate Mermaid class diagrams for domain model.

ACTIVITY 5.2: BUSINESS RULE EXTRACTION

5.2.1. Extract all explicit business rules:
    - Rules encoded in conditional logic (if/else, switch)
    - Rules encoded in configuration
    - Rules encoded in data validation
    - Rules encoded in workflow logic
    - Rules encoded in policy objects

5.2.2. For each business rule, document:
    - Rule description in plain language
    - Code location
    - Trigger conditions
    - Actions taken when rule applies
    - Exceptions and edge cases
    - Configuration parameters (if any)
    - Business rationale (if discernible)

5.2.3. Categorize business rules:
    - Validation rules: "Field X must be..."
    - Computation rules: "Calculate Y as..."
    - Constraint rules: "Action Z is only allowed when..."
    - Workflow rules: "After event A, do B..."
    - Policy rules: "If condition C, apply rate D..."

ACTIVITY 5.3: ALGORITHM AND CALCULATION DOCUMENTATION

5.3.1. Identify all algorithms, formulas, and calculations:
    - Mathematical formulas
    - Statistical calculations
    - Sorting, searching, filtering algorithms
    - Optimization algorithms
    - Machine learning inference
    - Data structure operations
    - String processing algorithms
    - Cryptographic operations

5.3.2. For each algorithm/calculation, document:
    - Algorithm name/description
    - Code location
    - Input parameters with types and ranges
    - Output with type and interpretation
    - Mathematical formula (if applicable)
    - Step-by-step logic breakdown
    - Time/space complexity
    - Edge cases and boundary conditions
    - Precision/rounding considerations
    - Dependencies on other algorithms

5.3.3. For complex algorithms:
    - Generate Mermaid flowchart of the algorithm
    - Provide worked examples with sample inputs/outputs
    - Document any known limitations or bugs

ACTIVITY 5.4: DECISION TREE EXTRACTION

5.4.1. Identify all decision points in the business logic:
    - Conditional branches that affect business outcomes
    - Feature flags and toggles
    - A/B test assignments
    - User segmentation logic
    - Permission/authorization decisions
    - Routing decisions
    - Pricing/tier decisions

5.4.2. For each decision tree, document:
    - Decision criteria
    - All possible branches
    - Outcome of each branch
    - Data used in decision
    - Configuration that influences the decision
    - Default behavior
    - Error/failure outcomes

5.4.3. Generate Mermaid decision tree diagrams.

ACTIVITY 5.4A: FEATURE FLAG AND TOGGLE ANALYSIS

5.4A.1. Identify all feature flags/toggles:
    - Release toggles (gradual rollout)
    - Experiment toggles (A/B testing)
    - Permission toggles (entitlement gating)
    - Operational toggles (kill switches)
    - Configuration overrides

5.4A.2. For each feature flag:
    - Flag name/key
    - Location in codebase
    - What behavior it controls
    - Default state
    - How it is evaluated
    - How it is configured (static, dynamic, remote)
    - Caching duration (if remote)
    - Owner/team
    - Lifetime/cleanup plan
    - Testing under both states

5.4A.3. Document the feature flag infrastructure:
    - Flag evaluation library/framework
    - Flag storage and distribution
    - Flag management UI/API
    - Flag auditing and analytics
    - Flag removal/deprecation process

ACTIVITY 5.5: WORKFLOW AND PROCESS EXTRACTION

5.5.1. Identify all business workflows:
    - User-facing workflows (registration, checkout, etc.)
    - Internal workflows (data processing, reporting, etc.)
    - Scheduled workflows (cron jobs, batch processes)
    - Event-driven workflows

5.5.2. For each workflow, document:
    - Workflow trigger/entry point
    - Complete step sequence
    - Actors involved (users, systems, timers)
    - Conditions and branching
    - Timeouts and delays
    - Error handling per step
    - Compensation/rollback actions
    - Completion criteria
    - Outputs and side effects

5.5.3. Generate Mermaid sequence diagrams for each workflow.
5.5.4. Generate Mermaid flowchart for each workflow.

ACTIVITY 5.6: FEATURE MAP CONSTRUCTION

5.6.1. Identify all features of the system:
    - A feature is a distinct user-facing capability
    - Group related features into feature areas

5.6.2. For each feature, document:
    - Feature name
    - Feature area/category
    - User story ("As a [user], I can [action]...")
    - Entry point (how the feature is accessed)
    - Implementation module/file location
    - Dependencies on other features
    - Configuration/enablement
    - Test coverage
    - Known limitations

5.6.3. Generate a feature map showing:
    - Feature hierarchy
    - Feature dependencies
    - Feature-to-module mapping

ACTIVITY 5.7: BUSINESS INVARIANTS AND CONSTRAINTS

5.7.1. Identify all business invariants:
    - Conditions that must always be true
    - Data integrity constraints
    - Business rules that cannot be violated
    - Consistency requirements

5.7.2. For each invariant, document:
    - Invariant description
    - Where it is enforced (code location)
    - What happens if it is violated
    - Enforcement mechanism (database constraint, code check, etc.)
    - Can it be temporarily violated? (e.g., during a multi-step operation)

ACTIVITY 5.8: POLICY AND CONFIGURATION-DRIVEN BEHAVIOR

5.8.1. Identify behavior driven by configuration/policy:
    - Feature flags
    - Dynamic configuration
    - Tenant-specific settings
    - Environment-specific behavior
    - User-specific policies

5.8.2. For each, document:
    - Configuration key/structure
    - How it affects behavior
    - Where it is read
    - Default value
    - Allowed values/ranges
    - Change propagation (hot reload vs restart required)

========================================================================
ANALYSIS METHODOLOGY
========================================================================

This phase requires DomainScan methodology:

Read the code with business intent, not just technical intent.
Ask yourself:
- "What business problem does this solve?"
- "What business rule is encoded here?"
- "What happens in the real world when this code executes?"
- "What domain concept does this represent?"

Separate business logic from technical infrastructure.
Document the "why" as well as the "what."

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT 5.1: DOMAIN_MODEL.md
- Domain entity catalog
- Entity relationship documentation
- Mermaid ER diagrams
- Mermaid class diagrams
- Value object documentation

ARTIFACT 5.2: BUSINESS_RULES_CATALOG.md
- Complete business rule inventory
- Rule categorization
- Rule documentation with code citations

ARTIFACT 5.3: ALGORITHM_CATALOG.md
- Algorithm inventory
- Formula documentation
- Complexity analysis
- Worked examples

ARTIFACT 5.4: DECISION_TREES.md
- Decision tree inventory
- Mermaid decision tree diagrams
- Branch documentation

ARTIFACT 5.5: WORKFLOW_CATALOG.md
- Complete workflow inventory
- Mermaid sequence diagrams per workflow
- Mermaid flowcharts per workflow
- Error handling per step

ARTIFACT 5.6: FEATURE_MAP.md
- Feature inventory
- Feature hierarchy
- Feature-module mapping
- Feature dependency graph

ARTIFACT 5.7: INVARIANTS_AND_CONSTRAINTS.md
- Business invariant catalog
- Enforcement documentation
- Violation handling

ARTIFACT 5.8: CONFIGURATION_POLICY.md
- Configuration-driven behavior
- Feature flag documentation
- Dynamic configuration catalog

========================================================================
QUALITY GATES
========================================================================

Before completing this phase, verify:

[ ] All domain entities are identified and documented.
[ ] All business rules are extracted with code citations.
[ ] All algorithms are documented with complexity analysis.
[ ] Decision trees are complete for core logic.
[ ] All workflows have sequence diagrams.
[ ] Feature map covers all system capabilities.
[ ] Business invariants are documented.
[ ] Configuration-driven behavior is mapped.
[ ] Business logic is separated from infrastructure in documentation.
[ ] Artifacts meet quality standards (score >= 4.0).

========================================================================
OUTPUTS TO NEXT PHASE
========================================================================

Pass to Phase 6:
- DOMAIN_MODEL.md
- BUSINESS_RULES_CATALOG.md
- FEATURE_MAP.md
- WORKFLOW_CATALOG.md

Pass to Phase 8:
- ALGORITHM_CATALOG.md
- DECISION_TREES.md
- INVARIANTS_AND_CONSTRAINTS.md

========================================================================
END OF PROMPT 05
========================================================================
