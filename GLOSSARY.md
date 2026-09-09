# GLOSSARY — FinTech Terms You Must Own

> Terms the whole curriculum assumes. Card the starred ones. Grouped by domain.

## Money, Banking & Macro

| Term | Definition |
|---|---|
| Base money (M0) | Central-bank-issued money: currency + bank reserves |
| Fractional reserve / money creation | Banks create deposits by lending; the money multiplier story (simplified) and its modern caveats |
| Policy rate | Central bank's steering rate (Fed funds, ECB deposit rate); anchors all other rates |
| Yield curve | Plot of rates across maturities; inversion historically precedes recessions |
| Term premium | Extra yield demanded for holding long maturities |
| Inflation (CPI/Core/PCE) | Price-level growth; "core" excludes food/energy; PCE is the Fed's target gauge |
| QT / QE | Central bank shrinking/expanding its balance sheet |
| FX spot/forward | Immediate currency exchange vs contracted future exchange |
| LIBOR → SOFR | Benchmark-rate transition: panel-bank estimates to risk-free overnight rates |
| Nostro/Vostro | Correspondent accounts banks hold with each other across borders |

## Banking & Payments

| Term | Definition |
|---|---|
| Core banking | The ledger-of-record system for accounts, deposits, loans, GL |
| Deposit vs loan lifecycle | Origins (funding) vs uses (lending) of a bank's balance sheet |
| LCR / NSFR | Liquidity Coverage Ratio / Net Stable Funding Ratio (Basel liquidity rules) |
| Four-party model | Card scheme economics: issuer, acquirer, merchant, network |
| Interchange | Fee paid issuer-ward per card transaction; the political football of payments |
| Authorization / capture | Approval of a payment vs final commitment of funds |
| Clearing vs settlement | Exchange of payment info/obligations vs final transfer of funds |
| ACH | US batch electronic payment network (high volume, T+1-ish) |
| RTP / FedNow | US instant-payment rails (Clearing House / Federal Reserve) |
| SEPA / SEPA Instant | EU cross-border euro payment scheme (and its instant variant) |
| UPI / PIX | India's and Brazil's national real-time payment systems |
| SWIFT | Messaging network for cross-border payments (not a settlement system) |
| ISO 20022 | Rich structured standard for financial messaging (payments migration) |
| Chargeback | Cardholder dispute flow reversing a transaction |
| PCI DSS | Card-data security standard constraining storage/processing of PANs |
| Open banking | Regulated API access to customer-permissioned bank data (PSD2, FDX) |
| BaaS / embedded finance | Bank functions offered via API inside non-bank products |
| BNPL | Buy-now-pay-later short installment credit |
| Reconciliation | Matching internal records vs counterparty statements; where money leaks |
| Idempotency key | Client token making repeated payment requests safe to retry |

## Lending & Credit

| Term | Definition |
|---|---|
| PD / LGD / EAD | Probability of default, loss given default, exposure at default |
| Expected loss | PD × LGD × EAD |
| Scorecard | Points-based additive credit model (WOE bins, points to double odds) |
| WOE / IV | Weight of evidence (bin log-odds) / information value (predictive strength) |
| Application vs behavioral scoring | Deciding new applicants vs managing existing customers |
| Reject inference | Correcting for outcomes observed only on approved applicants |
| Roll rate | Flow between delinquency buckets month over month |
| Vintage analysis | Cohort default curves by origination period |
| Risk-based pricing | Interest rate set by predicted risk |
| IFRS 9 / CECL | Accounting frameworks for expected credit loss provisioning |
| Basel III / IRB | Bank capital regulation; internal-ratings-based approaches |
| Tradeline | One credit relationship in a bureau file |
| Debt service ratio / DTI | Payment burden relative to income |
| Subprime / prime | Market segmentation by credit quality |
| Collections ladder | Treatment sequence as delinquency deepens |
| Charge-off | Loan declared uncollectible; moved off book |

## Fraud & Financial Crime

| Term | Definition |
|---|---|
| CNP fraud | Card-not-present (online) fraud |
| Account takeover (ATO) | Attacker seizes a legitimate account |
| Synthetic identity fraud | Fabricated identities from real+fake attributes; matures slowly |
| First-party fraud | Customer defrauds from the start (bust-out) |
| APP scam | Authorized push payment scam — victim is tricked into paying |
| Velocity features | Counts/rates over sliding windows (txns, amounts, devices) |
| Behavioral biometrics | Typing/swipe patterns as passive authentication signals |
| 3DS / step-up | Risk-based authentication challenge at checkout |
| KYC / CDD / EDD | Know-your-customer and (enhanced) due diligence depth tiers |
| KYB / UBO | Know-your-business; ultimate beneficial owner |
| PEP | Politically exposed person (higher-risk customer class) |
| Sanctions screening | Matching customers against OFAC/EU/UN lists (fuzzy, high false-positive) |
| Structuring / smurfing | Splitting amounts to evade thresholds |
| Layering / placement / integration | The three classic laundering stages |
| Mule network | Accounts used to relay illicit funds |
| SAR / STR | Suspicious activity/transaction report filed to FIU |
| TM scenario | Rule or model pattern used in transaction monitoring |
| Alert fatigue | Analyst overload from false positives; the AML tuning problem |
| Trade-based laundering | Moving value via mis-invoiced trade flows |
| FATF | Global AML standard-setter (40 recommendations) |
| FFIEC BSA/AML manual | US supervisory examination manual for AML |

## Markets & Quant

| Term | Definition |
|---|---|
| OHLCV | Open/high/low/close/volume bar data |
| Corporate actions | Splits/dividends/tickers changing raw prices — adjust or leak |
| Survivorship bias | Backtesting only on assets that still exist |
| Alpha / beta | Return vs market / return from skill (in theory) |
| Sharpe / Sortino | Risk-adjusted return ratios (total vs downside vol) |
| Information ratio | Active return vs tracking error |
| Max drawdown | Worst peak-to-trough loss |
| VaR / CVaR (ES) | Loss threshold at confidence / expected loss beyond it |
| Duration / convexity | Bond price sensitivity to rates (first/second order) |
| Greeks (Δ Γ Θ ν ρ) | Option price sensitivities |
| Implied vol / vol smile | Market-imputed volatility; its strike-dependence |
| Black-Scholes | Canonical option pricing model (and its failure modes) |
| GBM / Itô | Geometric Brownian motion; stochastic calculus used to price/hedge |
| Monte Carlo | Simulation-based pricing/risk estimation |
| Limit order book | The queue of resting buy/sell orders; market microstructure state |
| Market impact / slippage | Your own trading moving the price; cost models (Almgren-Chriss) |
| TCA | Transaction cost analysis |
| Pair trading / cointegration | Mean-reverting spread between co-moving assets |
| GARCH / HAR-RV | Volatility clustering models / realized-vol forecasting |
| Purged K-fold / embargo | Cross-validation fixes for overlapping financial labels |
| Deflated Sharpe ratio | Correcting SR for selection across many backtests |
| Triple-barrier labeling | Label by profit-take/stop/time outcomes (López de Prado) |
| Factor model (FF3/Barra) | Explaining returns via systematic risk factors |
| Tracking error | Vol of active returns vs benchmark |

## Data & Engineering

| Term | Definition |
|---|---|
| Point-in-time correctness | Features must reflect only information available at decision time |
| Feature store | Offline/online feature platform ensuring train/serve parity (Feast) |
| CDC | Change data capture (Debezium): streaming DB changes |
| Lakehouse | Table formats (Delta/Iceberg) over object storage |
| Exactly-once semantics | End-to-end effect-once processing (and what it really covers) |
| Transactional outbox | Pattern making DB writes + event publishing atomic |
| Watermark | Streaming engine's notion of "events up to time T have arrived" |
| Event time vs processing time | When it happened vs when the system saw it |
| Hot key | Skewed partition key (whale account) starving throughput |
| Entity resolution | Linking records of the same real-world entity |
| PSI / CSI | Population/characteristic stability indices for drift |
| Data drift vs concept drift | Input distributions shift vs input-output relationship shifts |
| BCBS 239 | Risk-data aggregation principles: accuracy, completeness, lineage |
| Champion/challenger | Running candidate models alongside production models |
| Shadow deployment | Scoring with a candidate model without acting on it |
| Decision audit store | Immutable log of inputs, model version, output, reasons |
| Adverse action | Legally-notified decline with specific reasons (Reg B) |
| SR 11-7 | US supervisory guidance on model risk management (2011, still the anchor) |
| Model risk tiering | Ranking models by materiality for validation depth |
| DORA | EU operational-resilience regulation (applies Jan 2025) |
| EU AI Act | EU AI regulation; creditworthiness uses = high-risk |
| Differential privacy | Noise-based privacy guarantees for data/ML |
| Federated learning | Training across institutions without moving raw data |

## GenAI & Agents

| Term | Definition |
|---|---|
| RAG | Retrieval-augmented generation; ground LLM answers in retrieved docs |
| Hybrid retrieval | BM25 + dense vectors, fused (RRF) |
| Reranker | Cross-encoder reordering of retrieved candidates |
| Citation-forced answering | Every claim must resolve to a retrieved span |
| Faithfulness | Eval metric: answer supported by provided context (RAGAS-style) |
| Golden set | Curated eval items with expert answers/rubrics |
| LLM-as-judge (calibrated) | Model grading model — validated against human labels (κ) |
| Numeric fidelity | Every number in an answer verified against the source |
| Prompt injection | Instructions hidden in content hijacking the LLM |
| Tool use / function calling | LLM invoking schemas-backed operations |
| Human-in-the-loop gate | Mandatory approval step for regulated actions |
| Workflow vs agent | Deterministic orchestration vs autonomous tool loop |
| Trajectory eval | Grading the *path* of agent actions, not just the final answer |
| MCP | Model Context Protocol — standard tool/context integration |

---

*Terms marked by usage density across phases are Anki priorities. Extend this file as you read — a glossary you grow is a glossary you own.*
