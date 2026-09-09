# Phase 15 — Real-Time & Streaming Financial AI

> **Stage V — Advanced Financial AI** · **Duration: 3-4 weeks** · **Mastery target: Production**
> **Position in path:** `14-agentic-fintech` ← **this phase** → `16-security-compliance-responsible-ai`

## 1. Objective

Fraud scoring at authorization time, velocity alerts, and instant-payment monitoring are streaming problems: the model must decide inside a payment's latency budget, from features that must be computed while the event is in flight, on infrastructure that degrades gracefully instead of going down. In this phase you will master Kafka and stream-processing fundamentals, online feature computation, low-latency serving, and exactly-once decisioning — and you will measure p99 under load until you hit it. You will finish able to design a sub-100ms decisioning service end to end, and to explain precisely what "exactly-once" does and does not buy you.

## 2. Why It Matters in Finance

Payments have become real-time by regulation and by market: instant-payment rails (FedNow and RTP in the US, SEPA Instant in the EU) settle in seconds, and the UK's APP-fraud reimbursement rules (effective October 2024) push fraud controls before funds leave. A fraud model that answers in 800ms is a declined card at the till. Streaming AI is where ML engineering meets the discipline of distributed systems — and where silent failure costs money per second.

- Card authorization commonly targets app-level p99 well under a few hundred milliseconds; the decisioning service typically gets a slice of that (order-of-magnitude budget: <100-200ms — verify against your rail and issuer contract). HFT microseconds are a different sport and out of scope here.
- Instant payments are irrevocable: there is no batch window to catch what you missed, so velocity and device intelligence must be computed in-stream.
- Exactly-once matters because a payment decision is a side effect — duplicate processing means double decisions, double alerts, and broken audit trails.
- Features that are stale by 30 seconds (last-transaction-count, device-seen-before) are how sophisticated fraud passes; freshness is an SLA, not a preference.
- Load sheds and rules-only fallbacks are how you survive an ML-service outage at 3 a.m. without declining every customer.

## 3. Prerequisites

- [ ] Phase 07 — fraud detection models, imbalanced data, cost-sensitive thresholds
- [ ] Phase 03 — data engineering: pipelines, schemas, orchestration
- [ ] Phase 17 awareness — you will build production-shaped services here and formalize them there
- [ ] Python services: FastAPI, async I/O, Docker (assumed known)
- [ ] Basic distributed-systems vocabulary: partitions, replication, retries

## 4. Learning Outcomes

- I can state latency budgets by use case (authorization, risk scoring, market risk, batch) and decompose a p99 budget across network, features, and model.
- I can design Kafka topics with correct partition keying so per-account ordering holds, and size consumer groups against throughput.
- I can explain exactly-once semantics — idempotent producers, transactions, read_committed — and what EOS does not cover (external side effects).
- I can build stateful Flink (or Kafka Streams) pipelines with event time, watermarks, windows, and checkpointing.
- I can compute streaming features into a Redis-backed online store and guarantee training/serving parity.
- I can serve a model sub-100ms at p99 with ONNX Runtime or native inference, warm pools, and horizontal scaling.
- I can make decisioning idempotent (event-ID dedupe, transactional outbox) and idempotent under retries.
- I can implement backpressure and load shedding that degrades to rules-only mode within SLA.
- I can monitor a streaming ML system: consumer lag, feature freshness SLA, drift on streams — and run a failover drill on it.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 15.1 | Latency budgets by use case | auth-time vs risk vs batch; HFT out of scope | budget decomposition worksheet |
| 15.2 | Kafka deep essentials | partitions, keying, consumer groups | topic design + throughput test |
| 15.3 | Exactly-once semantics | idempotent producer, transactions, read_committed | EOS demo + "what's not covered" memo |
| 15.4 | Flink fundamentals | event time, watermarks, windows, state, checkpoints | stateful job with checkpointing |
| 15.5 | Complex event processing | velocity patterns, sessionization | velocity-feature job |
| 15.6 | Online feature computation | streaming aggregates → Redis; Feast streaming | online store + freshness monitor |
| 15.7 | Architecture: feature-stream + scoring API vs model-in-stream | the core decision | ADR with tradeoffs |
| 15.8 | Low-latency serving | ONNX Runtime, Triton, native XGBoost, warm pools | p99-benchmarked scorer |
| 15.9 | Idempotent decisioning | event-ID dedupe, transactional outbox | outbox-pattern processor |
| 15.10 | Backpressure & load shedding | degrade to rules-only as first-class design | failover harness |
| 15.11 | Late events | watermarks vs wall clock for fraud signals | late-event experiment suite |
| 15.12 | Streaming joins & hot keys | tx × device × account history; whale skew | join topology + skew mitigation |
| 15.13 | Monitoring streaming ML | freshness SLA, consumer lag, stream drift | streaming dashboard |
| 15.14 | Patterns roundup: CQRS, reconciliation, vector search, GPU, cost | production pattern catalog | pattern notes with p99 numbers |

**15.1 Latency budgets by use case.** Authorization-time fraud scoring gets a slice of a budget that commonly sits at app-level p99 <100-200ms end to end (verify your specific rail); market-risk computations tolerate minutes; batch credit decisioning runs overnight; HFT is microseconds and a different industry. The engineering skill is decomposing the budget — network hop, feature fetch, model, logging — and measuring each at p99, because the tail, not the mean, declines cards.

**15.2 Kafka deep essentials.** Partitions are the unit of parallelism and ordering; key by account (or card) ID so per-entity ordering survives scaling, and accept that global ordering is gone. Consumer groups scale reads; replication factors and min.insync.replicas are your durability dials. Run a real topic through a load test and watch partition skew before it becomes a production incident.

**15.3 Exactly-once semantics.** Kafka EOS = idempotent producer (retries without duplicates) + transactions + consumers reading read_committed. It covers Kafka-to-Kafka. It does NOT cover your side effects — a fraud decision written to Postgres, a webhook fired, a case opened. Those need your own idempotency (event-ID dedupe, outbox). Interviewers probe exactly this boundary.

**15.4 Flink fundamentals.** Event time is when the payment happened; processing time is when your cluster saw it; watermarks bound how long you wait for stragglers. Windows and keyed state give you the aggregates fraud models eat; checkpoints (aligned or unaligned) give you exactly-once state recovery. Learn what a checkpoint restore actually restores before you trust it.

**15.5 Complex event processing.** Velocity patterns — N transactions in M minutes, first-seen device plus high value, rapid country hop — are CEP over keyed streams. Implement them as Flink/Streams stateful functions or as features feeding a model, and know the difference: rules explain themselves, features score ensembles, and you will ship both.

**15.6 Online feature computation.** Streaming aggregations (counts, sums, EWMA rates per account/device/merchant) land in Redis or another online store; Feast's streaming story shows the pattern. The invariant that matters: training features and serving features must be computed by the same logic — parity bugs are the silent killers (Phase 07's lesson, enforced in-stream here).

**15.7 Architecture: feature-stream + scoring API vs model-in-stream.** Option A: streams build features; a synchronous scoring API reads them and answers (low latency, model independent of pipeline). Option B: model runs inside the stream (lower latency, harder deploys and A/B). Most fraud teams pick A for authorization-time decisions; write the ADR yourself with the tradeoffs and a rollback story.

**15.8 Low-latency serving.** ONNX Runtime or native XGBoost/LightGBM inference beats general-purpose frameworks for small trees; Triton adds batching and model versioning when you outgrow a hand-rolled service. Warm pools (pre-forked workers, preloaded models, pre-opened connections) kill cold-start tails. Benchmark with real payloads and record p50/p95/p99, not averages.

**15.9 Idempotent decisioning.** Dedupe by event ID at the scoring boundary so producer retries never double-decide; use the transactional outbox pattern to write the decision and the "decision made" event atomically. Prove it under a retry storm — the pattern is only real if it survives chaos testing.

**15.10 Backpressure & load shedding.** When overload arrives, shed by tier: drop sampling of low-risk analytics, keep every authorization decision, and degrade to rules-only mode with the ML path bypassed. Graceful degradation is a first-class design artifact with its own SLA ("rules fallback engages within X seconds of ML failure"), tested by killing the ML service on purpose.

**15.11 Late events.** Offline transactions post late; devices report late; partitions backlog. Watermarks with allowed lateness bound how long velocity windows stay open; wall-clock cutoffs decide when a late event goes to reprocessing instead of live scoring. For fraud signals, being wrong about lateness means being blind to exactly the bursty attacks that arrive with jitter.

**15.12 Streaming joins & hot keys.** Enriching transactions needs stream-stream and stream-table joins (tx × device × account history). Whale accounts — marketplaces, payroll, exchanges — skew partitions and hot-state one node; mitigate with key salting, two-stage aggregation, or isolating whales onto dedicated topics. Know the skew before the OOM does.

**15.13 Monitoring streaming ML.** Consumer lag is your smoke detector; feature freshness SLA ("99% of scores used features younger than 10s") is your ML-specific one; drift metrics computed in-stream catch population shifts without labels. Alert on the pipeline, not just the model — a silently stale feature store outperforms in dashboards and fails in production.

**15.14 Patterns roundup.** CQRS separates the scoring read path from decision persistence; near-real-time reconciliation (matching streams instead of nightly files) closes the loop; vector search for real-time retrieval must be p99-budged like any other call; GPU serving pays only above certain throughput; and cost engineering (right-sizing, tiered storage, spot consumers) decides whether the platform survives its own success.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Percentiles & tail latency | Order statistics on latency distributions | Authorization fails at the tail; p99 is the contract | You optimize the mean while customers get declined |
| Little's law | L = λ × W | Sizing consumer parallelism and review queues | You scale by guesswork and melt at peaks |
| EWMA / decayed counts | Time-weighted rates with half-lives | Velocity features that forget gracefully | Your features are rectangular windows fraud exploits |
| Watermark math | Allowed-lateness vs completeness tradeoff | Bounding when windows close | Windows close too early (blind) or too late (blown SLA) |
| PSI/KS on streams | Distribution-shift statistics computed incrementally | Catching drift before labels exist | Silent population change, no alert until losses |
| Queueing loss probability | Erlang-style blocking/load-shed math | Deciding what to shed first under overload | You shed the revenue path and keep the telemetry |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Partition keying & ordering | Per-account ordering underlies velocity logic; wrong keys = corrupted features |
| Schema registry & contracts | Payment event schemas evolve; incompatible changes break consumers silently |
| Idempotency & outbox | Decisions are side effects; exactly-once effects need application patterns, not just Kafka EOS |
| Warm pools & connection reuse | Cold starts are the p99 killer in scoring services |
| Chaos/failover drills | Kill the ML service on schedule; the rules-only fallback must engage within SLA, every time |
| Backpressure configuration | Bounded queues + explicit shed tiers beat unbounded retries every time |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| Apache Kafka | Event backbone: topics, keying, consumer groups, transactions |
| Apache Flink (or Kafka Streams) | Stateful stream processing: event time, watermarks, windows, checkpoints |
| Redis | Online feature store and dedupe/cache layer at microsecond reads |
| Feast | Feature-store abstractions bridging offline training and online serving |
| FastAPI | The synchronous scoring API in the reference architecture |
| ONNX Runtime | Sub-millisecond tree/nn inference without framework overhead |
| NVIDIA Triton | Model serving with batching and versioning at higher throughput |
| Locust / k6 | Load generation to find p99 and the actual bottleneck |
| Prometheus + Grafana | Lag, freshness, latency, and fallback-state dashboards |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Apache Kafka documentation (kafka.apache.org) | Docs | Intermediate | Backbone | Partitions, EOS, consumer semantics from the source | Essential |
| Apache Flink documentation (flink.apache.org) | Docs | Intermediate | Stream processing | Event time, watermarks, checkpointing mechanics | Essential |
| Feast documentation (docs.feast.dev) | Docs | Intermediate | Feature store | Online/offline parity and streaming ingestion patterns | Recommended |
| ONNX Runtime documentation (onnxruntime.ai) | Docs | Intermediate | Serving | Low-latency inference options and tuning | Recommended |
| NVIDIA Triton documentation | Docs | Advanced | Serving | Batching, model versions, ensemble serving | Optional |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Akidau, Chernyak & Lax, *Streaming Systems* (O'Reilly 2018) | Book | Advanced | Stream semantics | The rigorous treatment of event time and watermarks | Essential |
| Kleppmann, *Designing Data-Intensive Applications* (2017), ch. 11 | Book | Intermediate | Streams | Stream processing concepts in context; the boundary chapter | Essential |
| Kafka: The Definitive Guide, 2nd ed. (O'Reilly; Shapira et al.) | Book | Intermediate | Kafka | Practitioner depth beyond the docs | Recommended |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Confluent engineering blog (confluent.io/blog) | Blog | Intermediate | Patterns | Production patterns: outbox, ktables, schema evolution | Recommended |
| Adyen engineering blog (adyen.com/engineering) | Blog | Intermediate | Payments scale | Public payments-infra engineering from a real processor | Recommended |
| Revolut / Nubank public engineering posts | Blog | Intermediate | Fintech scale | How fintechs publish (selectively) about scale and incidents | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| DataTalksClub free courses (Data Engineering Zoomcamp; verify current syllabus) | Course | Beginner-Intermediate | Pipelines | Guided reps if the book-first approach stalls | Optional |
| Kafka Summit / Current conference talks (public recordings) | Talks | Intermediate | War stories | Partition skew, EOS, and outage retrospectives from operators | Reference |

## 10. Practical Exercises

1. - [ ] Write the latency-budget decomposition for auth-time fraud scoring: budget 150ms p99 app-level; allocate and justify per hop; then measure your prototype and reconcile the difference.
2. - [ ] Create a Kafka topic keyed by account; run 10k synthetic PaySim transactions through 3 partitions; demonstrate per-key ordering and measure per-partition skew.
3. - [ ] Build a Flink (or Kafka Streams) job computing EWMA velocity per card (5-min, 1-h half-lives) with event time; verify identical results against an offline pandas recompute.
4. - [ ] Run the EOS demo: producer with idempotence + transactions, consumer with read_committed; then fire 1,000 retries and count duplicates — in Kafka and, separately, into Postgres (document what EOS did not protect).
5. - [ ] Load-test the scoring service with Locust/k6; plot p50/p95/p99; find the bottleneck (connection pool, model, feature fetch) and fix it; re-measure and record the delta.
6. - [ ] Inject late events (5-30 min) into your watermark-based job; sweep allowed-lateness and plot completeness vs window-close delay; write the fraud-specific recommendation.
7. - [ ] Simulate a whale account generating 40% of events; show partition skew, then mitigate (salting or dedicated topic) and quantify improvement.
8. - [ ] Implement the transactional outbox for decisions; run a retry storm and prove exactly-once effects at the Postgres sink.
9. - [ ] Kill the ML service mid-load; measure time-to-rules-fallback and decision-completeness during the gap; automate the drill into CI.
10. - [ ] Stand up the streaming monitor: consumer lag, feature-freshness SLA, and in-stream PSI; trigger each alert deliberately and screenshot the firing dashboards.

## 11. Mini Projects

**M1 — End-to-end fraud stream.** Kafka → Flink features → FastAPI scoring (ONNX), IEEE-CIS or ULB transactions replayed as a stream; measure p99 under load and iterate until the app-level decision path is <100ms on your hardware. Deliverable: repo + latency report. Difficulty: ★★★★☆.

**M2 — Watermark/late-event experiment suite.** Parameterized suite over allowed lateness, watermark intervals, and burst patterns; charts of completeness vs latency. Deliverable: experiment notebook + policy memo. Difficulty: ★★★☆☆.

**M3 — Idempotent payment event processor.** Outbox pattern + event-ID dedupe; chaos-tested under retries and restarts, with exactly-once effects proven at the sink. Deliverable: processor + chaos test log. Difficulty: ★★★☆☆.

**M4 — Failover demo.** Full topology with ML service killed on schedule; rules-only fallback engages within a stated SLA; dashboards capture the transition. Deliverable: demo script + recorded run. Difficulty: ★★★☆☆.

## 12. Major Project Hook

This phase powers the production architecture of **Flagship Project 1 — Real-Time Fraud Detection Platform** (`/projects/flagship/`; see `/projects/flagship/README.md`): the streaming spine, online features, and sub-100ms decisioning service it specifies are built here and hardened in Phase 17.

## 13. Case Studies & Industry Examples

- **Robinhood, March 2020**: publicly reported multi-day outages during market extremes — a reminder that "real-time" systems are judged on their worst day, not their median one (see `/case-studies/README.md`).
- **Knight Capital, 2012**: deployment discipline as a streaming-adjacent lesson — an automation flaw plus a dead flag produced publicly reported losses in under an hour; it recurs because it keeps being relevant.
- **Instant payments & APP fraud (UK, from Oct 2024)**: reimbursement rules shifted fraud controls to pre-settlement, real-time scoring — regulation as a forcing function for streaming ML.
- **Confluent / Adyen public engineering posts**: partition strategy, exactly-once pipelines, and load-shedding stories told by operators; read for the gap between architecture diagrams and 3 a.m. reality.

## 14. Interview Questions

**Design auth-time fraud scoring under 100ms.** Decompose the budget: gateway → scoring API → feature fetch (Redis, sub-ms) → model (ONNX, low ms) → decision log (async). Key stream features by account/card for ordering, warm pools for cold starts, and an explicitly bounded timeout that falls back to rules on breach. Measure p99 at every hop; the mean is decoration.

**What does exactly-once actually mean in Kafka?** Idempotent producers prevent duplicates from retries; transactions make consume-transform-produce atomic; read_committed consumers see only committed data. It covers Kafka-to-Kafka — your Postgres writes, webhooks, and case creation are your problem, solved with dedupe and the outbox pattern.

**How do you handle late events in fraud features?** Event time with watermarks and bounded allowed-lateness for velocity windows; late-beyond-bound events go to reprocessing that amends aggregates and re-flags if needed. Choose bounds from fraud economics: how much completeness you must trade for window-close latency.

**What breaks with hot keys?** Whale accounts skew partitions: one consumer overheats, lag climbs, ordering guarantees concentrate on a bottleneck. Mitigate with key salting plus two-stage aggregation, or split whales onto dedicated topics — and monitor per-partition lag, not just totals.

**Design failover for a decisioning service.** Health-checked ML path with a bounded timeout; on failure, load shed analytics first, then degrade to rules-only scoring with explicit customer-visible behavior; state stays in the stream so recovery replays what was missed. The design is only real after you kill the service in a drill.

**Why key by account ID and not globally?** Kafka orders only within a partition; per-entity ordering is what velocity logic needs, and partitioning by entity scales horizontally. Global ordering would serialize everything through one partition — a throughput non-starter.

**Online/offline feature parity — how do you guarantee it?** One transformation codebase compiled to both batch and streaming, tested against golden fixtures; monitor freshness and distribution parity in production. Parity bugs are the most common silent fraud-model killer.

**Model-in-stream or side scoring API — how do you choose?** Side API for authorization-time decisions: independent deploys, A/B and rollback, simple scaling; model-in-stream only when the extra hop genuinely breaks the budget. Write the ADR with the rollback story either way.

**How do you monitor a streaming ML system without labels?** Pipeline health (lag, freshness SLA), input drift (in-stream PSI), output stability (score distributions, decision rates), and proxies against delayed labels when they arrive. Alerts on the pipeline fire hours before any performance metric could.

**What is the transactional outbox?** Write the business record and the outbound event in one local transaction; a relay publishes the event from the outbox. It makes "decide" and "announce the decision" atomic without distributed transactions.

## 15. Assessment — Can You Pass the Bar?

- [ ] Present a latency-budget decomposition for auth-time scoring and defend each allocation with measurements.
- [ ] Demonstrate a stateful streaming job whose features match an offline recompute exactly.
- [ ] Prove exactly-once effects at the sink under a retry storm, and explain Kafka EOS's boundary.
- [ ] Show p99 before/after a bottleneck fix, with the profiler evidence.
- [ ] Run the failover drill: ML killed, rules-only fallback within SLA, dashboards recorded.
- [ ] Explain watermarks and allowed lateness to a risk officer using a fraud-attack example.
- [ ] Write the ADR for feature-stream+API vs model-in-stream, with rollback plans.

## 16. Mastery Checkpoint

You may proceed to Phase 16 when:

1. Repo evidence exists for: Kafka/Flink topology, online features with parity tests, sub-100ms p99 benchmark, outbox processor, and the failover harness with recorded drills.
2. Your late-event experiment suite produces a written policy recommendation tied to fraud economics.
3. You can whiteboard the full reference architecture from memory and answer "what happens when X dies?" for every box.

Evidence: repo links + latency report + recorded failover demo. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Averaging latencies and calling the system fast; the p99 tail is where authorizations die.
- Keying by event ID instead of account ID — partition-level parallelism destroys per-entity ordering.
- Trusting Kafka EOS to cover the Postgres write; side effects need outbox/dedupe patterns.
- Windowed features computed differently in training and streaming (parity bugs) — silent, and discovered only in production losses.
- No load-shed design: under overload, unbounded queues turn a slowdown into a total outage.
- Ignoring whale skew until one marketplace account OOMs a consumer on payday.
- Testing failover only in diagrams; the drill that kills the ML service is the only one that counts.

## 18. Where This Goes Next

Phase 16 wraps the streaming stack in its regulatory armor: model risk management, EU AI Act duties for high-risk systems like creditworthiness assessment, DORA operational-resilience expectations (in force since January 2025), and the security engineering that keeps decisioning services from becoming attack surfaces. The systems you just made fast must now be made defensible.
