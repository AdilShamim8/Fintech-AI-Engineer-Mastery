# Phase 10 — Quantitative Finance for AI Engineers

> **Stage IV — Markets & Quant** · **Duration: 5-6 weeks** · **Mastery target: Application → Production**
> **Position in path:** `09-financial-time-series` ← **this phase** → `11-financial-nlp-documents`

## 1. Objective

The goal is not to become a quant; it is to be dangerous *with* one — to speak the language of portfolio construction, risk, derivatives, and execution fluently enough to build AI systems quant teams trust. You will implement the canonical toolkit yourself: pricers with greeks, a three-method VaR engine with a Kupiec backtest, factor models, and cost-honest backtests, so that when you later bolt ML onto these pipelines you know exactly which assumptions you are stressing. This phase is the vocabulary-and-physics course beneath every markets-facing system you will build.

## 2. Why It Matters in Finance

Markets-facing AI lives inside a century of accumulated quantitative structure: signals are orthogonalized against factors, positions are sized by optimizers, risk is measured in VaR and expected shortfall, and orders are executed against microstructure with real transaction costs. An AI engineer who ignores this structure builds toys that quant teams quietly shelve; one who masters it can automate research workflows, build risk engines, and make backtesting infrastructure trustworthy. The AI-era roles — ML for alpha, deep hedging, transaction-cost modeling, generative research assistants — all sit on this substrate.

- Signal research is judged net of costs and factor exposures; without this lens, your ML "alpha" is a repackaged factor or a slippage artifact.
- Risk teams run daily VaR/stress processes; AI models entering that pipeline inherit backtesting and validation duties — a Kupiec test is the entry ticket.
- Options desks price, hedge, and quote off volatility surfaces; greeks and implied vol are prerequisites for any ML on derivatives.
- Execution costs (spread, impact, timing) routinely exceed gross alpha on realistic strategies — Almgren-Chriss thinking is not optional.
- History's blowups (LTCM, Knight Capital, the 2022 UK LDI crisis) are case studies in what happens when leverage, models, and deployment discipline fail together.

## 3. Prerequisites

- [ ] Phase 04 — compounding, present value, basic stochastic intuition
- [ ] Phase 05 — regression, hypothesis testing, multivariate statistics
- [ ] Phase 09 — return computation, walk-forward discipline, volatility forecasting
- [ ] Existing engineering skill: numpy/scipy fluency, optimization basics, API work (assumed)
- [ ] Willingness to read math-dense chapters slowly (Hull is the companion text)

## 4. Learning Outcomes

- I can construct and interpret a mean-variance efficient frontier and explain where MPT assumptions break and Black-Litterman helps.
- I can run Fama-French factor regressions and explain what beta, size, value, and momentum exposures mean for a strategy or ETF.
- I can build parametric, historical, and Monte-Carlo VaR plus expected shortfall, and validate VaR with a Kupiec backtest.
- I can price European options with Black-Scholes, American options with binomial trees, and path-dependent payoffs with Monte Carlo.
- I can compute and interpret greeks, invert an implied-vol solver, and describe the smile/skew and where Black-Scholes assumptions fail.
- I can model market impact and slippage (Almgren-Chriss intuition) and include realistic costs in backtests.
- I can explain the algo-trading stack (signal → portfolio construction → execution) and build a cost-honest backtest.
- I can run a Brinson performance attribution and compare rebalancing strategies under transaction costs.
- I can describe a daily risk-engine process (VaR, stress, scenarios, FRTB concept) well enough to build tooling for it.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 10.1 | The quant landscape | Buy-side/sell-side roles; where AI engineers add value | role/value map note |
| 10.2 | Portfolio theory | MPT, efficient frontier, diversification, Black-Litterman | frontier + BL notebook |
| 10.3 | Factor models | CAPM → Fama-French 3/5 → barra-style risk models | factor regression on ETFs |
| 10.4 | Risk measures | VaR (3 methods), ES, max drawdown; their failure modes | VaR engine |
| 10.5 | Risk validation & stress | Kupiec backtest, stress testing, scenario analysis | exception report |
| 10.6 | Fixed income analytics | Duration, convexity, OAS concept, curve construction basics | bond analytics module |
| 10.7 | Derivatives foundations | Black-Scholes, assumptions, where they break | BS pricer + notes |
| 10.8 | Greeks & the vol surface | Delta/gamma/vega/theta, implied vol, smile/skew | greeks + IV solver |
| 10.9 | Numerical pricing | Binomial trees, Monte Carlo, American options | tree + MC pricers |
| 10.10 | Hedging | Delta hedging mechanics; deep hedging as RL (research frontier) | hedging simulation |
| 10.11 | Market microstructure | Order types, LOB, spread, impact (Almgren-Chriss), TCA | impact model + FI-2010 EDA |
| 10.12 | The algo-trading stack | Signal → portfolio construction → execution; cost-honest backtests | end-to-end backtest |
| 10.13 | The quant research workflow | Alpha ideation, orthogonalization, signal decay | signal research log |
| 10.14 | Risk engines & attribution | Daily VaR process, FRTB concept, Brinson, rebalancing | attribution module + doc |

**10.1 The quant landscape.** Buy-side (asset managers, hedge funds) seeks returns; sell-side (banks) prices, hedges, and makes markets; both employ quants and increasingly AI engineers for data, ML, and platform work. Your realistic, high-value lane is building the data, backtest, risk, and execution infrastructure — plus the ML components — that quants direct. Learn the vocabulary before the math; it makes the math land faster.

**10.2 Portfolio theory.** Mean-variance optimization formalizes diversification but is notoriously input-sensitive — covariance estimation error and unconstrained corner solutions wreck it; Black-Litterman blends market-equilibrium priors with views to tame it. Build the frontier, then deliberately perturb expected returns by a percent and watch allocations explode: the most instructive hour in portfolio theory. Real optimizers carry constraints (long-only, turnover, risk limits), so add them.

**10.3 Factor models.** CAPM prices assets with one factor (market beta); Fama-French adds size and value (then profitability and investment in FF5); industry risk models in the barra style extend to hundreds of factors for risk decomposition. Regress your ETF or strategy returns on factor returns and learn what you actually own — much "alpha" is disguised factor exposure. This regression is the quant team's first question about any ML signal.

**10.4 Risk measures.** VaR answers "how bad is bad, at 95/99%?" three ways: parametric (variance-covariance), historical (empirical quantiles), Monte Carlo (simulated); expected shortfall adds the conditional tail mean VaR lacks; max drawdown measures realized pain. Each method encodes different assumptions, and knowing when historical VaR lies (fat tails, regime change, short samples) is the actual skill.

**10.5 Risk validation & stress.** Kupiec's proportion-of-failures test checks whether VaR exceptions occur at the promised rate — the minimum honesty check for any VaR model and an interview staple. Stress testing asks scenario questions ("rates +300bp, equities -20%") that history may not contain; FRTB (concept level) pushes banks toward ES-based, liquidity-horizon-aware capital. Build the exception report a risk committee would actually read.

**10.6 Fixed income analytics.** Duration (rate sensitivity) and convexity (its curvature) fall out of Taylor-expanding bond prices; the OAS concept spreads credit and option risk over the curve; curve construction (bootstrapping discount factors from instruments) underlies everything from pricing to liability-driven investing. The 2022 UK LDI crisis is the case study for why leveraged duration matters. Implement the analytics — they are simple and everywhere.

**10.7 Derivatives foundations.** Black-Scholes prices European options under continuous hedging, lognormal prices, and constant rates and volatility — assumptions that break visibly in the implied-vol smile. You must know the model's intuition, its greeks, and precisely where it fails, because every "better" model is defined relative to it. Implement it before you import it.

**10.8 Greeks & the vol surface.** Delta, gamma, vega, and theta are the risk coordinates of any derivative book; implied volatility inverts the pricing model to read the market's opinion. The smile and skew (index put skew especially) encode crash fear and falsify the constant-vol assumption. Build an implied-vol solver (bisection/Newton) and plot a surface from real option quotes available via free data sources.

**10.9 Numerical pricing.** Binomial trees handle American exercise by backward induction with a continuation-vs-exercise test at every node; Monte Carlo handles path dependence via simulation, with American exercise requiring tricks (Longstaff-Schwartz concept). Learn convergence behavior and basic variance reduction (antithetic variates). These two engines cover most pricing problems you will ever meet.

**10.10 Hedging.** Delta hedging rebalances to neutralize directional risk — mechanically simple, practically governed by the trade-off between rebalance frequency (gamma risk) and transaction costs. Deep hedging — reinforcement-learned hedging policies under costs and risk measures — is an active research frontier: know it exists, know the papers, and do not claim it is standard desk practice.

**10.11 Market microstructure.** Limit order books, order types, bid-ask spread, and queue mechanics determine what execution actually costs; market impact models (Almgren-Chriss optimal execution, square-root-law intuition) turn order size into price movement. FI-2010 gives you a real limit-order-book dataset to explore. Transaction cost analysis (TCA) is where AI engineers increasingly add measurable, monetizable value.

**10.12 The algo-trading stack.** Signal → portfolio construction (sizing, risk limits) → execution (scheduling, venue, order types) — each stage consumes the previous and each has its own models. Backtesting infrastructure must model costs, slippage, funding, and capacity honestly; vectorized engines (vectorbt) trade realism for speed, event-driven ones (backtrader) the reverse. Most published backtests lie; your job is to build ones that do not.

**10.13 The quant research workflow.** Alpha ideation starts from economic rationale, not data mining; candidate signals are orthogonalized against known factors, tested under multiple-hypothesis discipline, and expected to decay as they get crowded. This workflow is exactly what AI can accelerate — idea generation, code generation, result summarization — and exactly what naive ML breaks via selection bias. Keep a research log that would survive a quant's review.

**10.14 Risk engines & attribution.** A daily risk process runs: data cut → valuation → VaR/ES → stress and scenarios → exception escalation → reporting; FRTB (concept level) reshapes capital around ES and liquidity horizons. Brinson attribution decomposes portfolio-vs-benchmark performance into allocation and selection effects; rebalancing strategies (calendar, threshold, risk-parity-style) trade transaction costs against drift. Build the attribution code once and reuse it forever.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Mean-variance optimization | Quadratic program over expected returns and covariance | The portfolio-construction baseline everywhere | You cannot follow a single portfolio conversation |
| Factor regression | OLS of returns on factor portfolios | Risk decomposition; alpha-vs-beta adjudication | Your "alpha" is undisclosed factor exposure |
| VaR / expected shortfall | Loss quantile; conditional tail expectation | Regulatory and internal risk limits | You conflate "likely loss" with "tail loss" |
| Duration & convexity | First/second price sensitivity to yield | Bond and LDI risk management at scale | Fixed income stays mysterious; LDI-style crises unreadable |
| Black-Scholes & greeks | Closed-form option price plus risk sensitivities | Derivatives quoting, hedging, greeks limits | Derivatives teams cannot use anything you build |
| Binomial backward induction | Dynamic programming on a lattice | American (early-exercise) option pricing | You apply Black-Scholes where it is provably wrong |
| Almgren-Chriss impact model | Optimal execution under temporary/permanent impact | Execution scheduling, TCA, capacity analysis | Your backtests assume infinite liquidity |
| Brinson attribution | Allocation/selection decomposition vs benchmark | Performance accountability to clients and boards | You cannot explain why performance happened |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Backtesting infrastructure | Cost models, corporate actions, and survivorship-free data separate a backtest from a fantasy |
| Numerical correctness | Pricers and solvers need convergence tests, boundary checks, and known-answer tests |
| Optimization with constraints | cvxpy-style disciplined optimization; solvers fail silently on bad inputs |
| Market data pipelines | Corporate actions, adjusted vs raw prices, currency and calendar handling (Phase 03 at market speed) |
| Research reproducibility | Pin data, seeds, and versions; a quant will re-run your notebook and diff your numbers |
| Instrument identifiers | ISIN/LEI/ticker mapping and entity resolution bite hard in multi-source market data |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| QuantLib-Python | Industrial fixed-income and derivatives analytics (curves, pricers, calendars) |
| vectorbt | Fast vectorized backtesting for research iteration |
| backtrader | Event-driven backtesting when order-level realism matters |
| cvxpy | Disciplined portfolio optimization with constraints |
| statsmodels | Factor regressions, cointegration tests, diagnostics |
| yfinance / OpenBB | Market data and fundamentals access for experiments |
| scipy / numpy | Numerical pricing, solvers, simulation |
| quantstats / empyrical | Performance and risk reporting metrics |
| FI-2010 dataset | Real limit-order-book data for microstructure exploration |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| QuantLib documentation & examples | Docs | Advanced | Pricing/analytics | The open-source reference implementation of quant analytics | Essential |
| Ken French Data Library | Data | Intermediate | Factors | The canonical free factor-return dataset for regressions | Essential |
| CFA Institute curriculum overviews (public summaries) | Reference | Intermediate | Investment breadth | Structured map of portfolio/risk/fixed-income vocabulary | Reference |
| BIS publications on FRTB and market risk | Standard | Advanced | Regulation | Concept-level capital framing for risk engines | Optional |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| John C. Hull, *Options, Futures, and Other Derivatives* | Book | Intermediate | Derivatives | The standard textbook; keep it as your desk reference | Essential |
| David Luenberger, *Investment Science* | Book | Intermediate | Portfolio/valuation | The cleanest treatment of portfolio theory and NPV thinking | Recommended |
| Larry Harris, *Trading and Exchanges* (Oxford) | Book | Intermediate | Microstructure | The market-structure bible behind execution and TCA | Recommended |
| Marcos López de Prado, *Advances in Financial Machine Learning* (Wiley 2018) | Book | Advanced | ML + quant | Where your AI skills plug into quant research honestly | Essential |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Rishi Narang, *Inside the Black Box* (Wiley) | Book | Beginner | Algo trading | Plain-language map of the quant trading stack | Recommended |
| Sheldon Natenberg, *Option Volatility and Pricing* | Book | Intermediate | Volatility | Trader-grade intuition for vol, greeks, and smiles | Recommended |
| QuantConnect Bootcamp (free) | Course | Intermediate | Backtesting | Hands-on algorithm labs with realistic data | Optional |
| vectorbt documentation & examples | Docs | Intermediate | Backtesting | The fastest research-loop backtester in Python | Essential |
| Cartea, Jaimungal & Penalva, *Algorithmic and High-Frequency Trading* | Book | Advanced | Execution | Rigorous market-making and optimal-execution models (specialist) | Reference |
| Carol Alexander, *Market Risk Analysis* (vols I-IV) | Book | Advanced | Market risk | Deep, rigorous risk-analytics reference (specialist) | Reference |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| QuantConnect community forums & algorithm library | Forum | Intermediate | Practice | Searchable real implementation discussions | Reference |
| OpenBB documentation | Docs | Beginner | Data access | Terminal-style data workflows for experiments | Optional |

## 10. Practical Exercises

1. - [ ] Implement Black-Scholes with closed-form greeks; verify against QuantLib; write known-answer tests (deep ITM, deep OTM, long maturity).
2. - [ ] Build an implied-vol solver (bisection + Newton) and invert prices for a synthetic surface; plot the smile you create and one from real ETF option quotes.
3. - [ ] Price an American put with a CRR binomial tree; show convergence as steps increase; compare against the European price to visualize the early-exercise premium.
4. - [ ] Price an Asian option with Monte Carlo; add antithetic variates; quantify variance reduction vs step count.
5. - [ ] Build the three-method VaR engine (parametric/historical/MC) on five years of ETF returns; run the Kupiec test at 95/99%; write the exception report.
6. - [ ] Regress five sector ETFs on Ken French FF3 (+ momentum) returns; interpret the betas; explain which "active" fund is really a factor sleeve.
7. - [ ] Construct an efficient frontier for 10 assets; perturb expected returns by ±1%; document allocation instability; add long-only and turnover constraints and repeat.
8. - [ ] Implement a pairs-trading backtest (Engle-Granger cointegration, z-score entries) with spread, commission, and borrow costs; report net-of-cost honesty.
9. - [ ] Walk-forward 12-1 momentum across sectors with a slippage model (half-spread + square-root impact); report gross vs net and a capacity estimate.
10. - [ ] Explore **FI-2010** limit-order-book data: spread distributions, depth snapshots, and a simple mid-price-move labeling; note microstructure-noise effects.

## 11. Mini Projects

**M1 — Options pricing toolkit.** Black-Scholes + greeks + implied-vol solver + binomial American pricer + Monte Carlo engine, with tests; one CLI/notebook entry point. Deliverable: tested library + verification notebook. Difficulty: ★★★☆☆.

**M2 — Three-method VaR engine with validation.** Real ETF returns → parametric/historical/MC VaR + ES → Kupiec backtest → stress scenarios → committee-ready exception report. Deliverable: risk-engine repo + report. Difficulty: ★★★☆☆.

**M3 — Cost-honest pairs trading.** Cointegration-based pair selection on liquid ETFs → signal + z-score exits → full cost model → net P&L, drawdown, capacity estimate. Deliverable: backtest repo with cost ablation. Difficulty: ★★★★☆.

**M4 — Walk-forward momentum with execution realism.** Cross-sector momentum, rebalancing-policy comparison, slippage model, gross-vs-net reporting. Deliverable: strategy report with an honest limitations section. Difficulty: ★★★★☆.

## 12. Major Project Hook

Phase-culminating build: a "mini quant desk" — market data pipeline → factor model → portfolio construction with cvxpy → risk engine (VaR/ES + Kupiec) → cost-honest backtest → attribution report. This substrate is what flagship market-facing projects extend (see `/projects/README.md`).

## 13. Case Studies & Industry Examples

- **2010 Flash Crash**: publicly documented microstructure breakdown — liquidity vanishing in minutes, subsequent prosecutions for spoofing — the canonical argument for studying market structure and execution (see `/case-studies/README.md`).
- **Knight Capital, 2012**: a deployment error consumed hundreds of millions of dollars within minutes, as publicly reported — the canonical argument for deployment discipline, kill switches, and idempotent order handling.
- **LTCM (1998)**: Nobel-grade models plus extreme leverage plus a correlated unwind — the canonical model-risk and leverage case.
- **2022 UK LDI crisis**: liability-driven investment strategies met rapid rate rises; leveraged duration inside pension buffers forced fire sales — fixed-income analytics with systemic consequences.

## 14. Interview Questions

**Give me greeks intuition.** Delta is your directional exposure, gamma how fast delta changes (rebalancing need), vega sensitivity to implied vol, theta time decay — a book is a vector in greeks space, and desk limits are expressed in that space.

**VaR vs expected shortfall — why do regulators prefer ES?** VaR says you will not lose more than X with 95% confidence but nothing about beyond-X; ES is the conditional mean of tail losses and is coherent (subadditive), which is why FRTB moved capital toward ES.

**Why do backtests lie?** Survivorship bias, lookahead in adjusted data, optimistic fills, ignored costs and borrows, overfit parameters, and multiple testing without shrinkage — each alone can flip P&L; honesty is an engineering property of the backtest, not a statistic.

**Explain Black-Scholes' assumptions and where they fail.** Lognormal prices with constant volatility, continuous frictionless hedging, constant rates — it fails on jumps, stochastic/smiling volatility, transaction costs, and discrete hedging; the volatility smile is the market's standing correction to the model.

**How would you model market impact for a large order?** Decompose into spread/crossing cost plus temporary and permanent impact; use square-root-law or Almgren-Chriss-style scheduling; calibrate on TCA data and validate out-of-sample — then quote a capacity number, not just a return.

**Correlation vs cointegration for pairs trading?** Correlation is co-movement of returns and can drift apart forever; cointegration is a long-run equilibrium of levels with a mean-reverting spread — the tradeable property is spread stationarity, so test cointegration, not correlation.

**Explain duration and convexity to a non-quant executive.** Duration: "if rates move 1%, the bond moves roughly minus-duration percent"; convexity is the correction that makes losses smaller on rises and gains larger on falls — leveraged duration is why 2022 hurt LDI strategies so badly.

**What is signal orthogonalization and why do quants insist on it?** Regressing a new signal on known factors (and subtracting) reveals incremental value; without it you are re-selling size/value/momentum with extra steps and paying new costs for old exposure.

**Why do signals decay?** Crowding, arbitrage capital, regime change, and publication effects erode edge; monitoring factor-level and strategy-level performance is part of the research workflow, not an afterthought.

**What does a daily VaR process look like operationally?** EOD data cut → revaluation of positions → VaR/ES by method → exception counts vs limits → stress/scenario runs → escalation and sign-off; the AI engineer's job is making this pipeline fast, correct, and auditable.

**Where does deep hedging stand today?** Research frontier: RL agents learn hedging under transaction costs and risk measures and show cost-efficiency gains in academic studies — interesting and published, not yet standard desk practice; frame it exactly that way.

**What is FRTB, conceptually?** The Basel market-risk framework that replaces VaR-97.5 with ES at 97.5% plus liquidity horizons, with a standardized-calculation fallback — know the shape and its data implications (instrument-level histories, desk structure), not the fine print.

## 15. Assessment — Can You Pass the Bar?

- [ ] Implement and verify Black-Scholes + greeks + an implied-vol solver against known answers and QuantLib.
- [ ] Build three-method VaR + ES on real returns and pass a Kupiec backtest — or correctly diagnose the failure.
- [ ] Run a factor regression and correctly identify hidden factor exposure in a "mystery" strategy.
- [ ] Price an American option with a tree and explain the early-exercise premium numerically.
- [ ] Produce a backtest with explicit cost, slippage, and capacity analysis, and explain every realism choice.
- [ ] Explain duration/convexity, VaR/ES, and the daily risk-engine flow to a non-quant risk officer.
- [ ] Write a one-page honest research memo for a strategy: rationale, orthogonalized metrics, decay plan.

## 16. Mastery Checkpoint

You may proceed to Phase 11 when:

1. Your "mini quant desk" repo runs end to end: data → factors → optimization → risk → backtest → attribution, with tests.
2. Your VaR engine produces a committee-grade exception report (Kupiec + stress) a risk officer could read without translation.
3. Your best backtest includes a net-of-costs section you would defend to a skeptical portfolio manager.
4. You can present greeks, VaR/ES, and impact modeling in a recorded 10-minute market-vocabulary walkthrough — store under `/notes/artifacts/`.

Evidence: repo links + backtest reports + recorded walkthrough. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Backtesting on survivorship-ignorant universes with split-ignorant prices — the two classic fantasy generators.
- Using unadjusted closes for returns, then wondering about "-8% Mondays" (dividend gaps and splits).
- Filling backtest orders at mid price and wondering why production fills are worse — spread and impact are not optional.
- Optimizing portfolios without constraints and presenting corner-solution allocations as a strategy.
- Treating VaR as a maximum-loss guarantee — it is a quantile under a model, validated by exception counts.
- Conflating realized (historical) and implied volatility in risk or P&L conversations — they answer different questions.
- Shipping ML signals without factor orthogonalization — the quant team's first review question and the fastest route to lost credibility.

## 18. Where This Goes Next

Phase 11 moves from market data to market *documents* — filings, earnings calls, and research text — where NLP feeds the same research workflow you just learned through sentiment factors, event extraction, and document intelligence. Phase 12 will layer generative AI onto this vocabulary.
