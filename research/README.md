# Research — Field Areas & Frontier Lab

> The research wing of the curriculum: 8 field areas, each with a maturity label, key papers, open problems, and **solo-runnable experiments**. Policy and priorities: [`docs/08-research-agenda.md`](../docs/08-research-agenda.md).
> Standard: research here means verified awareness + one honest experiment + a written verdict. Distinguish **established knowledge / current industry practice / emerging practice / research frontier** in every note you write.

| # | Brief | Maturity (as of 2026) | Feeds |
|---|---|---|---|
| 1 | [LLM agents in financial workflows](research-briefs/01-llm-agents-finance.md) | Emerging → early practice | P14, F06 |
| 2 | [RAG & knowledge systems for finance](research-briefs/02-rag-knowledge-finance.md) | Current practice | P12-13, F05 |
| 3 | [Graph ML for financial crime](research-briefs/03-graph-ml-financial-crime.md) | Current practice (AML), frontier (temporal) | P08, F03 |
| 4 | [Time-series foundation models](research-briefs/04-ts-foundation-models.md) | Emerging practice | P09 |
| 5 | [Explainability & AI regulation](research-briefs/05-explainability-ai-regulation.md) | Established duties, consolidating rules | P06, P16 |
| 6 | [Privacy-enhancing technologies](research-briefs/06-privacy-enhancing-technologies.md) | Established theory, early practice | P16, P19-G |
| 7 | [Deep hedging & RL in execution](research-briefs/07-deep-hedging-rl-execution.md) | Research frontier | P10, P19-A |
| 8 | [Synthetic data for finance](research-briefs/08-synthetic-data-finance.md) | Current practice (sharing/testing), contested (training) | P19-G, fraud |

Optional watchlist (tracked, not briefed): quantum computing in finance · on-chain/DeFi analytics · neuro-symbolic compliance reasoning · multimodal financial AI · agentic simulation environments for risk testing.

## How to Run an Area (3-4 weeks part-time)

1. Read the brief's papers → 1-page synthesis in your own words (with maturity labels).
2. Run the solo experiment; log in `notes/experiments/` with the surprise field filled.
3. Publish a note (blog/repo page): what held, what didn't, what you'd bet on.
4. Update the brief's maturity label if your evidence moves it; link your note.

## Quality Bar

- Reproduce or refute — never just summarize leaderboards.
- Public financial datasets only ([`datasets/`](../datasets/README.md)); synthetic-data conclusions labeled as such.
- Regulatory collisions (novel method vs validation requirements) resolved in production's favor; research goes where governance permits.
- Every brief ends with "what would change my mind" — research without falsification is content marketing.
