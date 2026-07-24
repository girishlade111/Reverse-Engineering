# PROMPT_08.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_08: Class & Interface Documentation

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_08
- **Phase:** 1
- **Stage:** 8 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-03 (PROMPT_03), ART-06 (PROMPT_06), ART-07 (PROMPT_07).
- **Estimated Tokens:** 13000–20000
- **Output Artifacts:** ART-08 (Doc) — Class & Interface Reference.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Class & Interface Reference (ART-08) that documents every in-scope class, interface, struct, trait, protocol, abstract type, mixin, and enumeration with its responsibilities, public API, inheritance hierarchy, implemented interfaces, collaborators, lifecycle, invariants, and design-pattern markers — the latter flagged for PROMPT_23's detailed pattern analysis.

---

## 3. When to Invoke

PROMPT_08 is dispatched when ALL of the following predicates hold:

- PROMPT_07 has emitted a Completion Record satisfying its Handoff Criteria.
- ART-01, ART-03, ART-06, and ART-07 are present and non-empty.
- ART-03 records at least one `source` file with a language that supports classes or interfaces (TypeScript, JavaScript with classes, Python, Java, Kotlin, Swift, C#, C++, Rust traits, Go interfaces, Ruby classes, PHP classes, Scala, Elixir modules/protocols).

If the subject is purely functional (e.g., a Haskell codebase) and contains no classes or interfaces, PROMPT_08 emits a `NO_CLASSES_DETECTED` Completion Record with `status: SUCCESS` and an empty reference; downstream prompts proceed.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; fingerprint re-verification. |
| ART-03 | Map | File roles; `source` files are the analysis set. |
| ART-06 | Map | Module boundaries; classes are scoped to a module. |
| ART-07 | Map | Component classes (Angular, React class components, Vue class components) are pre-identified; ART-08 documents their structure. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R16, R17, R19, R21, R22. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation, and Mermaid class-diagram conventions. |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect classes and interfaces per § 6.1 across all in-scope `source` files.
3. For every class, extract its public API per § 6.2.
4. For every class, extract its inheritance hierarchy per § 6.3.
5. For every class, extract its implemented interfaces per § 6.4.
6. For every class, identify its collaborators per § 6.5.
7. For every class, infer its lifecycle per § 6.6.
8. For every class, infer its invariants per § 6.7.
9. For every interface, document its contract per § 6.8.
10. Detect abstract classes, mixins, traits, generics per § 6.9.
11. Detect design-pattern markers per § 6.10 and flag for PROMPT_23.
12. Emit ART-08 per § 8 with full front-matter, class catalog, interface catalog, hierarchy diagrams (Mermaid class diagrams), traceability index, open questions.
13. Run the Quality Checks in § 9.
14. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Class & Interface Detection

Detect classes and interfaces by language-specific syntax:

- **TypeScript** — `class <Name>`, `abstract class <Name>`, `interface <Name>`, `type <Name> = ...` (only if it has function signatures and is used as an interface), `enum <Name>`, `namespace <Name>`.
- **JavaScript (ES6+)** — `class <Name>`; no interfaces (use JSDoc `@typedef` and `@implements`).
- **Python** — `class <Name>:` (regular class), `class <Name>(ABC):` or `class <Name>(metaclass=ABCMeta):` (abstract); `Protocol` subclasses for structural typing; `TypedDict` for dict-typed interfaces; `Enum`/`IntEnum`/`StrEnum` subclasses; `dataclass` decorator; `attrs` decorator.
- **Java** — `class <Name>`, `abstract class <Name>`, `interface <Name>`, `@interface <Name>` (annotation), `enum <Name>`, `record <Name>(...)` (record class).
- **Kotlin** — `class <Name>`, `abstract class <Name>`, `interface <Name>`, `data class <Name>`, `sealed class <Name>`, `enum class <Name>`, `object <Name>` (singleton), `annotation class <Name>`.
- **Swift** — `class <Name>`, `struct <Name>`, `enum <Name>`, `protocol <Name>`, `actor <Name>` (Swift concurrency).
- **C#** — `class <Name>`, `abstract class <Name>`, `interface <Name>`, `struct <Name>`, `enum <Name>`, `record <Name>`, `record struct <Name>`, `delegate <Name>`.
- **C++** — `class <Name>`, `struct <Name>`, `enum class <Name>`, `union <Name>`; pure-virtual classes serve as interfaces.
- **Rust** — `struct <Name>`, `enum <Name>`, `trait <Name>`, `union <Name>`; trait objects and `impl Trait` are interface usage.
- **Go** — `type <Name> struct`, `type <Name> interface`, `type <Name> ...` (named types); Go has no classes; structs with methods serve as classes; interfaces are structural.
- **Ruby** — `class <Name>`, `module <Name>` (Ruby modules serve as namespaces and as mixins via `include`/`prepend`/`extend`).
- **PHP** — `class <Name>`, `abstract class <Name>`, `interface <Name>`, `trait <Name>`, `enum <Name>`.
- **Scala** — `class <Name>`, `abstract class <Name>`, `trait <Name>`, `case class <Name>`, `object <Name>`, `enum <Name>`.
- **Elixir** — `defmodule <Name>` (modules serve as namespaces and as struct containers); `defprotocol <Name>` (protocols serve as interfaces); `defimpl <Protocol>, for: <Type>`.
- **Clojure** — `defrecord`, `deftype`, `defprotocol`, `definterface`.

Each detected entity is assigned an ID per its kind:

- Classes and structs → `K-XX`.
- Interfaces, protocols, traits (when used purely as contracts) → `I-XX`.
- Enums → `K-XX` with `kind: enum`.
- Mixins (Ruby modules, Scala traits used as mixins, PHP traits) → `I-XX` with `kind: mixin`.

Each entity records `name`, `file`, `line_range`, `kind`, `module_id` (per ART-06), `language`.

### 6.2 Public API Extraction

For every class, extract its public API:

- **Visibility rules per language:**
  - TypeScript/JavaScript — `public` (default), `protected`, `private` (the `#` prefix is also private). Public API = public members.
  - Python — members without `_` prefix are public; `_` prefix is protected (convention); `__` prefix is private (name-mangled).
  - Java — `public` members; package-private (no modifier) is recorded as `package-private`.
  - Kotlin — `public` (default), `internal`, `protected`, `private`.
  - Swift — `public`, `open`, `internal` (default), `private`, `fileprivate`.
  - C# — `public`, `internal` (default), `protected`, `private`, `protected internal`, `private protected`.
  - C++ — `public:` section members.
  - Rust — `pub` members; private by default.
  - Go — exported members (uppercase first letter); unexported are private.
  - Ruby — `public` section methods; `private` and `protected` are scoped.
  - PHP — `public`, `protected`, `private`.
  - Scala — `public` (no modifier), `protected`, `private`.

For each public member, record:

- `member_id` — `FN-XX` for methods, `V-XX` for fields/properties, `K-XX` for nested types.
- `name`.
- `kind` — method | field | property | nested-type.
- `signature` — the full type signature (parameter types + return type).
- `static` — true|false.
- `readonly` / `final` / `const` — true|false|NA.
- `citation` — `<file>:<line>`.

For Python dataclasses and attrs classes, the generated `__init__`, `__repr__`, `__eq__` methods are recorded as `kind: method` with `generated: true` and `generator_hint: dataclass` or `generator_hint: attrs`.

### 6.3 Inheritance Hierarchy Extraction

For every class, extract its inheritance chain:

- **Single inheritance** — `class X extends Y` (TS/JS/Java/C#), `class X(Y):` (Python), `class X : public Y` (C++), `class X extends Y` (PHP), `class X extends Y` (Scala), `class X : Y` (Swift, but Swift classes are single-inheritance).
- **Multiple inheritance** — Python `class X(A, B):`; C++ `class X : public A, public B`; Scala `class X extends A with B with C`.
- **Interface implementation** — `class X implements I` (TS/Java/C#), `class X implements I, J` (PHP), `class X : I` (C#).
- **Trait mixing** — Scala `class X extends A with B`; Ruby `class X; include A; include B; end`; PHP `class X { use TraitA, TraitB; }`.
- **Composition** (Go, Rust) — Go structs embed other structs; Rust structs have fields of other types; these are recorded as `composition` relationships, not inheritance.

For each inheritance relationship, record an edge of type `EXTENDS` / `EXTENDED_BY` per `PROJECT_SPECIFICATION.md` § 4.2. The edge cites the line where the inheritance is declared.

The inheritance hierarchy is recorded as a Mermaid class diagram per `OUTPUT_RULES.md` § 7.4.

### 6.4 Interface Implementation Extraction

For every class, extract its implemented interfaces:

- **Java/Kotlin/Scala/C#/TypeScript** — explicit `implements` lists.
- **Go** — implicit; the class implements an interface if its method set is a superset of the interface's method set. Detect by checking that every interface method has a matching class method.
- **Rust** — explicit `impl <Trait> for <Type>` blocks; detect these.
- **Python** — explicit `class X(ABC)` or implicit Protocol conformance (checked via structural typing); for `Protocol`, conformance is structural.
- **Ruby** — implicit duck-typing; no formal interface implementation; record `kind: duck-typed` and the methods that form the implicit contract.
- **PHP** — explicit `implements` lists.
- **Elixir** — `defimpl <Protocol>, for: <Type>` blocks.

Each implementation relationship is an edge of type `IMPLEMENTS` / `IMPLEMENTED_BY`. The edge cites the line where the implementation is declared (or, for Go/Python Protocol, the line where the method that fulfills the contract is declared).

### 6.5 Collaborator Identification

For every class, identify its collaborators — the other classes, interfaces, and modules it interacts with:

- **Constructor injection** — constructor parameters of types other than primitives; the parameter types are collaborators.
- **Field injection** — fields typed as classes/interfaces (Spring `@Autowired` field injection).
- **Method parameters** — non-primitive method parameters (recorded as transient collaborators).
- **Method return types** — non-primitive return types (recorded as collaborators).
- **Internal instantiation** — `new X()` calls in the class's methods (recorded as collaborators with `kind: instantiation`).
- **Static calls** — `X.method()` calls (recorded as collaborators with `kind: static-call`).

For each collaborator, record `collaborator_id` (`K-XX` / `I-XX`), `relationship` (injection | parameter | return-type | instantiation | static-call), `citation`.

### 6.6 Lifecycle Inference

For every class with state, infer its lifecycle:

- **Construction** — the constructor(s); each overload is recorded with its parameters and pre-conditions (inferred from validation in the constructor body).
- **Initialization** — post-construction initialization methods (`init()`, `setup()`, `@PostConstruct`, `ngOnInit`, `Initializable.initialize()`); each is recorded with its phase.
- **Active state** — the methods that operate on the class's state during its active lifetime.
- **Disposal** — destruction methods (`destroy()`, `close()`, `dispose()`, `@PreDestroy`, `ngOnDestroy`, `Disposable.dispose()`, `AutoCloseable.close()`, `Drop::drop()`, `__del__`); each is recorded with the resource it releases.
- **Pooling** — if the class is registered in a DI container with `AddSingleton` or `AddScoped` (per ART-05), record `lifecycle_scope: singleton | scoped | transient`.

Each lifecycle phase is recorded with `phase`, `symbol` (`FN-XX`), `citation`, `pre_conditions`, `post_conditions`.

### 6.7 Invariant Inference

For every class with state, infer its invariants — conditions that hold before and after every public method call:

- **Type-system invariants** — non-null fields (Kotlin `!`, Swift non-optional, TypeScript `!`), final/readonly fields (recorded as `IMMUTABLE_FIELD`).
- **Constructor-established invariants** — validations in the constructor body (e.g., `if (x < 0) throw`); each is recorded as an invariant with the validation's citation.
- **Method-maintained invariants** — assertions (`assert`, `check()`, `require()`, `invariant()`) in method bodies; each is recorded as an invariant.
- **State-machine invariants** — if the class implements a state machine (detected by enum-typed state field and state-transition methods), each state's invariants are recorded (cross-referenced to PROMPT_13).
- **Concurrent-access invariants** — `synchronized` blocks, `Lock` fields, `Mutex` fields, `@GuardedBy` annotations, `Send`/`Sync` trait bounds (Rust); each is recorded as a concurrency invariant.

Each invariant is recorded with `invariant_id`, `kind`, `expression` (informal description), `citation`, `enforcement` (type-system | runtime-check | convention).

### 6.8 Interface Contract Documentation

For every interface (including Go interfaces, Rust traits, Python Protocols, Ruby duck-typed contracts, Elixir protocols):

- **Method set** — the methods declared by the interface, each with its signature.
- **Semantic contract** — the documented behavior of each method, derived from docstring/JSDoc/KDoc/JavaDoc; if absent, `UNVERIFIED`.
- **Pre-conditions** — conditions that must hold before the method is called (derived from docstring or from validation in implementing classes).
- **Post-conditions** — conditions that hold after the method returns (derived from docstring or from observable behavior in implementing classes).
- **Error contract** — the exceptions/errors the method may raise (derived from `@throws` Javadoc, `raises` Python docstring, `Result<T, E>` Rust return type, `throws` Java declaration).
- **Implementers** — the list of classes that implement the interface (per § 6.4).
- **Default methods** — Java/Kotlin/Scala default methods, Rust trait default methods, Java 8+ default methods; recorded with the default implementation's citation.

### 6.9 Abstract Class, Mixin, Trait, Generic Detection

- **Abstract classes** — `abstract class` (TS/Java/C#/Kotlin/PHP/Scala), `ABC`/`ABCMeta` (Python), pure-virtual methods (C++). Each abstract method is recorded with `abstract: true` and no body citation.
- **Mixins** — Ruby `module` included in classes (record the included methods as added to the including class's API); PHP `trait` used in classes; Python mixins (classes designed to be subclassed with `super().method()` chains); Scala `trait` mixed in with `with`.
- **Generics** — `class X<T>`, `interface I<T>`, `List<T>` (TS/Java/C#/Kotlin/Scala), `Generic[T]` (Python with `TypeVar`), `class X<T>` (Swift), `template<typename T> class X` (C++), `struct X<T>` (Rust). Each type parameter is recorded with its bounds (`T extends I`, `T: Clone`, `where T: ...`).
- **Variance** — `out T` (Kotlin/C# covariance), `in T` (contravariance), `? extends T` (Java wildcard), `? super T`; recorded with the variance annotation.
- **Associated types** (Rust) — `type Item;` in a trait; recorded as an associated-type bound.
- **Higher-kinded types** (Scala) — `class X[F[_]]`; recorded with the higher-kinded parameter.

### 6.10 Design-Pattern Marker Detection

Detect design-pattern markers; each is recorded with `pattern_marker_id`, `pattern_name`, `evidence`, `class_ids`. PROMPT_23 (Design Pattern Identification) consumes these markers and performs the full pattern analysis.

| Pattern | Markers |
|---------|---------|
| Singleton | Class with private constructor, static `instance()` method, self-typed static field. |
| Factory Method | Class with method returning an interface/abstract type, with concrete subclasses overriding the method. |
| Abstract Factory | Class with multiple factory methods returning related products. |
| Builder | Class with fluent setter methods returning `this`, plus a `build()` method. |
| Prototype | Class with `clone()` method or `copy()` returning a new instance of self. |
| Adapter | Class implementing interface `I` and holding a reference to class `C` with incompatible methods. |
| Bridge | Two orthogonal hierarchies composed via a field reference. |
| Composite | Class implementing an interface, with a collection of the same interface as a field. |
| Decorator | Class implementing interface `I`, with constructor parameter of type `I`, delegating calls to the wrapped instance. |
| Facade | Class with a small public API that internally calls many subsystem classes. |
| Flyweight | Class with a static cache of instances, factory method returning cached instances. |
| Proxy | Class implementing interface `I`, delegating to a real instance but adding behavior (logging, caching, access control). |
| Chain of Responsibility | Class with a `next` field of the same type, methods calling `next.method()` when not handled. |
| Command | Class with a single `execute()` method, often with `undo()`. |
| Interpreter | Class hierarchy representing grammar rules with an `interpret()` method. |
| Iterator | Class implementing `next()`/`hasNext()` or `Iterator`/`Iterable` interface. |
| Mediator | Class holding references to many colleague classes, with methods that coordinate them. |
| Memento | Class with a nested `State`/`Memento` type and `save()`/`restore()` methods. |
| Observer | Class with `subscribe()`/`attach()`/`on()` methods and an event/callback field; or class extending `Subject`/`Observable`. |
| State | Class with a state field and methods that delegate to a state object. |
| Strategy | Class with a field of an interface type, settable at runtime, used in a method. |
| Template Method | Abstract class with a concrete method calling abstract methods. |
| Visitor | Class with `visit()` methods overloaded by visited type; visited classes with `accept(visitor)` method. |

Each marker is recorded but NOT definitively classified as a pattern instance; PROMPT_23 verifies the full pattern structure.

---

## 7. Required Outputs

### ART-08 — Class & Interface Reference (Doc)

**Type:** Doc.

**Acceptance Criteria:**

- AC-08.1: The artifact file exists at `<output_root>/artifacts/phase1/ART08_<engagement_id>_class-interface-ref.md`.
- AC-08.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-08.3: The body contains: Executive Summary, Methodology, Class Catalog, Interface Catalog, Inheritance Hierarchy (Mermaid class diagram), Implementation Map, Public API Reference, Collaborator Map, Lifecycle Catalog, Invariant Catalog, Abstract/Mixin/Trait/Generic Catalog, Design-Pattern Markers, Traceability Index, Open Questions, Cross-References.
- AC-08.4: Every in-scope class has `name`, `file`, `line_range`, `kind`, `module_id`, `public_api`, `inheritance`, `implements`, `collaborators`.
- AC-08.5: Every in-scope interface has `name`, `file`, `line_range`, `method_set`, `implementers`.
- AC-08.6: Every public-API member has a citation to its declaration.
- AC-08.7: Every inheritance and implementation relationship is an edge with a citation.
- AC-08.8: Every design-pattern marker cites the evidence line.
- AC-08.9: Mermaid class diagrams are emitted per `OUTPUT_RULES.md` § 7.4; large hierarchies are decomposed.

---

## 8. Output Templates

### 8.1 ART-08 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-08
artifact_type: Doc
producing_prompt: PROMPT_08
phase: 1
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
classes:
  - class_id: K-01
    name: <name>
    file: <path>
    line_range: <start-end>
    kind: class | abstract-class | struct | enum | singleton | record | dataclass
    language: <name>
    module_id: M-XX
    modifiers: [abstract | final | sealed | static | public | internal | private]
    generics:
      - parameter: T
        bound: <type> | none
        variance: in | out | invariant | NA
    public_api:
      methods: [FN-XX]
      fields: [V-XX]
      properties: [V-XX]
      nested_types: [K-XX | I-XX]
    inheritance:
      - parent: K-XX
        relationship: extends
        citation: <file>:<line>
    implements: [I-XX]
    collaborators:
      - collaborator_id: K-XX | I-XX
        relationship: injection | parameter | return-type | instantiation | static-call
        citation: <file>:<line>
    lifecycle:
      - phase: construction | initialization | active | disposal
        symbol: FN-XX
        citation: <file>:<line>
        pre_conditions: <text> | UNVERIFIED
        post_conditions: <text> | UNVERIFIED
      - lifecycle_scope: singleton | scoped | transient | NA
    invariants:
      - invariant_id: INV-01
        kind: type-system | constructor-established | method-maintained | state-machine | concurrent-access
        expression: <text>
        citation: <file>:<line>
        enforcement: type-system | runtime-check | convention
interfaces:
  - interface_id: I-01
    name: <name>
    file: <path>
    line_range: <start-end>
    kind: interface | protocol | trait | mixin | duck-typed-contract
    language: <name>
    module_id: M-XX
    method_set: [FN-XX]
    semantic_contract: <text> | UNVERIFIED
    pre_conditions: <text> | UNVERIFIED
    post_conditions: <text> | UNVERIFIED
    error_contract: <text> | UNVERIFIED
    implementers: [K-XX]
    default_methods: [FN-XX]
    associated_types: [<name>]
design_pattern_markers:
  - marker_id: PM-01
    pattern_name: <name>
    evidence: <file>:<line-range>
    class_ids: [K-XX]
    confidence: low | medium | high
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
  - id: K-01
    name: <name>
    type: class | interface | struct | enum
    location: <file_path>:<line-range>
    attributes: { ... }
---
```

### 8.2 ART-08 Body Skeleton

```markdown
# ART-08: Class & Interface Reference

## 1. Executive Summary
## 2. Methodology
## 3. Class Catalog
   ### 3.1 <Module M-XX>
## 4. Interface Catalog
## 5. Inheritance Hierarchy
   **Diagram D-01: Class Hierarchy (Module M-XX)**
   ```mermaid
   classDiagram
       K01 --|> K02 : extends
       K01 ..|> I01 : implements
   ```
## 6. Implementation Map
## 7. Public API Reference
## 8. Collaborator Map
## 9. Lifecycle Catalog
## 10. Invariant Catalog
## 11. Abstract/Mixin/Trait/Generic Catalog
## 12. Design-Pattern Markers
## 13. Traceability Index
## 14. Open Questions
## 15. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every in-scope class and interface declaration is recorded; threshold ≥ 0.95.
- **Q2. Citation Check** — ≥ 0.95 of claims cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no class classification contradicts ART-07 (an Angular `@Component` class is recorded as `kind: class` in ART-08 with a back-reference to its `C-XX`).
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` semantic contract, pre-condition, post-condition has an Open Question.
- **Q6. Idempotence Spot-Check** — re-detecting classes in a 5% sample yields the same set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-08.A. Public API Completeness** — every public method/field/property is recorded; private members are excluded (recorded as `kind: private` count only).
- **Q-08.B. Hierarchy Edge Citation** — every `EXTENDS` and `IMPLEMENTS` edge cites the declaration line.
- **Q-08.C. Invariant Citation** — every invariant has a citation; type-system invariants cite the field declaration with its modifier.
- **Q-08.D. Pattern Marker Confidence** — every marker has a `confidence` field; low-confidence markers are flagged for PROMPT_23 verification.
- **Q-08.E. Go Interface Conformance** — for Go interfaces, every implementing struct is verified by method-set comparison (recorded as `conformance_verified: true`).
- **Q-08.F. Generics Recording** — every generic class has its type parameters recorded with bounds and variance.

---

## 10. Common Pitfalls

- Do not document private members in the public API; the public API is the contract, not the implementation.
- Always cite the line where a class is declared, not the line where it is first used.
- Do not infer an inheritance relationship from naming; `class MyController` does not extend `Controller` unless the source says so.
- Always record Go interface conformance by method-set analysis; Go's structural typing means the implementer may not explicitly declare the relationship.
- Do not infer invariants from variable names alone; invariants must be evidenced by type annotations, runtime checks, or constructor validations.
- Always distinguish abstract methods from concrete methods; an abstract method has no body and is recorded with `abstract: true` and no body citation.
- Do not conflate mixins with inheritance; a Ruby `include M` adds `M`'s methods to the class but is not an `EXTENDS` relationship; record it as `kind: mixin-inclusion` (a distinct edge type).
- Always cross-check generics against the language's variance rules; a Kotlin `out T` is covariant and is recorded accordingly.
- Do not classify a design-pattern marker as a confirmed pattern instance; PROMPT_23 verifies the full pattern structure. The marker here is evidence that PROMPT_23 should investigate.
- Always record the lifecycle scope when the class is registered in a DI container; an `AddSingleton`-registered class has a different lifecycle than a `AddTransient`-registered class, and PROMPT_18 (Caching & Performance) consumes this distinction.
- Do not skip enum documentation; enums are classes with a fixed instance set and are recorded with their values.

---

## 11. Handoff Criteria

PROMPT_09, PROMPT_10, and PROMPT_23 consume ART-08. Handoff requires ALL of:

- HC-08.1: ART-08 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-08.2: Every in-scope class and interface is recorded.
- HC-08.3: Every class has a public API and inheritance record.
- HC-08.4: Every interface has a method set and implementers list.
- HC-08.5: Inheritance and implementation graphs are emitted as Mermaid class diagrams.
- HC-08.6: Design-pattern markers are flagged for PROMPT_23.
- HC-08.7: `repository_fingerprint_recheck` matches ART-01.
- HC-08.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_09 (Function-Level — uses public-API methods as the function set), PROMPT_10 (Call & Dependency Graph — uses `IMPLEMENTS` and `EXTENDS` edges), PROMPT_23 (Design Patterns — uses pattern markers as the seed for full pattern analysis).
- **Depends on:** ART-01 (PROMPT_01), ART-03 (PROMPT_03), ART-06 (PROMPT_06), ART-07 (PROMPT_07).
- **Governing rules:** `OPERATING_RULES.md` R13, R16, R17, R19, R21, R22.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 3.1, § 4, § 6, § 7 (Mermaid class diagrams).
- **Forward reference:** PROMPT_30 verifies that every class in ART-08 has at least one method appearing in ART-09's function reference.

*End of PROMPT_08. Orchestrator may dispatch PROMPT_09 upon satisfaction of § 11.*
