# Track — Finance-Aware Evaluation

> Deliverable #19 of the brief. Generic ML metrics mislead in finance because errors have asymmetric, delayed, and regulated consequences. This is the evaluation doctrine used across every phase and project.
> Rule zero: **a metric is only valid if someone can state what one point of it is worth in money or risk.**

---

## 1. The Metric Doctrine

```text
Layer 1 — Statistical   Does the model rank/separate?        (AUC, PR-AUC, KS)
Layer 2 — Probabilistic Does it output honest probabilities? (Brier, reliability, CRPS)
Layer 3 — Economic      What is it worth?                    (expected profit, loss avoided, cost per alert)
Layer 4 — Operational   Can it run and survive?              (p99 latency, PSI, alert capacity)
Layer 5 — Governable    Can it be defended?                  (reasons, fairness gaps, stability, docs)
```

Reporting only Layer 1 in finance is malpractice. Every project in this repo ships all five layers or explains why a layer does not apply.

## 2. Metric Reference (what, when, and when NOT)

| Metric | Use when | Do NOT use when | Finance caveat |
|---|---|---|---|
| Accuracy | Never as headline | Always | Useless under imbalance (0.1% fraud) |
| Precision / Recall / F1 | Communicating tradeoffs | Pricing/provisioning decisions | Precision at *capacity* (top-K alerts) is the operational number |
| ROC-AUC | Comparing rank quality across thresholds | Imbalanced rare-event reporting as headline | Threshold-free ≠ decision-free; regulators don't ask for AUC |
| PR-AUC | Rare-event detection (fraud, AML) | Comparing across very different prevalences | Prevalence-dependent: state it |
| KS statistic | Scorecard health (single-number separation) | Deep-learning comparisons | Industry-standard in credit; works because scores are binned |
| Brier score / reliability curves | Any probability output (PD, fraud score) | Rank-only claims | **Calibration is load-bearing in credit** — pricing eats your miscalibration |
| Expected loss (PD×LGD×EAD) | Credit decisioning & provisioning | Fraud alerts (different cost structure) | Garbage in every term if uncalibrated |
| Cost-sensitive expected profit | Fraud, collections, targeting | When costs are genuinely unknown — estimate them first | FP cost = friction/review; FN cost = loss; both estimable from data |
| Expected profit / ROI curves | Threshold selection | Comparing model families directly | Sweep thresholds; report the curve, not one point |
| Sharpe ratio | Strategy/forecast backtests | Short samples, many trials | Must be **deflated** (Bailey-López de Prado) across trials |
| Max drawdown | Strategy risk communication | As the only risk number | Pair with time-under-water and tail stats |
| VaR / CVaR (ES) | Risk reporting, limits | Sub-additivity claims for VaR (CVaR is coherent) | Backtest with Kupiec/Christoffersen tests |
| PSI / CSI | Population/feature stability monitoring | Performance claims (it cannot measure performance) | Labels lag; PSI triggers investigation, not verdicts |
| MASE / sMAPE | Forecast accuracy across series | MASE without scale baseline; sMAPE near zero | Always pair with interval coverage |
| Pinball/quantile loss, CRPS | Probabilistic forecasts | Point forecasts | Coverage of intervals is the business-visible number |
| Hit@k / MRR / nDCG | Retrieval quality (RAG) | End-to-end answer quality alone | Pair with faithfulness and numeric fidelity |
| Faithfulness / context precision (RAGAS-style) | Grounded generation | As a substitute for human gold slices | Calibrate the judge (κ) or don't trust it |
| Disparate impact ratio, equalized odds | Fairness testing | Choosing one metric as "the" answer | Metrics conflict; document the choice and who it protects |
| Task success + trajectory evals | Agents | Final-answer-only grading | Path matters for audit; graded simulations |
| Cost per decision / per query | Production viability | Ignoring caching/routing effects | The number your CFO actually reads |

## 3. The Two Canonical Evaluation Problems

### 3.1 The fraud threshold problem
Fraud is rare; review capacity is fixed; friction costs real revenue.
1. Rank by calibrated fraud probability.
2. Cost model: FP = review cost + customer-friction estimate; FN = expected loss.
3. Choose threshold at capacity or max expected profit — show the curve.
4. Report: alert precision at K, $ saved per 1k txns, PR-AUC (context), p99 latency.
Trap: resampling breaks calibration — recalibrate after (Dal Pozzolo et al. 2015).

### 3.2 The credit calibration problem
Lending consumes probabilities (pricing, provisions), not rankings.
1. Scorecard/challenger → isotonic/Platt calibration on a clean fold.
2. Report reliability curve + Brier by segment, KS (health), and AUC (context).
3. Threshold = profit function: margin × approvals − expected loss − capital cost.
4. Fairness layer: gap report + documented mitigation tradeoff.
Trap: AUC gains with degraded calibration are net-negative for a lender.

## 4. Evaluation Anti-Patterns (with the phase that cures them)

| Anti-pattern | Why it fools you | Cure |
|---|---|---|
| Random K-fold on overlapping financial series | Leakage across correlated samples | Purged K-fold + embargo (Phase 05) |
| Ignoring label latency | "Excellent" precision on fast-confirmed-only labels | Delayed-label protocol; confirm-bias analysis (Phase 07) |
| Backtest on survivor-adjusted data | Ghost alpha | Corporate-action handling; point-in-time universe (Phase 03) |
| One backtest, one verdict | Selection over trials | Deflated Sharpe; trial registry (Phase 05/10) |
| Train/test split inside entity | Same customer in both | Group splits by entity (Phase 03/06) |
| Metric shopping post-hoc | p-hacking with metrics | Pre-registered eval plan in the PRD (Phase 18) |
| Judge-scored LLM output uncalibrated | Automated blind spots | κ vs human labels (Phase 12) |
| Fairness metric theater | One good-looking ratio | Multi-metric report + conflict discussion (Phase 16) |
| Benchmark leaderboard adoption | Different data, different truth | Local eval on your corpus (Phase 09/13) |

## 5. The Project Eval Template (paste into every flagship)

```markdown
## Evaluation
- Layer 1 stats: [metric: value vs baseline; prevalence stated]
- Layer 2 probabilistic: [calibration artifacts; CRPS if forecasting]
- Layer 3 economic: [cost model assumptions; profit curve; per-decision value]
- Layer 4 operational: [p50/p99 latency; alert capacity; drift alarms armed]
- Layer 5 governable: [reason-code quality; fairness gaps; docs/audit status]
- Honest negatives: [what got worse; where the model must not be used]
```

**If a section is empty, the evaluation is not done.**
