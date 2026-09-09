# CONTRIBUTING

You are the primary maintainer of this repository — contribution here means **maintaining and extending your own mastery system**. These conventions keep it professional enough to show to an employer and consistent enough that future-you can navigate it.

## 1. What "Contributing" Means Here

| Contribution | Where |
|---|---|
| Lesson notes, concept write-ups | `notes/concepts/` |
| Experiment logs and results | `notes/experiments/` |
| Case-study answers (before reading commentary) | `notes/case-notes/` |
| Architecture/decision records | `notes/decisions/` |
| Project code + write-ups | `projects/` (your repos; link from here) |
| Errata and resource reviews | PR against the relevant file |
| New dataset entries | `datasets/README.md` (with leakage/bias notes) |
| New research notes | `research/research-briefs/` or `notes/concepts/` |

## 2. Workflow

1. Open an issue describing the artifact (`[phase-06] scorecard notes`, `[flagship-01] week-3 update`).
2. Branch (`notes/phase06-woe`, `flagship01/p99-optimization`).
3. Commit in small units; evidence goes in the repo, not on your laptop.
4. Open a PR to yourself; self-review after ≥48h where possible.
5. Merge, close issue, update `PROGRESS.md` in the same PR when a checkpoint is passed.

## 3. Style Rules

- Markdown, GitHub-flavored. One idea per file; tables over prose walls; no emojis in core docs.
- Every factual claim about regulation or industry carries a date qualifier ("as of 2024", "verify current status").
- Resource citations: title + author + year minimum; deep URLs only when stable and official.
- Hedge company claims ("publicly reported"). Never invent statistics.
- Datasets referenced must appear in `datasets/README.md`.
- Cross-link phases by relative path (`../06-credit-risk/README.md`).

## 4. Adding a New Phase or Track

Open an issue with: objective, why it matters, prerequisites, 10-14 lesson skeleton, resources by tier, mini projects, assessment bar. Follow the exact 18-section skeleton used by `phases/*/README.md` (see Phase 06 as the template). Rebuild the ROADMAP dependency graph if ordering changes.

## 5. Resource Review Protocol

When you consume a resource, append one line to its table row context (or a review note): what it delivered, what it skipped, whether its priority should change. Resources that under-deliver get demoted — this file should reflect your experience, not marketing.

## 6. Versioning

- `vX.Y` tags at each stage gate.
- Major content revisions bump the minor; curriculum architecture changes bump the major.
- The curriculum is expected to evolve as regulations and industry practice change — that is a feature. Record *what changed and why* in the release notes.
