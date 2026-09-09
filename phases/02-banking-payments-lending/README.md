# Phase 02 — Banking, Payments & Lending Operations

> **Stage I — Domain Bridge** · **Duration: 3-4 weeks** · **Mastery target: Awareness → Working fluency**
> **Position in path:** `01-financial-foundations` ← **this phase** → `03-financial-data-engineering`

## 1. Objective

Phase 01 gave you the system; this phase gives you the machine room. You will learn how banks actually run — core banking, general ledgers, end-of-day batch — how money moves across every major payment rail, how the four-party card model divides a $100 payment, and how loans live from origination to recovery. These are the operational processes your AI systems will observe, predict, and automate; engineers who cannot trace a payment or a loan end to end build models on data they misunderstand, and it shows in production.

## 2. Why It Matters in Finance

Payments are the highest-frequency data in finance and the substrate of fraud (Phase 07) and AML (Phase 08); lending is the largest balance-sheet activity and the anchor of consumer credit ML (Phase 06). Both are operational processes with state, timing, and failure modes — exactly the things an engineer must understand before modeling them.

- Payment data is an AI goldmine only if you understand authorization, capture, clearing, settlement, declines, retries, and disputes — misreading lifecycle state is the single largest source of label errors in payment ML.
- Interchange and fee economics decide what a payments business can spend on risk; a 10bp swing in fraud loss or chargeback rate visibly changes unit economics.
- The loan lifecycle (origination → servicing → collections → recovery) defines what data exists, when, for whom — it is the data-generating process behind every credit model.
- Rails differ in speed, cost, finality, and reversibility (cards vs ACH vs wires vs RTP/FedNow vs PIX/UPI), and every fraud, AML, and UX decision is rail-specific.
- Regulation shapes APIs and data directly: PSD2/FDX open banking, the ISO 20022 migration, PCI DSS scope — compliance is an engineering constraint, not an afterthought.

## 3. Prerequisites

- [ ] Phase 01 — balance sheets, settlement basics, the tvm-finance kernel
- [ ] Phase 00 — fintech-lab with green CI and notes discipline
- [ ] Python OOP and pytest (state machines and parsers are class-heavy)
- [ ] No prior payments experience assumed

## 4. Learning Outcomes

- I can contrast retail, commercial, and corporate banking by customers, margins, data, and risk.
- I can explain a core banking system: accounts, general ledger, and end-of-day batch cycles.
- I can place every major rail (cards, ACH, wires, SEPA, UK Faster Payments, RTP, FedNow, PIX, UPI) on a speed/cost/finality/reach map.
- I can walk a card payment through the four parties and compute who pays whom on a $100 transaction.
- I can model a payment lifecycle as an explicit state machine with idempotent retries and a dispute branch.
- I can explain what PCI DSS constrains and how tokenization shrinks scope (as literacy, not compliance training).
- I can read a pain.001 ISO 20022 message and describe what the MT-to-MX migration changes.
- I can explain open banking (PSD2, FDX): consent, AIS/PIS, and what the APIs expose.
- I can map loan lifecycle stages to their data, decisions, and ML opportunities.
- I can compute loss and combined ratios, and explain mortgage mechanics including amortization and LTV.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 02.1 | Banking segments | Retail vs commercial vs corporate/investment | segment map note |
| 02.2 | Core banking systems | Accounts, GL, EOD batch cycles | mini EOD batch simulation |
| 02.3 | Payment rails world map | Cards, ACH, wires, SEPA, UK FPS, RTP/FedNow, PIX, UPI | rails comparison table |
| 02.4 | The four-party card model | Issuer/acquirer/network/merchant, interchange economics | $100 payment economics sheet |
| 02.5 | Payment lifecycle | Auth → capture → clearing → settlement; declines, refunds | state machine diagram |
| 02.6 | Disputes & chargebacks | Reason codes, representment, liability shift | dispute flow note |
| 02.7 | PCI DSS in one lesson | What it constrains, tokenization, scope reduction | PCI scope memo |
| 02.8 | Wallets & BNPL | Stored credentials, pay-in-4 economics | BNPL flow note |
| 02.9 | Cross-border & correspondent banking | Nostro/vostro, chains, cost layers | correspondent diagram |
| 02.10 | SWIFT & ISO 20022 | MT vs MX, pain/pacs/camt, migration | pain.001 annotated sample |
| 02.11 | Open banking | PSD2, FDX, AIS/PIS, consent flows | API sketch + consent diagram |
| 02.12 | Loan lifecycle | Origination → servicing → collections → recovery | annotated lifecycle diagram |
| 02.13 | Mortgages mechanics | Amortization, LTV/DTI, escrow, securitization intro | amortization schedule |
| 02.14 | Insurance & wealth management | P&C vs life, loss/combined ratio, AUM fees | ratio worksheet |

**02.1 Banking segments.** Retail (deposits, cards, consumer credit), commercial (SME lending, working capital, treasury services), and corporate/investment banking (markets, syndication, custody, advisory) are different businesses under one brand: different customers, margins, data, and regulatory lenses. The AI work differs too — retail is high-volume decisioning, corporate is workflow- and document-heavy. Produce a segment map listing dominant products and the data each generates.

**02.2 Core banking systems.** A core banking system is accounts plus a general ledger plus batch processing: interest accrual, fees, statements, and limit checks run in end-of-day cycles, which is why balances "jump at midnight" and why snapshot-based features (Phase 03) are the norm. Simulate a mini EOD batch — accrue savings interest, post fees, produce a trial balance — and you are building the very system whose data your models will later consume.

**02.3 Payment rails world map.** Cards, ACH (batched, cheap, days), wires/RTGS (final, expensive), SEPA and SEPA Instant, UK Faster Payments, the US instant rails RTP and FedNow (FedNow live since 2023, with roughly 1,700+ connected institutions publicly reported as of 2025 — verify current figures), and state-backed mass rails PIX (Brazil) and UPI (India). Build a comparison table across speed, cost, finality, reversibility, and reach; every fraud and AML decision later depends on which rail a payment rode.

**02.4 The four-party card model.** Issuer (cardholder's bank), acquirer (merchant's bank), network (scheme), merchant — with interchange flowing from merchant side to issuer to compensate for credit, float, and fraud, plus scheme fees and processing fees layered on. Work the $100 example end to end with public ranges (consumer card-present interchange is roughly 1-2%, varying by region, scheme, and card type — hedge accordingly). Internalize who bears fraud liability and why networks keep pushing 3DS authentication.

**02.5 Payment lifecycle.** Authorization (validate card, reserve funds, run risk) → capture → clearing (exchange of financial information) → settlement (actual movement between banks), with refunds, returns, and chargebacks as branch states. Model it as an explicit state machine — implicit booleans like `is_paid` are how fintech incidents are born. This artifact becomes the transaction factory for Phases 07 and 08.

**02.6 Disputes & chargebacks.** Card disputes run on reason codes, evidence windows, representment, and liability shifts (3DS moves liability toward the issuer); dispute loss is a first-class cost line and a classic ML target. Note the structural asymmetry: instant-payment rails are typically irrevocable, so push-payment fraud lands on the payer unless rules say otherwise — the UK's APP-fraud reimbursement regime (effective October 2024, with a £85k cap and split PSP liability as publicly reported) exists precisely because of this.

**02.7 PCI DSS in one lesson.** PCI DSS constrains the storage, processing, and transmission of cardholder data — network segmentation, encryption, access control, monitoring — and the engineering lever is scope reduction: tokenize the PAN and most of your estate leaves PCI scope. Treat this as literacy, not compliance training: know what it constrains, why tokenization exists, and let compliance professionals own the rest.

**02.8 Wallets & BNPL.** Wallets store credentials or value, with device tokenization being the reason card data inside wallets is comparatively safe; BNPL splits a purchase into installments, earning merchant discount rates and late fees, typically with underwriting lighter than card credit — a direct Phase 06 tie-in. Regulatory attention to BNPL is growing in several jurisdictions (verify current status locally); the business works because repayment data and merchant economics subsidize the risk.

**02.9 Cross-border & correspondent banking.** Cross-border payments ride chains of correspondent banks that hold nostro/vostro accounts for each other, adding hops, FX conversions, fees, and latency; SWIFT messages coordinate the flow but settlement still happens bilaterally across those accounts. Trace a EUR-to-USD payment through two correspondents and count the cost layers — this is why instant-rail interlinking and ISO 20022 carry so much industry hope.

**02.10 SWIFT & ISO 20022.** SWIFT MT messages carry payments in semi-structured fields; ISO 20022 (MX) replaces them with XML families — pain (payments initiation), pacs (payments clearing and settlement), camt (cash management) — carrying structured parties and rich remittance data. The MT/MX coexistence period was extended into 2025 (verify current status). The payoff: better screening, richer analytics, cleaner reconciliation. Parse a pain.001 and feel what MT could never give you.

**02.11 Open banking.** PSD2 (EU) mandated account-information and payment-initiation APIs with customer consent; the US runs on the FDX industry standard with market-driven adoption. The exposed data — accounts, balances, transactions — is the substrate for categorizers, affordability checks, and Phase 03 pipelines. Sketch the consent flow and the AIS/PIS surfaces, and note that data quality in the screen-scraping era was the original problem APIs were built to fix.

**02.12 Loan lifecycle.** Origination (application, KYC, underwriting, pricing) → servicing (billing, payment processing, delinquency management) → collections (workout strategies) → recovery and charge-off. Each stage emits different data at different frequencies, and each is an ML surface: underwriting (Phase 06), prepayment and attrition, collections optimization, recovery prediction. Draw the lifecycle with data and decision annotations per stage — it becomes your map of credit-data reality.

**02.13 Mortgages mechanics.** Amortization (with your tvm kernel), LTV and DTI ratios, escrow, fixed versus adjustable rates, and the securitization chain that turns pools of loans into agency MBS. Freddie Mac and Fannie Mae loan-level datasets (canonical list) preview Phase 06's mortgage work; compute an amortization schedule and observe how little principal moves in year one — the fact that makes rate refinancing and early-default loss math interesting.

**02.14 Insurance & wealth management.** P&C insurance runs short-tail, high-frequency claims where combined ratio below 100% means underwriting profit; life insurance is long-tail and investment-driven; both live on float. Asset management earns fees on assets under management under mandates. These industries are heavy ML consumers (claims, pricing, personalization), and their ratio vocabulary appears constantly in finance conversations.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Amortization & effective APR | Level-payment loan math incl. fees | Every lending product quote and schedule | You cannot audit a loan offer or build servicing features |
| Interchange arithmetic | bps on volume, split by party | Payments unit economics and pricing decisions | The $100 exercise will be hand-waving |
| FX conversion chains & spread | Stacked conversions with margins | Cross-border cost modeling | You undercount cross-border cost by layers |
| Float & settlement timing | Value of money in transit | Why rails compete on speed; treasury behavior | You miss why instant rails matter commercially |
| Expected dispute/fraud loss | Probability × severity per transaction | Risk reserves and rule thresholds | You price risk as if all transactions were equal |
| Ratio analysis | Loss ratio, combined ratio | Insurance health in two numbers | Insurer business models stay opaque to you |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Explicit state machines | Payment state is the ground truth of payments ML; transitions need guards and auditability |
| Idempotency keys & retries | Networks retry; without idempotency you double-charge — the classic fintech outage |
| XML schema validation (lxml/xmlschema) | ISO 20022 value lives in validation; regex parsing throws that away |
| Rule engines for categorization | Rules are auditable and instant; the standard hybrid is rules plus ML (later) |
| Reconciliation matching & exceptions | Every settlement file meets an internal ledger; exceptions are where the truth hides |
| Sandbox-driven development | Stripe/Adyen test modes give realistic payment behavior without real money |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| Stripe test mode & docs | Reference implementation of lifecycle, idempotency, and disputes |
| Adyen sandbox & docs | Acquirer-side view: risk, settlement, and reporting shapes |
| ISO 20022 message schemas (iso20022.org) | The spec your pain.001 parser validates against |
| transitions (Python) | Declarative state machines with guards and triggers |
| lxml / xmlschema | XSD validation and parsing for ISO 20022 XML |
| pytest | Testing state transitions, idempotency, and parser edge cases |
| pandas / DuckDB | Reconciliation joins and categorizer evaluation |
| markdown/lint toolchain from Phase 00 | Keeping artifacts in CI-clean shape |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| BIS CPMI glossary & Red Book (bis.org) | Reference | Intermediate | Payment systems | The vocabulary and country-by-country system descriptions | Essential |
| ISO 20022 (iso20022.org) | Standard/Docs | Intermediate | Payments messaging | Message catalogs including pain.001 — your parser's spec | Essential |
| FedNow Service pages (federalreserve.gov) | Official | Beginner | Instant payments (US) | Primary facts: launch, limits, participants | Essential |
| RTP pages (theclearinghouse.org) | Official | Beginner | Instant payments (US) | The other US instant rail; compare capabilities | Recommended |
| UK Open Banking standards (openbanking.org.uk) | Standard | Intermediate | Open banking | The original API and consent standard set | Essential |
| FDX (financialdataexchange.org) | Standard | Intermediate | Open finance (US) | The US API standard; compare with PSD2 | Recommended |
| SWIFT ISO 20022 migration pages (swift.com) | Official | Intermediate | Cross-border messaging | Migration status and MT/MX coexistence facts | Recommended |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Benson, Loftesness & Jones, *Payments Systems in the U.S.* (Glenbrook) | Book | Intermediate | US payments | The classic industry primer; the four-party economics chapters alone justify it | Essential |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Stripe documentation & engineering blog (stripe.com) | Docs/Blog | Intermediate | Payment operations | The best public lifecycle, idempotency, and dispute docs | Essential |
| Adyen documentation & blog (adyen.com) | Docs/Blog | Intermediate | Acquiring & risk | Acquirer-side view of risk, settlement, and reporting | Recommended |
| Plaid documentation (plaid.com) | Docs | Intermediate | Open banking APIs | What bank-connected data looks like in practice | Recommended |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Brett King, *Bank 4.0* (Wiley, 2018) | Book | Beginner | Banking futures | Big-picture "banking as experience" framing; read critically | Optional |

## 10. Practical Exercises

1. - [ ] Build the rails comparison table: nine rails × speed, cost, finality, reversibility, reach — cite one primary source per row.
2. - [ ] Produce the $100 card-payment economics sheet: fee flows among merchant/acquirer/network/issuer with hedged public ranges; show merchant net revenue.
3. - [ ] Implement the payment lifecycle as a `transitions` state machine; add guard conditions and illegal-transition tests.
4. - [ ] Add idempotency: retry the same payment twice with the same key and test that exactly one settlement occurs.
5. - [ ] Parse and validate a pain.001 sample against its schema; extract totals; corrupt it deliberately and produce actionable error messages.
6. - [ ] Reconcile two files (expected settlements vs internal ledger): match on amount/reference/date-window with tolerance; generate an exception report.
7. - [ ] Build a rule-based transaction categorizer v0 over synthetic open-banking-style transactions (optionally scaffold volume with PaySim from the canonical list); hand-label 100 and report precision/recall.
8. - [ ] Compute a 30-year mortgage amortization schedule with the tvm kernel; plot interest vs principal over time; annotate the first-year principal share.
9. - [ ] Read a Stripe or Adyen payment-lifecycle doc; map their state names onto your state machine; write a divergence note (their vocabulary vs yours).
10. - [ ] Write a one-page "what PCI DSS does and does not require" memo in your own words, from official sources only.

## 11. Mini Projects

**M1 — Payment lifecycle engine.** Data: synthetic payment commands (Stripe-shaped). Task: state machine with auth/capture/clearing/settlement, idempotency keys, retry semantics, refund and dispute branches. Deliverable: CLI simulation + test suite proving retries cannot double-settle. Difficulty: ★★★☆☆.

**M2 — ISO 20022 pain.001 parser/validator.** Data: official sample messages. Task: XSD validation, semantic checks (control sum vs transaction sum), structured extraction to tables, error taxonomy. Deliverable: parser library with failing-case tests. Difficulty: ★★☆☆☆.

**M3 — Transaction categorizer v0.** Data: synthetic open-banking transactions (or PaySim scaffold). Task: rule-based categorization (MCC/keyword/amount heuristics), evaluated against 100 hand labels. Deliverable: rules engine + evaluation report + a note on where ML will be needed. Difficulty: ★★☆☆☆.

**M4 — Two-file reconciliation matcher.** Data: generated expected/actual settlement files with planted breaks. Task: fuzzy-tolerant matching, unmatched-pair detection, exception report with severities. Deliverable: matcher + exception report a banker would recognize. Difficulty: ★★★☆☆.

## 12. Major Project Hook

Phase-culminating build: your payment lifecycle engine becomes the transaction factory for the fraud and AML phases — Phases 07-08 (`../07-fraud-payment-intelligence/README.md`, `../08-aml-financial-crime/README.md`) reuse the same state model with adversarial agents added, and the reconciliation matcher returns as a production control in Phase 17.

## 13. Case Studies & Industry Examples

- **Wirecard (2020)**: publicly reported collapse after roughly €1.9B of claimed cash could not be verified — third-party-acquiring controls and reconciliation failures at scale; the canonical argument that reconciliation is a control, not a chore.
- **Anatomy of a $100 card payment**: the worked example every payments engineer should be able to give — fee flows, liability, and timing, with public interchange ranges as the hedge.
- **UK APP-fraud reimbursement (effective October 2024)**: publicly reported rules with a £85k cap, 50/50 send/receive PSP split, and a five-business-day refund SLA (verify current details) — instant payments shift fraud liability and change model incentives.
- **FedNow vs RTP adoption**: publicly reported participant counts differ meaningfully between the two US instant rails — a live example of how network effects, not technology alone, determine rail success.

## 14. Interview Questions

**Walk me through a card payment end to end.** Checkout → authorization (issuer validates card, funds, and risk) → capture → clearing (financial info exchanged) → settlement (funds move merchant-side) → optional refund/chargeback — with the four parties and liability shifting at 3DS as the key overlay.

**Why is settlement not clearing?** Clearing is the exchange and netting of obligations and details; settlement is the final, irrevocable movement of value between institutions. A payment can be cleared and pending settlement — and the difference defines who bears what overnight.

**What breaks when a payment is retried?** Without idempotency keys, retries create duplicate charges; with them, the same logical payment maps to one settlement. Also at stake: stale authorizations, double-counted webhooks, and state-machine races — design the key, the TTL, and the dedupe check.

**Who pays whom in interchange, and who ultimately funds card rewards?** The merchant's side pays interchange to the issuer via the acquirer; merchants embed it in prices, so effectively all cardholders and cash payers subsidize rewards-bearing cardholders — the regressive economics everyone knows and rarely says aloud.

**ACH vs wire vs instant rail — pick one per scenario and defend it.** Payroll: ACH (batch, cheap, predictable). Real-estate closing: wire (finality). P2P split of dinner: instant rail (speed, low value). The decision variables are cost, speed, finality, and reversibility.

**What is a nostro account and why do banks need them?** A nostro is a bank's account held at another bank in a foreign currency — the plumbing that makes cross-border settlement possible, and the reason correspondent chains add cost, latency, and screening checkpoints.

**What does ISO 20022 change versus MT?** Structured, richly typed XML instead of semi-structured text: unambiguous parties, structured remittance, better screening and reconciliation, and one vocabulary across rails — the data-quality upgrade that payments analytics has wanted for decades.

**What does PCI DSS actually require and restrict?** Controls over storing, processing, and transmitting cardholder data (encryption, segmentation, access, monitoring); the engineering move is tokenization so most systems never touch the PAN — compliance follows scope reduction.

**What is open banking and what do PSD2 APIs enable?** Regulated, consented access to bank accounts: account information (AIS) powers categorization and affordability; payment initiation (PIS) powers account-to-account payments — third parties build on bank data without screen scraping.

**Where does ML create value across the loan lifecycle?** Underwriting (risk and affordability), pricing, prepayment and attrition prediction, servicing personalization, collections segmentation and timing, recovery modeling — with the constraint that data exists only from origination onward and label delay is months.

## 15. Assessment — Can You Pass the Bar?

- [ ] Implementation: the payment state machine survives a retry storm (duplicate idempotency keys, out-of-order webhooks) without double-posting — test-proven.
- [ ] Implementation: the pain.001 parser rejects a corrupted message with an actionable error, and validates a clean one against the schema.
- [ ] The reconciliation matcher produces an exception report with severities a banker would recognize.
- [ ] Explain the $100 payment's fee flows to a product manager in five minutes (record it).
- [ ] Explain to a compliance officer why lifecycle state determines what data exists when — the regulator-style item.
- [ ] Place nine rails on the speed/cost/finality/reach map from memory.
- [ ] Produce the loan-lifecycle diagram with data and decision annotations for every stage.
- [ ] State the combined-ratio rule and why insurers celebrate sub-100 results.

## 16. Mastery Checkpoint

You may proceed to Phase 03 when:

1. M1-M4 all exist in fintech-lab with green CI and their test suites prove the hard properties (idempotency, validation, exception handling).
2. The rails table and $100 economics sheet are written with cited primary sources.
3. The recorded five-minute card-payment walkthrough exists under `/notes/artifacts/`.
4. PROGRESS.md is updated; the Phase 00 quiz payment/banking sections show improvement.
5. You have read one full lifecycle doc from Stripe or Adyen and reconciled its vocabulary with yours.

Evidence: repo links, test output, recorded walkthrough, rails table. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Treating "payment successful" as one state — authorization is not capture, settlement is not finality, and models trained on conflated states are garbage.
- Retrying without idempotency keys: the classic duplicate-debit outage, and the first thing a payments interviewer probes.
- Confusing authorization holds with captures when engineering balances — pending holds make balances look wrong to naive features.
- Learning only card rails: instant rails invert the model (push not pull, near-irrevocable), which relocates fraud from cards to social engineering.
- Parsing ISO 20022 with regex instead of schemas — you lose the validation that is half the value of the format.
- Assuming categorization is an ML problem only: labels are fuzzy and taxonomies drift; production systems run rules plus ML hybrids.
- Treating PCI DSS as "HTTPS for payments" — the actual lever is data minimization and tokenization, not encryption folklore.

## 18. Where This Goes Next

Phase 03 (`../03-financial-data-engineering/README.md`) converts this operational fluency into data engineering: ledgers become schemas, payment events become streams, and loan stages become point-in-time features. With the rails understood, Phases 07 and 08 can attack them adversarially — fraud and AML are, at heart, operations phases applied with malice.
