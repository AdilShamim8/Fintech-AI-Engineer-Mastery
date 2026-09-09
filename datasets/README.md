# Dataset Roadmap

> Deliverable G. ~28 public datasets mapped to phases and projects. For each: contents, the problems it enables, quality issues, leakage traps, bias risks, and best ML applications.
> Legend: **[T1]** authoritative/competition-grade · **[T2]** solid public · **[T3]** useful/synthetic · ⚠️ caveats follow the row.

## 1. Master Table

| Dataset | Contents | Size | Enables | Tier | Used in |
|---|---|---|---|---|---|
| IEEE-CIS Fraud Detection (Kaggle/Vesta) | Card transactions, device/merchant features, fraud labels | ~590k rows, 400+ cols | Fraud detection, cost-sensitive learning, real-time scoring features | T1 | P07, F01, F07 |
| Credit Card Fraud Detection (ULB) | European card txns, PCA features + time/amount | 284,807 txns, 0.172% fraud | Imbalance, calibration under undersampling, PR-AUC discipline | T1 | P07 |
| PaySim | Synthetic mobile-money transactions with fraud/labels | 6.3M rows | Streaming fraud pipelines, graph-lite experiments | T3 ⚠️ synthetic | P07 |
| IBM Synthetic AML Transactions (Kaggle) | Synthetic txns + illicit-account labels (typology-based) | ~9.5M txns variants | AML TM engines, graph detection, alert tuning | T3 ⚠️ synthetic | P08, F03 |
| SAML-D (Kaggle) | Newer synthetic AML dataset with typology labels | ~9.3M rows | TM scenario design; typology classification | T3 ⚠️ synthetic | P08 |
| Elliptic Bitcoin Dataset | BTC transaction graph, 203k nodes/234k edges, illicit labels | graph | GNNs for AML, temporal splits | T1 | P08, F03 |
| Elliptic++ | BTC addresses + users + edges, multi-label | larger graph | Temporal/heterogeneous GNN research | T1 | P19-D, L5 |
| LendingClub Loan Data | 2007-2020 loans: grades, rates, outcomes, recoveries | ~2.3M loans | PD modeling, reject inference, LGD proxy, vintages | T1 ⚠️ | P06, F02 |
| Home Credit Default Risk (Kaggle) | Applicants + bureau/previous-app tables | 307k applicants | Relational credit features, ML scorecards | T1 | P06 |
| Give Me Some Credit (Kaggle) | Revolving credit records + 2-yr delinquency | 150k | Classic scorecards, calibration | T2 | P06 |
| German Credit (UCI) | 1,000 applicants, 20 attrs | tiny | Scorecard mechanics, WOE/IV demos | T2 | P06 |
| Taiwan Credit Default (UCI) | 30k clients, pay history | 23 cols | Default prediction, fairness demos (sex/age) | T2 | P06, P16 |
| FICO HELOC Challenge | HELOC applicants + risk flags, explainability contest | ~10k | Adverse-action reasons, monotonic constraints | T1 | P06, P16 |
| Freddie Mac Single-Family Loan-Level | US mortgages: origination + monthly performance | millions (free registration) | Vintages, roll rates, LGD, macro stress | T1 | P06, P09 |
| Fannie Mae Single-Family | Same shape as Freddie | millions (free registration) | Cross-validation of mortgage analytics | T1 | P06 |
| CFPB Consumer Complaint Database | Narrative complaints by product | millions | Text classification, topic modeling, routing | T1 | P11 |
| SEC EDGAR (full-text + company facts + XBRL) | Filings, financial statements APIs | all US filers | Doc intelligence, 10-K RAG, factor data | T1 | P11-13, F04, F05 |
| SEC Financial Statement Data Sets | Quarterly numeric XBRL extracts | quarters | Structured financial analytics, ratios | T1 | P11 |
| FRED (Federal Reserve) | US macro/time series via API | 800k+ series | Yield curve, nowcasting, macro features | T1 | P01, P04, P09 |
| ECB Data Portal / SDW | Euro-area rates, FX, financial stats | huge | Non-USD macro features | T1 | P01 |
| BIS Statistics | Credit, debt securities, FX, derivatives | global | Systemic context, stress scenarios | T1 | P01 |
| World Bank / IMF Data | Development & financial soundness indicators | global | Cross-country risk features | T1 | P01 |
| Financial PhraseBank | Sentences with finance sentiment labels (Malo et al. 2014) | 4,840 | Domain sentiment benchmarks | T1 | P11 |
| FiQA-2018 | Financial aspect-based sentiment + QA corpus | ~1.4k train | Sentiment/QA baselines | T2 | P11 |
| FinQA / TAT-QA / ConvFinQA | Numeric QA over financial reports | ~8k+8k+14k | Numeric reasoning evals, RAG benchmarks | T1 | P11, P13 |
| FinanceBench | Open benchmark: financial QA on real filings | ~10k Qs | LLM copilot evaluation (sobering baselines) | T1 | P12, P13 |
| EDGAR-CORPUS (Hugging Face) | Sectioned 10-K corpus | 2009-2020 filers | Long-doc chunking, retrieval experiments | T2 | P11, P13 |
| FI-2010 | Limit order book snapshots with labels | ~4M rows | Microstructure exploration, LOB ML | T2 | P10 |
| Kraken / Binance public OHLCV dumps | Crypto market data, free API/downloads | 24/7 history | TS experiments, triple-barrier labeling | T2 ⚠️ | P09, P10 |
| Bank Marketing (UCI) | Telemarketing outcomes | 45k | Uplift/causal baseline; churn-adjacent | T2 | P05, P19-B |
| Porto Seguro Safe Driver (Kaggle) | Auto insurance claim flags | 595k | Insurance pricing ML, imbalance | T2 | P19-C |
| Survey of Consumer Finances (Fed) | US household finance microdata | triennial | Alternative-data thinking, wealth features | T1 | P06 |
| OFAC SDN List (official download) | Sanctions names/aliases | 10k+ entities | Sanctions screening, fuzzy matching | T1 | P08 |
| GLEIF LEI Data | Legal entity identifiers + relationships | 2.5M+ LEIs | Entity canonicalization, KYB graphs | T1 | P13, F05 |
| Alpaca / IBKR paper APIs (not datasets — simulators) | Brokerage paper trading | live | Execution of backtests/agents in sim | T3 | P10, P14 |

## 2. Per-Dataset Notes (the ⚠️ ones)

**LendingClub ⚠️** — the most instructive credit dataset because of its flaws: (1) grades/rates assigned *before* outcomes → they encode prior policy (using `grade` as a feature is policy leak — use it to *simulate* approval policies instead); (2) the 2016-2018 filings/prosperity era changes data-generating process; (3) recoveries columns enable LGD proxies but are post-charge-off (survivor dynamics). Best uses: vintage curves, reject-inference simulation, calibration studies.

**ULB ⚠️** — PCA anonymization kills interpretable features; time column is seconds-offset (leaks nothing but requires care in splits). Ideal for: imbalance/calibration rigor and threshold economics. Not ideal for feature-engineering practice.

**IEEE-CIS ⚠️** — identity/device fields have heavy missingness patterns that *are* signal; card1-6/addr fields require careful grouping; competition-era FeatureHashing tricks overfit to the public LB. Use for feature craft + profit metrics, not leaderboard chasing.

**PaySim / IBM AML / SAML-D ⚠️ synthetic** — synthetic generators encode simplified typologies; models tuned on them overfit generator quirks. Value: pipeline/graph mechanics at scale, alert-tuning workflows, class-imbalance at AML ratios. Caveat every conclusion with "on synthetic data"; never cite their metrics as field performance.

**Elliptic ⚠️** — time-step structure invites accidental temporal leakage; use temporal splits (train on early steps, test on later) as Weber et al. do. Class imbalance ~10% illicit.

**Freddie/Fannie ⚠️** — registration required; data is huge (plan columnar tooling); performance fields need point-in-time discipline (delinquency status known *later* — classic leakage source); loan acquisitions span macro regimes (2003-2023), which is exactly why they're gold for vintage/stress work.

**Kraken/Binance ⚠️** — 24/7 markets lack session structure; exchange outages create holes; fee structures matter for any backtest (use realistic maker/taker + slippage).

**OFAC SDN ⚠️** — official list, but screening evaluation needs *seeded* aliases (construct your own perturbation set: transliterations, initials, swapped names) since real "misses" aren't labeled.

## 3. Dataset → Project Mapping

| Project | Primary datasets | Supporting |
|---|---|---|
| F01 Fraud platform | IEEE-CIS | ULB, PaySim |
| F02 Credit decisioning | LendingClub, Home Credit | GMS Credit, FICO HELOC |
| F03 AML platform | IBM AML, Elliptic | SAML-D, OFAC SDN |
| F04 Doc intelligence | SEC EDGAR, CFPB complaints | Financial PhraseBank, FinQA |
| F05 RAG assistant | EDGAR, EDGAR-CORPUS | FinanceBench, GLEIF |
| F06 Agentic compliance | synthetic ledgers + IBM AML | OFAC SDN |
| F07 Payment intelligence | IEEE-CIS + PaySim | ECB/BIS rails context |
| L5 deep hedging | Kraken/Binance OHLCV | FI-2010 |
| L5 GraphRAG entity QA | EDGAR + GLEIF | — |
| P09 forecasting | Kraken/Binance, FRED | Freddie Mac (cash-flow style) |

## 4. Data Hygiene Rules (apply to every dataset above)

1. **License & terms check** before any public artifact (competition data often bans redistribution — link, don't rehost).
2. **Point-in-time audit** before first model: list every column and ask "was this knowable at decision time?"
3. **Split by time or entity**, never randomly, unless you can defend it in writing.
4. **Document prevalence** in every eval (it changes every metric's meaning).
5. **Synthetic is a sandbox** — great for pipelines, invalid for performance claims.
6. **Bias note per dataset:** who is underrepresented (thin-file applicants, small merchants, non-US entities) and how that biases any model trained here.
