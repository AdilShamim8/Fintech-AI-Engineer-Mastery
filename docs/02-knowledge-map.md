# 02 — The FinTech AI Knowledge Map

> Deliverable B. The complete knowledge universe, priority-tagged.
> Tags: **[MUST]** interview-and-job critical · **[SHOULD]** expected of a strong engineer · **[ADV]** senior differentiator · **[SPEC]** specialist depth · **[OPT]** optional/context.

---

## 1. Financial Foundations

- [MUST] Money, monetary aggregates, money creation · double-entry bookkeeping & ledgers
- [MUST] Interest mechanics: TVM, compounding, discounting, NPV/IRR
- [MUST] Central banking: policy rates, inflation, the yield curve
- [MUST] Financial system architecture: institutions, markets, instruments (equity/bond/FX/derivatives taxonomy)
- [SHOULD] Market infrastructure: exchanges, CCPs, CSDs, custody, settlement cycles
- [SHOULD] Financial statements (bank balance sheet & P&L), 2008 anatomy, systemic-risk intuition
- [ADV] Macro transmission to credit/fraud cycles (rate cycles → default cycles)
- [OPT] Monetary history, crypto-monetary alternatives

## 2. Banking, Payments & Lending Operations

- [MUST] Bank business lines (retail/commercial/corporate); loan lifecycle end-to-end
- [MUST] Card four-party model, interchange, auth→settlement lifecycle, chargebacks
- [MUST] Payment rails map: cards, ACH, wires, SEPA, UK FPS, RTP/FedNow, PIX, UPI
- [MUST] Idempotency, retries, reconciliation — the engineering trinity of money movement
- [SHOULD] ISO 20022, SWIFT, cross-border/correspondent banking, FX conversion
- [SHOULD] Open banking (PSD2/FDX), BaaS/embedded finance, wallets, BNPL
- [SHOULD] Insurance fundamentals (loss/combined ratio), wealth-management basics
- [ADV] Core-banking internals, settlement finality, PCI DSS boundaries
- [OPT] Local rail deep-dives beyond your market

## 3. Financial Data Engineering

- [MUST] Transaction/market/reference/regulatory data species; ledger & event models
- [MUST] **Point-in-time correctness** (the defining skill); leakage taxonomy
- [MUST] Data quality & expectations testing; schema/contract management
- [MUST] Batch (dbt/Airflow) vs streaming (Kafka) pipelines; CDC
- [SHOULD] Feature stores (offline/online parity); lakehouse formats; columnar engines
- [SHOULD] Corporate actions, survivorship bias; market data (ticks, bars, LOB)
- [SHOULD] PII handling/tokenization; BCBS 239 lineage principles
- [ADV] Entity resolution at scale; bitemporal warehouses; data contracts governance
- [SPEC] Streaming feature computation; real-time online stores at p99

## 4. Financial Mathematics & Statistics

- [MUST] Probability & distributions for finance (lognormal, fat tails, Poisson); expectations & cost matrices
- [MUST] Logistic regression & calibration mathematics; WOE/IV
- [MUST] Time-value math, bond math (duration/convexity)
- [MUST] Multiple testing, selection bias, leakage — the statistics of deception
- [SHOULD] Regression/GLMs (Tweedie), regularization; Bayesian reasoning
- [SHOULD] Time-series econometrics: ARIMA, GARCH, cointegration, stationarity
- [SHOULD] Monte Carlo; basic stochastic processes (Brownian, GBM, Poisson arrivals)
- [ADV] Copulas & tail dependence; PCA on term structure; convex optimization (QP portfolios)
- [ADV] Causal inference (DiD, uplift, synthetic control); purged CV & deflated Sharpe
- [SPEC] Stochastic calculus (Itô), measure theory for pricing; numerical PDE methods

## 5. Credit & Risk

- [MUST] PD modeling: scorecards, constrained boosting, calibration
- [MUST] Expected loss economics; cutoff/profit optimization; risk-based pricing
- [MUST] Explainability for credit (reason codes, adverse action); fairness testing
- [SHOULD] LGD/EAD modeling; reject inference; vintage/roll-rate analytics
- [SHOULD] Drift monitoring (PSI), champion/challenger, model governance basics
- [ADV] IFRS 9/CECL provisioning mechanics; portfolio credit (ASRF), migration matrices
- [ADV] Stress testing & scenario analysis; collections treatment optimization
- [SPEC] IRB capital modeling; structured credit; SME/corporate rating models

## 6. Fraud & Financial Crime

- [MUST] Fraud taxonomy (CNP, ATO, synthetic identity, first-party, APP scams)
- [MUST] Imbalance & cost-sensitive learning done right; calibration under resampling
- [MUST] Velocity/behavioral/network features; alert economics (precision at capacity)
- [MUST] Real-time scoring architecture (auth-time budgets, fallback to rules)
- [SHOULD] Anomaly detection (IF, LOF, autoencoders); sequence models on transactions
- [SHOULD] AML stack: KYC/CDD, sanctions/PEP screening, TM scenarios, SAR workflow
- [SHOULD] Graph analytics: community detection, centrality; entity resolution
- [ADV] GNNs for fraud/AML (Elliptic line of work); label-latency handling; adversarial drift
- [ADV] Trade surveillance patterns; crypto compliance analytics
- [SPEC] Federated fraud intelligence; synthetic data for fraud sharing

## 7. Markets, Quant & Time Series

- [MUST] Forecasting discipline: walk-forward, probabilistic forecasts, leakage-free features
- [MUST] Volatility modeling basics (GARCH family, realized vol)
- [SHOULD] Portfolio theory & factor models; VaR/ES engines; backtesting honesty (purging, deflation)
- [SHOULD] Derivatives literacy: BS, greeks, IV surface; Monte Carlo pricing
- [SHOULD] Banking forecasting use cases: liquidity, cash demand, deposit flows
- [ADV] Market microstructure (LOB, impact, TCA); execution (Almgren-Chriss)
- [ADV] Meta-labeling, triple-barrier; hierarchical reconciliation; nowcasting
- [SPEC] Deep hedging (RL); optimal execution with ML; high-frequency signals

## 8. Financial NLP & Document AI

- [MUST] Financial text species & their evals; domain sentiment (L&M lexicon, FinBERT)
- [MUST] Extraction pipelines (tables, layout models); span-level evaluation
- [SHOULD] NER/relations/events for finance; EDGAR/XBRL data engineering
- [SHOULD] Numerical reasoning over tables+text (FinQA/TAT-QA); hallucination measurement
- [ADV] Long-document strategies; multilingual finance; complaint analytics at scale
- [SPEC] Contract intelligence; regulatory-text versioning systems

## 9. Generative AI, RAG & Agents for Finance

- [MUST] Risk-tiering financial GenAI use cases; human-approval gates
- [MUST] RAG with citation-forcing, version/jurisdiction filters, ACLs
- [MUST] Evaluation: golden sets, retrieval metrics, faithfulness, judge calibration
- [SHOULD] Numeric fidelity engineering; PII redaction; prompt-injection defense
- [SHOULD] Agent workflows under audit (deterministic-first design); trajectory evals
- [ADV] GraphRAG & finance knowledge graphs (LEI/ISIN); model routing & cost engineering
- [ADV] Multi-agent patterns; memory with privacy; sandboxed execution
- [SPEC] Domain pretraining economics (BloombergGPT lesson); on-prem LLM platforms

## 10. Production, Real-Time & Platform Engineering

- [MUST] Model serving + monitoring (drift, delayed labels, business KPIs)
- [MUST] Decision audit stores; reproducibility; incident response for ML
- [MUST] Exactly-once/idempotency in decisioning; streaming joins; backpressure & graceful degradation
- [SHOULD] CI/CD for models (canary/shadow/champion-challenger); feature platforms; registry discipline
- [SHOULD] Latency engineering (p99 budgets); load testing; cost per decision
- [ADV] Multi-region DR for decisioning; ML platform product design; retraining governance
- [SPEC] Bank-scale platform architecture; vendor-vs-build strategy per component

## 11. Compliance, Security & Responsible AI

- [MUST] SR 11-7 model risk framework (shape + vocabulary); fair-lending basics (Reg B, adverse action)
- [MUST] Explainability duties; PII/confidentiality engineering; audit logging
- [SHOULD] EU AI Act (high-risk duties for credit), DORA (operational resilience), GDPR Art 22
- [SHOULD] Fairness metrics & their conflicts; proxy discrimination; testing regimes
- [SHOULD] Secure ML: poisoning, theft, injection; OWASP LLM Top 10 awareness
- [ADV] Model validation preparation; third-party/vendor AI risk; red-teaming finance copilots
- [ADV] Privacy-enhancing tech: DP, federated learning, TEEs (concept + one honest experiment)
- [SPEC] Supervisory engagement; ISO 42001 implementation; AI governance operating models

## 12. Product, Leadership & Research

- [SHOULD] Decision economics (approval × margin − loss; cost per decision); PRD writing for AI
- [SHOULD] System design under financial constraints (see /system-design/)
- [SHOULD] Stakeholder translation (risk/compliance/audit); build-vs-buy reasoning
- [ADV] Vendor ecosystem literacy; org design (three lines of defense); platform strategy
- [ADV] Research literacy: reading/reproducing papers; maturity labeling (established → frontier)
- [SPEC] Writing/public speaking as career infrastructure; mentoring systems

---

## Reading the Map

- **MUST ≈ 60 items** — this is the employable core; the interview track tests almost exclusively these.
- Every [MUST] maps to ≥1 phase lesson + ≥1 exercise + ≥1 project hook — traceability is deliberate.
- Depth ordering follows the curriculum: the map is breadth, [ROADMAP.md](../ROADMAP.md) is sequence.
- Update this file yearly: promote items as the industry shifts (e.g., GenAI evaluation moved SHOUL→MUST during 2024-2026).
