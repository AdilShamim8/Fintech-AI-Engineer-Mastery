# 08 — Research Agenda

> The living index of research field areas this curriculum tracks. The depth lives in [`research/README.md`](../research/README.md) with eight field-area briefs; this file states the policy and the priority list.

---

## 1. Purpose & Standards

Research in this system means **verified awareness + one honest experiment**, not paper collection. Every field-area brief maintains:

- **Maturity label:** Established knowledge / Current industry practice / Emerging practice / Research frontier.
- **Key papers and resources** (tiered, real, dated).
- **Open problems** the field actually argues about.
- **Solo-runnable experiments** — reproducible on a laptop with public data.
- **Curriculum hooks** — which phases/projects the area feeds.

Claims policy: anything you cite publicly carries a date and a "verify current status" check each quarter. Vendor-reported numbers are hypotheses, not evidence. Peer review beats press release.

## 2. The Eight Field Areas (priority order)

| # | Field area | Maturity (as of 2026) | Feeds |
|---|---|---|---|
| 1 | **LLM agents in financial workflows** | Emerging → early practice (banks publicly moving pilots to governed production) | Phase 14, Flagship 06 |
| 2 | **RAG & knowledge systems for finance** | Current practice (retrieval quality, versioning, ACLs are the battleground) | Phases 12-13, Flagship 05 |
| 3 | **Graph ML for financial crime** | Current practice in AML; frontier in temporal/ring detection | Phase 08, Flagship 03 |
| 4 | **Time-series foundation models** | Emerging practice (Chronos-2, TimesFM-3; zero-shot multivariate arriving) | Phase 09 |
| 5 | **Explainability & AI regulation** | Established duties (SR 11-7), consolidating regulation (EU AI Act phase-in) | Phases 06, 16 |
| 6 | **Privacy-enhancing technologies** | Established in theory, early practice in banks (FL pilots, DP reporting) | Phase 16, Phase 19-G |
| 7 | **Deep hedging & RL in execution** | Research frontier with niche production use | Phase 10, Phase 19-A |
| 8 | **Synthetic data for finance** | Current practice for sharing/testing; contested for training | Phase 19-G, fraud research |

Optional watchlist (tracked, not briefed): quantum computing in finance (research frontier, distant), on-chain/DeFi analytics (emerging, jurisdiction-dependent), neuro-symbolic compliance reasoning (frontier), multimodal financial AI (emerging).

## 3. Standing Research Questions (the ones worth your time)

1. Do LLM agents beat tuned rules+ML workflows in **reconciliation/KYC-refresh** tasks at equal auditability? (measurable)
2. What retrieval configuration survives **versioned regulatory corpora** without stale-answer incidents?
3. Can graph features close the **alert-precision gap** in AML at fixed analyst capacity? (Elliptic/IBM-AML benchmarks)
4. When do TS foundation models beat per-series classical models on **banking-style hierarchies** (cash demand, deposit flows)?
5. What is the **fairness-metric cost curve** for credit models — how much accuracy does each mitigation buy/lose, and which regulator asks for which?
6. How much utility does **differential privacy** cost in fraud-analytics sharing across institutions?
7. Does **meta-labeling** survive honest purged validation outside its inventor's examples?
8. What numeric-fidelity failure modes persist in the best **citation-forced** financial copilots?

## 4. Research Workflow (per area, ~3-4 weeks part-time)

1. Read the brief's 3-5 key papers → 1-page synthesis in your own words (established/emerging/frontier labeled).
2. Reproduce one benchmark result or run the listed solo experiment.
3. Write a public note: what held, what didn't, what you'd bet on.
4. File the note under `notes/concepts/`, link from the brief, update the maturity label if evidence warrants.

Cadence: one area per quarter after Stage V; areas 1-3 are also capstone-grade material.

## 5. Evidence Habits

- Prefer benchmarks you can rerun over leaderboards you read about.
- Distinguish "works on the paper's data" from "works on your data" — always try the public financial datasets in [`datasets/`](../datasets/README.md).
- Log surprises; they are either bugs, breakthroughs, or blog posts.
- When the field and the regulator disagree (e.g., novel scoring methods vs validation requirements), the regulator wins in production — research goes where governance permits.
