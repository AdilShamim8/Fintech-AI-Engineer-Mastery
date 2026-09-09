# System Design — FinTech AI Architecture Problems

> Deliverable K. Six problems that senior FinTech AI interviews and real jobs actually contain. Practice each under 45 minutes: requirements → constraints → architecture → data/model flow → failure modes → compliance → capacity math. In Phase 20 you produce all six as written docs.
> Method note: draw the diagram first; numbers before nouns; every component needs a failure story.

---

## The Common Answer Skeleton (memorize)

```text
1. Clarify   users, decisions per second, decision latency budget, regs in scope
2. Requirements  functional (what decisions) + non-functional (p99, availability, DR, audit retention)
3. Constraints  data residency, model governance (SR 11-7-ish), vendor limits, cost ceiling
4. Data layer   sources, point-in-time discipline, feature store offline/online, retention
5. Model layer  candidates, training cadence, registry, validation gates, champion/challenger
6. Serving      sync API vs batch vs streaming; fallback to rules; latency budget breakdown
7. Data flow    event → features → score → policy → decision → audit store → downstream
8. Monitoring   drift, delayed labels, business KPIs, alerting ownership
9. Security/compliance  PII flows, entitlements, audit logging, explainability duties
10. Failure modes   component-by-component: what breaks, blast radius, mitigation
11. Capacity math   QPS × feature lookups × model cost; storage math for audit
12. Trade-offs  what you give up and why — the senior part of the answer
```

---

## Problem 1 — Real-Time Fraud Detection Platform

**Prompt:** Design the fraud decisioning system for a card issuer processing 5,000 txns/sec peak.

- **Decisions:** approve / decline / step-up (3DS) / queue for review. **Latency budget:** p99 <150ms app-level including features.
- **Data:** authorization stream (Kafka), account/device/merchant dimensions, 90-day histories, confirmed-fraud labels arriving 1-30 days later.
- **Architecture:** Kafka (keyed by account for ordering) → Flink streaming features (velocity aggregates, event-time watermarks) → online store (Redis) + feature fetch → model service (XGBoost ONNX, warm pools) → policy layer (thresholds + rules overlays) → decision + audit store (append-only, per-decision snapshot) → case management for analysts → label pipeline for retraining.
- **Model flow:** gradient boosting on tabular+velocity features; anomaly layer (Isolation Forest) for novel patterns; rules engine as fallback and veto; champion/challenger with shadow scoring.
- **Failure modes:** stream lag (shed load → rules-only mode), model service down (fallback rules), feature-store timeout (serve degraded feature set with flag), poison message (DLQ + replay), hot key whale account (salting, separate partition budget).
- **Capacity math:** 5k QPS × 50 feature lookups = 250k lookups/s → Redis cluster sizing; audit row ~10KB × 5k/s → ~4.3TB/day → tier storage, PII minimization in snapshots.
- **Compliance:** decision reasons logged; adverse-impact monitoring on declines; PCI DSS scope boundaries (tokenized PANs only).
- **Trade-offs to name:** exactly-once features vs latency; model depth vs p99; recall vs friction at holiday peaks.

## Problem 2 — AI-Powered Credit Decisioning System

**Prompt:** Design automated underwriting for a personal-loan fintech: 50 applications/sec peak, decision in <2s, US regulated.

- **Decisions:** approve/decline + amount + APR (risk-based pricing) + adverse-action reasons on decline.
- **Data:** application form, bureau pull (tradelines, inquiries), open-banking cash-flow (opt-in), internal performance of past loans; IFRS-9/CECL-adjacent outcome tracking.
- **Architecture:** intake API → application store → feature pipeline (point-in-time bureau snapshot, cash-flow aggregates) → calibrated PD model (scorecard + constrained GBM) → policy & pricing engine (cutoffs, affordability rules, margin − EL) → reason-code generator → decision letter service → audit store → monitoring (approval-rate stability, PSI, delayed default labels) → model governance (registry, validation reports).
- **Model flow:** calibration is load-bearing (pricing consumes PD); reason codes from SHAP mapped to controlled dictionary; fairness testing (disparate impact, error-rate gaps) per release; champion/challenger with swap-set analysis.
- **Failure modes:** bureau outage (queue vs manual lane), stale features (freshness SLA + version pinning), calibration drift after macro shift (guardrail alarms), unfair proxy discovered (retrain + retest protocol).
- **Capacity math:** 50 QPS decisions; bureau calls cost $ and rate limits → caching + prequalification tiers; audit retention 7+ years → immutable object storage with lifecycle.
- **Compliance:** Reg B adverse action (specific reasons), fair lending, EU AI Act analog if EU (creditworthiness = high-risk), model risk documentation.
- **Trade-offs:** model complexity vs reason-code quality; approval-rate growth vs loss-rate risk; alternative data upside vs proxy-discrimination exposure.

## Problem 3 — AML Transaction Monitoring Platform

**Prompt:** Design TM for a mid-size bank: 2,000 txns/sec ingested, analyst team of 40, regulator expects explainable scenarios.

- **Decisions:** alert / no-alert per customer-scenario pair; alert → triage → investigation → SAR (regulatory filing).
- **Data:** transactions, customer/KYC data, counterparty graph, external watchlists; typology library (structuring, mules, layering).
- **Architecture:** ingestion (batch + streaming) → scenario engine (thresholded rules, versioned) → graph feature jobs (community detection, centrality, flow paths) → risk scoring (rules + GBM + optional GNN on Elliptic-style graph) → alert queue ranked by expected value (learning-to-rank under capacity) → case management → SAR narrative drafting (LLM with human approval) → regulator-ready reporting.
- **Model flow:** scenarios are validated models too (SR 11-7 applies); tuning backlog driven by alert-precision and hitherto-undetected-typology reviews; entity resolution upstream (dedupe customers/counterparties).
- **Failure modes:** silent scenario regression after upstream schema change (data contracts + scenario backtesting), alert backlog (capacity-aware ranking + auto-closure criteria with audit), graph job explosion (sampling by risk band), LLM narrative hallucination (extract-from-case-evidence only, human sign-off).
- **Capacity math:** 40 analysts × 25 alerts/day = 1,000/day triage capacity → ranking must keep precision@1000 high; graph: 100M edges → incremental computation on risk subgraphs.
- **Compliance:** BSA/AML (US) or jurisdictional analog; FATF risk-based approach; full alert-decision audit trail; model validation for both rules and ML.
- **Trade-offs:** detection sensitivity vs analyst burnout; novel-typology exploration vs false-positive cost; GNN explainability for investigators.

## Problem 4 — Financial RAG & Research Assistant

**Prompt:** Design an internal research assistant over 10 years of filings, research notes, and internal policies for 2,000 analysts.

- **Requirements:** every claim cited to document+page; entitlements (analyst A cannot see client-confidential docs); version-aware (superseded policies must not resurface); p95 <3s; eval harness blocking regressions in CI.
- **Data:** EDGAR filings (parsed, sectioned), internal research PDFs, policy corpus with effective/superseded dates, metadata: issuer, doc type, date, jurisdiction, ACL groups.
- **Architecture:** ingestion pipeline (layout-aware parsing, table extraction) → chunking (section/table-aware) → embeddings + BM25 index (pgvector/OpenSearch) → retrieval service (hybrid + RRF + cross-encoder rerank; metadata filters first) → generation (citation-forced prompting, numeric-fidelity checker) → eval harness (golden set, faithfulness, judge calibration) → audit logs.
- **Model flow:** query rewriting/decomposition; table-aware retrieval for financial figures; GraphRAG layer over entity graph (LEI canonicalization) for "who-owns-whom" questions.
- **Failure modes:** stale-document answer (supersession filter tests), injection hidden in retrieved PDF (treat as data, sandbox tools), numeric hallucination (verification pass re-extracts figures), ACL leak (entitlement filter at retrieval, tested adversarially), cost blowout (semantic caching + model routing).
- **Capacity math:** 2k users × 20 queries/day = 40k QPS-day → ~1 QPS avg, 15 QPS peak; index sizing: 10 years × 5k issuers × ~1MB text → embedding cost and refresh strategy.
- **Compliance:** PII/confidentiality, audit of prompts/responses, entitlements model, retention policy.
- **Trade-offs:** freshness vs index cost; rerank latency vs answer quality; self-host vs API (residency).

## Problem 5 — Payment Risk & Orchestration Engine

**Prompt:** Design a payment-orchestration + risk layer for a marketplace: 1,000 checkouts/sec peak, multiple PSPs, cross-border.

- **Decisions:** routing (PSP choice by cost/success-rate/risk), 3DS step-up trigger, payout holds, chargeback-probability pricing.
- **Data:** checkout events, PSP performance telemetry (auth rates, latency, fees), merchant risk profiles, FX rates, fraud scores from P5-style model, scheme rules.
- **Architecture:** checkout API → risk pre-screen (velocity, device, fraud score <50ms) → routing engine (multi-armed bandit or score-based over PSPs with constraints) → PSP adapters (idempotent, retry-safe) → settlement reconciliation (near-real-time matching) → chargeback prediction → merchant dashboards → feedback loop into risk model.
- **Model flow:** routing = contextual bandit with guardrails (cost, success prob, risk); chargeback model = GBM on transaction+merchant features; all routing changes shadow-tested.
- **Failure modes:** PSP outage (health-checked failover, idempotent retries — no double charges), split-brain routing during deploy (versioned decisions, reconciliation catches), FX feed staleness (bounded-staleness SLA), reconciliation breaks (exception queue with owner).
- **Capacity math:** 1k QPS × 2 PSP calls p50 → connection pools, regional routing; reconciliation: 86M events/day → stream matching keyed by txn id.
- **Compliance:** PCI DSS (scope minimization via tokenization), PSD2/SCA where applicable, cross-border licensing awareness.
- **Trade-offs:** optimization aggressiveness vs routing stability; centralizing risk decisions vs merchant autonomy.

## Problem 6 — Bank-Scale AI Platform (Governed MLOps)

**Prompt:** Design the ML platform for a top-20 bank: 300+ models across credit, fraud, AML, marketing; strict governance; 15 ML teams as customers.

- **Requirements:** model inventory + tiering; validation workflow; reproducible training; feature store shared across teams; per-decision explanations; audit for regulators; multi-region DR; self-service for teams without governance bypass.
- **Architecture:** federated platform — shared: feature store (offline/online), training platform (notebooks→pipelines), registry with governance states (draft→validated→approved→production→retired), serving (batch + realtime), monitoring (drift, performance, fairness dashboards), decision audit store, lineage (BCBS 239-flavored); per-domain: risk-specific tooling, LLM gateway with logging/redaction.
- **Model flow:** promotion gates: reproducible build → independent validation report → risk-committee approval → staged rollout (shadow → canary) → production with monitoring SLAs → scheduled revalidation.
- **Failure modes:** platform outage blocks decisioning (multi-region, degraded-mode policy per model tier), pipeline/platform mismatch (feature parity tests), governance theater (gates too slow → shadow models emerge; measure gate cycle time), cost runaway (showback/chargeback per team).
- **Capacity math:** 300 models × retrain cadences → shared compute quotas; audit storage growth; validation-team throughput as the bottleneck — design gates to parallelize.
- **Compliance:** SR 11-7-shaped MRM, EU AI Act high-risk duties (where applicable), DORA operational resilience, third-party AI vendor risk.
- **Trade-offs:** central governance vs team velocity; build platform vs buy (FICO/SAS/DataRobot-class) vs hybrid; consistency vs domain-specific flexibility.

---

## Practice Protocol

1. **First pass:** 45 minutes, timer on, diagram + skeleton filled, no resources.
2. **Second pass:** compare against the problem's sections; list every component you missed.
3. **Third pass (48h later):** redo cold; the delta is your real knowledge.
4. **Write it up** as an ADR-style doc in `notes/decisions/` — Phase 20 collects all six.
5. **Verbal drill:** record a 10-minute walkthrough; watch for hand-waving (it marks exactly where depth is missing).
