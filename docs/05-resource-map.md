# 05 — Resource Map & Selection System

> Deliverable E. The resource hierarchy, the selection criteria, and the cross-domain map of what to use when. Per-phase resource tables (with this exact schema) live in the phase READMEs; this file is the system + the global must-reads.

---

## 1. The Tier System

| Tier | Definition | Examples |
|---|---|---|
| **Tier 1 — Primary/Authoritative** | Regulators, central banks, standards bodies, peer-reviewed papers, official docs. Ground truth. | SR 11-7, EU AI Act text, BIS/BCBS publications, FRED/ECB data, QuantLib/Feast/Kafka docs, canonical papers (Engle 1982, Lessmann 2015) |
| **Tier 2 — Technical education** | University courses, rigorous textbooks, expert lectures. The depth layer. | Mishkin, Hull, Luenberger, López de Prado AFML, Baesens (credit/fraud), Kleppmann DDIA, MIT OCW 15.401/18.S096, FPP3 (free) |
| **Tier 3 — Practitioner** | Engineering blogs, production case studies, OSS project docs, vendor postmortems. The reality layer. | Stripe/Adyen engineering, Kaggle solution write-ups, M5 competition reports, Nixtla docs, DataTalksClub camps |
| **Tier 4 — Supplementary** | Videos, tutorials, community content. Motivation and reps, never the source of truth. | 3Blue1Brown, Khan Academy, Ray Dalio's economic machine, Investopedia |

**Rule of consumption:** for any topic, take *one* Tier 1, *one* Tier 2, *one* Tier 3. If a Tier 1 exists, it outranks popularity. If a resource can't survive the selection test below, it doesn't enter the repo.

## 2. Selection Criteria (score before admitting)

| Criterion | Ask |
|---|---|
| Authority | Who stands behind it? Regulator/university/OSS core team beats content marketer |
| Technical depth | Does it show the math, the code, or the decision — or just the vibe? |
| Recency | Is it still true? (Regulations: verify. GenAI: assume 12-18-month half-life.) |
| Practical usefulness | Will it change what you build next week? |
| Mathematical rigor | Can you re-derive its claims? |
| Industry relevance | Do practitioners cite it in interviews, docs, or production choices? |

Priorities in phase tables: **Essential** (do the phase with it) · **Recommended** (strong default) · **Optional** (situation-dependent) · **Reference** (lookup, not linear reading).

## 3. The Global Canon (read across the whole curriculum)

| Resource | Tier | When |
|---|---|---|
| SR 11-7 (Fed, 2011) | 1 | Phase 06 first pass; Phase 16 in full |
| Kleppmann, *Designing Data-Intensive Applications* | 2 | Phase 03 |
| Huyen, *Designing Machine Learning Systems* | 2 | Phase 17 |
| López de Prado, *Advances in Financial Machine Learning* | 2 | Phases 05, 09, 10 (critically — see research note on its reception) |
| Baesens et al., *Fraud Analytics* | 2 | Phase 07 |
| Siddiqi, *Intelligent Credit Scoring* | 2 | Phase 06 |
| Hull, *Options, Futures, and Other Derivatives* | 2 | Phases 04, 10 |
| Hyndman & Athanasopoulos, *FPP3* (free) | 1/2 | Phase 09 |
| Barocas, Hardt & Narayanan, *Fairness and ML* (free) | 1/2 | Phase 16 |
| Molnar, *Interpretable ML* (free) | 2 | Phases 06, 11, 16 |
| Huyen, *AI Engineering* (2025) | 2 | Phases 12-14 |
| Chip-free tier: EU AI Act + DORA + NIST AI RMF texts | 1 | Phase 16 |
| OWASP Top 10 for LLM Apps + MITRE ATLAS | 1 | Phases 14, 16 |

Books shelf with sequencing: [`books/README.md`](../books/README.md). Papers with reading order: [`papers/README.md`](../papers/README.md).

## 4. Per-Domain Shortlists (the "if you only use three" list)

| Domain | Tier 1 | Tier 2 | Tier 3 |
|---|---|---|---|
| Financial systems | FRED + BIS publications | Mishkin | Investopedia as lookup only |
| Payments | ISO 20022 + FedNow/RTP pages + UK OBIE/FDX | Glenbrook *Payments Systems in the U.S.* | Stripe/Adyen docs & blogs |
| Data eng | Feast/dbt/Kafka docs + BCBS 239 | DDIA | DataTalksClub DE Zoomcamp |
| Credit | Lessmann 2015 + CFPB Circular 2022-03 + IFRS 9 materials | Siddiqi + Baesens *Credit Risk Analytics* | optbinning/skorecard docs + Kaggle Home Credit write-ups |
| Fraud | ULB/IEEE-CIS + Dal Pozzolo 2015 + UK PSR PS25/5 | *Fraud Analytics* (Baesens) | Stripe Radar engineering content |
| AML | FFIEC BSA/AML manual + FATF + OFAC SDN data + Weber 2019 | Hamilton *Graph Representation Learning* (free) | Chainalysis public research |
| Time series | Nixtla/sktime docs + TimesFM/Chronos papers | FPP3 + Tsay | M5 write-ups |
| Quant | QuantLib docs | Hull + Luenberger | QuantConnect Bootcamp + vectorbt |
| NLP/Docs | SEC EDGAR APIs + L&M lexicon + FinanceBench | Jurafsky & Martin (free) + HF course | FinBERT model cards |
| GenAI/RAG/Agents | Vendor API docs + RAG survey (Gao 2023) + OWASP LLM Top 10 | *AI Engineering* (Huyen) | Langfuse/Phoenix docs, Anthropic agents essay |
| Real-time | Kafka + Flink docs | *Streaming Systems* | Confluent engineering blog |
| Governance | SR 11-7 + EU AI Act + DORA + NIST AI RMF | *Fairness and ML* | IAPP practical guides |

## 5. Anti-Patterns in Resource Selection

1. **Popularity substituting for authority** — a viral YouTube video is not a Tier 1.
2. **Redundancy hoarding** — three books on scorecards is procrastination; one + build beats three.
3. **Recency bias against standards** — SR 11-7 is from 2011 and still governs; old ≠ wrong in regulation.
4. **Freshness bias against fundamentals** — Markowitz 1952 still structures portfolios; classic ≠ stale.
5. **Vendor claims as evidence** — treat "90% reduction" marketing numbers as hypotheses to test, not facts.
6. **Tutorial loops** — if you have watched two courses on the same topic without building, the third watch is avoidance.

## 6. Maintenance

Quarterly (Phase 19 cadence): re-verify regulatory anchors, check one emerging-practice claim per frontier area (see [`docs/08-research-agenda.md`](08-research-agenda.md)), demote/promote resources based on your logged experience (CONTRIBUTING §5). The resource system should age like a curated cellar, not a landfill.
