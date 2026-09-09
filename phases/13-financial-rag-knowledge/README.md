# Phase 13 — Financial RAG & Knowledge Systems

> **Stage V — Advanced Financial AI** · **Duration: 4 weeks** · **Mastery target: Application → Production**
> **Position in path:** `12-generative-ai-finance` ← **this phase** → `14-agentic-fintech`

## 1. Objective

Retrieval-augmented generation is the default architecture for finance knowledge work — policy Q&A, regulatory research, filings analysis — because finance knowledge is versioned, jurisdictional, entitlement-restricted, and citation-audited. You know RAG basics; this phase is the finance-hardened version: corpus-aware chunking, hybrid retrieval with reranking, metadata (date/version/jurisdiction) as first-class filters, entity-canonicalized GraphRAG-style question answering, full evaluation (retrieval metrics plus faithfulness), ACL-aware design, and the production economics of latency, caching, and cost. You finish able to build knowledge systems a bank's compliance function could approve.

## 2. Why It Matters in Finance

Banks run on institutional knowledge embedded in documents: policies and SOPs that supersede one another, regulatory texts with effective dates, client agreements with entitlement boundaries, research libraries that expire. A RAG system here is not a chatbot — it is a controlled retrieval-and-citation pipeline in which stale or unauthorized content is a compliance incident, and every answer needs an auditable document-and-page citation. Publicly reported deployments (Morgan Stanley's GPT-4-era research assistant) validated the pattern; the failure modes — stale policy answers, injection arriving via retrieved documents — are equally recognized in industry practice.

- Supersession: "the new policy replaces the old" is a correctness requirement — date/version filters cannot be afterthoughts.
- Entitlements: an employee must not retrieve client documents they are not entitled to — ACLs belong inside retrieval, not around it.
- Citations: audit requires claim → document + page; uncited answers are unusable in regulated workflows.
- Numeric content: tables and figures need different retrieval treatment than prose — FinQA-class questions break naive RAG.
- Economics: corpus scale × per-query cost decides embedding choice, reranking depth, and caching design.

## 3. Prerequisites

- [ ] Phase 11 — document parsing, chunking studies, EDGAR/XBRL pipelines
- [ ] Phase 12 — GenAI evaluation discipline, citation-forced answering, guardrails
- [ ] Existing AI engineering: embeddings, vector search basics, LLM APIs (assumed)
- [ ] Phase 03 — data engineering; Phase 05 — experimental discipline for evals
- [ ] Comfort with treating the corpus, not the chatbot, as the product

## 4. Learning Outcomes

- I can design finance-aware chunking (section/heading/table/definition-aware) and justify it with retrieval evals.
- I can select and evaluate embeddings on my own corpus instead of leaderboards (BGE/E5 family included).
- I can build hybrid retrieval (BM25 + dense + reciprocal rank fusion) with cross-encoder reranking and benchmark each stage's lift.
- I can implement metadata filtering with date/version/jurisdiction as first-class constraints, including supersession logic with tests.
- I can build entity-canonicalized retrieval over identifiers (LEI/ISIN/ticker) and GraphRAG-style entity-centric Q&A.
- I can evaluate retrieval (hit@k, MRR, nDCG) and generation (RAGAS-style faithfulness, context precision/recall) on a golden set I authored.
- I can design citation-forced UX where every claim maps to document + page, and audit it end to end.
- I can enforce ACL-aware retrieval with deny-by-default behavior and an entitlement test suite.
- I can budget latency/cost per query and harden retrieval against prompt injection arriving via retrieved content.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 13.1 | Finance knowledge corpora | Filings, policies, regs, research; versioning & entitlements | corpus property map |
| 13.2 | Document processing at scale | PDF/table parsing (unstructured), layout-aware ingestion | ingestion pipeline |
| 13.3 | Finance-aware chunking | Section/heading/table/definition-aware strategies | chunking config + eval |
| 13.4 | Embeddings for finance | BGE/E5 family; evaluate on your corpus | embedding benchmark |
| 13.5 | Hybrid retrieval & fusion | BM25 + dense, reciprocal rank fusion | hybrid retriever |
| 13.6 | Reranking | Cross-encoders, ColBERT concept, latency trade-offs | reranked pipeline |
| 13.7 | Metadata as first-class filters | Date/version/jurisdiction; supersession logic | filtered retrieval layer |
| 13.8 | Query understanding | Rewriting, decomposition, HyDE concept | query pipeline |
| 13.9 | Entities & GraphRAG-style QA | LEI/ISIN/tickers, canonicalization, entity-centric answers | entity QA prototype |
| 13.10 | Tables & numbers in retrieval | Table-aware representation, numeric QA routing | table-retrieval study |
| 13.11 | Evaluation: retrieval + generation | hit@k, MRR, nDCG; RAGAS-style faithfulness | eval harness + scorecard |
| 13.12 | Golden datasets & citation UX | Authoring finance QA goldens; forced citations | 50-question golden set |
| 13.13 | Freshness, supersession & ACLs | Versioned retrieval; entitlement enforcement | ACL test suite |
| 13.14 | Production: cost, latency, guardrails | Caching, budgets, injection defense | production design doc |

**13.1 Finance knowledge corpora.** Financial corpora differ from web text: documents are versioned (superseded policies), scoped (jurisdiction, entity), entitled (client data), and cross-referenced (terms defined once, used everywhere). Map your corpus on these axes before touching a retriever — the map decides chunking, filters, and ACL design. A "policy corpus" and a "research corpus" are different products with different correctness contracts.

**13.2 Document processing at scale.** Real corpora are PDFs with tables, scanned annexes, and legal layouts; parsing libraries (unstructured and similar), layout-aware parsing, and table extraction (Phase 11 tooling) feed the index. Build ingestion with idempotency and regression fixtures: a parser change must not silently corrupt the index. Track parse-quality metrics (tables found, headings detected) as pipeline SLOs, not vibes.

**13.3 Finance-aware chunking.** Chunk along document structure — sections, headings, clause boundaries, table rows — not raw token counts; carry breadcrumbs (document → section → heading) in metadata; keep defined terms ("Aggregate Exposure" means...) attached to their definitions. Your Phase 11 chunking study generalizes, but rerun it on this corpus because chunking is corpus-specific. Definition-aware chunks prevent the classic failure: an answer using a term whose definition lives three sections away.

**13.4 Embeddings for finance.** BGE and E5 families are strong general-purpose starting points; finance-specific checkpoints exist, but the honest rule is to evaluate on your corpus with your golden set — never on leaderboard claims alone. Build the benchmark: hand and synthetic queries → hit@k/MRR per candidate → cost/latency table. Domain fine-tuning is rarely the first win; chunking and hybrid search usually are.

**13.5 Hybrid retrieval & fusion.** BM25 catches exact identifiers (ISINs, defined terms, clause and section numbers) that embeddings blur; dense retrieval catches paraphrase ("can we onboard this client type?"); reciprocal rank fusion combines them without painful weight tuning. In finance corpora, exact-phrase matching is disproportionately valuable — pure-vector systems measurably lose. Benchmark BM25-only vs dense-only vs hybrid on your golden set and keep the ablation table.

**13.6 Reranking.** Retrieve wide (top 50-100), rerank with a cross-encoder (or the ColBERT late-interaction concept) into the top 5-10; this is usually the cheapest large quality win, at a latency cost you must budget. Learn when reranking hurts (queries far from corpus vocabulary) and how to A/B it. Reuse reranker scores for abstention logic — nothing scored above threshold means you say so, honestly.

**13.7 Metadata as first-class filters.** Filter before ranking on document type, effective date, version, jurisdiction, and entitlements — stale regulatory text or superseded policy is a compliance incident, not a ranking nuisance. Implement supersession (only current versions searchable by default; historical versions on explicit request) and test it like a unit test, because it is one. "Retrieve, then hope" is not an architecture here.

**13.8 Query understanding.** User queries are underspecified ("what's our KYC rule for trusts?"); pipelines rewrite (expand abbreviations, resolve defined terms), decompose multi-part questions, and optionally apply HyDE (hypothetical-document embeddings) for recall. Route numeric questions to table-aware paths. Evaluate the query pipeline itself — rewriting can fix recall while quietly breaking precision.

**13.9 Entities & GraphRAG-style QA.** "Who are X's subsidiaries?" is an entity-relation question that chunk retrieval answers badly; canonicalize entities with identifiers (LEI from GLEIF, ISIN, tickers) and build GraphRAG-style flows: entities and relations extracted and indexed → entity-centric retrieval → grounded answer. EDGAR and GLEIF data (your Phase 11 pipelines) supply the entity graph. This lesson is the difference between a document search and a knowledge system.

**13.10 Tables & numbers in retrieval.** Tables need their own representation (row/record chunks, header context, serialized markdown), numeric questions need answer-time computation over retrieved cells, and "closest chunk" often returns the wrong period's table. Build a table-aware study: represent, retrieve, verify. Route FinQA-class questions to the Phase 11 verification machinery rather than trusting the generator.

**13.11 Evaluation: retrieval + generation.** Retrieval metrics (hit@k, MRR, nDCG) on golden queries; generation metrics (RAGAS-style faithfulness, context precision/recall, answer relevance) on golden answers; end-to-end metrics (citation validity rate, numeric fidelity) for the business. Wire it into CI so index, model, or prompt changes cannot silently regress. Evaluation is the system; everything else is an implementation detail.

**13.12 Golden datasets & citation UX.** Author 50-200 questions with expected sources and answers — harvested from real user questions where possible, difficulty-stratified, versioned, and reviewed like code. Design the citation UX: every claim renders with document title plus page/section anchor; a claim without a citation is a defect that fails eval. This golden set is the artifact your compliance reviewers will actually inspect.

**13.13 Freshness, supersession & ACLs.** Ingestion stamps effective dates and triggers supersession; retrieval honors ACLs (user → entitlements → filtered index) with deny-by-default and an entitlement test suite (can user A see document D? should they?). A bank employee must not retrieve client documents they are not entitled to — enforce at query time, log the decision, and design the audit log ("who asked what and saw which document") on day one, not after the first review.

**13.14 Production: cost, latency, guardrails.** Budget p95 latency (embed + retrieve + rerank + generate), cache aggressively (exact and semantic caches; index-level caching), and measure cost per query against corpus growth. Guardrails: PII policy on queries and logs, and prompt-injection defense for content arriving via retrieved documents — an adversarial document is an untrusted input. Ship the design doc: SLOs, failure modes, escalation paths, cost model.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| BM25 scoring | Lexical relevance with term-saturation weighting | Exact identifiers and defined terms in finance text | Your hybrid loses its exact-match leg |
| Cosine similarity & embedding geometry | Dense semantic relevance in vector space | Paraphrastic policy/regulatory questions | You cannot diagnose dense-retrieval failures |
| Reciprocal rank fusion | Rank-based combination of result lists | Tool-agnostic hybrid retrieval | You hand-tune weights you cannot defend |
| hit@k / MRR / nDCG | Ranked-retrieval quality metrics | Golden-set benchmarking and CI gates | "It seems better" is your eval |
| Faithfulness metrics (RAGAS-style) | Claim-to-context support scoring | Audit-grade answer acceptance | Fluent answers the source does not support |
| Abstention threshold calibration | Score → "answer vs say I don't know" | Regulated environments prefer honest ignorance | Confident answers on empty context |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Ingestion idempotency & versioning | Reparsing a corpus must be deterministic; document versions tracked end to end |
| Vector DB choice (pgvector/Qdrant/Weaviate) | Filter support, hybrid features, and ops burden differ; choose by workload, not hype |
| BM25 engine (Elasticsearch/OpenSearch) | The lexical leg of hybrid and the metadata-filter workhorse |
| ACL enforcement point | Entitlement filtering at query time, deny-by-default, decision-logged |
| Eval-in-CI | Golden-set regression gates on every index/model/prompt change |
| Caching layers | Exact + semantic caching decides unit economics at corpus scale |
| Injection defenses | Retrieved content is untrusted input: delimiting, content policy, regression corpus |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| LlamaIndex / Haystack | RAG orchestration: ingestion, retrieval, evaluation abstractions |
| pgvector / Qdrant / Weaviate | Vector storage with metadata filtering |
| Elasticsearch / OpenSearch | BM25 plus filters — the hybrid leg |
| BGE / E5 embedding models | Strong general-purpose dense retrievers to benchmark |
| Cross-encoder rerankers (bge-reranker class) | The cheap, large quality win |
| RAGAS | RAG evaluation: faithfulness, context precision/recall |
| GLEIF API / SEC company facts API | Identifiers (LEI) and entity/filings data |
| unstructured (or equivalent) | PDF/table parsing at ingestion |
| pytest | Supersession and ACL test suites are unit tests |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| LlamaIndex & Haystack official documentation | Docs | Intermediate | RAG engineering | The two mainline frameworks; pick one and go deep | Essential |
| Gao et al. (2023), "Retrieval-Augmented Generation for Large Language Models: A Survey" | Paper | Intermediate | RAG theory | The shared vocabulary (naive vs advanced vs modular RAG) | Essential |
| RAGAS documentation | Docs | Intermediate | Evaluation | The standard faithfulness/context metrics you will implement | Essential |
| pgvector / Qdrant / Weaviate / Elasticsearch documentation | Docs | Intermediate | Infrastructure | The storage and filter substrate you must operate | Essential |
| GLEIF LEI data & API | Data/API | Intermediate | Identifiers | Canonical entity identifiers for finance knowledge graphs | Recommended |
| SEC company facts API (EDGAR) | Data/API | Intermediate | Filings data | Entity-structured filings data for GraphRAG-style work | Recommended |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Microsoft GraphRAG paper & repository (Edge et al., 2024) | Paper/Code | Advanced | Entity-centric RAG | The reference design for graph/entity-augmented QA | Recommended |
| BGE embedding papers (Xiao et al.) | Paper | Advanced | Embeddings | The family you will benchmark; training-objective literacy | Optional |
| ColBERT paper (Khattab & Zaharia, 2020) | Paper | Advanced | Late interaction | The concept behind efficient, strong reranking | Optional |
| BM25 and rank-fusion literature (original papers/summaries) | Paper | Intermediate | Retrieval classics | Know the scoring you deploy | Reference |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Morgan Stanley's publicly reported GPT-4-era research assistant | Case study | Intermediate | Deployment pattern | The canonical public bank-RAG deployment narrative | Recommended |
| Regtech vendor public materials on regulatory change monitoring | Blog/Reports | Intermediate | Freshness | How the industry sells supersession and change tracking | Optional |
| LlamaIndex / Haystack example galleries | Notebooks | Beginner | Quickstarts | Fast reproduction of standard patterns | Recommended |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| DeepLearning.AI RAG short courses | Course | Beginner | Mechanics | Gap-filling only; you outgrow them quickly | Optional |
| Vector-DB vendor benchmarks (public; take with salt) | Blog | Intermediate | Infrastructure | Orientation to trade-offs; re-benchmark on your data | Reference |

## 10. Practical Exercises

1. - [ ] Build a synthetic bank-SOP corpus (20-30 documents with versions and jurisdictions); register it in a repo with a manifest (effective dates, successors, jurisdictions).
2. - [ ] Implement section/heading/table-aware chunking over 5 EDGAR 10-Ks; measure hit@k against naive 512-token chunking on 20 hand-written questions.
3. - [ ] Benchmark BGE vs E5 embeddings on your golden queries; report hit@k/MRR plus cost/latency; write the adopt/reject memo.
4. - [ ] Build the hybrid retriever: BM25 (OpenSearch or in-memory) + dense + reciprocal rank fusion; ablate each leg on the golden set.
5. - [ ] Add cross-encoder reranking; report quality gain vs added p95 latency; set an abstention threshold on reranker scores.
6. - [ ] Implement metadata filters (date/version/jurisdiction) with a supersession rule; write pytest cases — "a query dated after the effective date returns the new policy only".
7. - [ ] Author a 50-question golden set over 10-Ks (question, expected answer, source document + page); version it and review it like code.
8. - [ ] Build the full evaluation harness: retrieval metrics + RAGAS-style faithfulness + citation-validity rate; print the scorecard.
9. - [ ] GraphRAG-style prototype: canonicalize 20 entities with LEI/tickers from GLEIF/EDGAR; answer "who are X's subsidiaries" questions with grounded sources.
10. - [ ] Injection-hardening: plant 10 adversarial instructions in the corpus; build the defense (delimiting, content-policy check) and a regression test that runs on every change.

## 11. Mini Projects

**M1 — 10-K RAG with forced citations and full eval.** 5 filings → finance-aware chunking → hybrid + rerank retrieval → cited answers → retrieval + faithfulness eval on your 50-question golden set. Deliverable: repo + eval scorecard. Difficulty: ★★★☆☆.

**M2 — Policy assistant with ACLs and supersession.** Synthetic SOP corpus → versioned ingestion → entitlement-filtered retrieval → test suite proving ACL + supersession correctness → cited Q&A demo. Deliverable: service + test suite. Difficulty: ★★★★☆.

**M3 — GraphRAG over EDGAR entities.** EDGAR/GLEIF-sourced entity data → canonicalized entity index → entity-centric Q&A ("subsidiaries of X", "who files with Y") with sources. Deliverable: notebook + mini service. Difficulty: ★★★★☆.

**M4 — Hybrid-search benchmark on numeric QA.** BM25 vs dense vs hybrid vs hybrid+rerank on FinQA/TAT-QA questions, with separate table-aware and naive chunking arms. Deliverable: benchmark report. Difficulty: ★★★☆☆.

## 12. Major Project Hook

This phase powers **Flagship Project 5 — Financial RAG Research Assistant** (see `/projects/flagship/`): filings and research corpus → hardened retrieval (filters, ACLs, supersession) → citation-forced generation → full evaluation harness. The extraction and eval cores come from Phase 11; production hardening lands in Phase 17.

## 13. Case Studies & Industry Examples

- **Morgan Stanley's GPT-4-era research assistant**: publicly reported internal deployment over the firm's research library with emphasis on access controls and evaluation — the canonical bank-RAG pattern to study (see `/case-studies/README.md`).
- **Regulatory change monitoring (regtech vendors)**: a public vendor category built on exactly the freshness/supersession mechanics of lesson 13.13 — study their published product anatomy.
- **EU AI Act and DORA documentation pressure (2024-2026 timeline)**: high-risk-system documentation and operational-resilience duties make retrievable, auditable knowledge infrastructure a compliance asset, not a luxury.
- **Court sanctions for fabricated citations (publicly reported legal-research incidents)**: hallucinated case citations have reached courts — the citation-forced UX is your defense in depth.

## 14. Interview Questions

**Design a regulatory-compliance Q&A system for a bank.** Corpus with effective dates and jurisdictions → versioned ingestion with supersession → entitlement-scoped hybrid retrieval with reranking → citation-forced generation with numeric verification → golden-set eval in CI → audit log of question-context-answer → human escalation for ambiguous rulings; stale text is the number-one failure to design against.

**How do you evaluate retrieval quality?** Author a golden query set with known relevant documents and spans; report hit@k, MRR, nDCG per query class; ablate chunking, hybrid legs, and reranking; gate CI on regressions — and hold out a slice of queries from your tuning loop.

**How do you handle document versioning and supersession?** Effective-date and successor metadata at ingestion, default-restrict retrieval to current versions, explicit query-time opt-in to history, and unit tests asserting which version answers which dated question — supersession is a correctness feature, not cleanup.

**How do entitlements work in retrieval?** Map users to entitlements at query time, filter the index (or hybrid engine) before ranking, deny by default, log access decisions, and test with an entitlement suite including "entitled to the org but not to client X"; post-filtering alone is an audit finding waiting to happen.

**Why hybrid search instead of pure vector?** Finance corpora are full of exact tokens — ISINs, clause numbers, defined terms, section headings — where lexical BM25 dominates; dense embeddings cover paraphrase; fusion captures both — and your benchmark should prove the lift on your data.

**How would you build the golden dataset?** Harvest real questions, stratify by difficulty and document type, write expected answers with source citations, dual-review like code, version it, and freeze a regression slice; 50-200 well-chosen items beat thousands of synthetic ones.

**How do you defend against prompt injection arriving via retrieved documents?** Treat retrieved text as untrusted: strict delimiting, instruction-stripping/normalization policy, output-side checks, and an adversarial corpus with regression tests — plus privilege separation so a poisoned document cannot reach other users' context.

**What is GraphRAG and when does finance need it?** Extract entities and relations into an index and answer entity-centric questions ("subsidiaries of X") that chunk retrieval handles poorly; identifiers (LEI/ISIN) and canonicalization are the finance-specific keys.

**Where does RAG cost go, and how do you optimize it?** Ingestion (parsing and embedding at corpus scale) and per-query (embed + retrieve + rerank + generate); optimize with exact/semantic caching, model routing (small rerankers, distilled generators), index pruning, and honest cost-per-query accounting against corpus growth.

**What makes a finance RAG answer "audit-grade"?** Every claim traceable to document + page, generated only from retrieved context (faithfulness measured), versions and entitlements recorded with the answer, and the whole trail retained — a compliance reviewer must be able to replay it.

## 15. Assessment — Can You Pass the Bar?

- [ ] Build a finance-aware chunked, hybrid, reranked retrieval pipeline and show the ablation table.
- [ ] Author and version a 50-question golden set with source citations and review discipline.
- [ ] Wire retrieval + faithfulness metrics into CI with regression gates.
- [ ] Demonstrate supersession and jurisdiction filtering with passing unit tests, including a dated-question scenario.
- [ ] Demonstrate ACL-aware retrieval with deny-by-default behavior and an entitlement test suite.
- [ ] Explain the production design (latency budget, caching, cost per query, injection defenses) to an engineering manager — and the audit trail to a compliance officer.
- [ ] Ship an end-to-end cited Q&A demo over 10-Ks where every numeric answer survives your verification pass.

## 16. Mastery Checkpoint

You may proceed to Phase 14 when:

1. Your RAG eval harness (golden set, retrieval metrics, faithfulness, citation validity) is reusable and CI-wired — the artifact every later project reuses.
2. Your policy-assistant repo proves ACL and supersession correctness with tests, not prose.
3. Your 10-K RAG scorecard shows the ablations (chunking, hybrid, rerank) with a written adopt/reject memo.
4. You can present the compliance-grade architecture (citations, ACLs, audit trail, cost model) in a recorded 10-minute walkthrough — store artifacts under `/notes/artifacts/`.

Evidence: repo links + eval scorecards + recorded walkthrough. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Retrieving superseded policy text because "the vector was close" — a compliance incident with a cosine-similarity root cause.
- Post-filtering ACLs after ranking instead of pre-filtering the index — leaked titles and snippets have already disclosed.
- Trusting leaderboard embeddings and skipping evaluation on your own corpus — finance vocabulary shifts rankings.
- One chunk size for tables and prose; tables need record-level treatment, prose needs section-level context.
- Evaluating only end-to-end answers; without retrieval metrics you cannot localize failures.
- Letting rewritten queries silently change semantics — evaluate the query pipeline, not just the retriever.
- Forgetting that a retrieved document is an untrusted input: prompt injection arrives through your own corpus.

## 18. Where This Goes Next

Phase 14 (Agentic FinTech) turns this knowledge system into an actor: agents that plan multi-step research, call your retrieval and verification tools, and operate under the audit and entitlement constraints you just built. Phases 16 and 17 will formalize the compliance and production engineering that hardens it for the bank.
