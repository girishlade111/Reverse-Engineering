# PROMPT_07.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_07: Component Architecture Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_07
- **Phase:** 1
- **Stage:** 7 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-03 (PROMPT_03), ART-06 (PROMPT_06).
- **Estimated Tokens:** 12000–18000
- **Output Artifacts:** ART-07 (Map) — Component Map.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Component Map (ART-07) that identifies every UI component (React, Vue, Svelte, Angular, Solid, web components), every service component (service-oriented and microservice architectures), every plugin and middleware component, and records each component's type, hierarchy, props/state, composition relationships, and service-boundary contracts.

---

## 3. When to Invoke

PROMPT_07 is dispatched when ALL of the following predicates hold:

- PROMPT_06 has emitted a Completion Record satisfying its Handoff Criteria.
- ART-01, ART-02, ART-03, and ART-06 are present and non-empty.
- ART-02 records at least one frontend framework (React, Vue, Svelte, Angular, Solid, Astro, Next.js, Nuxt, Remix) OR at least one backend framework indicative of a service-oriented architecture (Spring Boot, NestJS, FastAPI, Django, Go gin/echo, etc.).

If no frontend and no service framework is detected, PROMPT_07 emits a `NO_COMPONENTS_DETECTED` Completion Record with `status: SUCCESS` and an empty component map; downstream prompts proceed.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; fingerprint re-verification. |
| ART-02 | Manifest | Detected frameworks; drives the per-framework detection procedure in § 6.1. |
| ART-03 | Map | File roles; `source` files under `components/`, `ui/`, `pages/`, `views/`, `services/`, `controllers/` are candidate component hosts. |
| ART-06 | Map | Module boundaries; components are scoped to a single module. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R16, R17, R21, R22 (no behavior invention). |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation, and Mermaid conventions. |
| `QUALITY_STANDARDS.md` | Framework file | Apply Map schema (`§ 4.3`) and type-specific bar (aggregate ≥ 30, Coverage ≥ 4, Precision ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect components per § 6.1 by framework, using ART-02's framework list.
3. For every UI component, extract props per § 6.2 and state per § 6.3.
4. For every UI component, extract lifecycle hooks per § 6.4.
5. Construct the component hierarchy per § 6.5.
6. Detect composition patterns per § 6.6 (HOCs, render props, hooks, slots, compound components).
7. For every service component, extract its boundary contract per § 6.7.
8. For every plugin and middleware component, classify per § 6.8.
9. Compute component metrics per § 6.9.
10. Emit ART-07 per § 8 with full front-matter, component catalog, hierarchy diagrams, composition patterns, service contracts, metrics, traceability index, open questions.
11. Run the Quality Checks in § 9.
12. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Component Detection by Framework

#### 6.1.1 React

A React component is any function or class declared in a `.tsx`/`.jsx` file whose return type is `JSX.Element` (or `React.ReactNode`) or whose body returns JSX. Detection markers:

- `export function <Name>(props) { return <jsx>; }` where `<Name>` begins with an uppercase letter.
- `export const <Name> = (props) => <jsx>;` where `<Name>` begins uppercase.
- `class <Name> extends React.Component` or `class <Name> extends React.PureComponent`.
- `forwardRef((props, ref) => <jsx>)` — wrapped component.
- `memo((props) => <jsx>)` — memoized component.

Each component is recorded with `component_id` (`C-XX`), `name`, `file`, `line_range`, `kind` (function | class | forwardRef | memo), `module_id` (per ART-06).

#### 6.1.2 Vue

- `.vue` Single-File Components (SFCs) — every `.vue` file is a component; the `name` field in the `<script>` block or the filename (PascalCase) is the component name.
- `defineComponent({ ... })` — composition API component.
- `export default { name: 'X', ... }` — options API component.
- Components registered via `app.component('name', def)` — global components.

#### 6.1.3 Svelte

- Every `.svelte` file is a component.
- The component name is the PascalCased filename.
- `<script>` block content defines props (`export let x`), state (`let x`), and reactive statements (`$: ...`).

#### 6.1.4 Angular

- `@Component({...}) class <Name>` — every decorated class is a component.
- `@Directive({...}) class <Name>` — every decorated class is a directive (recorded as a component with `kind: directive`).
- `@Injectable({...}) class <Name>` — every decorated class is a service (recorded as a service component per § 6.7).
- `@Pipe({...}) class <Name>` — every decorated class is a pipe.

#### 6.1.5 Solid

- `function <Name>(props) { return <jsx>; }` — Solid components follow React-like syntax but use Solid's reactivity primitives (`createSignal`, `createMemo`, `createEffect`).

#### 6.1.6 Next.js / Nuxt pages

- Next.js `pages/` files exporting a React component — these are page components with route associations.
- Next.js `app/` directory `page.tsx` — App Router page components.
- Nuxt `pages/` files exporting a Vue component — page components.

#### 6.1.7 Service Components (Backend)

A service component is a class or module that encapsulates a bounded piece of business logic and exposes a contract (interface) to its collaborators. Detection markers:

- Spring `@Service`, `@Repository`, `@Component` classes.
- NestJS `@Injectable()` classes (excluding those already classified as middleware/guards/pipes/interceptors).
- Django services — classes in `services/` directories; the convention is structural, not annotation-based.
- FastAPI services — classes decorated with `@lru_cache` or instantiated as dependencies via `Depends()`.
- Go services — structs in `internal/` or `pkg/` packages that implement an interface; the interface is the contract.
- Rust services — structs implementing a trait; the trait is the contract.
- .NET services — classes registered in the DI container (per ART-05) via `services.AddScoped<IX, X>()`; `IX` is the contract.
- Rails services — classes under `app/services/` (ActiveRecord-Record pattern).

Each service component is recorded with `component_id` (`C-XX`), `name`, `file`, `line_range`, `kind: service`, `contract_interface` (`I-XX` per ART-08, or `UNVERIFIED` if ART-08 has not run), `module_id`.

#### 6.1.8 Plugin Components

- Webpack/Vite/Rollup plugins — objects with `name` and `apply()` or hook methods (`resolveId`, `transform`, `load`).
- Babel plugins — functions returning objects with `visitor` property.
- ESLint plugins — objects with `rules`, `configs`, `processors` properties.
- Babel presets, PostCSS plugins, Tailwind plugins — similar shape.
- Editor plugins (VSCode extensions) — classes activating on `activate()`.

Each plugin is recorded with `component_id`, `name`, `file`, `kind: plugin`, `host_tool` (the plugin's host: webpack, vite, babel, eslint, etc.).

#### 6.1.9 Middleware Components

Middleware components are functions or classes that wrap the request/response cycle. Detection markers per ART-05 § 6.6 — every middleware registered via `app.use()`, `app.add_middleware()`, `e.Use()`, `router.Use()`, `@Middleware()`, or listed in Django `MIDDLEWARE` is a middleware component.

Each middleware is recorded with `component_id`, `name`, `file`, `kind: middleware`, `registration_order` (per ART-05).

### 6.2 Props Extraction (UI Components)

For every UI component, extract its props:

#### React

- Function components — destructure from the `props` parameter; record each prop's name and (where TypeScript is used) its type from the props interface.
- Class components — `this.props.<name>`; the props interface is `MyComponentProps`.
- `forwardRef` — the second parameter is the ref; the props are the first parameter.
- The props interface is detected by parsing the TypeScript generic `React.FC<Props>` or the destructured-parameter type annotation.

#### Vue

- Options API — `props: { name: Type, ... }` or `props: ['name', ...]`.
- Composition API — `defineProps<{ ... }>()` or `defineProps({ ... })`.
- The `required` and `default` fields are recorded per prop.

#### Svelte

- `export let x` declarations in the `<script>` block.
- `export let x = default` — default value recorded.
- Type annotations come from TypeScript Svelte (`<script lang="ts">`).

#### Angular

- `@Input() x: Type;` — input property.
- `@Input('alias') x: Type;` — aliased input.
- `@Output() e = new EventEmitter<Type>();` — output (event).

For each prop, record `name`, `type`, `required`, `default_value`, `description` (from JSDoc/TSDoc comment if present, else `UNVERIFIED`).

### 6.3 State Extraction (UI Components)

For every UI component, extract its state:

#### React (Hooks)

- `useState<T>(initial)` — each call yields a state slice with `name` (the first destructured variable), `type`, `initial_value` (or `UNVERIFIED` for dynamic initials).
- `useReducer(reducer, initial)` — state slice with `reducer` (`FN-XX`), `initial`.
- `useRef(initial)` — ref state; recorded as `kind: ref`.
- `useContext(Context)` — context state; recorded with `context_symbol`.
- `useMemo(() => ..., deps)` — memoized value; recorded as `kind: memo`.
- Class components — `this.state = { ... }` in the constructor; each key is a state slice.

#### Vue (Composition API)

- `ref(initial)` — reactive ref.
- `reactive(initial)` — reactive object.
- `computed(() => ...)` — computed value.
- `watch(source, cb)` — watcher (recorded as a side-effect, not state).

#### Vue (Options API)

- `data() { return { ... } }` — each returned key is a state slice.
- `computed: { ... }` — computed values.
- `watch: { ... }` — watchers.

#### Svelte

- `let x = initial` — reactive state.
- `$: y = derived` — computed.
- `writable(initial)` (from `svelte/store`) — store subscription.

#### Angular

- Class properties assigned in the constructor or `ngOnInit` are state.
- `BehaviorSubject` and `ReplaySubject` instances are state stores.

#### Solid

- `createSignal(initial)` — signal state.
- `createStore(initial)` — store state.
- `createMemo(() => ...)` — memoized value.

For each state slice, record `name`, `kind` (state | ref | context | memo | computed | store), `type`, `initial_value` (or `UNVERIFIED`), `citation`.

### 6.4 Lifecycle Hook Extraction (UI Components)

For every UI component, extract its lifecycle hooks:

#### React (Function Components with Hooks)

- `useEffect(() => { ... }, deps)` — mount/update/unmount effect; the cleanup function is the unmount hook.
- `useLayoutEffect` — synchronous layout effect.
- `useInsertionEffect` — DOM insertion effect (rare).

#### React (Class Components)

- `componentDidMount()` — post-mount.
- `componentDidUpdate(prevProps, prevState)` — post-update.
- `componentWillUnmount()` — pre-unmount.
- `shouldComponentUpdate(nextProps, nextState)` — update gate.
- `getDerivedStateFromProps(props, state)` — derived state.
- `getSnapshotBeforeUpdate(prevProps, prevState)` — pre-commit snapshot.

#### Vue (Options API)

- `beforeCreate`, `created`, `beforeMount`, `mounted`, `beforeUpdate`, `updated`, `beforeUnmount`, `unmounted`, `errorCaptured`, `activated`, `deactivated`.

#### Vue (Composition API)

- `onMounted`, `onUpdated`, `onUnmounted`, `onErrorCaptured`, `onActivated`, `onDeactivated`, `onBeforeMount`, `onBeforeUpdate`, `onBeforeUnmount`.

#### Svelte

- `onMount(() => ...)`, `onDestroy(() => ...)`, `beforeUpdate`, `afterUpdate`, `tick()`.

#### Angular

- `ngOnInit`, `ngOnChanges`, `ngDoCheck`, `ngAfterContentInit`, `ngAfterContentChecked`, `ngAfterViewInit`, `ngAfterViewChecked`, `ngOnDestroy`.

For each lifecycle hook, record `name`, `phase` (mount | update | unmount | error), `symbol` (`FN-XX`), `citation`.

### 6.5 Component Hierarchy Construction

Construct the component hierarchy by analyzing JSX/template usage:

- For every component `C`, scan every other component's JSX/template for usage of `<C ...>` (or `<c ...>` for kebab-case).
- For Vue, scan `<template>` blocks and `components: { C }` registrations.
- For Angular, scan templates for `<app-c ...>` selectors (the selector is in the `@Component({ selector: 'app-c' })` metadata).
- For Svelte, scan markup for `<C ...>` tags.
- For Solid, same as React (JSX).

Each parent-child relationship is an edge `(parent_C, child_C)` of type `CONTAINS` / `CONTAINED_IN` per `PROJECT_SPECIFICATION.md` § 4.2. Each edge cites the line where the child is used in the parent's JSX/template.

Page components (per § 6.1.6) are the roots of the hierarchy. Components used only inside other components are non-root. Components never used anywhere are `ORPHAN_COMPONENT` and flagged.

### 6.6 Composition Pattern Detection

Detect composition patterns:

- **Higher-Order Components (HOCs)** — functions of the form `function withX(Component) { return function Wrapped(props) { ... <Component ... /> ... } }` or `const withX = (Component) => (props) => <Component ... />`. Detection: a function that takes a component and returns a component.
- **Render Props** — components that accept a prop of type function (e.g., `render`, `children` as function) and call it inside their JSX.
- **Custom Hooks** (React) — functions whose name starts with `use` and that call other hooks. Recorded as `kind: hook` (not `kind: function`); each hook is a `C-XX` with `kind: hook`.
- **Slots** (Vue) — `<slot>` tags in templates; named slots (`<slot name="x">`) recorded with `slot_name`.
- **Slots** (Svelte) — `<slot>` tags; named slots.
- **Slots** (Web Components) — `<slot>` tags in shadow DOM templates.
- **Compound Components** — sets of components designed to be used together (e.g., `<Select>`, `<Select.Option>`, `<Select.OptionGroup>`). Detected by naming convention (`<Parent>.<Child>` static property assignment) and by shared context.
- **Context Providers** (React) — `<Context.Provider value={...}>` usage; the provider component is recorded with `kind: context-provider`.
- **Custom Directives** (Vue) — `app.directive('name', { ... })` registrations.
- **Content Projection** (Angular) — `<ng-content>` tags.

Each composition pattern is recorded with `pattern_id`, `kind`, `participants` (`C-XX` list), `evidence` (`file:line`).

### 6.7 Service Boundary Contract Extraction

For every service component, extract its boundary contract:

1. Identify the service's public API — the methods exposed via the implementing class that are part of its interface (per ART-08). Where the service implements an interface (`I-XX`), the contract is the interface's method set.
2. Identify the service's collaborators — the other services or repositories it injects (via constructor injection, `@Autowired`, `Depends()`, `inject()`, `services.AddScoped<IX>()`).
3. Identify the service's data contracts — the DTOs, request/response types, and entities it accepts and returns. These are `K-XX` or `I-XX` references resolved in PROMPT_08.
4. Identify the service's error contract — the exceptions it throws or the error types it returns.
5. Identify the service's persistence interactions — repository calls, ORM queries, cache accesses. These are cross-referenced to PROMPT_20 (Database & Persistence).

Each service contract is recorded with `contract_id`, `service_id` (`C-XX`), `public_methods` (`FN-XX` list), `collaborators` (`C-XX` list), `data_contracts` (`K-XX` / `I-XX` list), `error_contract`, `persistence_interactions` (cross-reference to PROMPT_20, recorded as `UNVERIFIED` here).

### 6.8 Plugin and Middleware Classification

For every plugin component, record:

- `host_tool` — webpack, vite, babel, eslint, etc.
- `hooks` — the hooks the plugin registers (e.g., `resolveId`, `transform`, `load` for Vite).
- `applies_to` — the build phases the plugin affects.

For every middleware component, record:

- `registration_order` (per ART-05).
- `applies_to` — the routes or route groups the middleware protects (parsed from the registration call's arguments).
- `modifies_request` — whether the middleware mutates the request object (true|false|UNVERIFIED).
- `modifies_response` — whether the middleware mutates the response (true|false|UNVERIFIED).
- `short_circuits` — whether the middleware can short-circuit the pipeline (return without calling `next()`).

### 6.9 Component Metrics

For every component, compute:

- `prop_count` — number of props.
- `state_slice_count` — number of state slices.
- `lifecycle_hook_count` — number of lifecycle hooks.
- `child_component_count` — number of child components used.
- `line_count` — total source lines.
- `depth` — distance from the nearest page/root component.
- `reused_by_count` — number of distinct parent components that use this component.

---

## 7. Required Outputs

### ART-07 — Component Map

**Type:** Map.

**Acceptance Criteria:**

- AC-07.1: The artifact file exists at `<output_root>/artifacts/phase1/ART07_<engagement_id>_component-map.md`.
- AC-07.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.3.
- AC-07.3: The body contains: Executive Summary, Methodology, Component Catalog, Component Hierarchy (Mermaid), Props & State, Lifecycle Hooks, Composition Patterns, Service Contracts, Plugins & Middleware, Component Metrics, Traceability Index, Open Questions, Cross-References.
- AC-07.4: Every component has `component_id`, `name`, `file`, `line_range`, `kind`, `module_id`.
- AC-07.5: Every UI component has props extracted (or `NO_PROPS` documented).
- AC-07.6: Every service component has a contract recorded (or `UNVERIFIED` documented).
- AC-07.7: Every parent-child component relationship is an edge with a citation to the JSX/template line.
- AC-07.8: Mermaid diagrams are emitted per `OUTPUT_RULES.md` § 7.
- AC-07.9: Graphs larger than 30 components are decomposed into sub-graphs.

---

## 8. Output Templates

### 8.1 ART-07 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-07
artifact_type: Map
producing_prompt: PROMPT_07
phase: 1
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
components:
  - component_id: C-01
    name: <PascalCase-name>
    file: <path>
    line_range: <start-end>
    kind: function | class | forwardRef | memo | vue-sfc | svelte | angular-component | angular-directive | angular-pipe | service | plugin | middleware | hook | context-provider | page
    framework: react | vue | svelte | angular | solid | next | nuxt | spring | nestjs | fastapi | django | go-gin | go-echo | aspnet | rails | none
    module_id: M-XX
    props:
      - name: <name>
        type: <type-string>
        required: true | false | UNVERIFIED
        default_value: <value> | none | UNVERIFIED
        description: <text> | UNVERIFIED
        citation: <file>:<line>
    state_slices:
      - name: <name>
        kind: state | ref | context | memo | computed | store
        type: <type-string>
        initial_value: <value> | UNVERIFIED
        citation: <file>:<line>
    lifecycle_hooks:
      - name: <name>
        phase: mount | update | unmount | error
        symbol: FN-XX
        citation: <file>:<line>
    metrics:
      prop_count: <int>
      state_slice_count: <int>
      lifecycle_hook_count: <int>
      child_component_count: <int>
      line_count: <int>
      depth: <int>
      reused_by_count: <int>
composition_patterns:
  - pattern_id: CP-01
    kind: hoc | render-prop | hook | slot | compound | context-provider | custom-directive | content-projection
    participants: [C-XX]
    evidence: <file>:<line>
service_contracts:
  - contract_id: SC-01
    service_id: C-XX
    public_methods: [FN-XX]
    collaborators: [C-XX]
    data_contracts: [K-XX | I-XX]
    error_contract: <text> | UNVERIFIED
    persistence_interactions: UNVERIFIED | [PROMPT_20 ref]
plugins:
  - plugin_id: PL-01
    component_id: C-XX
    host_tool: webpack | vite | babel | eslint | postcss | tailwind | other
    hooks: [<name>]
    applies_to: [<phase>]
middleware_components:
  - middleware_id: MW-01
    component_id: C-XX
    registration_order: <int>
    applies_to: [<route-pattern> | *]
    modifies_request: true | false | UNVERIFIED
    modifies_response: true | false | UNVERIFIED
    short_circuits: true | false | UNVERIFIED
component_edges:
  - from: C-01
    to: C-02
    relationship: CONTAINS
    evidence: <file>:<line>
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
  - id: C-01
    label: <name>
    kind: component
    parent_id: C-XX | null
    depth: 0
edges:
  - from: C-01
    to: C-02
    relationship: CONTAINS
    evidence: <file_path>:<line-range>
---
```

### 8.2 ART-07 Body Skeleton

```markdown
# ART-07: Component Map

## 1. Executive Summary
## 2. Methodology
## 3. Component Catalog
   ### 3.1 UI Components
   ### 3.2 Service Components
   ### 3.3 Plugin Components
   ### 3.4 Middleware Components
## 4. Component Hierarchy
   **Diagram D-01: Component Tree (Overview)**
   ```mermaid
   graph TD
       C01[C-01: App] --> C02[C-02: Router]
       C02 --> C03[C-03: HomePage]
   ```
## 5. Props & State
## 6. Lifecycle Hooks
## 7. Composition Patterns
## 8. Service Contracts
## 9. Plugins & Middleware
## 10. Component Metrics
## 11. Traceability Index
## 12. Open Questions
## 13. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every UI component declared in the in-scope set is recorded; threshold ≥ 0.95.
- **Q2. Citation Check** — ≥ 0.95 of claims cited.
- **Q3. Schema Conformance Check** — validates against § 4.3.
- **Q4. Non-Contradiction Check** — no component classification contradicts ART-02's framework declarations (a React component is not classified as `framework: vue`).
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` prop description and `UNVERIFIED` service contract has an Open Question.
- **Q6. Idempotence Spot-Check** — re-detecting components in a 5% sample yields the same set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-07.A. Component Naming** — every React/Solid component name begins uppercase; every Vue/Svelte component name is PascalCase; violations are flagged.
- **Q-07.B. Hierarchy Acyclic** — the `CONTAINS` hierarchy is acyclic (verified by topological sort).
- **Q-07.C. Props Citation** — every prop has a citation to the line that declares it.
- **Q-07.D. Orphan Detection** — components with `reused_by_count: 0` and not page/root components are flagged `ORPHAN_COMPONENT`.
- **Q-07.E. Service Contract Method Set** — every service component has at least one public method (or `kind: data-service` with documented CRUD operations).

---

## 10. Common Pitfalls

- Do not infer props from PropTypes alone; cross-check with TypeScript types when present. PropTypes are runtime checks; TypeScript types are the authoritative contract.
- Always distinguish `useState` calls by their destructured names; recording a single `useState` slot misses the component's actual state shape.
- Do not classify a custom hook as a component; hooks are recorded with `kind: hook`, not `kind: function`.
- Always record the JSX usage line as the hierarchy edge citation; a parent-child relationship without a usage citation is `UNVERIFIED`.
- Do not invent service contracts when none are declared; if a service class has no interface, the contract is `implicit` and the public method set is the contract.
- Always cross-check Angular `@Injectable` services against ART-05's DI container; a service not registered in the container is `ORPHAN_SERVICE`.
- Do not collapse compound components into a single entry; `<Select>`, `<Select.Option>`, and `<Select.OptionGroup>` are distinct `C-XX` entries with composition relationships.
- Always record the slot name for named slots; an unnamed slot is the default slot.
- Do not infer lifecycle behavior from naming; a method named `componentDidMount` in a non-component class is not a lifecycle hook.
- Always distinguish page components from non-page components; page components are roots of the hierarchy and carry route associations (cross-referenced to ART-05).

---

## 11. Handoff Criteria

PROMPT_08, PROMPT_10, and PROMPT_25 consume ART-07. Handoff requires ALL of:

- HC-07.1: ART-07 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-07.2: Every UI component has props and state extracted (or `NO_PROPS` / `NO_STATE` documented).
- HC-07.3: Every service component has a contract recorded (or `UNVERIFIED` documented).
- HC-07.4: Component hierarchy is acyclic and emitted as a Mermaid graph.
- HC-07.5: Composition patterns are enumerated.
- HC-07.6: Plugins and middleware are classified.
- HC-07.7: `repository_fingerprint_recheck` matches ART-01.
- HC-07.8: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_08 (Class & Interface — uses component classes and prop interfaces as a seed for class documentation), PROMPT_10 (Call & Dependency Graph — uses component edges as part of the dependency graph), PROMPT_25 (Diagram Generation — re-renders the component hierarchy at higher fidelity).
- **Depends on:** ART-01 (PROMPT_01), ART-02 (PROMPT_02), ART-03 (PROMPT_03), ART-06 (PROMPT_06).
- **Governing rules:** `OPERATING_RULES.md` R13, R16, R17, R21, R22.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.3; Map bar (aggregate ≥ 30, Coverage ≥ 4, Precision ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 3.1, § 4, § 6, § 7 (Mermaid graphs).
- **Forward reference:** PROMPT_30 verifies that every component in ART-07 has at least one downstream artifact reference (in ART-08 or ART-10).

*End of PROMPT_07. Orchestrator may dispatch PROMPT_08 upon satisfaction of § 11.*
