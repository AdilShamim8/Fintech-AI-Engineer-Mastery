# Phase 12 — Generative AI for Finance

> **Stage V — Advanced Financial AI** · **Duration: 3-4 weeks** · **Mastery target: Application → Production**
> **Position in path:** `11-financial-nlp-documents` ← **this phase** → `13-financial-rag-knowledge`

## 1. Objective

You already know how to build LLM applications; this phase is about the specific physics of doing it inside financial institutions — where the outputs feed regulated processes, the inputs are confidential, the knowledge expires quarterly, and a confident hallucination about a balance sheet is a business incident. You will learn to select the right tool level (prompt vs RAG vs fine-tune), build finance-grade copilots with citation-forced answers and human approval gates, evaluate with task-based golden sets, and deploy with the data-residency, audit, and cost constraints that banks actually impose.

## 2. Why It Matters in Finance

Finance is the highest-ROI, highest-liability environment for generative AI: it is text-heavy, numeric, compliance-bound, and full of expensive experts whose time is spent reading, comparing, and drafting. Publicly reported deployments — research assistants at major banks, contract summarization, earnings-call analysis, service automation — show the value; the same period produced cautionary tales about over-claimed automation. The engineering gap between a demo and a defensible financial copilot is exactly what this phase closes.

- Financial documents are adversarial to naive LLM use: tables, nested definitions, versioned regulatory text, and numbers that must be quoted exactly.
- Advice, suitability, and consumer-protection rules constrain what an assistant may say — "not financial advice" disclaimers are not a compliance architecture.
- EU AI Act treats several finance-adjacent uses (creditworthiness, risk assessment) as high-risk, with documentation and human-oversight duties; regulators expect audit trails for AI-assisted decisions.
- Data confidentiality and residency decide the deployment model (API vs private hosting) before any modeling choice is made.

## 3. Prerequisites

- [ ] Existing AI engineering: prompting, structured outputs, embeddings, serving basics (assumed)
- [ ] Phase 11 — financial text domains, extraction, evaluation habits
- [ ] Phase 01-02 — enough banking context to know what a copilot is *for*
- [ ] Phase 05 — experimental discipline (you will need it for evals)

## 4. Learning Outcomes

- I can classify a finance GenAI use case by risk tier and choose prompt/RAG/fine-tune accordingly, with a written justification.
- I can build a document copilot whose every numeric claim is extracted-with-citation rather than generated-from-memory.
- I can design human-in-the-loop approval gates and explain which workflow steps must stay deterministic for compliance.
- I can construct a golden-set evaluation harness (task metrics, LLM-as-judge with calibration checks, human gold slices) and run it in CI.
- I can implement PII redaction and confidential-context controls for prompts and logs.
- I can deploy under enterprise constraints: private endpoints (vLLM) vs managed APIs, residency, logging, cost per query.
- I can red-team a finance copilot for injection, data exfiltration, and confident-wrong-numbers failure modes.
- I can write the ROI case for a copilot: minutes saved, quality deltas, risk accepted.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 12.1 | The finance GenAI landscape | Where value is real vs hyped; deployment archetypes | use-case map note |
| 12.2 | Risk-tiering use cases | Advice vs assistance; high-risk triggers; human oversight design | risk-tier decision tree |
| 12.3 | Financial prompting patterns | Analyst personas, structured schemas, numeric-grounding prompts | prompt library |
| 12.4 | Structured outputs & schemas | JSON-schema outputs for downstream systems, validation gates | schema-validated extractor |
| 12.5 | Grounding & citation-forced answers | Extract-then-answer vs generate-then-verify | cited summarizer on a 10-K |
| 12.6 | Numeric fidelity | Tables, units, restatements; exact-quote discipline; verification passes | number-checking harness |
| 12.7 | RAG vs fine-tune vs prompt | Decision framework (freshness, specificity, cost, auditability) | written decision memo |
| 12.8 | Domain adaptation lite | LoRA/QLoRA for style/tasks; when it helps in finance | small fine-tune experiment |
| 12.9 | Evaluation harnesses | Golden sets, rubrics, LLM-as-judge pitfalls, regression gates | eval harness + report |
| 12.10 | Safety & compliance controls | PII redaction, logging, advice boundaries, disclaimers that work | redaction + policy layer |
| 12.11 | Deployment patterns | Self-host (vLLM) vs API; residency; caching; cost routing | deployment comparison doc |
| 12.12 | Red-teaming finance copilots | Injection via documents, exfiltration, overconfident numerics | red-team script + findings |

**12.1 The finance GenAI landscape.** Archetypes: document copilots (summarize/compare filings), research assistants (company intel with citations), compliance assistants (policy Q&A, audit drafting), service automation (client support with human escalation), and code/data assistants for analysts (SQL/pandas generation with execution validation). Study 2-3 publicly reported deployments and write down what they claimed and what they conspicuously did not claim.

**12.2 Risk-tiering use cases.** A summarization assistant for analysts, a client-facing advice bot, and a credit-decision support tool are different species. Build a decision tree: does output influence a regulated decision? does it face customers? can a human review it? Does the use case trip high-risk categories (creditworthiness assessment does)? The output of this lesson is a gate you will apply to every future idea.

**12.3 Financial prompting patterns.** Finance-specific patterns: force the model to quote figures with their source location, forbid arithmetic in the generator (route to a calculator), define ambiguity handling ("the filing reports two figures for X in different sections — surface both"), and role framing that fixes the audience (IR analyst vs compliance officer). Keep a versioned prompt library; prompts are governed artifacts in banks.

**12.4 Structured outputs.** Downstream systems consume schemas, not prose. Practice schema-constrained outputs (JSON schema / function calling / constrained decoding) with validation-and-retry loops, and learn why "validate before commit" is the difference between a copilot and an incident.

**12.5 Grounding & citation-forced answers.** The single most valuable finance copilot property: every claim carries a pointer to a source document and location. Implement extract-then-answer: retrieval supplies spans, the model composes only from spans, and a verifier checks each claim's span exists. This anticipates Phase 13's full RAG stack — here you build the answer-side discipline.

**12.6 Numeric fidelity.** LLMs mangle finance numbers: wrong units (thousands vs millions), restated comparatives, share vs dollar figures, per-share dilution. Build a verification pass that re-extracts every number in the answer from the source table and diffs them; treat mismatches as hard failures. This one habit separates professional from demo-grade copilots.

**12.7 RAG vs fine-tune vs prompt.** Decision framework: knowledge freshness and auditability push toward RAG; task *style/format* gaps push toward light fine-tuning; both fail if the base model lacks capability. Write the memo for a real case (e.g., earnings-call summarizer): you will almost always land on RAG + prompting, with fine-tuning reserved for narrow, stable tasks.

**12.8 Domain adaptation lite.** LoRA/QLoRA on a few hundred to few thousand examples can adapt tone, format compliance, and narrow classification/extraction tasks. Run one honest experiment: base vs fine-tuned on a finance subtask with a fixed eval; measure whether the delta justifies the lifecycle cost (data curation, retraining, re-eval each model refresh).

**12.9 Evaluation harnesses.** Build a golden set (50-200 finance Q&A/summary items with expert answers or rubrics), task metrics (extraction precision/recall, citation validity, numeric fidelity rate), and LLM-as-judge for fluency/reasonableness — with a calibration check of the judge against your human labels. Wire it to CI so prompt or model changes cannot silently regress.

**12.10 Safety & compliance controls.** Layers: input PII detection/redaction (Presidio), context classification (what may enter a prompt), output policy checks (advice-sounding language, guarantees, omissions), full prompt/response audit logging with retention rules, and human approval where outputs touch regulated processes. Understand why generic "AI safety" tooling needs finance-specific policies.

**12.11 Deployment patterns.** Compare: managed APIs (fast, residency questions, per-token cost), private hosting (vLLM/TGI on your cloud — data control, ops burden), and hybrid routing (small local models for classification/redaction, big models for reasoning). Do the math on cost per query with caching (semantic + exact) and model routing; most production finance copilots are hybrids.

**12.12 Red-teaming finance copilots.** Attack surfaces: prompt injection hidden in retrieved documents (a malicious PDF is an untrusted input), convincing the assistant to reveal context from other documents or sessions, and confident wrong numbers (the most likely "successful attack" is accidental). Build an adversarial corpus and a regression test that runs it on every release.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Agreement statistics (κ) | Chance-corrected rater agreement | Validating LLM-as-judge against human labels | You trust a judge that disagrees with your experts |
| Confidence intervals on eval metrics | Bootstrap CIs on small golden sets | Deciding whether model B beats A with 150 eval items | You ship regressions/illusions from noise |
| Cost-per-decision arithmetic | Token cost × traffic × routing mix | Budget defense to a CFO; routing design | Your copilot is a demo with a surprise bill |
| Calibration of judges | Judge scores vs human gold | Trustworthy automation of evaluation | CI gates that gate the wrong thing |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Audit logging of prompts/responses | Regulated context: every AI-assisted action reconstructable |
| PII redaction pipelines (Presidio) | Confidential data must not leak into prompts or vendor logs |
| Semantic caching | Cost and latency at query-heavy workloads (policy Q&A) |
| Schema validation + retry | Downstream systems must never parse prose |
| CI for prompts/models | Eval regression gates on every change |
| Data residency & private endpoints | Bank procurement blocks non-compliant deployments |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| OpenAI/Anthropic APIs + structured outputs | Baseline capability and schema control |
| vLLM | Self-hosted serving for private deployments |
| LangChain / LlamaIndex | Rapid composition (use with judgment; understand what they hide) |
| Presidio | PII detection and redaction |
| Ragas / DeepEval / promptfoo | Eval harness starting points (finance golden sets are yours to build) |
| Langfuse / Arize Phoenix | Tracing, evaluation, and audit-friendly observability |
| PEFT (LoRA/QLoRA) | Domain-adaptation experiments |
| Guardrails libraries | Output policy enforcement patterns |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| OpenAI & Anthropic official docs (structured outputs, tool use, safety) | Docs | All | Building blocks | Ground truth for APIs you will actually call | Essential |
| Wu et al. (2023), "BloombergGPT: A Large Language Model for Finance" | Paper | Advanced | Domain pretraining | Read critically: what a data moat buys and what it doesn't | Recommended |
| Yang, Liu & Wang (2023), FinGPT series | Paper/Repo | Intermediate | Open finance LLMs | Open-source domain adaptation patterns | Optional |
| NIST AI RMF + Generative AI Profile (2023-2024) | Standard | All | Governance | The vocabulary regulators and enterprises use | Recommended |
| EU AI Act text (Reg (EU) 2024/1689), esp. high-risk provisions | Regulation | All | Compliance | Obligations your finance AI may trigger (verify current phase-in) | Essential |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Chip Huyen, *AI Engineering* (O'Reilly, 2025) | Book | Intermediate | Copilot/eval engineering | The best current systems treatment of evals and iteration | Essential |
| Chip Huyen, *Designing Machine Learning Systems* (O'Reilly 2022) | Book | Intermediate | Systems | Context for where GenAI slots into ML platforms | Recommended |
| Islam et al. (2023), "FinanceBench: Open-Source Benchmark for Financial QA" | Paper/Benchmark | Intermediate | Evaluation | Realistic financial QA benchmark; sobering baseline results | Essential |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| DeepLearning.AI short courses (RAG, evals, agents) | Course | Beginner+ | Fast reps | Quick practical reps — you are past beginner, skim selectively | Optional |
| vLLM documentation | Docs | Intermediate | Serving | Private deployment mechanics | Essential |
| Langfuse / Arize Phoenix docs | Docs | Intermediate | Observability | Tracing + eval loops that survive contact with production | Recommended |
| Publicly reported bank copilot deployments (vendor/bank engineering blogs) | Blog | All | Reality check | Claims language, controls described, what isn't said | Recommended |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Community prompt-engineering guides | Blog | Beginner | Patterns | Only as inspiration; your finance prompt library is the asset | Optional |
| LLM leaderboard aggregators | Website | All | Model selection | Directional only; build your own task eval | Reference |

## 10. Practical Exercises

1. - [ ] Write the risk-tier memo for three use cases (analyst summarizer, client chatbot, credit-decision support): data flows, human oversight, regulatory triggers, deployment constraint.
2. - [ ] Build a schema-constrained extractor for 10-K "Key Financial Data" tables (JSON schema + validation + retry); measure field-level precision/recall on 20 filings.
3. - [ ] Implement a citation-forced summarizer on one 10-K: every claim links to page/section; verifier rejects unsupported claims; report violation rate.
4. - [ ] Build the numeric-fidelity checker: re-extract every number in the answer from source tables; log and classify each mismatch (unit, period, restatement).
5. - [ ] Construct a 75-item golden set for earnings-call summarization (rubric-graded); compute bootstrap CIs on your metrics; estimate how many items you actually needed.
6. - [ ] Set up LLM-as-judge for your golden set; compute agreement (κ) vs your human labels; improve the rubric until κ is acceptable; document the ceiling.
7. - [ ] Add Presidio redaction to the prompt path; test against a crafted corpus of edge cases (tickers-as-names, IBANs, partial PII).
8. - [ ] Deploy the same summarizer on a hosted API and on vLLM; compare latency p50/p99, cost per 1k summaries, and throughput under load.
9. - [ ] Implement semantic caching for policy-QA queries; measure hit rate and cost delta on a synthetic query log.
10. - [ ] Red-team your own copilot: 15 adversarial cases (injection in retrieved docs, cross-session probing, numeric baiting); fix the top two failure modes; add regression tests.

## 11. Mini Projects

**M1 — Earnings-call copilot.** Input: call transcript + prior-quarter summary. Output: cited delta analysis (guidance changes, tone shifts, new risks), numeric-fidelity-checked. Difficulty: ★★★☆☆.

**M2 — 10-K comparison copilot.** Two fiscal years of one issuer → side-by-side changes in risk factors and key figures, every line cited. Difficulty: ★★★☆☆.

**M3 — Analyst SQL/pandas copilot with sandbox.** Natural-language question → SQL over a small market DB → executed in a locked-down sandbox → result table + explanation; validation blocks destructive or off-schema SQL. Difficulty: ★★★★☆.

**M4 — Eval harness in CI.** Golden set + judges + numeric checker as a GitHub Action that blocks merges on regression. Difficulty: ★★★☆☆.

## 12. Major Project Hook

This phase is the engine room of **Flagship Project 5 — Financial RAG Research Assistant** (`/projects/flagship/05-financial-rag-research-assistant.md`). Phase 13 adds the retrieval/knowledge-graph depth; here you built the answer-side, evaluation, and safety disciplines.

## 13. Case Studies & Industry Examples

- **Morgan Stanley's internal research assistant** (publicly reported, GPT-4-era): wealth-management knowledge access with human-advised workflow — note the careful "advisers remain responsible" framing.
- **BloombergGPT (2023)**: a domain-pretrained model built on a proprietary corpus — read as evidence about when private data moats justify pretraining (mostly: not for you).
- **Klarna's AI assistant claims (2024)**: widely reported headline metrics (agent-equivalents handled); study how the claim was framed and how practitioners pushed back — a lesson in evaluating vendor/AI claims.
- **Enterprise posture**: most large banks publicly disclose GenAI governance frameworks and restricted-use policies; read one or two and map their controls to Lesson 12.10's layers.

## 14. Interview Questions

**Where does GenAI actually create value in a bank, and where is it dangerous?** Value concentrates in document-heavy expert workflows (research, compliance drafting, service with escalation). Danger concentrates wherever output feeds regulated decisions (credit, suitability) or faces clients unsupervised — high-risk triggers under emerging AI regulation.

**How do you prevent an LLM from misquoting financial figures?** Don't trust generation for numerics: force span-grounded answers, run a post-generation verification pass that re-extracts each figure from the source and diffs, route arithmetic to deterministic tools, and fail closed on mismatch.

**RAG vs fine-tuning for a finance assistant — how do you decide?** Freshness, auditability, and source-attribution needs push to RAG; stable format/style/short-task gaps can justify light fine-tuning; capability gaps are a model-choice problem. Most finance copilots: RAG + strong prompting; fine-tune narrowly and only with an eval that proves the delta.

**How do you evaluate a summarization copilot beyond "looks good"?** Golden set with rubrics, extraction/claim metrics (citation validity, numeric fidelity), LLM-as-judge calibrated against human labels (κ), bootstrap CIs to know your noise floor, and regression gates in CI.

**What compliance controls does a finance copilot need at minimum?** PII/confidentiality redaction, context entitlements (users see only what they may see), audit logging of prompts/responses, output policy checks, human approval on regulated actions, and model/version pinning with change review.

**A retrieved document contains an injection instruction. What happens in your system?** Treat retrieved content as data, never instructions: delimit and escape it, strip or sandbox instruction-like patterns, apply allowlisted tool policies, and log/red-flag attempts — test with an adversarial corpus.

**How do you manage cost for a high-traffic copilot?** Model routing (small models for classification/redaction, large for reasoning), semantic + exact caching with correctness bounds, prompt compression where safe, and unit-economics tracking (cost per resolved task) — not per-token dashboards alone.

**Why is "not financial advice" insufficient?** Because liability and regulation attach to function, not labels: if the system recommendations influence client decisions, suitability/consumer-protection and AI-governance obligations apply regardless of the disclaimer; controls (human approval, scope limits) are the actual mitigations.

## 15. Assessment — Can You Pass the Bar?

- [ ] Deliver a cited, numeric-fidelity-checked copilot on a real 10-K with a violation rate you can quote.
- [ ] Defend a RAG vs fine-tune decision in writing for a specific use case.
- [ ] Run an eval harness with CIs and a calibrated judge; block a deliberately bad change with it.
- [ ] Show a red-team report with fixed failure modes and regression tests.
- [ ] Present a deployment comparison (API vs self-host) with residency, audit, and cost-per-query math.

## 16. Mastery Checkpoint

You may proceed to Phase 13 when:

1. Your copilot repo contains: prompt library, schema-validated extraction, citation-forced answering, numeric checker, golden set + eval harness in CI, and a red-team corpus.
2. You can articulate (in one recorded 5-minute brief) which workflow steps of your copilot must remain deterministic and why.
3. Your cost model shows cost per resolved task under a stated traffic assumption.

Evidence: repo + recorded brief + eval report. Log in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Generating numbers from model memory instead of extraction — the defining finance-LLM bug.
- Golden sets that are too easy (models trained on the internet have seen these filings' summaries).
- LLM-as-judge without human calibration — you automated your blind spots.
- Treating vendor claims of "90% time savings" as eval results; recreate the measurement yourself.
- Prompt/response logs without redaction — a data-incident factory.
- Fine-tuning to fix what is a retrieval or prompt problem.
- Letting the copilot's confidence set your trust: finance failures are quiet (a wrong digit), not loud.

## 18. Where This Goes Next

Phase 13 turns this answer-side discipline into a full knowledge system: retrieval that respects document versions and entitlements, finance knowledge graphs (LEI/ISIN), and retrieval-quality evaluation. Phase 14 then gives these components agency — with approval gates and audit trails you are now equipped to demand.
