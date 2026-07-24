# PROMPT_05.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_05: Entry Points & Bootstrap Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_05
- **Phase:** 1
- **Stage:** 5 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-03 (PROMPT_03), ART-04 (PROMPT_04).
- **Estimated Tokens:** 11000–17000
- **Output Artifacts:** ART-05 (Map) — Entry Point & Bootstrap Trace.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Entry Point & Bootstrap Trace map (ART-05) that identifies every entry point in the subject repository, traces the bootstrap sequence from each entry point to the first unit of work performed, and records the initialization order, configuration loading, dependency-injection container setup, middleware registration, and route registration encountered along each trace.

---

## 3. When to Invoke

PROMPT_05 is dispatched when ALL of the following predicates hold:

- PROMPT_04 has emitted a Completion Record satisfying its Handoff Criteria.
- ART-01, ART-02, ART-03, ART-04 are present and non-empty.
- ART-02 records at least one ecosystem whose entry-point conventions are defined in § 6.1.
- ART-04 records at least one script or build output (else the subject is a library with no runtime entry; that case is still processed but reported as `LIBRARY_NO_RUNTIME_ENTRY`).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; fingerprint re-verification. |
| ART-02 | Manifest | Ecosystems and frameworks to drive entry-point detection (e.g., Next.js implies `pages/` and `app/` entry points). |
| ART-03 | Map | File roles to filter the candidate set (only `source` and `build-script` files are entry-point candidates). |
| ART-04 | Spec | Scripts, build outputs, and CLI binaries to identify entry points and their invocation forms. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R14 (no execution), R17 (citation), R21 (no invention), R22 (no behavior invention). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, and Mermaid diagram conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Map schema (`§ 4.3`) and type-specific bar (aggregate ≥ 30, Coverage ≥ 4, Precision ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect entry points per § 6.1 using ART-02's ecosystems, ART-03's file roles, and ART-04's scripts.
3. For every entry point, classify its kind per § 6.2 (process, server, CLI, worker, lambda, migration, test-as-entry, cron).
4. For every entry point, trace the bootstrap sequence per § 6.3 from the entry-point symbol to the first unit of work.
5. Record initialization ordering per § 6.4 for every entry point.
6. Detect dependency-injection container setup per § 6.5.
7. Detect middleware registration per § 6.6.
8. Detect route registration per § 6.7.
9. Detect lifecycle hooks per § 6.8.
10. Emit ART-05 per § 8 with full front-matter, per-entry-point trace sections, Mermaid sequence diagrams, traceability index, open questions.
11. Run the Quality Checks in § 9.
12. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Entry-Point Detection

Detect entry points by combining ecosystem conventions, manifest declarations, and code patterns.

**Node.js / TypeScript:**

- `package.json` `main` field — CommonJS entry.
- `package.json` `module` field — ESM entry.
- `package.json` `bin` field (string or map) — CLI binary entry.
- `package.json` `exports` field — modern export map; the `.` subpath is the entry.
- `next.config.*` and `pages/` or `app/` — Next.js page entries (each `pages/*.tsx` or `app/*/page.tsx` is an HTTP entry).
- `nuxt.config.*` and `pages/` — Nuxt page entries.
- Server bootstrap files — files that call `app.listen()`, `server.listen()`, `http.createServer()`, `fastify.listen()`, `koa.listen()`.
- Worker entries — files that call `queue.process()`, `worker.on('message')`, `amqp.connect()` followed by `channel.consume()`.
- Lambda handlers — files exporting a function with `(event, context) => {...}` signature or marked with a Lambda runtime marker.
- Scripts that invoke `node`/`tsx`/`ts-node`/`bun`/`deno` — every `node <file>` reference in ART-04's scripts is an entry candidate.
- Files matching `bin/*` with executable bit and a Node shebang (`#!/usr/bin/env node`).

**Python:**

- `pyproject.toml` `[project.scripts]` and `[tool.poetry.scripts]` — CLI entries.
- `setup.py` `entry_points={'console_scripts': [...]}` — CLI entries.
- Files with `if __name__ == "__main__":` block — process entries.
- `manage.py` (Django) — CLI entry for management commands.
- `app.py`/`wsgi.py`/`asgi.py` (Django/Flask/FastAPI) — server entries.
- ASGI/WSGI app factories — files exporting `app = FastAPI()`, `app = Flask(__name__)`, `application = get_wsgi_application()`.
- Celery worker entries — files instantiating `Celery(__name__)`.
- AWS Lambda handlers — functions decorated or referenced in `template.yaml` `Handler` property.
- Click/Typer CLIs — files with `@click.group()` or `app = typer.Typer()` and a `__main__` block.

**Go:**

- `main()` functions in packages under `cmd/*/main.go` — CLI/server entries.
- `main()` functions in `main.go` at the repository root — single entry.
- `func main()` is the required Go entry; every `package main` directory is an entry point.

**Rust:**

- `Cargo.toml` `[[bin]]` targets — each names an entry file (default: `src/main.rs`).
- `Cargo.toml` `[lib]` target — library entry (not a runtime entry; recorded as `LIBRARY_ENTRY`).
- `#[tokio::main]` and `fn main()` — async or sync process entries.
- AWS Lambda entries — `lambda_runtime::run(handler).await` invocations.

**Java / Kotlin:**

- `public static void main(String[] args)` in any class — process entry.
- Spring Boot `@SpringBootApplication` classes — server entry; the class is auto-detected by the Spring Boot Maven/Gradle plugin.
- Quarkus `@QuarkusMain` classes — process entry.
- Micronaut `@MicronautApplication` classes — server entry.

**Ruby:**

- `bin/*` files with `#!/usr/bin/env ruby` shebang — CLI entries.
- `config.ru` — Rack server entry (Rails, Sinatra, Hanami).
- Rakefile `task :default` — task entry.
- Sidekiq worker entries — classes including `Sidekiq::Worker`.

**PHP:**

- `public/index.php` (Laravel, Symfony) — HTTP entry.
- `bin/console` (Laravel, Symfony) — CLI entry.
- `bin/<name>` artisan commands — CLI entries.
- Files with `#!/usr/bin/env php` shebang — CLI entries.

**.NET:**

- `static void Main(string[] args)` or `static async Task Main(string[] args)` — process entry.
- Top-level statements (C# 9+) — the file with top-level statements in a project with `<OutputType>Exe` is the entry.
- ASP.NET Core `Program.cs` — server entry calling `WebApplication.CreateBuilder(args).Build().Run()`.
- Azure Functions — methods decorated with `[FunctionName("X")]` or `[HttpTrigger]`.

**Docker entries:**

- Every `Dockerfile` `ENTRYPOINT` and `CMD` instruction references an entry-point (the executable or command); the referenced binary is recorded as an entry, and the Dockerfile line is the citation.

**Migration entries:**

- Every `.sql` file under `migrations/` or `db/migrate/` is a migration entry (executed by the migration tool, not by direct invocation).

**Test-as-entry:**

- Every test file (per ART-03 `test` role) is recorded as a `TEST_ENTRY` for the test runner; the test runner's bootstrap (e.g., `jest.config.js` `setupFiles`, `pytest` `conftest.py`, `Cargo` `#[test]`) is traced.

### 6.2 Entry-Point Classification

Classify each entry point:

| Kind | Description |
|------|-------------|
| `process` | Long-running process (server, worker, daemon). |
| `server` | HTTP/gRPC/WebSocket server bootstrap. |
| `cli` | Command-line interface; runs once and exits. |
| `worker` | Queue worker or background-job processor. |
| `lambda` | Serverless function handler. |
| `migration` | Database migration script. |
| `test` | Test file executed by a test runner. |
| `cron` | Scheduled task entry (cron, k8s CronJob, AWS EventBridge rule). |
| `library` | Library export entry (`package.json` `main`, `Cargo.toml` `[lib]`); not a runtime entry. |
| `repl` | REPL or interactive shell entry. |

### 6.3 Bootstrap Tracing

For every entry point (excluding `library` and `test` kinds, which have separate trace procedures), trace the bootstrap sequence from the entry-point symbol to the first unit of work. The trace is bounded by depth 20 call-hops and by the appearance of a terminal marker.

**Terminal markers** (any one ends the trace):

- HTTP server: `app.listen()`, `server.listen()`, `WebApplication.Run()`, `uvicorn.run()`, `gunicorn` invocation.
- Worker: `channel.consume()`, `worker.run()`, `queue.process()`, `consume_messages()`.
- Lambda: end of the handler function (the handler is the first unit of work).
- CLI: `program.parse()`, `app.run()`, `cli.run()`, `dispatch(args)` invocation.
- Migration: end of the `up()`/`down()` function (the migration's body is the first unit of work).

The trace procedure:

1. Start at the entry-point symbol (e.g., `main()` in `cmd/server/main.go:18`).
2. Read the function body; record every statement in execution order.
3. For every function call, recurse into the callee if it is in-scope and the trace depth ≤ 20.
4. For every call to an external function (e.g., `gin.New()`), record the call site with `kind: external` and do not recurse.
5. For every conditional, record both branches (`IF branch` and `ELSE branch`) with the condition cited.
6. For every loop, record the loop with its condition and the call sequence inside the body.
7. Stop at a terminal marker; record the terminal marker and its line.
8. If no terminal marker is found within depth 20, mark the trace `INCOMPLETE` and emit an Open Question.

The trace is emitted as an ordered list of steps, each with `step_id`, `kind` (call | branch | loop | assign | return | terminal), `symbol`, `file`, `line_range`, `callee_kind` (internal | external), and `notes`.

### 6.4 Initialization Ordering Analysis

For every entry point, record the initialization ordering. The ordering is the sequence of initialization actions performed before the first unit of work:

1. **Module-level execution** — top-level statements, module-level `const` initializations, and `init()` (Go), `__init__.py` (Python), static initializers (Java/C#), `#[ctor]` (Rust).
2. **Configuration loading** — reads from env vars (cross-referenced to ART-04), config files, and command-line flags.
3. **Logging setup** — logger instantiation, log-level configuration.
4. **Dependency-injection container setup** — per § 6.5.
5. **Database connection pool initialization** — `sql.Open()`, `createEngine()`, `Pool()`, `DataSource` instantiation.
6. **Middleware registration** — per § 6.6.
7. **Route registration** — per § 6.7.
8. **Lifecycle hook execution** — per § 6.8 (pre-start, post-start, pre-shutdown).
9. **Server/worker start** — the terminal marker.

Each initialization action is recorded as an `INIT-XX` entry with `step_id`, `kind`, `symbol`, `file`, `line_range`, and `citations`. The order is the order of execution as observed by the trace; ambiguous orders (e.g., two `init()` functions in different packages whose order is determined by the build tool) are marked `ORDER_UNVERIFIED` with an Open Question.

### 6.5 Dependency-Injection Container Detection

Detect DI container setup by framework and by code patterns:

- **Spring** — `@SpringBootApplication` class; `@Configuration`, `@Bean`, `@Component`, `@Service`, `@Repository`, `@Autowired`, `@ComponentScan` annotations.
- **NestJS** — `@Module()`, `@Injectable()`, `@Controller()`, `app.useGlobalFilters()`, `app.useGlobalPipes()`.
- **.NET (Microsoft.Extensions.DependencyInjection)** — `ConfigureServices(IServiceCollection)`, `services.AddScoped<>()`, `services.AddSingleton<>()`, `services.AddTransient<>()`.
- **Go (uber/fx, google/wire)** — `fx.New()`, `fx.Provide()`, `wire.Build()`.
- **Python (dependency-injector, Lagom, FastAPI Depends)** — `Container()`, `provide()`, `Depends()` parameters.
- **Rust (shaku, waitlist)** — `modules!()`, `HasComponent` trait bounds.
- **Java (Guice, Dagger)** — `@Inject`, `@Provides`, `@Module`, `@Component`, `AbstractModule` subclasses.

Each container is recorded with `container_id`, `framework`, `file`, `line_range`, `registered_services` (list of `K-XX`/`I-XX` entity IDs).

### 6.6 Middleware Registration Detection

Detect middleware registration chains:

- **Express** — `app.use(middleware)`.
- **Koa** — `app.use(middleware)`.
- **Fastify** — `app.register(plugin)`, `app.addHook()`.
- **NestJS** — `app.use()`, `@UseGuards()`, `@UseInterceptors()`, `@UseFilters()`, `APP_GUARD`/`APP_INTERCEPTOR` providers.
- **Django** — `MIDDLEWARE` list in `settings.py`.
- **Flask** — `app.before_request()`, `app.after_request()`, `app.teardown_request()`.
- **FastAPI** — `app.add_middleware()`, `@app.middleware("http")`.
- **Spring** — `WebMvcConfigurer`, `HandlerInterceptor` implementations, `@ControllerAdvice`.
- **Rails** — `Rails.application.config.middleware` chain.
- **Gin** — `router.Use(middleware)`.
- **Echo** — `e.Use(middleware)`.
- **ASP.NET Core** — `app.UseMiddleware<>()`, `app.UseRouting()`, `app.UseAuthentication()`, `app.UseAuthorization()`, `app.UseEndpoints()`.

Each middleware registration is recorded with `middleware_id`, `name`, `file`, `line_range`, `registration_order`. The order is the order of `use`/`register` calls. PROMPT_16 (Middleware & Pipeline) consumes this list in detail.

### 6.7 Route Registration Detection

Detect route registration:

- **Express** — `app.get()`, `app.post()`, `app.put()`, `app.delete()`, `app.use('/path', router)`.
- **Fastify** — `app.get()`, `app.post()`, `app.route()`.
- **Koa** — `router.get()`, `router.post()` (via `koa-router` or `@koa/router`).
- **NestJS** — `@Controller()` decorators, `@Get()`, `@Post()`, etc.
- **Django** — `urlpatterns` list in `urls.py`.
- **Flask** — `@app.route()`, `@app.get()`, `@app.post()`, `Blueprint` registrations.
- **FastAPI** — `@app.get()`, `@app.post()`, `APIRouter` registrations.
- **Spring** — `@RestController`, `@RequestMapping`, `@GetMapping`, `@PostMapping`.
- **Rails** — `config/routes.rb` `get`, `post`, `resources`, `namespace`.
- **Gin** — `router.GET()`, `router.POST()`, `router.Group()`.
- **Echo** — `e.GET()`, `e.POST()`, `e.Group()`.
- **Go stdlib** — `http.HandleFunc()`, `http.Handle()`, `mux.HandleFunc()`, `chi.Router` methods.
- **ASP.NET Core** — `app.MapControllers()`, `app.MapGet()`, `app.MapPost()`, `[Route]`, `[HttpGet]`, `[HttpPost]`.
- **Next.js** — file-system routing under `pages/` and `app/`; API routes under `pages/api/` or `app/api/`.

Each route is recorded with `route_id`, `method`, `path`, `handler_symbol`, `file`, `line_range`. PROMPT_15 (API & Interface Documentation) consumes this list in detail.

### 6.8 Lifecycle Hook Detection

Detect lifecycle hooks:

- **Node process** — `process.on('exit')`, `process.on('SIGINT')`, `process.on('SIGTERM')`, `process.on('uncaughtException')`, `process.on('unhandledRejection')`.
- **Spring** — `@PostConstruct`, `@PreDestroy`, `CommandLineRunner`, `ApplicationRunner`, `SmartLifecycle`, `DisposableBean`.
- **Jakarta EE / Servlet** — `@PostConstruct`, `@PreDestroy`, `ServletContextListener`.
- **Android** — `onCreate`, `onStart`, `onResume`, `onPause`, `onStop`, `onDestroy`.
- **iOS (Swift)** — `viewDidLoad`, `viewWillAppear`, `viewDidAppear`, `application(_:didFinishLaunchingWithOptions:)`.
- **React** — `useEffect` cleanup functions, `componentWillUnmount` (legacy), class component lifecycle.
- **Vue** — `onMounted`, `onUnmounted`, `beforeUnmount`.
- **Kubernetes (init containers, preStop hooks)** — referenced via IaC (ART-02 / ART-04).
- **Go** — `signal.Notify(c, os.Interrupt, syscall.SIGTERM)`, `context.WithCancel()`.
- **Rust** — `Drop` implementations for cleanup; `tokio::signal::ctrl_c()`.
- **Python** — `atexit.register()`, `signal.signal(SIGTERM, handler)`.
- **Ruby** — `at_exit` blocks, `Signal.trap("TERM")`.
- **Java/Spring Boot** — `SpringApplication.addListeners()`, `ApplicationReadyEvent` listeners.

Each lifecycle hook is recorded with `hook_id`, `kind`, `phase` (pre-start | post-start | pre-shutdown | error), `symbol`, `file`, `line_range`.

---

## 7. Required Outputs

### ART-05 — Entry Point & Bootstrap Trace Map

**Type:** Map.

**Acceptance Criteria:**

- AC-05.1: The artifact file exists at `<output_root>/artifacts/phase1/ART05_<engagement_id>_entry-bootstrap.md`.
- AC-05.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.3.
- AC-05.3: The body contains: Executive Summary, Methodology, Entry-Point Catalog, per-entry-point Bootstrap Trace sections, Initialization Ordering, DI Containers, Middleware Registration, Route Registration, Lifecycle Hooks, Mermaid Sequence Diagrams, Traceability Index, Open Questions, Cross-References.
- AC-05.4: Every entry point has `kind`, `symbol`, `file`, `line_range`, and a citation.
- AC-05.5: Every entry point (excluding `library` and `test`) has a bootstrap trace with at least one step and (where applicable) a terminal marker.
- AC-05.6: Every Mermaid sequence diagram is preceded by a `**Diagram D-XX: <Title>**` caption per `OUTPUT_RULES.md` § 7.2.
- AC-05.7: Every trace step cites its source line range.
- AC-05.8: Incomplete traces (depth 20 without terminal marker) are marked `INCOMPLETE` with an Open Question.

---

## 8. Output Templates

### 8.1 ART-05 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-05
artifact_type: Map
producing_prompt: PROMPT_05
phase: 1
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
entry_points:
  - entry_id: EP-01
    kind: process | server | cli | worker | lambda | migration | test | cron | library | repl
    name: <name>
    symbol: <symbol>
    file: <path>
    line_range: <start-end>
    invocation: <string>      # e.g., "node dist/server.js" or "go run ./cmd/server"
    invoking_script: SCR-XX | null
    trace_status: COMPLETE | INCOMPLETE
    terminal_marker: <symbol> | NONE
bootstrap_traces:
  - entry_id: EP-01
    steps:
      - step_id: T-01
        kind: call | branch | loop | assign | return | terminal
        symbol: <name>
        file: <path>
        line_range: <start-end>
        callee_kind: internal | external
        notes: <text> | ""
initialization_ordering:
  - entry_id: EP-01
    actions:
      - step_id: INIT-01
        kind: module-init | config-load | logging-setup | di-setup | db-pool | middleware | route-registration | lifecycle-hook | server-start
        symbol: <name>
        file: <path>
        line_range: <start-end>
        citations: [<file_path>:<line-range>]
        order_confidence: DETERMINED | ORDER_UNVERIFIED
di_containers:
  - container_id: DI-01
    framework: <name>
    file: <path>
    line_range: <start-end>
    registered_services: [K-XX | I-XX]
middleware_registrations:
  - middleware_id: MW-01
    name: <name>
    file: <path>
    line_range: <start-end>
    registration_order: <int>
    entry_id: EP-XX
route_registrations:
  - route_id: RR-01
    method: GET | POST | PUT | DELETE | PATCH | HEAD | OPTIONS | WS | *
    path: <pattern>
    handler_symbol: <name>
    file: <path>
    line_range: <start-end>
    entry_id: EP-XX
lifecycle_hooks:
  - hook_id: LH-01
    kind: pre-start | post-start | pre-shutdown | error
    symbol: <name>
    file: <path>
    line_range: <start-end>
    entry_id: EP-XX
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
  - id: EP-01
    label: <name>
    kind: entry-point
    parent_id: null
    depth: 0
edges:
  - from: EP-01
    to: FN-000123       # first function in the trace
    relationship: CALLS
    evidence: <file_path>:<line-range>
---
```

### 8.2 ART-05 Body Skeleton

```markdown
# ART-05: Entry Point & Bootstrap Trace

## 1. Executive Summary
## 2. Methodology
## 3. Entry-Point Catalog
## 4. Bootstrap Traces
   ### 4.1 EP-01: <name>
   **Diagram D-01: EP-01 Bootstrap Sequence**
   ```mermaid
   sequenceDiagram
       participant Entry as EP-01 main()
       participant Config as loadConfig()
       participant Server as app.listen()
       Entry->>Config: load config (file:line)
       Config-->>Entry: config object
       Entry->>Server: app.listen(port) (file:line)
   ```
   <ordered step list>
   ### 4.2 EP-02: <name>
   ...
## 5. Initialization Ordering
## 6. Dependency-Injection Containers
## 7. Middleware Registration
## 8. Route Registration
## 9. Lifecycle Hooks
## 10. Traceability Index
## 11. Open Questions
## 12. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every in-scope entry-point candidate is recorded; threshold ≥ 0.95.
- **Q2. Citation Check** — ≥ 0.95 of trace steps cited.
- **Q3. Schema Conformance Check** — validates against § 4.3.
- **Q4. Non-Contradiction Check** — entry points do not contradict ART-04's scripts (every `bin` script in `package.json` is recorded as an entry point).
- **Q5. UNVERIFIED Accounting** — every `INCOMPLETE` trace and `ORDER_UNVERIFIED` ordering has an Open Question.
- **Q6. Idempotence Spot-Check** — re-tracing a 5% sample of entries yields the same step list.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-05.A. Terminal Marker Presence** — every `process`/`server`/`cli`/`worker`/`lambda` entry has either a terminal marker or an `INCOMPLETE` flag.
- **Q-05.B. Mermaid Diagram Per Entry** — every entry point with `trace_status: COMPLETE` has an associated Mermaid sequence diagram.
- **Q-05.C. Citation Verbatim** — every step's `line_range` is a real line range in the cited file (verified by re-reading).
- **Q-05.D. Order Consistency** — initialization ordering is monotonic; no step appears before a step it depends on (e.g., `server-start` never precedes `config-load`).
- **Q-05.E. External Call Marking** — calls to functions defined outside the in-scope set are marked `callee_kind: external` and not recursed into.

---

## 10. Common Pitfalls

- Do not execute the entry point to trace the bootstrap; static and symbolic analysis only per R14.
- Do not infer the bootstrap sequence from comments or README descriptions; trace the actual code per R22.
- Always cap trace depth at 20; an unbounded trace will recurse through the entire call graph and exhaust the token budget.
- Do not collapse module-level initialization into a single step; each `init()` / `static` initializer / module-level statement is a distinct `INIT-XX` entry.
- Always record both branches of a conditional; downstream prompts (PROMPT_12 Control Flow) consume the branch structure.
- Do not mark a trace `COMPLETE` without a terminal marker; the absence of a terminal marker is a real finding (the entry may exit without performing work, or it may be a library entry miscategorized).
- Always cross-reference the entry point back to its invoking script in ART-04; an entry point with no invoking script is `ORPHAN_ENTRY` and is flagged.
- Do not invent middleware order when registration is dynamic (e.g., middleware added inside a loop); record the loop and mark the order `ORDER_UNVERIFIED`.
- Always distinguish `library` entries from `process` entries; a library's `main` field is not a runtime entry and does not have a bootstrap trace in the same sense.
- Do not document test entry-point bootstraps with the same depth as production entry points; test bootstrap is traced to the test runner's setup, not to a terminal marker.

---

## 11. Handoff Criteria

PROMPT_12 and PROMPT_16 consume ART-05. Handoff requires ALL of:

- HC-05.1: ART-05 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-05.2: Every entry-point candidate from ART-02/ART-03/ART-04 is classified or marked `UNVERIFIED` with rationale.
- HC-05.3: Every `process`/`server`/`cli`/`worker`/`lambda` entry has a bootstrap trace.
- HC-05.4: Every `COMPLETE` trace has a Mermaid sequence diagram.
- HC-05.5: Initialization ordering is recorded for every entry.
- HC-05.6: DI containers, middleware registrations, route registrations, and lifecycle hooks are enumerated.
- HC-05.7: `repository_fingerprint_recheck` matches ART-01.
- HC-05.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_12 (Control Flow & Execution Path — uses bootstrap traces as the starting point for execution path analysis), PROMPT_16 (Middleware & Pipeline — uses middleware registration list as its seed).
- **Depends on:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-03 (PROMPT_03), ART-04 (PROMPT_04).
- **Governing rules:** `OPERATING_RULES.md` R13, R14, R17, R21, R22.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.3; Map bar (aggregate ≥ 30, Coverage ≥ 4, Precision ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 3.1, § 4, § 6, § 7 (Mermaid sequence diagrams).
- **Forward reference:** PROMPT_25 (Diagram Generation) will re-render the bootstrap sequence diagrams at higher visual fidelity; ART-05 provides the source data.

*End of PROMPT_05. Orchestrator may dispatch PROMPT_06 upon satisfaction of § 11.*
