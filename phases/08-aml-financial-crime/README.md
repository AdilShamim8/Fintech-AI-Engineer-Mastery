# Phase 08 — AML & Financial Crime

> **Stage III — Core Financial ML** · **Duration: 4 weeks** · **Mastery target: Application → Production**
> **Position in path:** `07-fraud-payment-intelligence` ← **this phase** → `09-financial-time-series`

## 1. Objective

AML is where machine learning meets the state: obligations written in law, thresholds negotiated with examiners, and outcomes measured in regulatory findings rather than AUC. This phase teaches the compliance frame (BSA/AML, FATF, the risk-based approach), the operational stack (KYC/CDD/EDD, sanctions screening, transaction monitoring, SAR filing), and the analytics that improve it — fuzzy matching, entity resolution, graph analytics, and GNNs — with the explainability investigators and validators actually need. You finish able to build screening and monitoring components and, more importantly, to reason about where ML legally and practically fits in a financial-crime program.

## 2. Why It Matters in Finance

Financial-crime compliance is one of the largest ML-adjacent cost centers in banking: large institutions publicly report spending in the billions annually on AML programs, and the overwhelming share funds operations driven by alert backlogs with notoriously low precision. Unlike fraud, AML is non-discretionary — the obligation exists whether or not the ML works — so the engineer's leverage is precision, coverage, and defensibility, not whether to comply.

- Sanctions failures carry existential consequences: publicly reported settlements (HSBC 2012 at $1.9B; BNP Paribas 2014 at roughly $8.9B) dwarf typical model failures — screening is a hard gate, not an optimization nicety.
- The Danske Bank Estonia case (publicly reported ~€200B in suspicious flows, 2007-2016) showed how weak transaction monitoring compounds across correspondent-banking networks.
- Industry alert precision is notoriously low (commonly reported in single-digit to low-double-digit percentages) — a standing, well-funded business case for better analytics under fixed analyst capacity.
- GNN-for-AML is a genuinely active research frontier (Weber et al. 2019 and continuing work on the Elliptic dataset through 2025) — one of the few places where cutting-edge deep learning meets mandatory regulation.

## 3. Prerequisites

- [ ] Phase 02 — payments, correspondent banking, transaction flows
- [ ] Phase 07 — alert economics, imbalanced learning, evaluation under capacity
- [ ] Phase 04 — probability, linear algebra, covariance/graph-adjacent math
- [ ] Existing ML skill: gradient boosting, embeddings basics, evaluation discipline (assumed known)

## 4. Learning Outcomes

- I can explain the risk-based approach and map BSA/AML and FATF obligations to concrete engineering artifacts.
- I can design KYC/CDD/EDD lifecycle workflows, including KYB and ultimate-beneficial-ownership determination.
- I can build a sanctions screening pipeline on the OFAC SDN list with fuzzy matching, blocking, and defensible thresholds.
- I can implement rule/scenario-based transaction monitoring and tune thresholds against alert-quality metrics.
- I can recognize laundering typologies (structuring, layering, mule networks, TBML, crypto laundering) as detectable patterns.
- I can walk the alert → triage → SAR workflow and explain its confidentiality and audit constraints.
- I can perform entity resolution across messy systems and quantify match uncertainty.
- I can run community detection and centrality analyses on transaction graphs and convert them into investigator-facing explanations.
- I can train and honestly evaluate a GNN on the Elliptic dataset against gradient-boosting baselines.
- I can distinguish trade-surveillance patterns (insider trading, spoofing) from AML monitoring.
- I can outline model validation for monitoring scenarios and investigator-facing explainability.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 08.1 | Regulatory frame: BSA/AML, FATF, risk-based approach | Obligations, standards, proportionality | obligation-to-artifact map |
| 08.2 | KYC/CDD/EDD lifecycle; KYB & UBO | Onboarding through ongoing due diligence | CDD workflow + UBO sketch |
| 08.3 | Sanctions & watchlist screening | OFAC SDN, fuzzy matching, FP economics | seeded-alias screener |
| 08.4 | PEP screening & adverse media | Risk tiers, unstructured signals | PEP flagging + review queue design |
| 08.5 | Transaction monitoring: rules & scenarios | Typology rules, threshold tuning, precision problem | rule engine on IBM AML data |
| 08.6 | Laundering typologies as patterns | Structuring, layering, mules, TBML, crypto mixers | typology pattern simulator |
| 08.7 | Alert triage & SAR filing | Workflow, deadlines, tipping-off constraints | workflow simulation w/ capacity |
| 08.8 | Entity resolution across systems | Deterministic + probabilistic linkage | entity-resolution eval |
| 08.9 | Graph analytics for AML | Communities, centrality, layering chains | NetworkX graph analysis |
| 08.10 | GNNs for AML | Weber et al. (2019) on Elliptic; honest evaluation | GNN vs GBM benchmark |
| 08.11 | Trade surveillance vs AML | Insider trading patterns, spoofing | spoofing-pattern simulator |
| 08.12 | Model validation for monitoring scenarios | Design logic, thresholds, coverage, documentation | scenario validation checklist |
| 08.13 | Explainability for investigators | "Why this alert": rules + attribution + graph context | explanation generator |
| 08.14 | Crypto compliance analytics | Clustering heuristics, mixers, evolving regulation | mixer-pattern detection lab |

**08.1 Regulatory frame.** The US Bank Secrecy Act requires AML programs, reporting (currency transaction reports, suspicious activity reports), and recordkeeping; FATF sets global standards via its Recommendations and evaluates countries against them; the risk-based approach directs firms to allocate controls proportional to assessed risk. This is why ML has an opening at all: risk is heterogeneous, and uniform rules are simultaneously too weak on the bad and too loud on the good. Build the map from obligation to engineering artifact — it is the skeleton of every later lesson.

**08.2 KYC/CDD/EDD lifecycle; KYB & UBO.** Customer due diligence is a lifecycle, not a form: identity verification at onboarding, ongoing monitoring, and enhanced due diligence for high-risk customers (PEPs, high-risk geographies, correspondent relationships). KYB extends this to corporate customers, where ultimate beneficial ownership means resolving ownership chains through nominees and shells — an entity-resolution problem wearing a legal hat. Design the workflow and its failure modes: stale data, dormant-account awakening, and review triggers.

**08.3 Sanctions & watchlist screening.** The OFAC SDN list is freely downloadable; screening means matching customers and counterparties against it and its alternate-name files — where transliterations, initials, and aliases force fuzzy matching (edit distance, phonetic algorithms, Jaro-Winkler via rapidfuzz). The economics are brutal: recall on sanctions is near-mandatory, so false positives dominate operating cost, and thresholds are risk decisions, not tuning knobs. Build a screener on seeded aliases and measure the precision/recall/threshold frontier yourself.

**08.4 PEP screening & adverse media.** Politically exposed person status raises risk tiers and triggers EDD; adverse media adds unstructured-signal screening that previews the NLP work of Phase 11. Both are lifecycle-aware — PEP status changes, and the systems must notice. The engineering deliverable is not just a flag but a review queue with rationale, since every flag consumes analyst time.

**08.5 Transaction monitoring: rules & scenarios.** Monitoring engines encode typologies as scenarios — velocity bands, structuring just-below-threshold patterns, high-risk geography mixes, rapid in-and-out movement — with thresholds tuned via historical replay. Industry alert precision is notoriously low, which is precisely the business case for ML prioritization on top of deterministic coverage. Build a small rule engine on IBM's synthetic AML data and report alert quality the way a compliance officer would.

**08.6 Laundering typologies as patterns.** Each typology is a detectable signature: structuring/smurfing (sub-threshold splits), layering chains (rapid hops through accounts/jurisdictions), mule networks (fan-in/fan-out at recruitment accounts), trade-based laundering (over/under-invoicing across counterparties), and crypto laundering (mixers, chain-hopping, peel chains). Simulate them synthetically so you know exactly what feature would catch each one — then check which of your Phase 07-style models would and would not see them.

**08.7 Alert triage & SAR filing.** Alerts flow through triage tiers (L1/L2), investigation, and — when suspicion is documented — SAR filing within regulatory deadlines, under strict confidentiality: tipping off the subject is prohibited, which constrains logging, dashboards, and access design more than most engineers expect. SAR narratives fuse transaction facts, entity history, and investigator judgment — the end-product your analytics must support, not replace.

**08.8 Entity resolution across systems.** The same person exists differently in onboarding, transaction, case, and list systems: name variants, typos, transliterations, shared addresses. Deterministic keys miss; purely probabilistic linkage (Fellegi-Sunter-style) scales with blocking strategies; errors propagate into every graph and risk aggregation downstream. Evaluate resolution quality explicitly — auto-merging confidently is how clean graphs become quietly wrong.

**08.9 Graph analytics for AML.** Money laundering is relational: fan-in/fan-out structures, cycles, and layering chains are invisible per-transaction but obvious in the graph. Community detection (e.g., Louvain), centrality (hubs, brokers), and shortest-path analysis of layering chains turn raw transactions into investigator-ready structure. Build the transaction graph from IBM AML data and produce one well-explained anomalous community.

**08.10 GNNs for AML.** Weber et al. (2019) is the canonical study: graph convolutional networks classifying licit/illicit nodes on the Elliptic Bitcoin dataset, with an active follow-on literature through 2025. Two disciplines matter: honest temporal splits (random splits on graphs flatter results — a documented criticism in this literature) and a fair gradient-boosting baseline on node features, which GNNs must actually beat. Run the benchmark and report both honestly.

**08.11 Trade surveillance vs AML.** Trade surveillance watches markets, not accounts: insider trading shows up as pre-event return drift concentrated near information-linked actors; spoofing shows up as order-book patterns of orders placed to be cancelled. The data differs (order books vs payments — see the FI-2010 limit-order dataset), but the epistemology is shared: inferring intent from patterns, with regulators as the customer.

**08.12 Model validation for monitoring scenarios.** Scenarios are models: validation covers design logic against the typology definition, threshold rationale, historical replay performance, coverage and overlap across scenarios, and population stability — with SR 11-7-shaped documentation (formalized in Phase 16). The cost of weak validation is not a metric, it is an examination finding. Draft the checklist you would want a validator to hold you to.

**08.13 Explainability for investigators.** An alert an analyst ignores is an alert that does nothing: explanations must combine the rule trace, model attribution, and graph context into "why this, why now." Explanation quality is an adoption constraint on all AML ML — design it as a first-class deliverable, not a SHAP plot appended to a JSON payload.

**08.14 Crypto compliance analytics.** Blockchain analytics clusters addresses (co-spend and peeling heuristics), attributes services (exchanges, mixers), and traces flows — Chainalysis-style typology research is public and worth reading. Heuristics decay as protocols and mixing services evolve, and the regulatory perimeter (travel rule, mixers' legal status) continues to move — treat current status as verify-before-claiming.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| String similarity & edit distance | Levenshtein, Jaro-Winkler, phonetic distances | Name matching across scripts and aliases | Screening thresholds become folklore |
| Record linkage (Fellegi-Sunter) | Probabilistic match classification from agreement patterns | Entity resolution across messy systems | Duplicated entities silently fragment risk |
| Graph metrics | Degree, centrality, paths, modularity | Laundering is topology: fans, cycles, chains | The structure of the crime stays invisible |
| Message passing & GNN aggregation | Neighborhood feature aggregation layers | Node classification on transaction graphs | You cannot evaluate the frontier AML research |
| Ranking under capacity | Precision@k, NDCG with analyst-hour budgets | The real objective: maximize detection per alert reviewed | You optimize classification; the floor needs ranking |
| Class imbalance & calibration | Posterior correctness under rare positives | Alerts are decisions with dollar consequences | Alert priorities that misprice suspicion |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Blocking & candidate generation | Fuzzy matching at population scale requires cheap weak-key blocking before scoring |
| Graph storage choices | NetworkX in-memory for analysis; Neo4j/Memgraph for persistent, queryable graphs |
| Audit trails & tipping-off constraints | Access control and logging design are compliance artifacts, not IT hygiene |
| SAR narrative tooling & data lineage | Every fact in a narrative must trace to source data reproducibly |
| Scenario threshold change control | Threshold edits are model changes: versioned, approved, documented |
| Screening latency tiers | Real-time gates (payments, onboarding) vs batch rescans on list updates |
| Investigator workflow integration | Alerts land in case management with context, or they land in the ignore pile |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| NetworkX | Graph construction, community detection, centrality, path analysis |
| PyG (PyTorch Geometric) | GNN models for node classification on transaction graphs |
| PyG Temporal | Temporal GNN extensions for evolving graphs |
| rapidfuzz | High-performance fuzzy string matching for screening |
| recordlinkage | Probabilistic entity-resolution pipelines |
| Neo4j / Memgraph (optional) | Persistent graph storage and query layer |
| LightGBM | The fair baseline and feature-based challenger everywhere |
| DuckDB | Fast profiling over OFAC/IBM/Elliptic data |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| FFIEC BSA/AML Examination Manual (ffiec.gov) | Regulation | All | US AML framework | The examiner's own playbook — read how you will be judged | Essential |
| FATF Recommendations & typology reports (fatf-gafi.org) | Standard | All | Global standards | The global frame plus typology reports that read like casebooks | Essential |
| FinCEN guidance & SAR resources (fincen.gov) | Regulation | All | Reporting obligations | Filing requirements and guidance from the US FIU | Essential |
| OFAC SDN list (treasury.gov) | Data | All | Sanctions data | Free, canonical watchlist you will screen against in projects | Essential |
| Wolfsberg Group guidance (wolfsberg-group.org) | Standard | Advanced | Industry standards | How global banks operationalize the risk-based approach | Recommended |
| Weber et al. (2019), "Anti-Money Laundering in Bitcoin: Experimenting with Graph Convolutional Networks for Financial Forensics" (KDD) | Paper | Advanced | GNNs for AML | The canonical GNN-for-AML study on the Elliptic dataset | Essential |
| Elliptic Bitcoin dataset | Dataset | Advanced | Graph ML | The public benchmark for AML graph learning | Essential |
| IBM synthetic AML transaction dataset | Dataset | Intermediate | Monitoring | Free, labeled synthetic transactions for scenario and ranker work | Essential |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| William L. Hamilton, *Graph Representation Learning* (free book, 2020) | Book | Advanced | Graph ML | The rigorous backbone under your GNN work | Recommended |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Chainalysis public research blog (typology reports) | Blog | Intermediate | Crypto typologies | Practitioner-grade laundering pattern research | Recommended |
| Quantexa / ComplyAdvantage public case studies | Blog | Intermediate | Entity resolution, screening | How the vendor market frames contextual monitoring | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Elliptic++ dataset papers | Paper | Advanced | Multi-modal graph data | Extends Elliptic with more modalities; research-frontier material | Reference |

## 10. Practical Exercises

1. - [ ] Download the OFAC SDN list (including alternate-name files); profile name variability — scripts, transliterations, initials, honorifics — and write a normalization memo.
2. - [ ] Build a fuzzy matcher with rapidfuzz: compare exact, normalized, token-sort, and Jaro-Winkler scoring; implement country/DOB blocking to control candidate blow-up.
3. - [ ] Seed 50 corrupted aliases (typos, initials, transliterations) into a customer table; measure precision/recall across thresholds; pick a blocking threshold with written rationale.
4. - [ ] On the IBM AML dataset, implement five scenarios (structuring fan-in/fan-out, high-risk geography mix, rapid movement, cycle detection, dormant-then-active); report alert precision and alerts-per-day cost.
5. - [ ] Threshold tuning under capacity: replay history at several thresholds; plot alert volume vs detection vs analyst-hours; choose and defend an operating point.
6. - [ ] Build the transaction graph; run Louvain communities plus degree/PageRank centrality; produce a one-page investigator briefing on the largest anomalous community.
7. - [ ] Train a GNN on Elliptic with a temporal train/test split; compare against LightGBM on node features; report both results plus a leakage discussion.
8. - [ ] Entity resolution on seeded duplicates across two mock systems; evaluate precision/recall at match thresholds; decide which merges require human review.
9. - [ ] Write three SAR-style narratives (from synthetic data), each with the data lineage every fact relies on — practice the explainability end-product.

## 11. Mini Projects

**M1 — Sanctions fuzzy-matching screener with blocking thresholds.** OFAC SDN + alternate names; seeded corrupted aliases; precision/recall across thresholds and blocking schemes; FP-cost analysis per analyst workflow. Deliverable: screener + threshold rationale memo. Difficulty: ★★☆☆☆.

**M2 — Rule-based transaction-monitoring engine on IBM AML data.** Five typology scenarios; replay-based threshold tuning; alert-quality metrics (precision, alert cost, typology coverage). Deliverable: engine + alert-quality report. Difficulty: ★★★☆☆.

**M3 — Community-detection features + investigator-facing explanations.** Graph built from IBM AML data; Louvain communities and centrality features; a "why this entity" briefing generator combining rule trace, graph context, and features. Deliverable: analysis + explanation artifacts. Difficulty: ★★★☆☆.

**M4 — GNN classifier on Elliptic vs gradient boosting.** Temporal splits, GCN/GraphSAGE-style models via PyG, LightGBM baseline on node features; honest comparison with leakage discussion. Deliverable: benchmark notebook + write-up. Difficulty: ★★★★☆.

**M5 — Alert-prioritization ranker with capacity constraints.** Learning-to-rank (e.g., LambdaMART-style) over alerts from M2; precision@capacity and NDCG as objectives; comparison vs score-threshold ordering. Deliverable: ranker + capacity analysis. Difficulty: ★★★☆☆.

## 12. Major Project Hook

This phase is the modeling core of **Flagship Project 3 — AML Transaction Monitoring Platform** (`/projects/flagship/`; see `/projects/flagship/README.md`): screening + monitoring + graph analytics + alert workflow; Phases 15 and 17 add the streaming and production layers, Phase 16 the formal governance.

## 13. Case Studies & Industry Examples

- **Danske Bank Estonia (publicly reported ~€200B in suspicious flows, 2007-2016)**: the emblematic correspondent-banking control failure — monitoring existed but was ineffective, poorly governed, and ignored (see `/case-studies/README.md`).
- **HSBC (2012)**: publicly reported $1.9B settlement over AML/sanctions lapses; HSBC later publicly reported significant investment in analytics and automation for financial-crime compliance — a rare public before/after arc.
- **BNP Paribas (2014)**: publicly reported settlement of roughly $8.9B for sanctions violations — the existential end of the screening-failure spectrum.
- **Weber et al. (2019) and the Elliptic dataset**: the research community's public benchmark; GNN-for-AML work continued actively on it through 2025, including prominent critiques of evaluation hygiene.

## 14. Interview Questions

**Why do AML systems have terrible precision, and what would you do about it?** Rules encode worst-case typologies at conservative thresholds because regulators punish misses, not noise. Fixes: ML prioritization under fixed analyst capacity, richer features (graphs, entity data), threshold governance with measured alert quality, and disposition-label feedback loops.

**Design sanctions screening end to end.** Ingest SDN + alternate names, normalize, block by weak keys, fuzzy-score (edit/phonetic/Jaro-Winkler), set thresholds with a recall-first rationale, route hits to review, measure FP cost per analyst, keep full audit trails, and re-screen on list updates and customer changes.

**Why graphs for AML?** Laundering is relational: mule networks fan in and out, layering chains hop through accounts, rings share infrastructure. Per-transaction features cannot see topology; graph features and GNNs expose exactly the structure the typologies operate through.

**Walk me through the alert → SAR workflow.** Alert generated → L1 triage (clear/escalate) → L2 investigation (entity view, graph, documents) → escalation decision → SAR drafted and filed within regulatory deadlines → dispositions feed back into rules and models — all under confidentiality (no tipping off) enforced in access and logging.

**How would you validate a monitoring scenario?** Design logic against the typology definition, threshold rationale, historical replay performance, scenario coverage and overlap, population stability, false-positive economics, SR 11-7-shaped documentation, and periodic re-tuning under change control.

**Why is entity resolution a prerequisite rather than a feature?** Graphs, risk aggregation, and UBO analysis all assume one entity is one node; unresolved duplicates fragment networks and quietly split risk across identities that regulators see as one.

**When does the GNN beat gradient boosting on node features?** When the signal is genuinely structural — ring membership, roles in layering chains — and node labels are dense enough to learn it; on Elliptic the gains are real but modest and highly split-sensitive, so benchmark with temporal splits or do not claim it.

**What makes SAR narratives hard to automate?** They fuse transaction facts, entity history, and investigator judgment under legal scrutiny; LLM assistance is emerging practice, but confidentiality (tipping-off) and accuracy obligations constrain it — verify current guidance before deploying anything generative here.

**How do you detect crypto mixing?** Address-clustering heuristics (co-spend, peeling chains), service attribution lists, and flow-based anomaly detection — with the caveat that heuristics decay as mixing services and protocols evolve.

**What is the biggest modeling sin in AML ML?** Optimizing alert ranking without the capacity constraint: the real objective is maximizing detection under analyst-hours — a budgeted ranking problem, not a classification problem.

## 15. Assessment — Can You Pass the Bar?

- [ ] Build a sanctions screener with measured precision/recall on seeded aliases and a defensible threshold (implementation item).
- [ ] Implement five monitoring scenarios and report alert-quality metrics under a fixed analyst budget.
- [ ] Produce a graph analysis with an investigator-facing explanation of a detected community.
- [ ] Benchmark GNN vs GBM on Elliptic with temporal splits and an honest leakage discussion.
- [ ] Walk a regulator (role-play) through your alert → SAR workflow and its audit trail (explain item).
- [ ] Draft a validation memo for one monitoring scenario, SR 11-7-shaped.
- [ ] Explain the false-positive economics of screening thresholds to a compliance officer.

## 16. Mastery Checkpoint

You may proceed to Phase 09 when:

1. A repo exists containing: screener, rule engine, graph analytics, GNN benchmark, and alert ranker — each with evaluation artifacts.
2. A one-page validation memo and investigator-explanation examples are on file.
3. You have recorded a 5-minute walkthrough of "why AML precision is low and how I would fix it" (store under `/notes/artifacts/`).
4. Dataset fluency: you can load and profile OFAC SDN, IBM AML, and Elliptic from memory.

Evidence: repo links, evaluation artifacts, recorded walkthrough. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Random splits on Elliptic or other temporal graphs — flattering GNN results; temporal leakage is a documented criticism in this exact literature.
- One global fuzzy threshold across name scripts and lengths — recall collapses silently for transliterated names.
- Threshold tuning as a pure workload exercise: cutting alert volume without measuring lost detections creates regulatory blind spots.
- Entity-resolution overconfidence: auto-merging ambiguous entities corrupts graphs and risk aggregation downstream.
- Forgetting tipping-off constraints in dashboards, logs, and LLM prompts — a compliance violation engineered by your own tooling.
- Treating KYC data staleness as transaction monitoring (and vice versa) — periodic review and continuous monitoring are different obligations.
- Optimizing GPU metrics while investigators ignore unexplainable alerts — adoption is the metric that ships.

## 18. Where This Goes Next

Phase 09 returns to the time dimension: once the entity and graph layer is solid, monitoring sequences and market surveillance become time-series problems your Phase 05 foundations extend naturally. Phase 16 formalizes the model-governance instincts you practiced here (SR 11-7, validation), and Phase 15 supplies the streaming architecture that real monitoring demands.
