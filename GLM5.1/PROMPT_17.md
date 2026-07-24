# PROMPT_17.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_17: Error Handling & Resilience Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_17
- **Phase:** 2
- **Stage:** 7 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-09 (PROMPT_09), ART-12 (PROMPT_12), ART-13 (PROMPT_13), ART-16 (PROMPT_16).
- **Estimated Tokens:** 11000–17000
- **Output Artifacts:** ART-17 (Doc) — Error Handling & Resilience Report.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Error Handling & Resilience Report artifact (ART-17) that catalogs every error-handling mechanism in the subject repository (try/catch blocks, error boundaries, result types, panic/recover, circuit breakers, retries with backoff, timeouts, bulkheads, rate limiters, fallbacks, dead-letter queues), records for each mechanism what errors are caught, what recovery is attempted, what is logged, and what is propagated, identifies every error-propagation path, and detects every global error handler.

---

## 3. When to Invoke

PROMPT_17 is dispatched when ALL of the following predicates hold:

- Phase 1 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.2.
- ART-01, ART-09, ART-12, ART-13, and ART-16 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-09 records at least one function with non-empty `throws/raises` OR ART-12 records at least one `EH-XX` exception handler OR ART-12 records at least one rarely-traveled path with `trigger_kind: exception` (else `NO_ERROR_HANDLING` and the prompt emits a minimal report with that finding).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-09 | Doc | Function `throws/raises` declarations and side-effect records; functions that throw are error-emission points. |
| ART-12 | Graph | Exception handlers (`EH-XX`), exception paths (`EXP-XX`), and rarely-traveled paths with `trigger_kind: exception` or `dead-end`. These are the seed for error-handling catalog. |
| ART-13 | Doc | State-machine catalog; states with `FAILED`/`ERROR`/`CANCELLED` kinds and transitions from those states are recovery-flow candidates. |
| ART-16 | Doc | Middleware catalog; error-handling middleware and global error filters are detected here. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid flowchart conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect error handlers per § 6.1 (try/catch, error boundaries, result types, panic/recover, global handlers).
3. For every error handler, classify the recovery strategy per § 6.2 (rethrow, swallow, transform, retry, fallback, propagate).
4. Extract retry and timeout configurations per § 6.3 (retry counts, backoff, jitter, timeout durations).
5. Identify resilience patterns per § 6.4 (circuit breaker, bulkhead, rate limiter, fallback, dead-letter queue).
6. Trace error-propagation paths per § 6.5 from throw sites to catch sites to terminal handlers.
7. Detect global error handlers per § 6.6 (process-level, framework-level, domain-level).
8. Detect logging-at-error sites per § 6.7 (what is logged at each error site, log level, log structure).
9. Cross-reference state-machine recovery per § 6.8 using ART-13's `FAILED`/`ERROR` states.
10. Emit Mermaid flowchart diagrams per § 6.9 with error-path citations.
11. Cross-check the error-handler catalog against ART-12's `EH-XX` set per § 6.10; unaccounted handlers are `CONTRADICTION` findings per R33.
12. Emit ART-17 per § 8 with full front-matter, per-handler sections, resilience-pattern catalog, propagation-path catalog, global-handler catalog, traceability index, open questions.
13. Run the Quality Checks in § 9.
14. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Error-Handler Detection

Detect every error handler by scanning for catch patterns across all in-scope source files. Each handler records `handler_id` `HDL-XX`, `kind` (try-catch | error-boundary | result-match | panic-recover | global-process | global-framework | global-domain | middleware-error | rx-onError | promise-catch | async-catch), `function_id` `FN-XX` (the function containing the handler; for global handlers, the registration function), `caught_types` (list of exception types or `*`), `file:line-range`.

**Detection patterns:**

- **try/catch** — `try { ... } catch (e) { ... }` (C-family), `try { ... } except E: ...` (Python), `begin ... rescue E => e ... end` (Ruby), `catch { ... }` / `catch (...) { ... }` (C++), `do { try ... } catch { ... }` (Swift), `tokio::spawn(async { ... }).await.unwrap_or_else(|e| ...)` (Rust async).
- **Error boundaries** — React `class ErrorBoundary extends Component { componentDidCatch(...) }`, React `static getDerivedStateFromError(...)`, Vue `errorCaptured()` hook, Angular `ErrorHandler` class, Next.js `error.tsx` files.
- **Result-type matching** — Rust `match result { Ok(v) => ..., Err(e) => ... }`, `result.map_err(...)`, `result.unwrap_or(...)`, `result?` (propagation operator), Kotlin `Result<T>.fold(...)`, Swift `Result<Success, Failure>` matching, Haskell `Either`/`Maybe` matching, F# `Result` matching, Scala `Try`/`Either` matching, TypeScript `Result` discriminated-union matching (when the codebase defines its own Result type).
- **panic/recover** — Go `defer func() { if r := recover(); r != nil { ... } }()`, Rust `panic::catch_unwind(...)`, Lisp `handler-case`.
- **Global process handlers** — `process.on('uncaughtException', handler)` (Node.js), `process.on('unhandledRejection', handler)` (Node.js), `AppDomain.UnhandledException` (.NET), `Thread.setDefaultUncaughtExceptionHandler(...)` (Java), `sys.excepthook` (Python), `at_exit { ... }` (Ruby), `signal.SIGABRT` handlers.
- **Global framework handlers** — `@ControllerAdvice` + `@ExceptionHandler` (Spring), `app.use((err, req, res, next) => ...)` (Express 4-arg middleware), `app.setErrorHandler(handler)` (Fastify), `@Catch()` + `ExceptionFilter` (NestJS), `EXCEPTION_HANDLER` provider (NestJS), `ErrorHandler` middleware (ASP.NET Core), `config.exceptions_app = ...` (Rails), Django middleware `process_exception()`, Gin `Recovery()` middleware, Echo `e.HTTPErrorHandler`.
- **Rx/Promise error handlers** — RxJS `.subscribe(next, error, complete)`, `.catchError(...)`, `.retry(...)`, Promise `.catch(handler)`, `await promise.catch(...)`, Kotlin `CoroutineExceptionHandler`, Swift `Task` `await try ... catch`.
- **Middleware error handlers** — every stage from ART-16 with `concern: error-handling` is an error handler. The middleware's error-handling function is the handler.

### 6.2 Recovery-Strategy Classification

For every error handler, classify the recovery strategy:

- **Rethrow** — handler re-throws the caught exception (possibly after logging or augmenting). Detected by `throw e;`, `raise e`, `throw;`, `raise`, `return Err(e)`.
- **Swallow** — handler absorbs the exception without re-throwing. Detected by empty catch blocks, catch blocks that only log, catch blocks that return a default value. Flagged as `SWALLOWED_EXCEPTION` with severity MINOR or MAJOR depending on the exception type.
- **Transform** — handler catches one exception type and throws a different type. Detected by `catch (e) { throw new AppError(e) }`, `except ValueError as e: raise AppError(e)`. Records `transformed_from`, `transformed_to`, `transform_citation`.
- **Retry** — handler retries the failed operation. Detected by retry-loop patterns (cross-reference § 6.3). Records `retry_count`, `backoff_strategy`.
- **Fallback** — handler returns a fallback value or invokes a fallback function. Detected by `catch { return defaultValue; }`, `catch { return fallbackFn(); }`, `unwrap_or(default)`, `unwrap_or_else(fallback)`. Records `fallback_value` or `fallback_fn_id`.
- **Propagate** — handler propagates the exception to the caller without catching. Detected by functions with non-empty `throws/raises` that do not have a catch block for the thrown type.
- **Recover-with-state-change** — handler transitions the state machine to a `FAILED`/`ERROR` state (cross-reference ART-13). Records `from_state`, `to_state`.

Each handler records `recovery_strategy` (rethrow | swallow | transform | retry | fallback | propagate | recover-with-state-change | mixed), `recovery_details` (strategy-specific fields), `citation`.

### 6.3 Retry and Timeout Extraction

Extract every retry and timeout configuration:

- **Retry counts** — `maxRetries: 3`, `max_attempts: 5`, `retries: 2`, `MAX_RETRIES = 3`. Detected by configuration fields and constants matching `retry`/`retries`/`attempt` patterns.
- **Backoff strategy** — `exponential backoff`, `linear backoff`, `fixed delay`, `decorrelated jitter`. Detected by libraries (`p-retry`, `async-retry`, `tenacity`, `retrying`, `backoff`, `guava-retrying`, `resilience4j-retry`, `Polly Retry`). Records `initial_delay`, `max_delay`, `multiplier`, `randomization_factor`.
- **Jitter** — `full jitter`, `equal jitter`, `decorrelated jitter` (AWS-recommended patterns). Detected by `Math.random()` calls inside retry delay calculations.
- **Timeouts** — `timeout: 5000`, `deadline: Duration.ofSeconds(30)`, `context.WithTimeout(ctx, 30*time.Second)`, `tokio::time::timeout(duration, future)`, `Promise.race([op, timeout(5000)])`, `CancellationToken`/`Task.WhenAny`. Records `duration`, `unit`, `on_timeout` (cancel | return-default | throw-timeout-exception).
- **Cancellation** — `AbortController` (Web), `context.Context` (Go), `CancellationToken` (.NET), `Future::cancel` (Rust), `asyncio.CancelledError` (Python). Records `cancellation_propagation` (cooperative | forced | mixed).

Each retry/timeout configuration records `retry_id` `RT-XX`, `handler_id` `HDL-XX` (or `operation_id` `FN-XX` when the retry wraps a non-handler operation), `max_attempts`, `backoff_strategy`, `backoff_params`, `timeout_duration`, `on_timeout`, `citation`.

### 6.4 Resilience-Pattern Identification

Identify every resilience pattern:

- **Circuit breaker** — detected by libraries: `opossum` (Node.js), `resilience4j-circuitbreaker` (Java), `Polly CircuitBreaker` (.NET), `tenacity` (Python via `@retry(stop=...)`), `Hystrix` (Java, legacy), `ostrich` (Scala), `failsafe` (Java), `go-resilience` (Go). Each circuit records `breaker_id` `CB-XX`, `library`, `protected_operation` `FN-XX`, `failure_threshold`, `failure_rate_threshold`, `wait_duration_in_open_state`, `sliding_window_size`, `sliding_window_type` (count-based | time-based), `citation`.
- **Bulkhead** — detected by `@Bulkhead` (resilience4j), `BulkheadPolicy` (Polly), semaphore-based concurrency limits in handlers. Each bulkhead records `bulkhead_id` `BH-XX`, `max_concurrent_calls`, `max_wait_duration`, `protected_operation` `FN-XX`, `citation`.
- **Rate limiter** — every rate limit from ART-15's `rate_limits` catalog (cross-reference). Each rate limiter records `limiter_id` `RL-XX` (reusing ART-15's `RL-XX`), `limit`, `window`, `strategy`, `protected_scope` (global | per-API | per-tenant).
- **Fallback** — detected by `@Fallback`, `FallbackPolicy` (Polly), `recoverWith(...)`, `or_else(fallback)`. Each fallback records `fallback_id` `FB-XX`, `trigger_condition`, `fallback_value` or `fallback_fn_id`, `citation`.
- **Dead-letter queue (DLQ)** — detected by queue configurations with `deadLetterQueue` field (SQS), `x-dead-letter-exchange` (RabbitMQ), `deadLetterTopic` (Kafka), DLQ topics in pub/sub configs. Each DLQ records `dlq_id` `DLQ-XX`, `source_queue`, `dlq_target`, `trigger_condition` (max-receives | max-age | processing-failure), `max_receives`, `citation`.
- **Timeout** — every timeout configuration from § 6.3 is a resilience pattern; the catalog references `RT-XX`.

### 6.5 Error-Propagation-Path Tracing

Trace every error-propagation path — the path an exception takes from its throw site to its terminal handler:

1. Start from every throw site (functions in ART-09 with non-empty `throws/raises`, plus implicit throws detected per ART-12's exception paths).
2. Walk up the call graph (using `CALLED_BY` edges from ART-10) until a catch site (per § 6.1) is encountered.
3. At the catch site, determine the recovery strategy (per § 6.2): if `rethrow` or `transform`, continue propagating; if `swallow`/`fallback`/`recover-with-state-change`, terminate the path; if `propagate` (no catch), continue up.
4. If the path reaches the entry point without being caught, the exception is `UNCAUGHT` and the global handler (per § 6.6) is the terminal handler; if no global handler exists, the exception is `UNHANDLED` and triggers process termination.
5. Bound the path at 30 hops; deeper paths are marked `PROPAGATION_INCOMPLETE` with an Open Question.

Each propagation path records `prop_path_id` `PP-XX`, `thrown_at` `FN-XX`, `thrown_type`, `path` (ordered list of `FN-XX` the exception propagates through), `caught_at` `HDL-XX` or `UNCAUGHT`/`UNHANDLED`, `recovery_strategy_at_terminal`, `citations`.

### 6.6 Global-Error-Handler Detection

Detect every global error handler:

- **Process-level** — `process.on('uncaughtException')`, `process.on('unhandledRejection')`, `AppDomain.UnhandledException`, `Thread.setDefaultUncaughtExceptionHandler`, `sys.excepthook`, `at_exit`, `signal` handlers for `SIGABRT`/`SIGSEGV`. Each records `global_id` `GH-XX`, `scope: process`, `handler_fn` `FN-XX`, `citation`.
- **Framework-level** — `@ControllerAdvice` + `@ExceptionHandler` (Spring), `app.use((err, req, res, next) => ...)` (Express 4-arg), `app.setErrorHandler` (Fastify), `@Catch` + `ExceptionFilter` (NestJS), `ErrorHandler` middleware (ASP.NET Core), `config.exceptions_app` (Rails), Django `process_exception`, Gin `Recovery`, Echo `HTTPErrorHandler`. Each records `global_id` `GH-XX`, `scope: framework`, `framework`, `handler_fn` `FN-XX`, `caught_types`, `citation`.
- **Domain-level** — domain-specific global handlers, such as a saga's compensation handler, an aggregate's `onError` method, a workflow's `onFailure` hook. Each records `global_id` `GH-XX`, `scope: domain`, `domain`, `handler_fn` `FN-XX`, `citation`.

### 6.7 Logging-at-Error Site Detection

Detect every logging-at-error site — what is logged when an error occurs:

- For every error handler in § 6.1, inspect the catch block for logging calls: `logger.error(...)`, `log.error(...)`, `console.error(...)`, `print(..., file=sys.stderr)`, `Logging.LogError` (C#), `LOG(ERROR) << ...` (C++), `error_log(...)` (PHP), `Rails.logger.error`.
- Record the log level (ERROR | FATAL | WARN | CRITICAL), the log message structure (the format string or the structured fields), and whether the exception object is included (`include_exception: true|false`).
- Flag handlers that catch but do not log (`silent-catch`); flag handlers that log the exception object in plaintext (potential sensitive-data leak — cross-reference ART-11's sensitive-flow register).

Each logging-at-error site records `log_id` `LE-XX`, `handler_id` `HDL-XX`, `level`, `message_template`, `include_exception`, `structured_fields`, `citation`, `sensitive_data_warning: true|false`.

### 6.8 State-Machine Recovery Cross-Reference

Cross-reference state-machine recovery using ART-13:

1. For every state machine in ART-13 with states named `FAILED`, `ERROR`, `CANCELLED`, `DEAD`, `BROKEN`, or semantically equivalent, identify the transitions leading into and out of those states.
2. Transitions leading into a `FAILED`/`ERROR` state are triggered by exceptions (cross-reference ART-13's `trigger_kind: event` and the corresponding event emission in the catch block).
3. Transitions leading out of `FAILED`/`ERROR` are recovery transitions (retry, reset, escalate). Each recovery transition is cross-referenced to a `HDL-XX` handler.
4. State machines without recovery transitions from `FAILED`/`ERROR` are flagged `NO_RECOVERY_PATH`; the unit becomes stuck in the error state.

Each state-machine recovery records `sm_recovery_id` `SMR-XX`, `unit_id` `S-XX` (from ART-13), `error_state` `S-XX`, `entry_transition` `TR-XX`, `recovery_transitions` (list of `TR-XX`), `stuck: true|false`, `citation`.

### 6.9 Mermaid Flowchart Emission

Emit Mermaid `flowchart TD` diagrams per `OUTPUT_RULES.md` § 7:

- **Per-propagation-path diagram** — one diagram per propagation path with `≥ 3` hops. Nodes: throw sites (octagon), propagation functions (rectangle), catch sites (rounded), global handlers (hexagon). Edges labeled with the exception type and citation.
- **Resilience-pattern catalog diagram** — `graph LR` showing every `CB-XX`, `BH-XX`, `RL-XX`, `FB-XX`, `DLQ-XX`, and the operations they protect.
- **Global-handler overview diagram** — `flowchart TD` showing every `GH-XX` and the scope it covers (process / framework / domain).

Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

### 6.10 Coverage Cross-Check

Cross-check the error-handler catalog against ART-12's `EH-XX` set:

1. Compute `H_12` = set of exception handlers `EH-XX` from ART-12.
2. Compute `H_17` = set of error handlers `HDL-XX` from ART-17.
3. Expected: `H_17 ⊇ H_12`. Handlers in `H_12 \ H_17` are `COVERAGE_GAP` findings (a handler in ART-12 that ART-17 missed).
4. Handlers in `H_17 \ H_12` are handlers cataloged in ART-17 that ART-12 did not record (typically global handlers, result-type matches, and resilience-pattern handlers, which ART-12 may not detect); these are flagged for review.

---

## 7. Required Outputs

### ART-17 — Error Handling & Resilience Report

**Type:** Doc.

**Acceptance Criteria:**

- AC-17.1: The artifact file exists at `<output_root>/artifacts/phase2/ART17_<engagement_id>_error-resilience.md`.
- AC-17.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-17.3: The body contains: Executive Summary, Methodology, Error-Handler Catalog, Recovery-Strategy Classification, Retry and Timeout Catalog, Resilience-Pattern Catalog, Error-Propagation Paths, Global Error Handlers, Logging-at-Error Sites, State-Machine Recovery Cross-Reference, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-17.4: Every handler, retry, resilience pattern, propagation path, and global handler cites its source.
- AC-17.5: Every Mermaid diagram is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-17.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-17.7: Every `SWALLOWED_EXCEPTION` and `UNHANDLED` exception is flagged with severity.
- AC-17.8: Coverage cross-check is recorded with no unresolved contradictions.

---

## 8. Output Templates

### 8.1 ART-17 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-17
artifact_type: Doc
producing_prompt: PROMPT_17
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
error_handlers:
  - id: HDL-01
    kind: try-catch | error-boundary | result-match | panic-recover | global-process | global-framework | global-domain | middleware-error | rx-onError | promise-catch | async-catch
    function_id: FN-XX
    caught_types: [<type>]
    recovery_strategy: rethrow | swallow | transform | retry | fallback | propagate | recover-with-state-change | mixed
    recovery_details: <text>
    file: <path>
    line_range: <start-end>
retries_timeouts:
  - id: RT-01
    handler_id: HDL-XX | operation_id: FN-XX
    max_attempts: <int>
    backoff_strategy: exponential | linear | fixed | decorrelated-jitter
    backoff_params: { initial_delay: <text>, max_delay: <text>, multiplier: <float>, randomization_factor: <float> }
    timeout_duration: <text>
    on_timeout: cancel | return-default | throw-timeout-exception
    citation: <file>:<line-range>
resilience_patterns:
  circuit_breakers:
    - id: CB-01
      library: <name>
      protected_operation: FN-XX
      failure_threshold: <int>
      failure_rate_threshold: <float>
      wait_duration_in_open_state: <text>
      sliding_window_size: <int>
      sliding_window_type: count-based | time-based
      citation: <file>:<line-range>
  bulkheads:
    - id: BH-01
      max_concurrent_calls: <int>
      max_wait_duration: <text>
      protected_operation: FN-XX
      citation: <file>:<line-range>
  fallbacks:
    - id: FB-01
      trigger_condition: <text>
      fallback_value: <text> | fallback_fn_id: FN-XX
      citation: <file>:<line-range>
  dead_letter_queues:
    - id: DLQ-01
      source_queue: <name>
      dlq_target: <name>
      trigger_condition: max-receives | max-age | processing-failure
      max_receives: <int>
      citation: <file>:<line-range>
propagation_paths:
  - id: PP-01
    thrown_at: FN-XX
    thrown_type: <name>
    path: [FN-XX]
    caught_at: HDL-XX | UNCAUGHT | UNHANDLED
    recovery_strategy_at_terminal: <text>
    citations: [<file>:<line-range>]
global_handlers:
  - id: GH-01
    scope: process | framework | domain
    framework: <name>
    handler_fn: FN-XX
    caught_types: [<type>]
    citation: <file>:<line-range>
logging_at_error_sites:
  - id: LE-01
    handler_id: HDL-XX
    level: ERROR | FATAL | WARN | CRITICAL
    message_template: <text>
    include_exception: true | false
    structured_fields: [<name>]
    citation: <file>:<line-range>
    sensitive_data_warning: true | false
state_machine_recovery:
  - id: SMR-01
    unit_id: S-XX
    error_state: S-XX
    entry_transition: TR-XX
    recovery_transitions: [TR-XX]
    stuck: true | false
    citation: <file>:<line-range>
coverage_cross_check:
  handlers_from_art12: [EH-XX]
  handlers_cataloged: [HDL-XX]
  coverage_gaps: [EH-XX]
  catalog_only: [HDL-XX]
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

### 8.2 ART-17 Body Skeleton

```markdown
# ART-17: Error Handling & Resilience Report

## 1. Executive Summary
## 2. Methodology
## 3. Error-Handler Catalog
## 4. Recovery-Strategy Classification
## 5. Retry and Timeout Catalog
## 6. Resilience-Pattern Catalog
   ### 6.1 Circuit Breakers
   ### 6.2 Bulkheads
   ### 6.3 Fallbacks
   ### 6.4 Dead-Letter Queues
   **Diagram D-01: Resilience-Pattern Catalog**
## 7. Error-Propagation Paths
   ### 7.1 PP-01: <thrown_type>
   **Diagram D-02: PP-01 Propagation**
## 8. Global Error Handlers
   **Diagram D-03: Global-Handler Overview**
## 9. Logging-at-Error Sites
## 10. State-Machine Recovery Cross-Reference
## 11. Coverage Cross-Check
## 12. Traceability Index
## 13. Open Questions
## 14. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every `EH-XX` in ART-12 is cataloged as a `HDL-XX`; threshold ≥ 0.95.
- **Q2. Citation Check** — ≥ 0.95 of handlers, retries, resilience patterns, propagation paths, and global handlers cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no error handler in ART-17 contradicts ART-12's `EH-XX` records or ART-13's `FAILED`/`ERROR` transitions.
- **Q5. UNVERIFIED Accounting** — every `SWALLOWED_EXCEPTION`, `UNHANDLED`, `PROPAGATION_INCOMPLETE`, `NO_RECOVERY_PATH`, and `silent-catch` has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.5 on a 5% sample of throw sites yields the same propagation path.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-17.A. Handler Recovery Classification** — every `HDL-XX` has a non-empty `recovery_strategy` field.
- **Q-17.B. Resilience-Pattern Library Coverage** — every circuit breaker / bulkhead library imported (per ART-02) has at least one `CB-XX`/`BH-XX` entry or an Open Question.
- **Q-17.C. Propagation-Path Bounded** — no propagation path exceeds 30 hops without being marked `PROPAGATION_INCOMPLETE`.
- **Q-17.D. Global-Handler Coverage** — every entry point in ART-05 has either a framework-level global handler or an Open Question explaining the gap.
- **Q-17.E. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment.
- **Q-17.F. Sidecar Files** — every Mermaid block has a `.mmd` sidecar file.
- **Q-17.G. Sensitive-Data Logging** — every logging-at-error site with `include_exception: true` is cross-referenced against ART-11's sensitive-flow register; matches are flagged `sensitive_data_warning: true`.

---

## 10. Common Pitfalls

- Do not infer error-handling behavior from comments or docstrings; trace the actual catch block per R22.
- Always classify the recovery strategy for every handler; an unclassified handler is non-conformant.
- Do not omit swallowed exceptions because they are "intentional"; the swallow is a finding, not a stylistic choice.
- Always cite the retry/backoff configuration; an unspecified backoff is non-conformant.
- Do not conflate retry with circuit breaker; retry operates on a single call, while circuit breaker operates across calls.
- Always trace propagation paths to the terminal handler; a path that stops mid-call-graph is `PROPAGATION_INCOMPLETE`.
- Do not infer resilience patterns from library imports alone; verify the actual usage at the protected operation.
- Always cross-check handlers against ART-12's `EH-XX` set; a missing handler is a `COVERAGE_GAP`.
- Do not omit global error handlers; they are the safety net and downstream consumers need to know they exist.
- Always flag logging-at-error sites that include sensitive data; the leak risk is a first-class finding.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders diagrams from the sidecar source.
- Do not collapse dead-letter queues with normal queues; a DLQ has distinct semantics (max-receives trigger, separate routing).

---

## 11. Handoff Criteria

PROMPT_24 and PROMPT_26 consume ART-17. Handoff requires ALL of:

- HC-17.1: ART-17 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-17.2: Every error handler from ART-12 is cataloged with a recovery strategy.
- HC-17.3: Retries, timeouts, circuit breakers, bulkheads, fallbacks, and DLQs are cataloged.
- HC-17.4: Propagation paths are traced for every throw site.
- HC-17.5: Global error handlers are cataloged.
- HC-17.6: Logging-at-error sites are recorded with sensitive-data warnings.
- HC-17.7: Mermaid diagrams are emitted with `.mmd` sidecar files.
- HC-17.8: Coverage cross-check is recorded with no unresolved contradictions.
- HC-17.9: `repository_fingerprint_recheck` matches ART-01.
- HC-17.10: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_24 (Engineering Decisions & Trade-offs — uses resilience-pattern catalog as evidence of trade-offs), PROMPT_26 (Rebuild Guide — uses error-handling catalog as required content for the operational runbook).
- **Depends on:** ART-01 (PROMPT_01), ART-09 (PROMPT_09), ART-12 (PROMPT_12), ART-13 (PROMPT_13), ART-16 (PROMPT_16).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 7.
- **Forward reference:** PROMPT_30 verifies that every `HDL-XX` in ART-17 corresponds to a handler referenced by ART-12 or ART-16 and that every `UNHANDLED` exception is recorded as a finding in the QA report.

*End of PROMPT_17. Orchestrator may dispatch PROMPT_18 upon satisfaction of § 11.*
