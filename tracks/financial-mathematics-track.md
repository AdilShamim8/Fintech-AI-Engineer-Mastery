# Track — Financial Mathematics

> Deliverable H. The finance-only mathematics progression. You already know generic math; this track covers what finance adds and what AI consumes. Every concept uses the four-part frame: **What / Why finance uses it / Where AI uses it / Cost of skipping it.**
> Main teaching: Phases 04-05; applied in 06-10; tables below are the spine.

---

## 1. Progression Map

```text
Level A — Decision arithmetic        TVM, compounding, discounting, NPV/IRR, amortization
Level B — Probability & loss         distributions (lognormal, Poisson, fat tails), expectations, cost matrices, Bayes
Level C — Dependence & structure     correlation vs dependence, copulas, covariance, PCA (yield curves/factors)
Level D — Dynamics & uncertainty     random walks, Brownian/GBM, Itô intuition, Poisson arrivals, Monte Carlo
Level E — Risk & portfolio math      duration/convexity, VaR/ES, MPT, CAPM/factors, optimization (QP)
Level F — Inference & causality      regression/GLMs, calibration, multiple testing, purged CV, causal (DiD/uplift/IV)
Level G — Econometrics of time       stationarity, ARIMA, GARCH, cointegration, state space, nowcasting
```

## 2. Concept Tables

### Level A — Decision arithmetic (Phase 01/04)

| Concept | What it is | Why finance uses it | Where AI uses it | Cost if skipped |
|---|---|---|---|---|
| TVM | Value depends on time: FV=PV(1+r)^t | Every product price, loan, bond is discounted cash | Profit curves for decision thresholds; LTV features | You cannot price anything or read a business case |
| Amortization | Payment split into interest/principal over time | Loans/mortgages are schedules, not amounts | EAD evolution features; collections strategies | EAD modeling will confuse you |
| NPV/IRR | Discounted project value; rate clearing NPV=0 | Capital allocation decisions | Expected-profit ranking of model changes | You'll mis-rank initiatives; IRR's reinvestment trap will bite |

### Level B — Probability & loss (Phase 04/07)

| Concept | What | Why finance | Where AI | Cost |
|---|---|---|---|---|
| Lognormal returns | Prices ≈ lognormal (positive, multiplicative shocks) | Returns compound multiplicatively; prices can't go below zero | Return features; vol modeling; GBM simulation | Vol models and option math will feel arbitrary |
| Fat tails (Student-t, skew) | Extreme events are more likely than Gaussian says | Markets crash; single-day moves exceed "impossible" | Risk model choice; anomaly thresholds; stress tests | Your VaR will be a fairy tale; backtests will explode |
| Poisson processes | Count of rare events per unit time | Defaults, claims, fraud arrivals | Fraud rate modeling; survival analysis; queueing | Default/fraud intensity modeling will be guesswork |
| Bayes | Posterior ∝ likelihood × prior | Fraud is rare → base-rate fallacy dominates | Alert triage precision; A/B decisions; screening | You'll be shocked that 99%-accurate models are useless at 0.1% prevalence |
| Cost matrices | Asymmetric error costs | FP blocks a good customer; FN funds a fraudster | Threshold selection; cost-sensitive learning | You'll optimize accuracy while losing money |

### Level C — Dependence & structure (Phase 04)

| Concept | What | Why finance | Where AI | Cost |
|---|---|---|---|---|
| Correlation ≠ dependence | Pearson captures linear comovement only | Crises break correlations (tail dependence) | Copula-based joint default simulation; portfolio risk | "Diversified" portfolios that fail exactly when needed |
| Copulas | Joint distributions from marginals + dependence | Default correlation (CDO era), joint risk | Synthetic joint scenarios; stress dependence | Systemic-risk features will be invisible |
| PCA on term structure | Yield-curve decomposition → level/slope/curvature | Rates move in 3 dominant modes | Rate features; scenario generation | Macro features will be ad hoc |

### Level D — Dynamics & uncertainty (Phase 04/10)

| Concept | What | Why finance | Where AI | Cost |
|---|---|---|---|---|
| GBM | dS = μS dt + σS dW — multiplicative noise | Canonical price model; tractable | Price simulation; scenario data for backtests | Option/derivative ML features will be black boxes |
| Itô (intuition) | Noise changes the drift (Itô's lemma) | Derives BS PDE; why vol enters pricing | Deep-hedging papers; SDE-based generators | Quant papers will be unreadable |
| Monte Carlo | Estimate expectations by simulation | Pricing, VaR, capital, scenario analysis | Synthetic data generation; RL environments; variance-reduction in evals | You can't build simulators or understand backtest noise |

### Level E — Risk & portfolio math (Phase 04/10)

| Concept | What | Why finance | Where AI | Cost |
|---|---|---|---|---|
| Duration/convexity | 1st/2nd-order price sensitivity to rates | Bond risk in one number (+ correction) | Rate-shock features; ALM analytics | Fixed-income features will be meaningless |
| VaR/ES | Loss quantile / expected loss beyond it | Regulatory risk reporting; limits | Model risk targets; backtest stats (Kupiec) | Risk conversations will exclude you |
| MPT + QP | Mean-variance frontier via convex optimization | Diversification formalized | Portfolio construction; cvxpy pipelines | Allocation projects will be hand-wavy |
| Factor models | Returns = factor exposures + idio | Explains comovement; risk attribution | Barra-style features; alpha orthogonalization | "Alpha" claims will be factor bets in disguise |

### Level F — Inference & causality (Phase 05/06)

| Concept | What | Why finance | Where AI | Cost |
|---|---|---|---|---|
| Calibration | Predicted probs match observed frequencies | Pricing/provisions consume probabilities | Isotonic/Platt; Brier/reliability evals | Your PDs are fiction; pricing leaks |
| Multiple testing | Many trials guarantee false "signal" | Thousands of backtests → fake alphas | Deflated Sharpe; BH correction in feature mining | You'll ship noise that looked great in the lab |
| Selection bias / censoring | Outcomes observed only for chosen samples | Rejects have no outcomes; fraud labels lag | Reject inference; label-latency handling | Models confident exactly where blind |
| Causal (DiD/uplift/IV) | Estimating effects, not associations | Policy/pricing/treatment effects | Uplift targeting; policy evaluation; experiments | You'll attribute outcomes to the wrong lever |

### Level G — Econometrics of time (Phase 05/09)

| Concept | What | Why finance | Where AI | Cost |
|---|---|---|---|---|
| Stationarity/ADF | Stable distributional properties over time | Most TS models assume it; series must be transformed | Feature stability; regime features | Forecasts will trend into fantasy |
| GARCH | Conditional heteroskedasticity | Vol clusters — calm and storm regimes | Vol features; risk forecasts | Risk models will ignore regime shifts |
| Cointegration | Shared long-run stochastic trend | Pairs trading; economic equilibrium | Spread features; mean-reversion signals | You'll trade correlated pairs that never revert |
| State space / ETS | Latent-level models with noise | Smoothing, nowcasting, structural breaks | Nowcasting pipelines; baseline forecasters | Modern TS libraries will be magic boxes |

## 3. The "Do Not Relearn" List

You already own these from AI engineering — do not redo generic courses: calculus/gradients, linear algebra basics, standard ML estimation, generic optimization (SGD/Adam), probability basics at the coin-dice level. Where they appear below is as *inputs*, not as syllabus.

## 4. Evidence Bar for This Track

- [ ] Re-derive EL = PD × LGD × EAD and defend each term's estimation choice.
- [ ] Show, on real data, a case where correlation held but copula structure failed (or explain why you couldn't, honestly).
- [ ] Produce a calibrated model and prove calibration with a reliability curve + Brier.
- [ ] Implement purged K-fold and demonstrate the leakage random CV commits.
- [ ] Explain duration, VaR, GARCH, and cointegration to a product manager — recorded, 4 minutes each.
- [ ] Build one Monte Carlo simulator and one QP portfolio optimizer from scratch.

**Where this track pays rent:** every Stage III-VI phase. If a later phase exposes a gap, come back to the specific row, not the whole track.
