# Brief 04 — Time-Series Foundation Models

> **Maturity: Emerging practice** (as of 2026). Zero-shot foundation models (Chronos-2, TimesFM-3 with multivariate support, TimeGPT, Lag-Llama) are credible baselines and sometimes winners; classical and boosted models still win many banked, hierarchical, domain-heavy tasks. Verify generation/claims quarterly — this area moves fast.

## Status Map
- **Established:** classical (ETS/ARIMA), GARCH for volatility, boosting-on-lags for tabular-style TS; walk-forward discipline; probabilistic evaluation (pinball, CRPS).
- **Current practice:** global deep models (N-BEATS/N-HiTS/PatchTST/TFT) for large homogeneous series collections; hierarchical reconciliation; hybrid ensembles.
- **Emerging:** zero-shot TSFMs for cold-start and portfolio-wide baselines; fine-tuned TSFMs; multivariate zero-shot (TimesFM-3).
- **Frontier:** TSFM uncertainty quality; financial-domain pretraining; LLM+TS hybrids for explanations of forecasts.

## Key Papers & Resources
- Oreshkin et al., "N-BEATS" (2019); Lim et al., "Temporal Fusion Transformer" (2021); Zeng et al., "Are Transformers Effective for Time Series Forecasting?" (DLinear, 2023) — the skepticism check.
- Salinas et al., "DeepAR" (2020) — probabilistic canon.
- Ansari et al., "Chronos" (2024) + Chronos-2 (Amazon, 2025-26); Das et al., "TimesFM" (2024) + TimesFM-3 (Google, 2026) — verify current versions.
- Hyndman & Athanasopoulos, *FPP3* (free) — the evaluation and reconciliation foundations.
- M5 competition write-ups (Makridakis et al.) — how global models actually win.

## Open Problems
1. Do TSFMs beat per-domain models on *banking* data (cash demand, deposit flows) or only on public benchmarks?
2. Uncertainty: are TSFM quantiles calibrated enough for risk use (coverage, tail behavior)?
3. Regime shifts: zero-shot models trained on historical web-scale data vs sudden macro breaks.
4. Hierarchy: native reconciliation vs post-hoc MinT on TSFM outputs.
5. Fine-tuning economics: when does adaptation beat training a bespoke global model?

## Solo Experiments
1. **The honest bake-off:** Chronos-2/TimesFM-3 zero-shot vs ETS vs LightGBM-on-lags vs N-HiTS on (a) crypto OHLCV, (b) FRED macro series, (c) synthetic ATM-cash network — report MASE + pinball + coverage.
2. **Hierarchy test:** reconcile vs unreconcile TSFM forecasts on a synthetic hierarchy; measure business-level error (total network cash error, not leaf MAE).
3. **Regime break:** evaluate all models across a known structural break (e.g., 2020 in macro data); who degrades least?
4. **Cold-start:** brand-new series with 4 weeks of history — TSFM vs classical warmup; quantify the practical win.

## Curriculum Hooks
Phase 09 (main), Phase 19-H, L5 forecasting projects.

## What Would Change My Mind
TSFMs winning on hierarchical *business* metrics (not leaf MAE) across banking-style datasets with calibrated uncertainty — that moves them from "impressive baselines" to "default choice".
