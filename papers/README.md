# Papers — The Canon, Sequenced

> Tiered reading list. ~60 papers; the 20-canon subset is tracked in [`PROGRESS.md`](../PROGRESS.md). Read with the workflow in [`LEARNING.md`](../LEARNING.md) §7: synthesis in your own words, one reproduction where feasible.

## Reading Order (by phase relevance)

### Foundations of finance-ML thinking (Phase 04-05)
| Paper | Year | Why |
|---|---|---|
| Markowitz, "Portfolio Selection" | 1952 | The origin of quantitative risk-return thinking |
| Engle, "Autoregressive Conditional Heteroscedasticity with Estimates of the Variance of UK Inflation" | 1982 | ARCH — volatility clustering formalized (Nobel) |
| Bollerslev, "Generalized Autoregressive Conditional Heteroskedasticity" | 1986 | GARCH — the workhorse you will actually use |
| Bailey, Borwein, López de Prado, Zhu, "The Probability of Backtest Overfitting" | 2015(ish) | Why backtests lie; companion to deflated Sharpe |
| Bailey & López de Prado, "The Deflated Sharpe Ratio" | 2014 | Correcting risk-adjusted returns for selection |

### Credit (Phase 06)
| Paper | Year | Why |
|---|---|---|
| Hand & Henley, "Statistical Classification Methods in Consumer Credit Scoring: A Review" | 1997 | The classic survey; vocabulary source |
| Thomas, "A Survey of Credit and Behavioural Scoring" | 2000 | Behavioral scoring context |
| Lessmann, Baesens, Seow, Thomas, "Benchmarking State-of-the-Art Classification Algorithms for Credit Scoring: An Update of Research" | 2015 | The ML-vs-logistic benchmark everyone cites |
| Banasik, Crook, Thomas, "Sample Selection Bias in Credit Scoring Models" (reject inference line) | 2003 | Selection bias formalized for credit |
| Merton, "On the Pricing of Corporate Debt: The Risk Structure of Interest Rates" | 1974 | Structural default model — roots of PD thinking |

### Fraud & AML (Phase 07-08)
| Paper | Year | Why |
|---|---|---|
| Bolton & Hand, "Statistical Fraud Detection: A Review" | 2002 | The field's founding survey |
| Dal Pozzolo, Caelen, Johnson, Bontempi, "Calibrating Probability with Undersampling for Unbalanced Classification" | 2015 | The calibration-under-resampling correction every fraud model needs |
| Chandola, Banerjee, Kumar, "Anomaly Detection: A Survey" | 2009 | The taxonomy you will reuse forever |
| Liu, Ting, Zhou, "Isolation Forest" | 2008 | The anomaly detector you will actually ship |
| Weber, Ignatowicz, et al., "Anti-Money Laundering in Bitcoin: Experimenting with Graph Convolutional Networks for Financial Forensics" | 2019 | The Elliptic paper — GNN-AML canon |
| Pareja et al., "EvolveGCN: Evolving Graph Convolutional Networks for Dynamic Graphs" | 2020 | Temporal graphs for evolving transaction networks |

### Markets & quant (Phase 10)
| Paper | Year | Why |
|---|---|---|
| Fama, "Efficient Capital Markets: A Review of Theory and Empirical Work" | 1970 | Why prediction is hard — the humility paper |
| Fama & French, "Common Risk Factors in the Returns on Stocks and Bonds" | 1993 | The factor model every risk conversation assumes |
| Black & Scholes; Merton (1973) option pricing line | 1973 | Derivatives foundations |
| Almgren & Chriss, "Optimal Execution of Portfolio Transactions" | 2000 | Market impact mathematics |
| Buehler, Gonon, Teichmann, Wood, "Deep Hedging" | 2019 | RL meets hedging — the research-frontier bridge |
| Corsi, "A Simple Approximate Long-Memory Model of Realized Volatility" (HAR-RV) | 2009 | The vol benchmark that beats GARCH half the time |

### Time series & forecasting (Phase 09)
| Paper | Year | Why |
|---|---|---|
| Oreshkin et al., "N-BEATS: Neural Basis Expansion Analysis for Interpretable Time Series Forecasting" | 2019 | Deep TS done with rigor |
| Lim, Arik, Loeff, Pfister, "Temporal Fusion Transformers for Interpretable Multi-horizon Time Series Forecasting" | 2021 | The interpretable deep forecaster |
| Zeng et al., "Are Transformers Effective for Time Series Forecasting?" (DLinear) | 2023 | The skepticism check every deep-TS claim needs |
| Salinas et al., "DeepAR: Probabilistic Forecasting with Autoregressive Recurrent Networks" | 2020 | Probabilistic forecasting canon |
| Ansari et al., "Chronos: Learning the Language of Time Series" (+ Chronos-2, 2025-26) | 2024+ | TS foundation models — emerging practice, verify current |
| Das et al., "A Decoder-Only Foundation Model for Time-Series Forecasting" (TimesFM; TimesFM-3 2026) | 2024+ | The other TSFM line — verify current |

### NLP & GenAI (Phase 11-13)
| Paper | Year | Why |
|---|---|---|
| Loughran & McDonald, "When Is a Liability Not a Liability? Textual Analysis, Dictionaries, and 10-Ks" | 2011 | The finance lexicon paper |
| Araci, "FinBERT: Financial Sentiment Analysis with Pre-trained Language Models" | 2019 | Domain adaptation reference |
| Yang, Uy, Huang, "FinBERT: A Pretrained Language Model for Financial Communications" | 2020 | The other FinBERT — know both |
| Chen, Ehiqiao(?), et al., "FinQA: A Dataset of Numerical Reasoning over Financial Data" | 2021 | Numeric QA benchmark |
| Wu et al., "BloombergGPT: A Large Language Model for Finance" | 2023 | The domain-pretraining economics lesson |
| Islam et al., "FinanceBench: Open-Source Benchmark for Financial Question Answering" | 2023 | Sobering LLM-on-filings baselines |
| Lewis et al., "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" | 2020 | RAG origin |
| Gao et al., "Retrieval-Augmented Generation for Large Language Models: A Survey" | 2023 | The map of RAG failure modes |
| Es et al., "RAGAS: Automated Evaluation of Retrieval Augmented Generation" | 2023 | RAG evaluation scaffolding |

### Agents & safety (Phase 14, 16)
| Paper | Year | Why |
|---|---|---|
| Yao et al., "ReAct: Synergizing Reasoning and Acting in Language Models" | 2022 | The agent loop canon |
| Shinn et al., "Reflexion: Language Agents with Verbal Reinforcement Learning" | 2023 | Self-correction patterns |
| Wu et al., "AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation" | 2023 | Multi-agent patterns |
| OWASP, "Top 10 for Large Language Model Applications" | 2023-25 | The threat catalog (updates — verify current version) |
| Abadi et al., "Deep Learning with Differential Privacy" (DP-SGD) | 2016 | Privacy-preserving training foundations |

### Credit/fairness deep cuts (Phase 16)
| Paper | Year | Why |
|---|---|---|
| Hardt, Price, Srebro, "Equality of Opportunity in Supervised Learning" | 2016 | Equalized odds origin |
| Mitchell et al., "Model Cards for Model Reporting" | 2019 | Documentation pattern for governance |
| Bellamy et al., "AI Fairness 360" | 2018-19 | The fairness toolbox paper |

## How To Use This List

1. Do not read linearly; read on demand from phases (each phase's Tier 1 table points here).
2. Canon-20 (tracked in PROGRESS.md): Markowitz, Fama-French, Engle, Bollerslev, Merton-1974, Bolton & Hand, Dal Pozzolo, Lessmann, Isolation Forest, SHAP, Weber-Elliptic, Almgren-Chriss, Deflated Sharpe, TFT, FinQA, BloombergGPT, FinanceBench, RAG survey, ReAct, Deep Hedging.
3. For each: 1-page synthesis in your own words → file under `notes/concepts/`.
4. Reproduce one result for every family (fraud, credit, TS, RAG, agents) — that is the L5 project ladder.
5. Verify "current" claims (OWASP version, TSFM generations) quarterly via [`docs/08-research-agenda.md`](../docs/08-research-agenda.md).
