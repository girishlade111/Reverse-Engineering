# PROMPT_12.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_12: Control Flow & Execution Path Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_12
- **Phase:** 2
- **Stage:** 2 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-09 (PROMPT_09), ART-10 (PROMPT_10).
- **Estimated Tokens:** 13000–19000
- **Output Artifacts:** ART-12 (Graph) — Control Flow & Execution Path Maps.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Control Flow & Execution Path Maps artifact (ART-12) that reconstructs every execution path from each entry point in ART-05 through the call graph in ART-10, identifies decision points, branches, loops, exception paths, and async boundaries, detects every concurrency primitive, identifies critical paths (most-traveled routes) and rarely-traveled paths (error handlers, fallbacks), and emits Mermaid flowcharts with edge-level citations.

---

## 3. When to Invoke

PROMPT_12 is dispatched when ALL of the following predicates hold:

- Phase 1 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.2.
- ART-01, ART-05, ART-09, and ART-10 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-05 records at least one entry point with `trace_status: COMPLETE` (else the execution-path catalog is trivial and recorded as `NO_ENTRY_COMPLETE`).
- ART-10's call graph node count is greater than zero (else `EMPTY_CONTROL_FLOW`).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-05 | Map | Entry-point catalog and bootstrap traces; the first function of every bootstrap trace is the root of an execution-path tree. |
| ART-09 | Doc | Function reference; per-function complexity, async/await flags, exception declarations, and side-effect records drive branch and exception-path detection. |
| ART-10 | Graph | Call graph; provides the topology along which execution paths propagate. Critical paths from ART-10 seed the critical-path identification in § 6.6. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid flowchart conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Graph schema (`§ 4.4`) and type-specific bar (aggregate ≥ 30, Coverage ≥ 4, Coherence ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. For every entry point in ART-05 with `trace_status: COMPLETE`, enumerate execution paths per § 6.1 bounded by 30 hops.
3. Detect decision points (branches) per § 6.2 within every function along every path.
4. Detect loops per § 6.3 within every function along every path.
5. Trace exception paths per § 6.4 using ART-09's `throws/raises` declarations and language-specific catch patterns.
6. Identify async boundaries per § 6.5 (promises, callbacks, coroutines, fibers, channels, streams).
7. Detect concurrency primitives per § 6.6 (locks, mutexes, semaphores, channels, async/await, threads, actors).
8. Identify critical paths per § 6.7 by combining ART-10's critical paths with branch fan-in.
9. Identify rarely-traveled paths per § 6.8 (error handlers, fallbacks, default branches, dead-end handlers).
10. Emit Mermaid `flowchart TD` diagrams per § 6.9 with edge-level citations; decompose diagrams > 30 nodes by entry point.
11. Cross-check the execution-path node set against ART-10's reachability set per § 6.10; unaccounted nodes are `CONTRADICTION` findings per R33.
12. Emit ART-12 per § 8 with full front-matter, per-entry-point path sections, concurrency catalog, critical and rarely-traveled path registers, traceability index, open questions.
13. Run the Quality Checks in § 9.
14. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Execution-Path Enumeration

For every entry point in ART-05 with `trace_status: COMPLETE`, enumerate execution paths from the entry-point root function through ART-10's call graph. The enumeration procedure uses a bounded DFS with cycle detection:

1. Seed the DFS with `(entry_root_FN, hops=0, path=[entry_root_FN])`.
2. For every node, look up the function body (per ART-09's `file`/`line_range`) and detect branches per § 6.2; for each branch, create a child path with the branch condition cited.
3. For every call edge in ART-10 from the current node, recurse into the callee if `hops < 30` and the callee is not already on the current path (cycle guard).
4. For every loop detected per § 6.3, record the loop as a self-edge on the path node with the loop condition cited; do not unroll the loop in the path (the path is a call-chain abstraction, not a control-flow graph in the classical sense).
5. Terminate the path when reaching a leaf function (per ART-10's `leaf_functions` list), a terminal marker (per ART-05), or the hop budget.
6. Record every enumerated path with `path_id` `EP-XX-<seq>`, `entry_id` `EP-XX`, `root_FN` `FN-XX`, `terminal_kind` (leaf | terminal-marker | hop-budget-exhausted | cycle-truncated), `nodes` (ordered list of `FN-XX`), `branches` (list of branch conditions encountered with citations), `loops` (list of loop nodes with citations), `exception_handlers` (list of handler nodes), `async_boundaries` (list of boundary nodes), `path_citations` (citation for every edge).

For large call graphs (node_count > 1000), the agent MAY sample entry points rather than enumerating every entry; the sampling procedure is documented in the methodology section. Sampling prioritizes entries whose bootstrap trace in ART-05 touches high-fan-out functions (top 10% by `out_degree` per ART-10).

### 6.2 Branch (Decision-Point) Detection

Within every function along every path, detect branches by language-specific control-flow syntax. Each branch is recorded with `branch_id` `BR-XX`, `function_id` `FN-XX`, `kind` (if-then | if-then-else | switch-case | ternary | match | guard | try-catch-dispatch | null-coalesce | short-circuit-and | short-circuit-or), `condition_text` (the source text of the condition, ≤ 200 characters), `condition_citation` (`file:line-range`), `taken_targets` (list of `FN-XX` called in each branch), `branch_count` (number of distinct branches).

**Per-language branch syntax:**

- C-family (TS/JS/Java/C#/C++/Go/Rust/Swift): `if`, `else if`, `else`, `switch`/`match`/`select`, ternary `?:`, short-circuit `&&`/`||`, null-coalescing `??` (TS/JS/C#).
- Python: `if`, `elif`, `else`, `match` (3.10+), ternary `x if cond else y`, short-circuit `and`/`or`.
- Ruby: `if`, `elsif`, `else`, `unless`, `case`/`when`, ternary `?:`, modifier forms `x if cond`.
- PHP: `if`, `elseif`, `else`, `switch`, `match` (8+), ternary `?:`, null-coalescing `??`.
- Elixir/Clojure: `cond`, `case`, `if`, multi-clause function heads (recorded as branches on the function).

Short-circuit operators (`&&`, `||`, `and`, `or`) are recorded as branches because they conditionally evaluate the right-hand side; the right-hand side is a `taken_target` only when the operator short-circuits to it.

### 6.3 Loop Detection

Within every function along every path, detect loops. Each loop is recorded with `loop_id` `LP-XX`, `function_id` `FN-XX`, `kind` (for | while | do-while | for-in | for-of | foreach | iterator | recursion-as-loop | stream-pipeline | generator-yield), `condition_citation` (`file:line-range`), `body_calls` (list of `FN-XX` called in the loop body), `max_iterations_estimated` (when statically derivable: `arr.length`, `range(n)`, etc.; otherwise `UNVERIFIED`), `exit_conditions` (the conditions under which the loop terminates; for iterators, the iterator exhaustion).

Recursion-as-loop is recorded when a function calls itself (per ART-09's recursion classification); the recursion's `max_iterations_estimated` is `UNVERIFIED` unless bounded by a parameter whose value range is statically known. Stream pipelines (e.g., `arr.map().filter().reduce()` in JS, `Stream.filter().map()` in Java, `iterator.map().filter()` in Rust) are recorded as loops because they iterate over a collection.

### 6.4 Exception-Path Tracing

Trace exception paths from every function along every path. The trace combines static and inferred exception sources:

1. **Declared exceptions** — read from ART-09's `throws/raises` field; for Java `throws X`, Python `raises X`, Rust `Result<T, E>`, Swift `throws`, C# `throw`. Each declared exception type is a potential thrown exception at any call site of the function.
2. **Implicit exceptions** — language-runtime exceptions that any code can throw (e.g., `NullPointerException`, `TypeError`, `IndexError`, `KeyError`, `Option.unwrap()` panic, `unwrap()` on `None`, division by zero). Implicit exceptions are recorded when the function body contains a syntactic pattern that implies them (e.g., `obj.method()` implies potential NPE; `arr[i]` implies potential `IndexError`; `x / y` implies potential divide-by-zero).
3. **Catch sites** — detected by `try`/`catch` (C-family), `try`/`except` (Python), `rescue` (Ruby), `catch`/`rescue` (Rust), `begin/rescue` (Ruby), `do-catch` (Swift), `@try/@catch` (Java). Each catch site records `handler_id` `EH-XX`, `caught_types` (list), `handler_FN` (the function or block that handles), `citation`, `propagates` (true if the handler re-throws; false if it absorbs).
4. **Propagation** — exceptions not caught at a call site propagate up the call chain; the trace follows the propagation up the path until a catch site is reached or the entry point is exited (uncaught exception → process termination).

Each exception path is recorded with `exc_path_id` `EXP-XX`, `entry_id` `EP-XX`, `thrown_at` `FN-XX`, `thrown_type`, `caught_at` `EH-XX` (or `UNCAUGHT`), `propagation_chain` (list of `FN-XX` the exception propagates through), `citations`.

### 6.5 Async-Boundary Identification

Identify every async boundary along every path. An async boundary is a point where control transfers between synchronous and asynchronous execution, or between two asynchronous contexts. The categories are:

- **Promise/callback creation** — `new Promise()`, `Promise.resolve()`, `Promise.reject()`, callback registration (`fs.readFile(path, cb)`), `setTimeout(cb)`, `setInterval(cb)`, `setImmediate(cb)`, `process.nextTick(cb)`, `queueMicrotask(cb)`.
- **Await suspension/resumption** — `await expr` (JS/TS), `await expr` (C#), `await` (Python ≥ 3.5), `.await` (Rust), `suspend fun` calls (Kotlin), `async let` (Swift). Each `await` is a suspension point where the coroutine yields control; resumption is non-deterministic relative to other coroutines.
- **Coroutine creation** — `async function` calls, `async def` calls, `go func()`, `tokio::spawn()`, `Task.Run()`, `DispatchQueue.async`, `Future::spawn`. The created coroutine runs concurrently with the caller.
- **Channel operations** — `chan <- x` (Go send), `<-chan` (Go receive), `mpsc::channel()` (Rust), `tokio::sync::mpsc`, `channel.send()`/`channel.recv()` (Python `asyncio`), `Channel<T>` (C#).
- **Stream consumption** — `for await ... of` (JS/TS), `Stream.forEach` (Java), `flow.collect` (Kotlin), `for msg in chan` (Go), `Stream.ReadAsync` (C#).
- **Fiber/green-thread boundaries** — `Fiber.new` (Ruby), `go` statement (Go), `tokio::task::spawn` (Rust), `goroutine` boundaries.

Each async boundary is recorded with `async_id` `AB-XX`, `kind`, `function_id` `FN-XX`, `citation`, `suspends_caller` (true for `await`, false for spawn), `resumes_at` (citation of the resumption point, when statically derivable).

### 6.6 Concurrency-Primitive Detection

Detect every concurrency primitive in the subject repository. The catalog is exhaustive: any primitive used to coordinate access to shared state or to schedule concurrent work. Each primitive is recorded with `conc_id` `CP-XX`, `kind`, `symbol`, `file:line-range`, `protects` (the `V-XX` or `S-XX` it guards, when statically derivable), `acquired_at` (list of `FN-XX` that acquire it), `released_at` (list of `FN-XX` that release it).

**Primitive catalog by language/ecosystem:**

- **Mutex/Lock** — `sync.Mutex` (Go), `std::sync::Mutex` (Rust), `Lock` (C#), `ReentrantLock` (Java), `threading.Lock` (Python), `Mutex` (Ruby `thread`), `synchronized` block (Java), `@synchronized` (Obj-C), `lock` statement (C#).
- **RWLock** — `sync.RWMutex` (Go), `RwLock` (Rust/C#), `ReentrantReadWriteLock` (Java), `threading.RLock` (Python).
- **Semaphore** — `SemaphoreSlim` (C#), `Semaphore` (Java), `asyncio.Semaphore` (Python), `tokio::sync::Semaphore` (Rust), `golang.org/x/sync/semaphore`.
- **Channel** — Go channels, Rust `mpsc`/`oneshot`/`broadcast`, Python `asyncio.Queue`, Java `BlockingQueue`, C# `Channel<T>`.
- **Atomic** — `sync/atomic` (Go), `AtomicXxx` (Java), `Interlocked` (C#), `std::sync::atomic` (Rust), `atomics` (Python via `threading`).
- **Once** — `sync.Once` (Go), `OnceCell`/`once_cell` (Rust), `Lazy<T>` (C#), `init` package patterns (Java `static final` initialization).
- **WaitGroup/Barrier** — `sync.WaitGroup` (Go), `CyclicBarrier`/`CountDownLatch` (Java), `Barrier` (C#), `tokio::task::JoinSet`.
- **Async runtime** — `tokio::main`, `asyncio.run`, `tokio::runtime::Runtime`, `Runtime::new()`, `Task.Run`(.NET ThreadPool), `DispatchQueue.global()`, Go's per-goroutine scheduler.
- **Actor frameworks** — Akka (Java/Scala), Erlang/Elixir processes, `actix` (Rust), `Microsoft Orleans`.

### 6.7 Critical-Path Identification

Identify critical paths — the most-traveled routes through the system. The procedure combines ART-10's pre-computed critical paths with branch fan-in:

1. Take ART-10's `critical_paths` list (entry-to-leaf chains through high-traffic hubs).
2. For each path, compute the branch fan-in: the number of distinct branches (per § 6.2) across the path's nodes whose `taken_targets` include nodes on the path. Higher fan-in indicates the path is the convergence point of many branches.
3. Score each path by `score = betweenness_sum + 0.5 * branch_fan_in` (combining ART-10's betweenness centrality with branch convergence).
4. Rank the top-N (N = min(15, total paths)).
5. Each critical path is recorded with `path_id` `CRP-XX`, `entry_id`, `path_nodes`, `betweenness_sum`, `branch_fan_in`, `score`, `path_citations`.

### 6.8 Rarely-Traveled Path Identification

Identify rarely-traveled paths — error handlers, fallbacks, default branches, and dead-end handlers that execute only under specific conditions. The procedure:

1. Enumerate every `EH-XX` exception handler from § 6.4 that is reachable from at least one entry point but is not on any critical path.
2. Enumerate every branch (per § 6.2) whose `taken_target` is a fallback function (detected by name patterns: `fallback`, `default`, `onError`, `recover`, `handleError`, `safeDefault`, `gracefulDegradation`, `circuitFallback`).
3. Enumerate every default branch in a `switch`/`match`/`case` statement that does not match a primary case.
4. Enumerate every dead-end handler — a handler that logs and re-throws, or logs and returns a default value, terminating the path without performing the originally-requested work.

Each rarely-traveled path is recorded with `rare_path_id` `RTP-XX`, `entry_id`, `trigger_kind` (exception | fallback | default | dead-end), `trigger_citation`, `path_nodes`, `path_citations`. The register feeds PROMPT_17 (Error Handling & Resilience) for resilience-pattern classification.

### 6.9 Mermaid Flowchart Emission

Emit Mermaid `flowchart TD` diagrams per `OUTPUT_RULES.md` § 7. Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

- **Per-entry-point execution-path diagram** — one diagram per entry point. Nodes: `FN-XX` (rectangular), `BR-XX` (diamond for decisions), `EH-XX` (rounded for exception handlers), `AB-XX` (hexagon for async boundaries). Edges labeled with the branch condition (when applicable) and the citation (`file:line`).
- **Concurrency catalog diagram** — a single diagram showing every `CP-XX` primitive and the `FN-XX` that acquire/release it.
- **Critical-path overlay diagram** — one diagram per critical path highlighting the path's nodes in bold and dimming all other nodes.

Edge styles: solid for synchronous calls, dashed for async boundaries, dotted for exception propagation, thick for critical-path edges. Diagrams > 30 nodes are decomposed by entry point; the master index diagram maps entry points to sub-diagrams.

### 6.10 Reachability Cross-Check

Cross-check the execution-path node set against ART-10's reachability set:

1. Compute the set `P_12` of all `FN-XX` that appear in at least one enumerated execution path.
2. Compute the set `R_10` of all `FN-XX` reachable from any entry point per ART-10's reachability cross-check.
3. The expected relationship: `P_12 ⊆ R_10` (every path node is reachable) and `R_10 ⊆ P_12 ∪ {sampled_out}` (every reachable function appears on some path, modulo sampling).
4. Functions in `R_10 \ P_12` are `COVERAGE_GAP` findings (the function is reachable but no path was enumerated to it). When sampling was used (per § 6.1), record the gap as `SAMPLED_OUT` rather than `COVERAGE_GAP`.
5. Functions in `P_12 \ R_10` are `CONTRADICTION` findings per R33 (the path includes a function ART-10 says is unreachable).

---

## 7. Required Outputs

### ART-12 — Control Flow & Execution Path Maps

**Type:** Graph.

**Acceptance Criteria:**

- AC-12.1: The artifact file exists at `<output_root>/artifacts/phase2/ART12_<engagement_id>_control-flow.md`.
- AC-12.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.4.
- AC-12.3: The body contains: Executive Summary, Methodology, Per-Entry-Point Execution Paths, Branches, Loops, Exception Paths, Async Boundaries, Concurrency Primitives, Critical Paths, Rarely-Traveled Paths, Reachability Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-12.4: Every path edge cites its source line range.
- AC-12.5: Every Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-12.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-12.7: Every branch, loop, exception handler, async boundary, and concurrency primitive is cataloged.
- AC-12.8: Critical paths and rarely-traveled paths are enumerated with metrics.
- AC-12.9: Reachability cross-check is recorded with no unresolved contradictions.

---

## 8. Output Templates

### 8.1 ART-12 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-12
artifact_type: Graph
producing_prompt: PROMPT_12
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
execution_paths:
  - path_id: EP-01-001
    entry_id: EP-01
    root_fn: FN-XX
    terminal_kind: leaf | terminal-marker | hop-budget-exhausted | cycle-truncated
    nodes: [FN-XX]
    branches: [BR-XX]
    loops: [LP-XX]
    exception_handlers: [EH-XX]
    async_boundaries: [AB-XX]
    path_citations: [<file>:<line-range>]
branches:
  - id: BR-01
    function_id: FN-XX
    kind: if-then | if-then-else | switch-case | ternary | match | guard | try-catch-dispatch | null-coalesce | short-circuit-and | short-circuit-or
    condition_text: <text>
    condition_citation: <file>:<line-range>
    taken_targets: [FN-XX]
    branch_count: <int>
loops:
  - id: LP-01
    function_id: FN-XX
    kind: for | while | do-while | for-in | for-of | foreach | iterator | recursion-as-loop | stream-pipeline | generator-yield
    condition_citation: <file>:<line-range>
    body_calls: [FN-XX]
    max_iterations_estimated: <int> | UNVERIFIED
    exit_conditions: <text>
exception_paths:
  - id: EXP-01
    entry_id: EP-XX
    thrown_at: FN-XX
    thrown_type: <name>
    caught_at: EH-XX | UNCAUGHT
    propagation_chain: [FN-XX]
    citations: [<file>:<line-range>]
exception_handlers:
  - id: EH-01
    function_id: FN-XX
    caught_types: [<type>]
    handler_fn: FN-XX
    citation: <file>:<line-range>
    propagates: true | false
async_boundaries:
  - id: AB-01
    kind: promise | callback | await-suspend | await-resume | coroutine-spawn | channel-send | channel-recv | stream-consume | fiber
    function_id: FN-XX
    citation: <file>:<line-range>
    suspends_caller: true | false
    resumes_at: <file>:<line-range> | UNVERIFIED
concurrency_primitives:
  - id: CP-01
    kind: mutex | rwlock | semaphore | channel | atomic | once | waitgroup | barrier | async-runtime | actor
    symbol: <name>
    file: <path>
    line_range: <start-end>
    protects: [V-XX | S-XX]
    acquired_at: [FN-XX]
    released_at: [FN-XX]
critical_paths:
  - id: CRP-01
    entry_id: EP-XX
    path_nodes: [FN-XX]
    betweenness_sum: <float>
    branch_fan_in: <int>
    score: <float>
    path_citations: [<file>:<line-range>]
rarely_traveled_paths:
  - id: RTP-01
    entry_id: EP-XX
    trigger_kind: exception | fallback | default | dead-end
    trigger_citation: <file>:<line-range>
    path_nodes: [FN-XX]
    path_citations: [<file>:<line-range>]
reachability_cross_check:
  path_node_set_size: <int>
  reachable_set_size: <int>
  reachable_not_on_path: [FN-XX]
  on_path_not_reachable: [FN-XX]
  contradictions: [{ kind: <text>, fn_id: FN-XX, detail: <text> }]
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
nodes:
  - id: FN-01
    label: <name>
    kind: function
    parent_id: null
    depth: 0
edges:
  - from: FN-01
    to: FN-02
    relationship: CALLS
    evidence: <file_path>:<line-range>
layout_hint: TD
mermaid_source: |
  flowchart TD
      FN01[FN-01: main] --> FN02[FN-02: dispatch]
      FN02 --> BR01{BR-01: method?}
      BR01 -->|GET| FN03[FN-03: handleGet]
      BR01 -->|POST| FN04[FN-04: handlePost]
      %% edge: src/main.go:18
---
```

### 8.2 ART-12 Body Skeleton

```markdown
# ART-12: Control Flow & Execution Path Maps

## 1. Executive Summary
## 2. Methodology
## 3. Per-Entry-Point Execution Paths
   ### 3.1 EP-01: <name>
   **Diagram D-01: EP-01 Execution Paths**
   ```mermaid
   flowchart TD
       FN01[FN-01: main] --> FN02[FN-02: dispatch]
       FN02 --> BR01{BR-01: method?}
       BR01 -->|GET| FN03[FN-03: handleGet]
       BR01 -->|POST| FN04[FN-04: handlePost]
       %% edge: src/main.go:18
   ```
   <path list>
## 4. Branches
## 5. Loops
## 6. Exception Paths
## 7. Async Boundaries
## 8. Concurrency Primitives
   **Diagram D-02: Concurrency Catalog**
## 9. Critical Paths
   **Diagram D-03: Top Critical Path**
## 10. Rarely-Traveled Paths
## 11. Reachability Cross-Check
## 12. Traceability Index
## 13. Open Questions
## 14. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every entry point with `trace_status: COMPLETE` has at least one execution path; threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of path edges cited.
- **Q3. Schema Conformance Check** — validates against § 4.4.
- **Q4. Non-Contradiction Check** — no path node contradicts ART-10's call graph reachability.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` `max_iterations_estimated`, `resumes_at`, and `hop-budget-exhausted` path has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.1 on a 5% sample of entries yields the same path set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-12.A. Branch Coverage** — every branch recorded in ART-12 corresponds to a syntactic branch in the source; spot-check by re-reading the cited line range.
- **Q-12.B. Exception-Path Completeness** — every `FN-XX` with non-empty `throws/raises` in ART-09 has at least one exception path originating from it.
- **Q-12.C. Async-Boundary Coherence** — every `AB-XX` with `suspends_caller: true` is reachable only from an async function or coroutine (verified against ART-09's async classification).
- **Q-12.D. Concurrency Primitive Pairing** — every `CP-XX` of kind `mutex`/`rwlock`/`semaphore` has at least one `acquired_at` and at least one `released_at` (or an Open Question explaining the asymmetry).
- **Q-12.E. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment per `OUTPUT_RULES.md` § 7.5.
- **Q-12.F. Sidecar Files** — every Mermaid block has a corresponding `.mmd` file.
- **Q-12.G. Hop-Budget Enforcement** — no path exceeds 30 hops without being marked `hop-budget-exhausted` with an Open Question.
- **Q-12.H. Reachability Consistency** — `on_path_not_reachable` is empty OR every discrepancy is a recorded contradiction per R33.

---

## 10. Common Pitfalls

- Do not unroll loops into the path node list; the path is a call-chain abstraction, and unrolling creates combinatorial explosion.
- Always record both branches of an `if-then-else`; downstream prompts (PROMPT_17 Error Handling) consume the branch structure to identify error paths.
- Do not omit exception paths because the exception is "rare"; rare paths are first-class findings per § 6.8.
- Always distinguish `suspends_caller: true` (await) from `suspends_caller: false` (spawn); the distinction determines whether the caller continues concurrently.
- Do not infer concurrency primitives from variable names alone; verify the primitive's actual usage by reading the acquire/release sites.
- Always cite the loop condition; a loop without a condition citation is non-conformant.
- Do not conflate `try-catch-dispatch` branches with `if-then-else` branches; the dispatch table for catch clauses is a distinct branch kind.
- Always cross-check path reachability against ART-10; a path that includes an unreachable function is a contradiction per R33.
- Do not record short-circuit operators as linear control flow; `a && b()` conditionally evaluates `b()`, and the conditional must be recorded as a branch.
- Always cap path enumeration at 30 hops; unbounded enumeration exhausts the token budget per R29.
- Do not omit `.mmd` sidecar files; PROMPT_25 re-renders diagrams from the sidecar source.

---

## 11. Handoff Criteria

PROMPT_13, PROMPT_14, and PROMPT_17 consume ART-12. Handoff requires ALL of:

- HC-12.1: ART-12 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-12.2: Every entry point with `trace_status: COMPLETE` has at least one execution path.
- HC-12.3: Branches, loops, exception paths, async boundaries, and concurrency primitives are cataloged.
- HC-12.4: Critical paths and rarely-traveled paths are enumerated.
- HC-12.5: Mermaid diagrams are emitted with `.mmd` sidecar files; diagrams > 30 nodes are decomposed.
- HC-12.6: Reachability cross-check is recorded with no unresolved contradictions.
- HC-12.7: `repository_fingerprint_recheck` matches ART-01.
- HC-12.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_13 (State Management — uses execution paths to identify state mutations and synchronization points), PROMPT_14 (Event Workflow — uses async boundaries to identify event-driven workflows), PROMPT_17 (Error Handling — uses exception paths and rarely-traveled paths as the seed for resilience-pattern classification).
- **Depends on:** ART-01 (PROMPT_01), ART-05 (PROMPT_05), ART-09 (PROMPT_09), ART-10 (PROMPT_10).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.4; Graph bar (aggregate ≥ 30, Coverage ≥ 4, Coherence ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 7.
- **Forward reference:** PROMPT_30 verifies that every exception handler in ART-17 corresponds to an `EH-XX` in ART-12 and that every concurrency primitive is documented in both ART-12 (control flow) and ART-13 (state).

*End of PROMPT_12. Orchestrator may dispatch PROMPT_13 upon satisfaction of § 11.*
