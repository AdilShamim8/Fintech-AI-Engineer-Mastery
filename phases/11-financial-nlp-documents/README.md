# Phase 11 — Financial NLP & Document Intelligence

> **Stage V — Advanced Financial AI** · **Duration: 4 weeks** · **Mastery target: Application → Production**
> **Position in path:** `10-quantitative-finance` ← **this phase** → `12-generative-ai-finance`

## 1. Objective

Finance runs on documents: 10-Ks and 10-Qs, earnings-call transcripts, news, research notes, contracts, complaints, and policies — each a different text species with different structure, stakes, and evaluation needs. You already know transformers and NLP pipelines; this phase teaches the finance-specific layer: domain-adapted sentiment (Loughran-McDonald, FinBERT), entity and event extraction, numeric reasoning over tables and text (FinQA/TAT-QA), layout-aware parsing of 100+ page filings, EDGAR/XBRL data engineering, and the evaluation and human-in-the-loop discipline that makes extraction systems trustworthy. You finish able to build document-intelligence systems whose outputs analysts and compliance teams actually use.

## 2. Why It Matters in Finance

The marginal cost of reading finance text is enormous: analysts spend careers extracting numbers and tone from filings, compliance teams triage complaint queues, operations teams key in invoices. Text ML converts that cost into searchable, structured, monitorable data — and it is where generative AI has produced its most credible, publicly reported finance deployments (contract intelligence, research assistants). But finance text punishes generic NLP: "liability" is not negative sentiment, "default" is a term of art, tables carry the numbers, and a confidently wrong extraction is a trading or compliance incident.

- Sentiment lexicons built on movie reviews score financial text backwards; Loughran-McDonald and FinBERT exist because domain adaptation is measurable, not fashionable.
- Numeric reasoning over tables and text (the FinQA/TAT-QA class) is the core analyst task — and the core hallucination surface for LLMs.
- Filings are versioned, cross-referenced, and 100+ pages long; naive chunking destroys the structure that carries meaning.
- CFPB complaint text drives regulated response processes; classification and topic models must be defensible, monitored, and routing-aware.
- PII in customer communications makes de-identification a prerequisite for any pipeline, not an optional feature.

## 3. Prerequisites

- [ ] Phase 03 — data engineering habits (you will build document corpora and pipelines)
- [ ] Phase 10 — enough markets vocabulary to know what filings, earnings calls, and research notes are *for*
- [ ] Existing NLP skill: transformers, fine-tuning, tokenization, classification pipelines (assumed)
- [ ] Existing LLM skill: prompting, structured outputs, basic evals (assumed; deepened in Phase 12)
- [ ] Tolerance for PDF/HTML parsing pain — and the discipline to test parsers like code

## 4. Learning Outcomes

- I can classify finance text species and match each to the right evaluation design (classification vs extraction vs QA).
- I can benchmark FinBERT vs Loughran-McDonald lexicon scoring and defend when to use each.
- I can build NER for tickers, companies, people, amounts, and dates, with entity linking to identifiers.
- I can extract events (M&A, guidance changes) with span-level precision/recall and a human-review sampling plan.
- I can evaluate models on FinQA/TAT-QA numeric reasoning and measure hallucination rates by failure category.
- I can parse PDFs into structured text and tables with layout-aware tooling and choose chunking strategies per document species.
- I can build EDGAR/XBRL pipelines (company facts, full-text search, Financial Statement Data Sets) into clean point-in-time datasets.
- I can run complaint analytics: classification plus BERTopic with product-routing suggestions and drift monitoring.
- I can de-identify customer communications with Presidio and write the residual-risk report.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 11.1 | Finance text species | Filings, calls, news, contracts, complaints, policies + eval needs | species/eval map |
| 11.2 | Financial sentiment done properly | Loughran-McDonald vs FinBERT; domain adaptation | benchmark on Financial PhraseBank |
| 11.3 | Financial NER | Tickers, companies, people, amounts, dates; entity linking | NER pipeline + span-F1 eval |
| 11.4 | Relation & event extraction | M&A, guidance changes; span extraction + schema normalization | event-extraction prototype |
| 11.5 | Numeric reasoning over tables+text | FinQA/TAT-QA/ConvFinQA task design | benchmark eval harness |
| 11.6 | PDF/table extraction | Layout models (LayoutLM/Donut concept), OCR pipelines | extraction baseline + failure list |
| 11.7 | Long documents | Section-aware chunking, hierarchical processing of filings | chunking study |
| 11.8 | EDGAR & XBRL engineering | Company facts API, Financial Statement Data Sets, frames | EDGAR data pipeline |
| 11.9 | Complaint analytics | CFPB database: classification + BERTopic routing | complaint classifier + topics |
| 11.10 | Multilingual finance docs | Cross-lingual sentiment/extraction realities | multilingual eval note |
| 11.11 | Hallucination in numeric QA | Failure taxonomy, verification passes, abstention | hallucination audit |
| 11.12 | Human-in-the-loop extraction QA | Sampling plans, span-F1, annotator agreement, review queues | HITL sampling plan |
| 11.13 | PII & de-identification | Presidio in finance communications; residual risk | redaction pipeline + report |
| 11.14 | Text → model features | Timestamped sentiment aggregation with leakage care; summarization faithfulness | feature pipeline + faithfulness eval |

**11.1 Finance text species.** A 10-K is long, structured, and legally precise; an earnings call is temporal and hedged ("we expect"); news is fast and adversarial; contracts are clause-structured; complaints are short, angry, and routed. Each species needs different parsing, labeling schemas, and — critically — different evaluation: accuracy for routing, span-F1 for extraction, faithfulness for summaries. Write the map once; every later project inherits it.

**11.2 Financial sentiment done properly.** The Loughran-McDonald lexicon re-categorizes finance words ("liability", "cost", "default" are not sentiment-negative in filings), and FinBERT (Araci 2019; Yang/Uy/Huang 2020) adapts BERT to finance corpora — both exist because generic sentiment measurably fails on finance text. Benchmark them on Financial PhraseBank and FiQA-2018 and study the disagreements: that is where the domain knowledge lives. Know when a lexicon (auditable, cheap, stable) beats a model, and when the reverse holds.

**11.3 Financial NER.** Tickers, company names (with aliases and subsidiaries), people, monetary amounts, percentages, and dates form the backbone of finance extraction — and standard NER models miss ticker-vs-company ambiguity and unit-bearing amounts ("$1.2 billion"). Build with spaCy plus rules plus a fine-tuned model, evaluate with span-level F1, and link entities to identifiers (tickers, LEIs — Phase 13 will reuse this canon).

**11.4 Relation & event extraction.** Analysts care about events: acquisitions, guidance changes, management departures, credit events. Extraction means detecting the event, its participants, and its attributes as spans, then normalizing to a schema. Build one prototype (guidance changes from earnings-call sentences) and measure precision/recall against a small hand-labeled set you create yourself — and feel the annotation cost that every extraction project carries.

**11.5 Numeric reasoning over tables+text.** FinQA, TAT-QA, and ConvFinQA ask questions whose answers require reading tables *and* text and doing arithmetic ("what was revenue growth excluding FX?"). Task design (program-of-reasoning vs span vs free-form answers) shapes which architectures can win. Build the evaluation harness first — it is the artifact you will reuse for every LLM you test in Phases 12 and 13.

**11.6 PDF/table extraction.** Filings and invoices are layout problems: multi-column text, merged cells, footnotes, scanned pages. LayoutLM-family models fuse text, layout, and image; Donut is OCR-free document understanding; classic pipelines are OCR plus rules plus table detectors. Build one baseline end to end and quantify where it fails — the failure list, honestly written, is the real deliverable.

**11.7 Long documents.** A 10-K exceeds every model's context window and most analysts' patience. Strategies: section-aware chunking (respect Item 1A vs Item 7 boundaries), hierarchical summarization (section → item → filing), retrieval over structure (Phase 13), and Longformer-style long-context models where full-document attention genuinely helps. Run the chunking study — naive fixed-size vs structure-aware on retrieval and QA quality — because chunking is a first-class modeling decision, not preprocessing trivia.

**11.8 EDGAR & XBRL engineering.** SEC EDGAR provides company facts (XBRL), full-text search, and the Financial Statement Data Sets; XBRL frames give structured, near-point-in-time fundamentals. Build a pipeline that pulls facts for five companies, handles taxonomy and version changes, and emits a clean panel — the boring, decisive work that makes every downstream model possible.

**11.9 Complaint analytics.** The CFPB Consumer Complaint Database labels product but the narrative text is the asset: classify complaint type and routing, then BERTopic (with LDA as a baseline) for themes over time; route by product plus topic with human review queues. Regulated routing means drift monitoring and escalation paths — this is classification with operational consequences, not a Kaggle exercise.

**11.10 Multilingual finance docs.** European banks, global custodians, and regulators work multilingual; multilingual encoders transfer unevenly, and financial terminology resists naive translation ("provision", collateral terms, legal phrasing). Evaluate zero-shot cross-lingual transfer before promising multilingual support, and treat machine-translation preprocessing as a step that changes the error profile, not as a free lunch.

**11.11 Hallucination in numeric QA.** LLMs answer numeric questions with fluent wrongness: invented figures, wrong units (thousands vs millions), wrong period, plausible arithmetic on misread tables. Build a failure taxonomy (wrong cell, wrong period, wrong arithmetic, invented number), a verification pass that re-extracts from source, and abstention rules ("not in the document"). Measure the hallucination rate on FinQA/TAT-QA — the measured number is the deliverable.

**11.12 Human-in-the-loop extraction QA.** No extraction system ships without sampling QA: confidence-stratified sampling, dual annotation on a gold slice, Cohen's kappa for annotator agreement, and span-F1 with per-field breakdowns (dates are easy; amounts with currencies are not). Design the review queue as part of the system — what humans see, how corrections flow back into training data, and when auto-accept thresholds may rise.

**11.13 PII & de-identification.** Customer communications carry names, account numbers, IBANs, national IDs; Presidio detects and masks/pseudonymizes with configurable recognizers. Finance adds complexity: partial-masking rules, pseudonymization with controlled reversible keys, and regulatory retention that fights deletion. Produce a redaction pipeline plus a residual-risk note — "what could still leak, and who decided that was acceptable."

**11.14 Text → model features.** Timestamped sentiment from news and calls becomes model features only with publication-time discipline: timestamp at availability, lag properly, aggregate over windows, and expect leakage audits — Phase 09's rules apply verbatim to text. Summarization of filings and calls needs faithfulness evaluation (claim-to-source checking), not BLEU; a summary that reads well but misstates the number is a defect.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Span-level precision/recall/F1 | Overlap-based extraction metrics | Extraction contracts and QA sampling | You report token accuracy, which lies about fields |
| Cohen's kappa | Chance-corrected annotator agreement | Gold-label trust and HITL design | Your "gold" labels are noise and every metric inherits it |
| Confidence calibration | Does 0.9 mean 90% correct? | Stratified sampling and auto-accept thresholds | Review queues starve or drown |
| Additive log-odds scoring | Lexicon-style evidence accumulation (LM) | Auditable, explainable sentiment baselines | You ship a black box where a transparent tool suffices |
| Imbalance-aware metrics | PR curves/threshold analysis on skewed text | Complaint and adverse-event routing with real costs | Accuracy theater on 95%-negative data |
| Faithfulness/attribution metrics | Claim-to-source support measures | Summarization and cited-QA acceptance | Fluent summaries that misstate the filing |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Corpus building & versioning | Filings get amended (10-K/A); corpora must track versions or evals are irreproducible |
| PDF/HTML parsing pipelines | Filings arrive as HTML (EDGAR), invoices as scans; one tested pipeline per species |
| Point-in-time text features | Publication timestamps (EDGAR acceptance times) must gate feature availability — leakage again |
| Evaluation harnesses in CI | Every model/prompt change re-runs the benchmark; extraction regressions are silent otherwise |
| Review-tool integration | Extraction UIs, correction capture, and retraining loops are the product, not an add-on |
| PII-aware logging | Prompt/response logs are PII mines; redact before storage (Phase 16 deepens) |
| Cost/latency budgets | OCR + LLM pipelines multiply costs; batch vs interactive routing is an architecture decision |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| Hugging Face transformers | Model hosting and fine-tuning; FinBERT checkpoints |
| FinBERT (Araci 2019; Yang et al. 2020) | Domain-adapted financial sentiment encoders |
| spaCy | Industrial NER pipelines with custom entities and rule/model hybrids |
| Presidio | PII detection and de-identification with custom recognizers |
| BERTopic | Topic modeling over complaint and news corpora |
| lxml / BeautifulSoup | EDGAR HTML filing parsing |
| pandas / DuckDB | Panels of extracted facts and features |
| LayoutLM / Donut (HF) | Layout-aware document understanding experiments |
| OCR tooling (e.g., Tesseract) | Scanned-document ingestion with QA |
| EDGAR company facts / full-text search APIs | Filings and XBRL data access |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| SEC EDGAR APIs (company facts, frames, full-text search) | Data/API | Intermediate | Filings/XBRL | The primary source for US filings and structured fundamentals | Essential |
| Loughran-McDonald financial sentiment lexicon | Dataset/Tool | Intermediate | Sentiment | The finance-word lexicon; the baseline you must respect | Essential |
| FinQA / TAT-QA benchmark papers & data | Paper/Dataset | Advanced | Numeric QA | The numeric-reasoning bar for finance QA systems | Essential |
| FinanceBench benchmark | Dataset | Intermediate | Filings QA | Real analyst-style questions over public filings | Essential |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Jurafsky & Martin, *Speech and Language Processing* (free draft) | Book | Intermediate | NLP foundations | The reference behind tokenization, NER, and classification choices | Recommended |
| Hugging Face NLP course | Course | Intermediate | Transformers practice | Hands-on fine-tuning/eval patterns you will reuse daily | Recommended |
| LayoutLM paper (Xu et al., 2020) & model cards | Paper | Advanced | Document AI | Text+layout fusion for forms and filings | Recommended |
| Donut paper (Kim et al., 2022) | Paper | Advanced | OCR-free parsing | Concept grounding for OCR-free document understanding | Optional |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| FinBERT model cards & papers (Araci 2019; Yang/Uy/Huang 2020) | Docs/Paper | Intermediate | Sentiment | Practical adaptation recipes and known limitations | Essential |
| BERTopic documentation | Docs | Intermediate | Topics | Well-maintained topic-modeling tooling with eval notes | Recommended |
| spaCy usage documentation | Docs | Intermediate | NER pipelines | Custom entities, rules, training loops | Recommended |
| Public write-ups on research-intelligence platforms (AlphaSense-style) | Blog/Report | Intermediate | Product anatomy | How commercial finance-text platforms are architected | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Financial PhraseBank (Malo et al., 2014) | Dataset | Beginner | Sentiment | The standard financial-sentiment benchmark slice | Recommended |
| FiQA-2018 (aspect-based financial QA/sentiment) | Dataset | Intermediate | Sentiment/QA | Noisier real-world text for stress-testing | Optional |
| Kaggle NLP notebooks (CFPB-derived, news sentiment) | Notebooks | Beginner | Warm-up | Pipeline practice only, not methodology | Reference |

## 10. Practical Exercises

1. - [ ] Score **Financial PhraseBank** with the Loughran-McDonald lexicon, FinBERT, and a generic sentiment model; report agreement matrices; write 200 words on where generic sentiment fails.
2. - [ ] Build a spaCy NER pipeline for tickers, amounts, and dates over 20 EDGAR press releases; hand-label 50 spans; report span-F1 per entity type.
3. - [ ] Pull XBRL company facts for 5 companies over 10 years; build a revenue/net-income panel; document two taxonomy/version issues you had to resolve.
4. - [ ] Implement section-aware chunking of one 10-K (Item boundaries); compare naive 512-token chunks vs structure-aware chunks on retrieval hit@k for 20 hand-written questions.
5. - [ ] Evaluate a small open LLM on 100 **FinQA** questions; log per-question predicted answer, reasoning program, and hallucination category; compute accuracy and hallucination rate.
6. - [ ] Train a **CFPB** complaint classifier (product routing) with per-class PR-AUC; then BERTopic on narratives; propose routing rules and a human-review sampling plan.
7. - [ ] Build an invoice/receipt field-extraction baseline (Donut or OCR+LLM hybrid) on 20 synthetic invoices; report field-level span-F1 and a failure taxonomy.
8. - [ ] Run Presidio over 50 synthetic customer emails; measure detection F1 against hand labels; write the residual-risk note.
9. - [ ] Build a mini earnings-call tone tracker: use EDGAR full-text search for 5 companies across 8 quarters; plot Loughran-McDonald tone by section; annotate caveats.
10. - [ ] Summarize one 10-K Item 7 with an LLM; implement a claim-checking script that verifies each numeric claim against the filing; report the claim-support rate.

## 11. Mini Projects

**M1 — Sentiment benchmark: lexicon vs transformer.** Financial PhraseBank + FiQA-2018 → LM lexicon vs FinBERT vs generic model → agreement analysis + cost/latency table + recommendation memo. Deliverable: benchmark repo. Difficulty: ★★☆☆☆.

**M2 — 10-K chunking study.** 5 filings → naive vs structure-aware chunking → retrieval + QA quality comparison → written chunking guidance for the flagship. Deliverable: study notebook + guidance doc. Difficulty: ★★★☆☆.

**M3 — CFPB complaint router.** Classification + topics + routing suggestions + drift monitor on a held-out quarter. Deliverable: model + routing spec + monitoring plan. Difficulty: ★★★☆☆.

**M4 — Numeric-QA hallucination audit.** Small LLM on a FinQA/TAT-QA subset → taxonomy-tagged error log → verification pass that cuts the hallucination rate → before/after report. Deliverable: audit report + verifier code. Difficulty: ★★★★☆.

## 12. Major Project Hook

This phase powers **Flagship Project 4 — Financial Document Intelligence Platform** (see `/projects/flagship/`): ingestion of filings, contracts, and complaints → extraction with HITL QA → numeric QA with verification → evaluation harness. Build the extraction and evaluation cores here; productionize in Phase 17.

## 13. Case Studies & Industry Examples

- **JPMorgan COIN**: publicly reported contract-intelligence program that automated commercial-loan document review — the emblematic "documents → structured data" deployment; reported claims centered on review-hours reduction, so read the reporting carefully (see `/case-studies/README.md`).
- **Research-intelligence platforms (AlphaSense-style)**: publicly marketed products built on filings/call/transcript search with entity linking and monitoring — their anatomy is exactly lessons 11.1-11.8.
- **CFPB complaint-driven supervision**: complaint narratives feed supervisory prioritization through a publicly documented process — your complaint analytics is a regulated workflow in miniature.
- **EDGAR full-text search (2001-forward coverage, publicly documented API)**: transformed filings text into a queryable asset — the data-engineering substrate of this entire phase.

## 14. Interview Questions

**Design a loan-document understanding system end to end.** Species analysis → schema (fields, spans, tables) → parsing (layout-aware) → extraction (fine-tuned model or LLM with constrained outputs) → verification pass → confidence-stratified human review → span-F1 eval in CI → retraining loop from corrections; the eval and review loop differentiate the system, not the model.

**How do you evaluate extraction quality?** Span-level precision/recall/F1 per field, on a gold set built with dual annotation and kappa; stratify by confidence and document type; add end-to-end business metrics such as straight-through-processing rate — token-level accuracy hides field-level truth.

**Why does generic sentiment analysis fail on finance text?** Financial words are domain terms ("liability", "default", "charges" are often neutral), negation and hedging dominate ("we do not expect material impact"), and the target construct differs (tone vs direction vs surprise) — hence Loughran-McDonald and FinBERT exist; measure, do not assume.

**How do you stop numeric hallucination in document QA?** Force extraction-then-answer with quoted spans, forbid arithmetic in the generator (route to a calculator), verify every number against the source table, and abstain when the span is missing; then measure the hallucination rate on a benchmark — verification is a pipeline component, not a hope.

**How do you chunk 100+ page filings for retrieval?** Respect structure (Item/section boundaries), keep tables whole or table-aware, carry heading breadcrumbs in metadata, and tune chunk size on retrieval evals — chunking is a modeling decision with measurable consequences.

**When would you use a lexicon over a fine-tuned model?** When auditability, latency, cost, or tiny data dominate — lexicons are transparent and stable; models win on context, negation, and nuance; many production systems run both and reconcile disagreements.

**What is XBRL and why should an AI engineer care?** The structured-tagging standard for financial statements; company facts APIs turn filings into near-point-in-time fundamentals without OCR — the cleanest fundamentals data path, with taxonomy/version gotchas to engineer around.

**How do you build the gold set for extraction evals?** Sample documents by species and difficulty, dual-annotate with adjudication, measure kappa, version the gold set, and freeze a regression slice; without annotation discipline your "improvements" are measurement noise.

**How do you handle PII in customer-communication analytics?** Detect with configurable recognizers (Presidio), choose masking vs pseudonymization per downstream need, log redaction decisions, and quantify residual risk; retention rules and access control do the rest (Phase 16).

**What makes earnings-call analysis different from news sentiment?** Calls are scheduled, hedged, and management-controlled (forward-looking language; prepared remarks vs Q&A), while news is adversarial and fast; aggregation windows and speaker attribution differ, and leakage rules are stricter for calls.

**Where do layout models beat OCR+LLM pipelines?** On forms, tables, and scanned documents where spatial structure carries meaning (LayoutLM/Donut strengths); on clean digital text, parsing plus a strong LLM is often simpler — benchmark both on your own corpus.

**How would you monitor a deployed complaint classifier?** Track class-distribution drift, confidence calibration, per-class PR-AUC on delayed labels, topic-model drift (new themes), and escalation-rate anomalies; routing changes trigger review, never silent adoption.

## 15. Assessment — Can You Pass the Bar?

- [ ] Build and defend a sentiment benchmark with lexicon, transformer, and generic baselines on finance data.
- [ ] Produce span-F1 per field for an extraction system with a kappa-checked gold set.
- [ ] Run FinQA/TAT-QA evals on a small LLM and report a hallucination-rate reduction from your verification pass.
- [ ] Implement section-aware chunking and show its measurable retrieval advantage over naive chunking.
- [ ] Build an EDGAR/XBRL pipeline that handles at least two taxonomy/version quirks correctly.
- [ ] Explain to a compliance officer how complaint routing, drift monitoring, and human review interlock.
- [ ] Ship a PII-redaction pipeline with a residual-risk note a privacy reviewer could accept.

## 16. Mastery Checkpoint

You may proceed to Phase 12 when:

1. Your evaluation harness (sentiment, extraction span-F1, numeric QA with hallucination categories) exists as reusable, CI-wired code.
2. Your EDGAR/XBRL pipeline produces a clean, versioned panel for 5 companies — the substrate Flagship 4 and Phase 13 reuse.
3. Your chunking study and extraction failure taxonomy are written up and stored under `/notes/artifacts/`.
4. You can present a document-intelligence system design (extraction + HITL + eval) in a recorded 10-minute walkthrough.

Evidence: repo links + benchmark reports + design walkthrough. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Evaluating extraction with token-level accuracy — fields, not tokens, are the unit of business truth.
- Chunking filings by fixed token count and destroying Item boundaries, tables, and cross-references.
- Mishandling EDGAR amendments: a 10-K/A replaces the original; both "latest wins" and "keep both" can corrupt evals.
- Summarizing numbers from model memory instead of extraction — fluent, wrong, and expensive.
- Ignoring publication timestamps and turning news sentiment into a leakage machine (Phase 09 rules apply to text).
- Sending raw PII through third-party LLM APIs before redaction — a compliance incident, not an optimization.
- Hand-labeling 30 spans and reporting three significant figures on span-F1.

## 18. Where This Goes Next

Phase 12 builds generative copilots on top of these extraction and evaluation disciplines — citation-forced answers, numeric verification, and golden-set evals all transfer directly. Phase 13 then scales the corpus side into full retrieval systems over the filings pipelines you built here.
