# Interview Preparation — The Five-Track System

> Deliverable L. FinTech AI interviews test five things; this file drills all five. Self-grade monthly in [`PROGRESS.md`](../PROGRESS.md) §7. Answers here are sketches — expand them in your own words (that expansion is the study).

---

## Track Map

| Track | What's tested | Sections below |
|---|---|---|
| A. AI/ML core | Evaluation, imbalance, calibration, drift, modern stack | A1-A12 |
| B. FinTech domain | Credit, fraud, AML, payments, markets literacy | B1-B12 |
| C. ML system design | One design problem, 45 min | → [`system-design/`](../system-design/README.md) |
| D. Coding | Data-heavy Python/SQL + ML implementation | D1-D8 |
| E. Product & senior behavioral | Judgment, tradeoffs, stakeholder fluency | E1-E8 |

**30-day sprint plan** at the bottom.

---

## Track A — AI/ML Core (FinTech flavor)

**A1. Why is accuracy a terrible metric for fraud?**
Fraud is rare (~0.1-1%). A model predicting "never fraud" is 99%+ accurate and worthless. The operating metrics are PR-AUC for ranking quality, alert precision at analyst capacity, and expected profit with explicit FP/FN costs.

**A2. Your model has great AUC but poor calibration. What breaks?**
Everything that consumes probabilities: risk-based pricing, expected-loss provisioning, alert prioritization. AUC is rank-only. Fix with isotonic/Platt on a held-out fold; verify with reliability curves and Brier; watch for resampling-induced bias (Dal Pozzolo 2015).

**A3. How do you handle extreme class imbalance without lying to yourself?**
Options: class weights, focal loss, targeted resampling, threshold tuning, anomaly hybrid. The trap: resampling shifts the probability scale — recalibrate after. Never evaluate with accuracy; report prevalence alongside every metric.

**A4. Explain data drift vs concept drift and what you monitor for each.**
Data drift: input distribution shifts (new customer segment, new merchant category) — monitor PSI/KS on features and scores. Concept drift: the input→label relationship shifts (macro shock changes default behavior) — needs outcome data, which arrives late, so use proxy alarms (score distribution, acceptance patterns) plus business KPIs (loss rate, approval rate).

**A5. Labels arrive 30-90 days late. How do you evaluate a live fraud model?**
Match on confirmation-date cohorts; report metrics with maturity curves; use proxy short-term signals (chargeback intent, manual review outcomes) knowing they're biased toward easy cases; never compare "fresh" vs "mature" cohorts directly.

**A6. Why is point-in-time correctness the first thing to check in any financial ML pipeline?**
Features computed with post-decision information (future balances, later bureau pulls) leak — models look brilliant in test and collapse in production, and the error is undetectable from metrics alone. Audit every feature's as-of semantics before modeling.

**A7. When would you choose Isolation Forest over a supervised model for fraud?**
Cold-start (no labels yet), novel attack discovery, a second opinion layer for supervised systems. It's a complement: supervised catches known patterns; anomaly catches unknown-unknowns at the cost of higher false positives.

**A8. How do you explain a boosting model to a regulator?**
Three layers: global (feature importance with stability across folds), local (per-decision SHAP mapped to plain-language reasons), and behavioral (monotonicity constraints, sensitivity tests, counterfactuals). The deliverable is documentation, not a chart.

**A9. What's wrong with random K-fold cross-validation in financial time series?**
Overlapping label windows and autocorrelation leak future information; also entity overlap (same customer in train and test). Use purged K-fold with embargo (López de Prado) and group-by-entity splits.

**A10. RAG vs fine-tuning for a finance assistant?**
RAG for knowledge (fresh, citable, auditable, entitlement-aware); fine-tuning for narrow stable behaviors (format, tone, classification). Most finance copilots: RAG + prompting; fine-tune only with an eval that proves the delta and survives model refreshes.

**A11. How do you evaluate an LLM system rigorously?**
Golden set (50-200 items, expert rubrics), task metrics (extraction F1, citation validity, numeric-fidelity rate), LLM-as-judge *calibrated against human labels* (κ), bootstrap CIs, and regression gates in CI. Vibes are not evals.

**A12. A model degraded in production. Walk me through your diagnosis.**
Decompose: population drift (PSI) vs concept drift (labels) vs data breakage (upstream schema, feature-parity bugs) vs policy changes (new segments). Check the audit store before the model — most "model" incidents are data or policy incidents.

---

## Track B — FinTech Domain

**B1. Walk me through a card payment end-to-end.**
Checkout → auth request → merchant acquirer → network → issuer (risk checks: velocity, device, 3DS) → auth response → capture → clearing → settlement (interchange flows issuer-ward, fees to network/acquirer) → statement/dispute window (chargebacks). Know who holds risk at each step.

**B2. What happens on a bank's balance sheet when it issues a loan?**
Asset: loan receivable; liability: new deposit (money creation). Income: interest accrual; expense: expected credit loss provision. Capital: risk-weighted assets increase. Regulators care because leverage multiplies everything.

**B3. Explain PD, LGD, EAD and how they produce price.**
PD: default probability over a horizon; LGD: loss severity after recovery; EAD: exposure at default. EL = PD×LGD×EAD. Price ≈ EL + funding + opex + capital charge + margin. Miscalibrated PD misprices silently at scale.

**B4. What is reject inference and why does it matter?**
Outcomes exist only for approved applicants; the model is trained on a policy-filtered sample. Ignoring it bakes yesterday's policy into tomorrow's model. Methods: augmentation, reweighting, extrapolation, experiments — all assumptions; the senior answer names its assumption.

**B5. Design the features for card-fraud detection.**
Velocity (counts/amounts per window per card/device/merchant), device/session fingerprints, merchant category risk, geo-velocity, behavioral history (typical amounts/times), network features (shared devices/merchants), and post-auth confirmation feedback. Note label latency and adversarial drift.

**B6. Why do AML systems have terrible alert precision, and what would you do?**
Rules tuned for recall to satisfy examiners; typologies are deliberately obscured; thresholds set years ago. Improvements: entity resolution first, graph features, capacity-aware alert ranking, scenario backtesting, better feedback loops from investigations. All changes must survive validation.

**B7. What does KYC mean operationally, and where does ML fit?**
Identity verification, sanctions/PEP screening, risk rating, ongoing CDD/EDD. ML fits: document verification, name-screening (fuzzy matching + ranking), adverse-media screening, behavioral anomaly detection for EDD triggers, case triage. Humans own final risk decisions.

**B8. Explain the yield curve to a product manager and why it matters for a bank.**
Rates across maturities; normally upward (term premium). Banks borrow short, lend long — inversion compresses net interest margins and historically precedes recessions (credit-cycle signal for risk models).

**B9. What is VaR? Its flaws?**
Loss threshold at confidence level over a horizon (e.g., 99% 10-day). Flaws: says nothing about tail beyond the threshold (CVaR does), unstable under fat tails, can be gamed, needs backtesting (Kupiec). Banks layer stress testing because VaR is backward-looking.

**B10. What changed with real-time payments (FedNow/RTP, PIX, UPI)?**
Irrevocable instant settlement: fraud decisioning moves to pre-auth with milliseconds budget; recall windows shrink; authorized-push-payment scams become the dominant fraud type (hence UK's 2024 reimbursement rules). Risk moves from banks' schedules to real-time architecture.

**B11. How does IFRS 9 / CECL change what a credit model must output?**
Not just a score: staged, forward-looking, lifetime expected losses. Models must support SICR triggers, macro-scenario conditioning, and lifetime PD curves — documentation and scenario governance become part of the model deliverable.

**B12. Where does GenAI genuinely belong in a bank today?**
Document-heavy expert leverage: research/analyst copilots with citations, compliance drafting with human sign-off, service automation with escalation, code assistants. Not: autonomous advice, unattended credit decisions, anything without audit trails.

---

## Track C — ML System Design
Covered fully in [`system-design/README.md`](../system-design/README.md) — six problems with the 12-part skeleton. The graded dimensions: requirements elicitation, latency budget realism, point-in-time discipline, failure modes, compliance integration, capacity math, named trade-offs.

---

## Track D — Coding (data-heavy patterns)

**D1.** Implement WOE/IV binning with monotonicity enforcement (pandas/numpy; no optbinning). *Tests: information-value ordering, monotonic bins, missing-value bin.*
**D2.** Implement a purged K-fold splitter with embargo for overlapping labels. *Tests: no train label overlaps test period; gap respected.*
**D3.** Compute PSI between two distributions + alert thresholds. *Tests: identical → ~0; shifted → high; handle zero-buckets.*
**D4.** Write an idempotent payment-processing function (dedupe by idempotency key; safe under concurrent retries). *Tests: double-call same result; concurrent calls with a lock or upsert.*
**D5.** SQL: monthly active users + 30-day retention cohort table (window functions, date spines). *Variant: ATM cash-out series per branch.*
**D6.** Implement Isolation Forest scoring path from scratch (simplified: random trees, path length) OR implement the profit-curve/threshold-sweep with a cost matrix. *Tests: sanity on synthetic data.*
**D7.** Build a mini RAG eval: chunk a 10-K, embed (any model), compute hit@k and MRR on 20 authored questions; implement the numeric-fidelity checker (regex-extract figures from answer + source, diff).
**D8.** Streaming: implement a windowed velocity counter (debits per card per 10-min sliding window) with event-time handling and a late-event policy.

## Track E — Product & Senior Behavioral

**E1. "Tell me about a model you shipped that failed. What did you do?"** — Use a postmortem story: detection → hypothesis → data audit → fix → guardrail added. Show the runbook instinct.
**E2. "The business wants higher approval rates. Risk says no. You own the model — what do you do?"** — Quantify the tradeoff (profit curve, loss-rate guardrail), propose a bounded experiment (segment-limited champion/challenger), document decision rights. Never "I just built what was asked."
**E3. "How do you work with validators/compliance who slow you down?"** — Reframe: they are users of your documentation. Show the MDD habit, early design reviews, and gate-cycle-time optimization.
**E4. "A vendor claims 90% fraud-loss reduction. The CFO wants to buy. Your take?"** — Demand the denominator, population, time window, and holdout. Offer a pilot with your own profit metric. Vendor claims are hypotheses (see case-study #10).
**E5. "Design an experiment to launch a new credit policy."** — Shadow score → segment-level pilot with guardrails (loss rate cap) → sequential testing → swap-set analysis → rollout with monitoring plan.
**E6. "What would you automate with agents at this company, and what would you refuse?"** — Reconciliation/reporting/triage = yes (audit trail + HITL). Advice, payment execution, sanctions final decisions = no. The refusal is the senior signal.
**E7. "How do you keep your models fair without a legal team holding your hand?"** — Testing regime (metrics + proxies + error gaps), documentation, mitigation tradeoffs in writing, and knowing when to escalate. Name the metrics and their conflicts.
**E8. "Where do you want to be in 3 years?"** — Answer with the architect path: systems owned, governance fluency, mentoring. Tie to artifacts you can show (this repository is that answer).

---

## The Case Script (for take-homes and live cases)

```text
1. Restate the business decision and its error costs (money, not metrics)
2. Data audit: point-in-time violations, label latency, prevalence, bias risks
3. Baseline first (scorecard/logistic/naive) — beat it honestly
4. Finance-aware eval: calibration + cost curve + stability + fairness gaps
5. Production plan: serving mode, monitoring, fallback, audit
6. Risks & limitations: say what would make you wrong
```

## 30-Day Sprint Plan

| Week | Focus |
|---|---|
| 1 | Track A daily (4 questions/day, answers written aloud) + one system-design problem + D1-D2 |
| 2 | Track B daily + second design problem + D3-D5; record 2 oral answers |
| 3 | Track D timed (45-min blocks) + Track E stories written from your postmortems + third design problem |
| 4 | Mock loop: 1 system design + 1 domain deep-dive + 1 behavioral, recorded; gap-list → targeted redo |

Daily anchor: 1 Anki cycle + 1 recorded answer. The recording habit is what converts knowledge into interview performance.
