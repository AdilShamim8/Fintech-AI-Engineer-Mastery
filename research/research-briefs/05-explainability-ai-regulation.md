# Brief 05 — Explainability & AI Regulation

> **Maturity: Established duties, consolidating rules.** SR 11-7 has governed US bank models since 2011; EU AI Act phase-in and 2025-26 US model-risk guidance updates are reshaping the compliance surface. Regulatory claims in this brief carry "verify current status" flags by design — that's part of the research method.

## Status Map
- **Established:** SR 11-7 model risk management (development/validation/use); ECOA/Reg B adverse-action reasons; SHAP/LIME-class local explanation; counterfactual explanations; fairness metric definitions and their conflicts.
- **Current practice:** model inventories with tiering; independent validation workflows; reason-code pipelines mapped to controlled dictionaries; fairness testing as a recurring release gate; CFPB's position that complex models still owe applicants specific reasons (Circular 2022-03).
- **Emerging:** EU AI Act high-risk obligations phasing in (creditworthiness assessment explicitly listed — verify current timeline); ISO/IEC 42001 AI management systems; GenAI-specific governance profiles (NIST AI RMF GenAI Profile).
- **Frontier:** mechanistic/faithful explanations for deep models; explanation quality metrics; agentic-system accountability frameworks; regulator-supervisory tooling (SupTech) for model inspection.

## Key Resources (Tier 1 heavy — read the primary texts)
- SR 11-7 (Fed/OCC, 2011) + OCC 2011-12; 2025-26 MRM guidance updates (verify status).
- EU AI Act text (Reg (EU) 2024/1689) — Annex III high-risk categories; DORA (2022/2554, applies 2025).
- CFPB Circular 2022-03; Reg B (ECOA) commentary.
- NIST AI Risk Management Framework 1.0 (2023) + Generative AI Profile (2024); ISO/IEC 42001:2023.
- Barocas, Hardt & Narayanan, *Fairness and Machine Learning* (free); Molnar, *Interpretable ML* (free); Mitchell et al., "Model Cards" (2019); Hardt et al., "Equality of Opportunity" (2016).

## Open Problems
1. Faithfulness: do post-hoc explanations (SHAP) describe the model or the explainer? (Fidelity testing is rare in practice.)
2. Fairness metric conflicts: which metric wins under which legal/ethical frame — and who decides?
3. GenAI governance: SR 11-7 was written for statistical models; what is the validation story for prompt-and-retrieval systems?
4. Agentic accountability: who is the "model owner" when behavior emerges from tool use?
5. Cross-jurisdiction collisions: US reason-code doctrine vs EU AI Act duties vs UK/other regimes.

## Solo Experiments
1. **Fidelity audit:** measure SHAP explanation stability across retrains/seeds on a credit model; quantify explanation variance — would it survive a validator's question?
2. **Fairness cost curves:** on Taiwan/German credit, compute accuracy-fairness frontier for 3 mitigation strategies × 2 metrics; write the regulator-facing tradeoff memo.
3. **Reason-code quality:** generate adverse-action reasons via 3 methods (scorecard codes, SHAP-top-k, counterfactual); grade for specificity/stability/actionability with a rubric.
4. **Governance gap analysis:** map one real model (your Flagship 02) against SR 11-7 + EU AI Act high-risk checklists; document every gap and remediation.

## Curriculum Hooks
Phases 06, 16 (main); Phase 20 (governance fluency); interview tracks B11, E7.

## What Would Change My Mind
Evidence that post-hoc explanation methods achieve stable, validator-acceptable fidelity on production-grade financial models at scale — or conversely, a supervisory shift to accepting inherently-interpretable-only policies. Either would reshape Phase 16's doctrine.
