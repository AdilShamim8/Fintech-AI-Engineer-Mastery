# Phase 04 — Financial Mathematics

> **Stage II — Data & Quant Core** · **Duration: 4-5 weeks** · **Mastery target: Fluency → Application**
> **Position in path:** `03-financial-data-engineering` ← **this phase** → `05-statistics-econometrics`

## 1. Objective

This phase rebuilds your mathematical substrate around the objects finance actually prices and risks: cash flows, bonds, portfolios, and stochastic processes. You already know the ML; here you learn the quantitative grammar that decides whether your models mean anything — time value of money, duration and convexity, lognormal and fat-tailed returns, copulas, Itô calculus, Monte Carlo, and the portfolio math that turns predictions into positions. Every core concept is framed by where AI engineering touches it, so that by the end you can implement, verify, and defend quantitative models rather than consume them as black boxes.

## 2. Why It Matters in Finance

Machine learning sits on top of financial arithmetic, not instead of it. A fraud score feeds a decision priced in basis points; a credit PD feeds an amortization schedule; a return forecast feeds a covariance matrix and an optimizer. Engineers who lack this layer ship models that are silently wrong in ways no ML metric can catch — annualizing volatility incorrectly, mis-signing a duration hedge, or simulating returns that violate the lognormal structure options markets are priced against.

- Pricing: bonds, loans, and derivatives are deterministic transforms of a few curves — if you cannot reproduce them, you cannot validate any vendor or front-office number you are asked to ML-ify.
- Risk: VaR/CVaR, duration, and tail dependence are the vocabulary of every risk committee; AI outputs must translate into that vocabulary to be actionable.
- Portfolio construction: predictions become weights through constrained quadratic optimization — get the optimization wrong and alpha evaporates into turnover and constraint violations.
- Model validation: SR 11-7-style scrutiny assumes you can re-derive a model's math independently; that capability starts here.
- Interviews for quant-adjacent AI roles probe exactly this layer — it is the cheapest credible signal separating fintech engineers from generalists.

## 3. Prerequisites

- [ ] Phase 01 — instruments: bonds, loans, equities, derivatives at a conceptual level
- [ ] Phase 03 — time-indexed financial data handling (FRED, OHLCV series)
- [ ] Calculus and linear algebra at working level (derivatives, matrix operations, eigenvalues)
- [ ] Probability fundamentals (random variables, expectation, variance)
- [ ] Existing skill: numpy vectorization, matplotlib, Jupyter workflows (assumed known)

## 4. Learning Outcomes

- I can build amortization schedules and value annuities, loans, and cash-flow strips from first principles, verified against QuantLib.
- I can price bonds off a yield curve and compute yield-to-maturity, duration, convexity, and DV01.
- I can explain and correct the classic NPV/IRR pitfalls (multiple roots, scale-blindness, reinvestment assumptions).
- I can apply conditional probability and Bayes updating to fraud and risk priors with concrete base-rate arithmetic.
- I can choose and justify return distributions: lognormal price structure, Student-t tails, Poisson arrivals for defaults and claims.
- I can quantify fat tails (skew, kurtosis) and state their consequences for VaR and backtests.
- I can simulate correlated defaults with Gaussian and Student-t copulas and demonstrate the tail-dependence difference.
- I can run PCA on yield-curve history and interpret level/slope/curvature factors.
- I can formulate and solve portfolio problems as convex QP in cvxpy with realistic constraints.
- I can simulate GBM, explain Itô-calculus intuition, and apply variance reduction with measured speedups.
- I can compute VaR/CVaR three ways (parametric, historical, Monte Carlo) and articulate each method's failure modes.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 04.1 | Time value of money: annuities & amortization | Discounting, annuity formulas, schedule mechanics | amortization schedule generator |
| 04.2 | Bond math: price, yield, duration, convexity | Price/yield duality, risk approximations | bond analytics module |
| 04.3 | NPV, IRR & their pitfalls | Capital budgeting, IRR pathologies | project-economics worksheet |
| 04.4 | Probability for finance: conditional & Bayes | Conditional probability, prior updating | fraud-prior Bayes notebook |
| 04.5 | Return distributions | Lognormal, fat tails, Student-t, Poisson/exponential | distribution-fit study on real returns |
| 04.6 | Moments, skew & kurtosis | Beyond mean/variance; tail interpretation | cross-asset moments report |
| 04.7 | Correlation vs dependence: copulas | Tail dependence, Gaussian vs t copulas | copula joint-default simulator |
| 04.8 | Linear algebra for finance | Covariance matrices, PCA on yield curves | yield-curve factor notebook (FRED) |
| 04.9 | Optimization: convexity, QP, Lagrange | Convex analysis, quadratic programming | cvxpy optimization lab |
| 04.10 | Stochastic processes | Random walk → GBM, Itô intuition, Poisson processes | GBM + jump simulation lab |
| 04.11 | Monte Carlo & variance reduction | MC error, antithetic/control variates, Sobol | MC pricer with convergence study |
| 04.12 | Numerical methods | Newton/bisection, implied vol, Euler-Maruyama | implied-vol solver + SDE integrator |
| 04.13 | VaR & CVaR mathematics | Quantile risk measures, expected shortfall | multi-method VaR/CVaR engine |
| 04.14 | Portfolio math | MPT, CAPM, beta, IR, tracking error | frontier + attribution toolkit |

**04.1 Time value of money, annuities & amortization.** *What it is:* the principle that cash has a time-dependent price; discounting converts future cash flows to present value, and annuity/amortization formulas aggregate level payment streams into closed forms. *Why finance uses it:* every loan, bond, mortgage, and lease is a discounted cash-flow object; amortization schedules drive servicing, prepayment, and collections operations. *Where AI uses it:* payment schedules become features and targets — remaining-balance curves, prepayment flags, interest-split features — and loss forecasting consumes schedule arithmetic directly. *Cost of not knowing it:* you cannot sanity-check an APR claim, audit a pricing model, or engineer loan features without mis-modeling the cash-flow object the entire lending business runs on.

**04.2 Bond math: price, yield, duration & convexity.** *What it is:* a bond's price is the discounted value of coupons and principal; yield-to-maturity is the rate equating the two; duration measures price sensitivity to yield shifts and convexity adds the second-order correction. *Why finance uses it:* the bond market dwarfs equity markets, and duration/convexity are how banks, insurers, and pension funds measure and hedge interest-rate risk — 2022's UK LDI stress was, as widely reported, at its core a duration-matching failure. *Where AI uses it:* yield curves and duration buckets are standard model features, and formula-driven rate shocks generate scenario labels for ML models. *Cost of not knowing it:* you will misinterpret rate-driven P&L, build features linearly redundant with duration, and be unable to validate any fixed-income model you are handed.

**04.3 NPV, IRR & their pitfalls.** *What it is:* NPV discounts a project's cash flows at a hurdle rate; IRR is the rate that makes NPV zero. *Why finance uses it:* capital allocation everywhere — lending, infrastructure, fintech product bets — is argued in NPV/IRR terms. *Where AI uses it:* expected values from ML models feed NPV-style decision frames (customer lifetime value, collections strategy) and experiment ROI reviews. *Cost of not knowing it:* you will fall for IRR's classic traps — multiple roots on sign-flipping cash flows, scale-blindness, the implicit reinvestment assumption — and propagate them into automated decisions.

**04.4 Probability for finance: conditional probability & Bayes.** *What it is:* probability revised as evidence arrives; Bayes' theorem formalizes the inversion from effect back to cause. *Why finance uses it:* base rates are brutal — fraud is rare, defaults are rare — and every decision is conditional on noisy signals. *Where AI uses it:* Bayes is the skeleton under probability calibration, naive-Bayes baselines, Bayesian A/B analysis (Phase 05), and the correct mental model for precision at low prevalence. *Cost of not knowing it:* you will misread a "95% accurate" detector on a 0.1%-prevalence population as useful — the base-rate fallacy, with real money attached.

**04.5 Return distributions: lognormal, fat tails, Student-t, Poisson & exponential.** *What it is:* prices are positive, so prices (not returns) are modeled as lognormal; empirically tails are heavier than Gaussian, motivating Student-t; defaults and insurance claims are counts (Poisson) with waiting times (exponential). *Why finance uses it:* option prices embed lognormal assumptions; risk models that understate tails break exactly when needed; arrival processes govern operational and credit events. *Where AI uses it:* input distributions determine simulation labels, anomaly thresholds, and loss-function choices; Poisson regression predicts defaults and claim counts. *Cost of not knowing it:* your Monte Carlo scenarios, VaR numbers, and anomaly alerts will be calibrated to a Gaussian world that does not exist.

**04.6 Moments, skew & kurtosis.** *What it is:* mean, variance, skewness, and kurtosis summarize distribution shape; excess kurtosis quantifies tail weight relative to the Gaussian. *Why finance uses it:* asset returns are skewed and leptokurtic — crashes are more frequent and worse than normal models admit; this is established empirical knowledge, not a fashion. *Where AI uses it:* moment features feed regime detection, and estimator instability in fat tails dictates robust statistics for feature engineering. *Cost of not knowing it:* you will summarize assets by mean/variance alone, understate risk, and be surprised by precisely the events risk systems exist for.

**04.7 Correlation vs dependence: copulas.** *What it is:* correlation measures linear co-movement only; dependence is the full joint structure; copulas separate marginal distributions from the dependence function, with tail dependence measuring extreme co-movement. *Why finance uses it:* correlations converge toward 1 in crises, and the Gaussian copula's zero tail dependence is widely reported to have contributed to pre-2008 CDO mispricing. *Where AI uses it:* synthetic data generation, stress-scenario design, and joint-default simulation all require copulas; "correlation" features that hide dependence structure mislead models. *Cost of not knowing it:* your portfolio risk models will be precisely wrong in the tail — the only region that ends careers and firms.

**04.8 Linear algebra for finance: covariance matrices & PCA.** *What it is:* the covariance matrix encodes joint variability; eigendecomposition (PCA) finds orthogonal directions of maximum variance. *Why finance uses it:* portfolio risk is a quadratic form in the covariance matrix, and yield-curve movements concentrate in level/slope/curvature factors that PCA recovers empirically (Litterman-Scheinkman-style factor analysis is established practice). *Where AI uses it:* PCA is dimensionality reduction for correlated features, whitening, and latent-factor construction; PSD repair of noisy covariance matrices is a real production task. *Cost of not knowing it:* you will feed ill-conditioned covariance matrices into optimizers, get wild weights, and blame the optimizer.

**04.9 Optimization: convexity, quadratic programming & Lagrange multipliers.** *What it is:* convex problems have unique global optima; QP minimizes quadratic objectives under linear constraints; Lagrange multipliers and KKT conditions characterize constrained optima. *Why finance uses it:* mean-variance portfolios are QPs; risk parity, tracking-error limits, and transaction-cost penalties are convex formulations the industry solves daily. *Where AI uses it:* cvxpy-style modeling layers, constrained learning, and resource allocation under regulatory limits; Lagrange intuition also explains why regularization equals constrained MLE. *Cost of not knowing it:* you will reach for generic gradient descent on problems with exact solvers, or accept "optimal" weights without realizing the constraint set — not the model — drove them.

**04.10 Stochastic processes: random walks, Brownian motion, GBM, Itô intuition, Poisson processes.** *What it is:* random walks scale to Brownian motion; geometric Brownian motion (GBM) is the multiplicative process underlying Black-Scholes; Itô's lemma is the chain rule for stochastic calculus; Poisson processes model jumps and arrivals. *Why finance uses it:* GBM is the workhorse of derivatives pricing; jump-diffusions and Poisson default arrivals extend it toward realism — established theory with half a century of market use. *Where AI uses it:* SDE simulators generate training and scenario data, and research areas like deep hedging and neural SDEs build directly on this vocabulary. *Cost of not knowing it:* you cannot simulate a stock, a rate, or a default arrival correctly, and pricing conversations will stall at "why does (dW)² = dt?".

**04.11 Monte Carlo methods & variance reduction.** *What it is:* estimating expectations by simulated sampling; standard error shrinks as 1/√N; antithetic and control variates and quasi-random (Sobol) sequences reduce variance without more paths. *Why finance uses it:* many payoffs and risk measures have no closed form, so Monte Carlo is the industry's fallback estimator — and its error bars are a compliance matter, not a courtesy. *Where AI uses it:* MC underlies simulation-based evaluation, Bayesian posterior sampling, and RL evaluation; variance-reduction thinking transfers directly to experiment power. *Cost of not knowing it:* you will report six significant figures from a thousand paths and ship decisions on noise.

**04.12 Numerical methods: root finding & Euler-Maruyama.** *What it is:* Newton-Raphson and bisection solve nonlinear equations; Euler-Maruyama discretizes SDEs for simulation. *Why finance uses it:* implied volatility has no closed-form inverse — Newton on Black-Scholes is the canonical rite of passage — and SDE simulation powers pricing and risk systems. *Where AI uses it:* every solver-based calibration loop (fitting implied parameters to market data) is Newton or a cousin, and numerical-stability discipline transfers to model training. *Cost of not knowing it:* your solvers will diverge on bad seeds, and your SDE paths will carry discretization bias you never measured.

**04.13 VaR & CVaR mathematics.** *What it is:* VaR is a quantile of the loss distribution; CVaR (expected shortfall) is the conditional mean loss beyond it; estimation is parametric, historical, or Monte Carlo. *Why finance uses it:* VaR is the board-level and regulatory risk currency (market-risk capital frameworks build on it), while CVaR is coherent (subadditive) and increasingly preferred — established practice. *Where AI uses it:* ML feeds VaR inputs (volatility forecasts, scenario generators), and CVaR appears as a risk-aware training objective. *Cost of not knowing it:* you will present VaR as "maximum loss" — it is not, it says nothing about the tail beyond the quantile — and misconfigure the risk systems your models feed.

**04.14 Portfolio math: MPT, efficient frontier, Sharpe, CAPM, beta, information ratio, tracking error.** *What it is:* mean-variance optimization traces the efficient frontier; the Sharpe ratio normalizes excess return by volatility; CAPM prices systematic risk via beta; information ratio and tracking error govern active management versus a benchmark. *Why finance uses it:* this is the shared language of asset managers, robo-advisors, and treasury teams — even credit and payments roles meet it in concentration and RAROC discussions. *Where AI uses it:* return forecasts become frontier positions; factor models and beta estimation are regression tasks you already know, now with financial meaning. *Cost of not knowing it:* you cannot translate an ML signal into an investable, constraint-respecting portfolio — the last mile where model value is realized or destroyed.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Discounting & exponential growth | Present value via compound rates and e^(-rt) | Every asset is a discounted cash-flow object | You misprice and mis-compare anything with a time axis |
| Duration & convexity | First/second-order price sensitivity to yields | The standard interest-rate risk lens | Rate P&L and hedges stay opaque to you |
| Lognormal & heavy-tailed returns | Multiplicative price dynamics; Student-t tails | Matches positivity and empirical tails | Simulations and VaR underestimate catastrophe |
| Copulas & tail dependence | Margins and dependence structure, modeled separately | Joint defaults co-move in crises | Portfolio risk breaks exactly in the tail |
| Eigendecomposition & PCA | Orthogonal variance directions of a covariance matrix | Yield-curve factors; covariance cleaning | Correlated-feature models and optimizers misbehave |
| Convexity & QP | Global optima of quadratic objectives under linear constraints | Portfolio construction is convex QP | You approximate what has exact, reliable solvers |
| GBM & Itô's lemma | Stochastic chain rule for diffusions | Black-Scholes and scenario engines | Derivatives math and simulation bias stay mysterious |
| Monte Carlo error & variance reduction | 1/√N convergence; antithetic/control/Sobol | Estimator of last resort for pricing and risk | You report noise with false precision |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Vectorized numerics | Bond ladders, MC paths, and covariance math should never be Python loops; broadcasting is the idiom |
| Floating-point & day-count discipline | Actual/365 vs 30/360 conventions and float artifacts move prices by basis points that matter |
| Unit tests against closed forms | Annuity, bond, and GBM formulas have known values — test code against them before trusting it |
| Simulator validation | Seeds, convergence tables, and moment checks are how stochastic code earns trust |
| QuantLib vs hand-rolled | Know what the library does (day counts, calendars, conventions) before letting it answer for you |
| JIT performance (numba) | Path-dependent simulation at scale needs compiled inner loops |
| Reproducible stochastic research | Pinned seeds plus recorded convergence studies make results auditable (Phases 16-17 extend this) |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| numpy | Vectorized simulation, linear algebra, covariance/PCA |
| scipy | Root finding (brentq/newton), optimization, distribution fitting |
| cvxpy | Disciplined convex modeling: QP frontiers, risk parity, PSD problems |
| QuantLib-Python | Industry-grade bond/curve/option primitives to cross-check hand-rolled math |
| statsmodels | Regression and time-series plumbing feeding Phase 05 |
| numba | JIT-compiling simulation inner loops |
| pandas / polars | Panel and time-indexed data wrangling |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| QuantLib-Python documentation and examples | Docs | Intermediate | Pricing primitives | Ground truth for bond/curve/option conventions | Essential |
| Basel framework — risk measurement sections (BIS, bis.org) | Standard | Advanced | Risk math | Origin of VaR/CVaR and capital vocabulary | Reference |
| FRED — Federal Reserve Economic Data (fred.stlouisfed.org) | Data | All | Yield curves, rates | Canonical free source for the curve history you will PCA | Essential |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| David G. Luenberger, *Investment Science* (Oxford University Press) | Book | Intermediate | Financial math core | Best single bridge from mathematics to investment decisions | Essential |
| John C. Hull, *Options, Futures, and Other Derivatives* — selected chapters (bond pricing, Wiener processes, VaR) | Book | Intermediate | Derivatives & risk math | The market-standard reference; selected chapters suffice here | Essential |
| Capinski & Zastawniak, *Mathematics for Finance* (Springer) | Book | Intermediate | Foundations | Rigorous but compact TVM, annuities, and portfolio math | Recommended |
| MIT OCW 18.S096, *Topics in Mathematics with Applications in Finance* | Course | Intermediate | Broad | Free lectures connecting exactly these topics to practice | Recommended |
| McNeil, Frey & Embrechts, *Quantitative Risk Management* (Princeton) | Book | Advanced | Risk math | The specialist reference for VaR/CVaR, copulas, extreme value theory | Reference |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Paul Wilmott, *Frequently Asked Questions in Quantitative Finance* | Book | Intermediate | Intuition | Fast, opinionated answers to exactly the interview questions below | Recommended |
| QuantStart articles (quantstart.com) | Blog | Intermediate | Implementation | Practical Python implementations of pricing and simulation | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| 3Blue1Brown — linear algebra & calculus series (YouTube) | Video | Refresher | Intuition | Visual re-anchoring of eigenvectors and chain-rule thinking | Optional |
| Khan Academy — finance & capital markets track | Course | Refresher | TVM, bonds | Quick gap-closing on time-value mechanics | Optional |

## 10. Practical Exercises

1. - [ ] Build an amortization schedule generator (level payment; actual/365 vs 30/360) for a 30-year mortgage; verify every row against QuantLib's schedule and cashflow objects.
2. - [ ] Price a 10-year semiannual coupon bond off a fitted yield curve; compute YTM by bisection and Newton; compare duration/convexity P&L approximations vs full repricing under ±200bp shocks.
3. - [ ] Solve implied volatility from 5 real option quotes with Newton and with `scipy.optimize.brentq`; plot convergence from good and bad seeds; document failure modes.
4. - [ ] Simulate 100k GBM paths; produce a convergence table (RMSE vs N) for a European call price under plain MC, antithetic variates, and a Sobol sequence; report effective speedups.
5. - [ ] Pull 5+ years of daily Treasury yields (3M/2Y/10Y) from FRED; run PCA; write half a page interpreting PC1/PC2/PC3 as level/slope/curvature with named market episodes as evidence.
6. - [ ] Fit normal vs Student-t to daily S&P 500 log returns; overlay log-scale QQ plots; compute 99% and 99.9% VaR under both and quantify the gap.
7. - [ ] Build the Gaussian vs Student-t copula default simulator (mini project M3 groundwork); show joint-default probability differences as marginal PD rises.
8. - [ ] Bayes drill: given 0.2% fraud prevalence and a detector with 95% TPR / 2% FPR, compute posterior fraud probability per alert; extend to two rounds of evidence.
9. - [ ] Solve max-Sharpe and min-variance portfolios in cvxpy with long-only, turnover, and sector-cap constraints; trace how weights shift as each constraint tightens.
10. - [ ] Integrate GBM with Euler-Maruyama at three step sizes; compare terminal-distribution moments against the exact lognormal solution; plot the discretization bias.

## 11. Mini Projects

**M1 — GBM Monte Carlo option pricer with convergence study.** Black-Scholes closed form as ground truth; European call/put pricing with plain MC, antithetic, control variate, and Sobol. Deliverable: convergence table (paths vs RMSE) plus a README defending the variance-reduction choice. Difficulty: ★★☆☆☆.

**M2 — Duration/convexity bond risk calculator.** Input: bond terms plus a curve-shift scenario; output: predicted P&L via duration/convexity vs realized full-reprice P&L, with an error report across shock sizes. Deliverable: package + validation notebook. Difficulty: ★★☆☆☆.

**M3 — Gaussian vs t-copula joint-default simulator.** 50-credit portfolio with specified marginal PDs; simulate joint defaults under both copulas; show the tail-dependence effect on 99.9% loss quantiles side by side. Deliverable: simulator + tail-comparison report. Difficulty: ★★★☆☆.

**M4 — PCA on FRED yield-curve history.** 10+ years of daily Treasury curve from FRED; eigenvalues, loadings, and PC-score time series; interpret level/slope/curvature and annotate known rate episodes. Deliverable: notebook + written factor interpretation. Difficulty: ★★★☆☆.

**M5 — Efficient frontier optimizer with out-of-sample check.** Universe of ~20 liquid ETFs; cvxpy QP with realistic constraints; roll the frontier forward monthly and compare in-sample vs out-of-sample Sharpe degradation. Deliverable: optimizer module + honest evaluation report. Difficulty: ★★★☆☆.

## 12. Major Project Hook

This phase culminates in a personal `quant-toolkit` package (pricing, risk, and simulation modules with tests) that Phase 05's statistical layer, Phase 09's forecasting work, and Phase 10 (Quantitative Finance for AI Engineers) all extend; it is also the entry ticket to quantitative work in `/projects/flagship/` (see `/projects/flagship/README.md`).

## 13. Case Studies & Industry Examples

- **The Gaussian copula and pre-2008 CDO pricing**: David X. Li's 2000 one-factor Gaussian copula became the market-standard correlation model for structured credit; its thin tails are widely reported to have understated joint-default risk before the crisis — the canonical cautionary tale for lesson 04.07 (see `/case-studies/README.md`).
- **LTCM (1998)**: Nobel-calibrated models, heavy leverage, and tail events beyond historical correlations; widely documented as the textbook fat-tail-plus-correlation-break failure.
- **UK LDI crisis (2022)**: liability-driven investment strategies in gilts faced margin spirals when yields repriced; publicly reported as a duration-matching and liquidity-stress failure at pension-fund scale.
- **February 2018 "Volmageddon"**: the XIV short-volatility ETN lost most of its value in one session when VIX spiked — a live demonstration that Gaussian, continuous assumptions underestimate jumps in volatility products.

## 14. Interview Questions

**Explain duration to a non-quant.** Duration is the approximate percent price change of a bond for a 1% move in interest rates — think "how bouncy is this bond": a 7-year duration bond loses roughly 7% if rates rise 1%. Convexity is the correction that improves that estimate for large moves.

**Why do we model prices as lognormal rather than normal?** Prices cannot go negative, and compounding is multiplicative — log-returns add across time, so prices are lognormal. It also matches the right-skewed shape of long-horizon outcomes.

**What are VaR's key limitations?** It is a quantile: it says nothing about how bad losses beyond it are, it is not subadditive (diversification can appear to increase risk), and it is highly estimator-sensitive in fat tails. CVaR fixes coherence and tail-blindness but still depends on estimation quality.

**When does correlation lie?** Correlation summarizes only linear co-movement; it misses tail dependence, is unstable across regimes, and spikes toward 1 in crises. Two variables can have zero correlation yet extreme joint-tail dependence — which is exactly what portfolio risk cares about.

**How does Bayes updating appear in fraud work?** Start from the fraud base rate (prior), update with each signal's likelihood ratio (device match, velocity anomaly), and land at a posterior that drives the decision — it is also the arithmetic behind "why 95% accuracy is useless at 0.1% prevalence."

**What breaks with a Gaussian copula for joint defaults?** Zero tail dependence: extreme co-movements are underrepresented, so portfolio loss tails are too thin — precisely the failure widely reported in pre-2008 structured credit.

**Why quadratic programming for portfolios?** Variance is quadratic in weights and typical constraints (budget, bounds, turnover) are linear, so mean-variance is a convex QP with a unique global optimum, solvable reliably at scale — no local-minima ambiguity.

**How do you cut Monte Carlo error without more paths?** Antithetic variates (paired opposites), control variates (known-expectation siblings), importance sampling for rare events, and quasi-random Sobol sequences that fill the space more evenly than pseudo-random draws.

**Give the intuition for Itô's lemma.** Brownian paths are so jagged that (dW)² behaves like dt, so the ordinary chain rule picks up an extra second-order term: f(S+dS) changes not only linearly in dS but through the convexity of f against randomness.

**Why are fat tails a problem for backtests?** Historical samples undersample extreme moves, so backtest-based risk estimates are optimistic exactly in the states that matter; tail estimators have huge variance, and kurtosis estimates need enormous samples to stabilize.

**What is the difference between return volatility and price volatility in GBM?** GBM makes log-returns conditionally normal with volatility scaling with √t, while price uncertainty scales with the price level itself — percentage volatility, absolute dispersion growing over time.

## 15. Assessment — Can You Pass the Bar?

- [ ] Build an amortization schedule and price a bond off a curve, verified against QuantLib, without looking anything up.
- [ ] Implement Newton's method for implied vol, including divergence handling, and explain seed sensitivity.
- [ ] Produce a Monte Carlo convergence study with two variance-reduction techniques and honest error bars.
- [ ] Demonstrate Gaussian vs t-copula tail differences numerically and explain the pre-2008 connection.
- [ ] Interpret a yield-curve PCA (level/slope/curvature) to a fixed-income trader in three minutes.
- [ ] Solve a constrained portfolio QP in cvxpy and explain each constraint's marginal effect on the frontier.
- [ ] Compute VaR/CVaR three ways on the same series and explain to a risk officer where each method breaks.
- [ ] Explain duration, convexity, and the base-rate fallacy to a non-technical stakeholder (record it).

## 16. Mastery Checkpoint

You may proceed to Phase 05 when:

1. Your `quant-toolkit` repo contains tested modules for amortization, bond analytics, copula simulation, PCA factors, VaR/CVaR, and a QP optimizer.
2. Every simulator ships with a convergence/validation artifact (table or plot) — no untested stochastic code.
3. You have recorded a 5-minute explanation of duration and VaR limitations as if to a risk committee (store under `/notes/artifacts/`).
4. You can derive — not just quote — the Bayes posterior in the fraud base-rate exercise.

Evidence: repo links, validation notebooks, recorded explanation. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Mixing simple and log returns (or arithmetic and geometric means) — the classic silent error that inflates long-horizon forecasts.
- Annualizing wrong: √252 applies to volatility of (log-)returns, not to returns themselves; scaling the mean by 252 is drift-only and often unjustified.
- Reading VaR as "maximum loss" — it is a quantile; the tail beyond it is where risk lives.
- Building covariance matrices from short samples and feeding non-PSD, noise-dominated matrices to optimizers.
- Quoting Monte Carlo precision your path count cannot support — the 1/√N law is not negotiable.
- Ignoring day-count and calendar conventions, then "debugging" basis-point mismatches against QuantLib for hours.
- Applying the ordinary chain rule in stochastic settings — once (dW)² ≠ 0, the Itô correction is not optional.

## 18. Where This Goes Next

Phase 05 puts statistics and econometrics on top of these return processes — testing, forecasting, and causality — and inherits your lognormal and fat-tail vocabulary directly. Phases 06-08 keep consuming your discounting, base-rate, and cost-of-money arithmetic in every decision model, and Phase 10 will reuse the pricing and stochastic machinery at full depth.
