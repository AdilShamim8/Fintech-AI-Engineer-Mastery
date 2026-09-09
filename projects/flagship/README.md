# Flagship Projects — Index

> Level 4 in the project ladder. Seven systems that real FinTech companies pay teams to build. Each spec below follows the same skeleton: Problem & Users → Business Value → Datasets → Reference Architecture → Tech Stack → ML/AI Approach → Evaluation Plan → Security & Compliance → Deployment → Milestones → Difficulty/Resume Value/Research Potential → Stretch Goals.
> Capstone execution (choosing 1-2 and taking them to production + defense): [`../../phases/18-capstones/README.md`](../../phases/18-capstones/README.md).

| # | Flagship | Domain | Core phases | Datasets | Difficulty |
|---|---|---|---|---|---|
| 01 | [Real-Time Fraud Detection Platform](01-real-time-fraud-detection-platform.md) | Payments risk | 07 · 15 · 17 | IEEE-CIS, ULB | ★★★★★ |
| 02 | [Credit Risk Decisioning System](02-credit-risk-decisioning-system.md) | Lending | 06 · 16 · 17 | LendingClub, Home Credit, FICO HELOC | ★★★★☆ |
| 03 | [AML Transaction Monitoring Platform](03-aml-transaction-monitoring-platform.md) | Financial crime | 08 · 14 | IBM AML, Elliptic, OFAC SDN | ★★★★★ |
| 04 | [Financial Document Intelligence Platform](04-financial-document-intelligence-platform.md) | Doc AI | 11 · 12 | EDGAR, Financial PhraseBank, FinQA | ★★★★☆ |
| 05 | [Financial RAG Research Assistant](05-financial-rag-research-assistant.md) | GenAI/knowledge | 12 · 13 | EDGAR, EDGAR-CORPUS, FinanceBench, GLEIF | ★★★★☆ |
| 06 | [Agentic Compliance Operations Platform](06-agentic-compliance-operations-platform.md) | Agents + governance | 14 · 16 | synthetic ledgers, IBM AML, OFAC SDN | ★★★★★ |
| 07 | [Payment Intelligence Engine](07-payment-intelligence-engine.md) | Payments orchestration | 02 · 07 · 15 | IEEE-CIS, PaySim | ★★★★☆ |

## Selection Matrix (which flagship for which goal)

| Your goal | Build first | Why |
|---|---|---|
| Risk/fraud engineering roles | 01 or 02 | The two most-hired decisioning systems; streaming + governance depth |
| AML/compliance tech roles | 03 (with 06 second) | Regulatory-heavy; graph + workflow + human-in-loop |
| GenAI/LLM product roles | 05 (with 04 second) | Citation-forced RAG is the hiring signal of the modern stack |
| Platform/MLOps roles | 02 productionized in Phase 17 | The cleanest path from model to governed platform |
| Research-leaning trajectory | 06 + an L5 research project | Agents + evaluation frontier |

## Execution Rules

1. **Thin slice first:** ugly-but-complete end-to-end inside week 1.
2. **Finance-aware evaluation from day one** ([`../../tracks/finance-aware-evaluation.md`](../../tracks/finance-aware-evaluation.md) template goes in the repo README).
3. **Compliance by design:** audit store, reasons, and governance gates are sprint-2 work, not stretch goals.
4. **Public package standard:** README + architecture diagram + honest eval table + demo video + postmortem ([`../../notes/templates/project-postmortem.md`](../../notes/templates/project-postmortem.md)).
5. One flagship taken through Phase 18's full definition-of-done beats three abandoned at 70%.
