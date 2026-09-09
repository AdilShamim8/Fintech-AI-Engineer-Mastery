# Phase 03 — Financial Data Engineering

> **Stage II — Data & Quant Core** · **Duration: 3-4 weeks** · **Mastery target: Working fluency → Application**
> **Position in path:** `02-banking-payments-lending` ← **this phase** → `04-financial-mathematics`

## 1. Objective

Financial ML fails on data, not models. This phase makes you dangerous at the data layer: modeling ledgers and event logs honestly, keeping history straight with slowly changing dimensions and bitemporality, handling market data and corporate actions without leaking the future, enforcing quality with expectation suites, and — the defining financial-ML data skill — building point-in-time-correct features so training data reflects only what was knowable at decision time. You will build batch and CDC pipelines, a small lakehouse, and a feature store, all inside fintech-lab with production-grade discipline.

## 2. Why It Matters in Finance

Banks and funds employ more data engineers than modelers for a reason: the marginal return on a better join is larger than the marginal return on a fancier model, and regulators audit the join. BCBS 239 made risk-data aggregation, accuracy, and lineage a supervisory expectation, so in finance the data platform is a compliance surface, not a utility.

- Point-in-time correctness separates Kaggle-grade from production-grade financial ML; leaky features inflate backtests and lose real money — inflated public-leaderboard scores from leakage are a documented pattern in credit competitions.
- Corporate actions and survivorship bias quietly corrupt market-data research; raw versus adjusted prices is a multi-million-dollar trap that free data sources handle inconsistently.
- Training/serving feature parity is an organizational data problem; feature stores exist because ad-hoc joins drift between notebook and service.
- Append-only, auditable event data is simultaneously an engineering choice and a regulatory expectation (audit trails, reconstruction, reproducibility).
- Entity resolution underpins AML and credit: merging the wrong customers is as dangerous as missing the right ones.

## 3. Prerequisites

- [ ] Phases 00-02 — fintech-lab with CI, ledger simulator, payment lifecycle state machine
- [ ] SQL competence: joins, window functions, constraints
- [ ] pandas or polars basics; Docker from Phase 00
- [ ] Local Postgres and Kafka runnable via Docker Compose (no cloud spend required)

## 4. Learning Outcomes

- I can design a double-entry ledger schema with append-only postings, audit columns, and enforced invariants.
- I can implement SCD2 and bitemporal tables and know when each is required.
- I can build an OHLCV market-data lake with deliberate corporate-action handling and explain the leakage trap.
- I can author Great Expectations suites that act as enforceable financial data contracts in CI.
- I can build a dbt/Airflow batch pipeline with tests, docs, and lineage.
- I can run a CDC pipeline (Postgres → Kafka → DuckDB) with idempotent sinks and prove replay safety.
- I can build a point-in-time-correct feature pipeline on LendingClub and quantify the leakage I avoided.
- I can solve entity resolution on messy customer data with rapidfuzz/splink and evaluate precision/recall honestly.
- I can tokenize/pseudonymize PII while preserving analytical utility, and outline what BCBS 239 expects of risk data.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 03.1 | Financial data species | Transactional, market, reference, regulatory, alternative | data-species catalog |
| 03.2 | Ledger data models | Double-entry as schema, append-only postings, invariants | ledger DDL + tests |
| 03.3 | Immutable event logs | Append-only design, outbox, replayability | event-log design note |
| 03.4 | SCD & bitemporality | Type 2, valid vs system time | bitemporal customer table |
| 03.5 | Market data fundamentals | Ticks, OHLCV bars, order books, session traps | partitioned Parquet lake |
| 03.6 | Corporate actions & adjustments | Splits, dividends, adjustment conventions, leakage | adjustment playbook |
| 03.7 | Survivorship & selection bias | Point-in-time universes, delisting effects | bias demonstration notebook |
| 03.8 | Data quality & expectations | Contracts, Great Expectations in CI | expectation suite + planted-failure tests |
| 03.9 | Batch pipelines | Airflow/Prefect orchestration, dbt models | dbt project with docs |
| 03.10 | Streaming & CDC | Kafka semantics, Debezium, idempotent sinks | CDC pipeline (Postgres→Kafka→DuckDB) |
| 03.11 | Lakehouse & columnar formats | Parquet layout, Delta/Iceberg, DuckDB/Polars | layout benchmark + ADR |
| 03.12 | Feature stores & point-in-time correctness | Feast, as-of joins, training/serving parity | PIT feature pipeline + leakage memo |
| 03.13 | Entity resolution | Blocking, fuzzy scoring, probabilistic linkage | ER pipeline + eval report |
| 03.14 | PII tokenization & BCBS 239 | Pseudonymization, lineage, risk-data governance | tokenization util + lineage doc |

**03.1 Financial data species.** Transactional data (payments, postings), market data (ticks, bars, books), reference data (instruments, counterparties, calendars), regulatory data (filings, reports), and alternative data (card panels, web signals) differ in volume, freshness, quality, and failure modes — and pipelines that ignore the difference produce confident nonsense. Build a catalog table for the species you will use through Stage III; every design starts by naming its species.

**03.2 Ledger data models.** Model double-entry as an append-only postings table — account, signed amount, currency, transaction links, timestamps — with balances derived or carefully materialized, and never UPDATE money rows. Add the audit columns (who, what, when, source) now rather than when the auditor asks. Port your Phase 01 ledger simulator onto this schema with invariant tests; the schema becomes Phase 06's ground truth for label construction.

**03.3 Immutable event logs & append-only design.** Payments, orders, and account changes are facts that happened, so model them as append-only events with an outbox pattern for propagation, deriving current state by replay or projection. Replayability is what makes backfills, debugging, and audits tractable — and what regulators mean when they ask to reconstruct a decision. Write the design note contrasting CRUD tables with event logs and when each is acceptable.

**03.4 Slowly changing dimensions & bitemporality.** SCD2 keeps version history of entities (addresses, risk ratings, segment assignments); bitemporality keeps two timelines — valid time (when true in reality) and system time (when the database knew). "What did we know at decision time?" is both a modeling requirement and a regulatory question. Implement both on a customer table and query them as-of two different dates on each axis.

**03.5 Market data fundamentals.** Ticks, OHLCV bars, and order-book levels differ by orders of magnitude in volume; bars require session- and timezone-aware aggregation, and even daily bars hide venue traps (half-days, auctions, gaps). Build partitioned Parquet storage over Kraken/Binance OHLCV from the canonical dataset list and benchmark DuckDB versus pandas on a one-year query — the layout decisions you make here decide every later query cost.

**03.6 Corporate actions & adjustments.** Splits and dividends make raw price series incomparable across time; vendors apply adjustment conventions that differ (price vs total return, adjustment dates) and applying today's adjustment factors to historical decisions leaks future knowledge into backtests. Synthesize a 2-for-1 split plus a dividend on a synthetic ticker, produce raw and adjusted series, and write the playbook future-you follows — this trap recurs in Phase 09 and Phase 10.

**03.7 Survivorship & selection bias.** Datasets containing only survivors — live tickers, approved loans, retained accounts — systematically flatter research results. Understand point-in-time universes and cohort construction; the approved-loans variant of this bias becomes reject inference in Phase 06. Demonstrate it: drop delisted instruments from a synthetic universe and watch the average return inflate.

**03.8 Data quality & expectations testing.** Financial pipelines need contracts: schema, nullability, ranges (amounts ≥ 0, whitelisted currencies), referential integrity, freshness, and reconciliation totals. Encode them as Great Expectations suites that fail CI rather than dashboards nobody watches, and prove the suite fires by planting bad data. A failing expectation should block the pipeline, not decorate a report.

**03.9 Batch pipelines (Airflow/dbt).** Orchestration (Airflow or Prefect) manages dependencies, schedules, and backfills; dbt turns SQL transformations into tested, documented, lineage-tracked models from staging to marts. Incremental models and late-arriving data are the finance-normal case, not exceptions. Build a small dbt project over your market lake with `dbt docs generate` as the lineage artifact BCBS-239-style reviewers ask for.

**03.10 Streaming & CDC.** Kafka provides durable, ordered, partitioned logs with consumer groups; Debezium streams Postgres changes log-based into topics with schema topics included. Delivery is at-least-once, so sinks must be idempotent — prove it by killing your consumer mid-stream and verifying zero loss and zero duplicates on restart. This pipeline is the ancestor of Phase 15's real-time financial systems.

**03.11 Lakehouse & columnar formats.** Parquet is the substrate (columnar, compressed, partitioned); Delta and Iceberg add ACID transactions, time travel, and schema evolution over object storage; DuckDB and Polars give instant local compute. Partition-pruning arithmetic — file counts, row-group sizes, partition cardinality — moves query cost by 100x. Write the ADR justifying lakehouse over warehouse for this stage's workloads.

**03.12 Feature stores & point-in-time correctness.** The defining skill: features must be computed from data as it existed at each decision timestamp, which means as-of joins against event history, not snapshot joins against current state. Feast formalizes feature views, entities, and offline/online retrieval to guarantee training/serving parity. Build the PIT-correct LendingClub pipeline (canonical dataset), then build the leaky version too and quantify the AUC gap — the memo you write about that gap is your signature interview story.

**03.13 Entity resolution / record linkage.** Customers appear many times — typos, name variants, shared addresses, transpositions. Blocking narrows candidate pairs; fuzzy scoring (rapidfuzz) or probabilistic linkage (splink's Fellegi-Sunter model) scores them; clustering assigns identities. In AML, false merges are as dangerous as misses, so evaluate precision and recall against planted ground truth and justify thresholds explicitly.

**03.14 PII tokenization & BCBS 239.** Tokenize or pseudonymize PII — deterministic keyed hashing preserves joinability, format-preserving tokens preserve usability — and manage keys as crown jewels. BCBS 239 (BIS, 2013) expects banks to aggregate risk data with accuracy, completeness, timeliness, and clear lineage; read the principles once and map each to a concrete artifact you already own (quality suites, snapshot discipline, dbt docs). Governance is an engineering surface, and you just built most of it.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| As-of / interval join semantics | Matching events to states valid at a timestamp | The formal core of point-in-time correctness | Silent future leakage in every feature table |
| Similarity metrics | Edit distance, Jaro-Winkler | Record linkage over names and merchants | ER becomes guesswork |
| Fellegi-Sunter intuition | Probabilistic match scoring from agreement patterns | The principled core of splink-style linkage | Thresholds nobody can defend |
| Return aggregation & adjustment arithmetic | Compounding splits/dividends into comparable series | Correct backtests and signal research | Multi-million-dollar backtest illusions |
| Dedup & transitive closure | Clustering matched pairs into entities | Identity resolution for AML/credit | Duplicate and split customers everywhere |
| Partition/layout arithmetic | Cardinality, pruning, file sizing | Query cost engineering on the lake | 100x compute waste and abandoned pipelines |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Kafka (topics, keys, consumer groups) | The transport for payment and account events; ordering by key preserves per-account stories |
| dbt (models, tests, docs) | Governed SQL transformations with lineage — the reviewer-facing artifact |
| Airflow / Prefect | Orchestration, backfills, and retries for batch financial data |
| Debezium | Log-based CDC without application changes; captures what really changed |
| DuckDB / Polars | Local columnar compute for prototyping and benchmarks |
| Feast | Feature definitions shared between training and serving |
| Great Expectations | Data contracts enforced in CI, not reported in dashboards |
| Parquet partitioning & file sizing | The difference between a 2-second and a 2-hour market-data query |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| Postgres | The transactional source system for CDC and ledger exercises |
| Kafka (or Redpanda) | Event streaming backbone for CDC and payment events |
| Debezium | Log-based change data capture from Postgres to Kafka |
| dbt | SQL transformation models with tests and docs |
| Airflow or Prefect | Pipeline orchestration and backfill management |
| DuckDB | Local analytics over Parquet; CDC sink for exercises |
| polars | Fast dataframe transforms as pandas' sharper successor |
| Feast | Feature store: definitions, offline/online retrieval, PIT joins |
| Great Expectations | Expectation suites as enforceable data contracts |
| rapidfuzz / splink | Fuzzy matching and probabilistic record linkage |
| docker compose | The entire stack runs locally, zero cloud spend |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Feast documentation (docs.feast.dev) | Docs | Intermediate | Feature stores | The open standard for feature definitions and point-in-time retrieval | Essential |
| dbt documentation (docs.getdbt.com) | Docs | Intermediate | Transformations | SQL models, tests, docs — the batch backbone with lineage | Essential |
| Apache Kafka documentation (kafka.apache.org) | Docs | Intermediate | Streaming | Topics, keys, partitions, consumer semantics | Essential |
| Debezium documentation (debezium.io) | Docs | Intermediate | CDC | Log-based change capture patterns and gotchas | Essential |
| Great Expectations documentation (greatexpectations.io) | Docs | Intermediate | Data quality | Expectation suites as enforceable data contracts | Essential |
| BCBS 239 — Principles for effective risk data aggregation and risk reporting (BIS, 2013, bis.org) | Standard | Advanced | Governance | The supervisory driver behind lineage, accuracy, and completeness demands | Essential |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Kleppmann, *Designing Data-Intensive Applications* (O'Reilly, 2017) | Book | Advanced | Data systems | The conceptual backbone: logs, consistency, batch vs stream | Essential |
| Reis & Housley, *Fundamentals of Data Engineering* (O'Reilly, 2022) | Book | Intermediate | DE lifecycle | Frames the whole discipline; you supply the finance examples | Recommended |
| Akidau, Chernyak & Lax, *Streaming Systems* (O'Reilly, 2018) | Book | Advanced | Streaming theory | Watermarks and windowing rigor that Phase 15 assumes | Optional |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Polars documentation & user guide (pola.rs) | Docs | Intermediate | Columnar dataframes | Fast local transforms; the practical alternative to pandas chains | Recommended |
| dbt blog (getdbt.com/blog) | Blog | Intermediate | Analytics engineering | Practitioner patterns: incremental models, tests, semantics | Optional |
| Feast blog & case studies | Blog | Intermediate | Feature platforms | How teams actually run feature stores in production | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| DataTalksClub Data Engineering Zoomcamp (free, GitHub) | Course | Beginner | DE end-to-end | Warm hands-on reps if pipelines are entirely new to you | Optional |

## 10. Practical Exercises

1. - [ ] Write ledger DDL (append-only postings, audit columns, constraints); insert a month of synthetic transactions; materialize balances; enforce append-only via permissions and test it.
2. - [ ] Build SCD2 and bitemporal customer tables; change an address twice; query "as of" each date on both axes; explain the difference in a concept note.
3. - [ ] Download Kraken or Binance OHLCV (canonical list); build a partitioned Parquet lake; benchmark DuckDB vs pandas on a one-year window; record results.
4. - [ ] Synthesize a 2-for-1 split plus dividend on a synthetic ticker; produce raw and adjusted series; write the corporate-action playbook for future-you.
5. - [ ] Author a Great Expectations suite over synthetic payment transactions (amount ranges, currency whitelist, freshness, referential integrity); plant bad data and prove CI fails.
6. - [ ] Build a dbt project (staging → intermediate → marts) over the market lake with tests and generated docs.
7. - [ ] Stand up Postgres + Kafka + Debezium via Docker Compose; stream changes into DuckDB with idempotent upserts; kill the consumer mid-stream and verify zero loss and zero duplicates.
8. - [ ] Build the LendingClub point-in-time feature set using only issue-date-known data; then build the leaky version; report the AUC gap and write the memo.
9. - [ ] Generate a messy synthetic customer table (typos, transpositions, nicknames); run rapidfuzz blocking plus splink scoring; evaluate precision/recall against planted ground truth.
10. - [ ] Implement deterministic keyed pseudonymization; show joins still work across tables while raw PII is gone; document the key-management risk in a decision record.

## 11. Mini Projects

**M1 — Point-in-time-correct feature pipeline on LendingClub.** Data: LendingClub (canonical list). Task: PIT-correct features vs deliberately leaky features; quantify the gap; memo the mechanism. Deliverable: feature pipeline + comparison report (the signature artifact of the phase). Difficulty: ★★★★☆.

**M2 — DuckDB + Parquet market-data lake.** Data: Kraken/Binance OHLCV (canonical list). Task: partitioned lake, corporate-action handling notes (crypto data has few, so document how the pattern generalizes to equities), query benchmarks. Deliverable: lake + benchmark report + playbook. Difficulty: ★★★☆☆.

**M3 — CDC pipeline: Postgres → Kafka → DuckDB.** Data: synthetic ledger churn. Task: Debezium CDC, idempotent sink, restart/replay proof, schema-evolution handling. Deliverable: compose stack + reliability test log. Difficulty: ★★★☆☆.

**M4 — Entity resolution on a messy customer table.** Data: synthetic customers with planted duplicates. Task: blocking + rapidfuzz/splink scoring + clustering; precision/recall with threshold justification. Deliverable: ER pipeline + evaluation report. Difficulty: ★★★☆☆.

## 12. Major Project Hook

Phase-culminating build: your PIT feature pipeline plus lakehouse become the data backbone for Phase 06 credit models (`../06-credit-risk/README.md`) and Phase 07 fraud features — the same pattern (feature store + expectation suites + lineage) scales into the production platform of Phase 17.

## 13. Case Studies & Industry Examples

- **Leakage in credit competitions**: public write-ups of LendingClub- and Home Credit-style competitions have repeatedly shown leaderboard scores inflated by future information — the public, hedged version of the trap your M1 demonstrates and eliminates.
- **Archegos (2021)**: publicly reported counterparty failures at several prime banks included risk-data aggregation and concentration-visibility problems — the real-world argument for BCBS 239's existence.
- **Adjusted-price inconsistencies across vendors**: free data sources publish different adjusted-close series for the same ticker (widely discussed publicly) — why you own your adjustment logic rather than inheriting someone else's.
- **US T+1 migration (May 2024)**: publicly reported operational deadline pressure across the industry — market-infrastructure changes are data-engineering deadlines in disguise, with the EU legislating a later move (verify current status).

## 14. Interview Questions

**Explain point-in-time correctness with an example.** A feature table joined on "latest balance" injects data from after the decision date; the correct version joins the balance as it stood at decision time via an as-of join against event history. The leaky model trains beautifully and decays instantly in production.

**Batch vs streaming for credit features — how do you choose?** Decision latency and freshness requirements decide: application scoring tolerates minutes-to-hours (batch), account management and fraud want seconds (streaming). Batch is simpler and auditable; streaming costs you idempotent sinks and ordering discipline — buy streaming only when the business case survives those costs.

**Why do corporate actions cause leakage?** Adjustment factors are computed from future events (a split next month changes today's adjusted price); using today's adjusted series to simulate past decisions injects future knowledge. Store raw prices plus an action table, and apply adjustments as-of any simulated date.

**What does BCBS 239 demand?** Accurate, complete, timely aggregation of risk data with clear governance, defined ownership, and traceable lineage — in engineering terms: quality contracts, documented transformations, snapshot discipline, and lineage you can show a supervisor.

**Design a ledger table — what are your invariants?** Append-only postings with signed amounts, balanced per transaction (sum = 0), immutable rows, monotonic sequencing or event time ordering, explicit currency and timestamps, plus idempotent posting keys — and a test suite that proves each invariant, not a comment that claims it.

**SCD2 vs bitemporal — when do you need both?** SCD2 answers "what was the value at date X" as best we knew it; bitemporal additionally answers "what did we know at date Y" by separating valid time from system time — required when corrections and late arrivals matter, which in finance they always do.

**What is survivorship bias and how do you prevent it?** Researching only entities that still exist (live tickers, active loans) inflates results because the failures are missing; prevention is point-in-time universes — reconstruct the eligible set as of each decision date, including those later delisted or defaulted.

**How do you guarantee training/serving feature parity?** One feature definition consumed by both paths (feature store), identical transformation code, logged serving-time inputs, and parity tests comparing offline computation against online responses for the same entities and timestamps.

**How would you tokenize PII while keeping analytical utility?** Deterministic keyed hashing or format-preserving tokens preserve equality joins across datasets while removing raw identifiers; keys live in a managed vault, and anything needing value analysis (amounts, dates) is separated from identifiers by design.

**How do you evaluate entity resolution without labels?** Planted ground truth on synthetic data, holdout hand-labeling on samples, and proxy metrics (cluster size distributions, match-rate stability across runs) — with the explicit caveat that thresholds encode a precision/recall trade-off someone must sign for.

## 15. Assessment — Can You Pass the Bar?

- [ ] Implementation: the PIT pipeline demonstrably closes an AUC gap versus the leaky baseline, with numbers in the memo.
- [ ] Implementation: the CDC pipeline survives a mid-stream consumer kill with zero loss and zero duplicates — test-proven.
- [ ] The Great Expectations suite fails CI on planted bad data and passes on clean data.
- [ ] Explain to a risk officer how your dbt docs, expectation suites, and snapshots answer a BCBS 239-style lineage request (the regulator-style item).
- [ ] Draw the corporate-action leakage trap on a whiteboard using a split example.
- [ ] Review your ledger schema aloud: invariants named, append-only enforced, audit columns present.
- [ ] Justify ER thresholds with precision/recall numbers and state who owns the trade-off.
- [ ] Regenerate the entire lake and both pipelines from raw data in one command.

## 16. Mastery Checkpoint

You may proceed to Phase 04 when:

1. M1-M4 exist in fintech-lab with green CI; the PIT memo quantifies the leakage gap with real numbers.
2. The lake, dbt docs, and expectation suites are wired so a stranger can trace data from source to feature.
3. The recorded ten-minute "data platform tour" (lake → pipelines → features → governance) is stored under `/notes/artifacts/`.
4. PROGRESS.md is updated; the baseline quiz data-engineering sections show improvement.
5. Your ledger schema and CDC stack are reusable by later phases without modification.

Evidence: repo links, benchmark and leakage reports, recorded tour. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Joining features on the latest snapshot instead of as-of dates — the leak that every validator hunts first.
- Using adjusted prices for signals and raw for P&L (or vice versa) without deciding deliberately; both mistakes are silent and expensive.
- Partitioning Parquet by high-cardinality columns — thousands of tiny files, 100x query overhead, and a lake nobody can query.
- At-least-once delivery without idempotent sinks: double-counted transactions that quietly double your revenue metrics.
- Treating data quality as notebook afterthoughts instead of CI-enforced contracts; dashboards nobody watches are not controls.
- Entity resolution with naive thresholds: silently merging distinct customers is worse than missing matches, especially in AML.
- Backfills that overwrite history instead of versioning it — you destroyed the evidence the audit needs.

## 18. Where This Goes Next

Phase 04 (`../04-financial-mathematics/README.md`) formalizes the mathematics — rates, volatility, portfolio theory — that your clean, point-in-time-correct data can now support. Phase 06 consumes your feature pipeline wholesale for credit decisioning, and Phase 09 extends your bars into forecasting with the leakage discipline you just built.
