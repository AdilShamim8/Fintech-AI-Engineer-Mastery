# Brief 06 — Privacy-Enhancing Technologies (PETs)

> **Maturity: Established theory, early production practice.** Differential privacy and federated learning are mature science; bank deployments concentrate on cross-institution analytics, fraud intelligence sharing, and privacy-safe reporting. Verify current program announcements quarterly.

## Status Map
- **Established:** differential privacy definitions & DP-SGD (Abadi et al. 2016); federated averaging (McMahan et al. 2017); k-anonymity and its documented failures; tokenization/pseudonymization practice.
- **Current practice:** DP aggregate reporting (census-style patterns); FL pilots across banking associations; secure enclaves (TEEs) for data clean rooms; synthetic data for sharing/testing (see Brief 08).
- **Emerging:** cross-silo FL for fraud model improvement without raw-data movement; DP synthetic data with utility guarantees; hardware-backed confidential computing at scale.
- **Frontier:** secure MPC for joint scoring; horizontal FL with graph structures (fraud rings span institutions); formal utility bound certificates for shared models.

## Key Resources
- Dwork & Roth, "The Algorithmic Foundations of Differential Privacy" (free) — the theory.
- Abadi et al., "Deep Learning with Differential Privacy" (2016) — DP-SGD.
- McMahan et al., "Communication-Efficient Learning of Deep Networks from Decentralized Data" (FedAvg, 2017).
- Flower / OpenFL / PySyft documentation — runnable FL frameworks.
- NIST privacy framework materials; IAPP practitioner guidance (Tier 3).
- Regulators' interest documents: BIS/FSB and supervisory analyses of PETs for finance (verify current).

## Open Problems
1. Utility-privacy tradeoff quantification for *tabular financial* data (most published results are vision/language).
2. Fraud intelligence sharing: institutions want the signal, fear the exposure — what architecture actually ships?
3. Non-IID silos: banks' data distributions differ wildly; FedAvg assumptions break.
4. Governance: how do validators assess a model trained they-can't-see-how?
5. Cost: TEE/MPC overhead vs the business value of the joint task.

## Solo Experiments
1. **DP reporting utility curve:** compute DP-noised aggregate default rates (Laplace/Gaussian mechanisms) across epsilon values; plot utility vs privacy on Taiwan/German credit; pick a defensible operating point and defend it.
2. **DP-SGD honesty test:** train a small model with Opacus (DP-SGD) on Home Credit; report accuracy/privacy budget tradeoff vs non-DP baseline.
3. **Federated fraud signal:** simulate 3 institutions with partitioned transaction data (PaySim/IEEE-CIS slices); FedAvg vs local-only vs pooled (upper bound); measure AUC deltas — the honest gap FL must close.
4. **Re-identification demo:** show why k-anonymity fails on credit data (quasi-identifier combinatorics); contrast with a DP summary.

## Curriculum Hooks
Phase 16 (introduction), Phase 19-G (deep), interview track B12 adjacent.

## What Would Change My Mind
A production-scale, published cross-institution fraud model with FL/PETs matching pooled-data performance within a defensible margin — that would move PETs from pilot theater to infrastructure.
