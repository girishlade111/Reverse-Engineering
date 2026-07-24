# PHASE 4 — ARCHITECTURE RECONSTRUCTION

## Objective
Move from unit-level understanding to system-level understanding.

## Steps
1. **System Design Doc** — layered view (presentation / application / domain / infra or equivalent for the stack found), responsibilities per layer
2. **Component Map** — every major component/module and what it owns
3. **Module Map** — dependency direction between modules (who imports whom); flag any circular dependencies found
4. **Working Logic Documentation** — how a real request/action flows end-to-end through the system, in numbered prose (diagrams come in Phase 5, this is the prose backbone they'll be drawn from)
5. **Business Logic Documentation** — domain rules only: validation rules, permission rules, pricing/state-transition rules, anything encoding "how the business actually works" — kept clearly separate from technical/plumbing logic

## Required Outputs
- `04-architecture/` containing system-design.md, component-map.md, module-map.md, working-logic.md, business-logic.md

## Validation Checklist
- [ ] Every claimed business rule quotes (paraphrased) an actual code condition/branch
- [ ] Module map's dependency directions were verified from actual imports, not assumed from folder naming
- [ ] Working Logic doc traces at least the 2–3 most important user-facing flows end-to-end
