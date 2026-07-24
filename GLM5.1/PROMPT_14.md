# PROMPT_14.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_14: Event Workflow Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_14
- **Phase:** 2
- **Stage:** 4 of 10
- **Dependencies:** ART-01 (PROMPT_01), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-13 (PROMPT_13).
- **Estimated Tokens:** 11000–17000
- **Output Artifacts:** ART-14 (Doc) — Event Workflow Catalog.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Event Workflow Catalog artifact (ART-14) that identifies every event in the subject repository (domain events, integration events, UI events, system events), records each event's emitters, handlers, payloads, ordering guarantees, and delivery semantics (at-most-once, at-least-once, exactly-once), traces every event-driven workflow (sequence of events and handlers), detects every event bus, queue, and pub/sub system, and detects every idempotency mechanism.

---

## 3. When to Invoke

PROMPT_14 is dispatched when ALL of the following predicates hold:

- Phase 1 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.2.
- ART-01, ART-09, ART-10, and ART-13 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- ART-09 records at least one function with side-effect kind `event-emit` OR ART-10 records at least one `EMITS`/`HANDLES` edge OR ART-13 records at least one transition with `trigger_kind: event` (else `NO_EVENT_SYSTEM` and the prompt emits a minimal catalog with that finding).

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-09 | Doc | Function side-effect records; functions with `event-emit` side effects are emitter candidates; functions registered as event listeners are handler candidates. |
| ART-10 | Graph | `EMITS`/`HANDLES`/`EMITTED_BY`/`HANDLED_BY` edges; these are the raw event-graph edges aggregated into workflows. |
| ART-13 | Doc | State-machine catalog; transitions with `trigger_kind: event` indicate state changes driven by events, which feed the workflow reconstruction. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid sequence-diagram conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). HOOK-02 (Orphan Event) is enforced. |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect events per § 6.1 by scanning emit/call patterns across all in-scope source files.
3. For every event, enumerate emitters per § 6.2 and handlers per § 6.3.
4. Extract every event payload per § 6.4 with its type, fields, and serialization format.
5. Infer delivery semantics per § 6.5 (at-most-once, at-least-once, exactly-once) by inspecting the transport and acknowledgment patterns.
6. Determine ordering guarantees per § 6.6 (FIFO, causal, total, none).
7. Reconstruct event-driven workflows per § 6.7 by chaining handler-emit pairs.
8. Detect event buses, queues, and pub/sub systems per § 6.8.
9. Detect idempotency mechanisms per § 6.9.
10. Emit Mermaid sequence diagrams per § 6.10 for every non-trivial workflow with message-level citations.
11. Cross-check the event catalog against ART-09's `event-emit` side effects and ART-13's event-triggered transitions per § 6.11; unaccounted events are `CONTRADICTION` findings per R33.
12. Emit ART-14 per § 8 with full front-matter, per-event sections, workflow catalog, transport catalog, idempotency catalog, traceability index, open questions.
13. Run the Quality Checks in § 9.
14. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Event Detection

Detect events by scanning for emit/dispatch/publish call patterns. Each event is assigned an `E-XX` identifier and records `event_id`, `name`, `kind` (domain | integration | UI | system | lifecycle | log | metric), `scope` (in-process | cross-process | distributed), `definition_location` (`file:line-range, symbol`).

**Event-detection patterns by category:**

- **Custom event emitters (in-process)** — `EventEmitter.emit(name, ...)`, `events.emit(name, ...)`, `EventBus.publish(event)`, `dispatch(action)` (Redux), `store.dispatch(action)`, `subject.next(event)` (RxJS), `Mitt.emit(name, data)`, `pubsub.publish(name, data)`, `eventAggregator.publish(name, data)` (Aurelia), `Signal.dispatch(event)` (Julia), `NSNotificationCenter.post(name:)` (iOS), `LiveEventBus.get(key).post(data)` (Android).
- **Custom event handlers** — `EventEmitter.on(name, handler)`, `EventEmitter.addListener()`, `addEventListener(name, handler)`, `events.subscribe(name, handler)`, `@Subscribe` (Guava), `@EventListener` (Spring), `@receiver` (Python blinker), `on(name, handler)`, `mitt.on(name, handler)`, `subscribe(name, handler)`, `NotificationCenter.default.addObserver(forName:)`.
- **Message queues (cross-process)** — `channel.publish(exchange, routingKey, content)`, `channel.consume(queue, handler)` (RabbitMQ/AMQP), `kafkaTemplate.send(topic, message)`, `@KafkaListener(topics)` (Spring Kafka), `producer.send(topic, message)` (Kafka producer), `consumer.subscribe(topics)`, `consumer.poll()` (Kafka consumer), `sns.publish(...)`, `sqs.sendMessage(...)`, `lambda.invoke(...)` (AWS), `pubsub.topic.publish()`, `pubsub.subscription.on(...)` (Google Pub/Sub), `redis.publish(channel, message)`, `redis.subscribe(channels)` (Redis Pub/Sub), `redis.xadd(stream, fields)`, `redis.xread(streams)` (Redis Streams), `NATS.publish(subject, data)`, `NATS.subscribe(subject, handler)`.
- **Domain event frameworks** — `DomainEvent` subclasses with `@EventListener` (Spring), `AggregateRoot.applyEvent(event)` (Axon), `eventStore.append(event)`, `dispatch(event)` (Python `eventsourcing`), `Aggregate.event_apply(event)` (Python), `EventSourcedRepository.save(aggregate)`.
- **UI events (browser/native)** — DOM events: `element.addEventListener('click', handler)`, `element.onclick = handler`, `dispatchEvent(new CustomEvent('name', {detail}))`. React: `onClick={handler}`, `useEffect` subscription patterns. Vue: `v-on:click`, `@click`, `$emit('name', data)`. Angular: `@Output()`, `@Input()`, `(click)="handler()"`, `EventEmitter` service. Svelte: `on:click`, `createEventDispatcher`. Native iOS: `@IBAction`, target-action. Android: `View.OnClickListener`, `LiveData.observe()`.
- **System/lifecycle events** — `process.on('SIGTERM', handler)`, `process.on('exit', handler)`, `window.addEventListener('beforeunload')`, `applicationDidBecomeActive`, `application(_:didFinishLaunchingWithOptions:)`, `onCreate()`, `onDestroy()` (Android Activity), `@PostConstruct`, `@PreDestroy` (Spring), `ShutdownHook` (JVM), `signal.Notify(c, os.Interrupt)` (Go), `tokio::signal::ctrl_c()`, `atexit.register(handler)` (Python), `at_exit { ... }` (Ruby), `Drop` implementations (Rust), `Application.Enable`/`Disable` (C#).
- **Logging and metrics (treated as events)** — structured logs that emit semantic events (e.g., `logger.info('user.created', {userId})`) and metrics counters (e.g., `counter.inc({label})`) are recorded as `kind: log`/`kind: metric` events because they trigger downstream observability handlers.

### 6.2 Emitter Enumeration

For every event, enumerate its emitters — the functions that publish/dispatch/emit it. Each emitter records `emitter_id` `EM-XX`, `function_id` `FN-XX`, `emit_call_symbol` (e.g., `channel.publish`), `emit_call_citation` (`file:line-range`), `payload_expression` (the source expression passed as the event payload, ≤ 300 characters), `emit_kind` (sync-dispatch | async-publish | fire-and-forget | request-reply).

For dynamic emitters (where the event name is computed at runtime, e.g., `emitter.emit(dynamicName, data)`), record `event_name_resolution` as `DYNAMIC` and cite the resolution site. If the resolution cannot be statically determined, mark the event `UNVERIFIED_EMITTER` and record an Open Question.

### 6.3 Handler Enumeration

For every event, enumerate its handlers — the functions registered to receive it. Each handler records `handler_id` `HD-XX`, `function_id` `FN-XX`, `registration_symbol` (e.g., `EventEmitter.on`), `registration_citation` (`file:line-range`), `registration_kind` (static-registration | decorator-registration | dynamic-registration), `handler_signature` (the parameter types of the handler function), `consumes_payload: true|false`.

For dynamically-registered handlers (e.g., `emitter.on(getEventName(), handler)` where `getEventName()` is non-constant), record `registration_kind: dynamic-registration` and cite the registration call. If the handler function is anonymous (a lambda), assign it an `FN-XX` per ART-09's anonymous-function handling and cross-reference.

### 6.4 Payload Extraction

For every event, extract its payload schema. The payload schema records `payload_id` `PL-XX`, `event_id` `E-XX`, `format` (JSON | Protobuf | Avro | MessagePack | custom | opaque), `fields` (list of `{name, type, required, default}`), `serialization_call` (`file:line-range`), `deserialization_call` (`file:line-range`).

Payload schemas are extracted by:

1. Reading the emit call's payload expression and inferring the type from ART-08's class catalog or ART-09's signature catalog.
2. For library-defined payload types (e.g., Kafka's `ProducerRecord<K, V>`), extracting the `V` type parameter.
3. For schema-registry-managed events (Confluent Schema Registry, AWS EventBridge schema registry, Protobuf `.proto` files), cross-referencing the registered schema.
4. For opaque payloads (`Buffer`, `[]byte`, `string`, `object`), recording `format: opaque` and emitting an Open Question.

### 6.5 Delivery-Semantics Inference

Infer delivery semantics for every event by inspecting the transport and acknowledgment patterns:

- **At-most-once** — fire-and-forget emitters with no acknowledgment: UDP, `EventEmitter.emit` (in-process, no retry), log emissions, metric emissions. Detected by emit calls that do not await an acknowledgment and have no retry loop.
- **At-least-once** — emitters with acknowledgment and retry: AMQP with publisher confirms, Kafka with `acks=all` and idempotent producer, SQS, Pub/Sub, Redis Streams with consumer groups. Detected by `channel.waitForConfirms()`, `acks=all` config, retry loops around `publish()`.
- **Exactly-once** — at-least-once transport plus idempotency mechanism (per § 6.9) plus transactional outbox. Detected by the presence of an outbox table (`outbox`, `event_store`) written in the same transaction as the state mutation that triggered the event.
- **Request-reply** — emitters that block waiting for a response: RPC, `request`/`reply` queues, HTTP. Detected by `channel.sendToQueue(q, msg, {replyTo})`, `client.request(...)`.

Each event records `delivery_semantics` (at-most-once | at-least-once | exactly-once | request-reply | UNVERIFIED), `transport_id` (the `T-XX` from § 6.8), `acknowledgment_kind` (none | publisher-confirms | consumer-ack | transactional | UNVERIFIED), `retry_policy` (the retry configuration, when declared), `citation`.

### 6.6 Ordering Guarantees

Determine ordering guarantees for every event:

- **FIFO** — events on a single channel/queue are delivered in order. Detected by single-partition Kafka topics, single-consumer queues, in-process event emitters.
- **Causal** — events that have a causal relationship are delivered in causal order. Detected by causal-chain identifiers (`causationId`, `correlationId`).
- **Total order** — all consumers see events in the same order. Detected by single-partition topics with strict ordering config, total-order broadcast systems (Atomix, Paxos-based systems).
- **None** — no ordering guarantee. Detected by multi-partition Kafka topics without partition keys, multi-consumer queues, fan-out exchanges.

Each event records `ordering_guarantee` (FIFO | causal | total | none | UNVERIFIED), `partition_key_expression` (when Kafka-style partitioning is used), `citation`.

### 6.7 Workflow Reconstruction

Reconstruct event-driven workflows by chaining handler-emit pairs. A workflow is a sequence of events and handlers where each handler's execution emits the next event. Each workflow records `workflow_id` `W-XX`, `name`, `trigger_event` `E-XX`, `steps` (ordered list of `{event_id, handler_id, emit_id, citation}`), `total_steps`, `total_events`, `total_handlers`, `terminal_kind` (handler-returns-without-emitting | cycle-detected | depth-limit-reached | UNVERIFIED).

The reconstruction procedure uses a bounded DFS:

1. Seed the workflow with an initial event `E-XX` (an event with at least one emitter that is not a handler, OR an event triggered by an external stimulus).
2. For every handler `HD-XX` of the event, examine the handler's body and extract every event it emits (per ART-09's side-effect records and ART-10's `EMITS` edges).
3. Recurse into each emitted event; the workflow extends by one step per emit.
4. Bound the workflow at depth 20 steps; deeper workflows are marked `depth-limit-reached` with an Open Question.
5. Detect cycles (an event that re-emits itself directly or transitively) and mark the workflow `cycle-detected` with the cycle's events listed.
6. When a handler returns without emitting, mark the workflow `handler-returns-without-emitting` (terminal).

### 6.8 Event-Bus / Queue / Pub-Sub Detection

Detect every event transport in the subject repository. Each transport records `transport_id` `T-XX`, `kind` (in-process-emitter | message-broker | streaming-platform | service-bus | log-bus | db-outbox), `framework` (e.g., `EventEmitter`, `RabbitMQ`, `Kafka`, `AWS SNS/SQS`, `Google Pub/Sub`, `Redis Pub/Sub`, `Redis Streams`, `NATS`, `Axon`, `EventStoreDB`), `client_instantiation_citation` (`file:line-range`), `topology` (topics, exchanges, queues, subscriptions — when declared statically), `external: false` for self-hosted or `external: true` for managed services.

### 6.9 Idempotency-Mechanism Detection

Detect every idempotency mechanism. Each mechanism records `idem_id` `IDM-XX`, `event_id` `E-XX`, `kind` (dedup-key | idempotency-key | outbox | consumer-ack-with-dedup | processed-events-table | hash-check), `implementation_symbol`, `citation`, `dedup_window` (when statically declared; otherwise `UNVERIFIED`).

**Detection patterns:**

- **Dedup key** — handler checks `if (processedKeys.has(event.id)) return;` before processing. Detected by `Set`/`Map`/`Cache` lookups keyed by event ID at the handler's entry.
- **Idempotency key header** — HTTP-style idempotency keys (`Idempotency-Key` header, Stripe-style). Detected by header reads at handler entry.
- **Transactional outbox** — events are written to a database `outbox` table in the same transaction as the state mutation, then a relay process publishes them. Detected by `outbox` table writes within transaction boundaries (cross-reference ART-13's synchronization mechanisms).
- **Processed-events table** — handler records each processed event ID in a `processed_events` table; subsequent deliveries are no-ops. Detected by `INSERT INTO processed_events ... ON CONFLICT DO NOTHING` patterns.
- **Hash check** — handler computes a hash of the event payload and compares to a stored hash; same hash = already processed. Detected by hashing calls and comparison branches at handler entry.

### 6.10 Mermaid Sequence-Diagram Emission

Emit Mermaid `sequenceDiagram` diagrams per `OUTPUT_RULES.md` § 7. Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

- **Per-workflow sequence diagram** — one diagram per workflow with `≥ 3` steps. Participants: `Emitter`, `Bus`, `Handler`, `State`. Messages labeled with the event name and citation (`file:line`). Async messages use `->>` (solid arrow) for emits and `-->>` (dashed arrow) for acknowledgments.
- **Per-event fan-out diagram** — for events with `≥ 3` handlers, a sequence diagram showing the fan-out from emitter to all handlers.
- **Transport topology diagram** — a `graph LR` showing every `T-XX` transport and the events that flow through it.

### 6.11 Coverage Cross-Check

Cross-check the event catalog against ART-09's `event-emit` side effects and ART-13's event-triggered transitions:

1. Compute the set `E_09` of events implied by ART-09's `event-emit` side effects (each side effect's `target_event` field).
2. Compute the set `E_13` of events that trigger state transitions in ART-13 (`trigger_kind: event` with the event name).
3. Compute the set `E_14` of events cataloged in ART-14.
4. The expected relationship: `E_14 ⊇ E_09 ∪ E_13` (every event referenced by ART-09 or ART-13 is cataloged in ART-14). Events in `(E_09 ∪ E_13) \ E_14` are `COVERAGE_GAP` findings.
5. Events in `E_14 \ (E_09 ∪ E_13)` are events cataloged in ART-14 that ART-09 and ART-13 do not reference; these are flagged for review (they may be UI events or system events that ART-09 does not record as side effects).

---

## 7. Required Outputs

### ART-14 — Event Workflow Catalog

**Type:** Doc.

**Acceptance Criteria:**

- AC-14.1: The artifact file exists at `<output_root>/artifacts/phase2/ART14_<engagement_id>_events.md`.
- AC-14.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-14.3: The body contains: Executive Summary, Methodology, Event Catalog, Emitter Catalog, Handler Catalog, Payload Schemas, Delivery Semantics, Ordering Guarantees, Workflow Catalog, Transport Catalog, Idempotency Mechanisms, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-14.4: Every event, emitter, handler, payload, transport, and idempotency mechanism cites its source.
- AC-14.5: Every Mermaid sequence diagram is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-14.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-14.7: Every event has at least one emitter or is flagged `ORPHAN_HANDLER_ONLY` (per HOOK-02).
- AC-14.8: Every event has at least one handler or is flagged `ORPHAN_EMITTER_ONLY` (per HOOK-02).
- AC-14.9: Coverage cross-check is recorded with no unresolved contradictions.

---

## 8. Output Templates

### 8.1 ART-14 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-14
artifact_type: Doc
producing_prompt: PROMPT_14
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
events:
  - id: E-01
    name: <name>
    kind: domain | integration | UI | system | lifecycle | log | metric
    scope: in-process | cross-process | distributed
    definition_location: <file>:<line-range>
emitters:
  - id: EM-01
    event_id: E-XX
    function_id: FN-XX
    emit_call_symbol: <name>
    emit_call_citation: <file>:<line-range>
    payload_expression: <text>
    emit_kind: sync-dispatch | async-publish | fire-and-forget | request-reply
handlers:
  - id: HD-01
    event_id: E-XX
    function_id: FN-XX
    registration_symbol: <name>
    registration_citation: <file>:<line-range>
    registration_kind: static-registration | decorator-registration | dynamic-registration
    handler_signature: <text>
    consumes_payload: true | false
payloads:
  - id: PL-01
    event_id: E-XX
    format: JSON | Protobuf | Avro | MessagePack | custom | opaque
    fields:
      - name: <name>
        type: <type>
        required: true | false
        default: <value> | none
    serialization_call: <file>:<line-range>
    deserialization_call: <file>:<line-range>
delivery_semantics:
  - event_id: E-XX
    semantics: at-most-once | at-least-once | exactly-once | request-reply | UNVERIFIED
    transport_id: T-XX
    acknowledgment_kind: none | publisher-confirms | consumer-ack | transactional | UNVERIFIED
    retry_policy: <text> | UNVERIFIED
    citation: <file>:<line-range>
ordering_guarantees:
  - event_id: E-XX
    guarantee: FIFO | causal | total | none | UNVERIFIED
    partition_key_expression: <text> | NA
    citation: <file>:<line-range>
workflows:
  - id: W-01
    name: <name>
    trigger_event: E-XX
    steps:
      - event_id: E-XX
        handler_id: HD-XX
        emit_id: EM-XX
        citation: <file>:<line-range>
    total_steps: <int>
    total_events: <int>
    total_handlers: <int>
    terminal_kind: handler-returns-without-emitting | cycle-detected | depth-limit-reached | UNVERIFIED
transports:
  - id: T-01
    kind: in-process-emitter | message-broker | streaming-platform | service-bus | log-bus | db-outbox
    framework: <name>
    client_instantiation_citation: <file>:<line-range>
    topology: <text>
    external: false
idempotency_mechanisms:
  - id: IDM-01
    event_id: E-XX
    kind: dedup-key | idempotency-key | outbox | consumer-ack-with-dedup | processed-events-table | hash-check
    implementation_symbol: <name>
    citation: <file>:<line-range>
    dedup_window: <text> | UNVERIFIED
coverage_cross_check:
  events_from_art09: [E-XX]
  events_from_art13: [E-XX]
  events_cataloged: [E-XX]
  coverage_gaps: [E-XX]
  catalog_only: [E-XX]
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

### 8.2 ART-14 Body Skeleton

```markdown
# ART-14: Event Workflow Catalog

## 1. Executive Summary
## 2. Methodology
## 3. Event Catalog
## 4. Emitter Catalog
## 5. Handler Catalog
## 6. Payload Schemas
## 7. Delivery Semantics
## 8. Ordering Guarantees
## 9. Workflow Catalog
   ### 9.1 W-01: <name>
   **Diagram D-01: W-01 Workflow**
   ```mermaid
   sequenceDiagram
       participant E as Emitter
       participant B as Bus
       participant H as Handler
       participant S as State
       E->>B: publish(event) (file:line)
       B->>H: deliver(event) (file:line)
       H->>S: mutate(state) (file:line)
       H->>B: publish(nextEvent) (file:line)
   ```
   <step list>
## 10. Transport Catalog
   **Diagram D-02: Transport Topology**
## 11. Idempotency Mechanisms
## 12. Coverage Cross-Check
## 13. Traceability Index
## 14. Open Questions
## 15. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every event implied by ART-09's `event-emit` side effects and ART-13's event-triggered transitions is cataloged; threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of events, emitters, handlers, payloads, transports, and idempotency mechanisms cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no event in ART-14 contradicts ART-09's `event-emit` records or ART-13's `trigger_kind: event` records.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED_EMITTER`, `UNVERIFIED` delivery semantic, `cycle-detected`, and `depth-limit-reached` entry has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.7 on a 5% sample of workflows yields the same step list.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-14.A. Orphan Event Check (HOOK-02)** — every event has at least one emitter and at least one handler; `ORPHAN_HANDLER_ONLY` and `ORPHAN_EMITTER_ONLY` are flagged.
- **Q-14.B. Payload Coverage** — every event with `scope: cross-process` or `scope: distributed` has a payload schema or is marked `opaque` with rationale.
- **Q-14.C. Delivery-Semantics Coherence** — every event with `scope: cross-process` or `scope: distributed` has a non-`UNVERIFIED` `delivery_semantics` entry or an Open Question.
- **Q-14.D. Workflow Bound** — no workflow exceeds 20 steps without being marked `depth-limit-reached` with an Open Question.
- **Q-14.E. Mermaid Message Citation** — every message in the Mermaid sequence diagrams has a `file:line` citation in the message label or the immediately preceding `%%` comment.
- **Q-14.F. Sidecar Files** — every Mermaid block has a `.mmd` sidecar file.
- **Q-14.G. Idempotency for Exactly-Once** — every event with `delivery_semantics: exactly-once` has at least one `IDM-XX` idempotency mechanism recorded.

---

## 10. Common Pitfalls

- Do not infer events from variable names alone; verify the emit call site and the handler registration per R22.
- Always record the payload schema for cross-process events; an opaque payload without rationale is non-conformant.
- Do not assume `at-least-once` semantics without inspecting the acknowledgment pattern; many "async" emitters are fire-and-forget (at-most-once).
- Always cross-check events against ART-09 and ART-13; an event referenced elsewhere but missing from ART-14 is a `COVERAGE_GAP`.
- Do not collapse distinct events that share a name across transports; an `order.created` event on Kafka and an `order.created` event on RabbitMQ are distinct `E-XX` entries.
- Always record the partition-key expression for Kafka topics; omitting it leaves the ordering model underspecified.
- Do not infer idempotency from naming; only mechanisms enforced by code (dedup keys, outbox, processed-events tables) are recorded per R22.
- Always distinguish in-process events (synchronous dispatch, no retry) from distributed events (potentially at-least-once); the distinction affects every downstream consumer.
- Always emit `.mmd` sidecar files for sequence diagrams; PROMPT_25 re-renders diagrams from the sidecar source.
- Do not record workflow cycles as failures; cycles are first-class findings and may indicate intentional feedback loops.
- Do not omit transport topology (topics, exchanges, queues); the topology is required for PROMPT_22 (Streaming Workflow) to identify consumer groups.

---

## 11. Handoff Criteria

PROMPT_16 and PROMPT_22 consume ART-14. Handoff requires ALL of:

- HC-14.1: ART-14 status is `REVIEWED` or `DRAFT` with orchestrator waiver.
- HC-14.2: Every event has at least one emitter and at least one handler (or is flagged per HOOK-02 with rationale).
- HC-14.3: Every cross-process and distributed event has a payload schema and delivery semantics (or `UNVERIFIED` with rationale).
- HC-14.4: Workflows are reconstructed for every event with `≥ 1` handler.
- HC-14.5: Transports and idempotency mechanisms are cataloged.
- HC-14.6: Mermaid sequence diagrams are emitted with `.mmd` sidecar files.
- HC-14.7: Coverage cross-check is recorded with no unresolved contradictions.
- HC-14.8: `repository_fingerprint_recheck` matches ART-01.
- HC-14.9: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_16 (Middleware & Pipeline — uses event handlers as middleware candidates for event-driven pipelines), PROMPT_22 (Streaming Workflow — uses transport catalog and ordering guarantees to identify streaming workflows).
- **Depends on:** ART-01 (PROMPT_01), ART-09 (PROMPT_09), ART-10 (PROMPT_10), ART-13 (PROMPT_13).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). HOOK-02 (Orphan Event) is enforced by PROMPT_30.
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 7.
- **Forward reference:** PROMPT_30 verifies HOOK-02 (every event has emitter + handler) and that every event referenced by ART-09, ART-13, ART-16, or ART-22 resolves to an entry in ART-14.

*End of PROMPT_14. Orchestrator may dispatch PROMPT_15 upon satisfaction of § 11.*
