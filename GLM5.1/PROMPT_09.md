# PROMPT_09.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_09: Function-Level Reverse Engineering

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_09
- **Phase:** 1
- **Stage:** 9 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-03 (PROMPT_03), ART-06 (PROMPT_06), ART-08 (PROMPT_08).
- **Estimated Tokens:** 15000–22000
- **Output Artifacts:** ART-09 (Doc) — Function Reference.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Function Reference (ART-09) that documents every in-scope function and method with its signature, purpose, parameters, return value, side effects, callers, callees, cyclomatic complexity, algorithm summary, purity classification, recursion classification, higher-order classification, and dead-code candidacy.

---

## 3. When to Invoke

PROMPT_09 is dispatched when ALL of the following predicates hold:

- PROMPT_08 has emitted a Completion Record satisfying its Handoff Criteria.
- ART-01, ART-03, ART-06, and ART-08 are present and non-empty.
- ART-03 records at least one `source` file or `build-script` file containing function definitions.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; fingerprint re-verification. |
| ART-03 | Map | File roles; `source` and `build-script` files are the analysis set. |
| ART-06 | Map | Module boundaries; functions are scoped to a module. |
| ART-08 | Doc | Class catalog; methods belong to classes and carry the class's `K-XX` reference. Public-API method IDs (`FN-XX`) declared in ART-08 are the seed for method analysis. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R16, R17, R19, R21, R22. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions. |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect functions per § 6.1 across all in-scope `source` and `build-script` files.
3. For every function, extract its signature per § 6.2.
4. For every function, infer its purpose per § 6.3.
5. For every function, enumerate its callers per § 6.4.
6. For every function, enumerate its callees per § 6.5.
7. For every function, identify its side effects per § 6.6.
8. For every function, estimate cyclomatic complexity per § 6.7.
9. For every function, classify purity per § 6.8.
10. For every function, classify recursion per § 6.9.
11. For every function, classify higher-order status per § 6.10.
12. For complex functions (cyclomatic complexity ≥ 10), document the algorithm step-by-step per § 6.11.
13. Compute dead-code candidacy per § 6.12 using caller enumeration after transitive closure from entry points (per ART-05).
14. Emit ART-09 per § 8 with full front-matter, function catalog, traceability index, open questions.
15. Run the Quality Checks in § 9.
16. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Function Detection

Detect functions and methods by language-specific syntax:

- **TypeScript/JavaScript** — `function <name>(...)`, `const <name> = (...) => ...`, `const <name> = function(...)`, `<name>(...) { ... }` (class methods), `async function`, `async (...) =>`, `get <name>()`, `set <name>(v)`, generator functions (`function*`).
- **Python** — `def <name>(...)`, `async def <name>(...)`, `lambda` (recorded as anonymous function with parent), `@property`-decorated methods, `@staticmethod`, `@classmethod`.
- **Go** — `func <name>(...)`, `func (r <Type>) <name>(...)` (methods), `func init()`, `func main()`.
- **Rust** — `fn <name>(...)`, `fn <name>(...) -> <Type>`, `pub fn`, `async fn`, `const fn`, `unsafe fn`, methods inside `impl` blocks, trait default methods.
- **Java** — `<modifiers> <return-type> <name>(...) { ... }`, `default <name>(...)` (interface default methods), `static <name>(...)`, constructors (`<ClassName>(...)`).
- **Kotlin** — `fun <name>(...)`, `suspend fun`, `fun <name>(...) = <expr>` (expression-body).
- **Swift** — `func <name>(...)`, `func <name>(...) async`, `func <name>(...) throws`, initializers (`init(...)`), deinitializers (`deinit`).
- **C#** — `<modifiers> <return-type> <name>(...) { ... }`, `async <return-type> <name>(...)`, expression-bodied members (`<name>(...) => expr`), local functions.
- **C++** — `<return-type> <name>(...)`, `<return-type> <Class>::<name>(...)`, template functions (`template<typename T> <return-type> <name>(...)`), operator overloads (`operator+`, `operator[]`), constructors, destructors.
- **Ruby** — `def <name>`, `def <name>(...)`, `def self.<name>` (class methods), `define_method(:name) { ... }`.
- **PHP** — `function <name>(...)`, `function <name>(...): <type>`, `function <name>(...) use ($var)`, anonymous closures (`function (...) use (...)`), arrow functions (`fn(...) => ...`).
- **Scala** — `def <name>(...)`, `def <name>(...): <type> = <expr>`, `def <name>(...) = <expr>` (type-inferred).
- **Elixir** — `def <name>`, `defp <name>` (private), `defmacro <name>`, anonymous functions (`fn ... -> ... end`).
- **Clojure** — `(defn <name> ...)`, `(defn- <name> ...)` (private), `(fn [...] ...)`.

Each detected function is assigned an `FN-XX` ID. Each function records `name`, `file`, `line_range`, `kind` (function | method | constructor | destructor | getter | setter | generator | async | anonymous | lambda | macro | property-method), `class_id` (`K-XX` for methods; null for free functions), `module_id` (`M-XX` per ART-06), `language`.

### 6.2 Signature Extraction

For every function, extract its signature:

- **Parameters** — each parameter's `name`, `type` (or `any`/`UNVERIFIED` for untyped languages), `default_value` (or `none`), `variadic` (true|false), `optional` (true|false).
- **Return type** — the declared return type (or inferred type where the language supports inference and the type is recoverable; otherwise `UNVERIFIED`).
- **Async/await** — whether the function is async (`true|false`).
- **Throws/raises** — the declared exceptions (Java `throws`, Rust `Result<T, E>`, Python `raises` docstring, Swift `throws`), or `UNVERIFIED`.
- **Generic parameters** — type parameters with bounds.
- **Modifiers** — `static`, `abstract`, `virtual`, `override`, `final`, `const`, `inline`, `unsafe`, `synchronized`, `pub`, `internal`, `private`, `protected`.

For dynamically-typed languages (JavaScript pre-TS, Ruby, Python pre-3.5), the agent MAY infer types from call sites and return statements; inferred types are marked `inferred: true` and `UNVERIFIED` per R21.

### 6.3 Purpose Inference

For every function, infer a one-line purpose statement (≤ 120 characters):

1. Read the function's docstring/JSDoc/KDoc/JavaDoc; if a `@description` or first descriptive sentence exists, extract it.
2. ELSE read the function's first comment line; if descriptive, extract.
3. ELSE infer from the function name and the names of its primary callee(s): e.g., `validateUserInput(user)` calling `isValid(user)` and `throwIfInvalid(...)` → "Validates the user input and throws on invalid state."
4. ELSE mark `purpose: UNVERIFIED` and emit an Open Question.

Purpose statements MUST cite the source that evidences them (the docstring line, the comment line, or the function body line where the inference was derived).

### 6.4 Caller Enumeration

For every function, enumerate its callers — the sites where the function is invoked. Callers are detected by:

- **Direct invocation** — `<name>(args)` syntax (per language).
- **Method invocation** — `<obj>.<name>(args)` syntax; the receiver's type is resolved via ART-08 (class catalog) to determine which class's method is invoked.
- **Static invocation** — `<Class>.<name>(args)`.
- **Reference without invocation** — `const x = <name>;` or `pass <name> as callback`; recorded as `kind: reference` (the function is used as a value, not called directly).
- **Reflective invocation** — `Reflect.invoke(obj, 'name', args)`, `getattr(obj, 'name')(args)`, `obj.send('name', args)`, `Method.invoke(obj, args)`. Recorded as `kind: reflective` and the call site is recorded; the resolved function is `UNVERIFIED` because reflection defers resolution to runtime.
- **Dynamic dispatch** — virtual method calls; recorded as `kind: dynamic-dispatch` and the candidate targets are listed (every override of the method in the class hierarchy per ART-08).

Each caller is recorded as `caller_id` (`FN-XX`), `call_site` (`file:line`), `kind` (direct | method | static | reference | reflective | dynamic-dispatch), `candidate_targets` (for dynamic dispatch, the list of `FN-XX`).

### 6.5 Callee Enumeration

For every function, enumerate its callees — the functions it invokes. The procedure is the inverse of § 6.4: scan the function's body for every invocation and record the callee's `FN-XX`, the call site, and the kind. External callees (functions defined outside the in-scope set, including standard-library and third-party functions) are recorded as `EXTERNAL` with the callee's fully-qualified name.

### 6.6 Side-Effect Identification

For every function, identify its side effects:

- **Writes to module-level state** — assignments to module-level variables (`V-XX` per `PROJECT_SPECIFICATION.md` § 4.1); each write recorded with the variable ID and the line.
- **Writes to class fields** — assignments to `this.field` (JS/TS/Java/C#/Kotlin), `self.field` (Python), `@field` (Ruby), `this->field` (C++/PHP); each recorded with the field ID.
- **I/O** — file reads/writes, network calls, database queries, console output, log writes. Each recorded with the I/O kind (file | network | database | console | log) and the citation.
- **Mutation of arguments** — assignments to argument object fields; recorded per argument.
- **Exception throwing** — `throw`, `raise`, `panic!`, `throw new`, `except`, `rethrow`; each recorded with the exception type and the line.
- **Event emission** — `emit('event')`, `publish(topic, msg)`, `notify(listeners)`, `dispatch(action)`; each recorded with the event/topic/action name and cross-referenced to PROMPT_14 (Event Workflows).
- **Process state** — `process.exit()`, `os.exit()`, `sys.exit()`, `Environment.Exit()`; recorded as `kind: process-exit`.
- **External calls** — calls to external functions known to have side effects (e.g., `fs.writeFileSync`); recorded with the external function's name.

Functions with no identified side effects are `pure-candidate`. Functions with at least one side effect are `impure`.

### 6.7 Cyclomatic Complexity Estimation

Estimate cyclomatic complexity (CC) per the standard formula: `CC = 1 + number of decision points`. Decision points per language:

- `if`, `else if` (each `else if` adds 1).
- `for`, `while`, `do-while`, `foreach`.
- `case`/`when` in `switch`/`match`/`case` (each case adds 1; the default adds 0).
- `&&`, `||` (each adds 1; short-circuit operators are decision points).
- `?` ternary (adds 1).
- `catch`/`except`/`rescue` (each adds 1).
- Conditional expressions in list comprehensions (`[x for x in y if cond]` — the `if` adds 1).

CC is recorded as an integer. Functions with CC ≥ 10 are flagged `HIGH_COMPLEXITY` and trigger § 6.11.

### 6.8 Purity Classification

Classify each function as `pure` or `impure`:

- **Pure** — no side effects (per § 6.6) AND no I/O AND no mutation of arguments AND no reads of mutable module-level state. Reads of `const`/`readonly`/`final` module-level state are permitted.
- **Impure** — has at least one side effect or reads mutable global state.
- **Referentially-transparent** — pure AND the return value depends only on the arguments (no hidden inputs). A subset of pure.

Each function records `purity: pure | impure | referentially-transparent | UNVERIFIED`.

### 6.9 Recursion Classification

Classify each function's recursion:

- **Direct recursion** — the function calls itself by name.
- **Mutual recursion** — the function calls function `B`, which (directly or transitively) calls back the function.
- **Tail recursion** — direct recursion where the recursive call is the last operation; detected by checking that the recursive call is in tail position (return statement with no further computation).
- **Non-recursion** — no recursive calls.

Each recursive function records `recursion_kind` and the recursion partners (the `FN-XX` IDs involved in the cycle for mutual recursion).

### 6.10 Higher-Order Classification

Classify each function's higher-order status:

- **Takes a function argument** — at least one parameter is a function type (or, in dynamic languages, used as a function inside the body).
- **Returns a function** — the return type is a function type (or, in dynamic languages, the body returns a function/lambda).
- **Both** — takes and returns a function.
- **Neither** — not higher-order.

Each higher-order function records `higher_order: takes | returns | both | neither`.

### 6.11 Algorithm Documentation (Complex Functions)

For every function with CC ≥ 10 (per § 6.7), document the algorithm step-by-step. The procedure:

1. Read the function body and decompose it into a numbered list of steps in execution order.
2. For each step, record `step_id`, `description` (one sentence), `kind` (assign | branch | loop | call | return | throw), `citation` (the line range).
3. For each branch, record the condition and both branches' first steps.
4. For each loop, record the iteration variable, the iterable/condition, and the loop body's first step.
5. Identify the algorithm's overall shape (e.g., "iterative refinement", "divide-and-conquer", "linear scan with early exit", "dynamic programming") and record it as `algorithm_shape`.
6. If the function implements a known algorithm (e.g., quicksort, binary search, Dijkstra's), record `known_algorithm: <name>` with `confidence: low | medium | high` and the citation that evidences the identification.

The algorithm documentation is recorded under `algorithm_steps` per function.

### 6.12 Dead-Code Candidacy

A function is a `DEAD_CODE_CANDIDATE` if it has zero callers after transitive closure from the entry points (per ART-05). The procedure:

1. Construct the call graph (function-to-function) from the caller enumeration (per § 6.4) — this is the inverse of the callee enumeration.
2. Mark every function reachable from any entry point's transitive closure as `REACHABLE`.
3. Mark every function not reachable as `DEAD_CODE_CANDIDATE`.
4. For each `DEAD_CODE_CANDIDATE`, record `candidate_reason` (no-callers | only-test-callers | only-reflective-callers | only-dynamic-dispatch-with-no-implementations).
5. Functions called only via reflection or dynamic dispatch are `DEAD_CODE_CANDIDATE` with reduced confidence (`confidence: medium`), because the call may exist at runtime but is not statically resolvable.

PROMPT_30's HOOK-01 (Dead-Code Detection Hook per `QUALITY_STANDARDS.md` § 8) verifies this analysis.

---

## 7. Required Outputs

### ART-09 — Function Reference (Doc)

**Type:** Doc.

**Acceptance Criteria:**

- AC-09.1: The artifact file exists at `<output_root>/artifacts/phase1/ART09_<engagement_id>_function-ref.md`.
- AC-09.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-09.3: The body contains: Executive Summary, Methodology, Function Catalog, Caller/Callee Map, Side-Effect Catalog, Complexity Distribution, Purity Classification, Recursion Catalog, Higher-Order Catalog, Algorithm Documentation (for high-complexity functions), Dead-Code Candidates, Traceability Index, Open Questions, Cross-References.
- AC-09.4: Every in-scope function has `name`, `file`, `line_range`, `signature`, `purpose`, `callers`, `callees`, `cyclomatic_complexity`, `purity`, `recursion_kind`, `higher_order`, `dead_code_candidate`.
- AC-09.5: Every caller and callee has a citation to the call site.
- AC-09.6: Every side effect has a citation.
- AC-09.7: Every high-complexity function has an algorithm documentation block.
- AC-09.8: Every `DEAD_CODE_CANDIDATE` has a `candidate_reason` and a `confidence`.

---

## 8. Output Templates

### 8.1 ART-09 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-09
artifact_type: Doc
producing_prompt: PROMPT_09
phase: 1
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
functions:
  - function_id: FN-01
    name: <name>
    file: <path>
    line_range: <start-end>
    kind: function | method | constructor | destructor | getter | setter | generator | async | anonymous | lambda | macro | property-method
    class_id: K-XX | null
    module_id: M-XX
    language: <name>
    signature:
      parameters:
        - name: <name>
          type: <type-string> | UNVERIFIED
          default_value: <value> | none
          variadic: true | false
          optional: true | false
          inferred: true | false
      return_type: <type-string> | UNVERIFIED
      is_async: true | false
      throws: [<exception-type>] | UNVERIFIED
      generic_parameters:
        - parameter: T
          bound: <type> | none
      modifiers: [static | abstract | virtual | override | final | const | inline | unsafe | pub | internal | private | protected]
    purpose: <text>
    purpose_source: <file>:<line>
    callers:
      - caller_id: FN-XX | EXTERNAL
        call_site: <file>:<line>
        kind: direct | method | static | reference | reflective | dynamic-dispatch
        candidate_targets: [FN-XX]
    callees:
      - callee_id: FN-XX | EXTERNAL
        call_site: <file>:<line>
        kind: direct | method | static | reference | reflective | dynamic-dispatch
    side_effects:
      - kind: module-state-write | field-write | io | argument-mutation | throw | event-emit | process-exit | external-call
        target: <V-XX | field-name | io-kind | event-name | external-fn>
        citation: <file>:<line>
    cyclomatic_complexity: <int>
    high_complexity: true | false
    purity: pure | impure | referentially-transparent | UNVERIFIED
    recursion_kind: direct | mutual | tail | none
    recursion_partners: [FN-XX]
    higher_order: takes | returns | both | neither
    algorithm_steps:
      - step_id: AS-01
        description: <text>
        kind: assign | branch | loop | call | return | throw
        citation: <file>:<line-range>
    algorithm_shape: <text> | NA
    known_algorithm: <name> | NA
    known_algorithm_confidence: low | medium | high | NA
    dead_code_candidate: true | false
    dead_code_reason: no-callers | only-test-callers | only-reflective-callers | only-dynamic-dispatch-with-no-implementations | NA
    dead_code_confidence: low | medium | high | NA
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
items:
  - id: FN-01
    name: <name>
    type: function
    location: <file_path>:<line-range>
    attributes: { ... }
---
```

### 8.2 ART-09 Body Skeleton

```markdown
# ART-09: Function Reference

## 1. Executive Summary
## 2. Methodology
## 3. Function Catalog
   ### 3.1 Module M-XX
## 4. Caller/Callee Map
## 5. Side-Effect Catalog
## 6. Complexity Distribution
## 7. Purity Classification
## 8. Recursion Catalog
## 9. Higher-Order Catalog
## 10. Algorithm Documentation (High-Complexity Functions)
   ### 10.1 FN-XX: <name>
## 11. Dead-Code Candidates
## 12. Traceability Index
## 13. Open Questions
## 14. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every in-scope function definition is recorded; threshold ≥ 0.95.
- **Q2. Citation Check** — ≥ 0.95 of claims cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — every method's `class_id` resolves to a class in ART-08; every function's `module_id` resolves to a module in ART-06.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` purpose, return type, throws declaration has an Open Question.
- **Q6. Idempotence Spot-Check** — re-detecting functions in a 5% sample yields the same set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-09.A. Caller/Callee Bidirectionality** — for every `(caller, callee)` pair, the inverse `(callee, caller)` exists in the callee's `callers` list (recorded in callee's entry, not duplicated in caller's).
- **Q-09.B. Complexity Computation** — re-computing CC for a 5% sample yields the same value.
- **Q-09.P. Purity Consistency** — a function marked `pure` has zero side effects; if any side effect is recorded, the function is `impure`.
- **Q-09.R. Recursion Cycle Closure** — for mutual recursion, the recursion partners form a cycle in the call graph (verified by traversal).
- **Q-09.D. Dead-Code Transitive Closure** — re-running reachability from entry points yields the same `REACHABLE` set.
- **Q-09.H. High-Complexity Algorithm Docs** — every function with `high_complexity: true` has a non-empty `algorithm_steps` block.

---

## 10. Common Pitfalls

- Do not infer a function's purpose from its name alone; always consult the docstring or body first per R22.
- Always distinguish methods from free functions; methods are scoped to a class and carry the class's `K-XX` reference.
- Do not record private methods as dead-code candidates without checking reflective access; a method invoked via `Method.invoke` is reachable, just not statically.
- Always record external callees with `EXTERNAL` and the fully-qualified name; an external callee is not a missing citation, it is a categorically different relationship.
- Do not classify a function as `pure` if it reads mutable module-level state; purity requires both no side effects and no hidden inputs.
- Always cap algorithm documentation at the high-complexity functions; documenting every function's algorithm step-by-step exhausts the token budget without adding value for simple wrappers.
- Do not infer dynamic-dispatch targets without consulting ART-08's class hierarchy; the candidate targets are the overrides in the hierarchy.
- Always record the `confidence` for dead-code candidacy; a function called only via reflection is not certainly dead, and the confidence reflects that.
- Always cite the call site, not the callee definition, for caller enumeration per R19.
- Do not collapse generators and async functions into a single kind; they have different control-flow semantics that PROMPT_12 (Control Flow) consumes distinctly.
- Always record the algorithm shape for high-complexity functions; the shape (`linear scan`, `divide-and-conquer`, etc.) is downstream-consumable by PROMPT_23 (Design Patterns) for strategy-pattern detection.

---

## 11. Handoff Criteria

PROMPT_10, PROMPT_12, and PROMPT_28 consume ART-09. Handoff requires ALL of:

- HC-09.1: ART-09 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-09.2: Every in-scope function is recorded.
- HC-09.3: Every function has signature, purpose, callers, callees, complexity, purity, recursion, higher-order, and dead-code classification.
- HC-09.4: Every caller and callee has a citation.
- HC-09.5: Every high-complexity function has algorithm documentation.
- HC-09.6: Dead-code candidates are enumerated with reasons and confidence.
- HC-09.7: `repository_fingerprint_recheck` matches ART-01.
- HC-09.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_10 (Call & Dependency Graph — uses caller/callee edges as the call graph's seed), PROMPT_12 (Control Flow — uses algorithm steps and complexity), PROMPT_28 (Cross-Reference Checklists — uses ART-09 as the function coverage ground truth).
- **Depends on:** ART-01 (PROMPT_01), ART-03 (PROMPT_03), ART-06 (PROMPT_06), ART-08 (PROMPT_08).
- **Governing rules:** `OPERATING_RULES.md` R13, R16, R17, R19, R21, R22.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 3.1, § 4, § 6.
- **Forward reference:** PROMPT_30 runs HOOK-01 (Dead-Code Detection) against ART-09 to verify that every function with zero transitive callers from entry points is flagged.

*End of PROMPT_09. Orchestrator may dispatch PROMPT_10 upon satisfaction of § 11.*
