# 01 — The FinTech AI Engineer: Career Definition

> Deliverable A. What the role is, how it differs from its neighbors, and the ladder you are climbing. Revisit and rewrite this document yearly — the role moves fast.

---

## 1. The Role, Precisely

A **FinTech AI Engineer** designs, builds, and operates AI systems that make or materially support **financial decisions** — approving credit, blocking fraud, flagging money laundering, pricing risk, answering regulated questions — under the constraints that make finance unique: **regulatory defensibility, auditability, low latency at decision time, adversarial adaptation, and direct P&L consequence.**

The defining sentence: *where a general ML engineer optimizes a metric, a FinTech AI engineer optimizes a decision with a price tag on every error and a regulator reading the logs.*

Three archetypes dominate hiring (you will likely straddle two):

| Archetype | Core work | Typical employers |
|---|---|---|
| **Risk & Fraud AI Engineer** | Credit scoring, fraud/AML detection, decision engines, real-time scoring | Banks, card networks, lenders, payment processors, BNPL |
| **Financial AI Platform Engineer** | Feature stores, streaming pipelines, model governance, MLOps for regulated models | Banks' central ML platforms, financial infrastructure |
| **Financial LLM/Doc AI Engineer** | Document intelligence, RAG, copilots, agentic workflows | Investment banks, asset managers, insurtech, regtech, fintech product teams |

## 2. Boundary Map (vs adjacent roles)

| Role | What they optimize | Overlap with you | The difference |
|---|---|---|---|
| **AI Engineer (generic)** | LLM apps, agents, product features | Your Stage V stack | You add: financial data, regulatory constraints, decision economics |
| **Data Scientist (finance)** | Analysis, models, experimentation | Modeling in Phases 05-10 | You own the system: pipelines, serving, monitoring, governance — not just the notebook |
| **ML Engineer** | Training/serving infrastructure, scale | Stages II, V, VI | You add: financial domain semantics, model risk management, finance-aware evaluation |
| **Quant / Quant Developer** | Pricing, alpha, portfolio construction | Phase 10 + research frontier | They go deeper into pricing/alpha; you go deeper into decision systems and production AI. You are fluent enough to collaborate, rarely their replacement |
| **Financial Engineer** | Derivatives, risk mathematics | Phase 04, 10 math | Same as quant; more theory, less ML stack |
| **Data Engineer** | Pipelines, warehouse reliability | Phase 03 entirely | You specialize it: point-in-time correctness, financial data semantics, feature stores for decisioning |
| **Risk Analyst / Validator** | Model governance, validation | Phase 16 | You *engineer for* them; they challenge you. Learning their lens is a superpower |

**Positioning statement you should be able to say in an interview:** "I build AI systems for financial decisions — meaning I own the model *and* the pipeline, the evaluation *and* the economics, the deployment *and* the governance trail behind it."

## 3. The Skill Matrix (what the market tests)

| Layer | Must Know (interviews probe this) | Should Know | Advanced / Specialist |
|---|---|---|---|
| Financial domain | Banking 101, payment rails, credit mechanics, how P&L and balance sheets connect | Insurance, wealth, market microstructure | Regulatory capital, treasury, accounting-provisioning mechanics |
| Financial data | PIT correctness, bureau/market data, leakage traps | Entity resolution, BCBS 239 lineage, alt-data | bitemporal modeling, data contracts at scale |
| Classical financial ML | Calibrated classification, scorecards, cost-sensitive learning, drift | survival/two-stage LGD, uplift | deep fraud sequences, graph detection at scale |
| Time series & quant | Walk-forward discipline, vol basics, VaR | factor models, execution costs | deep hedging, market microstructure ML |
| Modern AI stack | RAG with citations, eval harnesses, agentic workflows with gates | fine-tuning decisions, doc-AI | multi-agent systems under governance, GraphRAG |
| Production | serving + monitoring + audit stores, incident response | feature platforms, DR | platform design for regulated ML at bank scale |
| Compliance | SR 11-7 shape, fair lending, GDPR/AI-Act awareness, adverse action | DORA, vendor risk | validation-readiness, supervisory engagement |

## 4. Seniority Ladder

| Level | Title band | Expectations | Evidence that gets you there |
|---|---|---|---|
| Entry bridge | FinTech AI Engineer (junior-mid) | Ships features on one domain (fraud or credit); solid ML + eager domain learning | L1-L3 projects; one domain's vocabulary fluently |
| Established | FinTech AI Engineer / Senior MLE, Finance | Owns a decisioning system end-to-end; writes validation-ready docs; handles drift/incidents | Flagship-level project with governance pack; production war stories with numbers |
| Senior | Senior FinTech AI Engineer | Designs systems (system-design/ level); mentors; speaks to risk committees; trades off build-vs-buy | 6 design docs; incident postmortems; cross-team influence |
| Lead | Staff / Principal, AI/ML | Sets platform/domain strategy; owns model governance posture; multi-team architecture | Platform decisions with written rationale; regulatory interactions |
| Architect / Head | FinTech AI/ML Architect, Head of AI | Org-level tradeoffs; vendor ecosystem; regulatory engagement; P&L ownership of AI | Track record + the writing in your Phase 20 deliverables |

## 5. Where the Jobs Are (and what they ask)

| Segment | You would build | Emphasis |
|---|---|---|
| Banks (retail/commercial) | credit decisioning, AML, doc AI, copilots | governance, validation-readiness, scale |
| Card networks & processors | real-time fraud/risk scoring, orchestration | latency, streaming, scale, precision economics |
| Neobanks / lenders | underwriting, collections, behavior scoring | speed, alternative data, fairness |
| InsurTech | pricing, claims triage, fraud | GLMs vs ML, telematics, document AI |
| Wealth/asset tech | personalization, research copilots, compliance | LLM systems, suitability constraints |
| Financial infra & regtech | entity resolution, monitoring platforms, KYC APIs | graphs, data quality, compliance vocab |
| Trading firms | execution, microstructure ML, risk | quant stack, latency culture (a different flavor) |

Interview loops converge on: (1) ML depth with finance-aware evaluation, (2) one domain's business mechanics, (3) system design under financial constraints, (4) governance/explainability instincts, (5) behavioral: judgment under ambiguity. This repository maps to all five: [`interview-prep/README.md`](../interview-prep/README.md).

## 6. The Honest Differentiators

Most candidates for "FinTech AI" roles present: generic ML portfolio + finance courses watched. You will present:

1. **A deployed, governed decisioning system** (flagship) with audit trail, monitoring, and a model-risk dossier.
2. **Finance-aware evaluation fluency** — you can explain why you rejected AUC as a headline metric in a lending context and what you used instead.
3. **Regulatory literacy** — SR 11-7-shaped documentation, fair-lending testing, AI-Act awareness, with the "verify current status" habit.
4. **A public knowledge base** — this repository, with commit history showing years of compounded judgment.
5. **Research awareness** — opinions on the frontier grounded in reproduced experiments (research/).

## 7. 90-Day Landing Plan (when you get the role)

- **Days 1-30:** map the decision flows (who decides what, with which models, logged where); read the model inventory and the last two validation reports; trace one decision end-to-end in the audit store; learn the org's three lines of defense.
- **Days 31-60:** own one production metric (alert precision, approval rate stability, drift alarms); ship one improvement with a full evaluation memo; meet risk/compliance stakeholders with specific questions from Phase 16.
- **Days 61-90:** propose one system-level improvement (feature parity, monitoring gap, reason-code quality) with a written design doc; present it the way Phase 20 taught you.

## 8. What To Read Next

- [`docs/02-knowledge-map.md`](02-knowledge-map.md) — the full knowledge universe and its priority tiers.
- [`docs/06-mastery-framework.md`](06-mastery-framework.md) — the objective bar for each ladder rung.
- [`docs/07-execution-plan.md`](07-execution-plan.md) — the schedule that gets you there.
