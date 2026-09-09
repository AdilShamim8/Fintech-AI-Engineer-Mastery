# Phase 06 — Credit Risk & Decisioning

> **Stage III — Core Financial ML** · **Duration: 5-6 weeks** · **Mastery target: Application → Production**
> **Position in path:** `05-statistics-econometrics` ← **this phase** → `07-fraud-payment-intelligence`

## 1. Objective

Credit is where machine learning earns its largest and most scrutinized paychecks in finance: every retail lending business is a sequence of binary decisions priced against expected loss. In this phase you will master the full credit decisioning stack — from WOE/IV scorecard engineering through PD/LGD/EAD modeling, calibration, reject inference, and adverse-action explainability — and learn to build models that survive both a profit-and-loss review and a regulator's validation. You will finish able to design an end-to-end lending decision system in which every approval or decline is defensible, calibrated, and monetizable.

## 2. Why It Matters in Finance

Retail and commercial lending run on probability of default. A 1-percentage-point miscalibration in PD across a large book translates directly into mispriced loans, unexpected charge-offs, and provisions that hit equity. Credit modeling is also the most heavily regulated corner of applied ML: models must be documented, validated, explainable per-applicant, and fair-lending tested before a single decision is automated. For a FinTech AI engineer this is the domain where "the model works" is a necessary but wildly insufficient claim.

- The interest rate you charge is a function of predicted PD × LGD × EAD plus operating cost and capital — bad probability calibration means bad pricing, not just bad AUC.
- IFRS 9 (international) and CECL (US) force forward-looking expected credit loss estimates from the same models — credit ML feeds accounting, not just decisions.
- Fair-lending law (ECOA/Reg B in the US) requires specific, accurate adverse-action reasons for every decline — explainability is a legal requirement, not a nice-to-have.
- Reject inference, population drift, and champion/challenger governance are production realities that Kaggle-style credit notebooks never touch.

## 3. Prerequisites

- [ ] Phase 01 — banking balance sheets, loan lifecycle, interest mechanics
- [ ] Phase 04 — logistic regression foundations, probability calibration, expectations
- [ ] Phase 05 — regularization, model validation discipline, multiple-testing awareness
- [ ] Existing ML skill: gradient boosting, cross-validation, SHAP, calibration curves (assumed known)
- [ ] Comfort with pandas/polars data pipelines

## 4. Learning Outcomes

- I can build a compliant application scorecard (WOE/IV binning, points-to-double-odds scaling) and defend why scorecards still dominate regulated retail credit.
- I can train gradient-boosting PD models that outperform scorecards and reconcile them into a governed decision stack.
- I can calibrate predicted probabilities so they are usable for pricing and provisioning, not just ranking.
- I can estimate LGD and EAD with the right model families (regression on severity, two-stage, Tweedie) and explain their data challenges.
- I can design a reject-inference approach and articulate the selection-bias problem it addresses.
- I can compute PSI/CSI, build vintage and roll-rate analyses, and specify drift-monitoring for a live credit model.
- I can generate adverse-action reason codes from both a scorecard and a gradient-boosting model.
- I can run a fairness assessment (disparate impact, error-rate parity) and describe the regulatory context for it.
- I can explain IFRS 9 / CECL staging and how my PD/LGD models plug into expected-credit-loss computation.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 06.1 | The credit decision problem | PD/LGD/EAD taxonomy, application vs behavioral scoring | concept note + glossary cards |
| 06.2 | Credit data & bureau data | tradelines, bureau scores, samples and their biases | data audit notebook on Home Credit |
| 06.3 | Scorecard methodology | WOE/IV binning, monotonicity, points-to-double-odds | scorecard on German/Taiwan credit |
| 06.4 | From scorecard to decision | cutoffs, approval rates, swap-set analysis | cutoff economics worksheet |
| 06.5 | ML for credit | boosting vs scorecards, interactions, constraints | XGBoost/LightGBM PD benchmark |
| 06.6 | Probability calibration | Platt/isotonic, why AUC ≠ pricing-grade probability | calibration study (Brier, reliability curves) |
| 06.7 | Reject inference | selection bias, augmentation, extrapolation, parcelling | reject-inference experiment on LendingClub |
| 06.8 | LGD & EAD modeling | severity regression, two-stage models, cures, downturn LGD | LGD model on LendingClub recovery data |
| 06.9 | Expected loss & provisioning | EL = PD×LGD×EAD, IFRS 9 staging, CECL lifetimes | mini ECL calculator |
| 06.10 | Portfolio view | vintage curves, roll rates, migration matrices, ASRF intuition | portfolio monitoring dashboard |
| 06.11 | Monitoring & drift | PSI/CSI, stability vs discrimination, retrain triggers | PSI monitor on simulated drift |
| 06.12 | Explainability & adverse action | reason codes, SHAP → reasons, counterfactuals | adverse-action letter generator |
| 06.13 | Fairness in credit | disparate impact, error-rate parity, proxy variables | fairness audit with fairlearn |
| 06.14 | Governance & validation | model documentation, independent validation, SR 11-7 lens | model development document (MDD) v1 |

**06.1 The credit decision problem.** Every credit system reduces to estimating three quantities — probability the borrower defaults (PD), loss severity if default happens (LGD), and exposure at default (EAD) — then pricing and deciding against their product. Application scoring decides whom to admit; behavioral scoring decides how to treat existing customers (limit increases, collections intensity). Internalize the difference between a *ranking* model and a *pricing-grade probability* model; the rest of the phase hangs on this distinction.

**06.2 Credit data & bureau data.** Bureau files are collections of tradelines (one per credit relationship) whose aggregation into applicant features involves judgment: utilization windows, delinquency recency, inquiry velocity. Real credit datasets are already filtered by past approval policies — the sample you train on was selected by a previous model, which is the seed of the reject-inference problem. Audit a public dataset (Home Credit) for these artifacts before modeling anything.

**06.3 Scorecard methodology.** Weight-of-Evidence binning converts each characteristic into log-odds points, enforced to be monotonic and stable; information value ranks predictive strength. Scorecards remain the regulated workhorse because every point is auditable, reasons are native, and drift is visible bin-by-bin. Build one end-to-end with `optbinning`/`skorecard` before you allow yourself gradient boosting.

**06.4 From scorecard to decision.** A model does not approve loans; a *policy* does — score cutoffs, exposure caps, rules overlays, and treatment assignments. Learn to express the cutoff choice in profit terms: expected revenue minus expected loss minus funding cost, per approval-rate scenario. Swap-set analysis (who gains/loses when you change models) is how risk teams reason about model changes.

**06.5 ML for credit.** Boosted trees reliably beat logistic scorecards on discrimination (Lessmann et al. 2015's benchmarking is the canonical evidence). But unexplained non-monotonicity, unstable segments, and weak reason codes are why many regulated lenders keep scorecards or hybrid stacks. Practice constrained boosting (monotonicity constraints) — it closes most of the gap while preserving defensibility.

**06.6 Probability calibration.** If PD is used for pricing or provisions, predicted probabilities must match observed default frequencies. Gradient boosting trained on imbalanced data is often badly miscalibrated; undersampling (as in Dal Pozzolo et al.) shifts intercepts. Make calibration (isotonic/Platt on a separate fold) a non-negotiable pipeline stage and evaluate with Brier score and reliability curves, not just AUC.

**06.7 Reject inference.** You only observe default outcomes for approved applicants — the denied population is a censored sample. Techniques: simple augmentation, reweighting, extrapolation from bureau performance, and accept/reject experiments. Understand that all of them are assumptions, and that a challenger model trained only on accepts can silently mis-rank the rejects you most need to price.

**06.8 LGD & EAD modeling.** Loss severity is bounded [0,1] (often with mass at 0 and 1), skewed, and macro-dependent — hence beta regression, two-stage cure models, or decision trees on segments; downturn LGD matters for capital. EAD concerns utilization at default (credit lines, CCF). Public data for LGD is scarcer than PD data; LendingClub's post-charge-off recovery amounts are a workable proxy with known limitations.

**06.9 Expected loss & provisioning.** EL = PD × LGD × EAD is the arithmetic; the accounting is the hard part. IFRS 9 stages exposures by significant increase in credit risk (12-month vs lifetime ECL); CECL requires lifetime estimates at origination. Know where your models feed this pipeline and why "forward-looking" macro adjustments are the most argued-over numbers in bank finance teams.

**06.10 Portfolio view.** Single-loan metrics mislead; portfolios evolve as cohorts. Vintage curves (cumulative default rate by origination month), roll-rate matrices (current → 30 → 60 → 90+ DPD), and migration matrices are the standard lenses. Vasicek's one-factor ASRF explains why concentration quietly breaks "average PD" thinking.

**06.11 Monitoring & drift.** PSI on scores and characteristics detects population shifts; but in credit, outcome labels arrive 12-24 months late, so you monitor *stability* long before you can measure *performance*. Define retrain triggers, champion/challenger cadence, and pre-agreed fallbacks as part of the model, not an afterthought (deepens in Phase 17).

**06.12 Explainability & adverse action.** Reg B requires specific principal reasons for adverse action. For scorecards, reason codes fall out of the points table; for ML, translate SHAP attributions into stable, human-readable, legally survivable reason statements. Counterfactual explanations ("what would change the decision") are increasingly expected by both customers and regulators.

**06.13 Fairness in credit.** Protected attributes are usually excluded, yet proxies (zip code, income source, device) leak them. Learn the metrics (disparate impact ratio, equalized odds, group-calibration), where they conflict, and the regulatory posture: US fair-lending enforcement (ECOA/Reg B, CFPB's 2022-03 circular requiring reasons for complex models) and EU AI Act treatment of creditworthiness assessment as high-risk. Fairness testing is becoming table stakes, not activism.

**06.14 Governance & validation.** A credit model is a governed asset: inventory entry, tiering, development documentation, independent validation, approved use scope, monitoring plan. Read SR 11-7 once now (it is short and it structures everything banks do); you will engineer for it properly in Phase 16.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Logistic regression & log-odds | Linear model on log(p/(1-p)) | Native scorecard form; coefficients = evidence in bits of odds | You cannot read or defend the industry's base model |
| WOE / information value | Bin-level log-odds encoding and predictive strength | Standardized, monotonic, auditable feature engineering | Scorecards will feel like folklore instead of a system |
| Calibration theory | Mapping scores to true frequencies | Pricing, provisions, and capital consume probabilities | Your "1.2% PD" is fiction; pricing quietly leaks money |
| Expected value & cost matrices | Probability-weighted P&L of decisions | Cutoff selection is an economics problem, not an AUC problem | You optimize AUC while the business optimizes profit |
| Censoring / selection bias | Outcomes observed only for selected samples | Approvals censor rejects (reject inference) | Your model is confident precisely where it is blind |
| Beta / Tweedie distributions | Bounded/skewed severity likelihoods | LGD and insurance loss modeling | You apply MSE to a variable that is not remotely Gaussian |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Point-in-time feature correctness | Bureau snapshots must reflect what was knowable at application; future-dated fields are silent leakage |
| Feature stores (Feast/offline first) | Application vs behavioral features have different freshness needs; parity between training and serving |
| Decision APIs & idempotency | A decision service must be retry-safe, logged, and versioned per request |
| Decision audit store | Every decision: inputs snapshot, model version, policy version, reasons — retained for years |
| Batch scoring vs real-time | Origination is batch-friendly; account management wants real-time limits/collections triggers |
| Reproducibility | Regulators re-run models; pin data snapshots, seeds, and environment |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| optbinning / skorecard | Industrial-grade WOE binning and scorecard fitting |
| scikit-learn | Baselines, calibration, metrics |
| XGBoost / LightGBM | Constrained boosting for challenger PD models |
| SHAP, shapash | Attribution and reviewer-friendly explanation reports |
| fairlearn / AIF360 | Fairness metrics and mitigation experiments |
| NannyML / evidently | Post-deployment stability monitoring patterns |
| MLflow | Experiment tracking and model registry discipline |
| DuckDB / pandas / polars | Fast analysis over the credit datasets |
| FastAPI | Wrap the decision logic into a defensible API (Phase 17 scales this) |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| SR 11-7: Supervisory Guidance on Model Risk Management (Federal Reserve/OCC, 2011) | Regulation | All | Governance | The blueprint every US-regulated model must live under | Essential |
| CFPB Circular 2022-03 (adverse action reasons for complex models) | Regulation | All | Explainability | Regulator's position on explaining ML credit decisions | Essential |
| Regulation B / ECOA overview (CFPB) | Regulation | All | Fair lending | Statutory basis for adverse action and fair lending | Essential |
| Basel framework — credit risk section (BIS) | Standard | Advanced | IRB concepts | Origin of PD/LGD/EAD vocabulary and capital logic | Reference |
| IFRS 9 summary materials (IASB plus Big-Four explainers) | Standard | Intermediate | Provisioning | How credit models feed financial statements | Recommended |
| Lessmann et al. (2015), "Benchmarking state-of-the-art classification algorithms for credit scoring: An update of research", EJOR | Paper | Intermediate | Benchmarks | The canonical "boosting beats logistic in credit" evidence | Essential |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Naeem Siddiqi, *Intelligent Credit Scoring* (2nd ed., Wiley 2016) | Book | Intermediate | Scorecards | The practitioner standard for scorecard lifecycle | Essential |
| Thomas, Crook & Edelman, *Credit Scoring and Its Applications* (SIAM) | Book | Advanced | Theory+practice | The rigorous reference for scoring mathematics | Recommended |
| Baesens, Roesch & Scheule, *Credit Risk Analytics* (Wiley 2016) | Book | Intermediate | PD/LGD/EAD | Broad coverage incl. LGD and survival methods | Recommended |
| Christoph Molnar, *Interpretable Machine Learning* (free online) | Book | Intermediate | Explainability | Reason codes and counterfactuals done right | Essential |
| Barocas, Hardt & Narayanan, *Fairness and Machine Learning* (free online) | Book | Advanced | Fairness | Formal treatment behind the fairness metrics | Recommended |
| Machine Learning for Credit Scoring school (Bart Baesens' lectures, public talks) | Course | Intermediate | Overview | Efficient orientation to the field | Optional |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| optbinning & skorecard documentation | Docs | Intermediate | Scorecards | Best open tooling for WOE/IV scorecards | Essential |
| Zest AI public materials on explainable underwriting | Blog/Reports | Intermediate | Fair lending | How the vendor market sells fairness+ML to lenders | Optional |
| Upstart public risk-factor disclosures (SEC filings) | Report | Advanced | AI lending reality | Candid industry view of AI-lending model risk | Recommended |
| Kaggle: Home Credit Default Risk winning-solution write-ups | Notebooks | Intermediate | Feature engineering | Bureau/tradeline feature craft at scale | Recommended |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Kaggle Learn tabular tracks | Course | Beginner refresher | Mechanics | Only if you want warm-up reps | Optional |
| Credit-scoring conference talks (Credit Scoring & Credit Control, Edinburgh, public slides) | Talks | Advanced | Industry practice | Where practitioners argue frontier topics | Reference |

## 10. Practical Exercises

1. - [ ] Load **Home Credit Default Risk**; produce a data audit: missingness by table, join integrity to SK_ID_CURR, and a list of every feature with a potential point-in-time violation.
2. - [ ] Build a WOE/IV binning table on **German Credit** with `optbinning`; enforce monotonicity; plot the points-to-double-odds scale.
3. - [ ] Train logistic scorecard vs LightGBM on **Give Me Some Credit**; compare AUC, Brier, and KS; write 200 words on which you would ship to a regulator and why.
4. - [ ] Calibrate the LightGBM model (isotonic, on a holdout) and show reliability curves before/after; convert calibrated PD into a risk-based price at 4% funding cost + 2% opex.
5. - [ ] Simulate reject inference on **LendingClub**: treat low-grade loans as "accepted", a policy band as "rejected", and compare naive model vs augmentation-based retraining on the recovered labels.
6. - [ ] Build vintage curves and a roll-rate matrix from Freddie Mac single-family sample data (free registration); explain in a note how cohorts reveal deterioration the pooled AUC hides.
7. - [ ] Compute PSI monthly on scores from two simulated populations; set and justify alert thresholds.
8. - [ ] Generate adverse-action reason codes for 5 declined applicants using SHAP top-contributors mapped to a plain-English reason library; review them as if you were a compliance officer.
9. - [ ] Run a fairness audit with `fairlearn` on the Home Credit model (sex/age proxies as available): report disparate impact ratio and error-rate gaps; discuss one mitigation and its accuracy cost.
10. - [ ] Draft a 2-page Model Development Document (purpose, data, method, performance, limitations, monitoring) for your best PD model following an SR 11-7 outline.

## 11. Mini Projects

**M1 — Regulated scorecard, end to end.** German/Taiwan credit → monotonic WOE scorecard → cutoff economics → reason codes. Deliverable: repo with notebook + policy memo. Difficulty: ★★☆☆☆.

**M2 — Calibrated challenger stack.** Home Credit → constrained LightGBM → isotonic calibration → profit-based cutoff vs scorecard cutoff; swap-set analysis of who is approved differently. Deliverable: benchmark report. Difficulty: ★★★☆☆.

**M3 — Reject inference experiment.** LendingClub engineered accept/reject split; quantify bias of an accepts-only model; test two reject-inference methods; document assumptions honestly. Deliverable: experiment log. Difficulty: ★★★★☆.

**M4 — Vintage & roll-rate monitor.** Freddie Mac sample → cohort dashboards + PSI alerts + a Markdown "monthly credit model health report" template. Difficulty: ★★★☆☆.

## 12. Major Project Hook

This phase powers **Flagship Project 2 — AI-Powered Credit Decisioning System** (`/projects/flagship/02-credit-risk-decisioning-system.md`): application intake → features → calibrated PD → policy+pricing engine → reasons API → monitoring. Build the modeling core here; productionize it in Phase 17.

## 13. Case Studies & Industry Examples

- **Upstart**: publicly reported the promise and the pain of AI-driven personal lending — growth, then a sharp 2022 downturn test that forced model and funding-model revisions; read its shareholder letters as a live case of model risk meeting macro reality (see `/case-studies/README.md`).
- **Zest AI**: vendor meaningfully associated with the push for ML underwriting with fairness/explainability packaging for regulated lenders.
- **Nubank**: publicly shares engineering culture around credit models for millions of customers in emerging markets — useful lens on scale + data scarcity.
- **Subprime auto / BNPL underwriting**: recurring CFPB and press scrutiny of alternative-data credit decisions — a reminder that "more data" and "defensible decisions" are different achievements.

## 14. Interview Questions

**Why can AUC be high while a credit model is commercially useless?** AUC measures ranking; lending consumes calibrated probabilities and threshold economics. A model can rank perfectly and still misprice if PD levels are biased — calibration and profit curves are the operating metrics.

**Explain WOE and why scorecards survived the ML era.** WOE re-expresses each bin as log-odds evidence, making every characteristic's contribution additive, monotonic, and explainable — exactly what validation, reason codes, and boards need. Boosting wins raw discrimination, but governance often keeps scorecards or constrained hybrids.

**What is reject inference and why does ignoring it bias models?** Outcomes exist only for approved applicants; a model trained on accepts extrapolates its own approval policy into the reject space. Techniques (augmentation, reweighting, extrapolation, experiments) all trade one assumption for another; the honest answer is to name the assumption.

**How do you produce adverse-action reasons from a gradient-boosting model?** Map per-applicant SHAP contributions to a controlled reason dictionary, aggregate correlated drivers, suppress unstable/duplicative codes, and test the output against Reg B expectations — the mapping layer is the engineering deliverable.

**Why is calibration a first-class requirement in credit?** PD feeds pricing, provisions (IFRS 9/CECL), and capital; a 2x systematic underprediction of default probability is a balance-sheet event, not a metric rounding error.

**What is PSI and what does a high score actually tell you?** Population Stability Index measures distribution shift between development and current populations. High PSI says "the population moved" — it cannot say whether performance degraded (labels lag), so it triggers investigation, not conclusions.

**Walk me through IFRS 9 staging.** Stage 1: performing, 12-month ECL; Stage 2: significant increase in credit risk (SICR triggers like 30-DPD or qualitative watchlists), lifetime ECL; Stage 3: credit-impaired, lifetime with interest on net basis. The fights are over SICR definitions and forward-looking macro scenarios.

**How would you test a credit model for fair lending?** Exclude protected attributes, then test proxies and outcomes: disparate impact ratios, group error-rate gaps, calibration by segment; document findings, mitigations, and business justification — the process, not a single metric, is the deliverable.

**LGD modeling: why not plain regression?** Severity is bounded, bimodal at 0/1 (cures vs total loss), and macro-dependent — hence beta regression, two-stage cure models, or segmented trees with downturn adjustments.

**Your model degrades 6 months post-deployment. Diagnose.** Distinguish population drift (PSI on features/scores), concept drift (relationship changed — often macro), data-quality breaks (upstream schema/parity), and policy changes (new segments routed differently). Check decision logs and input snapshots before touching the model.

## 15. Assessment — Can You Pass the Bar?

- [ ] Build a monotonic WOE scorecard and a constrained boosted challenger; both produce calibrated PDs.
- [ ] Express a cutoff decision as expected profit and defend the chosen approval rate.
- [ ] Explain reject inference to a risk officer, naming your assumptions.
- [ ] Produce reason codes for any declined applicant from both model families.
- [ ] Compute and interpret PSI, vintage curves, and roll rates on a real mortgage sample.
- [ ] Draft an SR 11-7-shaped model document a validator would not bounce.
- [ ] Run a fairness audit and articulate the metric trade-offs you accepted.

## 16. Mastery Checkpoint

You may proceed to Phase 07 when:

1. Your scorecard and challenger model repo contains: binning tables, calibration artifacts, profit analysis, reason-code generator, and a monitoring notebook.
2. You can give a 10-minute "risk committee" presentation defending your PD model (record it; store under `/notes/artifacts/`).
3. Your Model Development Document passes a self-review against SR 11-7's development/validation/use expectations.
4. You have implemented — not just read about — calibration, PSI, and a reject-inference experiment.

Evidence: repo links + recorded presentation + completed MDD. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Training on post-approval data and calling the result an application model (selection bias).
- Undersampling to fight imbalance and shipping uncalibrated probabilities to a pricing engine.
- Future-dated bureau attributes (e.g., current balance after decision date) — the most common silent leakage in credit competitions and the first thing validators hunt.
- Treating AUC lift as justification for a model change without swap-set and profit analysis.
- Reason codes generated from raw SHAP values without aggregation/stability controls — compliance teams will (correctly) reject them.
- Confusing stability with performance in monitoring: PSI high + labels too young is an open question, not a verdict.

## 18. Where This Goes Next

Phase 07 swaps the approval decision for the transaction decision: fraud shares credit's economics (cost-sensitive thresholds), imbalanced data, and explainability duties — but adds milliseconds-scale latency and adversarial adaptation. Phase 16 will formalize the governance instincts you built here into a full compliance stack.
