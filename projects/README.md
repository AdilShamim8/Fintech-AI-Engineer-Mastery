# Projects — The Ladder

> **Projects are the unit of progress, not courses.** Phases build capability; projects convert capability into evidence. A finished project is a repository with a working system, an honest evaluation, and a writeup a hiring manager or risk officer can read in five minutes. Everything in this curriculum routes toward the artifacts on this page.
>
> **Position in path:** `../phases/` (learning) ← **this page** → `../case-studies/README.md` (judgment)

## 1. Why Projects Are the Unit of Progress

- Courses and lessons create *recognition*; projects create *retention and proof*. In hiring and in production, nobody asks whether you watched the lecture — they ask what you shipped and what broke.
- Each project below produces three artifacts minimum: (1) a working system or reproducible study, (2) an evaluation with finance-aware metrics, (3) a written decision log explaining trade-offs. The third artifact is what separates senior engineers.
- The ladder is sequenced so that every level reuses the previous level's work. Your L1 bond toolkit becomes the pricing core of an L3 service; your L2 fraud model becomes the scoring engine of the L4 flagship.
- Difficulty is calibrated for an experienced AI engineer new to finance: L1 tests whether you learned the domain vocabulary; L3 tests whether you can operate software under financial-grade expectations (auditability, latency, degradation behavior).
- Time spent on projects is not linear in level: an L1 project is days, an L4 flagship is a season. Plan the calendar accordingly — one flagship completed beats four started, and the ladder's DoD discipline exists precisely to force closure.
- If you are converting from another ML domain, start at L2 with your strongest existing skill (NLP, tabular, streaming) applied to a finance dataset; drop to L1 only where the domain math (day counts, accruals, accounting identities) feels unfamiliar.
- Every finished project earns a line in your professional narrative: "I built X to decide Y, measured with Z, and here is what I would change." If you cannot say that sentence, the project is not done — regardless of code volume.

## 1.1 Working Vocabulary

| Term | Meaning in this repository |
|------|---------------------------|
| PRD | Product Requirements Document: the one-pager that frames the decision before any code exists |
| Data contract | The schema + semantics + point-in-time rules for every input the project consumes |
| PIT (point-in-time) correctness | Features computed only from data knowable at decision time; the anti-leakage discipline |
| Eval plan | The pre-declared metrics, splits, baselines, and targets — written before results are seen |
| Swap-set analysis | Who gains and loses approvals/scores when a model changes; the governance lens on model replacement |
| Capacity | The operational constraint (analyst hours, alert budget) that turns precision into a business number |
| DoD | Definition of Done: the checklist a project must satisfy at its level |
| Defense | The recorded, questioned walkthrough required at L4 and formally at L6 |

## 2. The Six Levels

| Level | Name | Typical duration | Entry condition | The question it answers |
|-------|------|------------------|-----------------|------------------------|
| L1 | Foundational | 3-7 days each | Phase 00-04 in progress | "Do I actually understand the primitive?" |
| L2 | Applied | 1-2 weeks each | Corresponding phase completed | "Can I run an honest modeling study on real financial data?" |
| L3 | Production | 2-4 weeks each | One L2 done in the same vertical | "Can I operate this under financial-grade engineering expectations?" |
| L4 | Advanced / Flagship | 6-10 weeks each | Two or more L3 projects done | "Can I deliver a complete, governed system, not a component?" |
| L5 | Research | 4-8 weeks each | One L4 done; Phase 19 reading underway | "Can I read, reproduce, and extend frontier work critically?" |
| L6 | Industry-grade capstone | 8-12 weeks | One flagship substantially complete | "Would this survive an institution's validation and defense?" |

### L1 — Foundational

Small, sharply scoped builds that force fluency in financial primitives: time value of money, accounting identities, term structures, transaction semantics. No ML. Correctness is verified with tests against hand-computed values. The trap to avoid here is skipping them because they look trivial — the L3/L4 projects assume you can price a bond and balance a ledger without thinking, and every later review (yours or a validator's) leans on that fluency.

**Definition of Done**

- [ ] Core logic implemented as an importable library (not just a notebook), with unit tests covering edge cases (leap years, odd first periods, zero-coupon boundaries).
- [ ] Results validated against at least one independent source (hand calculation, published table, or reference implementation).
- [ ] README explains the financial concept, the API, and two realistic use cases in under 400 words.
- [ ] Code passes lint; repo has pinned dependencies and a one-command demo.

### L2 — Applied

End-to-end modeling projects on real public financial datasets. The deliverable is not "a model" but a modeling *study*: baselines, honest validation, error analysis, and a decision-oriented conclusion (threshold economics, segment views, failure modes). This is the level where finance-specific discipline first bites: point-in-time correctness, calibration over raw discrimination, and error costs that are wildly asymmetric.

**Definition of Done**

- [ ] Problem framed as a business decision, not a Kaggle metric, with an explicit cost/error analysis.
- [ ] Point-in-time and leakage audit performed and documented (what could the model have known at decision time?).
- [ ] At least one naive baseline and one domain-informed model compared on the same split protocol.
- [ ] Evaluation includes discrimination *and* calibration (or rank-metric + economics) with confidence intervals where feasible.
- [ ] 1-2 page writeup: data, method, results, limitations, what you would do in production.
- [ ] Error analysis by segment (time, amount, customer type), not just aggregate scores.

### L3 — Production

Your L2 work wrapped in system-shaped engineering: APIs, latency budgets, monitoring, feature parity, audit logs, graceful degradation. The model is now one component among several, and the system — not the notebook — is the deliverable. Expect roughly half your effort here to go into things a Kaggle notebook never contains: logging schemas, fallback paths, load tests, and the parity discipline that keeps training and serving honest.

**Definition of Done**

- [ ] Serves predictions/decisions through a versioned API with a latency budget stated and measured (p50/p95/p99).
- [ ] Train/serve feature parity demonstrated (shared transformation code or feature store); parity test exists in CI.
- [ ] Every request logged with inputs snapshot, model version, and output; logs queryable for review.
- [ ] Monitoring in place: data drift, performance proxy, and system health, with alert thresholds justified in the README.
- [ ] Documented failure behavior: what happens when the model or an upstream is down (fallback, rules, refuse-and-queue).
- [ ] Containerized, reproducible locally with one command; infrastructure code (even simple compose files) in the repo.

### L4 — Advanced / Flagship

Seven full-scale systems spanning the discipline: fraud, credit, AML, documents, RAG, agents, payments. Each is a 6-10 week build with a production architecture, a compliance lens, and an evaluation plan a risk committee would recognize. This is portfolio headline material — and the entry requirement for L6. Each flagship has its own spec file in [`flagship/`](flagship/) with problem framing, reference architecture, evaluation targets, and a weekly plan; read the spec end to end before committing.

**Definition of Done**

- [ ] All L3 criteria, plus: the flagship spec's evaluation plan executed with targets met or misses explained.
- [ ] Compliance narrative written (regulatory context, control design, human-in-the-loop boundaries) — see each spec's Sections 8-9.
- [ ] Architecture documented with diagrams matching what is actually deployed locally.
- [ ] A 10-minute recorded walkthrough defending design decisions to a hypothetical risk/architecture review board.
- [ ] Honest limitations section: what is simulated, what would break at real scale, what you would need from a real institution.

### L5 — Research

Reproductions and extensions of published work at the research frontier: deep hedging, GraphRAG over financial entities, differentially private synthetic data, meta-labeling. The deliverable is a faithful reproduction plus a documented extension and a critical assessment of the paper's claims. A null or negative result, honestly reported, is a passing outcome here; an unexamined positive result is not.

**Definition of Done**

- [ ] Reproduction runs from a pinned environment on synthetic/public data with a documented deviation log.
- [ ] Results compared against the paper's reported numbers with differences explained (data, seeds, tuning).
- [ ] At least one original extension (new dataset, ablation, or variant) with a conclusion — including null results.
- [ ] Written assessment: what the paper claims, what it actually shows, and what an industrial team should take from it.

### L6 — Industry-Grade Capstone

One or two flagships carried to full production polish — hardened, monitored, documented to an SR 11-7-style standard, load-tested, with disaster behavior defined — and defended in a structured capstone defense. This is the finish line of the curriculum. The defense is graded on judgment under questioning, not on demo polish: expect "what breaks first at 100x volume" and "justify this control to an examiner" and know your answers.

**Definition of Done**

- [ ] Chosen flagship(s) meet every L4 criterion plus: load/latency test results, chaos or failure-injection exercise, and a documented incident runbook.
- [ ] Model/system documentation pack: development doc, validation-style self-review, monitoring plan, and change-management log (see `../phases/18-capstones/README.md`).
- [ ] Security review pass: threat model, secrets handling, dependency audit, least-privilege defaults.
- [ ] Formal 30-minute capstone defense delivered to at least one external reviewer (peer, mentor, or community), with the recording and Q&A notes archived.
- [ ] Public portfolio page ties the whole system together end to end.

## 3. Master Project Index

Difficulty: ★ scale from the phase exemplars (★ = gentle, ★★★★★ = expert). Phases reference `../phases/<dir>/README.md`. All datasets are in the canonical list (`../datasets/README.md`).

How to read the table: the **Phases** column is the learning dependency (do those phases first, or concurrently); **Dataset(s)** names the canonical public data the project is calibrated on; **Core skills** is what the project proves; **Resume value** is the honest signal it sends — a sentence you can defend in an interview. Difficulty reflects the full Definition of Done, not the minimum demo.

| ID | Project | Level | Phases | Dataset(s) | Core skills | Difficulty | Resume value |
|----|---------|-------|--------|-----------|-------------|------------|--------------|
| P01 | Bond & TVM toolkit | L1 | 04 | none (synthetic schedules) | Time value of money, accrual conventions, unit testing | ★☆☆☆☆ | Quant fundamentals signal |
| P02 | Double-entry ledger simulator | L1 | 01 | none (event streams) | Accounting identity, invariants, property-based testing | ★☆☆☆☆ | Domain credibility |
| P03 | FRED yield-curve explorer | L1 | 03, 04, 09 | FRED | API ingestion, term structure, curve visualization | ★☆☆☆☆ | Data engineering + rates literacy |
| P04 | Transaction categorizer | L1 | 02, 11 | PaySim + synthetic statements | Rules + text classification, merchant normalization | ★☆☆☆☆ | Practical NLP on money flows |
| P05 | Application scorecard | L2 | 06 | German Credit, Taiwan Credit Default | WOE/IV, monotonic binning, cutoff economics | ★★☆☆☆ | Core credit-risk skill, interview staple |
| P06 | Card-fraud model on ULB | L2 | 07 | ULB Credit Card Fraud | Imbalance, PR-AUC, cost-sensitive thresholds | ★★☆☆☆ | Fraud analytics credibility |
| P07 | CFPB complaint classifier | L2 | 11 | CFPB Consumer Complaints | Multi-class text, product routing, narrative analysis | ★★☆☆☆ | RegTech + NLP signal |
| P08 | Pairs-trading backtest | L2 | 09, 10 | Kraken/Binance OHLCV | Cointegration, backtest hygiene, transaction costs | ★★☆☆☆ | Quant trading literacy |
| P09 | Earnings sentiment tracker | L2 | 11 | Financial PhraseBank, FiQA-2018 | Financial sentiment, evaluation against price events | ★★☆☆☆ | NLP + markets signal |
| P10 | Sanctions screener | L2 | 08, 16 | OFAC SDN list, GLEIF LEI data | Entity matching, fuzzy name screening, false-positive cost | ★★☆☆☆ | Compliance engineering signal |
| P11 | Real-time scoring service | L3 | 15, 17 | ULB Credit Card Fraud, IEEE-CIS Fraud | Latency budgets, online features, fallback design | ★★★☆☆ | Strong production signal |
| P12 | Credit decision API with reason codes | L3 | 06, 17 | Home Credit Default Risk | Decision APIs, SHAP-to-reasons mapping, audit logs | ★★★☆☆ | Regulated-ML credibility |
| P13 | Document extraction pipeline | L3 | 11 | SEC EDGAR / Financial Statement Data Sets | Layout-aware parsing, table extraction, QA sampling | ★★★☆☆ | Doc-AI engineering signal |
| P14 | RAG policy assistant | L3 | 13 | FRED publications, public regulatory texts | Chunking, hybrid retrieval, citations, eval harness | ★★★☆☆ | GenAI-in-finance signal |
| P15 | Streaming feature pipeline | L3 | 03, 15 | IEEE-CIS Fraud | Kafka/Flink, feature parity, watermarks | ★★★☆☆ | Streaming ML signal |
| P16 | MLOps scorecard system | L3 | 17 | any L2 dataset | Tracking, registry, CI/CD, reproducibility | ★★★☆☆ | MLOps maturity signal |
| P17 | Volatility forecasting service | L3 | 09, 10 | Kraken/Binance OHLCV | GARCH baselines, forecast service, backtesting | ★★★☆☆ | Markets + production signal |
| P18 | [Real-time fraud detection platform](flagship/01-real-time-fraud-detection-platform.md) | L4 | 07, 15, 17 | IEEE-CIS Fraud, ULB Credit Card Fraud | Streaming features, hybrid scoring, case feedback | ★★★★☆ | Flagship — headline project |
| P19 | [Credit risk decisioning system](flagship/02-credit-risk-decisioning-system.md) | L4 | 06, 16, 17 | LendingClub, Home Credit, FICO HELOC | PIT features, calibrated PD, policy engine, reason codes | ★★★★☆ | Flagship — headline project |
| P20 | [AML transaction monitoring platform](flagship/03-aml-transaction-monitoring-platform.md) | L4 | 08, 14 | IBM Synthetic AML, Elliptic Bitcoin | Scenario engine, graph analytics, GNN, SAR drafting | ★★★★☆ | Flagship — headline project |
| P21 | [Financial document intelligence platform](flagship/04-financial-document-intelligence-platform.md) | L4 | 11, 12 | SEC EDGAR, Financial PhraseBank, FinQA | Layout extraction, LLM summarization with citations | ★★★★☆ | Flagship — headline project |
| P22 | [Financial RAG research assistant](flagship/05-financial-rag-research-assistant.md) | L4 | 12, 13 | SEC EDGAR, public regulatory corpora | Hybrid retrieval, reranking, entity graph, eval harness | ★★★★☆ | Flagship — headline project |
| P23 | [Agentic compliance operations platform](flagship/06-agentic-compliance-operations-platform.md) | L4 | 14, 16 | IBM Synthetic AML, OFAC SDN list | Agent workflows, approval gates, trajectory evals | ★★★★☆ | Flagship — headline project |
| P24 | [Payment intelligence engine](flagship/07-payment-intelligence-engine.md) | L4 | 02, 07, 15 | IEEE-CIS Fraud + synthetic payment streams | Routing economics, auth-time signals, chargeback prediction | ★★★★☆ | Flagship — headline project |
| P25 | Deep-hedging reproduction | L5 | 10, 19 | synthetic markets (GBM/Heston) | RL/free-boundary hedging, simulation fidelity | ★★★★★ | Research literacy |
| P26 | GraphRAG entity QA | L5 | 13, 19 | SEC EDGAR, GLEIF LEI data | Entity canonicalization, graph + vector retrieval | ★★★★☆ | Research literacy |
| P27 | DP synthetic transaction generator | L5 | 16, 19 | PaySim, IBM Synthetic AML | Differential privacy, utility/privacy trade-off measurement | ★★★★★ | Research literacy |
| P28 | Meta-labeling study | L5 | 10, 19 | Kraken/Binance OHLCV | Primary signal + size model, purged CV, deflated Sharpe | ★★★★☆ | Research literacy |

**L6 — Industry-grade capstone.** Select one or two of P18-P24 and carry them through the L6 Definition of Done above, then register and defend the capstone per `../phases/18-capstones/README.md`. Recommended pairings: P19 + P18 (a lending-and-fraud shop), P20 + P23 (a financial-crime stack), P21 + P22 (a research-and-documents platform).

### Flagship Selection Guide (L4)

- Choose by target vertical, not by novelty: fraud/payments → P18 or P24; credit/risk → P19; financial crime → P20 or P23; documents/research → P21 or P22.
- Choose by skill gap as a tiebreaker: the streaming flagships (P18, P24) force distributed-systems growth; the governance-heavy flagships (P19, P23) force documentation and control-design growth; the NLP flagships (P21, P22) force evaluation-harness growth.
- Do not run two flagships concurrently. The DoD requires a recorded defense — attention split across two systems produces two almost-finished portfolios.
- Each flagship spec lists its phase prerequisites; treat unmet prerequisites as scope risk, not as optional reading.

### Capstone Registration (L6)

1. Pick the flagship(s) and write a one-page capstone charter: scope, L6 gap list vs current state, defense date.
2. Work the gap list against the L6 Definition of Done; keep a change-management log from day one — it is itself a reviewed artifact.
3. Schedule the defense before you feel ready (8-week lead time); a defense is a production event and needs rehearsal like one.
4. After the defense, file the Q&A notes as known-issues — an L6 capstone with an honest open-issues list outranks a suspiciously clean one.

### A Complete Ladder Walk (worked example)

To make the reuse pattern concrete, here is the fraud/payments path a learner actually walks:

1. **P06 (L2)** — ULB fraud model: you learn imbalance, PR-AUC, cost-sensitive thresholds, and time-split discipline. Artifacts: EVAL.md, threshold memo.
2. **P11 (L3)** — the same model behind a FastAPI service with a latency budget, decision logging, and a fallback path. Artifacts: load-test numbers, audit-log schema.
3. **P15 (L3)** — the same features rebuilt as a streaming pipeline with parity tests. Artifacts: parity test in CI, watermark handling.
4. **P18 (L4)** — Flagship 01: the model, the service, and the streaming features become one platform with case management and drift monitoring. Roughly 70% of the flagship is assembled from work you already trust.
5. **L6** — hardening that platform into the capstone. This is why the ladder exists: no stage starts from zero.

## 4. Project Lifecycle

Every project above L1 follows the same six-stage lifecycle. The artifacts are mandatory; skipping them is how portfolios end up as notebooks nobody trusts.

| Stage | What you do | Output artifact |
|-------|-------------|-----------------|
| 1. PRD | Write the business problem, users, decisions, success metrics, and explicit non-goals before any code | `docs/PRD.md` (1 page) |
| 2. Data contract | Define every input: schema, units, point-in-time semantics, update frequency, known quality issues, and what you will *not* use and why | `docs/data_contract.md` |
| 3. Build | Implement in thin vertical slices (data → baseline → one model → one serving path), committing with tests; keep the baseline alive forever | repo + passing CI |
| 4. Eval | Run the evaluation plan: baselines, calibration, segment errors, cost sensitivity, fairness/regulatory checks where applicable | `docs/EVAL.md` with tables |
| 5. Deploy | Package, serve (or schedule), monitor; write the fallback and rollback behavior; log decisions for review | deployment config + monitoring dashboard |
| 6. Writeup | Close the loop: results vs targets, limitations, what broke, what you would change; 10-minute recorded walkthrough for L4+ | `README.md` + recording |

Two rules apply across all stages. First, the writeup is part of the project, not a chore after it — an undocumented system is a failed system. Second, every claim in the writeup must be traceable to an artifact (a table, a plot, a log) in the repo; hedge or delete anything you cannot trace.

Per-stage quality gates (a project that fails a gate is not at the next stage, however much code exists):

- **PRD gate:** someone unfamiliar with the project can state the decision it supports after reading only the PRD.
- **Data contract gate:** every column has an owner, a semantic type, and a point-in-time rule; the list of deliberately excluded fields (and why) exists.
- **Build gate:** the naive baseline runs in CI; the repository is one command from a working demo at every commit on main.
- **Eval gate:** every headline number has a table, a seed, and a split protocol behind it; the calibration/segment analysis exists for anything decisioning.
- **Deploy gate:** the fallback behavior has been demonstrated (not just described); the decision/audit log is queryable.
- **Writeup gate:** limitations section is specific enough that a hostile reviewer could not add anything material.

## 5. How to Choose Your Next Project

- **Default path:** complete at least one project per level before skipping ahead. L1 → L2 typically takes 2-4 weeks each; L3 3-4 weeks; L4 6-10 weeks.
- **Goal routing:** targeting risk/credit roles → P05, P12, P19 first. Fraud/payments roles → P06, P11, P18, P24. Financial-crime/compliance roles → P10, P20, P23. Doc-AI/LLM roles → P13, P14, P21, P22. Quant/markets roles → P08, P17, P25, P28.
- **Specialization rule:** after L2, pick one vertical (risk, crime, markets, documents/payments) and take it deep through L4; breadth is recovered by the case studies, depth by the flagship.
- **Energy rule:** if stuck at a level for more than three weeks, ship a smaller slice rather than expanding scope — a working narrow system outranks an abandoned broad one.
- **Sequencing note:** P15 (streaming features) is the hardest L3 project and directly unblocks P18 and P24; P12 directly unblocks P19; P14 unblocks P22.

### Project-to-Role Mapping (summary)

| Target role | Priority projects | Interview story they give you |
|-------------|-------------------|-------------------------------|
| Credit-risk ML engineer | P05, P12, P19 | Cutoff economics, reason codes, validation docs |
| Fraud / payments ML engineer | P06, P11, P18, P24 | Latency budgets, precision-at-capacity, routing economics |
| Financial-crime technology | P10, P20, P23 | Graph analytics, tuning governance, agent controls |
| Doc-AI / GenAI engineer (finance) | P13, P14, P21, P22 | Numeric fidelity, citation enforcement, eval harnesses |
| Quant / markets engineer | P08, P17, P25, P28 | Backtest hygiene, forecasting services, research literacy |
| Platform / MLOps in FinTech | P16, P15, P11 + any flagship L6 polish | Reproducibility, streaming, deployment discipline |

## 6. Portfolio Presentation Standards

- **Repository anatomy:** `README.md` (problem, demo, results, run instructions), `docs/` (PRD, data contract, EVAL), `src/`, `tests/`, `infra/` or `docker-compose.yml`, `notebooks/` for exploration only.
- **Lead with the decision, not the model:** the README's first paragraph states the business decision the system supports and the metric that proves it works.
- **Honest metrics:** report confidence intervals or at least multiple seeds; never report a test-set number you selected on; label every simulated/synthetic component clearly.
- **Claim hygiene:** hedge external claims ("publicly reported", "industry estimates"); link to the dataset's canonical source; never present public data as if it were production data from an institution.
- **Demo:** every L3+ project has something runnable in under five minutes (script, compose stack, or hosted demo) plus screenshots or a short GIF in the README.
- **Diagrams that match reality:** architecture diagrams depict what is deployed locally, not what a real bank would deploy — state the gap explicitly.
- **Ethics and compliance section:** for anything decisioning (credit, fraud, AML), include the fairness/regulatory considerations and where a human stays in the loop.
- **No fabricated production claims:** never write "deployed at a bank" or invented scale numbers. "Reproduced on public data" is a strength, not an apology.

### Review Rubric — How a Project Is Judged

Score each dimension 0-4. A project ready for the portfolio scores 3+ everywhere; flagships and L6 capstones must score 4 on Governance and Evaluation.

| Dimension | 0 (absent) | 2 (competent) | 4 (exemplary) |
|-----------|------------|---------------|----------------|
| Problem framing | Metric-first, decision unclear | Decision stated, users named | Decision + error economics + non-goals explicit |
| Data discipline | No PIT/leakage thought | PIT audit mentioned | PIT audit + data contract + labeled limitations |
| Evaluation | Single aggregate metric | Baselines + split protocol | Cost-sensitive, calibrated, segmented, reproducible |
| Engineering | Notebook only | Runs locally, some tests | Tested, containerized, monitored, fallback demonstrated |
| Governance | None | Compliance section present | Regulatory context + controls + human-in-loop boundaries designed and tested |
| Writeup | Results dump | Clear narrative with results | Traceable claims, honest limitations, recorded defense (L4+) |

### Common Failure Modes

- **Notebook sprawl:** the project lives in 14 notebooks with no importable core; nobody, including you, can re-run it in three months. Fix: thin slice of library code plus exploratory notebooks that are clearly marked as scratch.
- **Metric theater:** AUC/accuracy headlines with no decision economics attached. Fix: every metric must answer "so what does the business do differently?"
- **Silent leakage:** future-dated or target-adjacent fields inflate results; discovered in interview, not in review. Fix: the PIT audit is mandatory from L2 up.
- **Simulated everything:** a demo that only ever runs on trivially small synthetic data and breaks on real schema variety. Fix: at least one real public dataset in the loop from L2 up.
- **Compliance as decoration:** a last-paragraph disclaimer instead of designed controls (gates, audit logs, fallbacks). Fix: controls appear in the architecture diagram, in the code, and in the tests.
- **Scope gravity:** endless feature work, no evaluation or writeup; the project never reaches "Done". Fix: the lifecycle gates — ship small, close the loop, then extend.

### Data & Licensing Standards

- Use the canonical dataset list in `../datasets/README.md`; note license and access conditions (some, like Freddie Mac/Fannie Mae samples, require free registration) in the data contract.
- Cache raw data outside the repo; commit schema, hashes, and a download script instead of the data itself.
- Every dataset reference states the version/date accessed — financial datasets change silently, and your results must remain reconstructible.
- Synthetic data generators you build are first-class code: versioned, tested, and documented with the same rigor as models, because everything downstream inherits their assumptions.

## 7. Logging Progress

Record every completed project in `/PROGRESS.md` (project ID, level, completion date, link, one-line evidence summary) and store recordings, dashboards, and review notes under `/notes/artifacts/`. The capstone checkpoint in Phase 20 audits this log — evidence you cannot produce is evidence you do not have.

Minimum evidence set per completed project:

| Evidence | L1 | L2 | L3 | L4 | L6 |
|----------|----|----|----|----|----|
| Repo link with passing CI | required | required | required | required | required |
| EVAL.md with reproducible numbers | — | required | required | required | required |
| Recorded walkthrough | — | — | optional | required | required |
| Governance/compliance narrative | — | — | optional | required | required |
| External review notes | — | recommended | recommended | required | required |
| Defense recording + Q&A log | — | — | — | — | required |

## 8. Getting Projects Reviewed

Projects reviewed only by their author calcify. Build review into the loop:

- **Who reviews:** a peer at or above your level on the ladder, a mentor, or a domain-focused community (credit-risk, fraud, quant communities all run project-feedback threads).
- **What they review:** the writeup and the EVAL.md first, code second. Reviews that start in the code miss the framing errors that matter most.
- **How to ask:** one paragraph of context (which level, which DoD items you believe are met), the rubric table with your self-scores, and two specific questions you want challenged.
- **How to receive:** log every review point and your disposition (accepted / rejected with reason / deferred) in the repo — the disposition log is itself evidence of engineering judgment, and Phase 20's capstone defense will expect exactly that habit.
- **Cadence for flagships:** review at the PRD stage, at the eval stage, and before the recorded defense. Three reviews minimum; more is fine.
