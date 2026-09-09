# 04 — Detailed Roadmap (Phase Index)

> Deliverable D. The ordered lesson spine of all 21 phases. The full teaching content — resources, exercises, projects, assessments — lives in each `phases/<dir>/README.md` (linked).

---

## Stage I — Domain Bridge

### Phase 00 — Orientation & Baseline · [`README`](../phases/00-orientation/README.md)
Lessons: repo operating system (loop, evidence, PROGRESS) → baseline finance self-assessment → notes architecture → solo GitHub workflow → environment (uv/Docker/DuckDB/cloud sandbox) → reading a 10-K → pacing selection → artifact conventions.
**Ship:** `fintech-lab` repo with CI + note templates adopted.

### Phase 01 — Financial Systems Foundations · [`README`](../phases/01-financial-foundations/README.md)
Lessons: money & aggregates → double-entry ledgers → bank balance sheets & money creation → central banking & policy rates → TVM/NPV/IRR → yield curve → inflation & macro → FX basics → market taxonomy → instruments → institutions → market infrastructure & settlement → 2008 anatomy (+ SVB-2023 lens).
**Ship:** TVM/bond library · FRED yield-curve study · ledger simulator.

### Phase 02 — Banking, Payments & Lending Operations · [`README`](../phases/02-banking-payments-lending/README.md)
Lessons: bank business lines → core banking → payment rails world map → four-party card model & interchange → payment lifecycle & chargebacks → PCI DSS boundaries → wallets/BNPL → cross-border & correspondent banking → SWIFT/ISO 20022 → open banking → loan lifecycle → mortgages → insurance fundamentals → wealth basics.
**Ship:** idempotent payment state machine · ISO 20022 pain.001 parser · categorizer · reconciliation matcher.

## Stage II — Data & Quant Core

### Phase 03 — Financial Data Engineering · [`README`](../phases/03-financial-data-engineering/README.md)
Lessons: data species → ledger/event models → immutable logs → SCD/bitemporality → market data & corporate-action leakage → data quality/expectations → batch pipelines → streaming intro & CDC → lakehouse/columnar → feature stores & PIT correctness → entity resolution → PII/tokenization → BCBS 239.
**Ship:** PIT-correct LendingClub feature pipeline · DuckDB market lake · CDC pipeline · entity-resolution run.

### Phase 04 — Financial Mathematics · [`README`](../phases/04-financial-mathematics/README.md)
Lessons: TVM deep → bond math → NPV/IRR pitfalls → finance probability (Bayes) → distributions (lognormal/fat tails/Poisson) → moments/tails → correlation vs dependence/copulas → covariance & PCA → optimization (QP) → stochastic processes (GBM/Itô/Poisson) → Monte Carlo → numerical methods → VaR/CVaR math → portfolio math (MPT/CAPM/IR).
**Ship:** MC option pricer · duration/convexity calculator · copula default simulator · yield-curve PCA · efficient frontier.

### Phase 05 — Statistics, Econometrics & Causal Inference · [`README`](../phases/05-statistics-econometrics/README.md)
Lessons: inference for noisy finance → multiple testing & data snooping → OLS/robust errors → logistic as scorecard base → GLMs (Tweedie) → regularization → stationarity/ADF → ARIMA/SARIMAX → Granger limits → cointegration → VAR → GARCH → ETS/state space → purged CV & deflated Sharpe → Bayesian reasoning → causal: DAGs/A-B/DiD/synthetic control/uplift/IV → panels.
**Ship:** walk-forward GARCH eval · pairs screener · uplift model · DiD study · purged-K-fold leakage demo.

## Stage III — Core Financial ML

### Phase 06 — Credit Risk & Decisioning · [`README`](../phases/06-credit-risk/README.md)
Lessons: PD/LGD/EAD taxonomy → bureau data → scorecards (WOE/IV) → decision policy/cutoffs → ML challengers → calibration → reject inference → LGD/EAD → EL & IFRS 9/CECL → portfolio views → monitoring/PSI → adverse-action explainability → fairness → governance/MDD.
**Ship:** scorecard + calibrated challenger + reason codes + MDD. → Flagship 02.

### Phase 07 — Fraud Detection & Payment Intelligence · [`README`](../phases/07-fraud-payment-intelligence/README.md)
Lessons: fraud taxonomy → error economics → imbalance done right → business-level evaluation → feature engineering (velocity/device/network) → anomaly detection → sequence models → real-time architecture → rules+ML hybrid → case management/label latency → drift & adversaries → 3DS decisioning → chargeback prediction.
**Ship:** cost-sensitive IEEE-CIS model · ULB PR-AUC + threshold economics · velocity pipeline · hybrid rules engine. → Flagships 01, 07.

### Phase 08 — AML & Financial Crime · [`README`](../phases/08-aml-financial-crime/README.md)
Lessons: BSA/FATF frame → KYC/CDD/EDD/KYB/UBO → sanctions/PEP screening → TM scenarios & tuning → typologies → triage & SAR → entity resolution → graph analytics → GNNs (Elliptic) → trade surveillance → scenario validation → investigator explainability → crypto compliance.
**Ship:** sanctions screener · rules TM engine · community-detection features · GNN on Elliptic · alert ranker. → Flagship 03.

### Phase 09 — Financial Time Series & Forecasting · [`README`](../phases/09-financial-time-series/README.md)
Lessons: why financial TS is hostile → return computation pitfalls → leakage traps → walk-forward → classical baselines → boosting with lags → global models → deep TS → foundation models (2026 status) → probabilistic forecasting → volatility (GARCH/HAR-RV) → evaluation + utility → hierarchical reconciliation → banking use cases → nowcasting → regimes → triple-barrier/meta-labeling.
**Ship:** M5-style hierarchical forecast · vol comparison · ATM cash demand · nowcast · meta-labeling run. → forecasting platform.

## Stage IV — Markets & Quant

### Phase 10 — Quantitative Finance for AI Engineers · [`README`](../phases/10-quantitative-finance/README.md)
Lessons: portfolio theory → factor models → risk measures & VaR engines → fixed income analytics → derivatives & greeks → vol surface → hedging (incl. deep hedging concept) → microstructure & impact → algo-trading stack → quant research workflow → risk engines/stress → attribution → rebalancing.
**Ship:** BS pricer+IV solver · binomial American pricer · 3-method VaR engine + Kupiec · FF3 regression · cost-honest backtests · LOB exploration.

## Stage V — Advanced Financial AI

### Phase 11 — Financial NLP & Document Intelligence · [`README`](../phases/11-financial-nlp-documents/README.md)
Lessons: finance text species → domain sentiment (L&M vs FinBERT) → NER → relations/events → numerical reasoning (FinQA/TAT-QA) → table/layout extraction → long-document strategies → EDGAR/XBRL engineering → complaint analytics → multilingual → hallucination measurement → HITL extraction QA → PII (Presidio) → sentiment-as-features → summarization faithfulness.
**Ship:** FinBERT benchmark · 10-K retrieval baseline · earnings tone tracker · CFPB classifier · extraction pipeline. → Flagship 04.

### Phase 12 — Generative AI for Finance · [`README`](../phases/12-generative-ai-finance/README.md)
Lessons: GenAI landscape & archetypes → risk tiering → finance prompting patterns → structured outputs → citation-forced grounding → numeric fidelity → RAG-vs-finetune decisions → domain adaptation lite → eval harnesses → safety/compliance layers → deployment patterns → red-teaming.
**Ship:** cited copilot + numeric checker + golden set + CI evals + red-team corpus. → Flagship 05.

### Phase 13 — Financial RAG & Knowledge Systems · [`README`](../phases/13-financial-rag-knowledge/README.md)
Lessons: finance corpora properties → doc processing at scale → finance-aware chunking → embeddings (own-corpus evals) → hybrid retrieval → reranking → metadata filters (version/jurisdiction) → query understanding → identifiers & knowledge graphs (LEI/ISIN) → GraphRAG → table-aware retrieval → retrieval+faithfulness evals → golden sets → citation UX → freshness/supersession → ACL-aware retrieval → compliance uses → production economics → injection defense.
**Ship:** filings RAG with full eval · ACL policy assistant · GraphRAG demo · hybrid benchmark. → Flagship 05.

### Phase 14 — Agentic FinTech Systems · [`README`](../phases/14-agentic-fintech/README.md)
Lessons: agent anatomy → workflow-vs-agent decision framework → finance tool design → ReAct failure modes → multi-agent patterns → reflection → memory & privacy → human-approval gates → auditability → idempotent financial actions → guardrails/injection → agent evals & simulations → framework landscape (LangGraph/AutoGen/MCP) → archetypes by risk-adjusted ROI → trading agents as frontier → sandboxing → cost/degradation.
**Ship:** reconciliation agent · research agent · triage copilot · memo generator · scenario eval harness. → Flagship 06.

### Phase 15 — Real-Time & Streaming Financial AI · [`README`](../phases/15-real-time-streaming/README.md)
Lessons: latency budgets → Kafka essentials & EOS → Flink (event time/watermarks/state) → CEP → online features → stream+sync-scoring architecture → low-latency serving → idempotent decisioning/outbox → backpressure & load shedding → late events → streaming joins → hot keys → streaming ML monitoring → CQRS → near-real-time reconciliation → vector search p99 → cost.
**Ship:** Kafka→Flink→scoring path at p99 <100ms · watermark experiments · outbox processor · failover demo. → Flagship 01.

## Stage VI — Production, Compliance & Leadership

### Phase 16 — Security, Compliance & Responsible AI · [`README`](../phases/16-security-compliance-responsible-ai/README.md)
Lessons: model risk management (SR 11-7; 2025-26 updates caveat) → inventory & tiering → independent validation → EU AI Act & DORA → US consumer-protection stack → privacy (GDPR/CCPA) → fairness engineering → explainability duties → privacy-enhancing tech → secure ML (OWASP/MITRE) → vendor risk → governance operating model → auditability engineering → incident playbooks → red-teaming.
**Ship:** SR 11-7 dossier · fairness audit · DP experiment · threat model + red team · reason-code generator.

### Phase 17 — Production FinTech AI Engineering · [`README`](../phases/17-production-fintech-ai/README.md)
Lessons: reference architectures → platform components & build-vs-buy → CI/CD for models → testing ML → reproducibility → monitoring → retraining governance → ML incidents → rollback/shadow → per-decision explainability → decision audit stores → fintech experimentation → cost/perf → DR/multi-region → retention/PII → change control → SLOs → postmortems → docs-as-code.
**Ship:** full MLOps pipeline · drift monitors · shadow harness · audit store · load test to p99. → productionize flagship.

### Phase 18 — Capstone Systems · [`README`](../phases/18-capstones/README.md)
Selection matrix → PRD → data contract → thin slice → milestone plans (two worked examples: 10-week fraud platform, 8-week RAG assistant) → evaluation strategy → security/compliance by design → documentation standards → public portfolio package → mock defense.
**Ship:** 1-2 deployed, defended flagships with full definition-of-done.

### Phase 19 — Specialist Tracks & Research Frontier · [`README`](../phases/19-specialist-tracks/README.md)
Electives (pick 2-3): A Quant/Trading ML · B WealthTech & personalization · C InsurTech · D Graph ML deep · E Digital assets & DeFi risk · F RegTech/SupTech · G Privacy-enhancing tech · H Treasury & liquidity AI. Each with resources, a mini project, and a skill checklist. Frontier work feeds [`research/`](../research/README.md).

### Phase 20 — Senior & Architect Level · [`README`](../phases/20-senior-architect/README.md)
Timed system design (6 problems) → ADRs & tradeoff writing → build-vs-buy & vendor landscape → platform strategy → org design & three lines of defense → stakeholder translation → regulatory engagement → product economics → portfolio & brand → interviewing/hiring → mentoring → roadmap writing → ethics leadership.
**Ship:** 6 design docs · public talk/blog · promotion-packet self-review · teaching artifact.
