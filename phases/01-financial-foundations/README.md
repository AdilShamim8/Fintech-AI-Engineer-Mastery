# Phase 01 — Financial Systems Foundations

> **Stage I — Domain Bridge** · **Duration: 3-4 weeks** · **Mastery target: Awareness → Working fluency**
> **Position in path:** `00-orientation` ← **this phase** → `02-banking-payments-lending`

## 1. Objective

This phase builds the mental model of the financial system that every later phase assumes: what money is (ledger entries), how banks create it, how central banks steer it, how time and risk turn into interest rates, and how markets, instruments, institutions, and infrastructure fit together. You will implement the core mechanics in Python — time value of money, bond pricing, a double-entry ledger — and wire them to real data through FRED. The target is fluency: you should finish able to read a central-bank decision, a bank balance sheet, or a yield-curve chart and reason about it precisely, in the vocabulary finance professionals use back at you.

## 2. Why It Matters in Finance

Every financial model is downstream of interest rates and the banking system. Credit models (Phase 06) price against funding curves; fraud and payments teams (Phase 07) reason about money movement; forecasting systems (Phase 09) live or die on macro context. Without this substrate, an AI engineer in finance produces models that are technically sound and commercially illiterate.

- Interest rates are the gravity of finance: discount rates, funding costs, and valuations all derive from the policy curve — misreading the macro environment is how models broke across the industry during the 2022-2023 rate shock.
- The bank balance sheet is the unit of accounting for most of the industry; you cannot model credit, liquidity, or deposits without reading one.
- Double-entry bookkeeping is the original immutable, balanced data model — and the schema most financial data engineering (Phase 03) actually implements.
- Yield-curve state (normal, flat, inverted) conditions loan pricing, recession risk, and portfolio behavior; measuring it from public data is a baseline skill.
- 2008 and SVB-2023 are recurring reminders that duration and liquidity risk are system-level phenomena your models operate inside, not edge cases you can ignore.

## 3. Prerequisites

- [ ] Phase 00 — fintech-lab repo with green CI, notes tree, conventions
- [ ] Comfortable Python: functions, classes, pytest, pandas basics
- [ ] High-school algebra; no prior finance assumed
- [ ] Willingness to read primary sources (central-bank explainer pages, one FOMC statement) slowly

## 4. Learning Outcomes

- I can explain money creation end to end: reserves, bank lending, deposits, and the monetary aggregates.
- I can record any financial event as a balanced double-entry transaction and enforce the invariants in code.
- I can read a bank balance sheet, identify its asset/liability structure, and name its three core risks.
- I can trace how a policy-rate hike transmits to money-market rates, bank funding, loan pricing, and asset prices.
- I can implement TVM, NPV, IRR, and amortization from scratch, with tests and explicit day-count conventions.
- I can build a yield curve from FRED data, measure inversions, and state what they have historically signaled (with hedges).
- I can classify instruments (equity, fixed income, money market, FX, commodity, derivative) by issuer, cash flow, and risk bearer.
- I can map the institutional landscape (banks, asset managers, insurers, exchanges, CCPs, CSDs, custodians) onto the trade lifecycle.
- I can explain what happened in 2008 and at SVB in 2023 using duration and funding language.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 01.1 | Money & monetary aggregates | Base money, M1/M2, money as ledger entries | FRED aggregate charts + note |
| 01.2 | Double-entry bookkeeping | Debits/credits, journals, ledgers, trial balance | ledger.py with invariant tests |
| 01.3 | Bank balance sheets & money creation | Assets/liabilities, loans create deposits, constraints | T-account simulator |
| 01.4 | Central banking & policy rates | Tools, transmission channels, statements | annotated FOMC/ECB statement |
| 01.5 | Interest rate mechanics: TVM | Compounding, discounting, day counts | tvm kernel functions |
| 01.6 | NPV & IRR | Appraisal, pitfalls, sign conventions | NPV/IRR functions + edge-case tests |
| 01.7 | Bonds & the yield curve | Pricing, YTM, duration intro, term structure | bond pricer + curve plot |
| 01.8 | Inflation & macro indicators | CPI/PCE, GDP, real vs nominal (Fisher) | FRED macro dashboard |
| 01.9 | FX basics | Pairs, quotes, cross rates, spot vs forward | cross-rate consistency checker |
| 01.10 | Markets taxonomy | Money vs capital, primary vs secondary, OTC vs exchange | market map (v1) |
| 01.11 | Instruments primer | Equities, bonds, money-market, FX, commodities, derivatives | instrument glossary cards |
| 01.12 | Institutions & infrastructure | Banks, AMs, insurers, exchanges, CCPs, CSDs, custodians | institutional map |
| 01.13 | Clearing & settlement | Trade vs clearing vs settlement, netting, T+1/T+2 | settlement lifecycle diagram |
| 01.14 | Case study: 2008 & SVB 2023 | Duration risk, liquidity risk, run dynamics | case note |

**01.1 Money & monetary aggregates.** Money is best understood as ledger entries — central-bank reserves for banks, commercial-bank deposits for everyone else — rather than physical cash. The aggregates (base money, M1, M2) layer these claims and are watched as policy and liquidity indicators. Chart M2 growth against CPI inflation from FRED and annotate the 2020-2023 episode, which restarted a long-running public debate about how much aggregates matter; hold the debate, not a conclusion.

**01.2 Double-entry bookkeeping.** Every financial event posts at least two entries whose debits equal credits — a five-century-old data-integrity constraint that many modern fintech data models still fail to honor. Learn accounts, journals, the general ledger, and the trial balance, then implement a minimal ledger whose invariant check rejects unbalanced postings. This artifact is the direct ancestor of Phase 02's payment engine and Phase 03's ledger schema.

**01.3 Bank balance sheets & money creation.** A commercial bank funds itself with deposits and equity and holds loans, securities, and reserves; when it lends, it creates a deposit — loans create deposits, constrained by capital and liquidity regulation and by demand for loans, not by a mechanical multiplier of reserves. Read a real bank balance sheet from its 10-K and identify the three risks it runs: credit, interest-rate, and liquidity. Build a T-account simulator that plays out lending rounds and shows the constraints binding.

**01.4 Central banking & policy rates.** Central banks steer the short end of the curve with a policy rate and balance-sheet tools (interest on reserves, repo operations, quantitative tightening) and communicate through statements that move markets within seconds. Trace one transmission chain — policy rate to money-market rates to bank funding costs to loan and deposit pricing to demand and asset prices — and note where it can stall. Read one FOMC or ECB statement and annotate every tool and rate it references.

**01.5 Interest rate mechanics: TVM.** Present value is finance's master equation: any cash-flow stream reduces to a discounted sum, and most disagreements are about the discount rate and the horizon. Compounding conventions matter enormously in practice — nominal versus effective rates, ACT/360 versus 30/360 day counts — and silently shift numbers by percent-level amounts. Implement fv, pv, npv, and amortize in your tvm kernel with convention flags and property-based tests.

**01.6 NPV & IRR.** NPV expresses value added in currency and is additive across projects; IRR expresses it as a rate and is intuitive to executives but can be multiple, nonexistent, or misleading when cash flows change sign more than once. Implement both, then deliberately break IRR on a two-sign-change cash-flow stream so you never trust it blindly. This is also perennial interview territory — the pitfalls are the question.

**01.7 Bonds & the yield curve.** Bond price and yield move inversely; the yield curve is the set of discount rates by maturity and encodes policy expectations plus a term premium whose decomposition is genuinely contested in the literature. Price a bullet bond, compute duration numerically and analytically, and plot a curve from FRED constant-maturity series. This lesson feeds Phase 04 (formal term-structure math) and Phase 09 (forecasting) — build it to be reused.

**01.8 Inflation & macro indicators.** CPI and PCE, GDP, unemployment, and PMIs are the context variables every financial model breathes; the real-versus-nominal distinction (the Fisher relation) determines whether a 5% yield is generous or stingy. Build a FRED dashboard and practice converting series to real terms. Note that macro series get revised — snapshot every pull, a Phase 03 discipline you start practicing now.

**01.9 FX basics.** Currency pairs quote base against quote; spreads are the retail cost of immediacy; cross rates must be internally consistent or triangular arbitrage (in markets) and your tests (in code) will catch them. Spot and forward rates differ by interest-rate differentials — covered interest parity conceptually, with no arbitrage argument you should be able to sketch. Build a converter plus a cross-rate consistency checker with tests.

**01.10 Financial markets taxonomy.** Money markets (short-term, wholesale: T-bills, repo, commercial paper) versus capital markets (equities, bonds); primary (issuance) versus secondary (trading); exchange versus OTC. The taxonomy predicts data frequency, venue behavior, transparency, and regulation for any instrument you meet. Produce a one-page market map you will extend through Phase 10.

**01.11 Instruments primer.** Equities are residual claims, bonds are contractual claims, money-market instruments are short-term IOUs, FX is the exchange of claims on currencies, commodities are physical or futures exposure, and derivatives (forwards, futures, options, swaps) are contracts that transfer risk rather than raise capital. For each: who issues it, who buys it, what the cash flows are, who bears which risk. Glossary cards now prevent embarrassing silence in Phase 10 and in every stakeholder conversation before it.

**01.12 Institutions & infrastructure.** Retail, commercial, and investment banks; asset managers; insurers; pensions; broker-dealers; exchanges; CCPs; CSDs; custodians — each holds specific books and runs specific risks. Draw the map of who owes whom when you buy a US equity through a broker: the chain reveals why settlement takes days, where operational risk concentrates, and why "the market" is really a stack of institutions.

**01.13 Clearing & settlement.** Trade is not cleared and cleared is not settled: clearing nets obligations and — with a central counterparty — novates counterparty risk onto the CCP; settlement finalizes the exchange of asset for cash. US equities moved to T+1 in May 2024, with the EU legislating a later move (verify current status); shorter cycles are pure operational-data pressure, a theme Phase 03 picks up. Diagram the lifecycle including failure points (fails, buy-ins).

**01.14 Case study: 2008 & SVB 2023.** Two publicly documented failures share one anatomy: maturity and duration mismatch combined with runnable funding. SVB in March 2023 carried large unrealized losses on held-to-maturity securities as rates rose, alongside concentrated uninsured deposits that a digital-age run emptied at record speed (figures per regulatory and press reporting). Write the case note using duration and funding language — this is exactly the vocabulary a risk officer expects from you, and the template for Phase 02's Wirecard-style control-failure notes.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Compounding & discounting | Growth of money forward, valuation of money backward | Every price, provision, and valuation is a discounted sum | You cannot read a single instrument quote |
| Annuity & perpetuity formulas | Closed forms for level cash-flow streams | Loans, mortgages, coupons, dividends | You rederive basic pricing badly, every time |
| NPV & IRR | Currency value of a project; its implied rate | Capital allocation and interview screens | You conflate ranking rules that disagree |
| Duration (introduction) | Price sensitivity to yield, in years | First-order interest-rate risk everywhere | The SVB case will read like bad luck, not arithmetic |
| Fisher relation | nominal ≈ real + expected inflation | Comparing yields across regimes; real-rate features | You misjudge whether 5% is cheap or rich |
| Expectations & probability-weighted value | Σ p·x under simple discrete outcomes | Expected-loss and scenario intuition before formal stats | You hand-wave the "expected" in expected loss |
| Day-count & accrual conventions | ACT/360, 30/360, ACT/ACT | Interest accrual on nearly every contract | Percent-level silent errors in every calculation |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| FRED API ingestion & caching | Every exercise uses public macro data; snapshot pulls because series get revised |
| Time-indexed pandas frames | Timezones, frequencies, and resampling are where time-series bugs live |
| Testing numeric code | Approximation assertions and property tests (NPV at IRR = 0) catch convention errors |
| Structuring a reusable finance library | Package layout, versioning, and docs — the tvm kernel is consumed by later phases |
| DuckDB for indicator analytics | Window functions and joins over years of FRED data without infrastructure |
| Reproducible plotting | Every chart is a script over a snapshotted dataset, not a manual screenshot |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| fredapi (or requests against the FRED REST API) | Programmatic access to rates, aggregates, and macro series |
| pandas | Time-indexed series, resampling, and joins for macro data |
| DuckDB | SQL analytics over downloaded FRED CSVs and later datasets |
| matplotlib | Yield curves, aggregates, amortization splits — every figure scripted |
| numpy | Vectorized cash-flow and discounting math |
| pytest | The test harness for your tvm kernel and ledger invariants |
| uv / Jupyter | Environment and reasoning notebook from Phase 00 |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| FRED (fred.stlouisfed.org) | Data | All | Macro & rates | Canonical free source for rates, aggregates, CPI; API-first | Essential |
| FRED API documentation (fred.stlouisfed.org) | Docs | Intermediate | Data access | Ingestion and revision-handling patterns for your monitor | Essential |
| BIS publications (bis.org) | Papers/Reports | Advanced | Banking & payments | Central-bank-grade analysis; the CPMI glossary returns in Phase 02 | Recommended |
| Federal Reserve explainer pages (federalreserve.gov) | Guides | Beginner | Monetary policy | Primary-source definitions of IORB, discount window, QT | Essential |
| ECB explainer pages (ecb.europa.eu) | Guides | Beginner | Monetary policy (EUR) | The euro-side counterpart; compare mandates and tools | Recommended |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Mishkin, *The Economics of Money, Banking and Financial Markets* (Pearson) | Book | Intermediate | Money & banking | The standard text behind lessons 01.1-01.4 | Essential |
| Brealey, Myers & Allen, *Principles of Corporate Finance* (McGraw-Hill) | Book | Intermediate | TVM, NPV, bonds | Selected chapters — the practitioner default on valuation basics | Essential |
| MIT OCW 15.401 Finance Theory I | Course | Intermediate | Finance theory | Free lectures and problem sets for TVM and curves | Recommended |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| investor.gov education pages (SEC) | Guides | Beginner | Instruments | Plain-English regulator content on products and their risks | Recommended |
| Aswath Damodaran's course materials (NYU Stern, pages.stern.nyu.edu/~adamodar) | Course/Notes | Advanced | Valuation | Deep free material on DCF and rates — beyond this phase, the right rabbit hole | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Ray Dalio, "How the Economic Machine Works" (video, economicprinciples.org) | Video | Beginner | Macro machine | A 30-minute mental model; watch critically and verify against Tier 1 | Recommended |
| Investopedia | Reference | Beginner | Terminology | Fast lookups; always cross-check with Tier 1 | Reference |

## 10. Practical Exercises

1. - [ ] Chart M2, CPI, and the fed funds target from FRED on one timeline; annotate 2008, 2020, and 2022-2023; write a 300-word note on what "money printing" does and does not show.
2. - [ ] Implement the tvm kernel: fv, pv, npv, irr (bisection or Newton), amortize; include day-count flags; property-test that NPV at IRR equals zero.
3. - [ ] Price a 5-year bullet bond; shift yield by ±100bp; compute duration numerically and analytically and reconcile the two.
4. - [ ] Pull DGS2, DGS10, and T10Y2Y from FRED; mark inversion episodes; overlay US recession windows and write the association in hedged language (historical pattern, not law).
5. - [ ] Build ledger.py: post(), balances(), trial_balance(); test that unbalanced postings are rejected; simulate one bank month (deposits, lending, interest accrual) through it.
6. - [ ] Compute real interest rates from nominal rates and CPI on FRED; plot them across 2010-2024; write a note explaining negative real rates to a non-finance friend.
7. - [ ] Build a cross-rate table from three FX pairs; test triangular consistency; compute a 3-month forward using the rate differential.
8. - [ ] Read one FOMC (or ECB) statement; annotate every rate and tool mentioned; write a plain-English translation of at most 300 words.
9. - [ ] Write instrument cards for eight instruments: issuer, buyer, cash flows, risk bearer, venue — one card each, stored as concept notes.

## 11. Mini Projects

**M1 — tvm-finance kernel.** Data: none (pure library). Task: a typed, documented, tested Python package for TVM, NPV/IRR, amortization, and bond pricing with day-count conventions. Deliverable: package in fintech-lab with green CI, reused by Phases 02 and 06. Difficulty: ★★☆☆☆.

**M2 — Yield-curve monitor.** Data: FRED (DGS series, T10Y2Y). Task: scripted ingestion → inversion metrics → recession-window overlay report, refreshable in one command. Deliverable: report + plot set + hedged interpretation note. Difficulty: ★★☆☆☆.

**M3 — Double-entry ledger simulator with invariant checks.** Data: synthetic bank scenario. Task: ledger core with balanced-posting invariants, account hierarchies, trial balance, and failure tests proving corrupt entries are rejected. Deliverable: ledger module + test suite + a one-page design decision record. Difficulty: ★★★☆☆.

## 12. Major Project Hook

No flagship yet — the phase-culminating build is your tvm-finance kernel: the math engine later phases consume for loan pricing (Phase 06), payment economics (Phase 02), and formal valuation (Phase 04, `../04-financial-mathematics/README.md`).

## 13. Case Studies & Industry Examples

- **Global financial crisis (2008)**: publicly documented leverage, securitized-credit losses, and wholesale-funding runs — the systemic case that duration and funding structure, not just asset quality, determine survival.
- **SVB (March 2023)**: widely reported unrealized securities losses plus concentrated uninsured deposits and a fast digital run — duration and liquidity as joint risks, in one clean modern case.
- **Yield-curve inversions**: historically preceded US recessions by months-to-years (a robust pattern in public FRED data, with contested mechanisms and varying lags) — practice hedged causal language now, before Phase 05 formalizes it.

## 14. Interview Questions

**Explain money creation — where does bank money come from?** When a bank lends, it credits the borrower's deposit account: loans create deposits. Reserves and capital constrain the system at the aggregate level; the old "multiplier of idle reserves" story is a simplification regulators themselves retired.

**Walk me through what happens when the central bank hikes 50bp.** The policy rate rises, money-market rates reprice immediately, bank funding costs follow, loan and deposit rates adjust, demand for credit cools, and asset values fall as discount rates rise — with lags and pass-through friction at every step.

**Why does an inverted yield curve signal recession risk, and why is it not a law?** Inversion compresses the spread banks earn borrowing short and lending long, tightening credit; it also reflects expectations of policy cuts. It has historically preceded recessions, but mechanisms are debated, lags vary, and false positives exist — report it as a conditional signal.

**Describe a commercial bank's balance sheet and its main risks.** Liabilities: deposits and wholesale funding; assets: loans, securities, reserves; equity is thin. The three risks: credit (loans default), interest-rate (assets and liabilities reprice differently), liquidity (deposits can leave faster than assets liquidate).

**NPV vs IRR — when do they disagree?** With non-conventional cash flows or mutually exclusive projects of different scale or duration: IRR can be multiple or misleading, NPV ranks correctly. Default to NPV; use IRR for communication.

**What is duration, intuitively and mathematically?** The weighted-average time to a bond's cash flows, which approximates the percentage price change per unit yield change; it is first-order interest-rate risk, and the SVB case is the standard illustration of ignoring it.

**What happens to an existing bond's price when market rates rise, and why?** It falls, because its fixed cash flows must be discounted at the new, higher rates — the size of the fall scales with duration. This arithmetic, not sentiment, is the whole story.

**What is the difference between clearing and settlement?** Clearing computes and novates obligations (netting, counterparty risk transfer via a CCP); settlement is the actual final transfer of asset versus cash. Confusing the two is a reliable junior signal.

**Why do real (inflation-adjusted) rates matter for ML features?** Nominal rates mix inflation expectations with true economics; real rates separate them, so features built on nominal series can encode inflation regimes instead of the behavior you think you captured.

**What is a CCP and which risk does it transform?** A central counterparty interposes itself between buyers and sellers, turning bilateral counterparty risk into exposure to the CCP — reducing network contagion while concentrating risk that must be mutualized and margin-haircut-managed.

## 15. Assessment — Can You Pass the Bar?

- [ ] Implementation: tvm-finance passes CI including the NPV-at-IRR property test; the bond pricer matches a reference case.
- [ ] Implementation: ledger.py rejects unbalanced postings and passes all invariant tests.
- [ ] The yield-curve report regenerates from a fresh FRED pull in one command.
- [ ] Explain money creation and policy transmission to a non-finance engineer in five minutes (record it).
- [ ] Explain SVB 2023 to a risk officer using duration and funding language, with hedged figures (the regulator-style item).
- [ ] Read one FOMC statement unaided and produce a plain-English translation.
- [ ] Place ten institutions in the trade lifecycle from memory on the institutional map.

## 16. Mastery Checkpoint

You may proceed to Phase 02 when:

1. The tvm-finance kernel, yield-curve monitor, and ledger simulator exist in fintech-lab with green CI and test coverage.
2. At least ten concept notes from this phase are written in your own words, including the 2008/SVB case note.
3. The recorded five-minute explainer (money creation or policy transmission) is stored under `/notes/artifacts/`.
4. PROGRESS.md is updated with artifact links and any pacing adjustments.
5. You re-scored the Phase 00 quiz sections covering money, banking, and rates and can show improvement.

Evidence: repo links, test run output, recorded explainer, updated gap map. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Learning the "money multiplier" folk tale as literal mechanics instead of the modern "loans create deposits, constrained by capital and liquidity" view.
- Day-count and compounding-convention bugs (ACT/360 vs 30/360, nominal vs effective) that silently corrupt every downstream number.
- Treating yield-curve inversion as a deterministic recession clock rather than a historical association with mutable lags.
- Confusing stocks with flows (balance-sheet vs income-statement quantities) — a bug that later corrupts feature engineering.
- Pulling FRED series without snapshotting: macro series are revised, and unreproducible inputs poison every experiment.
- Ignoring units and scales in FRED metadata (millions vs levels vs index values) — the most common chart error in the phase.
- Reading blog summaries instead of one primary central-bank page, then quoting the blog's framing in interviews.

## 18. Where This Goes Next

Phase 02 (`../02-banking-payments-lending/README.md`) zooms into the operating layer: the balance sheet becomes core banking and ledgers, the settlement timeline becomes payment rails, and the lending instinct becomes the loan lifecycle. Your tvm kernel gets its first industrial consumer there; Phase 04 later formalizes the math you have been using intuitively.
