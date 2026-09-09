# Phase 09 — Financial Time Series & Forecasting

> **Stage III — Core Financial ML** · **Duration: 4 weeks** · **Mastery target: Application → Production**
> **Position in path:** `08-aml-financial-crime` ← **this phase** → `10-quantitative-finance`

## 1. Objective

Financial time series are the most hostile data an experienced ML engineer will ever face: the signal-to-noise ratio is brutal, distributions carry fat tails, variance clusters, and every naive pipeline quietly leaks the future into the past. In this phase you learn to compute returns correctly, validate with walk-forward discipline instead of shuffled folds, and build forecasters — classical, boosted, global, deep, and foundation-model — whose value is measured in distributions and decision utility, not point accuracy. You finish able to ship forecasting systems for liquidity, volatility, demand, and macro nowcasting that survive contact with a risk officer.

## 2. Why It Matters in Finance

Banks and trading firms run on forecasts: how much cash each ATM dispenses tomorrow, what deposit flows will do under stress, how volatile the book will be overnight, what clients will call about next week. A percentage point of forecast error converts directly into idle-cash cost, stock-outs, capital misallocation, or hedging slippage. And because financial series barely predict themselves, the discipline this phase teaches — aggressive baselines, leakage paranoia, probabilistic outputs, honest backtests — is precisely what separates professional forecasters from demo merchants.

- ATM/branch cash forecasting: every unit of error is either idle cash (funding cost) or a stock-out (customer harm plus emergency shipments).
- Liquidity and deposit-flow forecasting feeds regulatory liquidity management (Phase 01 context) — misforecasting deposit behavior is a supervisory conversation, as 2023 regional-bank stress publicly demonstrated.
- Volatility forecasts are inputs to VaR, margin, options pricing, and position limits — the GARCH lineage remains production reality.
- Call-volume and operational forecasts drive staffing economics; M5-competition methods transfer directly to bank networks.
- Macro nowcasting from FRED releases gives decision-makers "today's economy" months before official revisions settle.

## 3. Prerequisites

- [ ] Phase 03 — time-indexed data engineering, point-in-time correctness, as-of joins
- [ ] Phase 04 — expectations, compounding, basic stochastic intuition
- [ ] Phase 05 — hypothesis testing, autocorrelation, stationarity, cross-validation discipline
- [ ] Existing ML skill: gradient boosting, sklearn pipelines, hyperparameter search (assumed)
- [ ] Comfort with pandas/polars resampling and timezone handling

## 4. Learning Outcomes

- I can compute log vs simple returns correctly, align series across calendars and timezones, and resample without inventing or losing data.
- I can audit a financial feature set for lookahead bias and misaligned joins, and repair it with point-in-time logic.
- I can design anchored and rolling walk-forward validation with embargo gaps and defend the design choices.
- I can build ETS/ARIMA baselines and prove — often — that they embarrass more complex models.
- I can build a global gradient-boosting forecaster across many series with lag/rolling/calendar features.
- I can deploy zero-shot time-series foundation models (Chronos-2 / TimesFM-3 era) and judge when they beat classical baselines.
- I can produce probabilistic forecasts (quantiles, intervals) and evaluate them with pinball loss and CRPS.
- I can model volatility with GARCH-family and HAR-RV approaches and compare them via a utility backtest.
- I can build hierarchical forecasts with reconciliation, detect regimes with HMMs, and create trading labels with triple-barrier + meta-labeling.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 09.1 | Why financial TS is hostile | Nonstationarity, low SNR, fat tails, volatility clustering | stylized-facts notebook |
| 09.2 | Returns & alignment | Log vs simple, calendar/timezone alignment, resampling | return-computation audit |
| 09.3 | Leakage traps | Lookahead indicators, close-of-t decisions, misaligned joins | leakage checklist + demo |
| 09.4 | Walk-forward validation | Anchored/rolling splits, purge and embargo gaps | reusable walk-forward harness |
| 09.5 | Classical baselines | ETS/ARIMA as the bar every model must clear | baseline benchmark vs naive |
| 09.6 | Feature-based ML | Lag/rolling/calendar features + gradient boosting | LightGBM forecaster |
| 09.7 | Global models | One model across hundreds of series (Nixtla philosophy) | global-vs-local experiment |
| 09.8 | Deep TS architectures | N-BEATS/N-HiTS/PatchTST/TFT — when deep helps | honest deep-vs-GBM benchmark |
| 09.9 | TS foundation models | Chronos-2, TimesFM-3: zero-shot, current 2026 practice | zero-shot evaluation |
| 09.10 | Probabilistic forecasting | Quantile/pinball loss, CRPS, prediction intervals | calibrated quantile forecaster |
| 09.11 | Volatility modeling | GARCH family, HAR-RV, realized vol from intraday data | vol-forecast bake-off |
| 09.12 | Regimes & trading labels | HMM regime detection; triple-barrier + meta-labeling | labeling experiment on crypto |
| 09.13 | Evaluation: statistical + utility | MASE/sMAPE plus value-added backtests | forecast utility report |
| 09.14 | Hierarchies, nowcasting & banking problems | Reconciliation, FRED nowcasting, ATM/liquidity/call volume | reconciliation demo + use-case map |

**09.1 Why financial TS is hostile.** Prices are near-martingales: the best predictor of tomorrow's price is usually today's, so raw-level forecasting is a trap dressed as an opportunity. Returns are fat-tailed, variance clusters (big days follow big days), and regimes shift — these stylized facts explain why accuracy metrics mislead and why volatility is far more forecastable than direction. Every modeling choice in this phase inherits this hostility.

**09.2 Returns & alignment.** Log and simple returns diverge materially over long horizons and aggregation; picking the wrong one silently corrupts volatility estimates and portfolio math. Financial data arrives on mismatched calendars and timezones (crypto trades 24/7, equities do not), so naive joins and resampling manufacture phantom rows or stale prices. Build the audit habit: count rows per period, verify nothing is forward-filled from the future, and document every aggregation decision.

**09.3 Leakage traps.** The classic sins: computing an indicator on the full series then shifting it incorrectly, making a decision at time t using the close of time t (you cannot trade the close you just saw), joining a revised macro series as if it were published on time, and scaling with full-sample statistics. Each produces spectacular offline results and live disappointment. Make point-in-time construction — as-of joins, shift-by-one discipline, expanding-window scalers — a mechanical, unit-tested rule.

**09.4 Walk-forward validation.** Random k-fold is invalid on dependent data; the honest designs are anchored (expanding train) and rolling (fixed window) walk-forward, with a purge/embargo gap so labels straddling the boundary cannot leak. Size the gap from your forecast horizon, and report per-fold performance distributions rather than one pooled score. Build this harness once as reusable code — every subsequent phase reuses it.

**09.5 Classical baselines.** ETS and ARIMA remain shockingly hard to beat on single, well-behaved series, and seasonal-naive regularly embarrasses neural nets on strongly periodic demand data. Run them first, on every problem, and treat any complex model that cannot beat them as a cost center. `statsmodels` and Nixtla's `statsforecast` make this nearly free.

**09.6 Feature-based ML.** Lags, rolling means/standard deviations, calendar and holiday features turn forecasting into tabular ML, where gradient boosting is king. The danger is exactly the leakage patterns of lesson 09.3 — every feature must be computable strictly before the forecast origin. This is the workhorse pattern for demand, cash, and call-volume problems.

**09.7 Global models.** Fitting one model across hundreds of related series, with series identifiers as features, often beats per-series fitting because it shares strength across sparse or intermittent data — the core insight behind Nixtla's `mlforecast` and the M5 winners. Build both paradigms on the same data; global models usually win on short histories and intermittent demand.

**09.8 Deep TS architectures.** N-BEATS/N-HiTS (MLP basis decompositions), PatchTST (patched transformer), and TFT (attention with known future inputs) earn their complexity on long horizons, many-series problems, and rich covariates. On short, noisy, univariate financial series they frequently lose to boosting. Run one honest benchmark and let evidence, not fashion, decide.

**09.9 TS foundation models.** Large pretrained time-series models — Google's TimesFM lineage and Amazon's Chronos lineage, with Chronos-2 and TimesFM-3 current as of 2026 — deliver zero-shot forecasting, with multivariate support arriving. Treat them as strong new baselines: evaluate on your data, check quantile calibration, and remember that tokenization/quantization choices affect tails. "Zero-shot beats my tuned ARIMA" is now a real outcome in practice; so is the reverse.

**09.10 Probabilistic forecasting.** Finance consumes distributions: a cash forecast needs a low quantile for stock-out risk and a high one for idle-cash cost; a VaR input needs tail quantiles. Train with pinball (quantile) loss or CRPS, evaluate calibration across quantiles, and stop shipping point forecasts. Residual-based prediction intervals are a last resort, not a method.

**09.11 Volatility modeling.** Volatility clusters, which is why GARCH-family models (conditional variance as a recursive process) and HAR-RV (heterogeneous-autoregressive regression on daily/weekly/monthly realized volatility from intraday data) work. Compare both against a naive random-walk vol out of sample, and add a decision utility such as a variance-targeting position rule. This lesson feeds Phase 10's risk and derivatives work directly.

**09.12 Regimes & trading labels.** HMMs give an unsupervised read of market regimes (volatility states); triple-barrier labeling (López de Prado) converts price paths into supervised labels using profit-take, stop, and time-expiry barriers; meta-labeling layers a filter model on a primary signal to decide position sizing. Both fix the naivety of fixed-horizon up/down labels. Practice on Kraken/Binance crypto OHLCV, where data is free and runs 24/7.

**09.13 Evaluation: statistical + utility.** MASE and sMAPE compare honestly against naive baselines across scales; RMSE alone flatters models that nail the easy middle. Then add financial utility: run the forecast through the decision it feeds (cash order, hedge ratio, VaR limit) and report the cost or P&L delta. A forecast that improves MASE but loses money in the backtest is a failed forecast.

**09.14 Hierarchies, nowcasting & banking problems.** Bank networks forecast hierarchically — ATM → region → country, product → branch — and independent bottom-level forecasts rarely sum to top-level plans; reconciliation (bottom-up, top-down, MinT-style optimal) restores coherence. Nowcasting blends timely series (weekly claims, daily rates) to estimate the present before official publication — with the real-time vintages caveat that you only had data as first published, not as later revised. Map ATM cash demand, liquidity/deposit flows, and call-volume forecasting onto the methods of this phase.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Log vs simple returns | Additive vs portfolio-aggregating return definitions | Vol modeling, cross-asset and cross-time aggregation | Wrong tails, wrong aggregation, silent bias |
| Stationarity & differencing | Constant mean/variance after transformation | Prerequisite for ARIMA-class models; unit-root tests | You model trends as signal and fail out-of-sample |
| Quantile (pinball) loss & CRPS | Proper scoring rules for distributional forecasts | Cash buffers, VaR inputs, tail-sensitive decisions | You optimize the mean while the business consumes the tails |
| GARCH(1,1) recursion | Conditional variance as an ARMA-like process | The industry's volatility workhorse family | You cannot read a risk team's vol model |
| HAR-RV regression | Realized vol at daily/weekly/monthly lags | Simple OLS vol forecasting from intraday data | You miss the best simple vol model known |
| MASE | Error scaled against a naive baseline | Honest cross-series accuracy comparison | You brag about RMSE that a naive rule beats |
| Hierarchical reconciliation | Coherent forecasts via structured projection | Sum of parts must equal whole in bank networks | Branch plans that contradict head-office numbers |
| HMM forward algorithm | Probabilistic inference over hidden states | Regime detection for risk and strategy switching | Regime talk stays hand-wavy |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Point-in-time data & as-of joins | Revised macro data and late-arriving prices are the leakage superhighway |
| Calendar/timezone handling | Trading calendars, holidays, 24/7 crypto vs 5-day equity weeks — a silent correctness killer |
| Reusable walk-forward harness | Every model in every phase needs the same honest validation scaffold |
| Feature pipelines with shift discipline | Lag/rolling features must be computable from the strict past; test it |
| Scale-out forecasting | Hundreds of series × retraining cadence → global models and scheduling |
| Forecast registry & monitoring | Track quantile calibration and MASE drift in production; alert when calibration slips |
| Backtest-to-decision plumbing | Forecasts feed cash orders, hedges, limits — wire the utility backtest, not just accuracy |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| statsmodels | ETS/ARIMA/SARIMAX, unit-root tests, classic diagnostics |
| arch | GARCH-family volatility models in production-grade Python |
| Nixtla statsforecast / mlforecast | Fast classical baselines and global feature-based forecasting at scale |
| sktime | Unified TS API: composition, reduction, benchmarking |
| darts | Convenient deep-TS experimentation (N-BEATS/N-HiTS/TFT wrappers) |
| LightGBM / XGBoost | Global feature-based forecasters; quantile objectives |
| hmmlearn | Hidden Markov models for regime detection |
| pandas / polars | Alignment, resampling, as-of joins — the correctness layer |
| FRED API client | Macro data pulls for nowcasting |
| Chronos-2 / TimesFM-3 repositories | Zero-shot foundation-model baselines |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Nixtla statsforecast & mlforecast documentation | Docs | Intermediate | Baselines & global models | The current standard for fast, honest TS baselines and global boosting | Essential |
| sktime documentation | Docs | Intermediate | TS tooling | Unified API and reduction patterns for composition | Recommended |
| TimesFM (Google Research) paper & repository | Paper/Code | Advanced | Foundation models | The zero-shot forecasting lineage; TimesFM-3 current as of 2026 | Recommended |
| Chronos / Chronos-2 (Amazon) paper & repository | Paper/Code | Advanced | Foundation models | Probabilistic zero-shot forecasting lineage, current as of 2026 | Recommended |
| FRED (St. Louis Fed) API documentation | Data/API | Intermediate | Macro data | The canonical US macro series source for nowcasting | Essential |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Hyndman & Athanasopoulos, *Forecasting: Principles and Practice* (3rd ed., free online, OTexts) | Book | Intermediate | Forecasting foundations | The best forecasting textbook; hierarchical reconciliation included | Essential |
| Ruey Tsay, *Analysis of Financial Time Series* (Wiley) | Book | Advanced | Financial TS | Stylized facts, GARCH, and financial-specific TS rigor | Essential |
| Marcos López de Prado, *Advances in Financial Machine Learning* (Wiley 2018) | Book | Advanced | Leakage & labeling | Triple-barrier, meta-labeling, purged CV — the trading-ML canon | Essential |
| Nixtla technical blog | Blog | Intermediate | Global models | The philosophy and evidence behind global forecasters | Optional |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Makridakis et al., M5 competition papers (Int. J. Forecasting, 2022) | Paper | Intermediate | Forecast practice | Methodology gold: what actually won and why | Essential |
| Meta Prophet documentation | Docs | Beginner | Decomposition | Widely used; learn its limits as well as its convenience | Optional |
| darts documentation & examples | Docs | Intermediate | Deep TS | Fastest path to N-BEATS/N-HiTS/TFT experiments | Recommended |
| Practitioner write-ups on walk-forward/purged validation | Blog | Intermediate | Validation | Practical gap/embargo discussions beyond the textbook | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Kaggle M5 winning-solution write-ups | Notebooks | Advanced | Feature craft | WRMSSE, lags, and global-model tricks in the wild | Reference |
| Nixtla & darts example galleries | Notebooks | Intermediate | Quickstarts | Copy-paste starting points for benchmarks | Reference |

## 10. Practical Exercises

1. - [ ] Pull **Kraken/Binance OHLCV** for one asset; compute log vs simple returns at 1h/1d/1w; show where they diverge; document alignment and timezone decisions in the notebook header.
2. - [ ] Build a leakage demo: fit a model with a full-sample-standardized feature vs a point-in-time version; quantify the fake lift; write your personal leakage checklist.
3. - [ ] On three **FRED** series (e.g., industrial production, retail sales, claims), benchmark seasonal-naive, ETS, and ARIMA with MASE under walk-forward; produce a table and one paragraph of interpretation.
4. - [ ] Build the reusable walk-forward harness: anchored + rolling modes, configurable embargo gap, per-fold metrics plot; unit-test that no training row is ever after a test row.
5. - [ ] Train a global LightGBM with lag/rolling/calendar features on 50 synthetic retail series; compare per-fold MASE against per-series auto-ARIMA via statsforecast.
6. - [ ] Evaluate Chronos-2 or TimesFM-3 zero-shot on the same 50 series; report MASE and quantile calibration (pinball loss by quantile); note where foundation models win and lose.
7. - [ ] Fit GARCH(1,1) (arch) and HAR-RV on an equity index proxy; compare 1-day vol forecasts against a random-walk-vol baseline with a QLIKE-style loss and a variance-targeting utility backtest.
8. - [ ] Build calibrated quantile forecasts (0.05/0.50/0.95) for synthetic ATM cash demand; report pinball loss and empirical coverage vs nominal.
9. - [ ] Nowcasting notebook: blend two FRED release vintages into a nowcast of a quarterly series; write a note on the real-time vintages problem (first-published vs revised data).
10. - [ ] Fit a 2-3 state HMM (hmmlearn) to crypto returns; plot regimes; then implement triple-barrier labeling and meta-labeling on the same series and compare label distributions.

## 11. Mini Projects

**M1 — Hierarchical retail forecast, bank-network analog.** M5-style sales data (Kaggle, external) or a synthetic bank branch/product network → global LightGBM per level → MinT-style reconciliation → coherence error before/after. Deliverable: repo + reconciliation study. Difficulty: ★★★☆☆.

**M2 — Volatility forecast bake-off with utility.** GARCH(1,1) vs HAR-RV vs naive on index data → probabilistic vol forecasts → variance-targeting backtest measuring realized vol and P&L impact. Deliverable: benchmark report + decision memo. Difficulty: ★★★☆☆.

**M3 — ATM cash-demand forecaster.** Synthetic ATM network (locations, calendars, seasonality) → probabilistic forecasts → cash-order policy balancing stock-out risk vs idle-cash cost. Deliverable: forecast service + policy simulator. Difficulty: ★★★☆☆.

**M4 — Global vs local at scale.** 50+ series (crypto OHLCV + synthetic) → global boosting vs per-series ARIMA vs foundation-model zero-shot, all under the same walk-forward harness with MASE + pinball tables. Deliverable: head-to-head report. Difficulty: ★★★★☆.

## 12. Major Project Hook

This phase powers the financial forecasting flagship — a liquidity and volatility forecasting platform (probabilistic forecasts, walk-forward CI, utility backtests). See `/projects/README.md` for the flagship index and current numbering.

## 13. Case Studies & Industry Examples

- **M5 Forecasting Competition (2020-2022)**: thousands of teams and methodical post-mortems (Makridakis et al., IJF) — the clearest public evidence that global gradient boosting plus careful validation beats exotic architectures (see `/case-studies/README.md`).
- **Bank liquidity forecasting**: after 2008, and again after the publicly reported 2023 US regional-bank deposit runs, deposit-flow and liquidity forecasting moved up the priority stack — the lesson 09.14 use cases are board-level topics.
- **Meta Prophet adoption and backlash**: widely adopted for convenience, then widely criticized for uncontrolled drift — a lesson in tool fit versus rigor.
- **TS foundation models (2024-2026)**: the TimesFM and Chronos lineages moved zero-shot forecasting from paper demos to practitioner tooling; hedged expectation-setting matters more than hype.

## 14. Interview Questions

**Why is stock price prediction so hard?** Markets are close to efficient — prices already reflect available information, so edge must come from less-public information or faster processing — and the signal-to-noise ratio is tiny relative to volatility; direction forecasts hover near coin-flip accuracy while volatility and secondary quantities are more tractable.

**How do you avoid leakage in time-series features?** Every feature must be a function of information strictly before decision time: as-of joins for slowly updated data, shift-by-one discipline for close-based indicators, expanding-window scaling, embargo gaps in validation — then unit-test the boundary.

**Why do finance teams need probabilistic forecasts rather than point forecasts?** Decisions consume tails: cash buffers, VaR limits, and hedges need quantiles, and over- vs under-forecast costs are asymmetric; pinball loss and CRPS measure what actually matters.

**When do foundation models beat classical baselines?** Mostly on cold-start, short-history, or many-series problems where zero-shot transfer helps; on long, well-modeled seasonal series, tuned classical or global models still frequently win — settle it with your own walk-forward benchmark.

**Explain MASE and why it beats RMSE for cross-series work.** MASE scales error by the in-sample naive forecast, making errors comparable across series of different scales; RMSE rewards series with large absolute values and hides baseline-relative performance.

**GARCH vs HAR-RV — when do you pick which?** GARCH uses daily returns and captures conditional-variance persistence with few parameters; HAR-RV uses intraday-realized volatility at daily/weekly/monthly lags and often forecasts better where high-frequency data exists — use HAR-RV when you have intraday data, GARCH when you do not.

**What is triple-barrier labeling and why not fixed-horizon up/down?** Triple-barrier labels each observation by which of profit-take, stop-loss, or time-expiry barriers is hit first, matching how positions actually exit; fixed-horizon labels ignore the path and risk management, producing labels your strategy never faces.

**How does hierarchical reconciliation work and why does a bank need it?** Independent bottom-level forecasts rarely sum to top-level plans; reconciliation projects forecasts onto a coherent space (bottom-up, top-down, or MinT-optimal) so branch and head-office numbers agree — otherwise one of them is wrong by construction.

**What is the real-time vintages problem in nowcasting?** Official series are revised after publication; a nowcast trained on revised data uses information nobody had at decision time — point-in-time vintage data is the honest fix.

**Walk me through walk-forward validation for a 20-day-horizon forecast.** Rolling or anchored windows, test sets strictly after training, a 20-day embargo gap plus label-straddling purge so training labels cannot see test-period outcomes; report per-fold distributions, not one pooled number.

**Your model beats the baseline offline but loses money live. Diagnose.** Check leakage first (offline lift is the signature), then distribution shift and calibration drift, then whether the utility backtest matched the live decision (costs, constraints, latency); accuracy without utility is the usual root cause.

**Where does volatility clustering come from and why does it matter for ML?** Large moves cluster because risk appetite, leverage, and flow effects persist — variance is autocorrelated even when returns are nearly unpredictable; it makes volatility the most forecastable financial quantity and motivates GARCH/HAR and regime models.

## 15. Assessment — Can You Pass the Bar?

- [ ] Build a leakage-free feature pipeline with unit tests proving point-in-time correctness, and explain each guard to a risk officer.
- [ ] Beat naive and ETS/ARIMA baselines with a global boosted model — or honestly show the MASE evidence that you could not.
- [ ] Ship a probabilistic forecaster with per-quantile calibration analysis (pinball loss + coverage).
- [ ] Compare GARCH vs HAR-RV vs naive with both a statistical loss and a decision-utility backtest.
- [ ] Produce a coherent hierarchical forecast with reconciliation and explain the MinT idea in plain language.
- [ ] Run zero-shot foundation-model baselines and write a defensible adopt/reject recommendation.
- [ ] Implement triple-barrier + meta-labeling and explain when it beats fixed-horizon labels.

## 16. Mastery Checkpoint

You may proceed to Phase 10 when:

1. Your walk-forward harness exists as a reusable, tested module and has been used for at least three model families.
2. Your benchmark repo shows baselines vs global boosting vs foundation models with MASE and pinball tables — and a written verdict.
3. Your volatility study compares GARCH/HAR-RV/naive under a utility backtest, not just RMSE.
4. You can give a 10-minute "forecast committee" walkthrough of one production-grade use case (ATM cash, liquidity, or volatility) with honest limitations — record it and store under `/notes/artifacts/`.

Evidence: repo links + benchmark reports + recorded walkthrough. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Deciding at time t with the close of time t — the single most common leak in financial ML, and it always inflates results.
- Random k-fold cross-validation on time series "because it uses more data" — invalid, full stop.
- Reporting RMSE on raw levels of a trending series; a naive "tomorrow = today" beats your model and you cannot tell.
- Shipping point forecasts to tail-sensitive consumers (cash, VaR) because intervals were inconvenient to build.
- Backtesting nowcasts on revised macro data and calling it historical realism (the vintages problem).
- Full-sample normalization, or rolling stats with center/inclusive windows, silently leaking the present.
- A deep architecture on a 200-observation univariate series because the paper said so — the baseline wins and you burned a week.

## 18. Where This Goes Next

Phase 10 turns forecasting output into market decisions: your volatility forecasts feed VaR and options work, your walk-forward discipline becomes backtesting discipline, and your return-computation rigor becomes portfolio mathematics. Phase 15 will revisit the streaming and latency side of the same problems in production.
