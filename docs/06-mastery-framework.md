# 06 — Mastery Framework

> Deliverable M. Nine levels, objective evidence bars, stage gates. Reading does not move you up a level; artifacts, explanations, and systems do.

---

## 1. The Nine Levels

```text
1 Awareness      I can define it and place it in the financial landscape
2 Understanding  I can explain it, its math, and why finance needs it
3 Implementation I can implement it from scratch or with the right library, correctly
4 Application    I can apply it to a real financial dataset and make it work end-to-end
5 Production     I can deploy it with monitoring, audit, and failure handling
6 Optimization   I can make it faster, cheaper, calmer under drift — with evidence
7 Architecture   I can design the system around it and defend the tradeoffs
8 Research       I can read the frontier, reproduce results, and extend or refute them
9 Leadership     I can set direction, teach it, govern it, and grow people in it
```

**Level transitions are earned by evidence type:**

| From → To | Evidence that moves you |
|---|---|
| 1→2 | Concept note in your own words + oral explanation without notes |
| 2→3 | Working implementation (build-it-then-library where honest) |
| 3→4 | End-to-end run on a canonical dataset with an eval table |
| 4→5 | Deployed service: monitoring, drift, audit logging, incident runbook |
| 5→6 | Measured improvement: latency p99, cost/decision, drift response time |
| 6→7 | System design doc with alternatives considered and rejected |
| 7→8 | Paper reproduction or novel experiment with a written verdict |
| 8→9 | Teaching artifact + governance contribution + mentee progress |

## 2. Domain-by-Level Matrix (target for a FinTech senior)

| Domain | Min level to claim competence | Phase |
|---|---|---|
| Financial systems & banking ops | 4 | 01-02 |
| Financial data engineering | 5 | 03 |
| Financial math & stats | 4 | 04-05 |
| Credit risk ML | 5 | 06 |
| Fraud ML | 5 | 07 |
| AML & financial crime | 4 | 08 |
| Time series & forecasting | 4 | 09 |
| Quantitative finance literacy | 3-4 | 10 |
| Financial NLP / doc AI | 4 | 11 |
| GenAI / RAG for finance | 5 | 12-13 |
| Agents in finance | 4-5 | 14 |
| Real-time & streaming | 5 | 15 |
| Compliance & responsible AI | 4 | 16 |
| Production engineering | 5-6 | 17 |
| System design | 6-7 | 20 |

## 3. Stage Gates (promotion bars)

**Gate I→II (Domain Bridge)** — evidence pack:
1. Ledger simulator + idempotent payment state machine (code + tests).
2. Yield-curve analysis artifact (FRED) with written interpretation.
3. Recorded 10-min oral exam: explain bank balance sheet, money creation, a card payment end-to-end, and the 2008 mechanism — no notes.
4. Baseline assessment re-scored; weak areas logged as issues.

**Gate II→III (Data & Quant Core)** — evidence pack:
1. PIT-correct feature pipeline with the leakage it avoids demonstrated (AUC gap shown).
2. Purged K-fold implementation + the random-CV leakage demo.
3. Monte Carlo pricer + duration/convexity calculator with tests.
4. One causal/econometric study (DiD or uplift) with honest limitations section.

**Gate III→IV (Core Financial ML — the employable bar)** — evidence pack:
1. Calibrated PD model (scorecard + challenger) + reason codes + SR 11-7-shaped MDD.
2. Cost-sensitive fraud model with profit metric and threshold economics.
3. AML mini-stack: sanctions screener + rules TM + one graph model.
4. Recorded "risk committee" defense of one model (10 min, hostile questions prepared).
5. Interview-track self-grade ≥3/5 on AI/ML core + FinTech domain.

**Gate IV→V (Markets fluency)** — evidence pack: VaR engine (3 methods) with Kupiec backtest; one cost-honest strategy backtest; recorded 5-min explainer of greeks/vol to a product manager.

**Gate V→VI (Advanced AI)** — evidence pack: one governed GenAI/RAG/agent system (citation-forced, eval harness in CI, approval gates) + streaming decisioning path at its latency budget with fallback demo.

**Gate VI→Senior** — evidence pack: deployed flagship (definition-of-done complete) + 6 system-design docs + 1 public writing artifact + fairness/compliance dossier + mentoring or teaching artifact.

## 4. The Eight Demonstration Verbs (self-test per topic)

For any topic, you may claim the level the verbs allow:

> **Explain** it · **Implement** it · **Debug** it · **Evaluate** it · **Deploy** it · **Design** with it · **Teach** it · **Improve** it

- Can explain but not implement → level 2.
- Can implement and evaluate on real data → level 4 territory.
- Can deploy, monitor, and survive an incident → level 5.
- Can design the system and teach it → level 7-9.

Log your verb-level per domain in [`PROGRESS.md`](../PROGRESS.md) §5.

## 5. Anti-Completionism Rules

1. **Evidence or it did not happen.** Checkboxes without artifacts are debt, not progress.
2. **Harsh grading:** at each gate, play the hostile examiner — write the 5 questions you hope nobody asks, then answer them on the record.
3. **No level skipping on paper:** you may *move fast* through levels you can evidence, but every transition leaves an artifact.
4. **The teach test:** you have not mastered what you cannot teach; each domain needs ≥1 teaching artifact (note, talk, tutorial) before level 7+.
5. **Timeboxing honesty:** if a gate slips twice, reduce scope of the *evidence pack*, never the *evidence*.

## 6. Quarterly Self-Assessment Protocol

1. Re-run the baseline assessment (Phase 00) — same rubric, new answers.
2. Update the domain-level matrix; pick 2 domains to push a level this quarter.
3. Audit artifacts: are they deployable/showable? Prune or upgrade.
4. Re-verify 5 regulatory/industry claims you have been quoting (dates change).
5. Write one public artifact (post/notebook/talk) about the quarter's best insight.
