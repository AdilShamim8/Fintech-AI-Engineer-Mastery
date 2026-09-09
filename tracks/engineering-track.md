# Track — Production Engineering for FinTech AI

> Deliverable I. The engineering progression, with the financial-system-specific constraints that make this track different from generic MLOps: money moves, decisions are legally defensible, and failures cost real dollars in seconds.
> Main teaching: Phase 03 (data), 15 (real-time), 17 (production), 16 (governance engineering).

---

## 1. Progression Map

```text
Level 1 — Data foundations      SQL/python data modeling, ledgers, quality, lakehouse, PIT correctness
Level 2 — Pipelines             batch orchestration, CDC, streaming intro, feature stores
Level 3 — Services              model serving, APIs, containers, CI/CD
Level 4 — Real-time money       Kafka/Flink, exactly-once, idempotency, outbox, p99 budgets
Level 5 — Governed production   monitoring, drift, audit stores, incidents, reproducibility
Level 6 — Platform & scale      multi-domain platform design, DR, cost, vendor-vs-build
```

## 2. What Finance Adds to Standard MLOps

| Standard MLOps concern | FinTech version | Why |
|---|---|---|
| Train/serve skew | **Point-in-time correctness + online/offline feature parity** | A feature computed with post-decision data invalidates the model legally and statistically |
| Monitoring | **Delayed-label performance + PSI drift + business KPIs** | Fraud/credit labels arrive weeks late; approval-rate stability is the early alarm |
| Reproducibility | **Regulator-grade reproducibility** | Validators re-run your model years later: data snapshots, seeds, env pins, immutable logs |
| Logging | **Decision audit stores** | Every decision reconstructable: inputs, model version, policy version, output, reasons — retained for years |
| Deployment | **Shadow + champion/challenger as governance steps** | Model changes often need risk-committee approval, not just CI |
| Reliability | **Graceful degradation to rules** | Auth-time scoring must fall back to a rules engine within SLA, not to a 500 |
| Testing | **Golden datasets + statistical tests + load tests to p99** | Model quality gates are contractual; latency is a feature |
| DR | **RTO/RPO for decisioning** | Payment/fraud decisions cannot wait hours for failover |
| Change management | **Model change = controlled change** | Versions, approvals, and rollback plans are part of the artifact |

## 3. The Stack Map (by level)

**Level 1 — Data foundations**
- SQL advanced (window functions, incremental models) · Python data stack (polars/duckdb/pyarrow)
- Storage: Parquet, lakehouse tables (Delta/Iceberg), warehouse modeling (dbt)
- Data quality: Great Expectations/soda; data contracts; schema registries
- **FinTech anchor:** PIT feature pipeline on LendingClub; corporate-action handling on market data

**Level 2 — Pipelines & features**
- Orchestration (Airflow/Prefect), CDC (Debezium), streaming fundamentals (Kafka topics/keys/consumer groups)
- Feature stores (Feast): offline/online, entity dataframes, freshness SLAs
- **FinTech anchor:** streaming velocity features; entity resolution pipelines; BCBS 239-style lineage doc

**Level 3 — Services**
- FastAPI serving, containers, CI/CD (GitHub Actions), model registry (MLflow), canary/shadow deploys
- **FinTech anchor:** decision API with idempotency keys + reason codes; per-decision explanation endpoint

**Level 4 — Real-time money movement**
- Kafka in depth (EOS semantics, transactions), Flink (event time, watermarks, state, checkpoints), CEP patterns
- Idempotent consumers, transactional outbox, dedupe stores, hot-key handling
- Low-latency serving: ONNX Runtime/Triton, warm pools, horizontal scale, load shedding
- **FinTech anchor:** end-to-end auth-time fraud path at p99 <100ms with rules fallback (Phase 15 capstone)

**Level 5 — Governed production**
- Monitoring: Evidently/NannyML/Alibi-Detect; drift alarms; delayed-label eval jobs; business dashboards
- Decision audit store design + retention; incident management & runbooks; postmortems
- Reproducibility: pinned envs, data snapshots, deterministic training where feasible
- **FinTech anchor:** full MLOps pipeline for the Phase 06 scorecard; simulated-drift drill; ML on-call rotation sim

**Level 6 — Platform & scale**
- Multi-domain platform design (feature platform, training platform, serving platform, governance layer)
- Cost engineering: right-sizing, distillation, caching, batch/real-time reuse
- DR: multi-region active/passive for decisioning; chaos drills; vendor-vs-build per component
- **FinTech anchor:** written platform design doc (Phase 20 problem #6); vendor evaluation matrix

## 4. Security & Access Engineering (woven through all levels)

- PII minimization, tokenization/pseudonymization at rest and in features
- Access control to features/decisions (entitlements), least-privilege service identities
- Secrets management; encryption in transit/at rest; PCI DSS boundaries for card data
- LLM-era: prompt/response logging with redaction; prompt-injection surface reduction (Phase 14/16)

## 5. Evidence Bar for This Track

- [ ] Level 1-2: PIT-correct pipeline + streaming velocity features (code + tests + lineage diagram).
- [ ] Level 3: decision API with idempotency, reasons, and CI (OpenAPI spec + load test results).
- [ ] Level 4: p99 <100ms end-to-end demo + failover demo (recorded).
- [ ] Level 5: drift alarm drill + postmortem written from a seeded incident.
- [ ] Level 6: platform design doc with per-component build-vs-buy rationale.

**Relation to phases:** this track is the engineering spine of Phases 03 → 15 → 17; its evidence artifacts double as those phases' gate evidence.
