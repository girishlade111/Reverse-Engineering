========================================================================
SUPPLEMENTARY PROMPT S7: MONOREPO ANALYSIS
========================================================================
Supplementary Analysis: Monorepo and Multi-Package Architecture
Enterprise Reverse Engineering Prompt Framework

========================================================================
PURPOSE
========================================================================

This supplementary prompt provides deep analysis of monorepo
and multi-package repository structures.

========================================================================
WHEN TO USE
========================================================================

Execute this prompt if:
- Multiple packages exist in a single repository
- Package manager workspaces are used
- Shared and package-specific configurations exist
- Inter-package dependencies need mapping
- Build orchestration across packages exists

Execute after Phase 1 and before Phase 3.

========================================================================
ACTIVITIES
========================================================================

S7.1. PACKAGE INVENTORY
- Document all packages/applications in the repo
- Document package manifests and their purposes
- Document package versioning strategy
- Document package dependency relationships

S7.2. SHARED VS PACKAGE-SPECIFIC ANALYSIS
- Document shared configurations
- Document shared utilities and libraries
- Document shared types and interfaces
- Document shared build tooling

S7.3. INTER-PACKAGE DEPENDENCY GRAPH
- Build complete inter-package dependency graph
- Document circular dependency issues (if any)
- Document shared dependency version alignment
- Document external dependency duplication

S7.4. BUILD ORCHESTRATION
- Document build order and dependency resolution
- Document build caching strategy
- Document test selection for changes
- Document deployment sequencing

========================================================================
REQUIRED ARTIFACTS
========================================================================

ARTIFACT S7.1: PACKAGE_INVENTORY.md
ARTIFACT S7.2: SHARED_VS_PACKAGE.md
ARTIFACT S7.3: INTER_PACKAGE_GRAPH.md
ARTIFACT S7.4: BUILD_ORCHESTRATION.md

========================================================================
END OF PROMPT S7
========================================================================
