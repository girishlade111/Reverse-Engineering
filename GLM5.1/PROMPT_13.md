# PROMPT_13.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_13: State Management Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_13
- **Phase:** 2
- **Stage:** 3 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-12 (PROMPT_12).
- **Estimated Tokens:** 12000–18000
- **Output Artifacts:** ART-13 (Doc) — State Machine Catalog.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the State Machine Catalog artifact (ART-13) that identifies every stateful unit in the subject repository (in-memory state, persisted state, distributed state), enumerates every state variable, reconstructs every state machine (states, transitions, guards, invariants, initial and terminal states), detects every state-mutation point, and detects every state-synchronization mechanism (locks, transactions, events).

---

## 3. When to Invoke

PROMPT_13 is dispatched when ALL of the following predicates hold:

- Phase 1 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.2.
- ART-01, ART-08, ART-09, ART-10, and ART-12 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-10's `field_access_edges` is non-empty OR ART-08 records at least one class with mutable fields OR ART-09 records at least one function with side-effect kind `state-mutation` (else `NO_STATEFUL_UNITS` and the prompt emits a minimal catalog with that finding).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-08 | Doc | Class catalog; classes with mutable fields are stateful-unit candidates. Class lifecycle methods (`onCreate`, `dispose`, etc.) seed state-machine reconstruction. |
| ART-09 | Doc | Function side-effect records; functions whose side effects include `state-mutation` or `state-read` are mutation-point and read-point candidates. |
| ART-10 | Graph | `field_access_edges` (`READS`/`WRITES`); these are the raw state-access events aggregated into state machines. |
| ART-12 | Graph | Control flow and execution paths; branches and exception paths provide the conditions (guards) under which state transitions occur. Concurrency primitives from ART-12 inform synchronization-mechanism detection (§ 6.5). |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid state-diagram conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). HOOK-03 (State Reachability) is enforced. |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Identify every stateful unit per § 6.1 by clustering mutable fields, module-level variables, singletons, caches, and persisted entities.
3. Enumerate every state variable per § 6.2 with its type, scope, and lifetime.
4. For every stateful unit, reconstruct the state machine per § 6.3 (states, transitions, guards, invariants, initial state, terminal states).
5. Detect every state-mutation point per § 6.4 by combining ART-10's `WRITES` edges with ART-09's `state-mutation` side effects.
6. Detect every state-synchronization mechanism per § 6.5 by combining ART-12's concurrency primitives with transaction patterns and event-synchronization patterns.
7. Identify initial and terminal states per § 6.6.
8. Reconstruct state invariants per § 6.7 from assertions, pre/post-conditions, and type constraints.
9. Emit Mermaid `stateDiagram-v2` diagrams per § 6.8 with transition-level citations.
10. Cross-check state-machine reachability per § 6.9 (every state reachable from the initial state; every terminal state reachable from some state).
11. Emit ART-13 per § 8 with full front-matter, per-stateful-unit state-machine sections, mutation-point catalog, synchronization-mechanism catalog, reachability cross-check, traceability index, open questions.
12. Run the Quality Checks in § 9.
13. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Stateful-Unit Identification

Identify stateful units by clustering the following sources:

- **Class instances with mutable fields** — every `K-XX` recorded in ART-08 with at least one non-`final`/non-`readonly`/non-`const` field is a stateful-unit candidate. The unit ID is `K-XX`.
- **Module-level variables** — every `V-XX` recorded in ART-06 (or implied by ART-09) at module scope that is written by at least one function. Each such variable, together with all variables it is grouped with in a singleton, factory, or store, forms a stateful unit.
- **Singletons** — detected by patterns: `Object.freeze({...})`, `getInstance()` (Java), `static readonly Instance`, `__new__` overriding in Python, Go `sync.Once`, Rust `OnceCell`/`Lazy`, Ruby `Singleton` module. Each singleton is a stateful unit.
- **Caches** — detected by cache-library usage (Redis client, Memcached, LRU cache, `lru-cache` npm, `functools.lru_cache`, `@functools.cache`, Go `groupcache`, Rust `moka`, C# `MemoryCache`). Each cache instance is a stateful unit with `kind: cache`.
- **Store objects** — detected by framework markers: Redux store (`createStore`, `configureStore`), Zustand store (`create`), Vuex/Pinia stores, MobX observables, NgRx store, Akita stores, Jotai/Recoil atoms, Valtio proxies, EF Core `DbContext`, Hibernate `Session`. Each store is a stateful unit.
- **Persisted entities** — entities written to or read from a persistence layer (forward reference: PROMPT_20's persistence catalog). Each entity-table pair is a stateful unit with `kind: persisted`.
- **Distributed state** — shared caches (Redis, Memcached), distributed lock services (etcd, Zookeeper, Consul), message-queue offsets, session stores. Each distributed-state instance is a stateful unit with `kind: distributed`.
- **Session state** — HTTP sessions, JWT state, refresh-token state. Detected by session middleware (forward reference: PROMPT_19's auth catalog).

Each stateful unit is assigned an `S-XX` identifier and records `unit_id`, `kind` (class | module-var | singleton | cache | store | persisted | distributed | session), `name`, `file:line-range`, `carried_variables` (list of `V-XX` or `K-XX` field IDs), `lifetime` (process | request | session | persistent | ephemeral), `external: false` for in-scope or `external: true` for managed services (e.g., a managed Redis).

### 6.2 State-Variable Enumeration

For every stateful unit, enumerate every state variable. Each variable records `var_id` `V-XX`, `name`, `type`, `scope` (instance | class-static | module | process | distributed), `lifetime`, `default_value` (from initializer or `UNVERIFIED`), `mutability` (mutable | readonly-after-init | write-once), `serialized: true|false` (whether the variable is persisted or marshaled), `citations` (`file:line-range`).

For composite types (objects, structs, records), the variable's `subfields` are enumerated when the type is in-scope (per ART-08); for external types, the subfield enumeration is `UNVERIFIED` and an Open Question is recorded.

### 6.3 State-Machine Reconstruction

For every stateful unit, reconstruct the state machine. A state machine is a tuple `(S, T, G, s_0, S_t, I)` where `S` is the set of states, `T` is the set of transitions, `G` is the set of guards (conditions on transitions), `s_0` is the initial state, `S_t` is the set of terminal states, and `I` is the set of invariants.

**State identification procedure:**

1. Identify explicit state fields — variables named `state`, `status`, `phase`, `stage`, `lifecycle`, `_state`, or variables whose type is a finite enum/union/sum-type. The state's value set is the enum's variants.
2. Identify implicit state fields — variables whose value range is constrained by the code to a small finite set (detected by inspecting branch conditions per ART-12 that compare the variable to constants). The value range is the inferred state set.
3. Identify derived states — combinations of multiple variables that together constitute a logical state (e.g., `isAuthenticated && !isExpired` constitutes `ACTIVE`). Derived states are recorded with their composition predicate.
4. Identify lifecycle states — phases of an entity's existence: `CREATED`, `INITIALIZED`, `RUNNING`, `STOPPED`, `DISPOSED` (for objects); `PENDING`, `PROCESSING`, `COMPLETED`, `FAILED` (for tasks); `DRAFT`, `PUBLISHED`, `ARCHIVED` (for content entities). Lifecycle states are detected by examining state transitions and constructor/destructor calls.

Each state is assigned an `S-XX` identifier (reusing the `S-` prefix from `PROJECT_SPECIFICATION.md` § 4.1; the stateful unit and the state share the prefix but are distinguished by the `kind` field: `stateful-unit` vs `state`).

**Transition identification procedure:**

1. For every state-mutation point per § 6.4, identify the source state (the state before the mutation) and the target state (the state after).
2. Identify the guard — the condition that must hold for the transition to fire. Guards come from ART-12's branches whose `taken_targets` include the mutation function.
3. Identify the action — the side effects performed during the transition (other state mutations, event emissions, external calls). Actions are recorded as `act_XX` entries citing the side-effect line.
4. Identify the trigger — what causes the transition: a method call (name the method), an event (name the event), a timer (cite the timer), a condition becoming true (cite the polling loop).

Each transition is recorded with `transition_id` `TR-XX`, `from_state` `S-XX`, `to_state` `S-XX`, `trigger_kind` (method | event | timer | condition), `trigger_symbol`, `guard` (the condition text and citation), `actions` (list of `act_XX`), `citation`.

### 6.4 State-Mutation-Point Detection

Detect every state-mutation point by combining ART-10's `field_access_edges` of kind `WRITES` with ART-09's `state-mutation` side effects. Each mutation point records `mut_id` `MUT-XX`, `function_id` `FN-XX`, `target_variable` `V-XX`, `target_unit` `S-XX` (the stateful unit), `mutation_kind` (assign | increment | collection-add | collection-remove | collection-clear | object-merge | field-update | reference-reassign), `citation` (`file:line-range`), `pre_state` and `post_state` (when the mutation is a state-machine transition; otherwise `NA`).

Mutations are classified as `internal` (the mutator is a method of the same class as the variable) or `external` (the mutator is a free function or a method of a different class). External mutations of `readonly-after-init` fields are flagged as `INVARIANT_VIOLATION_CANDIDATE` and recorded as Open Questions.

### 6.5 Synchronization-Mechanism Detection

Detect every state-synchronization mechanism. Synchronization mechanisms ensure that concurrent access to a stateful unit is safe. Each mechanism records `sync_id` `SYN-XX`, `kind` (lock | transaction | event | stm | crdt | atomic | immutable-snapshot), `protected_unit` `S-XX`, `acquired_at` `FN-XX`, `released_at` `FN-XX`, `citation`, `strategy` (pessimistic | optimistic | none).

**Detection patterns:**

- **Locks** — every `CP-XX` of kind `mutex`/`rwlock`/`semaphore` from ART-12 that protects a `V-XX` or `S-XX` (per ART-12's `protects` field). The lock's acquire/release sites are the synchronization mechanism's acquire/release sites.
- **Transactions** — database transactions detected by `BEGIN`/`COMMIT`/`ROLLBACK` SQL, `connection.beginTransaction()`, `TransactionScope` (C#), `@Transactional` (Java/Spring), `with transaction.atomic():` (Django), `db.transaction(fn)` (Node), `sql.Tx` (Go), `tokio_postgres::Transaction` (Rust). Each transaction records its isolation level when declared (read-uncommitted | read-committed | repeatable-read | serializable) and its propagation behavior (REQUIRED | REQUIRES_NEW | NESTED | etc., for Spring).
- **Events** — event-based synchronization where one state change emits an event that triggers another state change. Cross-reference PROMPT_14's event catalog (forward reference).
- **STM (Software Transactional Memory)** — `clojure.core/dosync`, Haskell `STM` monad, Scala `scala-stm`. Each STM transaction records its retry policy.
- **CRDTs** — `Y.js`, `Automerge`, `riak_dt`, Redis `bitmap`/`hyperloglog`. Each CRDT records its merge semantics.
- **Atomics** — atomic variables from ART-12's `CP-XX` of kind `atomic`.
- **Immutable snapshots** — patterns where state is replaced rather than mutated: Redux reducers returning new state objects, persistent data structures (`immer`, `Immutable.js`, `persistent-scheduler`, Rust `im` crate, Clojure `persistent` collections). Each snapshot records the copy-on-write site.

### 6.6 Initial and Terminal State Identification

For every state machine, identify the initial state and the terminal states.

**Initial state identification:**

1. Examine the stateful unit's constructor / initializer / factory / `init` method (per ART-08's lifecycle methods).
2. The state variable's value immediately after initialization is the initial state.
3. If the initial value depends on configuration (env vars, config files), the initial state is `CONFIG_DEPENDENT` with the config key cited.
4. If the initial value depends on a database read (for persisted units), the initial state is `LOADED_FROM_PERSISTENCE` with the load function cited.

**Terminal state identification:**

1. Examine the stateful unit's destructor / `dispose` / `close` / `Drop` / `teardown` method.
2. The state variable's value immediately before destruction is a terminal state.
3. For state machines with explicit terminal states (e.g., `COMPLETED`, `FAILED`, `CANCELLED`), the terminal state is the state with no outgoing transitions.
4. For lifecycle state machines, `DISPOSED`/`DESTROYED`/`CLOSED` are terminal states.

### 6.7 Invariant Reconstruction

Reconstruct invariants for every stateful unit. Invariants are conditions that must hold in every stable state (between transitions). Invariants are reconstructed from:

1. **Assertions** — `assert` statements (Python), `assert` (Java), `debug_assert!`/`assert!` (Rust), `Debug.Assert` (C#), `AssertionError` raises. Each assertion that involves a state variable is a candidate invariant.
2. **Pre/post-conditions** — `@pre`, `@post` Javadoc tags; Eiffel-style `require`/`ensure`; Rust contracts crate; Code contracts (C#). Each pre/post-condition that involves state is a candidate invariant.
3. **Type constraints** — types that encode invariants: `NonZeroU32` (Rust), `NonNullable` (C#), refined types, branded types (TypeScript `type UserId = string & { __brand: 'UserId' }`).
4. **Validation in setters** — setter methods that validate the new value (e.g., `setAge(int age) { if (age < 0) throw ...; this.age = age; }`). The validation predicate is the invariant.
5. **State-machine guards** — guards that prevent invalid transitions encode invariants about reachable states.

Each invariant is recorded with `inv_id` `INV-XX`, `unit_id` `S-XX`, `predicate_text`, `predicate_citation`, `enforcement` (assertion | type-system | runtime-check | guard | unenforced), `citations`.

### 6.8 Mermaid State-Diagram Emission

Emit Mermaid `stateDiagram-v2` diagrams per `OUTPUT_RULES.md` § 7. Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

- **Per-stateful-unit state diagram** — one diagram per stateful unit with `≥ 2` states. States rendered as `S01: <name>`; transitions rendered as `S01 --> S02: <trigger> [guard]`. Each transition carries an `edge: file:line` comment.
- **Composite state diagrams** — when a stateful unit has nested states (e.g., `RUNNING` contains `PROCESSING` and `IDLE`), use Mermaid's `state S01 { ... }` composite syntax.
- **Master state-machine index** — a single diagram listing all stateful units as nodes with edges to their state diagrams; decomposed when > 30 units.

Edge styles: solid for normal transitions, dashed for event-triggered transitions, dotted for timer-triggered transitions, thick for terminal transitions.

### 6.9 Reachability Cross-Check

Cross-check state-machine reachability per HOOK-03 (State Reachability):

1. For every state machine, perform a BFS from the initial state.
2. Every state must be reachable from the initial state; unreachable states are `UNREACHABLE_STATE` findings recorded in Open Questions.
3. Every non-initial state must reach at least one terminal state (when terminal states exist); states that cannot reach any terminal state are `LIVELOCK_CANDIDATE` findings.
4. For state machines without terminal states (e.g., a long-running cache), the check is `NA` and recorded as such.
5. For state machines with multiple initial states (e.g., loaded-from-persistence with multiple possible initial values), each initial state's reachability is checked separately.

---

## 7. Required Outputs

### ART-13 — State Machine Catalog

**Type:** Doc.

**Acceptance Criteria:**

- AC-13.1: The artifact file exists at `<output_root>/artifacts/phase2/ART13_<engagement_id>_state-machines.md`.
- AC-13.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5 (Doc schema).
- AC-13.3: The body contains: Executive Summary, Methodology, Stateful-Unit Catalog, Per-Unit State Machines, Mutation Points, Synchronization Mechanisms, Initial and Terminal States, Invariants, Reachability Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-13.4: Every state, transition, mutation, synchronization, and invariant cites its source.
- AC-13.5: Every Mermaid `stateDiagram-v2` is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-13.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-13.7: Every stateful unit has at least one state or is recorded `STATELESS` with rationale.
- AC-13.8: Reachability cross-check findings are recorded; `UNREACHABLE_STATE` and `LIVELOCK_CANDIDATE` are flagged.

---

## 8. Output Templates

### 8.1 ART-13 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-13
artifact_type: Doc
producing_prompt: PROMPT_13
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
stateful_units:
  - id: S-01
    kind: class | module-var | singleton | cache | store | persisted | distributed | session
    name: <name>
    file: <path>
    line_range: <start-end>
    carried_variables: [V-XX]
    lifetime: process | request | session | persistent | ephemeral
    external: false
state_variables:
  - id: V-01
    unit_id: S-XX
    name: <name>
    type: <type>
    scope: instance | class-static | module | process | distributed
    lifetime: <text>
    default_value: <value> | UNVERIFIED
    mutability: mutable | readonly-after-init | write-once
    serialized: true | false
    citations: [<file>:<line-range>]
state_machines:
  - unit_id: S-XX
    states:
      - id: S-01
        name: <name>
        kind: explicit | implicit | derived | lifecycle
        composition: <text> | NA
        citation: <file>:<line-range>
    transitions:
      - id: TR-01
        from_state: S-XX
        to_state: S-XX
        trigger_kind: method | event | timer | condition
        trigger_symbol: <name>
        guard: <text>
        guard_citation: <file>:<line-range>
        actions: [act-XX]
        citation: <file>:<line-range>
    initial_state: S-XX | CONFIG_DEPENDENT | LOADED_FROM_PERSISTENCE
    terminal_states: [S-XX]
    invariants: [INV-XX]
mutation_points:
  - id: MUT-01
    function_id: FN-XX
    target_variable: V-XX
    target_unit: S-XX
    mutation_kind: assign | increment | collection-add | collection-remove | collection-clear | object-merge | field-update | reference-reassign
    citation: <file>:<line-range>
    pre_state: S-XX | NA
    post_state: S-XX | NA
    classification: internal | external
synchronization_mechanisms:
  - id: SYN-01
    kind: lock | transaction | event | stm | crdt | atomic | immutable-snapshot
    protected_unit: S-XX
    acquired_at: FN-XX
    released_at: FN-XX
    citation: <file>:<line-range>
    strategy: pessimistic | optimistic | none
    isolation_level: read-uncommitted | read-committed | repeatable-read | serializable | NA
    propagation: REQUIRED | REQUIRES_NEW | NESTED | NA
invariants:
  - id: INV-01
    unit_id: S-XX
    predicate_text: <text>
    predicate_citation: <file>:<line-range>
    enforcement: assertion | type-system | runtime-check | guard | unenforced
    citations: [<file>:<line-range>]
reachability_cross_check:
  - unit_id: S-XX
    unreachable_states: [S-XX]
    livelock_candidates: [S-XX]
    na: true | false
mermaid_sources:
  - diagram_id: D-01
    title: <text>
    sidecar_file: <relative-path>
    node_count: <int>
source_coverage:
  - path: <file_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <file_path>:<line-range>
    symbol: <name>
sections:
  - id: S-01
    title: <string>
    claims: [C-XX]
---
```

### 8.2 ART-13 Body Skeleton

```markdown
# ART-13: State Machine Catalog

## 1. Executive Summary
## 2. Methodology
## 3. Stateful-Unit Catalog
## 4. Per-Unit State Machines
   ### 4.1 S-01: <name>
   **Diagram D-01: S-01 State Machine**
   ```mermaid
   stateDiagram-v2
       [*] --> S01: CREATED
       S01: CREATED
       S02: RUNNING
       S03: STOPPED
       S01 --> S02: start() [isInitialized]
       S02 --> S03: stop()
       S03 --> [*]
       %% edge: src/lifecycle.ts:42
   ```
   <state, transition, invariant list>
## 5. Mutation Points
## 6. Synchronization Mechanisms
## 7. Initial and Terminal States
## 8. Invariants
## 9. Reachability Cross-Check
## 10. Traceability Index
## 11. Open Questions
## 12. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every stateful unit identified per § 6.1 has a state machine or `STATELESS` rationale; threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of states, transitions, mutations, synchronizations, and invariants cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no state-machine transition contradicts ART-10's `WRITES` edges or ART-09's `state-mutation` side effects.
- **Q5. UNVERIFIED Accounting** — every `UNREACHABLE_STATE`, `LIVELOCK_CANDIDATE`, `INVARIANT_VIOLATION_CANDIDATE`, and `STATELESS` entry has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.3 on a 5% sample of units yields the same state set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-13.A. Initial-State Presence** — every state machine has an `initial_state` or `CONFIG_DEPENDENT`/`LOADED_FROM_PERSISTENCE` with rationale.
- **Q-13.B. Mutation Coverage** — every `WRITES` edge in ART-10 corresponds to a `MUT-XX` entry (modulo field-access edges for non-stateful fields).
- **Q-13.C. Synchronization Coverage** — every `CP-XX` lock/semaphore/mutex in ART-12 protecting a stateful unit has a corresponding `SYN-XX` entry.
- **Q-13.D. Reachability Check (HOOK-03)** — every state is reachable from the initial state; every state reaches a terminal state (or the unit is `no-terminal`).
- **Q-13.E. Mermaid Transition Citation** — every transition in the Mermaid diagrams has an `edge: file:line` comment.
- **Q-13.F. Sidecar Files** — every Mermaid block has a `.mmd` sidecar file.
- **Q-13.G. Invariant Enforcement** — every invariant with `enforcement: unenforced` has an Open Question documenting the risk.

---

## 10. Common Pitfalls

- Do not infer state machines from variable names alone; verify the state set by inspecting assignments and branch conditions per R22.
- Always cite the guard for every transition; a transition without a guard citation is incomplete.
- Do not conflate the stateful unit's `S-XX` identifier with the state's `S-XX` identifier; both share the prefix but are distinguished by the `kind` field (`stateful-unit` vs `state`).
- Always record the initial state; an initial-state-less machine is non-conformant per Q-13.A.
- Do not omit terminal states for long-running state machines (caches, servers); record `no-terminal` explicitly rather than leaving the field empty.
- Always cross-check mutation points against ART-10's `WRITES` edges; a discrepancy is a `CONTRADICTION` per R33.
- Do not infer invariants from comments; only invariants enforced by code (assertions, type constraints, runtime checks, guards) are recorded per R22.
- Always record the synchronization mechanism's isolation level for transactions; omitting the isolation level leaves the concurrency model underspecified.
- Do not record derived states without recording their composition predicate; a derived state without a predicate is non-reproducible.
- Always emit `.mmd` sidecar files for state diagrams; PROMPT_25 re-renders diagrams from the sidecar source.
- Do not collapse multiple state machines in a single stateful unit into one; if the unit has multiple independent state variables, emit multiple state machines.

---

## 11. Handoff Criteria

PROMPT_14, PROMPT_17, and PROMPT_25 consume ART-13. Handoff requires ALL of:

- HC-13.1: ART-13 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-13.2: Every stateful unit identified per § 6.1 has a state machine or `STATELESS` rationale.
- HC-13.3: Every state machine has an initial state and (where applicable) terminal states.
- HC-13.4: Mutation points, synchronization mechanisms, and invariants are cataloged.
- HC-13.5: Reachability cross-check is recorded with no unresolved `UNREACHABLE_STATE` or `LIVELOCK_CANDIDATE` findings (or each is an Open Question with rationale).
- HC-13.6: Mermaid `stateDiagram-v2` diagrams are emitted with `.mmd` sidecar files.
- HC-13.7: `repository_fingerprint_recheck` matches ART-01.
- HC-13.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_14 (Event Workflow — uses state transitions to identify event triggers and event-driven workflows), PROMPT_17 (Error Handling — uses state machines to identify error states and recovery transitions), PROMPT_25 (Diagram Generation — re-renders state diagrams at higher visual fidelity).
- **Depends on:** ART-01 (PROMPT_01), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-12 (PROMPT_12).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). HOOK-03 (State Reachability) is enforced by PROMPT_30.
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 7.
- **Forward reference:** PROMPT_30 verifies that every stateful unit referenced by ART-14, ART-17, or ART-18 resolves to an entry in ART-13 and that HOOK-03 reachability checks pass for every state machine.

*End of PROMPT_13. Orchestrator may dispatch PROMPT_14 upon satisfaction of § 11.*
