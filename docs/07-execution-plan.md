# 07 — Execution Plan

> Deliverable O. Three pacing tracks, weekly/daily operating rhythm, revision cycles, and portfolio checkpoints. The workflow mechanics live in [`LEARNING.md`](../LEARNING.md); this file is the schedule.

---

## 1. Choose Your Pace

| | **Sprint** | **Standard (default)** | **Deep** |
|---|---|---|---|
| Weekly hours | 20-25 | 12-15 | 8-10 |
| Total | 12 months | 18 months | 24-30 months |
| Stage I | 2 mo | 3 mo | 4 mo |
| Stage II | 3 mo | 4 mo | 6 mo |
| Stage III | 4 mo | 6 mo | 8 mo |
| Stage IV | 1 mo | 1.5 mo | 2 mo |
| Stage V | 4 mo (parallel with III tail) | 6 mo | 8 mo |
| Stage VI | 6 mo (overlapping) | 8+ mo | 10+ mo |
| Capstones | 1 | 1-2 | 2 |
| Fits | Career transition urgency, sabbatical blocks | Working engineer, sustainable | Maximum retention + research depth |

**Rule:** consistency beats intensity. An honest 12h/week for 18 months outperforms a fantasy 25h/week that collapses in month 3. Re-choose pace only at stage gates.

## 2. Standard Plan — Month-by-Month

| Months | Phases | Ship before moving |
|---|---|---|
| 1 | 00, 01 | fintech-lab repo + CI; TVM library; yield-curve study |
| 2 | 02 | payment state machine; pain.001 parser; reconciliation matcher; **Gate I→II** |
| 3-4 | 03, 04 | PIT feature pipeline; market lake; MC pricer; copula study |
| 5 | 05 | purged K-fold leakage demo; DiD study; **Gate II→III** |
| 6-7 | 06 | scorecard + calibrated challenger; reason codes; MDD; risk-committee recording |
| 8 | 07 | cost-sensitive fraud model; threshold economics |
| 9 | 08, 09 | sanctions screener; GNN-on-Elliptic; walk-forward forecast |
| 10 | 10 | VaR engine + Kupiec; honest backtest; **Gate III→IV + IV→V** |
| 11-12 | 11, 12 | extraction pipeline; cited copilot + eval CI |
| 13 | 13, 14 | filings RAG with ACLs; reconciliation agent |
| 14 | 15 | streaming path at p99 target; **Gate V→VI** |
| 15-16 | 16, 17 | SR 11-7 dossier; full MLOps pipeline; audit store |
| 17-18 | 18 | flagship deployed + defended; **Senior-readiness review**; start 19/20 tracks ongoing |

Sprint compresses by pairing (03+04 together; 07+08 overlapping; 19/20 concurrent with 18). Deep extends with L5 research projects and a second capstone.

## 3. Weekly Rhythm (Standard)

- **4 × 2h build blocks** (Mon-Thu evenings): lessons → exercises → mini-project increments. Mechanics: [`LEARNING.md`](../LEARNING.md) §4.
- **1 × 3h deep block** (Sat): flagship work or case study — write case answers *before* reading commentary.
- **1 × 1h review** (Sun): PROGRESS.md update, Anki prune, next week's issues.
- **Daily Anki 15-20 min** (any gap in the day).

## 4. Revision & Retention Cycles

| Cycle | What | How |
|---|---|---|
| Daily | Anki queue | formulas, definitions, thresholds, regulatory anchors |
| Weekly | Recall block | re-explain the week's #1 concept from memory, record if shaky |
| Monthly | Artifact audit | can I still run/fix last month's code in <10 min? |
| Quarterly | Stage-gate oral exam | recorded, hostile-question format |
| Yearly | Baseline re-test + career definition rewrite | [`docs/01-career-definition.md`](01-career-definition.md) is versioned for a reason |

**Decay assumption:** financial-regulatory knowledge decays slowly; GenAI-stack knowledge decays in months. Budget revision time accordingly (more for Stage V, less for Stage I).

## 5. Portfolio & Public Evidence Checkpoints

| Quarter | Public artifact |
|---|---|
| Q1 | Repo live with Stage-I artifacts; first technical note (yield curve or payment lifecycle) |
| Q2 | Credit model repo with MDD; note on calibration vs AUC for lending |
| Q3 | Fraud/AML notebook series; one case-study writeup |
| Q4 | Streaming scoring demo (GIF/benchmarks); MLOps pipeline writeup |
| Q5 | GenAI copilot demo with eval harness; research note #1 |
| Q6 | **Flagship deployed** with README, architecture, demo video; talk or long-form post |

Public evidence converts study into career optionality: recruiters and interviewers weight *shipped, explained systems* over certificates.

## 6. When Life Happens (recovery protocols)

- **Missed <1 week:** resume at the current block; no debt collection.
- **Missed 1-3 weeks:** cut the current phase's Optional resources; keep all Essential + projects; note the slip.
- **Missed >1 month:** re-run baseline; re-enter at the last gate with a 1-week review sprint; consider pace downgrade honestly.
- **Motivation flatline (the real killer):** switch from consumption to building for two weeks — start a flagship thin slice early. Building regenerates pull; watching generates drag.

## 7. Definition of "Done" for the Whole System

You are done — for this pass — when:

1. All six stage gates passed with evidence packs.
2. ≥1 flagship deployed and defended; ≥1 research note published.
3. Interview-track self-grades ≥4/5 across all five tracks.
4. The repo itself is presentable as your professional knowledge base: clean history, working links, honest PROGRESS.md.
5. You have taught one full topic to another human (meetup, colleague, post) — the final mastery verb.

Then you start the *next* pass: Phase 19 electives, L5 research, and maintaining the frontier map (research/) — because a mastery system is a practice, not a finish line.
