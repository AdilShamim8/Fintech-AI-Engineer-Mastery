# Phase 05 — Statistics, Econometrics & Causal Inference

> **Stage II — Data & Quant Core** · **Duration: 4 weeks** · **Mastery target: Application**
> **Position in path:** `04-financial-mathematics` ← **this phase** → `06-credit-risk`

## 1. Objective

Financial data is low-signal, high-noise, non-stationary, and — worst of all — mined by everyone before you got to it. This phase installs the statistical discipline to survive that: rigorous estimation and inference, econometric time-series modeling, honest backtest validation, Bayesian reasoning, and causal inference for decisions that matter. You will learn why most published backtests are fiction, how to build ones that are not, and how to move from correlation to credible claims about what causes what — the difference between an AI engineer who fits models and one whose models ship into regulated financial decisions.

## 2. Why It Matters in Finance

The economics of finance punish statistical sloppiness asymmetrically: a backtest that lies survives until real capital is behind it, and then fails loudly. Regulators, validators, and sophisticated counterparties all assume you can defend how you know what you know. This phase is the source of that defense.

- Backtest overfitting is the quant industry's occupational disease; multiple-testing corrections and leakage-proof validation are the only standing defenses.
- The regulated core of financial ML is econometrics: credit scorecards are logistic GLMs, insurance severities are Tweedie, volatility is GARCH — Phase 06 and beyond assume this fluency.
- Product and policy decisions (rate changes, limit changes, collections treatments, fee experiments) require causal designs, not dashboard before/after comparisons.
- "Why do you believe your model generalizes?" is the first validator question; walk-forward and purged validation are the answers.

## 3. Prerequisites

- [ ] Phase 04 — return distributions, fat tails, stochastic processes, expectation/covariance fluency
- [ ] Phase 03 — time-indexed data pipelines, point-in-time feature discipline
- [ ] Existing ML skill: regularization, cross-validation, gradient boosting (assumed known)
- [ ] Basic probability and linear algebra (the finance-specific parts live in Phase 04)

## 4. Learning Outcomes

- I can construct honest confidence intervals (including bootstrap) for financial metrics like Sharpe ratios.
- I can detect and correct multiple testing with Bonferroni/Benjamini-Hochberg and explain precisely how backtests lie.
- I can diagnose OLS assumption failures in financial data and apply robust, Newey-West, and clustered standard errors.
- I can build logistic scorecards and Tweedie GLMs as the regulated baseline model families.
- I can test stationarity (ADF/KPSS), difference appropriately, and explain spurious regression.
- I can fit ARIMA/SARIMAX, ETS/state-space, VAR, and GARCH models with honest rolling-origin evaluation.
- I can test cointegration and build an error-correction view of a pairs relationship.
- I can implement purged K-fold with embargo and demonstrate the leakage random K-fold commits on overlapping labels.
- I can run Bayesian updating (Beta-Bernoulli) and build simple pymc models for conversion and credit priors.
- I can design A/B tests with guardrail metrics and analyze quasi-experiments (DiD, synthetic control) including their failure conditions.
- I can build uplift models and connect selection bias to credit reject inference.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 05.1 | Estimation & inference for noisy markets | CIs, tests, bootstrap on financial metrics | bootstrap inference notebook |
| 05.2 | Multiple testing & data snooping | Bonferroni/BH, why backtests lie | strategy-mining simulation |
| 05.3 | OLS in finance | Assumptions, robust & clustered SEs | robust-inference study |
| 05.4 | Logistic regression & GLMs | Scorecard base, Tweedie severity | GLM vs GBM benchmark |
| 05.5 | Regularization with correlated factors | Ridge/lasso/group penalties under collinearity | grouped-regularization experiment |
| 05.6 | Stationarity, unit roots & differencing | ADF/KPSS, spurious regression | stationarity audit on FRED series |
| 05.7 | ARIMA/SARIMAX & ETS/state space | Classical forecasting done honestly | walk-forward forecast benchmark |
| 05.8 | Granger causality & VAR | Lead-lag tests, limits, system modeling | VAR + Granger notebook |
| 05.9 | Cointegration & error correction | Engle-Granger, Johansen, ECM | pairs-relationship study |
| 05.10 | GARCH family | Volatility clustering, asymmetry | volatility forecast model |
| 05.11 | Backtest validation discipline | Walk-forward, purged K-fold + embargo, deflated Sharpe | validator library + leakage demo |
| 05.12 | Bayesian inference & MCMC | Beta-Bernoulli, priors in credit, posterior thinking | pymc Bayesian A/B model |
| 05.13 | Causal foundations: confounders, DAGs, experiments | RCTs, A/B tests, guardrail metrics | experiment design document |
| 05.14 | Quasi-experiments & targeting | DiD, synthetic control, uplift, IV, panel FE, reject inference | DiD + uplift study |

**05.1 Estimation & inference for noisy markets.** Financial metrics are estimated, not observed: a Sharpe ratio from three years of daily data carries a standard error driven by sample length and fattened by tails, and two "competing" strategies often differ by less than their CIs. Learn to report uncertainty intervals alongside every headline metric, using parametric formulas where they exist and bootstrap (with block structure for serial correlation) where they do not. You build a bootstrap inference notebook that will embarrass several of your earlier conclusions — that is the point.

**05.2 Multiple testing & data snooping.** Test enough strategies on random walks and some will look brilliant — the maximum of many noisy statistics is biased upward, and published backtests are the survivors of exactly this filter. Bonferroni controls family-wise error (conservative), Benjamini-Hochberg controls false discovery rate (practical); Harvey, Liu & Zhu documented that factor research needs much higher t-stat hurdles for the same reason. You simulate mining 1,000 strategies on pure noise and watch impressive Sharpe ratios manufacture themselves.

**05.3 OLS in finance.** Gauss-Markov assumptions rarely hold here: heteroskedasticity is the norm, autocorrelation is pervasive, outliers are crashes, and regressors are endogenous more often than admitted. White/HC robust standard errors fix heteroskedasticity, Newey-West handles autocorrelation, and clustered SEs matter in panels (many names, shared macro shocks). Naive SEs on financial residuals produce significance theater — you will demonstrate the difference numerically.

**05.4 Logistic regression & GLMs.** Logistic regression is the credit scorecard's native form (Phase 06 builds directly on it), and the GLM framework generalizes it: Poisson for counts, Gamma/Tweedie for severities with mass near zero and a long right tail — Tweedie being standard actuarial practice for insurance loss severity. The unifying idea is the link function: model the mean through a transformation that respects the data-generating process. You benchmark a well-specified GLM against gradient boosting and learn when interpretability plus correct likelihood beats raw discrimination.

**05.5 Regularization with correlated financial factors.** Financial predictors are massively collinear — macro factors, rate tenors, momentum windows — and lasso responds by arbitrarily picking one of a correlated group, making selection unstable across samples. Ridge shrinks coherently without dropping, elastic net balances both, and group lasso respects factor structure (keep or drop whole groups). Stability selection (does a feature survive resampling?) is the honest test of importance — you run the experiment and watch selections flip.

**05.6 Stationarity, unit roots & differencing.** Regressing one trending (integrated) series on another produces spurious regression — high R² and t-stats from pure nonsense, the Granger-Newbold warning. ADF and KPSS tests diagnose, with low power and role-reversed nulls, so treat them as evidence not verdicts; financial levels are usually I(1) and returns I(0). You audit FRED series (CPI, unemployment, rates) and build the habit: test, difference, re-test, and never model levels blindly.

**05.7 ARIMA/SARIMAX & ETS/state space.** Classical time-series models remain hard to beat at short horizons on well-behaved series, and they force the diagnostic discipline ML often skips: residual autocorrelation, seasonality, exogenous inputs (SARIMAX), and Hyndman's forecast-principles workflow with time-series cross-validation. ETS and the state-space view handle the same problems from a different angle. You build a walk-forward benchmark on FRED retail sales with baselines (naive, seasonal-naive) that occasionally win — internalize that humility.

**05.8 Granger causality & VAR.** Granger causality is predictive lead-lag, not causation: X helps predict Y given Y's past, nothing more — in near-efficient markets the edge is small, fragile, and sample-sensitive. Vector autoregressions model systems (rates, inflation, equities) jointly, with impulse-response analysis as the payoff. You fit a small VAR, run the tests, and write the caveats a skeptic would write — because that is the voice you need in design reviews.

**05.9 Cointegration & error correction.** Two I(1) series can share a stationary linear combination — a long-run equilibrium with short-run error-correction dynamics pulling deviations back. Engle-Granger (two-step) and Johansen (system) tests identify these relationships, which are the statistical foundation of pairs trading, basis trading, and hedging ratios that do not drift apart. You test a set of liquid ETF pairs and estimate mean-reversion half-lives, with multiple-testing honesty from lesson 05.2.

**05.10 GARCH family.** Volatility clusters: calm begets calm, crisis begets crisis, and returns show little autocorrelation while squared returns show a lot — the ARCH signature. GARCH(1,1) captures persistence with three parameters; EGARCH/GJR variants capture the leverage asymmetry (bad news raises vol more). Volatility forecasts feed VaR (Phase 04), position sizing, and options models. You fit GARCH with the `arch` package and evaluate forecasts honestly against squared-return proxies.

**05.11 Backtest validation discipline.** Walk-forward (rolling-origin) evaluation is the honest default for time-ordered data; for overlapping labels (multi-horizon returns), purged K-fold with embargo (López de Prado) removes train/validation contamination; the deflated Sharpe ratio (Bailey & López de Prado) adjusts the best backtest for the number of trials tried. The meta-rule: your validation design is part of the model and must be documented with it. This lesson produces the validator library you will use for every later phase.

**05.12 Bayesian inference & MCMC.** Beta-Bernoulli is the canonical conversion model: a prior plus observed conversions yields a posterior — no asymptotics required. Priors are institutional memory: a new segment with 20 observations should not get an unshrunk default rate, and conservative priors are defensible to validators. MCMC (conceptually) samples posteriors when conjugacy ends; pymc makes it practical. You build a Bayesian A/B analysis and a hierarchical shrinkage demo for thin credit segments.

**05.13 Causal foundations: confounders, DAGs, experiments.** Acting on a correlation requires a causal story; DAGs make assumptions explicit and reveal which adjustments identify the effect. Randomized experiments are the gold standard, and fintech runs them constantly — A/B tests on approval flows, fees, nudges — where the craft is guardrail metrics (complaints, regret rates, fairness) that catch "winning" variants that quietly harm trust. You write an experiment design document including stopped-early rules and guardrails.

**05.14 Quasi-experiments & targeting.** When you cannot randomize: difference-in-differences with parallel-trends diagnostics, synthetic control for single-unit interventions, instrumental variables for unmeasured confounding (concept level), and uplift modeling to target treatments at the persuadable rather than everyone. Selection bias gets formalized here and meets credit directly: reject inference (Phase 06) is selection bias in production clothing, and panel fixed effects absorb time-invariant heterogeneity in credit portfolios. You run a DiD on a FRED-observable policy shock and an uplift model, then write down what could invalidate each.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Sampling error & confidence intervals | Uncertainty of an estimator given noisy, finite data | Sharpe ratios and default rates are estimates, not facts | You compare models within each other's noise bands |
| FWER vs FDR control | Bonferroni-style vs Benjamini-Hochberg corrections | Backtests are thousands of implicit hypothesis tests | Your "alpha" is the max of a noise distribution |
| (Non)stationarity & integration orders | I(0)/I(1) behavior; unit-root diagnostics | Levels trend, returns mean-revert; mixing them is spurious | Regressions with beautiful t-stats on garbage |
| Cointegration & ECM | Stationary combinations of non-stationary series | Long-run equilibria: pairs, bases, hedging ratios | You difference away the long-run signal or trade noise |
| Conditional heteroskedasticity (GARCH) | Time-varying volatility with persistence | Volatility clusters; risk models need it | VaR sized for calm markets in stormy ones |
| Beta-Bernoulli posterior | Conjugate Bayesian updating for rates | Conversion and default rates with priors | Small samples get unshrunk, unstable estimates |
| Potential outcomes & parallel trends | The formal object causal designs estimate | Policy and treatment evaluation in fintech | You sell correlation as impact; someone pays for it |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Lookahead-proof pipelines | As-of joins, lag discipline, and point-in-time features (extends Phase 03) — the #1 source of fraud in your own results |
| Backtesting harnesses | Reusable rolling-origin runners so every model is evaluated the same honest way |
| Time-series CV tooling | Purged K-fold + embargo as library code, not a notebook copy-paste |
| Experiment infrastructure | Assignment logging, guardrail dashboards, maturity-aware readouts |
| Data revision handling | FRED/macro series get revised; models must pin vintages or handle revisions explicitly |
| Reproducible statistical workflows | Pinned data snapshots, seeds, and environments — validators re-run things |
| Panel data alignment | Entity-time joins, balanced vs unbalanced panels, fixed-effect encoding at scale |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| statsmodels | OLS/GLM with robust SEs, ARIMA/SARIMAX, VAR, cointegration tests |
| arch | GARCH-family volatility models (Sheppard) |
| linearmodels | Panel regressions, fixed effects, IV estimation |
| scikit-learn | Regularization, uplift-style learners, general ML plumbing |
| pymc | Bayesian models and MCMC sampling |
| sktime | Unified time-series forecasting/CV interfaces |
| numpy / pandas | Panel alignment, bootstrap, and everything else |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| statsmodels documentation (statsmodels.org) | Docs | Intermediate | Econometrics | The reference implementation you will use daily | Essential |
| arch documentation (Kevin Sheppard) | Docs | Intermediate | Volatility | Canonical GARCH tooling and background notes | Essential |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Hyndman & Athanasopoulos, *Forecasting: Principles and Practice* (FPP3, free online) | Book | Intermediate | Forecasting | The modern forecasting workflow, free and excellent | Essential |
| Tsay, *Analysis of Financial Time Series* (Wiley) | Book | Advanced | Financial TS | Rigorous financial time-series reference (GARCH, VAR, cointegration) | Recommended |
| López de Prado, *Advances in Financial Machine Learning* — backtesting/CV chapters (Wiley 2018) | Book | Advanced | Validation | Purged K-fold, embargo, backtest overfitting in full detail | Essential |
| Bailey & López de Prado, "The Deflated Sharpe Ratio" (Journal of Portfolio Management, 2014) | Paper | Advanced | Multiple testing | The formal correction for trial-count-inflated Sharpe ratios | Recommended |
| Harvey, Liu & Zhu, "…and the Cross-Section of Expected Returns" (Review of Financial Studies, 2016) | Paper | Advanced | Data snooping | The academic evidence that factor research is a multiple-testing minefield | Recommended |
| Cunningham, *Causal Inference: The Mixtape* or Huntington-Klein, *The Effect* (both largely free online) | Book | Intermediate | Causal inference | Accessible DiD, IV, synthetic control, DAGs with code | Essential |
| Kohavi, Tang & Xu, *Trustworthy Online Controlled Experiments* (Cambridge, 2020) | Book | Intermediate | Experiments | The A/B testing bible: guardrails, pitfalls, maturity | Recommended |
| McElreath, *Statistical Rethinking* (2nd ed.; lectures free online) | Book | Intermediate | Bayesian | Bayesian modeling with scientific humility; lectures are superb | Recommended |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Nixtla documentation (statsforecast, mlforecast) | Docs | Intermediate | Forecasting | Fast, production-grade baselines worth benchmarking against | Optional |
| sktime documentation | Docs | Intermediate | TS tooling | Composable pipelines and time-series CV interfaces | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| QuantConnect lecture series (quantconnect.com) | Course | Intermediate | Empirical methods | Free lectures bridging these methods to trading practice | Optional |

## 10. Practical Exercises

1. - [ ] Bootstrap (block bootstrap, 10k resamples) the Sharpe ratio of a simple strategy on real index returns; report CI width vs sample length; state what differences between strategies are actually distinguishable.
2. - [ ] Strategy-mining simulation: fit 1,000 random parameterizations to pure-noise returns; record the max in-sample Sharpe across 100 repetitions; apply Bonferroni and Benjamini-Hochberg; write 300 words on what you saw.
3. - [ ] Spurious regression demo: regress one random walk on another; show R², t-stats, and Durbin-Watson; then difference, re-run, and watch the illusion collapse.
4. - [ ] ADF + KPSS audit on five FRED series (CPI, unemployment, 10Y yield, retail sales, industrial production); classify I(0)/I(1) with reasoning, not just p-values.
5. - [ ] Walk-forward ARIMA/SARIMAX vs ETS vs seasonal-naive on FRED retail sales; compare with MASE and prediction-interval coverage.
6. - [ ] Fit GARCH(1,1) and EGARCH to daily S&P 500 returns; evaluate one-step-ahead vol forecasts against squared returns and an EWMA baseline.
7. - [ ] Engle-Granger and Johansen tests across a 20-ETF universe; for the top pair, fit an error-correction model and estimate the mean-reversion half-life via an OU approximation.
8. - [ ] Bayesian A/B in pymc on the Bank Marketing (UCI) data: Beta-Bernoulli conversion posteriors, credible intervals, and a decision under a pre-agreed loss function.
9. - [ ] DiD event study around a documented FRED-observable policy episode (e.g., a rate-decision window): pre-trend plot, DiD estimate, placebo-timing robustness check.
10. - [ ] OLS with naive vs HC-robust vs Newey-West vs entity-clustered SEs on a small panel; make a table of how "significance" changes under each.

## 11. Mini Projects

**M1 — Walk-forward ARIMA/GARCH forecast with honest evaluation.** FRED series plus an equity index; rolling-origin evaluation, baselines included, prediction intervals checked. Deliverable: forecast report with an explicit "claims I will not make" section. Difficulty: ★★☆☆☆.

**M2 — Cointegration pairs screener on liquid ETFs.** Engle-Granger/Johansen screening with multiple-testing correction; half-life estimates; z-score strategy evaluated walk-forward. Deliverable: screener + honest evaluation memo. Difficulty: ★★★☆☆.

**M3 — Uplift model on public marketing data.** Criteo-uplift public dataset (fallback: Bank Marketing UCI with a simulated treatment); two-model and class-transformation approaches; Qini/AUUC evaluation; targeting policy vs blanket treatment economics. Deliverable: uplift notebook + policy memo. Difficulty: ★★★☆☆.

**M4 — Difference-in-differences on a policy shock.** FRED series around a documented rate or regulatory event; pre-trend diagnostics, event-study plot, threats-to-validity memo. Deliverable: analysis notebook + validity assessment. Difficulty: ★★★☆☆.

**M5 — Purged K-fold + embargo validator, with a leakage demonstration.** Implement the splitter as a reusable library; construct an overlapping-label dataset; show quantitatively how random K-fold inflates scores vs purged/embargoed validation. Deliverable: library + leakage demonstration report. Difficulty: ★★★☆☆.

## 12. Major Project Hook

The M5 validator library becomes evaluation infrastructure for every Stage III+ phase and the backtesting core of **Flagship Project 2 — AI-Powered Credit Decisioning System** (`/projects/flagship/`; see `/projects/flagship/README.md`); the causal toolkit recurs in product, marketing, and policy analytics across Stage V.

## 13. Case Studies & Industry Examples

- **The factor-zoo correction**: Harvey, Liu & Zhu (2016) documented that most published return-predictive factors are consistent with heavy multiple testing, and proposed far higher t-stat hurdles — the canonical data-snooping case study (see `/case-studies/README.md`).
- **The August 2007 "quant quake"**: Khandani & Lo's widely cited post-mortem of crowded quant strategies unwinding together — historically validated models sharing hidden, correlated risk.
- **Experiment programs at scale**: Kohavi-documented experiments at major web companies (with close analogues at consumer fintechs) show guardrail metrics catching variants that "won" the target metric while degrading trust or compliance.
- **FRED revisions**: macro series are revised after publication; models trained without revision awareness silently change behavior across vintages — a mundane but production-critical econometrics trap.

## 14. Interview Questions

**Why is R² almost meaningless in finance?** Returns are dominated by noise, so tiny out-of-sample R² can still be economically exploitable, while high R² on levels often just signals spurious regression between trending series. Judge models by out-of-sample economics and calibrated uncertainty, not R².

**Explain p-hacking in backtests.** Trying many parameter sets, universes, and variants and reporting the best one inflates results by pure selection — with enough trials, random strategies shine. Defenses: count your trials, apply deflated Sharpe or BH correction, keep a truly untouched holdout, and pre-register hypotheses.

**Cointegration vs correlation?** Correlation measures short-run co-movement of (usually) returns; cointegration is a long-run equilibrium between levels such that some linear combination is stationary. Two series can be uncorrelated day-to-day yet cointegrated — and correlated yet diverging without bound.

**How do you evaluate an A/B test when outcomes are delayed?** Cohort by assignment date with a fixed maturity window (measure only matured units), use sequential testing with spending functions if you must peek, track guardrails continuously, and never compute naive significance on immature cohorts.

**Why does random K-fold leak on financial data?** Overlapping labels put near-duplicate samples on both sides of the split, and serial correlation lets the model see adjacent-time information; purging overlapping labels from training and embargoing a gap around validation restores honesty.

**What does the deflated Sharpe ratio correct?** It adjusts the best observed Sharpe for the number of trials, sample length, and non-normality (skew/kurtosis), answering: "how impressive is the maximum Sharpe given how much you searched?"

**When can you trust Granger causality?** Only as predictive lead-lag within your sample and feature set: it is sensitive to omitted variables and lag choice, and it licenses no intervention claim. Use it as one clue, alongside economics and design-based evidence.

**Why robust or clustered standard errors in financial panels?** Residuals are heteroskedastic and cross-sectionally correlated (shared macro shocks hit many names at once), so naive SEs understate uncertainty and manufacture significance; the corrections fix inference, not coefficients.

**How would you use Bayesian priors in credit scoring?** Shrink thin-segment default rates toward portfolio-level or bureau-anchored estimates (empirical-Bayes or hierarchical models), encode conservative regulatory priors explicitly, and update as vintages mature — auditable and stable, unlike unshrunk small-sample rates.

**When does difference-in-differences fail?** Parallel-trends violations, anticipation before the policy, composition changes in treated/control groups, and spillovers onto controls; diagnostics include pre-trend event studies, placebo timing, and alternative control groups.

**What is uplift modeling and when is it worth it?** Modeling the treatment effect per customer rather than the outcome, to target persuadables instead of everyone. Worth it when treatment is costly or double-edged — a credit-limit increase raises both usage and risk — because the money is in the heterogeneity.

## 15. Assessment — Can You Pass the Bar?

- [ ] Produce bootstrap CIs for a Sharpe ratio and explain what drives their width.
- [ ] Show, numerically, how a 1,000-strategy search manufactures impressive in-sample results, and correct it with BH.
- [ ] Diagnose an OLS inference failure on financial data and fix it with the right robust SEs (implementation item).
- [ ] Fit and honestly evaluate ARIMA, ETS, and GARCH with rolling-origin backtesting.
- [ ] Test a pairs relationship for cointegration and explain its error-correction dynamics.
- [ ] Implement purged K-fold + embargo and demonstrate the leakage random K-fold commits on overlapping labels.
- [ ] Design an A/B test with guardrail metrics and delayed-outcome handling, and defend it to a product/risk review.
- [ ] Run a DiD analysis with pre-trend diagnostics and articulate its validity threats.

## 16. Mastery Checkpoint

You may proceed to Phase 06 when:

1. A reusable validator library exists (purged K-fold + embargo, walk-forward runner) and is used by all your later projects.
2. A walk-forward forecasting report (ARIMA/GARCH/ETS) with honest error accounting is on file.
3. A causal-inference notebook (DiD or uplift) exists with an explicit assumptions section.
4. You have recorded a 5-minute explanation of "why backtests lie and how I defend against it" (store under `/notes/artifacts/`).

Evidence: repo links, evaluation reports, recorded explanation. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Random K-fold on overlapping financial labels — the most common statistical crime in fintech ML.
- In-sample feature selection followed by cross-validation on the same data (selection leakage wearing a CV costume).
- Treating an ADF pass/fail as proof of (non)stationarity — the tests have low power; combine with economics.
- Over-differencing away a cointegrating relationship and losing the long-run signal you were hunting.
- Reporting naive OLS standard errors on heteroskedastic, autocorrelated residuals — significance theater.
- Reading experiments before outcomes mature, or against a conversion definition that changed mid-test.
- Presenting Granger tests or VAR impulse responses as causal policy analysis.

## 18. Where This Goes Next

Phase 06 applies your GLM, calibration, and validation discipline to credit decisioning — reject inference is your selection-bias knowledge wearing production clothes. Phase 09 deepens the time-series line from GARCH toward multivariate and ML forecasting, and the causal toolkit recurs in product, marketing, and policy work throughout Stage V.
