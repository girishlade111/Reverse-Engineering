# PROMPT_22.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_22: Streaming Workflow Analysis

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_22
- **Phase:** 3
- **Stage:** 2 of 5
- **Dependencies:** ART-01 (PROMPT_01), ART-09 (PROMPT_09), ART-11 (PROMPT_11), ART-14 (PROMPT_14), ART-16 (PROMPT_16), ART-21 (PROMPT_21).
- **Estimated Tokens:** 12000–20000
- **Output Artifacts:** ART-22 (Doc) — Streaming Workflow Report.
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Streaming Workflow Report artifact (ART-22) that enumerates every streaming workflow in the subject repository — HTTP streaming, LLM token streaming, reactive streams, message-stream processing, and file/I/O streaming — identifies each workflow's producer, consumer, buffer strategy, backpressure strategy, error handling, and completion semantics, detects stream composition (map, filter, merge, split, window) and flow control, and records every citation so that a downstream engineer can rebuild a behaviorally equivalent streaming layer.

---

## 3. When to Invoke

PROMPT_22 is dispatched when ALL of the following predicates hold:

- Phase 2 exit conditions are satisfied per `PROJECT_SPECIFICATION.md` § 5.3 (PROMPT_20 emitted `status: SUCCESS` or `SKIPPED`).
- PROMPT_21 has emitted its completion record (`SUCCESS`, `SKIPPED`, or `BLOCKED` with orchestrator waiver). ART-21 may be `NOT_PRODUCED` under the skipped behavior; PROMPT_22 MUST degrade gracefully by treating ART-21 as `ABSENT` and proceeding with non-LLM streaming analysis.
- ART-01, ART-09, ART-11, ART-14, and ART-16 are present, non-empty, and have `status` of `REVIEWED` or `DRAFT` with orchestrator waiver.
- At least one streaming primitive is detected in the in-scope source per § 6.1; otherwise the prompt emits a `SKIPPED` completion record per § 3.1 and ART-22 is recorded as `NOT_PRODUCED`. Downstream consumers (PROMPT_24, PROMPT_25) MUST degrade gracefully.

### 3.1 Skipped Behavior (No Streaming Detected)

If § 6.1's marker detection returns an empty set under `SCOPE_FULL`, the prompt emits a `SKIPPED` completion record:

```
COMPLETION_RECORD {
  prompt_id: PROMPT_22,
  status: "SKIPPED",
  artifacts_produced: [],
  quality_checks_passed: [],
  quality_checks_failed: [],
  open_questions: [],
  handoff_ready: true,
  notes: "No streaming primitives detected in scope. ART-22 not produced. Downstream consumers MUST treat ART-22 as ABSENT and not require it for handoff."
}
```

Under `SCOPE_CORE` or `SCOPE_MODULE`, the orchestrator skips dispatch. Under `SCOPE_TRIAGE`, PROMPT_22 is never dispatched.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-09 | Doc | Function catalog; producer and consumer functions are detected by streaming-API call patterns. Async functions (`async fn`, `async def`, `Future`, `Promise`) are streaming-adjacent. |
| ART-11 | Graph | Data-flow diagrams; flows that carry streams (rather than discrete values) are the streaming workflows. Significant data types (`D-XX`) that are themselves stream types (e.g., `Observable<T>`, `Stream<T>`, `AsyncIterator<T>`) seed stream-type detection. |
| ART-14 | Doc | Event-workflow catalog; message-stream processing (Kafka, Kinesis, Pulsar, NATS, RabbitMQ) is identified by the transports recorded here. Each `E-XX` event emitted or handled via a stream transport is a streaming-workflow participant. |
| ART-16 | Doc | Middleware & pipeline map; HTTP middleware chains that stream responses (SSE, chunked) and reactive middleware are detected here. Middleware composition patterns inform stream-composition analysis. |
| ART-21 | Doc | AI/LLM workflow report; `streaming_usage` entries (per PROMPT_21 § 6.11) anchor the LLM-token-streaming workflow analysis. When ART-21 is `ABSENT`, LLM streaming analysis is skipped with an Open Question. |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (§ 4.5) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
2. Detect every streaming primitive per § 6.1 across all five categories (HTTP, LLM, reactive, message-stream, file/I/O).
3. If no markers are detected and the scope modifier is `SCOPE_FULL`, emit `SKIPPED` per § 3.1 and halt.
4. Enumerate every streaming workflow `SW-XX` per § 6.2 by clustering primitives into producer/consumer pairs.
5. Identify each workflow's producer per § 6.3.
6. Identify each workflow's consumer per § 6.4.
7. Extract each workflow's buffer strategy per § 6.5.
8. Extract each workflow's backpressure strategy per § 6.6.
9. Extract each workflow's error handling per § 6.7.
10. Extract each workflow's completion semantics per § 6.8.
11. Detect stream composition per § 6.9 (map, filter, merge, split, window, scan, debounce, throttle).
12. Detect flow control per § 6.10 (credit-based, rate-based, window-based, none).
13. Cross-reference ART-21's `streaming_usage` entries per § 6.11 to anchor LLM token streaming; IF ART-21 is `ABSENT`, skip LLM streaming analysis and record an Open Question.
14. Cross-reference ART-14's transports per § 6.12 to anchor message-stream processing.
15. Emit Mermaid streaming-pipeline diagrams per § 6.13 with producer/consumer/buffer/backpressure annotations.
16. Cross-check the streaming-workflow catalog against ART-09's call graph per § 6.14; unaccounted primitives are `CONTRADICTION` findings per R33.
17. Emit ART-22 per § 8 with full front-matter, per-workflow sections, composition catalog, flow-control catalog, traceability index, open questions.
18. Run the Quality Checks in § 9.
19. Emit the Completion Record per `MASTER_PROMPT.md` § 6.

---

## 6. Analysis Procedures

### 6.1 Streaming-Primitive Detection

Detect every streaming primitive by scanning imports, call patterns, and configuration. The five categories are exhaustive; each primitive is classified into exactly one.

**6.1.1 HTTP Streaming**

- **Server-Sent Events (SSE)** — `text/event-stream` content type, `EventSource` (browser API), `@sse` decorators, `sse.headers['Content-Type'] = 'text/event-stream'`, `res.write('data: ...')` with `flush()`, NestJS `@Sse()` decorator, FastAPI `StreamingResponse` with SSE media type, Go `http.Flusher`, `EventSource` (Node `eventsource` package), Java `SseEmitter`, Spring WebFlux `ServerSentEvent`.
- **Chunked Transfer Encoding** — `Transfer-Encoding: chunked`, `res.chunkedEncoding = true`, Go `http.ResponseWriter` with `Write` + `Flusher`, `TransferEncoding` field in Go's `http.Request`/`Response`, Express `res.write()` without `Content-Length`.
- **WebSockets** — `ws` (Node), `WebSocket` (browser), `@WebSocketGateway()` (NestJS), `WebSocketHandler` (Spring), `channels` (Django Channels), `gorilla/websocket` (Go), `tokio-tungstenite` (Rust), `tungstenite` (Rust), `uWebSockets.js`, `Socket.IO` (`socket.io`), `SignalR` (.NET), `ActionCable` (Rails).
- **HTTP/2 streams** — `http2.createSecureServer`, `stream.respond()`, `Http2ServerRequest`/`Http2ServerResponse`, Go `golang.org/x/net/http2`, `HTTP/2 server push`.
- **Long polling** — `req.on('close', ...)` with delayed response, polling endpoints with `Last-Event-ID` header.

**6.1.2 LLM Token Streaming**

- **OpenAI** — `stream: true` in `chat.completions.create()`, `for chunk in response:`, `await openai.chat.completions.create({stream:true})`.
- **Anthropic** — `messages.stream(...)`, `stream=True` in `messages.create()`, `for event in client.messages.stream(...)`.
- **Gemini** — `generateContentStream(...)`, `startChatStream`.
- **LangChain** — `model.stream(...)`, `model.astream(...)`, `chain.stream(...)`, `astream_log`.
- **LlamaIndex** — `query_engine.query(..., streaming=True)`, `response_gen = response.response_gen`.
- **Other** — `litellm.completion(..., stream=True)`, `vllm` `StreamingResponse`, Ollama `Generate` with `stream: true`.

Each LLM streaming primitive is cross-referenced to its `ST-XX` entry in ART-21 per § 6.11.

**6.1.3 Reactive Streams**

- **RxJS** — `Observable`, `Subject`, `BehaviorSubject`, `ReplaySubject`, `fromEvent`, `interval`, `from`, operators (`map`, `filter`, `mergeMap`, `switchMap`, `concatMap`, `exhaustMap`, `buffer`, `window`, `debounceTime`, `throttleTime`, `auditTime`, `sample`, `combineLatest`, `zip`, `forkJoin`).
- **Project Reactor** — `Flux`, `Mono`, `Flux.from(...)`, `Mono.from(...)`, operators (`map`, `filter`, `flatMap`, `concatMap`, `switchMap`, `buffer`, `window`, `debounce`, `throttleLast`, `sample`, `combineLatest`, `zip`, `merge`, `concat`).
- **Akka Streams** — `Source`, `Flow`, `Sink`, `RunnableGraph`, `Source.fromIterator`, operators (`map`, `filter`, `merge`, `concat`, `zip`, `grouped`, `sliding`, `throttle`, `backpressure`).
- **Java Flow** — `java.util.concurrent.Flow.Publisher`, `Flow.Subscriber`, `Flow.Subscription`, `SubmissionPublisher`.
- **Reactive Streams (Java)** — `org.reactivestreams.Publisher`, `Subscriber`, `Subscription`.
- **Spring WebFlux** — `@RestController` returning `Flux`/`Mono`, `WebClient` returning reactive types.
- **SmallRye Mutiny** — `Multi`, `Uni`, `Multi.createFrom()`.
- **Bacon.js** — `Bacon.fromEvent`, `Bacon.Bus`, `EventStream`, `Property`.
- **Most.js** — `most.from`, `most.stream`, `most.periodic`.
- **Callbags** — `callbag-...` libraries.
- **Reactive Extensions (.NET)** — `IObservable<T>`, `IObserver<T>`, `Subject<T>`, `System.Reactive` operators.

**6.1.4 Message-Stream Processing**

- **Apache Kafka** — `kafkajs` (Node), `confluent-kafka-python`, `kafka-python`, `aiokafka`, `org.apache.kafka.clients.consumer.KafkaConsumer`, `org.apache.kafka.clients.producer.KafkaProducer`, `@KafkaListener` (Spring), `@KafkaStreams`, `KStream`/`KTable` (Kafka Streams), `sarama` (Go), `kafka` (Rust `kafka-rust`), `rdkafka` (Rust `rust-rdkafka`).
- **AWS Kinesis** — `@aws-sdk/client-kinesis`, `aws-sdk` Kinesis, `boto3.client('kinesis')`, `KinesisClient` (Java), `KinesisProducer`, `KCL` (Kinesis Client Library).
- **Apache Pulsar** — `pulsar-client` (Node/Python), `org.apache.pulsar.client.api.Consumer`, `org.apache.pulsar.client.api.Producer`.
- **NATS / NATS JetStream** — `nats` (Node), `nats-py`, `nats.go`, `nats.java`, `io.nats.client`.
- **RabbitMQ** — `amqplib` (Node), `pika` (Python), `amqp-client` (Java), `rabbitmq` (Go `streadway/amqp`), `@RabbitListener` (Spring).
- **Redis Streams** — `XADD`, `XREAD`, `XREADGROUP`, `XCLAIM`, `XACK`, `redis` (Node) `.xadd()`, `redis-py` `.xreadgroup()`.
- **Apache Flink** — `DataStream`, `KeyedStream`, `WindowedStream`, `ProcessFunction`, `flink-streaming-java`.
- **Apache Spark Structured Streaming** — `spark.readStream`, `DataFrame.writeStream`, `StreamingQuery`.
- **AWS SQS (long polling, batched)** — `@aws-sdk/client-sqs`, `boto3.client('sqs')`, `ReceiveMessageRequest` with `WaitTimeSeconds`.
- **Azure Event Hubs** — `@azure/event-hubs`, `azure-eventhubs` (Python), `EventData`, `PartitionContext`.
- **Google Pub/Sub** — `@google-cloud/pubsub`, `google-cloud-pubsub` (Python), `pubsub_v1.subscriber`.

**6.1.5 File / I/O Streaming**

- **Node streams** — `fs.createReadStream`, `fs.createWriteStream`, `stream.Readable`, `stream.Writable`, `stream.Duplex`, `stream.Transform`, `pipeline()`, `pipe()`, `Readable.from()`.
- **Python iterators/generators** — `yield`, `async for`, `asyncio.StreamReader`, `aiofiles`, `open(...).readline()` in a loop, `io.BytesIO` chunked reads, `csv.reader` iteration.
- **Go readers/writers** — `io.Reader`, `io.Writer`, `bufio.Scanner`, `bufio.Reader`, `io.Pipe`, `io.Copy`, `io.MultiWriter`, `io.TeeReader`, `csv.Reader`.
- **Rust async I/O** — `tokio::io::AsyncReadExt`, `AsyncWriteExt`, `tokio_stream`, `futures::stream`, `tokio_util::io::ReaderStream`.
- **Java I/O streams** — `java.io.InputStream`, `OutputStream`, `BufferedReader.lines()`, `java.nio.file.Files.lines()`, `java.util.stream.Stream`, `java.util.stream.Collectors`.
- **.NET streams** — `System.IO.Stream`, `StreamReader`, `StreamWriter`, `IAsyncEnumerable<T>`, `System.IO.Pipelines`, `PipeReader`, `PipeWriter`.
- **Ruby** — `IO.pipe`, `Enumerator`, `Enumerator::Lazy`, `each_line` on file/socket.

Each primitive records: `primitive_id` `SP-XX`, `category` (http | llm | reactive | message-stream | file-io), `framework` (e.g., `rxjs`, `project-reactor`, `kafkajs`), `symbol`, `kind` (producer | consumer | operator | type), `citation` (`file:line-range, symbol`).

### 6.2 Streaming-Workflow Enumeration

Enumerate every streaming workflow `SW-XX` by clustering primitives into producer→consumer pairs (or chains). A streaming workflow is the maximal set of `SP-XX` and `FN-XX` entities that cooperate to move a stream of values from a producer to a consumer through zero or more operators, without crossing a non-streaming boundary (a function call that consumes the entire stream as a single value, e.g., `collect()` on a `Flux`, breaks the workflow).

Each workflow records: `workflow_id` `SW-XX`, `name`, `category` (http | llm | reactive | message-stream | file-io), `producer_primitive` `SP-XX`, `consumer_primitive` `SP-XX`, `operator_chain` (list of `SP-XX`), `module_id` `M-XX`, `summary` (one sentence), `citation`.

### 6.3 Producer Identification

For each workflow, identify the producer. The producer is the primitive that originates the stream — it does not consume another in-scope stream as input. Producer kinds by category:

- **HTTP streaming** — the server endpoint that opens the stream (`@Sse()`, `StreamingResponse`, `SseEmitter`, `http.Flusher`).
- **LLM streaming** — the LLM-client call with `stream: true` (cross-referenced to `LC-XX` in ART-21).
- **Reactive** — `Source.from(...)`, `Flux.from(...)`, `Observable.create(...)`, `fromEvent(...)`, `interval(...)`, `Subject` instances that emit values.
- **Message-stream** — the consumer registration (`@KafkaListener`, `consumer.subscribe(...)`, `XREADGROUP`), since the consumer side of a message stream is the producer of the in-process stream that the application processes.
- **File/I/O** — `fs.createReadStream`, `open()` followed by chunked reads, `Files.lines()`, `tokio::fs::File` with `ReaderStream`.

Each producer records: `producer_id` `SP-XX`, `kind`, `source_description` (what the stream originates from: a Kafka topic, a file, an LLM call, an HTTP request), `rate_characteristics` (push-based | pull-based | request-response | long-poll), `citation`.

### 6.4 Consumer Identification

For each workflow, identify the consumer. The consumer is the primitive that terminates the stream — it does not pass the stream onward to another in-scope primitive. Consumer kinds:

- **HTTP streaming** — the client-side `EventSource`, `WebSocket`, `fetch()` with stream body reading (`response.body.getReader()`), `XmlHttpRequest` with `onprogress`.
- **LLM streaming** — the iteration pattern (`for chunk in response:`, `await for chunk`) OR the LangChain/LlamaIndex aggregation pattern.
- **Reactive** — `.subscribe(...)`, `block()`, `blockFirst()`, `blockLast()`, `collectList()`, `collectMap()`, `toFuture()`, `await()`, `Single.subscribe()`.
- **Message-stream** — the producer registration (`producer.send(...)`, `kafkaTemplate.send(...)`, `XADD`), since the producer side of a message stream is the consumer of the in-process stream the application produces.
- **File/I/O** — `fs.createWriteStream`, `pipe()` to a write stream, `io.Copy`, `pipeTo()`.

Each consumer records: `consumer_id` `SP-XX`, `kind`, `sink_description` (where the stream terminates: a Kafka topic, a file, an HTTP response, an in-process accumulator), `terminal_behavior` (collects | forwards | side-effects-only), `citation`.

### 6.5 Buffer-Strategy Extraction

Extract each workflow's buffer strategy. The buffer strategy governs how the workflow holds in-flight values between producer and consumer. The taxonomy:

- **Unbuffered** — values flow directly from producer to consumer; no in-process queue. Detected by direct iteration (`for chunk in stream:`), `io.Copy` (Go), `pipe()` without intermediate transforms, reactive `.subscribe()` with no operator chain.
- **Bounded buffer** — a fixed-size in-process queue. Detected by `bufferSize` in Reactor `onBackpressureBuffer(n)`, `ArrayBlockingQueue` (Java), `chan` with capacity (Go), `Buffer(size)` in RxJS, `prefetch` in Kafka consumer config, `max.poll.records` (Kafka), `Channel` capacity (Kotlin coroutines).
- **Unbounded buffer** — a growable in-process queue. Detected by `onBackpressureBuffer()` without size, `LinkedBlockingQueue` (Java, unbounded), `asyncio.Queue()` without `maxsize`, `Subject` (RxJS, unbounded).
- **Windowed buffer** — values are buffered until a window condition (time or count) closes. Detected by `bufferTime(n)`, `bufferCount(n)`, `window(Time)`, `window(SIZE)`, `groupedWithin(n, ms)` (Mutiny), tumbling-window logic.
- **No buffer (lossy)** — values are dropped when the consumer is slow. Detected by `onBackpressureDrop()`, `throttle`, `sample`, `debounce`, `audit`, `Flow OverflowStrategy.DROP`.
- **Latest-only buffer** — only the most recent value is retained. Detected by `onBackpressureLatest()`, `BehaviorSubject`, `Property` (Bacon.js), `DistinctUntilChanged` combined with `switchMap`.

Each workflow records: `buffer_strategy` (one of the above), `buffer_size` (when bounded; `UNBOUNDED` when unbounded; `NA` when unbuffered), `buffer_config_citation`, `evidence_citation`.

### 6.6 Backpressure-Strategy Extraction

Extract each workflow's backpressure strategy. Backpressure is the mechanism by which a slow consumer signals a fast producer to slow down. The taxonomy:

- **Credit-based** — the consumer requests N items; the producer emits only those N. Detected by `request(n)` calls on `Subscription` (Reactive Streams), `Flow.Subscription.request(n)`, `Subscriber.request(n)`, Akka Streams backpressure (implicit, credit-based), Reactive-Streams `request()` semantics. This is the gold-standard strategy.
- **Rate-based** — the consumer signals a maximum rate. Detected by `throttle(n, period)` operators, `Throttle` (Akka), `sample`/`throttleLast` (RxJS), token-bucket implementations.
- **Window-based** — flow control by windowing. Detected by TCP-style windowing implemented in application code (rare), `Flowable.window()`, sliding-window operators used for flow control rather than semantic windowing.
- **Lossy** — the producer emits at full rate; excess values are dropped. Detected by `onBackpressureDrop()`, `OverflowStrategy.DROP`, `sample`, `throttle`.
- **Blocking** — the producer blocks when the consumer is slow. Detected by Go `chan` send blocking, `BlockingQueue.put()` (Java), `asyncio.Queue.put()` (Python), Kotlin `Channel` with `BufferOverflow.SUSPEND`.
- **None / fire-and-forget** — the producer emits at full rate; the consumer keeps up or fails. Detected by `Subject` (RxJS), `SubmissionPublisher` without buffer, naive `pipe()` without backpressure-aware operators, `EventEmitter` (Node).
- **External** — backpressure is delegated to an external system. Detected by Kafka consumer `pause()`/`resume()`, Kinesis KCL checkpoint-based flow control, RabbitMQ `basic.qos` prefetch, NATS `maxInflight`, SQS `MaxNumberOfMessages`.

Each workflow records: `backpressure_strategy` (one of the above), `backpressure_evidence_citation`, `flow_control_signals` (list of mechanisms: `request(n)`, `pause()/resume()`, `qos`, `chan cap`, `none`).

### 6.7 Error-Handling Extraction

Extract each workflow's error handling. Streaming errors include producer errors, consumer errors, network errors, deserialization errors, and timeout errors. The taxonomy:

- **On-error-stop** — the stream terminates on error. Default for most reactive streams; `Flux` errors propagate to `onError` and terminate. Detected by absence of explicit error-handling operators.
- **On-error-resume** — the stream resumes with a fallback. Detected by `onErrorResume(...)` (Reactor), `catchError(...)` (RxJS), `onErrorReturn(...)` (Reactor), `retryWhen(...)`, `orElse(...)`.
- **Retry** — the operation is retried on error. Detected by `retry(n)`, `retryWhen(...)`, `retryBackoff`, `@RetryableTopic` (Spring Kafka), `RedeliveryPolicy` (ActiveMQ).
- **Dead-letter** — failed messages are routed to a dead-letter topic/queue. Detected by `@DltHandler`, `dead-letter-queue` config, `ProducerRecord` to a DLQ topic, Kafka `errors.deadletterqueue.topic.name`, RabbitMQ `x-dead-letter-exchange`, SQS redrive policy.
- **Skip** — the failed value is skipped and the stream continues. Detected by `onErrorContinue`, `onErrorMap`, `filter` that excludes error results, `try/catch` inside a `map` that returns a sentinel.
- **Restart** — the entire stream is restarted. Detected by `restart()` (Akka Streams), `ReconnectStrategy` (HTTP clients), `retryWhen` with restart semantics.
- **Propagate-to-supervisor** — errors are propagated to a supervisor actor/process. Detected by Akka `SupervisorStrategy`, Erlang `supervisor`, Kubernetes pod-restart-on-crash.

Each workflow records: `error_handling_strategy` (one of the above), `error_handling_evidence_citation`, `dead_letter_target` (when applicable), `max_retries` (when retry), `retry_backoff` (when applicable).

### 6.8 Completion-Semantics Extraction

Extract each workflow's completion semantics. Completion is how the stream signals that no more values will arrive. The taxonomy:

- **Producer-completes** — the producer emits a completion signal (e.g., `onComplete`, `onCompleted`, `Subscriber.onComplete()`, `done` event, `Readable 'end'` event, `iterable exhausted`). The stream terminates when the producer completes.
- **Consumer-cancels** — the consumer cancels the subscription (`subscription.cancel()`, `take(n)`, `takeUntil(...)`, `takeWhile(...)`, `AbortController.abort()`, `req.on('close')`).
- **External-cancels** — an external signal cancels the stream (timeout, process exit, parent stream completion in a derived stream).
- **Never-completes** — the stream is infinite and never terminates (e.g., `interval()`, a Kafka topic with no end, `EventSource` reconnecting indefinitely). Recorded as `infinite`.
- **Error-terminates** — the stream terminates on an unrecoverable error per § 6.7.

Each workflow records: `completion_semantic` (one of the above), `completion_signal_source` (producer | consumer | external | none), `cancellation_cleanup` (what happens to resources: file close, socket close, consumer `unsubscribe()`, checkpoint write), `cancellation_evidence_citation`.

### 6.9 Stream-Composition Detection

Detect stream composition — the operators that transform, combine, or split streams. Composition operators are recorded per workflow as an ordered `operator_chain`. The taxonomy:

- **Transform** — `map`, `flatMap`, `concatMap`, `switchMap`, `exhaustMap`, `scan`, `reduce`, `collect`.
- **Filter** — `filter`, `distinct`, `distinctUntilChanged`, `skip`, `take`, `first`, `last`, `elementAt`, `ignoreElements`.
- **Combine** — `merge`, `concat`, `zip`, `combineLatest`, `forkJoin`, `withLatestFrom`, `amb`, `race`.
- **Split** — `window`, `buffer`, `groupBy`, `split`, `partition`, `flatMap` (which can split).
- **Time-based** — `debounce`, `throttle`, `sample`, `audit`, `bufferTime`, `bufferCount`, `delay`, `timeout`, `timeInterval`.
- **Error-operators** — `onErrorResume`, `onErrorReturn`, `retry`, `retryWhen`.
- **Side-effect** — `doOnNext`, `tap`, `doFinally`, `doOnSubscribe`, `doOnComplete`, `peek` (Java Streams).
- **Backpressure-operators** — `onBackpressureBuffer`, `onBackpressureDrop`, `onBackpressureLatest`.

Each operator in the chain records: `operator_id` `OP-XX`, `workflow_id` `SW-XX`, `name`, `arity` (unary | binary | n-ary), `input_streams` (list of `SP-XX`), `output_stream` `SP-XX`, `citation`.

### 6.10 Flow-Control Detection

Detect flow control — the application-level mechanisms that govern how fast the producer emits and how fast the consumer consumes. Flow control is distinct from backpressure (§ 6.6) in that flow control is the application's policy choice, while backpressure is the mechanism. The taxonomy:

- **Credit-based flow control** — the consumer explicitly requests items. Cross-referenced to `backpressure_strategy: credit-based`.
- **Rate limiting** — the producer is throttled to a maximum rate. Detected by `RateLimiter` (Resilience4j, Guava), `token-bucket` libraries, `leaky-bucket` libraries, application-level `time.sleep(1/rate)`.
- **Concurrency limiting** — the producer is limited by a concurrency cap. Detected by `Semaphore`, `Mutex`, `Promise.all` with chunks, `asyncio.Semaphore`, `Flowable.flatMap(..., maxConcurrency)`, `flatMap` with `prefetch`.
- **Batching** — values are batched upstream to amortize per-item overhead. Detected by `buffer(n)`, `window(n)`, Kafka `max.poll.records`, `batchSize`, `linger.ms`, `BATCH_SIZE`.
- **None** — no application-level flow control; relies entirely on transport-level backpressure.

Each workflow records: `flow_control` (one of the above), `flow_control_evidence_citation`, `concurrency_cap` (when applicable), `batch_size` (when applicable).

### 6.11 LLM-Streaming Cross-Reference

Cross-reference ART-21's `streaming_usage` entries. For each `ST-XX` in ART-21:

1. Locate the corresponding `SW-XX` in ART-22 (the streaming workflow whose `producer_primitive` is the LLM call).
2. If no `SW-XX` is found, create one and record the missing producer/consumer pattern as `INCOMPLETE_LLM_STREAM` with an Open Question.
3. Record the LLM streaming workflow's aggregation strategy (per ART-21's `aggregation_strategy`) into the workflow's `buffer_strategy` analysis.
4. Record the LLM streaming workflow's partial-response handling (per ART-21's `partial_response_handling`) into the workflow's `completion_semantic` analysis.

When ART-21 is `ABSENT` (PROMPT_21 skipped), the LLM-streaming analysis is skipped. Record `OQ-XX: "ART-21 ABSENT; LLM streaming workflows not analyzed"` as an Open Question. The non-LLM streaming analysis proceeds normally.

### 6.12 Message-Stream Cross-Reference

Cross-reference ART-14's transports. For each transport in ART-14:

1. If the transport is one of the message-stream frameworks in § 6.1.4 (Kafka, Kinesis, Pulsar, NATS, RabbitMQ, Redis Streams, Flink, Spark Structured Streaming, SQS, Event Hubs, Pub/Sub), enumerate the streaming workflows that use it.
2. For each consumer-group registration (e.g., Kafka `group.id`, Kinesis application name, Pub/Sub subscription), record the consumer-group identifier in the workflow's `consumer_primitive` metadata.
3. For each topic/queue/exchange, record the topic name in the producer's `source_description` or the consumer's `sink_description`.

### 6.13 Mermaid Streaming-Pipeline Diagram Emission

Emit Mermaid diagrams per `OUTPUT_RULES.md` § 7. Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

- **Per-workflow streaming-pipeline diagram** — one `flowchart LR` per workflow showing producer → operator-chain → consumer, with buffer and backpressure annotations on the edges. Nodes: `SP-XX` (primitives), `FN-XX` (functions), `OP-XX` (operators). Edges labeled with stream type and citation.
- **Backpressure-strategy matrix diagram** — a single diagram categorizing all workflows by backpressure strategy (`credit-based`, `lossy`, `blocking`, `none`) for at-a-glance comparison.
- **Master streaming-layer diagram** — a `graph LR` showing every workflow and its cross-workflow dependencies (one workflow's consumer feeding another workflow's producer). Decomposed by module when > 30 nodes.

Edge styles: solid black for in-process streams, dashed blue for HTTP streams, dashed purple for LLM streams, dashed green for message-stream transports, dashed orange for file I/O, red for backpressure lossy/drop strategies.

### 6.14 Coverage Cross-Check

Cross-check the streaming-workflow catalog against ART-09's call graph:

1. Compute `P_09` = set of `FN-XX` in ART-09 whose call patterns match § 6.1 markers.
2. Compute `P_22` = set of `FN-XX` appearing as a producer, consumer, or operator in any streaming workflow in ART-22.
3. Expected: `P_22 ⊇ P_09`. Functions in `P_09 \ P_22` are `COVERAGE_GAP` findings recorded as Open Questions.
4. Cross-check `SW-XX` consumer-side primitives against ART-14's emitters and handlers; every event `E-XX` transported via a message-stream framework MUST have its emitter and handler appear in a streaming workflow's producer/consumer pair. Mismatches are `CONTRADICTION` findings per R33.

---

## 7. Required Outputs

### ART-22 — Streaming Workflow Report

**Type:** Doc.

**Acceptance Criteria:**

- AC-22.1: The artifact file exists at `<output_root>/artifacts/phase3/ART22_<engagement_id>_streaming-workflows.md`.
- AC-22.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-22.3: The body contains: Executive Summary, Methodology, Streaming-Primitive Catalog, Workflow Catalog (per-workflow sections with pipeline diagrams), Buffer-Strategy Catalog, Backpressure-Strategy Catalog, Error-Handling Catalog, Completion-Semantics Catalog, Composition Catalog, Flow-Control Catalog, LLM-Streaming Cross-Reference, Message-Stream Cross-Reference, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-22.4: Every workflow records its producer, consumer, buffer strategy, backpressure strategy, error handling, and completion semantics.
- AC-22.5: Every Mermaid block is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-22.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-22.7: Coverage cross-check is recorded with no unresolved contradictions.
- AC-22.8: When ART-21 is `ABSENT`, an Open Question records the LLM-streaming analysis gap.

---

## 8. Output Templates

### 8.1 ART-22 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-22
artifact_type: Doc
producing_prompt: PROMPT_22
phase: 3
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
streaming_primitives:
  - id: SP-01
    category: http | llm | reactive | message-stream | file-io
    framework: <name>
    symbol: <name>
    kind: producer | consumer | operator | type
    citation: <file>:<line-range>
workflows:
  - id: SW-01
    name: <text>
    category: http | llm | reactive | message-stream | file-io
    producer_primitive: SP-XX
    consumer_primitive: SP-XX
    operator_chain: [SP-XX]
    module_id: M-XX
    summary: <sentence>
    buffer_strategy:
      kind: unbuffered | bounded | unbounded | windowed | no-buffer-lossy | latest-only
      buffer_size: <int> | UNBOUNDED | NA
      buffer_config_citation: <file>:<line-range>
      evidence_citation: <file>:<line-range>
    backpressure_strategy:
      kind: credit-based | rate-based | window-based | lossy | blocking | none-fire-and-forget | external
      evidence_citation: <file>:<line-range>
      flow_control_signals: [<text>]
    error_handling:
      strategy: on-error-stop | on-error-resume | retry | dead-letter | skip | restart | propagate-to-supervisor
      evidence_citation: <file>:<line-range>
      dead_letter_target: <text> | NA
      max_retries: <int> | NA
      retry_backoff: <text> | NA
    completion_semantic:
      kind: producer-completes | consumer-cancels | external-cancels | never-completes | error-terminates
      completion_signal_source: producer | consumer | external | none
      cancellation_cleanup: <text>
      cancellation_evidence_citation: <file>:<line-range>
    flow_control:
      kind: credit-based-flow | rate-limiting | concurrency-limiting | batching | none
      evidence_citation: <file>:<line-range>
      concurrency_cap: <int> | NA
      batch_size: <int> | NA
    citation: <file>:<line-range>
operators:
  - id: OP-01
    workflow_id: SW-XX
    name: map | filter | merge | concat | zip | combineLatest | switchMap | flatMap | window | buffer | debounce | throttle | sample | retry | onErrorResume | onBackpressureBuffer
    arity: unary | binary | n-ary
    input_streams: [SP-XX]
    output_stream: SP-XX
    citation: <file>:<line-range>
llm_streaming_cross_reference:
  - st_id: ST-XX
    workflow_id: SW-XX
    aggregation_strategy: concatenate | yield-immediately | buffer-window | none
    partial_response_handling: yield-partial | yield-only-final | reconstruct-on-done
    gap: true | false
message_stream_cross_reference:
  - transport_id: <id-from-ART-14>
    workflow_id: SW-XX
    consumer_group: <text> | NA
    topic: <text> | NA
coverage_cross_check:
  primitives_in_art09: [FN-XX]
  primitives_in_art22: [FN-XX]
  unaccounted_primitives: [FN-XX]
  event_emit_handler_mismatches: [{ event_id: E-XX, kind: <text>, detail: <text> }]
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

### 8.2 ART-22 Body Skeleton

```markdown
# ART-22: Streaming Workflow Report

## 1. Executive Summary
## 2. Methodology
## 3. Streaming-Primitive Catalog
## 4. Workflow Catalog
   ### 4.1 SW-01: <name> (category)
   **Diagram D-01: SW-01 Pipeline**
   ```mermaid
   flowchart LR
       SP01[SP-01: kafka consumer] --> OP01[OP-01: map]
       OP01 --> OP02[OP-02: filter]
       OP02 --> SP02[SP-02: kafka producer]
       %% edge: src/stream/processor.ts:42
   ```
   - Producer: <description>
   - Consumer: <description>
   - Buffer: <strategy>
   - Backpressure: <strategy>
   - Error handling: <strategy>
   - Completion: <semantic>
## 5. Buffer-Strategy Catalog
## 6. Backpressure-Strategy Catalog
## 7. Error-Handling Catalog
## 8. Completion-Semantics Catalog
## 9. Composition Catalog
## 10. Flow-Control Catalog
## 11. LLM-Streaming Cross-Reference
## 12. Message-Stream Cross-Reference
## 13. Coverage Cross-Check
## 14. Traceability Index
## 15. Open Questions
## 16. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks

- **Q1. Coverage Check** — every streaming primitive has a workflow or is recorded `UNACCOUNTED` with rationale. Threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of claims cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no workflow assertion contradicts ART-09's call graph, ART-11's data flows, or ART-14's event catalog.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` and `INCOMPLETE_LLM_STREAM` entry has a corresponding Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.2 on a 5% sample of workflows yields the same producer/consumer pair.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks

- **Q-22.A. Producer/Consumer Pairing** — every `SW-XX` records both `producer_primitive` and `consumer_primitive`. Absent either, the workflow is `INCOMPLETE` with an Open Question.
- **Q-22.B. Buffer/Backpressure Distinction** — every workflow records both `buffer_strategy` and `backpressure_strategy`. Confusing the two (e.g., recording `buffer_strategy: credit-based` when the buffer is bounded and backpressure is credit-based) is a `MAJOR` finding.
- **Q-22.C. Error-Handling Coverage** — every workflow records `error_handling.strategy`. `on-error-stop` is the default and MUST be explicit; an absent error-handling record is a `MINOR` finding.
- **Q-22.D. Completion-Semantics Coverage** — every workflow records `completion_semantic.kind`. Infinite streams (`never-completes`) MUST be explicitly marked; absence is treated as `unknown` and is a `MINOR` finding.
- **Q-22.E. Operator-Chain Ordering** — every operator in `operator_chain` has its `input_streams` referencing the prior operator's `output_stream` (or the producer for the first operator). Misordered chains are `MAJOR` findings.
- **Q-22.F. LLM-Streaming Closure** — for every `ST-XX` in ART-21 (when ART-21 is present), there is a corresponding `SW-XX` in ART-22 with category `llm`. Mismatches are `CONTRADICTION` findings per R33.
- **Q-22.G. Message-Stream Closure** — every event `E-XX` in ART-14 whose transport is a message-stream framework appears in at least one `SW-XX` with category `message-stream`. Mismatches are `CONTRADICTION` findings.
- **Q-22.H. Mermaid Edge Citation** — every edge in the Mermaid diagrams has an `edge: file:line` comment per `OUTPUT_RULES.md` § 7.5.
- **Q-22.I. Sidecar Files** — every Mermaid block has a corresponding `.mmd` file under `<output_root>/diagrams/`.

---

## 10. Common Pitfalls

- Do not confuse buffer strategy with backpressure strategy; buffer is the in-flight queue, backpressure is the consumer-to-producer flow-control signal. Conflating them is a `MAJOR` finding per Q-22.B.
- Always distinguish producer from consumer in message-stream workflows; the Kafka *consumer* registration is the *producer* of the in-process stream, and the Kafka *producer* is the *consumer* of the in-process stream. Recording them in reverse is a `MAJOR` finding.
- Do not assume `pipe()` (Node) implies backpressure; `pipe()` propagates backpressure only when both ends are `Readable`/`Writable` streams with proper `write()`/`read()` semantics. Verify per-call.
- Always check the buffer size; `onBackpressureBuffer()` without a size is `UNBOUNDED`, which is a memory-growth risk. Record `UNBOUNDED` explicitly.
- Do not classify a synchronous `for` loop over an in-memory array as streaming; arrays are not streams. Only iterators/generators/async-iterables qualify.
- Always record `never-completes` for infinite streams (e.g., `interval()`, Kafka topics); absence is treated as `unknown` per Q-22.D.
- Do not omit dead-letter targets when present; dead-letter routing is a critical operational property and its omission misleads PROMPT_24's engineering-decision analysis.
- Always cross-reference ART-21's `ST-XX` entries when ART-21 is present; omitting LLM streaming from ART-22 is a `CONTRADICTION` per Q-22.F.
- Always cross-reference ART-14's transports; every event `E-XX` on a message-stream transport MUST appear in a streaming workflow per Q-22.G.
- Do not classify `Promise.all` or `await Promise.all([...])` as streaming; concurrent promise resolution is not streaming. `IAsyncEnumerable<T>` (C#), `AsyncIterator` (Python `async for`), and `Flow<T>` (Kotlin) are streaming.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders the diagrams from the sidecar source.

---

## 11. Handoff Criteria

PROMPT_24 and PROMPT_25 consume ART-22. Handoff requires ALL of:

- HC-22.1: ART-22 status is `REVIEWED` or `DRAFT` with orchestrator waiver, OR `SKIPPED` per § 3.1 with downstream degradation declared.
- HC-22.2: Every streaming primitive appears in at least one workflow OR is recorded `UNACCOUNTED` with an Open Question.
- HC-22.3: Every workflow records producer, consumer, buffer strategy, backpressure strategy, error handling, and completion semantics.
- HC-22.4: LLM-streaming cross-reference is complete (or ART-21 is `ABSENT` with an Open Question).
- HC-22.5: Message-stream cross-reference is complete; every `E-XX` on a message-stream transport appears in a workflow.
- HC-22.6: Mermaid diagrams are emitted with `.mmd` sidecar files; diagrams > 30 nodes are decomposed.
- HC-22.7: Coverage cross-check is recorded with no unresolved contradictions.
- HC-22.8: `repository_fingerprint_recheck` matches ART-01.
- HC-22.9: No `BLOCKING` open questions remain.

---

## 12. Cross-References

- **Consumed by:** PROMPT_24 (Engineering Decisions — consumes ART-22's backpressure and error-handling strategies as engineering-decision inputs), PROMPT_25 (Diagram Generation — re-renders the Mermaid sources at higher visual fidelity), PROMPT_26 (Rebuild Guide — consumes ART-22's streaming-layer reconstruction), PROMPT_28 (Cross-Reference Checklists — verifies that every `SW-XX` referenced by ART-25 resolves to an entry in ART-22).
- **Depends on:** ART-01 (PROMPT_01), ART-09 (PROMPT_09), ART-11 (PROMPT_11), ART-14 (PROMPT_14), ART-16 (PROMPT_16), ART-21 (PROMPT_21).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33 (contradiction escalation between ART-22's streaming primitives and ART-09/ART-14/ART-21).
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4 (sidecar files), § 3.1, § 4, § 6, § 7 (Mermaid conventions, edge citations, ≤ 30 nodes, decomposition).
- **Forward reference:** PROMPT_30 (Self-Review & QA) verifies that every `SW-XX` referenced by ART-26 resolves to an entry in ART-22, and that every `OP-XX` referenced by ART-25 resolves to an entry in ART-22.

*End of PROMPT_22. Orchestrator may dispatch PROMPT_23 upon satisfaction of § 11.*
