# Case Studies — Decisions Under Uncertainty in Financial AI

> **Ten cases, one method.** Every case is framed as a decision, not a story: what was decided, by whom, with what data, and what each error type cost. The nine-question template below is the curriculum's core analytical instrument — you will use it on every case here, on every project you build, and in every architecture review you ever run.
>
> **Position in path:** `../projects/README.md` (building) ← **this page** → `../phases/20-senior-architect/README.md` (judgment)

## 1. Why Case Studies

- Projects teach you to build; cases teach you to judge. The recurring failure pattern in financial AI is not bad models — it is good models attached to badly framed decisions (wrong threshold, wrong metric, wrong owner, wrong macro assumption).
- Each case below is drawn from publicly reported events, public datasets, or published research. Where numbers are disputed or estimated, the text says so — mirroring the claim-hygiene standard you must apply in your own writeups.
- The cases deliberately span build-failure, governance-failure, market-failure, and vendor-claim categories. Technical excellence does not immunize against any of them.

## 2. The Nine Questions

Apply these, in order, to every case — and to every model you ship:

1. **What is the business problem?** (Not "predict X" — the decision the business is trying to make.)
2. **What data exists?** (What was available, when, at what quality — and what was merely assumed.)
3. **What decision is being made?** (Who acts, how fast, with what options.)
4. **What does a false positive cost?** (Friction, review cost, lost revenue, customer harm.)
5. **What does a false negative cost?** (Direct loss, contagion, regulatory, reputational.)
6. **What model/approach fits?** (Fit to decision structure and error economics, not to fashion.)
7. **How should it be evaluated?** (Metrics that mirror the decision and the cost asymmetry.)
8. **How would it operate in production?** (Latency, human-in-the-loop, monitoring, feedback, failure behavior.)
9. **Lessons for AI engineers** (What changes in how you build and what you refuse to ship.)

## 3. The Case Studies

### Case 1 — Card Fraud at Scale: What Public Fraud Data Teaches

**What is the business problem?**
A card issuer or PSP must decide, per transaction in real time, whether to approve, decline, or challenge — trading fraud loss against customer friction under an operational budget for human review.

**What data exists?**
The two canonical public datasets: ULB Credit Card Fraud (~284k transactions, 492 frauds, about 0.17% positive rate, PCA-anonymized features) and IEEE-CIS/Vesta (~590k labeled transactions with hundreds of identity and transaction features). Both are snapshots — they lack true adversary adaptation, merchant economics, and post-decision feedback.

**What decision is being made?**
Per-authorization: allow, decline, or step-up/challenge; plus queue design — which transactions go to human review given fixed analyst capacity.

**What does a false positive cost?**
A declined legitimate purchase: lost revenue, customer frustration and attrition risk, call-center load. At scale this frequently exceeds fraud losses — publicly reported industry experience treats false declines as a first-order business problem, not noise.

**What does a false negative cost?**
The fraud amount plus fees, plus dispute-handling cost, plus (in some regimes) reimbursement exposure; plus the signal loss of undetected attack patterns.

**What model/approach fits?**
Calibrated gradient boosting on tabular features is the workhorse; the decision layer is a cost-sensitive threshold policy over the model score plus deterministic rules; hybrid rules+model designs dominate production for auditability and hard constraints.

**How should it be evaluated?**
PR-AUC on time-based splits (never random); precision at the operational alert capacity; expected profit per 1,000 transactions pricing both error types; value-weighted recall (fraud value is not uniformly spread across fraud count); calibration quality since thresholds consume probabilities.

**How would it operate in production?**
Sub-100 ms scoring path over streaming velocity features, decision logging with feature snapshots, analyst dispositions fed back as labels (with the censoring bias that creates acknowledged and managed), drift monitoring, and champion/challenger promotion.

**Lessons for AI engineers**
- Class imbalance is an economics problem before it is a sampling problem: undersampling changes priors and miscalibrates the probabilities your threshold consumes.
- A time-based split is not optional hygiene; random splits on event data fabricate performance.
- The binding constraint is review capacity, not AUC — precision@capacity is the metric operations actually lives on.
- Public datasets cannot show adversary adaptation; say so, and simulate the adversary before believing your robustness.

### Case 2 — UK APP Scams and the 2024 Reimbursement Rules: When Liability Rewrites Model Economics

**What is the business problem?**
Authorized push payment scams — customers tricked into paying fraudsters — historically sat in a liability gray zone because the customer authorized the payment. The UK's Payment Systems Regulator made reimbursement mandatory for eligible claims under its regime (policy statement PS25/5), effective 7 October 2024: an £85,000 cap per claim, with liability split 50/50 between the sending and receiving payment service provider, and a prompt-refund service expectation. Detection of scams (and mule accounts receiving proceeds) is now a direct balance-sheet exposure for PSPs.

**What data exists?**
Payment flows and account behavior within each PSP; scam typology intelligence shared through industry channels; claims history post-regime (new); public typology guides from regulators and consumer bodies. As with most fraud domains, outcome labels for "would have been a scam" are censored by whatever controls already fired.

**What decision is being made?**
At payment time: allow, delay, warn, challenge, or block an outbound payment that may be a scam; on the receiving side: restrict or investigate accounts exhibiting mule behavior; post-claim: reimburse or contest under the regime's rules.

**What does a false positive cost?**
A legitimate payment delayed or blocked — rent, deposit, invoice — plus complaint volume and (given subjective grounds rules around customer warnings) evidence-quality problems if the block is not well documented.

**What does a false negative cost?**
Reimbursement of the scam (up to the cap), investigation cost, plus the receiving-side half borne by the receiving PSP — meaning poor inbound mule detection now leaks value to *other* institutions' losses and vice versa.

**What model/approach fits?**
Outbound: behavior-sequence and payee-risk models, anomaly detection on first-time payees, and conversational-risk signals where available. Inbound: mule-account detection using graph features and inflow/outflow pattern models. Rules provide hard interdiction patterns; models rank and price the gray zone.

**How should it be evaluated?**
Value of scam payments detected at a bounded false-positive rate on legitimate payment volume; typology coverage against published typologies; time-to-interdiction; post-implementation claim-rate change — the regime's own KPI. Note that the 50/50 split creates a game-theoretic evaluation question: your inbound mule model affects losses at institutions you cannot measure.

**How would it operate in production?**
Real-time scoring on instant rails with warn-and-confirm journeys, case management with claims workflows, intelligence-sharing ingestion, and quarterly typology refreshes. Regulation changes the cost matrix in code: thresholds, playbooks, and even feature priorities get re-derived when liability moves — this case is the cleanest demonstration that model economics is a regulatory artifact.

**Lessons for AI engineers**
- The cost matrix is an input, not a constant: when a regulator moves liability, your optimum threshold moves with it — design thresholds as configuration with documented provenance.
- Liability shared across parties means your model's performance affects others' P&L; industry-level metrics (claims prevented, not alerts raised) become the honest lens.
- Rules that shift error costs (caps, eligibility, subjective grounds) belong in your evaluation harness explicitly, not in a compliance appendix.
- Regulatory facts here — effective 7 October 2024, £85k cap, 50/50 split — are exactly the kind of hard parameters you should verify against the current rulebook before relying on them in production.

### Case 3 — Synthetic Identity Fraud: The Fraud That Books Itself as a Credit Loss

**What is the business problem?**
Synthetic identity fraud combines real identifiers (often a genuine SSN — Federal Reserve whitepapers highlight use of children's and other "clean" SSNs) with fabricated PII to create a person who does not exist. The identity passes point-in-time KYC because each component checks out; the fraudster nurtures the credit file for months or years, then "bursts out" — drawing maximum credit across many institutions and disappearing. The Federal Reserve's synthetic identity fraud whitepapers (2019, with subsequent updates) document the mechanics, and a Boston Fed analysis (April 2025) cites industry estimates of synthetic identity fraud losses exceeding $35 billion in 2023 — treat such figures as industry estimates, not audited losses.

**What data exists?**
Application and account data; bureau files; SSN-issuance data (restricted — the Fed's research used it under arrangement); bankruptcy and charge-off records. The core data problem: institutions largely cannot see across each other, and much of the exposure hides in accounting categories.

**What decision is being made?**
Application decisions (is this applicant a real, creditworthy person?), account monitoring (is this seasoned file about to burst?), and loss classification (is this charge-off a credit loss or an unrecognized fraud loss?).

**What does a false positive cost?**
Declining or restricting genuinely thin-file real people — disproportionately young adults, new immigrants, and others with legitimate thin files — with the fair-lending and inclusion sensitivity that implies.

**What does a false negative cost?**
Potentially unbounded per-identity loss at burst-out: multiple institutions, multiple products, recovery near zero — and the loss is systematically misbooked as credit loss, corrupting both fraud metrics and credit models trained on that data.

**What model/approach fits?**
Identity-graph/entity-resolution approaches (shared SSNs, shared devices, shared addresses across "different" people), velocity and bureau-inquiry anomaly detection, "credit-invisible then suddenly active" pattern models, and survival analysis on file age. Point-in-time KYC alone structurally cannot catch a well-nurtured synthetic — the defense is longitudinal and relational.

**How should it be evaluated?**
Detection at burst-attempt time (recall on known synthetic cohorts); false-positive impact on thin-file legitimate segments (fairness-sensitive reporting); measurement of the accounting discovery dynamic — how much recognized fraud loss was previously booked as credit loss; time-to-detection from first application.

**How would it operate in production?**
Application-time screening plus longitudinal account monitoring feeding a case investigation workflow; cross-institution collaboration channels (where lawful); and a loss-reclassification feedback loop so the fraud book actually learns from its charge-offs. The Fed's research exists partly because the discovery dynamic — matching charge-offs against SSNs of deceased or minor holders — is how the industry learned the size of the problem; production systems should build equivalent self-audits.

**Lessons for AI engineers**
- Accounting categories are a data-quality layer: if losses are misclassified at source, no downstream model sees the truth — audit the label, not just the features.
- Point-in-time verification is necessary and insufficient; some fraud only exists in the time dimension (nurture-and-burst) — design features accordingly.
- Your false positives here land on inclusion-sensitive populations; evaluation must report segment impact, not just aggregate precision.
- Loss figures in this domain circulate as estimates; cite them as estimates with provenance, exactly as this case does.

### Case 4 — Upstart's AI Lending: Model Risk Meets the Funding Market

**What is the business problem?**
Upstart publicly built its business on ML underwriting for personal loans — using alternative data and claiming approval/rate advantages over traditional FICO-centric underwriting. The business problem behind the case: an underwriting model is only as good as its validation across macro regimes, and an originate-to-distribute funding model couples model performance to capital-market appetite.

**What data exists?**
Upstart's public SEC filings, shareholder letters, and prospectuses document the model-driven origination growth through 2021 and the 2022 stress; loan-level tape data circulated to investors; macro rate data is public. Independent loan-performance data is limited — a fact that is itself a lesson.

**What decision is being made?**
Whom to approve at what rate; how fast to originate; and implicitly, how much model risk the funding model can carry — when delinquencies rose in the 2022 rate shock, institutional loan buyers retreated, publicly reported, tightening the very funding the model needed to keep originating.

**What does a false positive cost?**
An approval that defaults: credit loss to whoever holds the loan, plus the reputational premium of "the AI lender" missing — with a coupling effect: losses scare off funding, shrinking volume.

**What does a false negative cost?**
A declined good borrower — lost origination income and market share during growth phases, which is why growth pressure tempts models toward the optimistic side exactly before a macro turn.

**What model/approach fits?**
 ML underwriting with alternative data is defensible; what the case tests is the surrounding system: macro-conditional calibration, cyclically tested scorecards, funding-stress alignment, and honest model risk management — the machinery Phases 06 and 16 teach, not a different model family.

**How should it be evaluated?**
Vintage performance across origination cohorts; calibration drift as macro conditions shift; stress-tested approval rates; funding-model correlation — what happens to the business when delinquency rises 200 bp; and comparison of claimed lift against a traditional-score baseline under the same constraints.

**How would it operate in production?**
Continuous calibration monitoring with macro indicators as covariates; pre-committed credit-box tightening triggers; model change governance with swap-set analysis; and investor-facing transparency on model limitations — the mitigation of coupled model-and-funding risk is organizational, not algorithmic.

**Lessons for AI engineers**
- A model trained only in a benign rate regime has never been validated — regime coverage is part of validation, not an afterthought.
- Distribution channels and models are coupled systems: a model that requires ever-growing volume to feed a funding machine carries institutional pressure toward optimism; name that pressure in your governance docs.
- Public filings (shareholder letters in particular) are underused, high-quality case material for AI engineers — read them as model-risk post-mortems.
- Hedge your claims exactly as this page does: report what is publicly documented, and be explicit about what independent verification cannot see.

### Case 5 — Danske Bank Estonia: A Control Failure at Scale, Not an Algorithm Failure

**What is the business problem?**
Between roughly 2007 and 2015, Danske Bank's Estonian branch processed what its own 2018 public investigation report and subsequent official investigations indicated was on the order of hundreds of billions of euros of suspicious, largely non-resident money flows. The business problem this case poses: how does a monitoring-and-compliance organization fail this completely, and what would have detected it?

**What data exists?**
Danske's published investigation report, Danish and European regulatory findings and press coverage, the widely reported figure of approximately EUR 200 billion in suspicious flows, and the publicly reported US DOJ settlement of about $2 billion in 2022. The internal alert-level data (what fired, what was closed, why) is partially public through the report — rare and valuable.

**What decision is being made?**
Continuous, distributed decisions: which alerts to close, which relationships to exit, whether a non-resident portfolio is acceptable, whether the branch's controls are real — each individually small, collectively a catastrophe of governance.

**What does a false positive cost?**
An unnecessary review, a de-risked relationship, lost fee income — the costs compliance leaders cite when defending thin monitoring.

**What does a false negative cost?**
The case itself: multibillion-euro enforcement, criminal exposure, management collapse, and the bank's full exit from the affected business. This is the upper bound of false-negative cost anywhere in financial AI.

**What model/approach fits?**
Honestly, none — the failure was governance: diluted local control, alerts that were closed without adequate investigation, data and systems that did not connect the branch to group-level monitoring, and incentives that de-prioritized the portfolio. The analytical lesson is that scenario rules and models are necessary but are only as strong as the organization that acts on them.

**How should it be evaluated?**
Not with model metrics: evaluate control design — independence of compliance, escalation paths, data completeness for monitoring, exit-decision discipline, and whether alert-closure quality is sampled and audited. The questions in this template that matter most here are #3 (who acts) and #8 (how it operates), not #6.

**How would it operate in production?**
Group-level visibility into branch data (the reported inability to see Estonian transaction data at group level was pivotal), independent validation of monitoring coverage, whistleblower channels, and hard-coded escalation when alert volumes or closure patterns look anomalous — monitoring the monitors is a systems-engineering task AI engineers can own.

**Lessons for AI engineers**
- "The model had good AUC" is meaningless if the alert goes into a queue that no empowered person reads; the decision loop, not the score, is the unit of effectiveness.
- Data silos are a compliance failure mode before they are a modeling inconvenience — the ability to join data across the group would have been the control.
- Governance artifacts (independence, escalation, audit trails) are what regulators examine after an event; build them with the same rigor as the model.
- When you evaluate a financial AI system, ask what happens when it is right but inconvenient — Danske is the canonical publicly reported answer.

### Case 6 — HSBC and Financial-Crime Analytics: Modernization After Enforcement

**What is the business problem?**
HSBC's 2012 settlement with US authorities (publicly reported at about $1.9 billion, deferred prosecution) for AML and sanctions failures — including cartel-related laundering exposure and unmonitored correspondent accounts — made financial-crime analytics an existential priority. The business problem: how does a global bank with decades-fragmented data and enormous alert volumes actually modernize detection without drowning its analysts?

**What data exists?**
The public settlement documents and court filings; subsequent public reporting on HSBC's analytics partnerships (publicly reported collaborations with specialist AML-analytics vendors, including graph-analytics-based contextual monitoring) and on its AI adoption for financial crime in public statements and industry reporting. Internal performance data remains, naturally, private.

**What decision is being made?**
Alert prioritization and investigation focus at massive scale; network-level risk assessment (which correspondent relationships, which customer networks); and technology-investment decisions — build vs buy, and how to validate vendor AI.

**What does a false positive cost?**
Analyst hours at global scale — alert backlogs are publicly cited industry pain — plus customer friction and de-risking pressure (with the documented industry debate about wholesale exit from correspondent relationships).

**What does a false negative cost?**
Enforcement-scale fines and criminal exposure; the 2012 settlement is the reference point.

**What model/approach fits?**
Graph analytics and entity-resolution for network context (the publicly reported direction of HSBC's partnerships), ML-based alert prioritization, and — the modern chapter — LLM-assisted investigation tooling. The pattern: AI augments analyst capacity within a control framework; accountability stays human.

**How should it be evaluated?**
Detection effectiveness vs alert-volume reduction on real alert populations (measured against historical SAR outcomes where usable); network-discovery quality (do graph methods surface known bad networks without drowning analysts?); and vendor-claim validation — the bank's own testing before trusting marketed performance.

**How would it operate in production?**
Analytics embedded in a tiered investigation workflow with full case audit trails; model governance for prioritization models (they decide where human attention goes — a model risk like any other); and continuous tuning against new typologies. Note the sequence: enforcement first, modernization second — the business case for AI in compliance is often written by regulators.

**Lessons for AI engineers**
- Legacy data fragmentation is the real adversary in financial-crime AI; data engineering is most of the project.
- Alert prioritization is a decision-allocation problem — your model directs scarce human attention, so it inherits full model-risk obligations.
- Vendor claims (including graph-AUI and, today, GenAI claims) require internal golden-set validation before production; "publicly reported partnership" is not evidence of performance.
- The 2012-to-modernization arc shows compliance AI budgets follow enforcement; understanding the regulatory driver is part of your job, not context you can ignore.

### Case 7 — Knight Capital, 2012: A Deployment Discipline Case Study

**What is the business problem?**
On 1 August 2012, market maker Knight Capital deployed updated trading code in a way that, per the SEC's 2013 order and widely reported public accounts, reactivated obsolete test code on one of eight production servers; the errant system sent a flood of erroneous orders into the market, and the firm reportedly lost about $440 million in roughly 45 minutes — an amount that effectively ended the independent company. The business problem: deployment, configuration management, and kill-switch design for systems that move money at machine speed.

**What data exists?**
The SEC's administrative order (2013), the Senate subcommittee report, contemporaneous press coverage, and Knight's public disclosures. This is one of the best-documented operational failures in finance — the details of the deployment error are public.

**What decision is being made?**
Continuous automated trading decisions — but the case is really about the meta-decision: what release, configuration, and emergency-stop processes a firm runs on systems with balance-sheet consequences.

**What does a false positive cost?**
In this framing, an erroneous automated "opportunity" — orders the system should never have sent; the cost was the loss itself plus market-integrity scrutiny.

**What does a false negative cost?**
An overly cautious deployment process costs hours of engineering time — a rounding error against the observed downside; this asymmetry is the entire argument for discipline.

**What model/approach fits?**
Not ML at all: configuration management, immutable/automated deployment with verification, dead-code removal policies, canary releases, automated anomaly detection on order flow (an ML-adjacent guardrail — self-trading and quote-stuffing patterns were visible in the tape within minutes), and kill switches that are rehearsed, not theoretical.

**How should it be evaluated?**
Deployment-process audits: can any server end up with a different config than its peers? Is there automated post-deploy verification? What is the measured time from anomaly to effective stop — and was a human in the loop who knew the procedure? These are testable properties; treat them like tests, not like policies.

**How would it operate in production?**
Every financial AI system inherits this case: model rollouts are deployments; feature pipelines have configuration; scoring services have flags that can silently activate old paths. The production answer is staged rollout with automated verification, config diffing as a deployment gate, anomaly alerts on output distributions in the first minutes after any change, and a rehearsed stop-everything procedure with named owners.

**Lessons for AI engineers**
- The deadliest financial AI failure mode is not a bad model — it is a deployment and configuration process that lets a stale, untested path execute with live money.
- Detect-then-stop in minutes is a solvable engineering problem; Knight's reportedly slow manual response is as instructive as the bug itself.
- Dead code and repurposed flags are recurring root causes; deletion is a security practice.
- Your MLOps stack (Phase 17) should be able to answer the Knight question — "prove every server runs the same audited code" — on demand.

### Case 8 — Zillow Offers, 2021: When a Pricing Model Owns the Balance Sheet

**What is the business problem?**
Zillow's iBuying program (Zillow Offers) used pricing models to buy homes directly, hold and renovate them, and resell — meaning its algorithm's output became inventory on the balance sheet, financed with debt. In November 2021, Zillow publicly announced it would wind down the program, citing, per its public statements, forecasting unpredictability and operational volatility; public filings recorded hundreds of millions of dollars in inventory write-downs and a large workforce reduction.

**What data exists?**
Zillow's public filings and shareholder letters (the wind-down and write-downs); public housing-market data (listings, price indices); contemporaneous reporting on the labor and supply constraints (renovation capacity, contractor scarcity) that management publicly cited alongside forecast error.

**What decision is being made?**
At what price to buy each home, how many to buy, how fast — a per-item pricing decision aggregated into a portfolio-position decision with leverage and operational dependencies.

**What does a false positive cost?**
Overpaying for a home that must later be sold at a loss — individually modest, catastrophic in aggregate when the buying pipeline keeps feeding during a regime shift.

**What does a false negative cost?**
A missed purchase in a rising market — lost opportunity, zero balance-sheet risk. The asymmetry (bounded upside miss, unbounded downside accumulation) is the crux the risk controls must encode.

**What model/approach fits?**
Price prediction is tractable; the failure was system design: feature lag (the case is widely analyzed as one where market conditions moved faster than listing-data features could reflect), confidence-interval blind aggregation, and risk limits that did not cap exposure to model disagreement. The fit is not a better regressor — it is prediction inside a position-management system: per-item uncertainty, portfolio exposure caps, buy-rate throttles keyed to realized-vs-predicted spreads, and macro-shift circuit breakers.

**How should it be evaluated?**
Realized-vs-predicted spread monitoring in near-real time; error stratified by market segment and vintage (did specific metros drift first?); uncertainty calibration of the pricing model; and decision-level metrics — inventory turn, per-home economics under stress scenarios — not just MAE on a test set.

**How would it operate in production?**
A buying pipeline where the model proposes and a risk layer disposes: exposure caps per market, automatic buy-rate reduction when realized spreads exceed tolerance, independent data checks (how stale are the features feeding this estimate?), and a documented wind-down playbook — which, notably, is how the program actually ended once losses breached tolerance.

**Lessons for AI engineers**
- Feature lag is a first-class failure mode: if your features describe a world that no longer exists, your model is confidently wrong with a delay you cannot see.
- When a model's output commits capital, evaluation must move from accuracy to decision-and-portfolio metrics — per-item error aggregates into exposure.
- Build the circuit breakers with the model: tolerance bands on realized-vs-predicted error with automatic de-risking, rehearsed before the regime shift, not after.
- This case generalizes directly to market-making, pricing, and any AI system whose output becomes inventory — the math of bounded-upside, unbounded-downstream decisions.

### Case 9 — Equifax, 2017: Blast Radius and the Data You Hold

**What is the business problem?**
In 2017, Equifax disclosed a breach publicly reported to have affected roughly 147 million consumers — driven, per public accounts including official investigations and congressional reporting, by exploitation of an unpatched Apache Struts vulnerability, compounded by inadequate asset visibility, delayed patching, certificate-expiry failures, and data sprawl across systems. The 2019 settlement with US regulators was publicly reported at up to about $700 million. The business problem: data-hygiene engineering — what you hold, where it is, who can reach it, and how fast you can patch and prove it.

**What data exists?**
Official investigation reports, the GAO report, congressional testimony, and the settlement documents — unusually complete public material on a breach's anatomy, from the initial vulnerability to the exfiltration and the response failures.

**What decision is being made?**
Continuous infrastructure and data decisions: patch cadence, asset inventory completeness, network segmentation, data minimization and retention, and breach-response decisions (notification timing, remediation offering) once compromise is detected.

**What does a false positive cost?**
In security terms, an over-broad alert or an over-cautious patch costs engineering hours and occasional downtime — noise the organization learns to tolerate or (dangerously) to ignore.

**What does a false negative cost?**
The case itself: nation-scale PII exposure, years of remediation, regulatory settlement, and durable reputational damage — with the twist that a credit bureau's product is trust in data stewardship.

**What model/approach fits?**
Again, not a modeling gap: the fit is engineering practice — asset inventory as a queryable system of record, vulnerability-scanning with enforced SLAs, segmentation that makes exfiltration paths expensive, least-privilege data access, encrypted-and-tokenized PII, and anomaly detection on data-access patterns (an ML surface AI engineers genuinely own). Post-breach, identity-protection and fraud-monitoring products became the remediation surface — with their own model risks.

**How should it be evaluated?**
Mean time-to-patch against known exploited vulnerabilities; inventory completeness (can you enumerate every system holding PII?); segmentation test results (red-team path exercises); access-anomaly detection coverage; and rehearsal evidence for the response runbook — properties you can audit, not aspirations.

**How would it operate in production?**
Continuous compliance automation: scanners feeding patch pipelines with SLA gates, access logs feeding anomaly models with human review, data classification driving storage decisions, and DORA-style operational-resilience expectations (applying to EU financial entities since January 2025) formalizing what good looks like — resilience engineering is now a regulatory surface, and AI systems sit inside it.

**Lessons for AI engineers**
- The data your models consume is a liability you manage: minimization, retention limits, and tokenization reduce the blast radius of any future incident.
- Your training data, feature stores, and logs are part of the attack surface — inventory them like production assets, because they are.
- Patching-and-inventory discipline sounds unglamorous; it is also what separates a vulnerability from a company-defining breach.
- Anomaly detection on access patterns is an AI-engineering deliverable with direct breach-detection value — a place where your skills meet security practice.

### Case 10 — Klarna's AI Assistant, 2024: Reading Vendor Claims Like an Engineer

**What is the business problem?**
In early 2024, Klarna publicly announced that its OpenAI-powered assistant, in its first month, had handled 2.3 million conversations, performed the equivalent work of about 700 full-time agents, and was expected to drive roughly $40 million in profit improvement for 2024 — per Klarna's own public statements. The business problem for every financial institution (and every AI engineer evaluating tools): how do you evaluate GenAI service-automation claims — vendor's or your own — with the rigor finance requires, when the headline numbers come from the party selling the story?

**What data exists?**
Klarna's public announcements and subsequent interviews; later public reporting and executive commentary in 2025 indicating a rebalancing toward human agents for service-quality reasons; industry discussion of the announcement's metrics. Independent audits of the original claims — such as the definition and measurement of "equivalent work of 700 agents" — are not publicly available, which is precisely the analytical point.

**What decision is being made?**
Automation strategy: how much customer-service volume to route to AI, at what quality floor, with what human fallback; and, for the audience of this curriculum, whether to believe and repeat headline metrics when making your own platform decisions.

**What does a false positive cost?**
Over-automating: customers with complex or sensitive issues stuck in loops, mis-sold or mis-explained products, compliance exposure in regulated conversations (complaints handling, financial-promotions rules), and brand damage — the costs that surfaced, in public commentary, as the company later rebalanced toward humans.

**What does a false negative cost?**
Under-automating: paying for human capacity on volume that AI handles perfectly well — real but bounded, and reversible.

**What model/approach fits?**
Constrained, tool-using assistants with routing tiers: LLM handling for high-volume informational queries, deterministic flows for regulated actions, human handoff as a designed path (not a failure state); plus an evaluation stack — deflection rate, escalation quality, CSAT/NPS deltas, complaint-rate deltas by topic, and audit sampling of conversations.

**How should it be evaluated?**
Treat claims like a model card you did not write: what is the denominator (all chats or resolved chats?), what counts as "handled", what is the quality floor (CSAT parity? complaint parity?), what is the time window (first month of a fresh deployment — cherry-pickable), and what got worse. The honest evaluation is a longitudinal dashboard of automation rate *and* quality metrics together, with pre-registered targets.

**How would it operate in production?**
Tiered routing with confidence thresholds, continuous conversation-quality sampling, complaint-topic mining feeding the router, human review of regulated conversation classes, and a policy that quality regressions automatically shrink automation scope — the inverse of the usual incentive, which is why it must be engineered deliberately.

**Lessons for AI engineers**
- Headline automation claims are marketing until decomposed: ask for denominators, definitions, time windows, and the metrics that did not improve.
- "Equivalent work of N agents" is not an auditable unit; insist on auditable ones (resolved-with-quality rate, complaint deltas, escalation accuracy).
- The publicly reported follow-up (re-hiring and rebalancing in 2025) is not a refutation of GenAI value — it is the normal lifecycle of an over-extrapolated first-month number; plan for the curve, not the point.
- When you present your own systems, publish the metrics you did not improve alongside the ones you did — it is the single cheapest credibility signal in this industry.

## 4. How to Run a Case-Study Session

A case study you only read teaches little; one you commit to on paper before reading teaches calibration. Run sessions solo or in a group of 2-6. Budget 30-60 minutes.

**Protocol**

1. **Brief (2 min).** Read only the case title and the first question's situation line. Do not read the commentary.
2. **Commit (10-15 min).** Write your own one-to-three-sentence answers to all nine questions before reading further — especially the two cost questions. Vague answers are not allowed; "higher costs" is not an answer. For the false-positive/false-negative questions, estimate in units: money, hours, customers, basis points.
3. **Compare (5-10 min, group mode).** Read answers aloud. Focus disagreements on #4/#5 (error economics) and #8 (production) — these are where judgment actually differs.
4. **Read the commentary (5 min).** Read the case's printed answers and lessons. Mark each of your nine answers: match / partial / miss. Do not negotiate with the text — the point is your calibration, not agreement.
5. **Extract (5 min).** Write one sentence: "The transferable rule from this case is ..." If you cannot state a rule, you have read a story, not a case.
6. **Log (2 min).** Append to `/notes/case-study-log.md`: date, case, self-score (x/9), and your extracted rule. Revisit old entries every quarter — calibration drifts.

**Facilitator notes (group mode)**

- Assign one participant to argue the regulator's view and one to argue the P&L owner's view during step 3; the tension is the lesson.
- Ban the words "depends" and "it's complicated" during step 2; forced specificity is the exercise.
- Rotate case selection across categories: fraud/AML (Cases 1, 2, 3, 5, 6), model-risk/macro (4, 8), operations/deployment (7, 9), GenAI/vendor claims (10).
- For deeper runs, pull the primary sources: SEC order for Case 7, Danske's published report for Case 5, PSR materials for Case 2, the Federal Reserve whitepapers for Case 3 — and verify that figures still match current public accounts.

## 5. Cross-Reference Map

| # | Case | Primary phases | Related flagship | Related projects |
|---|------|----------------|------------------|------------------|
| 1 | Card fraud at scale | [07](../phases/07-fraud-payment-intelligence/README.md), [15](../phases/15-real-time-streaming/README.md) | [Flagship 01](../projects/flagship/01-real-time-fraud-detection-platform.md) | P06, P11 |
| 2 | UK APP scams & reimbursement rules | [02](../phases/02-banking-payments-lending/README.md), [07](../phases/07-fraud-payment-intelligence/README.md) | [Flagship 07](../projects/flagship/07-payment-intelligence-engine.md) | P06 |
| 3 | Synthetic identity fraud | [01](../phases/01-financial-foundations/README.md), [06](../phases/06-credit-risk/README.md), [08](../phases/08-aml-financial-crime/README.md) | [Flagship 02](../projects/flagship/02-credit-risk-decisioning-system.md) | P05, P10 |
| 4 | Upstart's AI lending | [06](../phases/06-credit-risk/README.md), [16](../phases/16-security-compliance-responsible-ai/README.md) | [Flagship 02](../projects/flagship/02-credit-risk-decisioning-system.md) | P05, P12 |
| 5 | Danske Bank Estonia | [08](../phases/08-aml-financial-crime/README.md), [16](../phases/16-security-compliance-responsible-ai/README.md) | [Flagship 03](../projects/flagship/03-aml-transaction-monitoring-platform.md) | P20 |
| 6 | HSBC financial-crime analytics | [08](../phases/08-aml-financial-crime/README.md), [14](../phases/14-agentic-fintech/README.md) | [Flagships 03, 06](../projects/README.md) | P20, P23 |
| 7 | Knight Capital 2012 | [15](../phases/15-real-time-streaming/README.md), [17](../phases/17-production-fintech-ai/README.md) | [Flagship 01](../projects/flagship/01-real-time-fraud-detection-platform.md) | P11, P16 |
| 8 | Zillow Offers 2021 | [09](../phases/09-financial-time-series/README.md), [10](../phases/10-quantitative-finance/README.md), [16](../phases/16-security-compliance-responsible-ai/README.md) | — | P17, P28 |
| 9 | Equifax 2017 breach | [03](../phases/03-financial-data-engineering/README.md), [16](../phases/16-security-compliance-responsible-ai/README.md), [17](../phases/17-production-fintech-ai/README.md) | — | P16 |
| 10 | Klarna AI assistant 2024 | [12](../phases/12-generative-ai-finance/README.md), [13](../phases/13-financial-rag-knowledge/README.md), [14](../phases/14-agentic-fintech/README.md) | [Flagships 05, 06](../projects/README.md) | P14, P23 |

Project IDs reference the master index in [`../projects/README.md`](../projects/README.md); phase numbers reference the phase directories. Claims in this page are hedged by design — before repeating any figure or regulatory detail (especially the APP reimbursement parameters and the industry loss estimates), verify against the current primary source.
