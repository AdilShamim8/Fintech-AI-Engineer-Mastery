# Brief 08 — Synthetic Data for Finance

> **Maturity: Current practice for sharing/testing; contested for model training.** Synthetic transaction data is now standard for pipeline testing, demos, and cross-team sharing; whether synthetic training data carries real signal (or just the generator's assumptions) remains genuinely contested. Verify current tooling quarterly.

## Status Map
- **Established:** statistical disclosure control; generative basics (GANs since 2014; CTGAN/TVAE for tabular, 2019); differential-privacy connections.
- **Current practice:** synthetic data for dev/test environments, vendor evaluations, red-team corpora, and class-rebalancing experiments; open synthetic financial datasets (PaySim, IBM AML, SAML-D) as infrastructure.
- **Emerging:** diffusion/LDM-class tabular generators; LLM-generated realistic transaction narratives for analyst training; DP-synthetic data with measurable utility bounds; generator-evaluation standards (fidelity vs downstream utility).
- **Frontier:** causal-faithful synthetic data (preserving mechanism, not just marginals); stress-scenario generation for risk; foundation-model-based simulators for agent training (ties to Brief 01).

## Key Papers & Resources
- Xu et al., "Modeling Tabular Data using Conditional GAN" (CTGAN/TVAE, 2019).
- Potluru et al. (JPMorgan), "Synthetic Data Applications in Finance" (2023) — the practitioner survey.
- Dal Pozzolo et al. on imbalance + resampling (2015) — the caution that synthetic fraud samples can distort calibration.
- SDV (Synthetic Data Vault) documentation; Mostly AI / Gretel public materials (Tier 3, tooling landscape).
- PaySim (Lopez-Rojas et al., 2016/2017) and IBM AML dataset papers — how synthetic generators encode (and over-simplify) typologies.

## Open Problems
1. Downstream validity: models trained on synthetic data — when do they transfer, when do they inherit generator blind spots?
2. Evaluation standards: fidelity metrics (marginals, correlations) vs utility metrics (does a model trained on synthetic beat one trained on less real data?) — the field argues about which matters.
3. Privacy: synthetic data that memorizes rare real records (nearest-neighbor leakage) — the DP connection.
4. Adversarial realism: generators that reproduce *adversary adaptation*, not just historical patterns.
5. Governance: can synthetic data be used in regulated validation? Under what documentation?

## Solo Experiments
1. **Train-on-synthetic test-on-real:** train fraud models on CTGAN/SDV-synthetic IEEE-CIS vs real data; quantify the AUC/calibration gap — the headline experiment for this brief.
2. **Generator leakage:** measure nearest-neighbor distances between synthetic and real records; demonstrate memorization risk without DP constraints.
3. **Rebalancing value:** synthetic-minority oversampling vs SMOTE vs class weights on ULB — effects on PR-AUC *and* calibration.
4. **Typology fidelity:** generate synthetic AML patterns with rules + noise; test whether a detector trained on the generator's data catches perturbed (non-identical) typologies — measure brittleness.

## Curriculum Hooks
Phase 19-G, fraud research (Phase 07), PETs (Brief 06), dataset hygiene rules in [`datasets/`](../../datasets/README.md).

## What Would Change My Mind
Rigorous evidence that DP-synthetic pretraining + real fine-tuning consistently outperforms real-only training at matched privacy budgets on downstream financial tasks — until then: synthetic for engineering and sharing, real for learning.
