# Brief 07 — Deep Hedging & RL in Execution

> **Maturity: Research frontier with niche production use.** RL for trading is where finance's data problems (low signal, non-stationarity, expensive errors) meet RL's weaknesses (sample inefficiency, simulator dependence). The honest work here is simulation design and benchmark honesty.

## Status Map
- **Established:** classical hedging (delta hedging under BS assumptions), optimal execution (Almgren-Chriss 2000), backtest honesty disciplines.
- **Current practice:** RL in limited production niches (order execution policy tuning, spread management) at sophisticated firms; simulator-driven research; deep hedging as a valuation-and-risk framework (Buehler et al.).
- **Emerging:** model-free hedging under transaction costs and constraints; market simulators as RL environments; offline RL from logged order-book data.
- **Frontier:** end-to-end learning execution pipelines; multi-agent market simulation; generative market models as training grounds; LLM-assisted strategy research (far from production).

## Key Papers & Resources
- Buehler, Gonon, Teichmann, Wood, "Deep Hedging" (2019) + "Deep Hedging: Learning to Simulate Equity Option Markets" (follow-up).
- Almgren & Chriss, "Optimal Execution of Portfolio Transactions" (2000) — the baseline to beat.
- Cartea, Jaimungal, Penalva, *Algorithmic and High-Frequency Trading* — the stochastic-control foundations.
- Sutton & Barto, *Reinforcement Learning* (free) — the RL canon.
- FinRL / FinRL-Meta (open-source gym environments) — reproduction infrastructure.
- Kolm & Ritter papers on RL in finance (practitioner-honest analyses).

## Open Problems
1. Sim2real: policies trained in simulators break on real microstructure; how do you certify simulators?
2. Benchmarks: most papers beat straw-man baselines; Almgren-Chriss with honest cost models is a strong opponent.
3. Non-stationarity: regimes change policies' edge; online adaptation without blowup.
4. Risk-adjusted objectives: RL rewards vs drawdown/tail constraints (constrained MDPs in finance).
5. Explainability: justifying an RL execution policy to a risk committee.

## Solo Experiments (all paper-traded, never "live")
1. **Deep hedging reproduction (simplified):** GBM+jump simulator; train an RL (or direct-optimization) hedger under transaction costs; compare hedging-error distribution vs delta hedging — reproduce the qualitative result (RL trades cost vs error).
2. **Execution policy:** level-1 LOB simulation; RL market-order placement vs TWAP vs Almgren-Chriss-style schedule; report implementation shortfall distributions.
3. **Meta-labeling cross-check:** López de Prado's meta-labeling on a simple signal with purged validation — the honest version of "ML on trading".
4. **Simulator certification:** build a GBM+Hawkes-order-flow simulator; show which market stylized facts it reproduces and which it fails (this is the actual research skill).

## Curriculum Hooks
Phase 10 (foundations), Phase 19-A (deep), L5 deep-hedging project, [`papers/`](../../papers/README.md).

## What Would Change My Mind
A public, reproducible study where an RL execution/hedging policy beats Almgren-Chriss-class baselines on *out-of-sample real data* with transaction costs, regime shifts, and risk constraints — not just in its own simulator.
