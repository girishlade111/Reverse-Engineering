# PROMPT_23.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_23: Design Pattern Identification

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_23
- **Phase:** 3
- **Stage:** 3 of 5
- **Dependencies:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-06 (PROMPT_06), ART-07 (PROMPT_07), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-21 (PROMPT_21).
- **Estimated Tokens:** 14000–22000
- **Output Artifacts:** ART-23 (Doc) — Design Pattern Catalog.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Design Pattern Catalog artifact (ART-23) that identifies every design-pattern instance in the subject repository across the creational, structural, behavioral, architectural, concurrency, and AI-specific categories, records each instance's participants, location, variation, and inferred rationale, and cross-references each instance to the entities (`K-XX`, `I-XX`, `FN-XX`, `M-XX`, `W-XX`) that realize it — so that a downstream engineer can recognize the architectural vocabulary the original authors used and rebuild the system in the same idiom.

---

## 3. When to Invoke

PROMPT_23 is dispatched when ALL of the following predicates hold:

- Phase 2 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.3.
- PROMPT_21 has emitted its completion record. ART-21 may be `NOT_PRODUCED` under the skipped behavior; PROMPT_23 MUST degrade gracefully by skipping AI-specific pattern analysis (§ 6.7) and recording an Open Question.
- ART-01, ART-02, ART-06, ART-07, ART-08, ART-09, and ART-10 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-08 records at least one class (`K-XX`) OR ART-09 records at least one function (`FN-XX`); absent both, the prompt emits `BLOCKED` with `INPUT_GAP`.

PROMPT_23 is not gated by a marker-detection trigger; every codebase uses some pattern (even if only "no-pattern" / "ad-hoc"), so the prompt always produces ART-23.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-02 | Manifest | Tech stack & dependencies; framework markers indicate idiomatic patterns (e.g., Spring → likely MVC/IoC; React → likely Component/Observer). |
| ART-06 | Map | Module boundaries; architectural patterns are aggregated to the module level. |
| ART-07 | Map | Component map; component composition reveals structural patterns (Composite, Decorator, Facade). |
| ART-08 | Doc | Class & interface catalog; pattern participants are typically classes (`K-XX`) and interfaces (`I-XX`). Inheritance (`EXTENDS`) and implementation (`IMPLEMENTS`) edges drive creational and structural pattern detection. |
| ART-09 | Doc | Function catalog; behavioral patterns are often function-pair relationships (e.g., Observer's `subscribe`/`notify`, Strategy's `execute`/`setStrategy`). |
| ART-10 | Graph | Call graph and dependency graph; pattern-detection heuristics rely on call-edge topology (e.g., Chain of Responsibility is a linear call chain; Composite is a tree). |
| ART-21 | Doc | AI/LLM workflow report; AI-specific patterns (ReAct, Plan-and-Execute, Tool Use, Memory) are cross-referenced from ART-21's `agents` and `workflows` entries. When ART-21 is `ABSENT`, AI-specific pattern analysis is skipped. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (§ 4.5) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect every creational-pattern instance per § 6.1 (Singleton, Factory, Abstract Factory, Builder, Prototype, Object Pool).
3. Detect every structural-pattern instance per § 6.2 (Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy).
4. Detect every behavioral-pattern instance per § 6.3 (Chain of Responsibility, Command, Interpreter, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor).
5. Detect every architectural-pattern instance per § 6.4 (MVC, MVP, MVVM, Layered, Hexagonal, Clean Architecture, CQRS, Event Sourcing, Microservices, Serverless, Plugin).
6. Detect every concurrency-pattern instance per § 6.5 (Active Object, Monitor, Reactor, Thread Pool, Producer-Consumer, Read-Write Lock, Barrier, Future/Promise, Actor).
7. Cross-reference ART-21's agents and workflows per § 6.6 to detect AI-specific patterns (ReAct, Plan-and-Execute, Tool Use, Memory, RAG). IF ART-21 is `ABSENT`, skip AI-specific pattern analysis and record an Open Question.
8. For each detected instance, identify the participants per § 6.7.
9. For each detected instance, identify the location per § 6.8 (file, class, function, module).
10. For each detected instance, classify the variation per § 6.9 (e.g., Singleton: eager | lazy | thread-safe | monostate).
11. For each detected instance, infer the rationale per § 6.10 (distinguish `documented` from `inferred`).
12. Emit Mermaid pattern-instance diagrams per § 6.11.
13. Cross-check the pattern catalog against ART-08's class catalog and ART-10's call graph per § 6.12; participants not in ART-08 are `CONTRADICTION` findings per R33.
14. Emit ART-23 per § 8 with full front-matter, per-category sections, per-instance subsections, traceability index, open questions.
15. Run the Quality Checks in § 9.
16. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Creational-Pattern Detection

Detect every creational-pattern instance. Detection combines structural markers (class shape, inheritance), naming conventions, and call patterns. Each instance records `pattern_id` `P-XX`, `category: creational`, `name`, `variation`, `participants`, `location`, `rationale_kind` (documented | inferred), `rationale`, `citation`.

- **Singleton** — a class with a private/protected constructor, a static instance field, and a static accessor (`getInstance()`, `instance`, `Instance`, `shared`). Variations: eager (instance initialized at class load), lazy (initialized on first access), thread-safe (with double-checked locking, `synchronized`, `Once` in Go, `sync.Once`, `std::sync::Once`), monostate (all instances share state via static fields). Markers: `class X { private constructor(); static instance; static getInstance() }`, `object X` (Kotlin singleton), `module.exports = new X()` (Node module singleton), `__new__` override in Python metaclass, `@singleton` decorator (Python).
- **Factory Method** — a class with a method that returns a new instance of an interface/abstract class, with the concrete type chosen by a subclass or by a parameter. Markers: `createX()` method returning `IX`, `@classmethod` factory in Python, `from_*` class methods, `make()` functions.
- **Abstract Factory** — a class that returns a family of related objects (multiple `create` methods returning related interfaces). Markers: `AbstractFactory` interface with multiple `createA()`, `createB()` methods; concrete factories implementing the interface.
- **Builder** — a class with a fluent API (`withX()`, `setY()`, `build()`) that constructs a complex object step-by-step. Markers: `Builder` class with `build()` method, `Director` class that calls builder methods in sequence, Lombok `@Builder`, Immutables `@Builder`, Kotlin `apply { }` block (lightweight builder), Proto/protobuf message builders.
- **Prototype** — a class with a `clone()` method or `ICloneable`/`Cloneable` implementation, or a copy constructor. Markers: `clone()`, `deepCopy()`, `copy()`, `__copy__`/`__deepcopy__` (Python), `MemberwiseClone` (C#), `Clone` trait (Rust).
- **Object Pool** — a class that maintains a pool of pre-allocated objects and lends them out. Markers: `acquire()`/`release()`, `pool` field, ` commons-pool` (Java), `Apache Commons Pool`, connection pools (cross-reference ART-20).

### 6.2 Structural-Pattern Detection

- **Adapter** — a class that implements an interface (`I-XX`) and holds an instance of an incompatible type (`K-XX`), delegating calls. Markers: `class Adapter implements Target { private Adaptee adaptee; }`, `class XAdapter : ITarget`, Python duck-typing wrappers, `@adapter` pattern, Go adapters wrapping external types.
- **Bridge** — two orthogonal hierarchies (abstraction + implementor) where the abstraction holds a reference to the implementor. Markers: `class Abstraction { protected Implementor impl; }`, `class RefinedAbstraction extends Abstraction`, Java JDBC `Driver`/`Connection` (classic bridge).
- **Composite** — a tree of `Component` instances where `Leaf` and `Composite` share an interface and `Composite` delegates to children. Markers: recursive type (a class holding a list of its own type or interface), `add(child)`, `remove(child)`, `getChildren()`, React component trees, AST node hierarchies, file-system hierarchies.
- **Decorator** — a class that implements an interface and holds an instance of the same interface, adding behavior. Markers: `class Decorator implements Component { protected Component wrapped; }`, Python decorators (`@decorator`), Java `InputStream`/`OutputStream` chain, TypeScript decorator factories, JavaScript higher-order functions wrapping functions.
- **Facade** — a class with a simplified API that hides a complex subsystem. Markers: a class with high fan-in (calls many subsystem classes) but low fan-out (called by few external classes), `class ServiceFacade { methodA() { sub1.do(); sub2.do(); } }`, NestJS provider that wraps multiple services.
- **Flyweight** — a class that shares intrinsic state across instances via a factory/cache. Markers: `FlyweightFactory.get(key)` returning cached instances, `intern()` (Java string pool), `Integer.valueOf` cache, Go `sync.Pool`.
- **Proxy** — a class that implements an interface and holds a reference to the real subject, controlling access. Variations: remote proxy, virtual proxy (lazy init), protection proxy (auth), smart proxy (caching/logging). Markers: `class Proxy implements Subject { private RealSubject real; }`, Java dynamic proxies (`Proxy.newProxyInstance`), C# `RealProxy`/`DispatchProxy`, ES6 `Proxy`, gRPC client stubs (remote proxy), Hibernate lazy-loading proxies.

### 6.3 Behavioral-Pattern Detection

- **Chain of Responsibility** — a sequence of handlers each deciding to handle or pass to the next. Markers: `Handler` interface with `setNext()` and `handle()`; middleware chains (cross-reference ART-16); Express middleware; ASP.NET Core middleware; Java Servlet filters; CoR pattern in `chain_of_responsibility.py`.
- **Command** — an object encapsulating an operation with `execute()` (and optionally `undo()`). Markers: `interface Command { void execute(); }`, `Runnable` (Java), `Callable<T>`, C# delegates, Redux action objects `{ type, payload }`, event-sourced command objects, `@CommandHandler` (Axon).
- **Interpreter** — a class that interprets a grammar (rule engine, expression evaluator, regex). Markers: `Expression` interface with `interpret(Context)`, AST-walking interpreters, rule-engine implementations, SpEL (Spring Expression Language) interpreters.
- **Iterator** — a class with `hasNext()`/`next()` or `__iter__`/`__next__` (Python) or `IEnumerable`/`IEnumerator` (C#) or `Iterator<T>` (Java) or `Iter` (Rust). Markers: `Iterator` interface, generator functions (`yield`), `Symbol.iterator`, Java `Stream.iterator()`.
- **Mediator** — a class that coordinates communication between colleague objects, which do not communicate directly. Markers: `Mediator` interface, `Colleague` classes that hold a mediator reference, React Context as mediator, Redux store as mediator, event-bus patterns.
- **Memento** — a class that captures and restores another object's state. Markers: `Memento` class, `Originator.createMemento()` / `restore(memento)`, `undo()` stacks, serialization-based snapshots.
- **Observer** — a subject that notifies subscribers on state change. Markers: `subscribe()`/`unsubscribe()`/`notify()`, `addEventListener`/`removeEventListener`, `EventEmitter` (Node), `INotifyPropertyChanged` (C#), `PropertyChangeSupport` (Java), `Observable`/`Flowable` (Reactor), `@Subscribe` (Guava EventBus), reactive streams in general.
- **State** — a class whose behavior changes based on a state object. Markers: `State` interface, `Context` class with `setState()`, state machines (cross-reference ART-13's `S-XX`), `enum State` dispatch, `@StateMachine` (Spring StateMachine), XState machines.
- **Strategy** — a class that delegates to a strategy object implementing an interface. Markers: `Strategy` interface, `Context.setStrategy(strategy)`, `Comparator<T>` (Java), `IComparer<T>` (C#), policy-based design (C++ templates), `sort(key=...)` (Python).
- **Template Method** — an abstract class with a final method calling abstract hook methods. Markers: `abstract class Template { final void templateMethod() { step1(); step2(); } abstract void step1(); }`, Go's `sort.Interface` (template method via interface), Spring `JdbcTemplate`, React `componentDidMount` lifecycle hooks.
- **Visitor** — a class with visit methods for each element type, dispatching on the element's concrete type. Markers: `Visitor` interface with `visitConcreteElementA()`, `visitConcreteElementB()`, `accept(visitor)` method on elements, AST visitors, Babel/TypeScript AST transformers, Roslyn `CSharpSyntaxVisitor`.

### 6.4 Architectural-Pattern Detection

Architectural patterns are detected at the module and component level, not at the class level. Each instance records `pattern_id` `P-XX`, `category: architectural`, `name`, `variation`, `participants` (list of `M-XX`), `location`, `rationale_kind`, `rationale`, `citation`.

- **MVC (Model-View-Controller)** — detected by `models/`, `views/`, `controllers/` directory structure; Spring `@Controller`/`@Repository`/`Model`; Rails `ActiveRecord` + `ActionView` + `ActionController`; Django MTV variant (Model-Template-View); ASP.NET MVC.
- **MVP (Model-View-Presenter)** — detected by `Presenter` classes that hold a `View` interface and a `Model`, common in Android (deprecated in favor of MVVM) and WinForms.
- **MVVM (Model-View-ViewModel)** — detected by `ViewModel` classes (often extending `AndroidViewModel`, `ObservableObject`, or implementing `INotifyPropertyChanged`), data-binding syntax (`@Binding`, `{{ }}`, `[(ngModel)]`), Kotlin `ViewModel`, SwiftUI `@State`/`@Binding`.
- **Layered (N-tier)** — detected by `presentation/`, `business/`, `data/`, `persistence/` layer separation; Spring's `controller`/`service`/`repository` stereotype; Clean Architecture variants.
- **Hexagonal (Ports and Adapters)** — detected by `port` interfaces and `adapter` implementations, Alistair Cockburn's structure, `application/`/`domain/`/`infrastructure/` directories, NestJS modules with provider tokens.
- **Clean Architecture** — detected by `entities/`, `use_cases/`, `interface_adapters/`, `frameworks/` directories; dependency rule (dependencies point inward); Uncle Bob's structure.
- **CQRS** — detected by separate `Command` and `Query` services/handlers; `@CommandHandler`/`@QueryHandler` decorators (Axon, MediatR); separate write and read models.
- **Event Sourcing** — detected by event stores (`event_store` table, `events` topic), `@Aggregate` with `@EventSourcingHandler`, append-only event logs, snapshot tables.
- **Microservices** — detected by multiple independently-deployable services, each with its own entry point (`ART-05`), its own data store (when applicable), and inter-service communication via HTTP/gRPC/message-stream. Cross-reference ART-02's IaC and ART-04's CI for deployment topology.
- **Serverless** — detected by serverless-framework configs (`serverless.yml`), AWS Lambda handlers (`exports.handler`), Azure Functions (`@FunctionName`), Google Cloud Functions, Vercel/Netlify function directories.
- **Plugin** — detected by plugin directories (`plugins/`, `extensions/`), plugin loaders (`loadPlugin()`, `registerPlugin()`), plugin manifests (`plugin.json`, `package.json` with `plugin` field), VSCode extensions, Webpack/Vite/Rollup plugins, Babel plugins, ESLint plugins.

### 6.5 Concurrency-Pattern Detection

- **Active Object** — a class with a method that enqueues work onto a private queue and returns a future. Markers: `class ActiveObject { private Queue<Runnable> queue; Future<T> method() { queue.add(() -> ...); } }`, Actor-like decoupling.
- **Monitor** — methods synchronized on `this` or an explicit lock; `synchronized` keyword (Java), `@Synchronized` (Kotlin), `lock`/`RLock` (Python), `sync.Mutex` (Go), `std::sync::Mutex` (Rust), `lock` statement (C#).
- **Reactor** — event-loop demultiplexing I/O events to handlers. Markers: `Selector` (Java NIO), `epoll`/`kqueue` (Node libuv, Go runtime, Tokio), `Selector` (.NET), event-loop frameworks (Netty, Node, asyncio, Tokio, Vert.x).
- **Thread Pool** — `ExecutorService` (Java), `ThreadPoolExecutor` (Python), `Worker` pools (Node), `sync.Pool` + goroutines (Go), `rayon` thread pool (Rust), `ThreadPool` (.NET).
- **Producer-Consumer** — a queue between producers and consumers; cross-reference ART-22's streaming workflows.
- **Read-Write Lock** — `ReentrantReadWriteLock` (Java), `RwLock` (Rust, Python), `sync.RWMutex` (Go), `ReaderWriterLockSlim` (C#).
- **Barrier** — `CyclicBarrier` (Java), `Barrier` (.NET), `sync.WaitGroup` (Go, related), `JoinHandle` (Rust).
- **Future / Promise** — `Future<T>` (Java), `Promise` (JavaScript), `Task<T>` (C#), `Future` (Rust), `asyncio.Future` (Python), `CompletableFuture` (Java).
- **Actor** — Akka actors (`AbstractActor`, `ActorRef`), Erlang/Elixir processes, Actix (Rust), Protoactor (Go), Microsoft Orleans (`Grain`).

### 6.6 AI-Specific-Pattern Detection (cross-reference ART-21)

Cross-reference ART-21's `agents` and `workflows` to detect AI-specific pattern instances:

- **ReAct Pattern** — every `AG-XX` with `architecture: react` or `react-with-tools` is a ReAct pattern instance. Participant: the agent class `K-XX`. Variation: with-tools (when `parallel_tool_calls` or tool dispatch is present) or without-tools.
- **Plan-and-Execute Pattern** — every `AG-XX` with `architecture: plan-and-execute`. Participants: the planner `FN-XX` (or `K-XX`), the executor `FN-XX` (or `K-XX`). Variation: hierarchical (when sub-agents are dispatched) or flat.
- **Reflexion Pattern** — every `AG-XX` with `architecture: reflexion`. Participants: the actor `FN-XX`, the evaluator `FN-XX`, the reflector `FN-XX`.
- **Tool-Use Pattern** — every `W-XX` with `kind: tool-use` (or any workflow with a non-empty `tool_loops`). Participants: the LLM client `CL-XX`, the tool registry `K-XX`, the dispatch function `FN-XX`.
- **Memory Pattern** — every `MEM-XX` in ART-21 is a Memory pattern instance. Variation: short-term, long-term-semantic, long-term-episodic, vector-store, external-service. Participants: the memory class `K-XX` (when applicable), the read/write functions `FN-XX`.
- **RAG Pattern** — every `RAG-XX` in ART-21 is a RAG pattern instance. Variation: naive, hyde, rag-fusion, flare, self-rag, corrective-rag. Participants: the chunker `FN-XX`, the embedder `CL-XX`, the retriever `FN-XX`, the reranker `FN-XX`, the generator `W-XX`.
- **Multi-Agent Pattern** — every `AG-XX` with `architecture: multi-agent`. Participants: the orchestrator `FN-XX`, the sub-agents `K-XX`.

When ART-21 is `ABSENT`, skip § 6.6 entirely and record `OQ-XX: "ART-21 ABSENT; AI-specific pattern analysis skipped"` as an Open Question. AI-specific patterns are the only optional category; the other five categories (§ 6.1–§ 6.5) always execute.

### 6.7 Participant Identification

For each detected pattern instance, identify the participants. Participants are the entities that realize the pattern's roles. Each participant records: `participant_id` `PT-XX`, `role` (the pattern role, e.g., "Singleton", "Adapter", "Adaptee", "Subject", "ConcreteStrategy"), `entity_kind` (Class | Interface | Function | Module | Component | Workflow | Memory | RAG | Agent), `entity_id` (`K-XX`, `I-XX`, `FN-XX`, `M-XX`, `C-XX`, `W-XX`, `MEM-XX`, `RAG-XX`, `AG-XX`), `citation`.

Participants MUST resolve to entities in ART-06/ART-07/ART-08/ART-09/ART-21. A participant that does not resolve is `EXTERNAL_UNRESOLVED` per R21 and is recorded as a `CONTRADICTION` finding per R33.

### 6.8 Location Identification

For each detected pattern instance, identify the location. Location is the file-system position where the pattern is realized. Each instance records: `location_kind` (file | class | function | module), `location_id` (`F-XX`, `K-XX`, `FN-XX`, `M-XX`), `file_path`, `line_range`, `symbol`.

### 6.9 Variation Classification

Classify each instance's variation. Variations are pattern-specific and enumerated in § 6.1–§ 6.5. When the instance does not match a known variation, record `variation: custom` with a one-sentence description in `variation_notes`.

### 6.10 Rationale Inference

Infer the rationale for each pattern instance. The rationale MUST be one of two kinds:

- **Documented** — the rationale is stated in code comments, ADRs, README files, or commit messages. The citation MUST point to the documenting artifact (e.g., `docs/adr/0007-use-repository-pattern.md:1-40`, `// We use a Singleton here because ...`). Documented rationales are preferred.
- **Inferred** — the rationale is inferred from structural evidence (the pattern's typical purpose, the surrounding code's constraints). Inferred rationales MUST be hedged with "inferred from <evidence>" and MUST cite the structural evidence. Example: "Repository pattern inferred from `UserRepository` class implementing `IRepository<User>` interface; rationale: abstraction over persistence to enable testing and persistence-technology swaps."

Inferred rationales that go beyond the pattern's typical purpose are forbidden (R22). Example: inferring "Repository pattern chosen for performance reasons" without evidence is forbidden; the Repository pattern's typical purpose is abstraction, not performance.

### 6.11 Mermaid Pattern-Instance Diagram Emission

Emit Mermaid diagrams per `OUTPUT_RULES.md` § 7. Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

- **Per-pattern-instance class diagram** — for each significant pattern instance, a `classDiagram` showing the participants and their relationships (inheritance, implementation, association, aggregation). Nodes: `K-XX`, `I-XX`. Edges labeled with the relationship type per ART-10's relationship kinds.
- **Per-category overview diagram** — one `graph LR` per category (creational, structural, behavioral, architectural, concurrency, AI-specific) showing all instances in the category as nodes, with edges to shared participants.
- **Master pattern map** — a `graph LR` showing every pattern instance `P-XX` and its primary participant `K-XX`/`M-XX`, with edges for cross-pattern participation (a class participating in multiple patterns). Decomposed by module when > 30 nodes.

### 6.12 Coverage Cross-Check

Cross-check the pattern catalog against ART-08's class catalog and ART-10's call graph:

1. Compute `K_08` = set of `K-XX` in ART-08.
2. Compute `K_23` = set of `K-XX` appearing as a participant in any pattern instance in ART-23.
3. Expected: `K_23 ⊆ K_08` (every pattern participant is a real class). Participants in `K_23 \ K_08` are `CONTRADICTION` findings per R33.
4. Cross-check that each architectural pattern's participant modules (`M-XX`) resolve to ART-06's module map. Unresolved modules are `CONTRADICTION` findings.
5. Cross-check AI-specific patterns against ART-21 when ART-21 is present; every `AG-XX`, `MEM-XX`, `RAG-XX` referenced by an AI pattern instance MUST exist in ART-21.

---

## 7. Required Outputs

### ART-23 — Design Pattern Catalog

**Type:** Doc.

**Acceptance Criteria:**

- AC-23.1: The artifact file exists at `<output_root>/artifacts/phase3/ART23_<engagement_id>_design-patterns.md`.
- AC-23.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-23.3: The body contains: Executive Summary, Methodology, Creational Patterns, Structural Patterns, Behavioral Patterns, Architectural Patterns, Concurrency Patterns, AI-Specific Patterns (or "Skipped: ART-21 absent"), Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-23.4: Every pattern instance records `category`, `name`, `variation`, `participants`, `location`, `rationale_kind`, `rationale`, `citation`.
- AC-23.5: Every participant resolves to an entity in ART-06/ART-07/ART-08/ART-09/ART-21 or is `EXTERNAL_UNRESOLVED`.
- AC-23.6: Every Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-23.7: A `.mmd` sidecar file exists for every Mermaid block.
- AC-23.8: Coverage cross-check is recorded with no unresolved contradictions.

---

## 8. Output Templates

### 8.1 ART-23 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-23
artifact_type: Doc
producing_prompt: PROMPT_23
phase: 3
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
pattern_instances:
  - id: P-01
    category: creational | structural | behavioral | architectural | concurrency | ai-specific
    name: <pattern-name>
    variation: <variation-name> | custom
    variation_notes: <text> | NA
    participants:
      - participant_id: PT-01
        role: <role-name>
        entity_kind: Class | Interface | Function | Module | Component | Workflow | Memory | RAG | Agent
        entity_id: K-XX | I-XX | FN-XX | M-XX | C-XX | W-XX | MEM-XX | RAG-XX | AG-XX | EXTERNAL_UNRESOLVED
        citation: <file>:<line-range>
    location:
      kind: file | class | function | module
      id: F-XX | K-XX | FN-XX | M-XX
      file_path: <path>
      line_range: <start-end>
      symbol: <name>
    rationale_kind: documented | inferred
    rationale: <text>
    rationale_evidence: <file>:<line-range> | NA
    citation: <file>:<line-range>
coverage_cross_check:
  classes_in_art08: [K-XX]
  classes_in_art23: [K-XX]
  unresolved_participants: [PT-XX]
  contradictions: [{ kind: <text>, participant: PT-XX, detail: <text> }]
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

### 8.2 ART-23 Body Skeleton

```markdown
# ART-23: Design Pattern Catalog

## 1. Executive Summary
## 2. Methodology
## 3. Creational Patterns
   ### 3.1 P-01: Singleton (variation: thread-safe)
   **Diagram D-01: P-01 Singleton Structure**
   ```mermaid
   classDiagram
       class K001 {
           -instance: K001
           -K001()
           +getInstance() K001
       }
   ```
   - Participants: PT-01 (Singleton: K-001)
   - Location: src/config/Config.ts:14-42
   - Rationale (documented): "Thread-safe singleton to ensure single config instance across worker pool" (src/config/Config.ts:14)
## 4. Structural Patterns
## 5. Behavioral Patterns
## 6. Architectural Patterns
## 7. Concurrency Patterns
## 8. AI-Specific Patterns
   (or "## 8. AI-Specific Patterns — Skipped: ART-21 absent" with OQ reference)
## 9. Coverage Cross-Check
## 10. Traceability Index
## 11. Open Questions
## 12. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every class in ART-08 is examined for pattern participation; classes with no pattern recorded are listed in a `NO_PATTERN` register (not a finding). Threshold ≥ 0.90 of classes examined.
- **Q2. Citation Check** — ≥ 0.95 of pattern instances cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no pattern instance contradicts ART-08's class catalog or ART-10's call graph.
- **Q5. UNVERIFIED Accounting** — every `custom` variation and `inferred` rationale has a corresponding Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.1 on a 5% sample of classes yields the same pattern set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-23.A. Participant Resolution** — every `PT-XX` resolves to an entity in ART-06/ART-07/ART-08/ART-09/ART-21 or is `EXTERNAL_UNRESOLVED` with an Open Question.
- **Q-23.B. Rationale-Kind Discrimination** — every pattern instance records `rationale_kind: documented | inferred`. A `documented` rationale MUST cite the documenting artifact; an `inferred` rationale MUST cite the structural evidence. A rationale without a citation is a `MAJOR` finding.
- **Q-23.C. No Over-Inference** — inferred rationales are restricted to the pattern's typical purpose (e.g., Singleton → "single instance"; Repository → "persistence abstraction"). Inferring atypical purposes (e.g., Singleton → "performance") without evidence is a `MAJOR` finding per R22.
- **Q-23.D. AI-Specific Closure** — when ART-21 is present, every `AG-XX`, `MEM-XX`, `RAG-XX` referenced by an AI-specific pattern instance MUST exist in ART-21. Mismatches are `CONTRADICTION` findings per R33.
- **Q-23.E. Architectural-Pattern Module Resolution** — every architectural pattern's participant modules resolve to ART-06's module map. Unresolved modules are `CONTRADICTION` findings.
- **Q-23.F. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment per `OUTPUT_RULES.md` § 7.5.
- **Q-23.G. Sidecar Files** — every Mermaid block has a corresponding `.mmd` file under `<output_root>/diagrams/`.
- **Q-23.H. Variation Specificity** — `variation: custom` instances are < 10% of the catalog; an excess suggests the detection heuristics are missing known variations.

---

## 10. Common Pitfalls

- Do not infer a pattern from naming alone; `UserFactory` may be a factory or may be a class with a misleading name. Verify structurally (does it return an interface? do subclasses override the creation method?).
- Always distinguish Abstract Factory from Factory Method; Abstract Factory returns a family of related objects, Factory Method returns a single object type. Conflating them misleads PROMPT_24's engineering-decision analysis.
- Do not record a generic class as a pattern instance; every class has *some* relationship to patterns, but only deliberate instantiations count. If evidence is thin, mark `variation: ad-hoc` and `rationale_kind: inferred` with hedged language.
- Always prefer documented rationales; before recording an inferred rationale, search for ADRs (`docs/adr/`, `docs/architecture/`), commit messages (`git log` if available), and inline comments (`// We use X because ...`). Documented rationales are first-class evidence.
- Do not classify Spring's `@Service` as a pattern instance by itself; `@Service` is a stereotype, not a pattern. The pattern is the layered architecture that the stereotype participates in.
- Always cross-reference architectural patterns against ART-06's module map; an architectural pattern without participant modules is incomplete per Q-23.E.
- Do not omit AI-specific patterns when ART-21 is present; omitting ReAct, RAG, or Memory patterns that ART-21 records is a `CONTRADICTION` per Q-23.D.
- Always record `EXTERNAL_UNRESOLVED` for participants that reference external library classes; do not invent a `K-XX` for a library class per R21.
- Do not record two pattern instances for the same structural realization; one structural configuration realizes one pattern (the most specific applicable). Overlapping instances (e.g., both Composite and Decorator for the same class) require evidence that both patterns are deliberately in play.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders the diagrams from the sidecar source.

---

## 11. Handoff Criteria

PROMPT_24, PROMPT_26, and PROMPT_25 consume ART-23. Handoff requires ALL of:

- HC-23.1: ART-23 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-23.2: Every pattern instance records `category`, `name`, `variation`, `participants`, `location`, `rationale_kind`, `rationale`, `citation`.
- HC-23.3: Every participant resolves to an entity in ART-06/ART-07/ART-08/ART-09/ART-21 or is `EXTERNAL_UNRESOLVED` with an Open Question.
- HC-23.4: AI-specific patterns are cataloged (or ART-21 is `ABSENT` with an Open Question).
- HC-23.5: Mermaid diagrams are emitted with `.mmd` sidecar files; diagrams > 30 nodes are decomposed.
- HC-23.6: Coverage cross-check is recorded with no unresolved contradictions.
- HC-23.7: `repository_fingerprint_recheck` matches ART-01.
- HC-23.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_24 (Engineering Decisions — consumes ART-23's pattern instances and rationales as engineering-decision evidence), PROMPT_25 (Diagram Generation — re-renders the Mermaid sources), PROMPT_26 (Rebuild Guide — consumes ART-23's pattern vocabulary to preserve the original architectural idiom in the rebuild), PROMPT_28 (Cross-Reference Checklists — verifies that every `P-XX` referenced by ART-26 resolves to an entry in ART-23).
- **Depends on:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-06 (PROMPT_06), ART-07 (PROMPT_07), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-21 (PROMPT_21).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22 (no behavior invention — restricts inferred rationales), R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4 (sidecar files), § 3.1, § 4, § 6, § 7 (Mermaid conventions, edge citations, ≤ 30 nodes, decomposition).
- **Forward reference:** PROMPT_30 (Self-Review & QA) verifies that every `P-XX` referenced by ART-26 resolves to an entry in ART-23, and that no `inferred` rationale violates R22.

*End of PROMPT_23. Orchestrator may dispatch PROMPT_24 upon satisfaction of § 11.*
