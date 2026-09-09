# Phase 18 — Capstone Systems

> **Stage VI — Production & Leadership** · **Duration: 8-12 weeks** · **Mastery target: Portfolio → Defense**
> **Position in path:** `17-production-fintech-ai` ← **this phase** → `19-specialist-tracks`

## 1. Objective

This phase is not new material — it is the execution guide for proving everything before it, once, publicly, end to end. You will choose one (maximum two) flagship projects, write a compliance-aware PRD, plan milestones on a weekly cadence, and drive the system through data, modeling, serving, monitoring, security, and documentation to a public-portfolio package — finishing with a mock defense in front of an imagined risk committee. Two worked example plans (real-time fraud detection platform; financial RAG research assistant) show exactly what "done" means, week by week.

## 2. Why It Matters in Finance

Hiring managers and clients in FinTech do not evaluate notebooks; they evaluate systems and the judgment visible around them. A capstone that shows a governed decision path — data contracts, evaluation, audit store, monitoring, reasons — is worth more than ten competition scores, because it demonstrates the scarce skill: finishing regulated software. The mock defense is practice for the real ones: every significant financial AI system eventually faces a committee that can say no.

- Portfolio signal in FinTech is "can finish and defend," not "can try": public repos with architecture diagrams, ADRs, and honest limitations out-perform private cleverness.
- Compliance-by-design in the PRD is what separates a fintech project from a generic AI demo at exactly the points interviewers probe.
- Weekly milestone cadence with acceptance criteria is how 8-12 week personal projects actually ship instead of stalling at week five.
- The defense (recorded) doubles as interview rehearsal: the questions a risk committee asks are the questions a staff interviewer asks.
- One excellent flagship beats two mediocre ones; the second project is optional and only starts after the first is defensible.

## 3. Prerequisites

- [ ] Phases 01-17 — the full stack; specifically Phase 06/07 models, Phase 16 governance, Phase 17 production spine
- [ ] A chosen flagship spec from `/projects/flagship/README.md`
- [ ] `/system-design/README.md` read once — its templates are your architecture starting points
- [ ] `/tracks/README.md` scanned — finance-aware evaluation metrics come from there
- [ ] Realistic calendar: 8-12 weeks at 8-15 focused hours/week

## 4. Learning Outcomes

- I can choose a capstone with a written decision matrix (skill coverage, portfolio value, market demand, feasibility) instead of enthusiasm alone.
- I can write a financial-AI PRD with problem, users, constraints, and compliance-by-design requirements.
- I can define data contracts and ingestion boundaries before writing pipeline code.
- I can adapt a system-design template into a concrete architecture and record deviations as ADRs.
- I can plan weekly milestones with acceptance criteria and hold the plan under scope pressure.
- I can design a finance-aware evaluation strategy (decision metrics, guardrails, slice analysis) before building.
- I can produce the public-portfolio package: repo standards, diagrams, demo video, and a technical blog post.
- I can defend the system in a recorded mock risk-committee session and convert the questions into backlog items.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 18.1 | Choosing your capstone | decision matrix across four axes | choice memo |
| 18.2 | PRD-writing for financial AI | problem, users, constraints, compliance-by-design | PRD v1 |
| 18.3 | Data contracts & ingestion | schemas, ownership, point-in-time rules | contract doc + loader |
| 18.4 | Architecture from a template | adapt /system-design/; record deviations | diagram + ADRs |
| 18.5 | Milestone planning | weekly cadence with acceptance criteria | milestone plan |
| 18.6 | Evaluation strategy | finance-aware metrics from /tracks/ | eval plan + harness |
| 18.7 | Security & compliance by design | Phase 16 duties embedded in the build | control inventory |
| 18.8 | Documentation standards | README, ADRs, model docs | documentation set |
| 18.9 | The public-portfolio package | repo standards, diagrams, video, blog | published package |
| 18.10 | The mock defense | risk-committee rehearsal, recorded | recorded session |

**18.1 Choosing your capstone.** Score each flagship 1-5 on skill coverage (how much of Phases 01-17 it exercises), portfolio value (market demand for that domain), market demand (job postings, client problems), and feasibility (data availability, compute, your calendar). Weight the axes honestly; the top-scoring project — not the most exciting one — gets the weeks. Write the matrix down; you will reuse it when mentoring others.

**18.2 PRD-writing for financial AI.** A capstone PRD states the problem in decision terms (what choice the system improves, at what volume, with what error costs), the users (and the human reviewers), explicit constraints (latency, fairness, privacy, audit), and compliance-by-design requirements lifted from Phase 16 (reason codes, retention, oversight paths). One page per constraint beats ten pages of aspiration.

**18.3 Data contracts & ingestion.** Fix schemas, semantics, and point-in-time rules before building: every field typed, every timestamp defined (event time vs load time), every label's availability delay documented, every PII field marked and minimized. Ingestion code validates the contract and quarantines violations — the Phase 17 data gate, from day one of the capstone.

**18.4 Architecture from a template.** Start from the closest `/system-design/README.md` problem (fraud platform, decisioning service, RAG service), adapt to your data and constraints, and record every deviation as an ADR with the options you rejected. Templates compress the blank-page problem; ADRs convert your judgment into portfolio-visible evidence.

**18.5 Milestone planning.** Weekly cadence, each milestone with acceptance criteria that are checkable (a metric hit, a demo recorded, a test passing) and a visible artifact. Plan the unglamorous middle weeks (hardening, evaluation, docs) explicitly — they are what get squeezed otherwise. If a milestone slips twice, cut scope, not quality gates.

**18.6 Evaluation strategy.** Choose metrics from `/tracks/`: decision-quality metrics (precision at operating point, calibration, expected cost) over vanity metrics; guardrails (approval-rate stability, alert-volume caps, fairness screens) from Phase 16; slice analysis across segments and time. Freeze a test set before iterating, and keep an untouched final holdout for the defense.

**18.7 Security & compliance by design.** Threat model in week one (Phase 16 kit), not week nine; the audit store, reason pipeline, and access controls are first-class milestones. A capstone that appends compliance at the end will not survive its own mock defense — the committee's first questions are always "who reviews this?" and "where are the logs?"

**18.8 Documentation standards.** README that lets a stranger run the system in 15 minutes; ADRs for every consequential choice; model docs in the Phase 16 dossier format; an honest limitations section. Documentation is not the tax you pay on the project — it is the portfolio.

**18.9 The public-portfolio package.** Repo standards (clean history, CI badge, license, secrets-free), architecture diagram (C4-lite), a 3-5 minute demo video scripted around the decision path, and one blog post telling the story of one hard problem you solved. Publish where recruiters actually look; link from your CV and `/notes/`.

**18.10 The mock defense.** Present to an imagined risk committee — model risk officer, compliance, business owner — for 30 minutes: problem, architecture, evaluation, controls, limitations. Record it, watch it, and convert every question you fumbled into backlog items. Repeat once before calling the capstone done.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Decision-cost evaluation | Expected cost = errors × cost matrix at the operating point | Capstones must show business-relevant metrics | Your demo optimizes AUC while the story needs money |
| Calibration & Brier scoring | Probabilities matching frequencies | Pricing/threshold claims must be calibrated | "High risk" claims collapse under one reliability curve |
| Guardrail statistics | Rate caps, PSI screens, fairness ratios as limits | Proves stability of the decision path | Demo works, committee finds the instability |
| Slice analysis | Metric breakdowns by segment and time | Finds the segment where your model quietly fails | Your best-case metrics hide your worst segment |
| Milestone risk estimation | Buffer math on schedule estimates | Keeps 8-12 weeks honest | Week-6 crunch deletes the documentation |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Repo hygiene & CI from day one | Tests, lint, and badges are portfolio evidence; retrofitting week 10 fails |
| Secrets & PII discipline | No keys in history; datasets minimized and licensed; the repo must be safe to be public |
| One-command run | Demo environments decay; a runbook + compose file keeps the video reproducible |
| Audit store & reason pipeline | The two artifacts interviewers ask to see first in fintech demos |
| Diagrams-as-code | Mermaid/diagrams that live in the repo stay truthful; screenshots rot |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| GitHub (Projects, Actions, Pages) | Milestone tracking, CI, and hosting the public package |
| Docker Compose | One-command local environment for demos and reviewers |
| MLflow | Registry and evidence for the modeling milestone |
| Evidently / NannyML | Monitoring milestone artifacts with real reports |
| Mermaid / draw.io | Architecture diagrams kept in-repo and versioned |
| OBS / camera | Demo video and the recorded mock defense |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| `/projects/flagship/README.md` + flagship specs (this repo) | Specs | All | Project choice | The menu and the acceptance bars for each flagship | Essential |
| `/system-design/README.md` + problem set (this repo) | Guide | Advanced | Architecture | Starting templates for every capstone shape | Essential |
| `/tracks/README.md` (this repo) | Guide | Intermediate | Evaluation | Finance-aware metric definitions per domain | Essential |
| Phase 16 governance kit (your artifacts) | Kit | All | Compliance | Dossier, threat model, audit-store patterns to embed | Essential |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Chip Huyen, *Designing Machine Learning Systems* (O'Reilly 2022) | Book | Intermediate | Architecture | Reference for platform decisions in your ADRs | Recommended |
| Michael Nygard, "Documenting Architecture Decisions" (2011, article) | Article | All | ADRs | The original ADR format; short and canonical | Essential |
| Kleppmann, *Designing Data-Intensive Applications* (2017) | Book | Advanced | Data systems | Justifying storage/streaming choices in writing | Reference |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Google SRE books (sre.google, free) | Book | Intermediate | Reliability | SLO/postmortem formats for your ops artifacts | Recommended |
| High-quality public ML project repos (choose 2-3 exemplars) | Repos | Intermediate | Portfolio craft | Study README/CI/docs patterns worth imitating | Recommended |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Technical-writing guides for engineers (e.g., Google's technical writing courses) | Course | Beginner | Writing | Tightens the blog post and PRD prose | Optional |
| OpenSSF Scorecard / repo-security checklists (github.com/ossf) | Checklist | Intermediate | Repo security | Supply-chain hygiene for a public repo | Optional |

## 10. Practical Exercises

1. - [ ] Build the capstone decision matrix for all 7 flagships; score, weight, decide, and record the choice memo with the top rejected alternative.
2. - [ ] Write the one-page PRD: decision problem, users, constraints, compliance-by-design requirements; have one practitioner review it cold.
3. - [ ] Author the data contract for your primary dataset (schema, semantics, point-in-time rules, PII flags, label delay) and the validating loader that enforces it.
4. - [ ] Adapt the closest `/system-design/` template; produce the diagram plus three ADRs for deviations you actually made.
5. - [ ] Draft the milestone plan (weekly, acceptance criteria, artifacts) and stress-test it: cut 20% of scope and re-plan deliberately.
6. - [ ] Freeze evaluation: test set, metrics, guardrails, slices; commit the eval harness before the first model iteration.
7. - [ ] Write the threat model and control inventory (Phase 16 kit) as week-one artifacts.
8. - [ ] Record the 3-5 minute demo video scripted around one decision traveling the full path.
9. - [ ] Draft and publish the technical blog post on one hard problem from the build.
10. - [ ] Run the mock defense: 30 minutes to an imagined risk committee, recorded; convert every fumbled question into backlog items.

## 11. Mini Projects

The capstone's "mini projects" are the two worked example plans below — a full milestone schedule with acceptance criteria each, plus the shared definition of done.

### Plan A — Real-Time Fraud Detection Platform (10 weeks)

Stream-replayed IEEE-CIS/ULB transactions → Kafka → streaming features (Redis) → FastAPI scoring (ONNX) → decision + audit store → dashboard; rules-only fallback; monitoring stack.

| Week | Milestone | Acceptance criteria |
|---|---|---|
| 1 | PRD + architecture + data contract | PRD reviewed; diagram in repo; loader enforces contract |
| 2 | Ingestion + replay stream | 1M tx replayed into Kafka; per-account ordering verified |
| 3 | Streaming features | EWMA/window features match offline recompute exactly |
| 4 | Model baseline + calibration | Fraud model beats baselines; calibration curves included |
| 5 | Scoring service | p99 <100ms under load test; bottleneck documented |
| 6 | Decisioning + audit store | Idempotent decisions; any decision reconstructable |
| 7 | Fallback + failover drill | ML kill → rules-only within SLA; recorded |
| 8 | Monitoring + alerting | Lag, freshness, drift dashboards firing on induced faults |
| 9 | Hardening + security | Threat model closed; injection/PNI checks pass; retention live |
| 10 | Docs + demo + defense | Video, blog, README; mock defense recorded |

### Plan B — Financial RAG Research Assistant (8 weeks)

SEC EDGAR + FRED + internal-style docs → chunking/metadata → hybrid retrieval (BM25 + embeddings) → generation with citations → eval suite → web UI; guardrails on unverified output.

| Week | Milestone | Acceptance criteria |
|---|---|---|
| 1 | PRD + corpus scope + contracts | Source list frozen; licensing/PII reviewed |
| 2 | Ingestion + chunking + metadata | Full corpus indexed; provenance on every chunk |
| 3 | Retrieval v1 (hybrid) | Recall@k beats each single method on gold queries |
| 4 | Generation + citations | Answers cite verified passages; unverified claims flagged |
| 5 | Eval suite (FinanceBench/FinQA-style) | Faithfulness + citation precision reported with CIs |
| 6 | Guardrails + PII + injection tests | Red-team script passes; exfiltration blocked |
| 7 | UI + latency + caching | p95 answer time within budget; cached-path evidence |
| 8 | Docs + demo + defense | Video, blog, README; mock defense recorded |

### Definition of Done (both plans)

- [ ] Data ingestion with enforcing data contract
- [ ] Processing layer tested (unit + contract tests)
- [ ] Features with training/serving parity evidence
- [ ] Training pipeline reproducible from cold checkout
- [ ] Evaluation with decision metrics, guardrails, slices, CIs
- [ ] Serving with stated SLOs and load-test evidence
- [ ] API documented and versioned
- [ ] UI or dashboard demonstrating the decision path
- [ ] Monitoring with firing-on-demand alerts
- [ ] Security: threat model closed, secrets clean, PII minimized
- [ ] Explainability: reasons/reasons+counterfactuals where the domain requires
- [ ] Documentation: README, ADRs, model dossier, limitations
- [ ] Testing: CI green, golden-set regression in place
- [ ] Deployment: one-command run; demo video recorded

## 12. Major Project Hook

This phase exists to execute the seven flagships (`/projects/flagship/`; index at `/projects/flagship/README.md`):

| # | Flagship | Spec | Best-fit phases |
|---|---|---|---|
| 01 | Real-Time Fraud Detection Platform | `/projects/flagship/01-real-time-fraud-detection-platform.md` | 07, 15, 17 |
| 02 | Credit Risk Decisioning System | `/projects/flagship/02-credit-risk-decisioning-system.md` | 06, 16, 17 |
| 03 | AML Transaction Monitoring Platform | `/projects/flagship/03-aml-transaction-monitoring-platform.md` | 08, 14, 17 |
| 04 | Financial Document Intelligence Platform | `/projects/flagship/04-financial-document-intelligence-platform.md` | 11, 12 |
| 05 | Financial RAG Research Assistant | `/projects/flagship/05-financial-rag-research-assistant.md` | 12, 13, 17 |
| 06 | Agentic Compliance Operations Platform | `/projects/flagship/06-agentic-compliance-operations-platform.md` | 14, 16 |
| 07 | Payment Intelligence Engine | `/projects/flagship/07-payment-intelligence-engine.md` | 02, 07, 15 |

(Phase 09/10 forecasting work flows into portfolio-grade projects via `/projects/README.md` and the Stage IV gate — it pairs naturally with a forecasting elective in Phase 19-H.)

## 13. Case Studies & Industry Examples

- **Zillow Offers (2021)**: the canonical capstone-adjacent lesson, publicly reported — an algorithmic system scaled past its assumptions; your mock defense should include a "what would have caught this?" slide (see `/case-studies/README.md`).
- **Public fintech engineering blogs (Adyen, Capital One, Nubank)**: published system designs and postmortems model the documentation standard your package imitates — selective, but structural.
- **Open-source portfolio norms (OSS project templates, OpenSSF checks)**: the repo-hygiene bar your public package should clear; reviewers do judge hygiene before architecture.
- **Your own mock defense recording**: the most honest case study you will produce — watch it and list what a committee would push on.

## 14. Interview Questions

**Walk me through your capstone in five minutes.** Problem in decision terms → architecture in one diagram → the two hardest engineering choices (with rejected alternatives) → evaluation and guardrails → controls and audit → what you would do next. Practice this; it is your interview opener everywhere.

**How did you choose evaluation metrics?** Decision-cost metrics at the operating point, calibration for probability claims, guardrails for stability and fairness, slice analysis for the segments that matter — chosen before building and frozen; the final holdout stayed untouched until the defense.

**What did you cut, and why?** Name the scope cut explicitly (milestone plan, week 5), what protected it (quality gates), and the risk you accepted. The ability to cut scope without cutting controls is the senior signal in the whole project.

**How does your system comply with [Reg B / EU AI Act / GDPR]?** Answer from artifacts: reason pipeline for adverse action; high-risk duties mapped to logging/oversight/risk management; human-review path and retention rules. If you cannot point to a file, you do not have the answer.

**Defend one architecture decision against the obvious alternative.** Take a real ADR: state context, options, the tradeoff that decided it, and what evidence would change your mind. ADRs make this question easy — that is their purpose.

**What failed during the build?** Pick a real incident (the induced pipeline break, the parity bug): detection, diagnosis, fix, and the systemic change after. Failure stories with systemic fixes are stronger than success stories.

**How would your system behave at 100x volume?** Load-test evidence, the bottleneck you found and fixed, the shed/degrade design, and the cost-per-decision model at scale. Speculation without a load test is visible immediately.

**Where are the limits of your project?** The honest limitations list — data realism, single-jurisdiction rules, simulated labels — plus the follow-up plan. Committees trust engineers who name limits first.

## 15. Assessment — Can You Pass the Bar?

- [ ] Public repo meets the DoD checklist above, with CI green.
- [ ] One-command run works from a cold clone on a reviewer's machine.
- [ ] Decision path is demonstrable end to end: input → features → model → policy → reasons → audit → monitor.
- [ ] Evaluation report shows decision metrics, guardrails, and slices — with the final holdout untouched.
- [ ] Governance artifacts attached: dossier, threat model, retention policy.
- [ ] Demo video (3-5 min) and blog post published.
- [ ] Mock defense recorded; every fumbled question has a written follow-up answer.

## 16. Mastery Checkpoint

The capstone is done when:

1. The DoD checklist is fully checked and evidenced in the repo.
2. The mock defense is recorded twice (improvement visible between takes) and stored under `/notes/artifacts/`.
3. The public package (repo + video + post) is live and linked from your CV and `/PROGRESS.md`.
4. You can re-present the five-minute walkthrough cold, from memory.

A second flagship is optional; start it only after the first fully clears this bar.

## 17. Failure Modes & Gotchas

- Starting the second project before the first is defensible — portfolio spread is not depth.
- Scope as a vector: adding modalities, markets, and features instead of hardening one decision path.
- Notebook-first architecture: building before the data contract exists, then discovering point-in-time leaks in week 8.
- Compliance as a final-week coat of paint; committees detect this within two questions.
- The untouched holdout that quietly gets used for iteration — evaluation integrity is a one-way door.
- Demo video showing features instead of the decision path; fintech reviewers care about the latter.
- Documentation written for yourself; test it on someone who has never seen the repo.

## 18. Where This Goes Next

Phase 19 offers specialist tracks to deepen the domain your capstone opened — quant, wealth, insurance, graph, digital assets, RegTech, PETs, or treasury — each with its own project menu. Phase 20 then converts the finished system and its defense into senior-level currency: system design under pressure, strategy writing, and leadership practice.
