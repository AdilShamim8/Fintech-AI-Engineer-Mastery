# Phase 19 — Specialist Tracks & Research Frontier

> **Stage VI — Production & Leadership** · **Duration: ongoing/elective — 4+ weeks per track** · **Mastery target: Specialization**
> **Position in path:** `18-capstones` ← **this phase** → `20-senior-architect`

## 1. Objective

Generalist fintech AI engineers are employable; specialists are sought. This phase is an electives menu: pick two or three tracks and go deep — quantitative/trading ML, WealthTech, InsurTech, graph ML, digital assets, RegTech, privacy-enhancing technologies, or treasury AI. Each track gives you a purpose statement, curated resources, one project spec, and a skill checklist. The phase ends where the frontier begins: a standing pointer into `/research/` and a research agenda you update as the field moves.

## 2. Why It Matters in Finance

Depth is how compensation and influence compound in this field: banks, funds, and fintechs pay premiums for the engineer who owns a hard niche — execution analytics, pricing GLMs, entity resolution, or federated learning — rather than one more generalist. Tracks also future-proof you: regulatory and market shifts (EU AI Act phase-in, instant payments, tokenization pilots) create demand spikes that only specialists can serve credibly. Choose by market demand you can verify, genuine interest you can sustain, and the base it builds on your capstone.

- Specialization compounds with your flagship: a fraud-platform capstone plus graph ML becomes fraud-ring detection expertise, not two disconnected artifacts.
- Several tracks map to hiring categories with explicit tooling expectations (quant research, pricing actuarial-adjacent, AML analytics) — the checklists below mirror them.
- Research-frontier literacy (papers, datasets, reproducibility) is a differentiator that interviews test more often than job ads admit.
- Tracks are deliberately uneven in maturity: some are established practice, some emerging — each section flags which is which, so you do not build a career on vapor.

## 3. Prerequisites

- [ ] Phase 18 capstone complete and defensible (specialization lands better on a finished base)
- [ ] Phase 09/10 for Track A; Phase 06/08 for Tracks B/D/F; Phase 07 for Track G; Phase 03 for Track H
- [ ] Reading fluency in academic papers (or willingness to build it — see `/research/README.md`)
- [ ] Time budget honestly assessed: 4+ weeks per track at 6-10 hours/week

## 4. Learning Outcomes

- I can select 2-3 tracks using verifiable market demand, my capstone base, and sustained-interest evidence — and defend the choice in writing.
- I can enter any chosen track's literature and tooling without hand-holding, producing a working project within 4-6 weeks.
- I can read a research paper critically (claims, data, baselines, reproducibility) and extract what is production-relevant versus frontier-only.
- I can maintain a personal research agenda that tracks a frontier (models, datasets, regulation) over quarters.
- I can present a specialist topic to a generalist audience in writing and on a whiteboard.
- I can decide when a frontier technique is NOT yet production-appropriate, and document that judgment.

## 5. Core Concepts (Lessons)

Each lesson below is a specialist track: pick 2-3, not all eight. A track is complete when its mini project ships and its skill checklist is evidenced.

| # | Track | Focus | Best base |
|---|---|---|---|
| 19.A | Quant / Trading ML deep | execution, RL for trading, microstructure | Phases 09-10 |
| 19.B | WealthTech & personalization | robo-advisory, NBA, suitability | Phases 02, 06 |
| 19.C | InsurTech analytics | GLM/Tweedie pricing, claims, telematics | Phase 06 |
| 19.D | Graph ML deep | temporal graphs, entity resolution, fraud rings | Phases 07-08 |
| 19.E | Digital assets & DeFi risk | on-chain analytics, oracle/smart-contract risk | Phase 08 |
| 19.F | RegTech / SupTech | regulatory change, supervisory tech | Phases 13, 16 |
| 19.G | Privacy-enhancing technologies deep | FL, DP, secure computation | Phase 16 |
| 19.H | Treasury & liquidity AI | cash forecasting, FX, collateral | Phases 04, 09 |

**19.A Quant / Trading ML deep.** Purpose: operate at the markets end of the craft — order books, execution costs, and the brutal overfitting discipline that separates real quant ML from backtest theater. Established practice in funds; frontier in most fintechs. This track extends Phase 10 and pairs with `/research/`.
Resources: López de Prado, *Advances in Financial Machine Learning* (Wiley 2018); López de Prado, *Machine Learning for Asset Managers* (Cambridge 2020); Harris, *Trading and Exchanges* (Oxford 2003) for microstructure; FI-2010 LOB dataset (from the Zhang et al. 2019 LOB paper); FinRL (Liu et al. 2020) as RL-for-trading reference code.
Mini project: limit-order-book direction/impact model on FI-2010 with realistic cost assumptions and a combinatorially-purged cross-validation report.
Skill checklist: - [ ] explain CSCV/deflated Sharpe and why naive backtests lie - [ ] model execution costs in any strategy backtest - [ ] read a market-microstructure paper and restate its alpha claim skeptically - [ ] run one RL agent in a simulated venue and articulate why it is not production.

**19.B WealthTech & personalization.** Purpose: build the advisory layer — robo-advisory architecture, goal-based planning, and next-best-action systems that respect suitability constraints (MiFID II in the EU, FINRA suitability in the US). Established practice with heavy governance. Recommendation craft meets fiduciary duty here.
Resources: CFA Institute materials on goal-based wealth planning; Ricci et al. (eds.), *Recommender Systems Handbook* (2nd ed., Springer 2015); public robo-advisor disclosures (SEC filings of listed platforms) for architecture hints; FINRA suitability/Reg BI notices (finra.org); MiFID II product-governance summaries.
Mini project: next-best-action engine on synthetic client data — candidate actions, uplift-style propensity scoring, suitability filters as hard constraints, and an audit log of why each action fired.
Skill checklist: - [ ] design a robo-advisory decision stack (risk profiling → allocation → rebalancing) - [ ] implement suitability constraints as constraints, not post-filters alone - [ ] explain uplift vs propensity in NBA and when each is honest - [ ] audit-log a recommendation for suitability review.

**19.C InsurTech analytics.** Purpose: own pricing and claims analytics — Tweedie/GLM ratemaking tradition now merging with ML, claims triage, telematics features, and parametric products. Established actuarial practice; ML layer still maturing — a rare lane where regulated depth meets greenfield tooling.
Resources: CAS monograph *Generalized Linear Models for Insurance Rating* (Peng Zhao); Charpentier (ed.), *Computational Actuarial Science with R* (CRC 2014); French motor third-party liability portfolio (CASdatasets R package); telematics/UBI literature reviews (search journals; verify recency); Lloyd's Lab / parametric insurance public explainers.
Mini project: pure-premium model on the French MTPL portfolio — Tweedie GLM vs gradient boosting with monotonic constraints; rate-disruption and lift charts as the actuary would read them.
Skill checklist: - [ ] fit and defend a Tweedie GLM for pure premium - [ ] read an actuarial rate filing and translate its logic - [ ] build a lift/double-lift comparison of GLM vs ML - [ ] explain telematics feature risks (privacy, proxy discrimination).

**19.D Graph ML deep.** Purpose: model the relational structure finance actually runs on — payments, accounts, counterparties — with temporal GNNs, entity resolution, and fraud-ring detection, plus explainability for the investigators who consume scores. Active research frontier with established AML niches (GNN-for-AML work on the Elliptic dataset has been active through 2025).
Resources: Hamilton, *Graph Representation Learning* (free online, 2020); Weber et al. (2019), "Anti-Money Laundering in Bitcoin: Experimenting with Graph Convolutional Networks for Financial Forensics" (Elliptic dataset paper); PyTorch Geometric documentation (pytorch-geometric.readthedocs.io); Ying et al. (2019), "GNNExplainer"; IBM Synthetic AML dataset documentation for transaction-network data.
Mini project: fraud-ring detection on IBM Synthetic AML — build the transaction graph, resolve entities (fuzzy + deterministic rules), train a temporal GNN baseline, and ship GNNExplainer reports investigators can read.
Skill checklist: - [ ] design node/edge features for a payment graph under point-in-time rules - [ ] run entity resolution and measure its error budget honestly - [ ] train and ablate a temporal GNN baseline - [ ] explain a flagged subgraph to a non-ML investigator.

**19.E Digital assets & DeFi risk.** Purpose: risk engineering for on-chain rails — analytics over public ledgers, oracle risk, smart-contract and bridge risk, and the AML/TF lens regulators apply. Flag: emerging practice; tooling and standards are unstable, and several "risk models" are marketing. Verify everything; assume volatility in both assets and claims.
Resources: Elliptic Bitcoin dataset (Weber et al. 2019) for on-chain labeling; Chainalysis public research/blog (chainalysis.com) for typologies; rekt.news for publicly documented exploit postmortems; BIS papers on DeFi and stablecoins (bis.org); NIST/industry primers on blockchain risk where current (verify).
Mini project: on-chain risk scoring on the Elliptic dataset plus a written typology study of three publicly reported exploits (oracle manipulation, reentrancy, bridge compromise) and the detection features each implies.
Skill checklist: - [ ] build labeling pipelines over public ledger data - [ ] explain oracle/bridge/smart-contract risk classes with real examples - [ ] map DeFi risks to AML/sanctions obligations - [ ] distinguish auditable risk signals from hype.

**19.F RegTech / SupTech.** Purpose: automate the compliance factory and its supervisors — regulatory-change monitoring, horizon scanning, regulatory reporting, and supervisory technology (SupTech) that regulators themselves deploy. Established demand on the RegTech side; SupTech is institution-facing and growing (BIS publishes regularly).
Resources: BIS papers on SupTech and regtech (bis.org); FCA Digital Regulatory Reporting materials (fca.org.uk); your jurisdiction's rule-change feeds (Fed/FCA/ESMA registers); CFPB complaint-data tooling as supervision-adjacent data; vendor landscape scans (ComplyAdvantage and peers — landscape only, no endorsement).
Mini project: regulatory-change radar — ingest rule-register feeds (Fed/FCA/ESMA), embed and cluster changes, and generate impact summaries mapped to your model inventory from Phase 16.
Skill checklist: - [ ] build a regulatory change-ingestion pipeline with provenance - [ ] map a rule change to affected systems/models automatically (draft) - [ ] explain SupTech to a compliance officer in their vocabulary - [ ] assess vendor claims in this market skeptically.

**19.G Privacy-enhancing technologies deep.** Purpose: make cross-institution and cross-border ML legally and technically possible — federated learning with Flower, differential privacy with real budgets, and secure computation/TEE patterns. PETs are moving from research to regulated production; EU AI Act-era data-governance duties raise their value (Phase 16's concepts, now at depth).
Resources: Flower documentation (flower.ai); Dwork & Roth, *The Algorithmic Foundations of Differential Privacy* (2014, free); Opacus documentation; Google research blog/papers on federated analytics (verify specific papers); Kairouz et al. (2021), "Advances and Open Problems in Federated Learning" for the map of the field.
Mini project: cross-silo federated fraud model — Flower simulation across 3 clients, Opacus DP-SGD with an ε budget, and a measured utility-delta report against a centralized baseline.
Skill checklist: - [ ] run a federated experiment with honest threat assumptions (honest-but-curious) - [ ] set and defend an ε budget with utility curves - [ ] explain TEEs vs MPC vs HE at decision-making level - [ ] identify where FL leaks (gradients, membership inference) and cite mitigations.

**19.H Treasury & liquidity AI.** Purpose: corporate and bank treasury analytics — cash-flow forecasting, FX exposure netting, and collateral optimization — an undersupplied ML lane with dull-but-deep data and measurable P&L. Established practice inside banks/treasuries; little public tooling, which is exactly the opportunity.
Resources: FRED data (cash, rates, FX) as the public backbone; AFP (Association for Financial Professionals) treasury primers on cash forecasting; hedging/FX-exposure chapters of a corporate-finance reference (e.g., Brealey/Myers/Allen *Principles of Corporate Finance*); SSRN cash-flow-forecasting literature (search; verify recency); ECB Data Portal for EUR-side series.
Mini project: daily corporate cash-position forecaster on FRED-derived synthetic treasury series — hierarchical forecasts (entity × currency × bucket) with reconciliation, plus a simple FX-hedge-ratio optimizer with cost constraints.
Skill checklist: - [ ] build hierarchical, reconciled cash forecasts - [ ] quantify forecast error in treasury terms (buffer cost vs overdraft risk) - [ ] net FX exposures and explain hedge-ratio choices - [ ] model collateral/liquidity buffers under stress scenarios.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Deflated Sharpe / CSCV | Backtest-overfitting statistics (Track A) | Separates strategy skill from multiple testing | You trade noise and call it alpha |
| Tweedie / compound Poisson | Loss distributions mixing frequency and severity (Track C) | Insurance pricing's native likelihood | You fit Gaussian to a variable that is not |
| Spectral graph concepts | Adjacency, propagation, temporal snapshots (Track D) | GNNs and fraud-ring structure | You treat relational data as tabular |
| DP budget arithmetic | ε, sensitivity, composition (Track G) | Privacy guarantees you can defend | Noise either destroys utility or guarantees nothing |
| Hierarchical reconciliation | Coherent forecasts across aggregates (Track H) | Treasury forecasts must add up | Entity totals contradict group totals |
| Uplift modeling | Treatment-effect estimation (Track B) | Next-best-action needs causal effects | You recommend to whoever converts anyway |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Point-in-time discipline on new data shapes | Graph snapshots and order-book states leak as easily as bureau fields |
| Reproducible research code | Frontier claims must be re-runnable; your repos should prove it |
| Cost/latency profiling per track | LOB inference, GNN training, FL rounds all have distinct cost profiles |
| Evaluation harness reuse | Your Phase 17/18 harnesses extend per track; do not rebuild ad hoc |
| Literature-tracking workflow | Alerts, note vaults, and quarterly agenda reviews keep the frontier tractable |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| PyTorch Geometric / DGL | GNN training for Track D |
| Flower | Federated learning simulation and deployment for Track G |
| Opacus | DP-SGD training with budget accounting for Track G |
| statsmodels / glm tooling | Tweedie GLMs for Track C |
| VectorBT / backtesting libraries | Strategy research scaffolding for Track A (use skeptically) |
| DuckDB + Elliptic/IBM-AML loaders | Graph and ledger data wrangling for Tracks D/E |
| FRED API client | Macro/treasury series for Track H |
| Zotero / Obsidian / your vault | Literature and research-agenda management across all tracks |

## 9. Resources

Cross-track anchors below; each track's inline resources in section 5 are the first reads.

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| BIS publications on SupTech/DeFi/stablecoins (bis.org) | Reports | Advanced | Tracks E/F | Central-bank-grade views of both frontiers | Recommended |
| Hamilton, *Graph Representation Learning* (free online, 2020) | Book | Advanced | Track D | The GNN textbook with the right depth | Essential |
| Dwork & Roth, *Algorithmic Foundations of DP* (2014, free) | Book | Advanced | Track G | Formal DP when you implement budgets | Recommended |
| CAS monograph: *Generalized Linear Models for Insurance Rating* (Peng Zhao) | Monograph | Advanced | Track C | The pricing-GLM practitioner standard | Essential (Track C) |
| FCA Digital Regulatory Reporting materials (fca.org.uk) | Program docs | Intermediate | Track F | Where machine-readable regulation is actually heading | Recommended |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| López de Prado, *Advances in Financial Machine Learning* (Wiley 2018) | Book | Advanced | Track A | Overfitting discipline for financial ML | Essential (Track A) |
| Harris, *Trading and Exchanges* (Oxford 2003) | Book | Advanced | Track A | Microstructure foundations behind the ML | Recommended |
| Kairouz et al. (2021), "Advances and Open Problems in Federated Learning" | Paper | Advanced | Track G | The field map in one paper | Recommended |
| Ying et al. (2019), "GNNExplainer" | Paper | Advanced | Track D | Explainability for the graphs you ship | Recommended |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Flower / Opacus / PyTorch Geometric documentation | Docs | Intermediate | Tracks D/G | The tooling you will actually run | Essential (Tracks D/G) |
| Chainalysis public research; rekt.news postmortems | Blog | Intermediate | Track E | Typologies and exploit anatomy from practitioners | Recommended |
| AFP treasury primers (afponline.org) | Guides | Intermediate | Track H | Treasury vocabulary and forecasting practice | Recommended (Track H) |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| FinRL (Liu et al. 2020) codebase | Code | Advanced | Track A | RL-for-trading reference implementation | Optional |
| CASdatasets R package (French MTPL) | Dataset | Intermediate | Track C | Public pricing data with actuarial pedigree | Essential (Track C) |
| Vendor landscape blogs (ComplyAdvantage and peers) | Blog | Beginner | Track F | Market texture only; treat claims as marketing | Optional |

## 10. Practical Exercises

1. - [ ] Write your track-selection memo: market-demand evidence (job postings scanned), capstone synergy, interest sustainability — choose 2-3 tracks and justify rejections.
2. - [ ] Build the literature workflow: paper inbox, one-page summaries, and a quarterly research-agenda draft (see `/research/README.md` and `/docs/08-research-agenda.md`).
3. - [ ] Track A: reproduce one published LOB baseline on FI-2010; write the skeptical restatement of its alpha claim.
4. - [ ] Track B: design the NBA experiment (uplift vs propensity) and show where the naive model misallocates actions.
5. - [ ] Track C: double-lift chart GLM vs ML on the French MTPL portfolio; annotate it as an actuary would.
6. - [ ] Track D: entity-resolution error budget on IBM Synthetic AML — measure how ER errors propagate into graph-model results.
7. - [ ] Track E: typology study of three public exploits; extract the detection features each implies.
8. - [ ] Track F: rule-change impact map — one week of register feeds mapped to your Phase 16 model inventory.
9. - [ ] Track G: ε-budget sensitivity table for one DP experiment (three ε values, utility curves, recommendation).
10. - [ ] Track H: stress-test your cash forecaster with a simulated rate/liquidity shock; report buffer adequacy.

## 11. Mini Projects

**M1 (A) — Execution-aware LOB model.** FI-2010 direction model with cost model and CSCV report. Deliverable: repo + honest backtest memo. Difficulty: ★★★★☆.
**M2 (B) — Suitability-constrained NBA engine.** Synthetic clients; uplift scoring; hard suitability filters; audit log. Deliverable: engine + governance note. Difficulty: ★★★☆☆.
**M3 (C) — Pure-premium bake-off.** Tweedie GLM vs constrained GBM; actuarial lift charts; rate-disruption view. Deliverable: pricing notebook + memo. Difficulty: ★★★☆☆.
**M4 (D) — Fraud-ring detector.** IBM Synthetic AML graph + ER + temporal GNN + investigator-facing explanations. Deliverable: repo + explanation gallery. Difficulty: ★★★★☆.
**M5 (E/G) — On-chain typology study or cross-silo FL study.** Choose one: exploit-typology analysis with detection features (E), or Flower+Opacus federated fraud model with ε report (G). Deliverable: study repo. Difficulty: ★★★★☆.
**M6 (F/H) — Regulatory radar or treasury forecaster.** Register-ingestion impact map (F), or hierarchical cash forecast with hedge optimizer (H). Deliverable: working prototype. Difficulty: ★★★☆☆.

## 12. Major Project Hook

Each track's mini project is designed to extend your Phase 18 flagship rather than replace it — e.g., graph ML bolted onto the fraud platform (01), PETs onto the credit decisioning system (02), RegTech radar onto the compliance platform (06). Track the chosen tracks' progress in `/PROGRESS.md`.

## 13. Case Studies & Industry Examples

- **GNN-for-AML research on Elliptic (2019-2025)**: an active, public research thread — a model of how an academic frontier matures toward tooling (and how slowly regulated adoption lags).
- **Publicly reported DeFi exploits (rekt.news archives)**: oracle manipulations and bridge compromises with postmortems — the raw material for Track E's typology discipline.
- **SupTech deployments by supervisors (BIS-published surveys)**: regulators themselves adopting ML for supervision — the clearest signal that Track F skills have an institutional market.
- **Robo-advisory platform disclosures (SEC filings of listed platforms)**: sparse but real architectural evidence of suitability, rebalancing, and governance constraints in production wealth products.

## 14. Interview Questions

**Why this specialization, and what did you build in it?** Answer with the selection logic (market demand evidence, capstone synergy), one shipped artifact, and one frontier judgment you made (what you refused to use and why). Specialists get hired on judgment, not exposure.

**How do you read a paper you might apply at work?** Claims → data and baselines → reproducibility → regulatory/operational fit; then a small reproduction before any production conversation. Name what would make you reject the technique.

**Where is [GNN/FL/DeFi-risk] actually in production in finance, and what blocks wider adoption?** Honest answers name both: real deployments (fraud graph features, entity resolution) and blockers (explainability duties, data-sharing law, evaluation standards). Hedge with "as of my last literature pass."

**How do you keep a research frontier from becoming a distraction?** Timeboxed tracking (quarterly agenda, alerts), production criteria written in advance, and a rule that frontier tools enter only through the same governance gates as everything else.

**Teach me your specialty in ten minutes.** The mastery test: one motivating problem, one key insight, one worked number, one failure mode. If you cannot do this, the track is not done.

**Which track pairs best with my background?** Map base phases to tracks (markets → A; credit/ops → B/C/F; fraud/AML → D/E; security/privacy → G; corporate finance → H) and pick where synergy compounds — then verify demand with postings before committing.

## 15. Assessment — Can You Pass the Bar?

- [ ] Track-selection memo with verifiable market-demand evidence for your chosen 2-3 tracks.
- [ ] One shipped mini project per chosen track, each re-runnable from its repo.
- [ ] One paper reproduction or critical restatement with a written verdict.
- [ ] A maintained research agenda (quarterly cadence) with at least one frontier judgment call documented.
- [ ] A ten-minute teaching artifact (talk notes or tutorial) for one specialty topic, stored under `/notes/artifacts/`.
- [ ] For each chosen track: the skill checklist fully checkable, with evidence links.

## 16. Mastery Checkpoint

A track is complete when:

1. Its mini project is shipped, documented, and linked from `/PROGRESS.md`.
2. Its skill checklist is evidenced (artifacts, not adjectives).
3. You have taught the topic once (mock talk or written tutorial) and answered follow-ups.
4. Its contribution back to your flagship (if any) is merged and documented.

Two completed tracks + one in progress is a strong target for this phase; revisit annually — the frontier moves.

## 17. Failure Modes & Gotchas

- Collecting tracks like badges: three shallow specialties signal nothing; depth does.
- Building on frontier techniques without pre-written production criteria — demo infection becomes architectural debt.
- Backtests (Track A) without multiple-testing discipline; the field's oldest trap and still the most common.
- Treating vendor marketing (Track E/F especially) as evidence; hedge everything with primary sources.
- Ignoring the regulatory frame of your specialty (suitability, privacy, AML duties) — specialists are hired precisely for that frame.
- Paper reading without reproduction: the gap between claimed and real performance is where careers are made.

## 18. Where This Goes Next

Phase 20 converts specialization into seniority: system-design mastery under pressure, build-vs-buy judgment (where your specialist knowledge of vendor markets pays directly), platform strategy, and leadership practice. Keep `/research/README.md` and `/docs/08-research-agenda.md` alive — the frontier is a permanent input to an architect's judgment, not a phase you finish.
