# Phase 07 — Fraud Detection & Payment Intelligence

> **Stage III — Core Financial ML** · **Duration: 5 weeks** · **Mastery target: Application → Production**
> **Position in path:** `06-credit-risk` ← **this phase** → `08-aml-financial-crime`

## 1. Objective

Fraud detection is adversarial, imbalanced, latency-bound machine learning where the metric that matters is money saved net of customer friction. In this phase you master the full transaction-intelligence stack: fraud typologies, cost-based evaluation, imbalance handling without breaking calibration, velocity and behavioral feature engineering, anomaly detection, sequence models over transaction streams, real-time scoring architecture, and rules+ML hybrid decisioning. You finish able to design an auth-time scoring system that survives fraudsters' adaptation, a sub-100ms latency budget, and a compliance review.

## 2. Why It Matters in Finance

Fraud is the sharpest optimization problem in applied ML: a rarity measured in fractions of a percent, adversaries that respond to your model within days, labels that arrive weeks late, and a decision that must be returned before the customer's card is done dipping. Instant-payment rails make it harsher — money that moves in seconds cannot be recalled — and the UK's APP-reimbursement regime (effective October 2024) moved scam liability onto payment providers, turning detection quality into a direct P&L line.

- Every false positive is a declined legitimate customer: friction compounds into abandonment and churn, so the real objective is bi-objective — losses down, friction down.
- Synthetic identity fraud shows how blind conventional anomaly thinking is: industry estimates cited by the Boston Fed (2025) put US synthetic-identity losses in the tens of billions annually — an identity that behaves perfectly for years is the hardest label problem in the field.
- Accuracy is meaningless at fraud prevalence; the operating metrics are precision@capacity, expected cost, and fraud-$ saved net of friction — an AI engineer who optimizes these outperforms one who optimizes AUC.
- Fraud models decay adversarially: detection-rate fade after deployment is a security property to monitor, not an inconvenience to ignore.

## 3. Prerequisites

- [ ] Phase 02 — payment rails, authorization flow, chargebacks and disputes
- [ ] Phase 04 — Bayes/base rates, cost-sensitive decision theory
- [ ] Phase 06 — calibration, imbalanced learning discipline, explainability (sibling skill set)
- [ ] Existing ML skill: gradient boosting, evaluation metrics, SHAP (assumed known)

## 4. Learning Outcomes

- I can classify fraud typologies (card-not-present, account takeover, synthetic identity, first-party, authorized-push-payment scams, merchant/promo abuse) and name the signal class each demands.
- I can define a cost matrix for a payments business and optimize expected profit instead of accuracy.
- I can apply resampling, cost-sensitive learning, focal loss, and threshold moving while preserving calibration — and explain the Dal Pozzolo undersampling-calibration effect.
- I can evaluate at business level: PR-AUC, precision@capacity, and fraud-$ saved versus friction-cost curves.
- I can engineer velocity, device/session, merchant/category, and network features with point-in-time correctness.
- I can deploy Isolation Forest/LOF/autoencoder baselines and articulate when unsupervised beats supervised.
- I can build sequence models over transaction streams for ATO-style detection.
- I can design a sub-100ms scoring path with online features, p99 budgets, and rules fallback.
- I can design rules+ML hybrid decisioning with champion/challenger and override logging.
- I can reason about label latency, feedback loops, and investigator bias in training data.
- I can plan drift/adversarial monitoring and risk-based 3DS step-up decisioning.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 07.1 | Fraud taxonomy & threat landscape | CNP, ATO, synthetic ID, first-party, APP scams, abuse | typology-to-signal map |
| 07.2 | Economics of fraud decisions | Cost matrices, friction vs loss, why accuracy is meaningless | expected-cost decision model |
| 07.3 | Imbalance toolbox done right | Resampling & calibration, focal loss, cost-sensitive learning | calibration-preserving pipeline |
| 07.4 | Business-level evaluation | PR-AUC, precision@capacity, $ saved vs friction | evaluation framework + curves |
| 07.5 | Feature engineering for fraud | Velocity, device/session, behavioral, merchant, network features | feature library on IEEE-CIS |
| 07.6 | Anomaly detection & when it beats supervised | Isolation Forest, LOF, autoencoders, deep SVDD | unsupervised vs supervised study |
| 07.7 | Sequence models for transaction streams | LSTM/GRU/transformers over transaction histories | sequence anomaly model |
| 07.8 | Graph signals for fraud rings | Shared devices/cards/addresses; ring detection preview | graph feature starter (→ Phase 08) |
| 07.9 | Real-time scoring architecture | Online features, p99 budgets, model fallback to rules | latency-budgeted scoring design |
| 07.10 | Rules + ML hybrid decisioning | Expert rules, champion/challenger, overrides | hybrid decision prototype |
| 07.11 | Case management & label latency | Confirmed fraud arrives late; feedback loops & bias | label-latency simulation |
| 07.12 | Drift & adversarial adaptation | Fraudsters respond to your model; monitoring posture | drift playbook |
| 07.13 | 3DS/step-up authentication decisioning | Risk-based authentication, friction routing | step-up decision policy |
| 07.14 | Dispute & chargeback prediction | Chargeback forecasting, representment support | chargeback risk model |

**07.1 Fraud taxonomy & threat landscape.** Different frauds are different problems: card-not-present fraud is a transaction-ranking problem; account takeover is a behavioral-drift problem; synthetic identity is a longitudinal identity-graph problem; first-party fraud and APP scams are consent problems where the "victim" performs the payment (the UK's reimbursement regime formalizes this shift); merchant and promo abuse are policy-abuse problems. Each implies different features, labels, and latency profiles — build the typology-to-signal map before touching a model.

**07.2 The economics of fraud decisions.** Every decision trades two error types with different prices: a false negative costs the fraud amount plus fees and operations; a false positive costs the good customer's transaction, their patience, and possibly their loyalty. Encode these into a cost matrix (segmented by amount band and customer value) and optimize expected cost — this is Phase 04's Bayes decision theory with a P&L attached, and it is why accuracy is meaningless at 0.1% prevalence.

**07.3 Imbalance toolbox done right.** Resampling changes the prior, and Dal Pozzolo et al. (2015) showed undersampling shifts calibration — scores stop meaning what they say unless you re-weight or recalibrate. Cost-sensitive learning and threshold moving alter the decision, not the probability; focal loss reshapes training gradients. The discipline: pick one intervention deliberately, keep a calibration stage, and validate probabilities with reliability curves before anything consumes them as probabilities.

**07.4 Business-level evaluation.** PR-AUC conditions on the rare class and tracks the region alerts actually live in; precision@capacity reflects the truth that alert/investigation capacity is fixed; the profit curve (fraud-$ saved minus friction cost minus review cost, swept over thresholds) is the board-level artifact. Report all three plus calibration — ROC-AUC alone will flatter models no operations team can use.

**07.5 Feature engineering for fraud.** Velocity features (counts and amounts over 1h/24h/7d windows per card, device, merchant) capture rate anomalies; device and session fingerprints capture infrastructure reuse; behavioral signals (typing cadence, navigation patterns — behavioral biometrics as a concept) capture "not the usual human"; merchant/category and geographic features capture context drift; graph features (shared device/card/address degree) capture rings. Every feature must be point-in-time computable at auth time — the fraud twin of Phase 06's leakage discipline.

**07.6 Anomaly detection & when it beats supervised.** Isolation Forest, LOF, autoencoders, and (conceptually) deep SVDD model "normal" and flag deviations: they need no labels, catch novel patterns, and bootstrap cold-start segments — but they typically deliver worse precision than supervised models and produce scores that are not probabilities. The production pattern is complementary: supervised for the known, anomaly scores as features and as discovery/triage queues for new patterns.

**07.7 Sequence models for transaction streams.** Account takeover lives in sequence breaks — a purchase category, geography, or cadence that is individually normal but collectively wrong. LSTM/GRU or small transformers over the last-N transactions per card learn these dynamics and outperform flat features on ATO-style patterns, at the cost of latency and interpretability budget. Build one on card-level sequences and compare honestly against a velocity-feature GBM.

**07.8 Graph signals for fraud rings.** Fraud is organized: rings share devices, cards, addresses, and merchants. Per-transaction features cannot see fan-in/fan-out structure, but graph features (shared-neighbor counts, community membership, proximity to known-bad) expose it — a direct preview of Phase 08's full graph stack. Even lightweight graph features on top of a GBM produce meaningful lift on ring-type fraud.

**07.9 Real-time scoring architecture.** Auth-time scoring is a latency-budget problem: precomputed online features (streaming aggregates), a compact model (GBM or distilled NN), cache-friendly serving, and a rules fallback on timeout. The SLO is p99, not p50 — the tail is what declines cards at checkout. Design the full path on paper, then benchmark it; Phase 15 industrializes this with Kafka and stream processors.

**07.10 Rules + ML hybrid decisioning.** Rules encode policy and known-bad patterns (instant blocks, compliance hard-stops, velocity caps) and provide the always-available fallback; ML ranks the uncertain middle where most money and friction live. Champion/challenger deployment, override logging, and shadow testing keep the hybrid honest — overrides are also a data source: every analyst override is a label about where the model is wrong.

**07.11 Case management & label latency.** Confirmed fraud arrives weeks late via chargebacks and investigations, and only for cases someone looked at — labels are delayed AND selected, which biases training toward patterns investigators already find. Handle it explicitly: model label-arrival delay, reweight by confirmation likelihood, use chargebacks as a noisy proxy, and retrain on consistent-maturity vintages. Ignoring this silently trains the model to match the team's priors.

**07.12 Drift & adversarial adaptation.** Fraudsters probe, adapt, and share what works: expect detection-rate decay after deployment, new-pattern clusters among escapes, and shifting attack geography. Monitor score/feature drift and per-segment precision, maintain challenger models, and run internal red teams — the model is a security control with a threat model, not a static artifact.

**07.13 3DS/step-up authentication decisioning.** Risk-based authentication routes transactions among frictionless approval, 3DS challenge, step-up (OTP/biometric), and decline; the economics are explicit — a challenge costs conversion but caps liability (liability-shift rules under 3DS2), so the model's job is to spend friction only where it pays for itself. Build the policy layer as a cost-aware decision over model score, amount, and customer segment.

**07.14 Dispute & chargeback prediction.** Chargebacks arrive after settlement with fees, fines, and program-monitoring risk (network thresholds); predicting them supports representment evidence, merchant risk scoring, and proactive refunds that are sometimes cheaper than disputes. Note the label subtlety: chargeback ≠ fraud (friendly fraud, service disputes) — treat it as its own model with its own cost matrix.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Expected-cost decision theory | Bayes decision with an FP/FN cost matrix | Thresholds are economics, not metrics | You optimize accuracy while the business loses money |
| Base rates & posterior probability | Bayes under extreme imbalance | Alerts must be read as posteriors | Precision illusions at 0.1% prevalence |
| PR-AUC / precision@k geometry | Ranking quality on the rare class at operating points | Imbalance makes ROC-AUC optimistic | You oversell models alert teams cannot use |
| Calibration under resampling | Dal Pozzolo's undersampling intercept shift | Scores feed dollar decisions downstream | Resampled models misprice every decision |
| Point-process / velocity math | Poisson-style rate estimates over rolling windows | Velocity features are event-rate statistics | Leakage-prone, unstable velocity features |
| Sequence likelihood & embeddings | Models over ordered transaction histories | ATO lives in sequence breaks | You miss fraud per-transaction models cannot see |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| p99 latency budgets & serving paths | Auth-time decisions are latency-bound; the tail declines customers |
| Online/offline feature parity | Velocity windows must compute identically in training and at auth time |
| Streaming ingestion (Kafka preview) | Online features come from streams; full stack arrives in Phase 15 |
| Shadow mode & champion/challenger | Candidate models prove themselves on live traffic before they decide |
| Override & decision audit logging | Every decision, input snapshot, model/rules version, and override retained |
| Rules engine integration | Policy DSL, versioning, instant-block semantics alongside ML ranking |
| Case management integration | Alerts route to investigators; dispositions return as (delayed) labels |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| XGBoost / LightGBM | Primary supervised fraud models; latency-friendly at serving |
| imbalanced-learn | Resampling experiments (with explicit recalibration afterwards) |
| scikit-learn | Isolation Forest, metrics, calibration utilities |
| PyOD | Broader anomaly-detection model zoo (LOF, autoencoders, more) |
| River | Incremental/online ML experiments for streaming-shaped features |
| Kafka (preview) | Streaming backbone for online features; deep dive in Phase 15 |
| SHAP | Alert explanations for analysts and model debugging |
| DuckDB / pandas | Fast experimentation over the fraud datasets |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| ULB Credit Card Fraud dataset + Dal Pozzolo et al. (2015), "Calibrating Probability with Undersampling for Unbalanced Classification" (IEEE IJCNN) | Dataset+Paper | Intermediate | Imbalance & calibration | The canonical dataset and the paper explaining its most common pitfall | Essential |
| IEEE-CIS Fraud Detection dataset (Kaggle/Vesta, 2019) | Dataset | Intermediate | Feature engineering | Realistic-scale fraud competition data with identity/device signals | Essential |
| UK Payment Systems Regulator, APP fraud reimbursement policy (PS25/5; effective Oct 2024, £85k cap) | Regulation | All | APP scams | The policy shift making scam detection a provider P&L obligation | Essential |
| Federal Reserve synthetic identity fraud whitepapers (2019 and updates) | Report | Intermediate | Synthetic identity | The definitional and measurement foundation for the hardest fraud type | Recommended |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Baesens, Van Vlasselaer & Verbeke, *Fraud Analytics Using Descriptive, Predictive, and Social Network Techniques* (Wiley 2015) | Book | Intermediate | Fraud analytics | The field's standard textbook, including social-network methods | Essential |
| Bolton & Hand (2002), "Statistical Fraud Detection: A Review" (Statistical Science) | Paper | Advanced | Foundations | The review that frames supervision, asymmetry, and misuse detection | Recommended |
| Chandola, Banerjee & Kumar (2009), "Anomaly Detection: A Survey" (ACM Computing Surveys) | Paper | Advanced | Anomaly detection | The taxonomy behind every anomaly algorithm you will meet | Recommended |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Stripe Radar public engineering content | Blog | Intermediate | ML fraud ops | A rare transparent window into production fraud ML at scale | Recommended |
| Visa / Mastercard public risk & rules resources | Docs | Intermediate | Network programs | How the networks define liability, 3DS, and dispute rules | Reference |
| Revolut / N26 public engineering posts on fraud & risk | Blog | Intermediate | Neobank operations | Practitioner accounts of ATO waves and scam pressures | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Kaggle IEEE-CIS solution write-ups | Notebooks | Intermediate | Feature craft | The public record of what actually moved the metric | Optional |

## 10. Practical Exercises

1. - [ ] Baseline GBM on the ULB card-fraud dataset; report ROC-AUC and PR-AUC side by side; write down why they disagree.
2. - [ ] Undersampling experiment: reproduce the calibration breakage, apply the Dal Pozzolo re-weighting correction, and plot reliability curves before/after.
3. - [ ] Cost matrix on IEEE-CIS: assign FP cost (customer friction + review) and FN cost (fraud $) per amount band; sweep thresholds; plot the expected-profit curve and pick the operating point.
4. - [ ] Precision@capacity: fix an alert budget (top 0.1% of transactions/day) and compare models by precision at that budget instead of AUC.
5. - [ ] Velocity features on PaySim (or a synthetic generator): 1h/24h/7d rolling counts per card and merchant with point-in-time correctness; then build the naive leaky version and quantify the AUC gap.
6. - [ ] Isolation Forest + LOF vs supervised GBM on ULB: identify the region (novel-pattern simulation) where unsupervised wins; where it never does.
7. - [ ] Sequence model: GRU over last-20-transactions per card (PaySim/IEEE-CIS-derived sequences); compare against the velocity-feature GBM at matched capacity.
8. - [ ] Graph features on seeded ring data: shared-device degree and community-membership features; measure ring-detection lift over per-transaction features.
9. - [ ] Latency-budget worksheet: decompose a 100ms auth-time budget (feature fetch, inference, rules, logging, fallback); benchmark a mock scoring path and report p50/p99.
10. - [ ] Drift simulation: inject a new fraud pattern after training; measure per-segment detection decay over weeks and define the retraining trigger you would automate.

## 11. Mini Projects

**M1 — Cost-sensitive fraud model on IEEE-CIS.** Custom expected-profit metric from a documented cost matrix; threshold sweep; champion/challenger comparison vs an AUC-optimized model. Deliverable: model + profit-curve report. Difficulty: ★★★☆☆.

**M2 — ULB card fraud with PR-AUC + calibration + alert-capacity analysis.** Resampling with correction, isotonic calibration, precision@capacity table, and a one-page "what this model does and does not claim" memo. Deliverable: notebook + memo. Difficulty: ★★☆☆☆.

**M3 — Velocity feature pipeline on a synthetic transaction stream.** PaySim plus a generator for point-in-time velocity, device, and merchant features; leakage A/B (naive vs correct); feature-refresh design for online serving. Deliverable: feature library + leakage study. Difficulty: ★★★☆☆.

**M4 — Sequence-based anomaly model on transaction sequences.** GRU (or small transformer) over card-level transaction sequences; ATO-style anomaly detection; comparison vs flat features; false-alarm analysis at fixed capacity. Deliverable: model + comparison report. Difficulty: ★★★★☆.

**M5 — Rules-engine + ML hybrid prototype with override logging.** Simulated alert stream; rule layer (policy/known-bad) + ML ranking layer; override interface with logging; weekly "override review" report showing where the model is wrong. Deliverable: prototype + override log analysis. Difficulty: ★★★☆☆.

## 12. Major Project Hook

This phase supplies the modeling brain of **Flagship Project 1 — Real-Time Fraud Detection Platform** and **Flagship Project 7 — Payment Intelligence Engine** (`/projects/flagship/`; see `/projects/flagship/README.md`): Phase 15 adds the streaming spine, Phase 17 the production hardening, and Phase 08 the networked-crime and regulatory layer.

## 13. Case Studies & Industry Examples

- **UK APP reimbursement (PS25/5; effective 7 October 2024, £85k cap, send/receive PSP liability split)**: publicly reshaped bank risk models — the industry now detects scam victims performing their own payments, not just stolen cards (see `/case-studies/README.md`).
- **Synthetic identity anatomy**: Federal Reserve whitepapers document fabricated identities ("credit-building") that pass bureau checks for years before bust-out; the Boston Fed (2025) cites industry estimates above $35B in 2023 US losses — the canonical delayed-label, cross-system detection case.
- **Stripe Radar**: public engineering content on ML-first fraud fighting with network-level signals — a rare honest industry window into thresholds, features, and trade-offs.
- **Neobank ATO waves**: consumer fintechs have publicly discussed account-takeover surges (credential stuffing, SIM swap) driving investment in device and behavioral intelligence — a live drift-and-adaptation case.

## 14. Interview Questions

**Why is accuracy meaningless in fraud detection?** At 0.1% prevalence, "always approve" scores 99.9% accuracy and blocks nothing; the operating metrics are precision@capacity, expected cost, and fraud-$ saved net of friction.

**How do you price a false positive versus a false negative?** FP: declined good customer → lost revenue, churn, review cost; FN: fraud loss, chargeback fees, ops. Encode both into a cost matrix per segment and amount band and optimize expected cost — the numbers are business inputs, not model outputs.

**Design auth-time scoring under 100ms.** Precomputed online features (streaming velocity aggregates, device/session state), a compact model, cache-heavy feature serving, rules fallback on timeout, p99 as the SLO, and shadow-mode validation for candidates before they decide.

**How do you handle label latency?** Treat labels as delayed and selected: confirmed fraud ≠ all fraud. Model label-arrival delay, weight by confirmation likelihood, use chargebacks as a noisy proxy, retrain on consistent-maturity vintages, and audit for investigator-prior bias.

**How do you detect synthetic identity?** Look for thin-file consistency anomalies (bureau footprint vs application velocity), identity-graph relationships (shared PII components across applicants), behavioral naivety, and credit-builder patterns — a longitudinal, cross-system, graph-shaped problem more than a transaction problem.

**Why does resampling break calibration, and what do you do?** Undersampling shifts the class prior, so outputs overstate fraud probability; correct via Dal Pozzolo's re-weighting or calibrate on original-distribution data afterwards — mandatory before scores feed dollar decisions.

**When does anomaly detection beat supervised learning?** Novel attack patterns with few labels, cold-start segments, and discovery/triage for new typologies; but precision is usually worse and scores are not probabilities — it complements supervised models rather than replacing them.

**PR-AUC vs ROC-AUC under extreme imbalance?** ROC-AUC plots TPR against FPR, and with 99.9% negatives enormous FPRs hide visually; PR-AUC conditions on the rare class and tracks the operating region where alerts and money actually live.

**How do rules and ML coexist in one decision?** Rules encode policy and known-bad with instant, explainable semantics and serve as the outage fallback; ML ranks the uncertain middle; champion/challenger and override logging keep both accountable and generate labels.

**How would you know fraudsters adapted to your model?** Segment-wise detection-rate decay post-deployment, new-pattern clusters among escapes, rising manual-review dependence, and drift on score/feature distributions — monitored continuously, with red-team probes and challenger models on standby.

## 15. Assessment — Can You Pass the Bar?

- [ ] Implement an expected-profit evaluation (cost matrix + threshold sweep) on IEEE-CIS (implementation item).
- [ ] Demonstrate resampling-induced calibration breakage and fix it, with reliability curves as evidence.
- [ ] Build point-in-time-correct velocity features and prove the naive version leaks.
- [ ] Ship a fraud model card: cost matrix, PR-AUC, precision@capacity, latency budget, fallback plan.
- [ ] Explain label latency and feedback-loop bias to a fraud-operations leader (explain item).
- [ ] Detect a seeded fraud ring using graph-derived features.
- [ ] Articulate 3DS step-up economics: when friction pays for itself.

## 16. Mastery Checkpoint

You may proceed to Phase 08 when:

1. A fraud modeling repo exists with: cost-sensitive model, calibration artifacts, business-level evaluation, and a reusable feature library.
2. A sequence anomaly model is built with documented false-alarm behavior at fixed capacity.
3. A hybrid decision prototype logs overrides and includes a written override-review analysis.
4. A drift playbook exists: monitored signals, thresholds, retraining and rollback triggers.

Evidence: repo links, evaluation reports, prototype demo. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Optimizing ROC-AUC against a Kaggle label and calling it a production strategy — label definitions (confirmed fraud vs chargeback vs rules hit) change everything.
- Resampling without recalibration: shipping probabilities that no longer mean probabilities.
- Velocity features computed over post-decision events — the fraud twin of point-in-time leakage.
- Feedback loops: training only on what investigators confirmed builds a model blind to what they never look for.
- Dashboards showing p50 latency while p99 breaches burn customers at checkout.
- Treating anomaly scores as probabilities and feeding them into profit arithmetic.
- Forgetting friction: a model that saves $1M of fraud while declining $10M of good volume is a bad model.

## 18. Where This Goes Next

Phase 08 zooms out from single transactions to networks and regulation: the graph signals previewed here become GNNs and entity resolution, and case management becomes the alert-to-SAR workflow. Phase 15 industrializes the streaming and latency layer you designed on paper here, and Phase 16 adds the formal compliance frame (note: the EU AI Act's high-risk list treats creditworthiness assessment and fraud-adjacent systems differently — verify current status).
