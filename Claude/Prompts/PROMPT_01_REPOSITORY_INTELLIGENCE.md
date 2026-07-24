# PHASE 1 — REPOSITORY INTELLIGENCE

## Objective
Establish top-level ground truth about the repository before any deep analysis begins.

## Steps
1. Produce a full folder tree, depth-limited to meaningful levels. Collapse `node_modules`, `vendor`, `dist`, `build`, `.git`, and other generated/dependency folders into single collapsed lines.
2. Detect monorepo structure: list every distinct app/package/service found. Identify workspace tooling in use (nx, turborepo, lerna, pnpm/yarn workspaces, Cargo workspaces, etc.) and how apps/packages relate (shared libs, internal package references).
3. Per distinct stack found, identify: language(s) + version (from tsconfig/pyproject/go.mod/etc.), framework(s) + version, package manager, runtime target (node version, browser targets, Python version).
4. Identify entry points for every app/service (main files, index files, server bootstrap, CLI entry).
5. Identify build & tooling setup: bundler, linter/formatter config, CI/CD config files if present, test runner.
6. Write a one-paragraph hypothesis of "what this system does" — explicitly labeled as a hypothesis to be refined, not a final claim.

## Required Outputs
- `01-repository-intelligence.md` containing all of the above, organized per app/service if monorepo

## Validation Checklist
- [ ] Every top-level folder is accounted for (documented or explicitly excluded with reason)
- [ ] Every distinct language/framework combination found is listed exactly once, not duplicated across sections
- [ ] Entry points are verified against actual package.json/pyproject/etc. scripts, not guessed from file names alone
