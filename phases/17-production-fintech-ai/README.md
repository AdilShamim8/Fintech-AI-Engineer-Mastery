# Phase 17 — Production FinTech AI Engineering

> **Stage VI — Production & Leadership** · **Duration: 4-5 weeks** · **Mastery target: Production → Leadership**
> **Position in path:** `16-security-compliance-responsible-ai` ← **this phase** → `18-capstones`

## 1. Objective

A model that scores 0.79 AUC in a notebook is worth nothing; a governed, monitored, rollback-capable service is a product. This phase assembles the full production stack for regulated financial ML: reference architectures per domain, ML platforms and their build-vs-buy seams, ML-aware CI/CD with quality gates and shadow deploys, monitoring for drift and delayed labels, decision audit stores, incident management, and cost engineering. You will finish by running the complete pipeline — data validation to drift alerting — on your own Phase 06 scorecard, and by load-testing it until you have found and fixed the bottleneck.

## 2. Why It Matters in Finance

Financial ML fails in production quietly: a feature pipeline stalls, a population drifts, labels arrive 18 months late, and the model keeps answering confidently while the book quietly deteriorates. The institutions that do this well treat ML like SRE treats software — SLOs, error budgets, runbooks, postmortems — while adding regulation-specific machinery: governance-linked approvals, per-decision explainability, and audit stores with retention rules. Zillow's 2021 shutdown of its home-buying arm is the canonical public lesson of what happens when algorithmic confidence meets production reality at scale.

- Silent failures dominate: dashboards stay green while upstream data breaks; monitoring must test the pipeline, not just the model.
- Delayed labels change monitoring design: you steer by stability proxies for months before true performance is knowable.
- Every decision must be reconstructable: inputs, model version, policy version, output, human overrides — with retention that satisfies regulators.
- Deployment is governed: champion/challenger, shadow periods, and approvals are not bureaucracy; they are how a lender changes the who-gets-money function safely.
- Cost per decision is a competitive number in thin-margin fintech: right-sizing, caching, and distillation are product features.

## 3. Prerequisites

- [ ] Phase 06 — a trained credit model you now productionize
- [ ] Phase 15 — streaming and serving fundamentals (p99 thinking, online features)
- [ ] Phase 16 — governance gates and audit requirements you now automate
- [ ] Docker, FastAPI, GitHub Actions, and one orchestration tool (Airflow or Prefect) at working level
- [ ] SQL and warehouse/dbt basics

## 4. Learning Outcomes

- I can choose and adapt a reference architecture per domain (fraud scoring service, batch credit decisioning, document pipeline, RAG, agent service) with justified deviations.
- I can specify ML platform components (feature store, registry, orchestrator, serving, monitoring) and make a defensible build-vs-buy call for each.
- I can implement ML-aware CI/CD: data validation gates, model quality gates, and canary/shadow deployments with automated champion/challenger.
- I can test ML systems like software plus statistics: feature unit tests, golden-dataset tests, eval-delta significance, load tests, and data-contract tests.
- I can monitor production models — PSI/KS drift, concept drift, delayed-label proxies, business KPIs — and wire alerts with thresholds I can justify.
- I can run ML on-call: runbooks for silent failures and pipeline breaks, and postmortems that produce fixes rather than blame.
- I can design a decision audit store with retention rules and per-decision explainability at serving time.
- I can design disaster recovery for decisioning services (RTO/RPO, multi-region) and set SLOs with error budgets for AI services.
- I can optimize cost per decision without degrading compliance properties.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 17.1 | Reference architectures per domain | five canonical shapes | architecture selection memo |
| 17.2 | ML platform components & build-vs-buy | feature store, registry, orchestrator, serving, monitoring | component ADRs |
| 17.3 | CI/CD for ML | data + model quality gates, canary/shadow | pipeline with gates |
| 17.4 | Testing ML systems | feature units, golden datasets, delta stats, contracts | test suite |
| 17.5 | Reproducibility engineering | pins, containers, seeds, snapshots | one-command rebuild |
| 17.6 | Monitoring: drift & delayed labels | PSI/KS, concept drift, proxies, KPIs | monitoring stack |
| 17.7 | Retraining policy & approvals | governance-linked triggers | retraining policy doc |
| 17.8 | ML incident management | silent failures, on-call, runbooks | runbook set + drill |
| 17.9 | Rollback & shadow modes | last known good model | rollback mechanism |
| 17.10 | Per-decision explainability & audit store | reasons at serving; retention | audit store implementation |
| 17.11 | Experimentation at fintechs | guardrail metrics: loss, complaints | experiment design doc |
| 17.12 | Cost/performance optimization | right-sizing, distillation, caching, reuse | cost model + win |
| 17.13 | SLOs, change control & DR | error budgets, CCB, RTO/RPO, multi-region | SLO sheet + DR plan |
| 17.14 | Operations culture | postmortems, docs-as-code | postmortem on a real fault |

**17.1 Reference architectures per domain.** Fraud scoring service (stream features + sync API + fallback — Phase 15's shape), batch credit decisioning (orchestrated scoring runs + policy engine + reasons), document processing (ingest → OCR/parse → extract → validate → human QA), RAG service (indexers + retrieval + generation + citation checks), agent service (orchestrator + gated tools + trace store — Phase 14's shape). Learn the shapes, then deviate deliberately and write down why.

**17.2 ML platform components & build-vs-buy.** Feature store (build glue vs buy parity), model registry (MLflow is a default), orchestrator (Airflow/Prefect/dbt), serving (homegrown FastAPI vs K8s + Triton), monitoring (Evidently/NannyML vs homegrown). The decision axes: team size, regulatory evidence needs, and the cost of migrating later. A good platform is boring, over-provisioned in auditability, and adopted by its internal customers — platform-as-product thinking starts here.

**17.3 CI/CD for ML.** Beyond code tests: data validation gates (schema, ranges, class balance) as merge-blocking checks; model quality gates (minimum metric deltas on golden sets, calibration checks); canary and shadow deploys (new model scores silently, agreement analyzed); automated champion/challenger with pre-registered decision rules. A model promotion should be a pipeline event with artifacts, not a Slack message from a scientist's laptop.

**17.4 Testing ML systems.** Unit tests for feature logic (including point-in-time correctness — Phase 06's leak lesson, enforced), golden-dataset regression tests, statistical tests on eval deltas (bootstrap CIs, not eyeballing), load tests before promotion, and data-contract tests at producer boundaries. Financial ML gets one extra suite: policy/simulation tests (what would this model have decided on last quarter's book?).

**17.5 Reproducibility engineering.** Pin everything: data snapshots, package versions, container images, seeds, and hardware notes where numerics matter. The one-command rebuild is the deliverable; validators (Phase 16) consume it directly. If your model cannot be re-run cold by a stranger, it is not production-ready for finance.

**17.6 Monitoring: drift & delayed labels.** PSI/KS on features and scores for population shift; concept-drift signals from error proxies and stability; delayed-label performance via proxies (early payment behavior, complaint rates, dispute rates) and cohort vintage curves. Business KPIs — approval rate, loss rate, alert precision — belong on the same dashboard: ML monitors that never touch the P&L get ignored.

**17.7 Retraining policy & approvals.** Pre-agreed triggers (PSI threshold, performance proxy floor, calendar cadence) with a governance-linked approval path: retraining is routine, deployment is gated. Write the policy before the incident — ad-hoc retraining under loss pressure is how silent overfitting to the recent past enters a book.

**17.8 ML incident management.** Silent failure taxonomy: upstream schema change, stale feature store, serving-model/registry mismatch, traffic shift, partial fallback. On-call for ML needs runbooks with concrete first moves (compare distributions, diff feature freshness, replay golden set), escalation paths, and drills. If your runbook's first step is "look at the dashboard," it is not a runbook.

**17.9 Rollback & shadow modes.** Keep the last known good model warm and instantly promotable; every deploy has a written rollback trigger (metric, threshold, duration). Shadow mode is the standard first stage for risky changes: new model scores live traffic invisibly; agreement analysis with the champion decides promotion. Design rollback before deploy — during an incident is too late.

**17.10 Per-decision explainability & audit stores.** Reasons computed at serving time and stored with the decision: input snapshot, model version, policy version, output, reasons, overrides — append-only, retention-managed (regulators and privacy law both care; Phase 16 set the rules). This store is the substrate for adverse action, dispute defense, and postmortems alike.

**17.11 Experimentation at fintechs.** Decisioning experiments are not classic A/Bs: policies move money and people's access to it. Use guardrail metrics (loss rate, complaint rate, approval-rate caps by segment), holdout cohorts where lawful, and pre-registered success criteria. Some experiments are only legal on synthetic or shadow data — know which, before legal finds out for you.

**17.12 Cost/performance optimization.** Right-size instances to p99 (not peak), cache expensive features and static context, distill large models to small ones where accuracy budgets allow, reuse features across models, and batch what latency permits. Track cost per decision as a product metric with the same care as loss rate.

**17.13 SLOs, change control & DR.** SLOs for AI services (availability, latency, freshness of features, explanation-coverage) with error budgets that gate feature work; change control boards for model/policy changes (Phase 16's gates in operation); DR with RTO/RPO per service tier and multi-region failover for decisioning paths. These three documents — SLO sheet, CCB charter, DR plan — are what senior engineers are hired to produce.

**17.14 Operations culture.** Postmortems without blame but with owners and deadlines; docs-as-code (architecture, runbooks, model docs reviewed in PRs); dashboards reviewed on cadence, not just alerted. The culture test: when something breaks, does the organization learn something it did not know, or relearn something it filed away?

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| PSI / KS statistics | Distribution-shift measures with thresholds | Population-shift detection without labels | You discover drift via the P&L |
| Bootstrap inference on eval deltas | CIs over metric differences | Proving challenger > champion honestly | You ship noise and call it lift |
| SLO math & error budgets | (1 - reliability) budgets vs change velocity | Governing AI-service risk quantitatively | Debate-driven risk decisions |
| RTO / RPO arithmetic | Recovery time/point objectives vs cost | Sizing DR for decisioning services | Gaps sized by hope |
| Statistical power for experiments | Sample size for guardrail-metric deltas | Sizing holdouts in decisioning | Experiments that cannot detect what matters |
| Cost-per-decision accounting | Fixed + variable cost over decision volume | Unit economics of the ML product | Optimization aimed at the wrong term |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Data contracts at producer boundaries | Schema/range violations are the top silent killer; tests belong in CI |
| Registry-driven serving | Model version + policy version recorded per decision; mismatches are incidents |
| Shadow/canary infrastructure | Agreement analysis and instant rollback make risky changes survivable |
| Append-only audit store | The substrate for explanations, disputes, and regulatory response |
| Feature/platform parity | Training/serving skew checks automated, not assumed |
| IaC & multi-region basics | DR for decisioning is an architecture choice, not a runbook paragraph |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| MLflow | Experiment tracking and model registry (promotion states, lineages) |
| Evidently | Drift and data-quality reports wired into CI and monitors |
| NannyML | Post-deployment performance estimation with delayed labels |
| Alibi-Detect | Online drift detection for streaming and serving paths |
| Airflow / Prefect / dbt | Orchestration for batch scoring, backfills, and feature builds |
| Docker + Kubernetes | Packaging and horizontal serving; shadow/canary at the platform level |
| FastAPI | Decision and explanation endpoints with typed contracts |
| Prometheus + Grafana | SLO dashboards, alerting, error-budget tracking |
| GitHub Actions | ML-aware CI: data gates, model gates, reproducibility checks |
| Locust / k6 | Load tests that find the p99 bottleneck before production does |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| MLflow documentation (mlflow.org) | Docs | Intermediate | Registry/tracking | The default registry workflow you will implement | Essential |
| Evidently documentation (evidentlyai.com) | Docs | Intermediate | Monitoring | Drift/report patterns for CI and production | Essential |
| NannyML documentation (nannyml.com) | Docs | Intermediate | Delayed labels | Performance estimation without immediate labels | Recommended |
| Kubernetes documentation (kubernetes.io) | Docs | Intermediate | Platform | Serving primitives: probes, HPA, rollouts, quotas | Recommended |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Chip Huyen, *Designing Machine Learning Systems* (O'Reilly 2022) | Book | Intermediate | ML systems | The best single map of the platform landscape | Essential |
| *Reliable Machine Learning* (O'Reilly 2024; Chen et al., applied-SRE authors) | Book | Advanced | ML reliability | SRE discipline applied to ML at scale | Recommended |
| Kleppmann, *Designing Data-Intensive Applications* (2017) | Book | Intermediate | Data systems | The reliability/consistency foundations under everything | Reference |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Google Cloud MLOps whitepaper ("MLOps: Continuous delivery and automation pipelines in machine learning") | Whitepaper | Intermediate | Architecture | The canonical maturity-model framing | Recommended |
| DataTalksClub MLOps Zoomcamp (free, github.com/DataTalksClub) | Course | Intermediate | Hands-on | Guided end-to-end pipeline reps | Recommended |
| Made With ML (madewithml.com, Goku Mohandas) | Course | Intermediate | MLOps | Production workflow patterns with code | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Capital One Tech / Nubank engineering blogs | Blog | Advanced | FinTech platforms | Public write-ups of ML platform choices at regulated scale | Optional |
| SRE books from Google (sre.google, free online) | Book | Intermediate | Operations | Runbook, SLO, and postmortem culture sources | Recommended |

## 10. Practical Exercises

1. - [ ] Write the architecture selection memo: pick the right reference shape for three systems you own (credit, fraud, RAG), with explicit deviations and reasons.
2. - [ ] Build the ML-aware CI pipeline for your Phase 06 scorecard: data validation gate → training → registry → quality gate (bootstrap CI on golden set) → containerized FastAPI serving → deployment job.
3. - [ ] Add reproducibility: DVC/dataset snapshot + pinned image + seed; verify a cold rebuild by re-running from a clean checkout and diffing metrics.
4. - [ ] Simulate drift on IEEE-CIS (resample populations month by month); stand up Evidently + NannyML monitors; set PSI and performance-proxy alert thresholds and justify each number.
5. - [ ] Implement the shadow-deploy harness: challenger scores live (replayed) traffic silently; compute agreement, disagreement drivers, and write the promotion recommendation.
6. - [ ] Implement the decision audit store: append-only schema (inputs snapshot, model version, policy version, output, reasons, overrides) with retention automation; write the query that reconstructs any past decision.
7. - [ ] Break your own pipeline three ways (schema change, stale feature store, registry mismatch); follow your runbook each time and fix the runbook from what you learn.
8. - [ ] Load-test the scoring API with Locust/k6; find p99 and the bottleneck; fix it (pool, cache, ONNX); re-test and record before/after.
9. - [ ] Write the SLO sheet (availability, latency p99, feature freshness, explanation coverage) with error budgets, and wire the budget burn into Grafana alerts.
10. - [ ] Write the DR plan for the decisioning service: RTO/RPO per tier, multi-region notes, and a tabletop walkthrough with gaps identified.

## 11. Mini Projects

**M1 — Full MLOps pipeline for the Phase 06 scorecard.** Data validation → training → registry → FastAPI serving in Docker → CI/CD with gates → drift monitoring. Deliverable: one repo, one pipeline, one-command deploy and rebuild. Difficulty: ★★★★☆.

**M2 — Drift monitoring on IEEE-CIS.** Simulated drift + monitors + alert thresholds + a monthly "model health report" template. Deliverable: monitoring stack + report. Difficulty: ★★★☆☆.

**M3 — Shadow-deploy harness.** Champion/challenger scoring with agreement analysis and a written promotion decision. Deliverable: harness + decision memo. Difficulty: ★★★☆☆.

**M4 — Decision audit store.** Schema, append-only implementation, retention policy, and a reconstruction demo for an arbitrary past decision. Deliverable: store + demo queries. Difficulty: ★★★☆☆.

**M5 — Load-test and fix.** Locust/k6 campaign on the scoring API; profile, fix, re-test; document the p99 journey. Deliverable: report with graphs. Difficulty: ★★★☆☆.

## 12. Major Project Hook

Productionize **Flagship Project 2 — Credit Risk Decisioning System** or **Flagship 1 — Real-Time Fraud Detection Platform** (`/projects/flagship/`; index at `/projects/flagship/README.md`) to full production standard here: every pipeline, gate, store, and runbook the capstone defense will audit.

## 13. Case Studies & Industry Examples

- **Zillow Offers (2021)**: publicly reported shutdown after algorithmic pricing losses — the canonical ML-incident case: model assumptions, market shift, and operational scale interacting (see `/case-studies/README.md`).
- **Capital One / Nubank public engineering write-ups**: how large regulated institutions discuss ML platforms, feature stores, and model governance in public (selective, but instructive on structure).
- **Publicly reported fintech data-pipeline incidents** (various, 2021-2025): silent upstream breaks discovered via customer complaints rather than dashboards — the recurring argument for data-contract tests and freshness SLOs.
- **Your own fault inventory**: the three breaks you induced in Exercise 7 are legitimate case studies; write one up as a postmortem artifact.

## 14. Interview Questions

**Design MLOps for a regulated model.** Start from governance gates (Phase 16): registry with states, evidence-producing CI (data gates, model gates, reproducibility), staged deployment (shadow → canary), decision audit store, monitoring with pre-agreed retraining triggers, and rollback to last known good. The regulator's question — "show me why this decision and this change were safe" — should be answerable from the platform's artifacts.

**How do you monitor a model without labels?** Monitor the pipeline (data contracts, freshness), the population (PSI/KS), the outputs (score distributions, decision rates), and delayed-label proxies (early behavior, complaints); estimate performance with NannyML-style methods; reconcile against true labels when vintages mature.

**What deployment strategies apply to models with regulatory approval?** Approval is version-specific: shadow first, canary on limited traffic with instant rollback, champion/challenger with pre-registered promotion criteria, and a change record linking the approval to the deployed artifact. The approved version must be reconstructable bit-for-bit.

**How do you run ML on-call?** Triage taxonomy (pipeline, population, serving, policy), runbooks with concrete first steps and expected durations, escalation to model owners, and periodic game days. Silent failures mean alerting on the pipeline, not just the model.

**PSI vs KS — when do you use which?** PSI bins and is standard on scores/categoricals with established thresholds; KS is distribution-free and better for continuous features without binning choices. Both detect population shift, neither proves performance loss — they trigger investigation.

**What belongs in a decision audit store?** Input snapshot, feature values, model version, policy version, output, reasons, human overrides, timestamps — append-only with retention rules, and the ability to reconstruct any decision exactly. It serves adverse action, disputes, and postmortems.

**How do error budgets work for an AI service?** SLO (e.g., 99.5% within-latency decisions) defines the budget; burn releases or freezes change velocity. For decisioning services, budget exhaustion means holding feature/policy changes — reliability becomes a shared currency between ML and platform teams.

**Where does cost per decision come from and how do you reduce it?** Fixed platform cost amortized over volume plus per-decision compute (features, inference, logging). Levers: right-sizing, caching, distillation, feature reuse, batch-what-you-can — never by dropping audit or explainability coverage, which are compliance features.

**What makes a good postmortem for an ML incident?** Timeline, contributing causes (including model/policy/monitoring interactions), what the dashboards showed vs what was true, corrective actions with owners and dates, and a check for the same class of failure elsewhere. Blameless on people, unsparing on systems.

**Build vs buy the feature store — how do you decide?** Parity guarantees and governance evidence are the hard requirements; buy accelerates both if the vendor fits your stack, but glue builds often win at small scale. Decide on migration cost, team size, and audit fit — not feature-checklist length.

## 15. Assessment — Can You Pass the Bar?

- [ ] Ship one model through the full pipeline: validated data → gated training → registry → containerized serving → CI/CD → monitoring, reproducible from a cold checkout.
- [ ] Show drift monitors firing on simulated drift with thresholds you can justify in writing.
- [ ] Demonstrate shadow deploy with agreement analysis and a written promotion decision.
- [ ] Reconstruct any past decision from your audit store, including reasons and overrides.
- [ ] Produce p99 before/after evidence from your load-test campaign.
- [ ] Present an SLO sheet and DR plan for the decisioning service, and survive 10 minutes of "what breaks?" questioning.
- [ ] Explain to a risk officer how deployment, rollback, and retraining respect the governance gates.

## 16. Mastery Checkpoint

You may proceed to Phase 18 when:

1. Your flagship repo (Flagship 2 or 7) meets the definition-of-done spine: pipeline, gates, audit store, monitors, runbooks, SLOs, DR plan.
2. You have run one game-day: induced a silent failure and recovered within your own SLOs using your runbooks.
3. Your postmortem artifact (from a real or induced fault) shows systemic fixes, not blame.

Evidence: repo links + game-day log + postmortem + SLO/DR docs. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Monitoring the model, not the pipeline: stale features produce confident nonsense with green dashboards.
- Treating model promotion as a deploy: without shadow/canary and rollback triggers, every release is a bet the book cannot afford.
- Audit stores built for compliance only — then postmortems and dispute defense discover they lack the fields everyone needed.
- Thresholds copied from blog posts; PSI limits must come from your population's variance and business tolerance.
- Cost optimization that quietly removes logging or explanation coverage — savings now, findings later.
- Runbooks written during the incident; drills and game days exist precisely to prevent this.
- Reproducibility claims without a stranger-tested cold rebuild; pins drift faster than documentation.

## 18. Where This Goes Next

Phase 18 is where the whole curriculum assembles: you take a flagship end to end with the architectures, gates, and operations discipline built here, under the compliance frame of Phase 16. Phase 19 then lets you specialize — quant, wealth, insurance, graph, PETs, or treasury — while Phase 20 converts the craft into architecture judgment and leadership.
