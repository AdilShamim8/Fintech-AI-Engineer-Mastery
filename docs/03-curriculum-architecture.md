# 03 — Curriculum Architecture

> Deliverable C. The design decisions behind the 6 stages and 21 phases — and why this beats the alternatives.

---

## 1. First Principles

1. **Bridge, don't restart.** The learner's AI stack is an asset; the curriculum grafts finance onto it. Generic ML content is banned unless finance changes it (e.g., calibration under undersampling).
2. **Domain before models.** Modeling banked on misunderstood banking produces confident nonsense — Stage I buys the vocabulary and mechanics first.
3. **The money-critical three get full phases.** Credit (06), fraud (07), AML (08) are where finance ML is hired, regulated, and litigated. They are not chapters; they are phases.
4. **Statistics of deception early (05).** Selection bias, leakage, multiple testing, backtest overfitting — the canonical failure modes of financial ML — are taught *before* the first credit model, then enforced everywhere.
5. **Compliance is architecture (16), not paperwork.** Model risk management, fairness, and auditability are engineered from Phase 06 onward and formalized in 16.
6. **Modern AI stack under financial constraints (11-15).** NLP/GenAI/RAG/agents/streaming are taught as *deployment subjects* — latency budgets, citations, gates, audit trails — not as demo subjects.
7. **Everything terminates in artifacts.** 25-project ladder, 7 flagships, 10 case studies, 8 research briefs. Reading is substrate; building is the curriculum.

## 2. The Architecture

```text
Stage I   Domain Bridge (00-02)           "speak finance before modeling it"
Stage II  Data & Quant Core (03-05)       "the substrate: data discipline + math honesty"
Stage III Core Financial ML (06-09)       "credit, fraud, AML, forecasting — the employable core"
Stage IV  Markets & Quant (10)            "credible collaboration with quant teams"
Stage V   Advanced Financial AI (11-15)   "NLP, GenAI, RAG, agents, real-time — production constraints on"
Stage VI  Production & Leadership (16-20) "compliance, production, capstones, electives, architecture"
```

Full phase table + dependency graph: [`ROADMAP.md`](../ROADMAP.md). Per-phase lesson lists: [`docs/04-detailed-roadmap.md`](04-detailed-roadmap.md) and each `phases/*/README.md`.

## 3. Redesign Decisions (vs the naive 21-phase sketch)

| Naive sketch | This design | Why |
|---|---|---|
| Banking and payments as separate phases | Merged (02) | One operating reality; a loan is sold, settled, serviced across both |
| Math and stats late or generic | Early, finance-only (04-05) | Every later phase leans on them; generic math is the learner's existing asset |
| Fraud/AML under one "risk" umbrella | Full phases each (07, 08) | Different regulation (consumer protection vs BSA/AML), different data, different economics |
| NLP → GenAI → RAG → agents as four equal silos | Sequenced pipeline (11→12→13→14) with shared eval discipline | Each builds the prior's artifacts; avoids four half-integrations |
| Real-time as an afterthought in "production" | Own phase (15) | Auth-time decisioning is an architecture discipline (streaming, EOS, p99, fallback) |
| Capstone at the end only | Capstones (18) fed by flagship hooks in every phase (12's project feeds 05, etc.) | Compounding portfolio instead of one final sprint |
| "Advanced topics" = grab bag | Elective tracks (19) with a research agenda (08) and a research/ directory | Specialist depth with traceable frontier mapping |
| Senior level = more reading | Phase 20 = timed design docs, governance fluency, public writing, mentoring | Seniority is demonstrated judgment, not consumed content |

## 4. Learning-Path Overlays

The phases are the trunk; these paths are pruning guides for different goals:

| Path | Phases in order | For |
|---|---|---|
| **Risk & Fraud Engineer** | 00-06, 07, 08, 15, 16, 17, 18 (flagship 01/02/03) | Payments, lending, banks |
| **Financial LLM/Doc AI Engineer** | 00-02, 03, 05, 11-14, 16, 17, 18 (flagship 04/05/06) | GenAI teams, regtech, wealth |
| **Quant-adjacent ML Engineer** | 00-05, 09, 10, 15, 17, 19-A | Trading firms, asset managers |
| **Platform / MLOps in banking** | 00-03, 06, 15, 16, 17, 18 (flagship 02 productionized), 20 | Central ML platforms |

## 5. Progression Philosophy (mastery over completion)

- The 9-level mastery ladder (Awareness → Leadership) with objective gates: [`docs/06-mastery-framework.md`](06-mastery-framework.md).
- Stage gates require **evidence packs**, not checkmark completion: recorded explanations, deployed artifacts, honest eval tables.
- Soft prerequisites are marked in ROADMAP; hard prerequisites are enforced by the gates.
- Revision loops (spaced repetition + stage-gate oral exams) exist because financial knowledge decays without use — and because interviews test recall under pressure.

## 6. Maintenance Model

This curriculum is a living system:

- Regulations carry "as of / verify current status" flags by design; the quarterly review (Phase 16 + research/) rechecks them.
- Emerging practice (e.g., TS foundation models, agentic patterns) is labeled and re-assessed via [`docs/08-research-agenda.md`](08-research-agenda.md) and [`research/`](../research/README.md).
- Resource priorities demote on experience (CONTRIBUTING §5) — the repository learns from your usage.
