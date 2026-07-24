# PHASE 8 — CONDITIONAL DOCUMENTATION

## Objective
Cover infrastructure-adjacent concerns that only apply to some repos — skip individually with a stated reason where not applicable.

## Subsections
1. **API Documentation** (if any routes/endpoints exist) — every route, method, auth requirement, request/response shape, status codes, rate limits if visible in code/config
2. **Database Documentation** (if a database is present) — schema, relationships, indexes, migration history if tracked in-repo
3. **Authentication Documentation** (if auth exists) — strategy used (session/JWT/OAuth/etc.), token lifecycle, permission/role model as implemented
4. **Deployment Documentation** (if deploy config exists) — build/deploy process from repo's own Docker/CI/CD/hosting config, not generic platform docs
5. **Environment Documentation** (if env vars are used) — every env var found, what it controls, required vs optional, default values if visible

## Required Outputs
- `08-conditional-docs/` — one file per applicable subsection; explicitly state "N/A — not present in this repo" for any that don't apply

## Validation Checklist
- [ ] Every documented endpoint/schema/env var traces to actual code/config, not framework defaults assumed
- [ ] N/A subsections are explicitly stated, not silently omitted
