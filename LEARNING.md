# LEARNING — The Operating System for Your Study

> How to run this curriculum: the loop, the notes, the weekly cadence, the revision engine.
> Progress tracking lives in [`PROGRESS.md`](PROGRESS.md); the 12/18/24-month schedules live in [`docs/07-execution-plan.md`](docs/07-execution-plan.md).

---

## 1. The Learning Loop (non-negotiable)

For every important topic, run the full 14-step loop. Skipping steps is how people end up with "course completion" instead of capability.

```text
1. Concept              what is it, precisely
2. Why it matters in finance    the money, risk, or regulation at stake
3. First principles      derive/re-derive it; no formula without meaning
4. Mathematics           the formal statement and its assumptions
5. Financial intuition   an analogy that survives a trader's sarcasm
6. Implementation        code it; where honest, build-before-library
7. Real dataset          the canonical datasets in /datasets/
8. Experiment            vary something; measure; be surprised
9. Evaluation            finance-aware metrics (see /tracks/finance-aware-evaluation.md)
10. Failure analysis     make it fail deliberately; understand the failure
11. Production considerations   latency, audit, drift, governance
12. Mini project         an artifact someone could run
13. Assessment           the phase's bar; grade yourself harshly
14. Notes / artifact     teach it in writing; file it in /notes/
```

The objective is never *"I watched a course."* It is: **"I understand it, can implement it, can evaluate it, can explain its financial implications, and can place it in a production system."**

Inspired by build-first curricula: implement the small version yourself before trusting the library (`Build It → Use It → Ship It`).

---

## 2. Notes Architecture

Keep notes inside this repo under `/notes/` — they are part of your knowledge base and your commit history tells your story.

```text
notes/
├── concepts/        one file per concept, in your own words + the math
├── experiments/     one file per experiment: hypothesis, setup, result, surprise
├── case-notes/      your answers to case studies BEFORE reading the commentary
├── decisions/       ADR-style: context, options, decision, consequences
├── artifacts/       recorded explanations, checkpoint presentations, demos
└── reviews/         weekly and quarterly reviews (templates in /notes/templates/)
```

Rules:

- A concept note exists only after you can close the source and write it yourself.
- Every experiment note records **what surprised you** — surprises are the actual curriculum.
- Decision notes use the 4-field ADR format; financial AI is decision-dense, practice here.
- Write notes as if explaining to a smart engineer who lacks the finance context — because that was you eight weeks ago, and it is also your future interviewer.

---

## 3. Weekly Structure (12-15 h default)

| Slot | Time | Activity |
|---|---|---|
| Mon | 2h | New lessons (read + implement). Set the week's 3 targets as repo issues |
| Tue | 2h | New lessons continue; evening: 20-min spaced-repetition review |
| Wed | 2h | Exercise block: datasets + notebooks; commit evidence |
| Thu | 2h | Mini-project build; write experiment notes |
| Fri | 2h | Mini-project finish + failure analysis + production-considerations note |
| Sat | 3h | Deep block: project work or case study (write answers first!) |
| Sun | 1h | Weekly review (template), update PROGRESS.md, plan next week, Anki pruning |

Sprint plan (20h/w): double the daily blocks. Deep plan (8-10h/w): cut Mon/Tue to 1.5h, keep Sat as the anchor.

**Every study cycle produces tangible artifacts.** If a week ends with zero new files in `/notes/` or zero commits, the week did not happen — that is a signal to reduce scope, not to skip evidence.

---

## 4. Daily Workflow (90-minute unit)

1. **Recall (10 min):** yesterday's concept, from memory, out loud or on paper. No peeking.
2. **Learn (45 min):** one lesson step of the loop. Finish with code run and output saved.
3. **Apply (25 min):** exercise or project increment. Commit.
4. **Log (10 min):** note what you built/broke/learned; queue Anki cards; mark the issue.

Two units per weekday ≈ the default plan. Protect them like meetings; reschedule, never delete.

---

## 5. Revision System (spaced repetition that respects finance)

- **Anki (or equivalent), 15-20 min/day.** Card what is compressible: definitions, formulas (duration, WOE, EL = PD×LGD×EAD), thresholds, typologies, regulatory anchors (who issued SR 11-7; what DORA covers; APP reimbursement cap).
- Do **not** card things you must be able to *do* (implementations, system design) — card only the recall substrate that unblocks doing.
- Schedule: 1d → 3d → 7d → 21d → 60d. Prune ruthlessly at weekly review; a deck full of stale cards is procrastination with extra steps.
- **Oral self-exams:** at each stage gate, record a 10-15 minute explanation (bank balance sheet, your credit model, your fraud architecture) and store it in `/notes/artifacts/`. Watching yourself explain is the cheapest simulation of a risk-committee defense.

---

## 6. Project Workflow

1. **PRD first** (1-2 pages, template from the flagship spec): problem, users, decision, costs, constraints.
2. **Data contract:** schema, freshness, leakage review, license check against `/datasets/`.
3. **Thin slice:** end-to-end skeleton (ugly but complete) within week 1.
4. **Depth passes:** features → model → evaluation → production concerns, one per iteration.
5. **Write-up:** README with architecture diagram, honest eval table (including the metrics that got worse), limitations, and "what I would do in production."
6. **Postmortem:** 3 things that went wrong, 2 things you would reuse, 1 thing you now believe differently.

Level definitions and Definition-of-Done per level: [`projects/README.md`](projects/README.md).

---

## 7. Research Workflow (Phase 19+ / research/)

1. Pick a frontier from [`research/README.md`](research/README.md).
2. Read 3-5 key papers (tiered list provided); write a 1-page synthesis **in your own words**.
3. Reproduce one result or run the listed solo experiment.
4. Write a public note (blog/repo discussion): what holds, what is hype, what you measured.
5. File under `/notes/concepts/` and cross-link from the research brief.

The standard: **distinguish established knowledge, current industry practice, emerging practice, and research frontier** — in that vocabulary, explicitly.

---

## 8. GitHub Workflow (solo, but professional)

- One issue per lesson target / project / case study; labels: `phase-06`, `flagship`, `case-study`, `revision`.
- A project board: `Backlog → Reading → Building → Reviewing → Done`.
- Branch per significant artifact (`phase06/scorecard`), PR to yourself, review the diff before merge — reviewing your own work after 48h is embarrassingly effective.
- Tag stage gates: `gate/I-to-II`, `checkpoint/phase06`.
- Monthly: squash chaos, prune dead branches, update PROGRESS.md, write the review.

---

## 9. Anti-Completionism Rules

1. Reading without an artifact within 48h = it did not happen.
2. You may not mark a phase complete with an unchecked Assessment item — renegotiate scope instead.
3. Two consecutive weeks of "no surprises logged" means you are in your comfort zone; jump a difficulty tier.
4. If you are 90% through a resource and 10% engaged, drop it and write the note from memory instead.
5. Optimize for **capability velocity**, not content velocity: "how much can I now do that I could not do last month?"

---

## 10. The Personal Learning System at a Glance

```text
DAILY      recall → learn → apply → log          (90-min units, 1-2/day)
WEEKLY     4 build blocks + 1 deep block + 1 review + daily Anki
MONTHLY    artifact audit, board cleanup, PR of the month review
QUARTERLY  stage-gate check, portfolio review, research note, pacing recalibration
YEARLY     re-run baseline assessment, rewrite career definition from /docs/, plan next year
```

Full schedules: [`docs/07-execution-plan.md`](docs/07-execution-plan.md). Trackers: [`PROGRESS.md`](PROGRESS.md).
