# Brief 03 — Graph ML for Financial Crime

> **Maturity: Current practice in AML analytics; frontier for temporal/ring detection and explainability.** The Elliptic line of work is the public canon; production systems blend rules, graph features, and supervised learning.

## Status Map
- **Established:** entity resolution as the prerequisite; community detection (Louvain/Leiden), centrality, path analysis as investigator features; rules+ML hybrids in production.
- **Current practice:** graph-feature pipelines feeding GBMs; GNNs as challengers; alert ranking under analyst capacity; scenario backtesting.
- **Emerging:** temporal GNNs (EvolveGCN lineage), heterogeneous graphs (accounts×devices×merchants), explainable GNNs (GNNExplainer) for investigator trust.
- **Frontier:** ring/anomaly detection with few labels; cross-institution graph learning (privacy-colliding with PETs — see Brief 06); LLM-graph interfaces for investigator copilots.

## Key Papers & Resources
- Weber et al., "Anti-Money Laundering in Bitcoin: Experimenting with Graph Convolutional Networks for Financial Forensics" (2019) — the Elliptic paper; also the cautionary tale about temporal evaluation.
- Elliptic++ (2022) — heterogeneous extension.
- Pareja et al., "EvolveGCN" (2020) — temporal graph baseline.
- Ying et al., "GNNExplainer" (2019) — explanation for investigators.
- Hamilton, *Graph Representation Learning* (free book) — the foundations.
- Datasets: Elliptic, Elliptic++, IBM Synthetic AML, SAML-D ([`datasets/`](../../datasets/README.md)).

## Open Problems
1. Label scarcity: AML confirmed labels are tiny and biased by what investigators already looked at.
2. Temporal leakage: most public benchmarks are easy to cheat; temporal splits must be mandatory (Weber et al.'s own lesson).
3. Explainability for examiners: can a GNN's alert be defended in a SAR narrative?
4. Entity resolution quality dominates model quality — how to quantify that dependence?
5. Scale: full-bank graphs (100M+ edges) vs incremental subgraph strategies.

## Solo Experiments
1. **Baseline honesty:** GBM on node features vs GCN/GraphSAGE on Elliptic *with temporal splits* — the canonical comparison most blog posts get wrong.
2. **Community features:** Louvain/Leiden communities + centrality as investigator-facing features; measure alert-precision lift at fixed capacity.
3. **Leakage demo:** random-split vs temporal-split AUC gap on Elliptic — quantify how misleading public numbers are.
4. **Synthetic rings:** inject known ring patterns into IBM-AML data; measure detection recall by pattern type (structuring vs layering vs mule chains).

## Curriculum Hooks
Phase 08 (main), Flagship 03, Phase 19-D.

## What Would Change My Mind
A reproducible result showing GNNs decisively beating feature-engineered GBMs on temporally-split, realistic AML data at matched alert capacity — until then, graphs are a feature source with excellent ROI and models are challengers.
