# Track — AI Specialization for Finance

> Deliverable J. The financial-AI progression across the six application families. You already know the generic ML/DL/LLM toolbox; this track is about how each tool changes under financial constraints — and which tool carries which business weight.
> Main teaching: Phases 06-09 (classical core), 11-15 (modern stack).

---

## 1. The Six Families and Their Business Weight

| Family | Business problem | Canonical model families | Latency regime | Governance heat |
|---|---|---|---|---|
| **Credit ML** | Who gets money, at what price | Scorecards, constrained GBM, calibrated LR | Batch + API (<1s) | Very high (fair lending, adverse action, AI Act) |
| **Fraud ML** | Which events are attacks | GBM, rules hybrids, anomaly, sequences, graphs | Real-time (<100-200ms) | Medium-high (model secrecy, drift) |
| **Risk & forecasting** | What will happen, how bad | GARCH, GBM lags, TFT/deep TS, foundation models | Batch / minutes | Medium (model risk, capital) |
| **Financial NLP** | What do documents say | FinBERT, extraction (layout models), LLM pipelines | Batch / seconds | Medium (accuracy + PII) |
| **GenAI/RAG** | Expert leverage on text | Hybrid retrieval + rerankers + LLMs with citations | 1-3s | High (entitlements, injection, audit) |
| **Agents** | Executing workflows | Deterministic workflows + LLM steps, tool use, HITL gates | Seconds-minutes | Highest (actions, money, approvals) |

## 2. Progression by Family

### 2.1 Fraud ML (Phase 07, 15; Flagship 01)
```text
Supervised baseline (GBM) → cost-sensitive thresholds & profit metrics
→ imbalance done right (calibration under resampling)
→ feature craft (velocity, device, merchant, network)
→ anomaly layer (IF/LOF/autoencoders) for unknown-unknowns
→ sequence models on transaction histories
→ graph signals for rings → real-time serving under p99 → drift & adversary response
```
Eval anchor: PR-AUC **plus** expected profit per 1k txns **plus** alert precision at analyst capacity. Accuracy is banned from your vocabulary.

### 2.2 Credit ML (Phase 06; Flagship 02)
```text
WOE/IV scorecard (monotonic, auditable) → constrained GBM challenger
→ calibration (isotonic/Platt) → cutoff economics & risk-based pricing
→ reject inference → LGD/EAD → reason codes & counterfactuals → fairness testing
→ drift/PSI monitoring → SR 11-7-style documentation
```
Eval anchor: AUC is table stakes; calibration (Brier, reliability), profit curves, stability (PSI), fairness gaps, and reason-code quality decide shipment.

### 2.3 Risk & Forecasting (Phases 09, 10)
```text
Honest baselines (ETS/ARIMA) → global boosting on lags → probabilistic (quantiles, CRPS)
→ vol modeling (GARCH/HAR-RV) → hierarchical reconciliation
→ foundation models (zero-shot, evaluated locally) → backtest honesty (purged, deflated)
```
Eval anchor: MASE/sMAPE **plus** financial utility (value-added backtest) **plus** interval coverage. Point-accuracy alone is a rookie tell.

### 2.4 Financial NLP & Doc AI (Phase 11; Flagship 04)
```text
Domain lexicons (L&M) → FinBERT vs baselines → NER/relations/events
→ layout-aware table extraction → long-doc chunking strategies
→ numerical reasoning (FinQA-style) with hallucination audit → HITL extraction QA
```
Eval anchor: span-F1 and table-field precision **plus** numeric-fidelity rate **plus** reviewer minutes saved.

### 2.5 GenAI & RAG (Phases 12, 13; Flagship 05)
```text
Citation-forced answering → numeric fidelity checker → hybrid retrieval (BM25+dense+rerank)
→ metadata filters (version/jurisdiction) → ACL-aware retrieval → GraphRAG for entities
→ golden sets + faithfulness evals → injection defense → cost engineering (routing/caching)
```
Eval anchor: retrieval hit@k/MRR **plus** faithfulness **plus** numeric-fidelity **plus** cost per resolved query. "Vibes" evaluations are non-compliant with this curriculum.

### 2.6 Agents (Phase 14; Flagship 06)
```text
Workflow-first design (deterministic spine) → LLM steps where judgment is needed
→ typed tools with schemas → approval gates & confidence escalation
→ full trace/replay audit → trajectory evals & simulated environments
→ idempotent financial actions → graceful degradation to humans
```
Eval anchor: task success **plus** trajectory quality **plus** audit completeness **plus** escalation correctness. An agent that cannot show its work is a liability, not a feature.

## 3. Cross-Cutting AI Disciplines (owned in this track)

| Discipline | Where taught | Non-negotiable habit |
|---|---|---|
| Calibration | 06, 07, 09 | Every probability consumer gets a reliability curve |
| Explainability | 06, 11, 16 | Per-decision reasons, not global feature importances |
| Fairness | 06, 16 | Metrics + conflicts + documented mitigation tradeoffs |
| Drift | 06, 07, 17 | PSI/KS alarms wired to a response runbook |
| Evaluation design | 05, 12, 13 | Golden sets + CIs + judge calibration |
| Cost engineering | 12, 15, 17 | Cost per decision/query on every design doc |
| Security | 14, 16 | Threat model per AI surface (OWASP LLM Top 10 lens) |

## 4. What NOT to Spend Time On (specialization discipline)

- Building LLMs from scratch, training foundational models, generic Kaggle climbs, papers-without-reproduction, tool-hopping between agent frameworks, GPU infrastructure for models you don't need (a tuned GBM beats an unfinetuned LLM on most tabular financial tasks — and is governable).
- The test for any new AI technique: *does it change a financial decision's economics, defensibility, or latency?* If no — it is a demo, and this repo does not collect demos.

## 5. Evidence Bar for This Track

- [ ] One fraud model with profit-based threshold analysis (not AUC headline).
- [ ] One credit model: calibrated, reason-coded, fairness-tested, MDD-documented.
- [ ] One probabilistic forecast with utility backtest and honest purged validation.
- [ ] One extraction pipeline with span-F1 + numeric-fidelity audit.
- [ ] One citation-forced RAG system with golden-set evals in CI.
- [ ] One agent workflow with approval gates, audit trail, and trajectory evals.

Six artifacts, six families — this track's evidence pack is the portfolio.
