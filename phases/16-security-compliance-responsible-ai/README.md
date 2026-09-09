# Phase 16 — Security, Compliance & Responsible AI

> **Stage VI — Production & Leadership** · **Duration: 3-4 weeks** · **Mastery target: Production → Governance**
> **Position in path:** `15-real-time-streaming` ← **this phase** → `17-production-fintech-ai`

## 1. Objective

In finance, the model that cannot be governed cannot ship: this phase turns model risk management (SR 11-7), the EU AI Act's high-risk obligations, consumer-protection law, fairness engineering, privacy-enhancing technologies, and ML security into engineering disciplines you can execute. You will build a model risk dossier, run fairness audits with mitigation trade-offs, red-team an LLM application, and construct the auditability artifacts validators expect. You will finish able to speak both languages fluently — regulator and engineer — and to design systems that pass the conversation.

## 2. Why It Matters in Finance

Banks spend substantial, publicly reported sums on compliance and model governance annually, and the spend is growing as AI regulation phases in. The EU AI Act (Regulation (EU) 2024/1689, in force August 2024) classifies creditworthiness assessment as high-risk, attaching risk-management, data-governance, logging, and human-oversight duties (phase-in timing: verify current status). DORA (in force since January 2025) makes operational resilience an explicit duty. Meanwhile US fair-lending enforcement and privacy law constrain every decision system you build. For the AI engineer, governance is not a department — it is a set of build requirements.

- SR 11-7 (2011) remains the US model-risk anchor; US agencies were updating model-risk guidance during 2025-2026, including GenAI questions — verify current status before you quote it in a memo.
- Adverse-action law (ECOA/Reg B) requires specific, accurate reasons per decline — your explainability stack is a legal control, and CFPB Circular 2022-03 extends the expectation to complex models.
- GDPR Article 22 gives individuals rights around solely automated decisions with significant effects; credit is the canonical example. CCPA/CPRA adds US-side duties.
- Fairness is a recurring testing regime, not a one-off audit: populations, products, and proxies drift.
- Security failures are existential: Equifax 2017 remains the canonical demonstration of what poor data hygiene costs.

## 3. Prerequisites

- [ ] Phase 06 — credit decisioning: scorecards, adverse-action reasons, PSI monitoring
- [ ] Phase 12/13/14 — GenAI, RAG, agents (you will secure what you built)
- [ ] Phase 07/08 — fraud/AML systems (high-risk use cases with governance obligations)
- [ ] Comfort reading primary regulatory text (you will read summaries AND sources)
- [ ] Working SHAP/knowledge of model explanation techniques (assumed known)

## 4. Learning Outcomes

- I can explain SR 11-7's development/validation/use controls and the three lines of defense, and prepare artifacts that survive independent validation.
- I can maintain a model inventory with tiering and state each model's approved use scope.
- I can map EU AI Act high-risk obligations (risk management, data governance, logging, human oversight) and DORA duties onto a concrete AI delivery plan.
- I can engineer adverse-action reason flows hardened for Reg B language and CFPB 2022-03 expectations.
- I can run a fairness audit — metrics, conflicts, proxy analysis, mitigation, accuracy cost — as a recurring process.
- I can apply privacy techniques appropriately: pseudonymization/tokenization, k-anonymity limits, differential privacy, and federated learning concepts.
- I can threat-model an ML system using MITRE ATLAS and the OWASP Top 10 for LLM Applications, and execute a red-team script.
- I can design an AI governance operating model: committees, lifecycle gates, change management.
- I can engineer auditability: immutable logs, lineage, and reproducibility via pinned seeds, environments, and data snapshots.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 16.1 | Model risk management as engineering | SR 11-7 development/validation/use controls | MRM obligations map |
| 16.2 | Model inventory & tiering | risk tiers, use scopes, lifecycle states | inventory schema + seed entries |
| 16.3 | Independent validation readiness | what validators ask; evidence packs | validation-readiness checklist |
| 16.4 | EU AI Act & DORA | high-risk duties; operational resilience | compliance mapping matrix |
| 16.5 | US consumer-protection stack | ECOA/Reg B, FCRA, GLBA safeguards | obligations-per-system table |
| 16.6 | Privacy law in practice | GDPR Art. 22, CCPA/CPRA, data minimization | privacy design checklist |
| 16.7 | Fairness engineering | metrics, conflicts, proxies, recurring testing | fairness audit + memo |
| 16.8 | Explainability toolbox, applied | reason codes, counterfactuals, per-decision | hardened reason-code generator |
| 16.9 | Privacy-enhancing tech | pseudonymization, k-anonymity limits, DP, FL | PET selection note + DP demo |
| 16.10 | Secure ML | adversarial robustness, poisoning, theft, injection | threat model (ATLAS + OWASP) |
| 16.11 | Third-party AI vendor risk | interagency guidance (2023); vendor due diligence | vendor risk questionnaire |
| 16.12 | AI governance operating model | committees, gates, change management | governance charter draft |
| 16.13 | Auditability engineering | immutable logs, lineage, reproducibility | reproducible-run artifact set |
| 16.14 | Incident response & red-teaming | AI playbooks; LLM red-team scripts | playbook + red-team log |

**16.1 Model risk management as engineering.** SR 11-7 frames model risk across development, validation, and use — documentation sufficient for independent re-performance, controls over implementation, and monitoring in use. Treat it as an engineering spec: your repo should produce the evidence as a byproduct of building, not as a pre-audit scramble. Note that US agencies were revisiting MRM guidance during 2025-2026 (GenAI partially covered) — verify current status; OCC 2011-12 is the OCC's mirror of SR 11-7, and EU institutions run analogous internal-model governance.

**16.2 Model inventory & tiering.** Every model gets an entry: owner, purpose, tier (materiality × complexity), approved use scope, validation status, monitoring plan, and next review date. Tiering drives everything downstream — validation depth, change-control rigor, monitoring cadence. A model used outside its approved scope is a finding waiting to happen; the inventory is where scope is written down.

**16.3 Independent validation readiness.** Validators test conceptual soundness, data quality, outcomes analysis, and ongoing monitoring. Prepare the pack: development document, data lineage, feature definitions with point-in-time logic, benchmark comparisons, sensitivity and robustness checks, limitation statements. The skill is anticipating re-performance: could a stranger rebuild your model from your artifacts alone?

**16.4 EU AI Act & DORA.** The AI Act (Reg (EU) 2024/1689, in force Aug 2024) makes creditworthiness assessment high-risk: mandatory risk management, data governance, technical documentation, logging, human oversight, and accuracy/robustness/cybersecurity attention, with high-risk obligations phasing in around 2026 (verify current phase-in). DORA (applies since Jan 2025) adds ICT operational-resilience duties — incident reporting, resilience testing, third-party oversight — that hit your serving infrastructure directly. Map each obligation to a build artifact, not a policy sentence.

**16.5 US consumer-protection stack.** ECOA/Reg B: adverse action notices with specific principal reasons; FCRA: duties when consumer reports feed decisions; GLBA: safeguards for financial data. For the engineer these translate into reason-code pipelines, permissible-purpose and data-source controls, and security programs. CFPB Circular 2022-03 states creditors using complex models must still provide specific reasons — design for it.

**16.6 Privacy law in practice.** GDPR Art. 22 restricts solely automated decisions with legal or similarly significant effects — meaning human review paths and meaningful information about the logic. CCPA/CPRA adds access/deletion and opt-out duties. Data minimization is your default architecture principle: collect less, retain shorter, pseudonymize earlier — every field you skip is a field you never have to defend.

**16.7 Fairness engineering.** Metrics — demographic parity, equalized odds, disparate impact ratio (the four-fifths heuristic), calibration by group — conflict mathematically (impossibility results, e.g., Kleinberg et al. 2016; Chouldechova 2017); choosing one is a values-plus-law decision, not a coding exercise. Proxy discrimination (zip code, device, income source) defeats attribute exclusion. The deliverable is a recurring testing regime with documented trade-offs, not a one-time audit report.

**16.8 Explainability toolbox, applied.** Reason codes from scorecards, SHAP-to-reason mapping with stability controls for ML, counterfactual explanations ("what would change this decision") via dice-ml, per-decision logging at serving time. Phase 06 built these; here you harden them: stable language, aggregated correlated drivers, and output that survives a compliance review of the exact wording.

**16.9 Privacy-enhancing tech.** Pseudonymization and tokenization are baseline; k-anonymity hides individuals in crowds but leaks at tail distributions and resists composition poorly. Differential privacy adds calibrated noise (ε budget math) — usable for aggregate reporting and DP-SGD training at a measured utility cost. Federated learning (Flower as reference framework) keeps data local for cross-institution collaboration but leaks through gradients without DP/b secure aggregation. Match the PET to the threat; PETs are controls, not talismans.

**16.10 Secure ML.** Threats: adversarial examples at inference, data poisoning upstream, model theft via query APIs, and prompt injection for LLM systems. Catalogs: MITRE ATLAS for ML attacks and the OWASP Top 10 for LLM Applications (verify current version). Engineer defenses proportionate to the threat model — rate limits and output filtering beat exotic robustness training for most fintech systems.

**16.11 Third-party AI vendor risk.** The 2023 interagency guidance on third-party relationships (Fed/FDIC/OCC) makes vendor AI risk your risk. Due diligence: model provenance and documentation, validation rights, data flows, sub-processors, incident SLAs, audit access. Your vendor questionnaire is an engineering artifact — the answers must map to the same evidence packs you produce in-house.

**16.12 AI governance operating model.** Committees (model risk, AI/GenAI councils), policies, lifecycle gates (approve to develop, approve to deploy, approve to change), and change management with model revalidation triggers. The failure pattern is governance as theater: gates with no teeth. The working pattern is risk-tiered gates with clear evidence requirements and a documented exception path.

**16.13 Auditability engineering.** Immutable append-only decision and run logs, data lineage from source to feature, and reproducibility: pinned dependencies, container images, seeds, and immutable data snapshots so validators can re-run. This generalizes the audit stores of Phases 06, 14, and 15 into one architecture.

**16.14 Incident response & red-teaming.** AI-specific playbooks: model degradation incidents, data-pipeline poisoning, LLM data leakage, discriminatory-output findings — each with detection, containment, notification (who: legal, regulator, DPO), and rollback. Red-team LLM applications on schedule: injection, exfiltration, PII leakage, jailbreaks — with the same seriousness as pen-testing.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Fairness metric formalism | Demographic parity, equalized odds, calibration as equations | Metric choice is a legal/ethical commitment | You pick metrics from blogs and get picked apart in review |
| Impossibility results | Calibration vs equalized odds tradeoffs (Kleinberg et al. 2016) | Explains why no metric satisfies everyone | You promise stakeholders the impossible |
| Disparate impact ratio | Group approval-rate ratio; four-fifths heuristic | US fair-lending screen | Your model passes AUC and fails a title-vii-style screen |
| Differential privacy (ε, sensitivity) | Noise calibrated to query sensitivity and budget | Defensible aggregate sharing and DP-SGD | You either leak or destroy utility with ad-hoc noise |
| k-anonymity limits | Generalization classes and their tail leakage | Knowing when anonymization is not | You publish "anonymous" data that re-identifies |
| Subgroup sample sizes | Error bars on small-cohort metrics | Fairness claims need statistical honesty | You flag or clear groups on noise |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Immutable decision/run logs | Append-only, retention-aware audit stores are the substrate of every defense |
| Data lineage & snapshots | Validators and the EU AI Act's data-governance duties demand provenance you can show |
| Reproducibility (pins, seeds, images) | Re-performance is the validator's core method; make it one command |
| Tokenization & PII vaults | Minimize exposure at the schema level, not by policy documents |
| Policy gates in CI | Model quality and compliance checks as merge-blocking controls, not wikis |
| PII minimization & retention automation | Deleting on schedule is an engineering job; forgetting is a finding |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| fairlearn | Fairness metrics and mitigation algorithms (post-processing, reweighting) |
| AIF360 (IBM) | Broader fairness toolkit incl. bias mitigation primitives |
| dice-ml | Counterfactual explanations for individual decisions |
| Microsoft Presidio | PII detection, redaction, and anonymization pipelines |
| Opacus | Differential-privacy SGD for PyTorch training |
| Flower | Federated learning framework for cross-silo experiments |
| OWASP LLM checklists | Guardrail and threat-coverage checklists for LLM apps |
| Git LFS / DVC + containers | Data snapshots and pinned environments for reproducibility |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| SR 11-7 (Federal Reserve/OCC, 2011; federalreserve.gov) | Regulation | All | MRM | The anchor document of model governance | Essential |
| EU AI Act, Reg (EU) 2024/1689 (eur-lex.europa.eu) | Regulation | All | AI regulation | High-risk obligations for creditworthiness systems | Essential |
| DORA, Reg (EU) 2022/2554 (eur-lex.europa.eu) | Regulation | All | Resilience | ICT/operational-resilience duties since Jan 2025 | Essential |
| NIST AI RMF 1.0 + Generative AI Profile (nist.gov) | Framework | All | AI risk | The de-facto US risk-management vocabulary | Essential |
| ISO/IEC 42001:2023 (AI management systems) | Standard | Intermediate | Governance | Auditable AI management-system structure | Recommended |
| CFPB Circular 2022-03 (consumerfinance.gov) | Guidance | All | Explainability | Reasons required even for complex models | Essential |
| ECOA / Regulation B overview (consumerfinance.gov) | Regulation | All | Fair lending | Statutory basis for adverse-action duties | Essential |
| GDPR text, esp. Art. 22 (eur-lex.europa.eu) | Regulation | All | Privacy | Automated-decision rights you must design around | Essential |
| OWASP Top 10 for LLM Applications (owasp.org) | Standard | All | LLM security | Threat catalog for everything you built in Phases 12-14 | Essential |
| MITRE ATLAS (atlas.mitre.org) | Knowledge base | Intermediate | ML attacks | Adversarial-ML tactics mapped like ATT&CK | Recommended |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Barocas, Hardt & Narayanan, *Fairness and Machine Learning* (free online, 2023) | Book | Advanced | Fairness | Formal foundations behind the metrics | Essential |
| Molnar, *Interpretable Machine Learning* (free online) | Book | Intermediate | Explainability | Counterfactuals and reason techniques done right | Essential |
| Dwork & Roth, *The Algorithmic Foundations of Differential Privacy* (2014, free) | Book | Advanced | DP | The formal DP basis if you implement it | Optional |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| IAPP practical guides (iapp.org) | Guides | Intermediate | Privacy ops | Operational privacy practice, not just statute text | Recommended |
| Public AI governance frameworks from major banks | Reports | Intermediate | Governance | How institutions structure committees and gates in public | Recommended |
| 2023 US interagency guidance on third-party relationships | Guidance | Intermediate | Vendor risk | The basis of AI-vendor due diligence | Essential |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| fairlearn / AIF360 / dice-ml documentation | Docs | Intermediate | Tooling | Worked examples for your audits | Recommended |
| NIST AI RMF Playbooks & community profiles (nist.gov) | Reports | Intermediate | Implementation | Cross-industry implementation notes | Optional |

## 10. Practical Exercises

1. - [ ] Read SR 11-7 and produce a one-page obligations map: development, validation, use — with the artifact each obligation demands from a working repo.
2. - [ ] Draft a model inventory schema (owner, tier, use scope, status, monitoring, review date) and seed it with every model you built in Phases 06-15.
3. - [ ] Run a fairness audit with fairlearn on a Home Credit model: disparate impact ratio, equalized-odds gaps, calibration by segment; apply one mitigation and write the accuracy-trade-off memo.
4. - [ ] Test proxy discrimination: exclude protected attributes, then measure how much demographic information survives in zip/income/device proxies (mutual information or predictive probe).
5. - [ ] Differential-privacy experiment: DP-noise an aggregate reporting pipeline at three ε levels; then DP-SGD (Opacus) on a small fraud model; chart utility delta vs ε and write the recommendation.
6. - [ ] Build the threat model for your Phase 13/14 LLM app using MITRE ATLAS + OWASP LLM Top 10; write a 10-payload red-team script (injection, exfiltration, PII) and run it; log results and fixes.
7. - [ ] Harden the adverse-action generator: map SHAP attributions to a controlled reason library; have three adversarial reviewers (compliance persona) attack the wording; iterate until the language survives.
8. - [ ] Implement reproducibility: pin data snapshot (DVC), container image, seeds; have a colleague re-run your Phase 06 model from scratch and record any drift between runs.
9. - [ ] Draft a vendor-risk questionnaire for an AI underwriting vendor: provenance, validation rights, data flows, sub-processors, incident SLAs, audit access.
10. - [ ] Write the AI incident playbook: model-degradation, poisoning, and LLM-leakage scenarios with detection, containment, notification, and rollback steps; tabletop-test one scenario.

## 11. Mini Projects

**M1 — Model risk dossier.** SR 11-7-shaped development document for your Phase 06 credit model: purpose, data, conceptual soundness, outcomes analysis, limitations, monitoring plan. Deliverable: dossier a validator would accept to start review. Difficulty: ★★★☆☆.

**M2 — Fairness audit + trade-off memo.** Home Credit or German Credit: metrics, mitigation (fairlearn), accuracy cost, proxy analysis, and a written recommendation with the metric-choice justification. Deliverable: audit notebook + 2-page memo. Difficulty: ★★★☆☆.

**M3 — Differential-privacy study.** DP aggregate reporting + DP-SGD on a small model; utility-vs-ε curves and a deployment recommendation. Deliverable: study notebook. Difficulty: ★★★★☆.

**M4 — LLM threat model + red team.** Full ATLAS/OWASP threat model for your RAG or agent app; 10-payload script; before/after hardening evidence. Deliverable: threat model + red-team log. Difficulty: ★★★☆☆.

**M5 — Hardened adverse-action generator.** Reason-code pipeline with stable, Reg-B-aware language and per-decision logging. Deliverable: generator + compliance-review notes. Difficulty: ★★★☆☆.

**M6 — Inventory + validation-readiness kit.** Inventory schema, seeded entries, and the checklist + evidence templates your repos emit. Deliverable: kit adopted across your repo. Difficulty: ★★☆☆☆.

## 12. Major Project Hook

Governance artifacts from this phase attach directly to your flagship projects: every flagship's spec (`/projects/flagship/`) includes the model-risk dossier, fairness audit, and threat model you produce here — the capstone defense in Phase 18 assumes they exist.

## 13. Case Studies & Industry Examples

- **Equifax (2017)**: publicly reported breach affecting ~147m consumers, rooted in unpatched known vulnerabilities and data-hygiene failures — the permanent argument that security is a modeling-adjacent engineering discipline (see `/case-studies/README.md`).
- **Fair-lending enforcement involving automated underwriting** (public CFPB/DOJ actions through 2023-2024): recurring themes — insufficient testing, proxy effects, and reason-giving failures; read the public orders for the control language.
- **Danske Bank Estonia (publicly reported from 2018)**: AML governance failure at scale — monitoring existed, escalation and governance failed; the lesson maps directly to alert-triage agent governance in Phase 14.
- **EU AI Act implementation (2024-2026)**: public standards-development and enforcement-preparation activity is ongoing; follow it — your credit models are squarely in scope (verify current phase-in status).

## 14. Interview Questions

**Explain SR 11-7's three lines of defense.** First line: model owners/developers who build and use models with controls. Second line: independent model risk management that validates and challenges. Third line: internal audit checking the whole framework. The engineer lives in line one but builds for line two — evidence sufficient for independent re-performance.

**What does the EU AI Act require for credit scoring models?** Creditworthiness assessment is high-risk: risk-management systems, data governance, technical documentation, automatic logging, human oversight, accuracy/robustness/cybersecurity attention, plus registration and transparency duties — phasing in around 2026 (verify current status). Practically: governance artifacts and logging become build requirements.

**Which fairness metric would you choose for lending, and why?** Start from the legal context (disparate impact screening) plus error-rate parity concerns for allocation harms, while keeping calibration by segment for decision quality — then document the conflict you accepted, because impossibility results mean you cannot have all three. The justification memo matters as much as the metric.

**What is GDPR Article 22?** The right not to be subject to solely automated decisions with legal or similarly significant effects, with exceptions (contract necessity, consent, law) and duties: safeguards including human intervention, an ability to contest, and meaningful information about the logic. In credit, this drives human-review paths and explanation surfaces.

**Design a model validation process.** Scoping by tier → evidence pack (dev doc, lineage, benchmarks, sensitivity, limitations) → independent re-performance and challenge → findings with severity and remediation → approved use scope → monitoring plan and review cadence. Treat validator questions as a test suite you run before they do.

**How do you secure an LLM application handling financial data?** Threat-model with OWASP LLM Top 10 + MITRE ATLAS: input filtering and injection-resistant tool design, output filtering against PII/exfiltration, least-privilege tools, no secrets in context, rate limits against model theft, and red-teaming on schedule.

**What is your data-minimization strategy for a decisioning system?** Collect per approved purpose, pseudonymize/tokenize at ingestion, minimize feature sprawl (proxy risk is also privacy risk), enforce retention schedules in code, and snapshot only what reproducibility requires — under access control.

**A validator rejects your model. What do you do?** Triage the findings: conceptual-soundness challenges get reasoned written responses or architecture changes; data-quality findings get pipeline fixes; performance findings get rework. Never argue AUC at a governance review; respond in evidence.

**How do you make a model reproducible for re-performance?** Immutable data snapshot, pinned environment and container, fixed seeds, deterministic feature logic with point-in-time correctness, and a one-command runbook — validated by a stranger re-running it cold.

**What goes into AI vendor due diligence?** Model provenance and documentation, your right to validate or review validations, data flows and sub-processor chains, performance/monitoring guarantees, incident SLAs, and audit access — mapped to the same evidence standard you hold in-house per the 2023 interagency guidance.

## 15. Assessment — Can You Pass the Bar?

- [ ] Explain SR 11-7's three lines of defense and the evidence each expects from engineering.
- [ ] Map EU AI Act high-risk duties for a credit model to concrete build artifacts.
- [ ] Run a fairness audit, choose a metric with written justification, and accept its trade-offs honestly.
- [ ] Explain GDPR Art. 22 and point to the human-review path in your own system.
- [ ] Red-team your LLM app and demonstrate a closed loop from finding to fix.
- [ ] Hand a stranger your repo and have them re-perform your model from snapshots and pins.
- [ ] Explain to a regulator-in-role how your reason codes, logs, and oversight satisfy Reg B and CFPB 2022-03.

## 16. Mastery Checkpoint

You may proceed to Phase 17 when:

1. Artifacts exist and are attached to your flagship repos: model risk dossier, inventory, fairness audit + memo, threat model + red-team log, hardened reason codes, reproducibility kit.
2. You have completed one tabletop incident exercise with written timelines.
3. You can defend your governance choices in a 10-minute mock model-council session (record it; store under `/notes/artifacts/`).

Evidence: dossier + audit memo + red-team log + recorded council session. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Treating compliance as documentation written after the build instead of requirements that shape it.
- Metric-shopping: running every fairness metric and reporting only the flattering one — validators and plaintiffs both notice.
- Excluding protected attributes and declaring fairness done, while proxies do the discriminating.
- Differential privacy applied without an ε budget story, destroying utility or providing no guarantee.
- Red-teaming the demo, not the deployment: injection paths through tool results and documents go untested.
- Vendor models adopted without validation rights — you own the outcome but cannot see the model.
- Retention policies in PDFs; deletion obligations enforced nowhere in code.

## 18. Where This Goes Next

Phase 17 operationalizes everything: the governance gates, audit stores, monitoring, and incident playbooks you designed here become the CI/CD, observability, and on-call reality of a production FinTech ML platform. Governance defines what must be true; production engineering makes it true every day.
