# Brief 02 — RAG & Knowledge Systems for Finance

> **Maturity: Current practice.** Retrieval is solved-enough that the battleground moved to versioning, entitlements, numeric fidelity, and evaluation. Verify model/generator choices quarterly; retrieval principles age slowly.

## Status Map
- **Established:** hybrid retrieval (BM25+dense+RRF), rerankers, chunking matters more than generator choice for many corpora; RAGAS-style metrics.
- **Current practice:** metadata-first filtering (version, jurisdiction, entitlements), citation-forced answers, golden-set evals in CI, ACL-aware retrieval.
- **Emerging:** GraphRAG over entity graphs (LEI/ISIN canonicalization), table-aware retrieval for financial figures, query decomposition for multi-hop questions ("compare 2022 vs 2023 risk factors").
- **Frontier:** agentic retrieval (self-directed multi-hop with verification), numeric-reasoning-native retrieval, certified provenance.

## Key Papers & Resources
- Lewis et al., "Retrieval-Augmented Generation..." (2020) — origin.
- Gao et al., "RAG for LLMs: A Survey" (2023) — failure-mode map (the most useful section).
- Es et al., "RAGAS" (2023) — eval scaffolding.
- Microsoft GraphRAG (2024) — entity-centric retrieval.
- FinanceBench (Islam et al., 2023) — sobering financial-QA baselines; FinQA/TAT-QA/ConvFinQA — numeric reasoning benchmarks.
- Official docs: LlamaIndex/Haystack, pgvector/OpenSearch, BGE/E5 model cards.

## Open Problems
1. Versioned corpora: how do systems guarantee superseded documents never resurface? (Compliance-critical, under-benchmarked.)
2. Numeric fidelity: retrieval that treats tables as first-class citizens rather than flattened text.
3. Entitlements: ACL-aware retrieval without leaking existence of documents a user cannot see.
4. Evaluation realism: golden sets that reflect expert difficulty rather than internet-easy questions.
5. Cost/latency: reranking quality vs p95 budgets at bank scale.

## Solo Experiments
1. **Hybrid benchmark:** BM25 vs dense vs hybrid vs reranked on 50 FinQA/TAT-QA questions; report hit@k/MRR and cost/query.
2. **Supersession test:** build a tiny policy corpus with v1/v2 docs; adversarially query for deprecated claims; measure leak rate with/without version filters.
3. **Numeric fidelity audit:** run a cited copilot over FinanceBench-style questions; classify every numeric error (unit/period/restatement/extraction).
4. **Chunking study:** fixed-size vs section-aware vs table-aware on a 10-K; retrieval quality per question type (prose vs table).

## Curriculum Hooks
Phases 12-13 (main), Flagship 05, interview track A11.

## What Would Change My Mind
Consistent evidence that long-context generators without retrieval beat well-engineered RAG on citation-validity, numeric fidelity, and cost — on financial corpora, not needle-in-haystack toys.
