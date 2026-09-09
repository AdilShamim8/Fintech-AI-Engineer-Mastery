# Phase 00 — Orientation & Baseline

> **Stage I — Domain Bridge** · **Duration: 1 week** · **Mastery target: Baseline → Operating rhythm**
> **Position in path:** curriculum start ← **this phase** → `01-financial-foundations`

## 1. Objective

You already know how to build ML systems; this week installs the operating system for learning finance. Phase 00 converts generic AI-engineering skill into a directed FinTech apprenticeship: you will score yourself against a finance baseline, stand up a permanent `fintech-lab` repository with CI and a notes architecture, learn to read a 10-K, and commit to a 12/18/24-month pacing plan. Every artifact you produce for the rest of the curriculum — models, pipelines, case notes, decision records — will live inside the structures you build here. Skip this phase and six months from now you will have read a lot and produced nothing you can re-run, show, or defend.

## 2. Why It Matters in Finance

Finance is the most audit-heavy industry that hires AI engineers: models, features, and decisions are expected to be reproducible months or years later, by people who do not yet trust you. Engineers who struggle in finance rarely fail on modeling; they fail on provenance, on misread domain data, and on process. This phase hard-codes the process so it never becomes the bottleneck.

- Financial work products are evidence. A model without a pinned dataset, a locked environment, and a decision record is not a deliverable; it is a liability (governance formalizes this in Phase 16).
- Domain literacy compounds. One hour spent learning to read a 10-K or a central-bank statement pays out across all twenty phases; there is no later substitute.
- The learning loop (problem → concept → build it → use it → ship it) mirrors how finance teams actually absorb engineers: through shipped, reviewable artifacts, not certificates.
- Process failures are catastrophic in money systems. The habits formed here — CI, immutable data snapshots, idempotent scripts — are the same habits that keep production systems safe; Knight Capital (publicly reported) is the standing cautionary tale.

## 3. Prerequisites

- [ ] Existing engineering skill: Python, virtual environments, pytest, Jupyter, git basics (assumed known — this curriculum never teaches generic tooling from zero)
- [ ] A machine where you can install Docker, or a cloud sandbox you control
- [ ] A personal GitHub account you are willing to treat as your portfolio
- [ ] A realistic weekly time budget (5-8+ hours) you can hold for 12+ months
- [ ] No finance knowledge assumed — that is exactly what the baseline quiz measures

## 4. Learning Outcomes

- I can explain this curriculum's learning loop, evidence-artifact convention, and PROGRESS.md protocol in my own words.
- I can score myself on a finance-knowledge baseline quiz and map every weak area to a specific phase.
- I can maintain a notes tree (concepts / experiments / decisions / case-notes) with atomic, linkable notes.
- I can run a solo GitHub workflow: issue → branch → PR to self → self-review → merge, with a project board tracking phases.
- I can create a locked, reproducible Python environment (uv or conda) and a Docker image that runs my tests.
- I can query CSV/Parquet files with DuckDB and articulate when it beats pandas or a warehouse.
- I can name the major sections of a 10-K and locate MD&A, risk factors, and the financial statements in a real filing.
- I can state which pacing plan I committed to (12/18/24 months) and what I will cut first if I fall behind.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 00.1 | What changes when the domain is money | Skill-transfer map: what carries over, what does not | transfer-matrix note |
| 00.2 | How this repo works | Learning loop, evidence artifacts, PROGRESS.md | walkthrough concept note |
| 00.3 | Baseline self-assessment | Finance quiz, weak-area routing | scored quiz + gap map |
| 00.4 | Notes architecture | concepts/experiments/decisions/case-notes | populated notes tree |
| 00.5 | Solo GitHub workflow | Issues as tasks, project board, PRs to self | project board + 3 merged PRs |
| 00.6 | Python environments with uv | Lockfiles, reproducibility, tool-choice ADR | locked env + ADR-001 |
| 00.7 | Docker & cloud sandbox | Containerized tests, free-tier cloud discipline | Dockerfile + sandbox note |
| 00.8 | DuckDB & the local analytics stack | SQL over files, Parquet, tool-choice heuristics | DuckDB smoke-test in CI |
| 00.9 | Reading a financial document | 10-K anatomy: business, risk factors, MD&A, statements, notes | annotated 10-K case note |
| 00.10 | Artifact conventions | Naming, units, currency, snapshots, checksums | conventions doc in repo |
| 00.11 | Pacing & goal routing | 12/18/24-month plans, stage budgets, cut rules | pacing plan in PROGRESS.md |
| 00.12 | Building fintech-lab | Repo scaffold, CI (ruff + pytest), templates | green-CI repository |

**00.1 What changes when the domain is money.** Your ML and MLOps skills transfer almost fully; what changes is the substrate and the stakes. Finance data is adversarial (fraud), regulated (credit, AML), time-obsessed (everything is as-of a date), and denominated — where a wrong sign, unit, or currency is a P&L event, not a metric glitch. Write a one-page transfer matrix: rows are your current skills, columns are the phases where they land, and mark the three places you expect finance to surprise you; revisit it after Phase 06.

**00.2 How this repo works.** The curriculum runs a six-beat loop per lesson — motto, problem, concept, build it, use it, ship it — and every phase ends in evidence artifacts: tested code, plots, notes, and a PROGRESS.md checkpoint. Evidence beats enthusiasm: a green CI badge and a linked decision record are what "done" means here. Read LEARNING.md and ROADMAP.md at the repo root now, then write a 200-word summary in your own words; if your summary disagrees with the files, fix the files.

**00.3 Baseline self-assessment.** Take the finance-knowledge quiz (template in `/notes/templates/`) covering money, banking, rates, instruments, markets, and regulation — then score it honestly, because the goal is a map, not a grade. Route every weak area to a phase: "no idea what a CCP does" routes to Phase 01, "never heard of interchange" to Phase 02, "point-in-time what?" to Phase 03. Store the gap map where you will see it weekly; you will re-take the quiz after Phase 06 and again mid-curve to measure drift.

**00.4 Notes architecture.** Four note kinds: concepts (durable explanations in your own words), experiments (what you ran, what happened, what you conclude), decisions (ADRs: options, choice, rationale, reversal condition), and case-notes (real incidents and what they teach). Notes must be atomic, searchable by title, and cross-linked; in a regulated domain the decision log alone justifies the overhead, because it is precisely what validators and auditors request. Copy the templates from `/notes/templates/` rather than inventing formats.

**00.5 Solo GitHub workflow.** Treat yourself as a team of one with professional process: work starts as an issue, lives on a project board (Backlog / In progress / Review / Done), lands through a pull request you review yourself, and closes with a one-line rationale. The PR-to-self habit feels silly for a week, then becomes how you think — it forces commit-level history, reviewable diffs, and honest "why did I do this" notes. It is also the exact history a future employer reads.

**00.6 Python environments with uv.** uv gives project-scoped, lockfile-pinned environments that install in seconds; conda remains acceptable where you need non-Python binaries. The requirement is not the tool but the guarantee: anyone — including you-in-six-months — can recreate the exact environment from the repo. Write ADR-001 recording the choice, the alternatives rejected, and the condition under which you would revisit.

**00.7 Docker & cloud sandbox.** A Dockerfile that runs `ruff check` and `pytest` makes CI identical to your laptop and prepares the multi-container stacks of Phase 03 (Postgres + Kafka + Debezium) and Phase 17. A free-tier cloud sandbox (small VM or managed notebook) is enough for later phases needing a public URL or heavier compute. Keep cloud spend at zero by default and document every resource you create so you can destroy it.

**00.8 DuckDB & the local analytics stack.** DuckDB queries CSV and Parquet in place with real SQL — window functions, as-of joins, grouping sets — at speeds that embarrass pandas on analytics workloads, with no server to babysit. It becomes the default for market data (Phase 03), credit panels (Phase 06), and every "quick look" that used to be a notebook full of pandas chains. Build a smoke test — load a CSV, run an aggregate, assert a row count — which becomes the template for testing data code everywhere.

**00.9 Reading a financial document.** A 10-K has a stable anatomy: Business (what the company does), Risk Factors (what management fears, legally hedged), MD&A (management's narrative on the numbers), Financial Statements (the auditable core), and Notes (where the detail hides — accounting policies, contingencies, segments). AI engineers read 10-Ks because lending, document AI, and RAG systems (Phases 06, 11, 13) all operate on these documents and the facts extracted from them. Download one real filing from SEC EDGAR and annotate the five sections; notice how much of the substance lives in the Notes, not the headline tables.

**00.10 Artifact conventions.** Decide now, never renegotiate: ISO 8601 dates; explicit currency codes and unit scales on every monetary number; raw inputs immutable and checksummed; every figure produced by a script, not a screenshot; naming as `YYYYMMDD-short-slug`. Conventions sound bureaucratic until the day a regulator, a teammate, or your own future self asks "which version of the data produced this number?"

**00.11 Pacing & goal routing.** Choose 12, 18, or 24 months based on weekly hours and stage priorities: 12 months compresses Stage V breadth and shrinks flagship depth; 24 months admits the research frontier (Phase 19) properly. Write the plan into `/PROGRESS.md` with target dates per stage and an explicit cut rule — what drops first when life happens (typically Tier 4 resources and second mini projects) and what never drops (evidence artifacts, checkpoint recordings).

**00.12 Building fintech-lab.** The phase deliverable: a repository with `src/`, `tests/`, `notes/`, `data/` (git-ignored, checksummed), `notebooks/`, CI running ruff + pytest on every push, and the note templates copied in. Everything from Phase 01 onward — the TVM kernel, the yield-curve monitor, the ledger simulator, the lakehouse — is built inside this one repo, which by Phase 17 resembles a production financial-ML monorepo.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Percentages & basis points | 1bp = 0.01%; the native unit of rates and fees | Fees, spreads, and rate moves are quoted in bps everywhere | You misread every fee schedule and headline |
| Compound growth | a·(1+r)^n intuition | Interest, inflation, and AUM growth all compound | You underestimate long-horizon effects by orders of magnitude |
| Order-of-magnitude estimation | Fermi-style sanity arithmetic | Checking that pipeline totals balance before modeling them | You ship "insights" that are unit errors |
| Expected value | Σ p·x | Every financial decision is an expected value with risk adjustments | You reason in averages where tails dominate |
| Logarithms & log scales | Log transforms for ratios and growth | Return math, price charts, heavy-tailed data | You misread the log-scale charts finance lives in |
| Units & dimensional analysis | Currency codes, scales, per-period vs annualized | Financial data mixes currencies, thousands/billions, daily/monthly | Silent factor-of-12 or factor-of-1000 errors |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| CI as definition of done | In finance, "works on my machine" is a compliance finding; green CI is the minimum bar for every artifact |
| Reproducible environments (lockfiles) | Validators and auditors re-run your work months later; pin everything |
| Containers | Phase 03+ needs multi-service stacks (Postgres, Kafka); Docker is the substrate |
| Notebook hygiene | Saved outputs, fixed seeds, provenance headers — notebooks are reasoning documents that must stay auditable |
| Data snapshotting & checksums | Raw inputs are immutable evidence; all transformation happens in code |
| Secrets hygiene | API keys (FRED, exchanges, cloud) never enter git; `.env` plus secret managers from day one |
| Semantic naming & versioning | Artifacts must be findable and chronologically ordered without being opened |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| uv (or conda) | Fast, locked, reproducible Python environments |
| Docker | Identical dev/CI environments; substrate for later multi-service stacks |
| DuckDB | In-place SQL over CSV/Parquet; the local analytics engine |
| GitHub Actions | Runs ruff + pytest on every push — your CI |
| ruff + black | Lint and format; one config, zero debates |
| pytest | The test framework every phase's artifacts must pass |
| Jupyter | Reasoning notebook; not a production surface |
| pandas / matplotlib | Baseline wrangling and plotting until Polars/DuckDB take over (Phase 03) |
| requests | Minimal API client for FRED, EDGAR, and sandbox experiments |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| LEARNING.md (this repository) | Guide | All | Learning system | Defines the loop, evidence artifacts, and progress protocol everything else runs on | Essential |
| ROADMAP.md (this repository) | Guide | All | Curriculum map | Phase dependencies, stage structure, goal routing | Essential |
| Git documentation (git-scm.com) | Docs | Intermediate | Version control | The reference for the workflow you will execute daily | Essential |
| uv documentation (docs.astral.sh/uv) | Docs | Intermediate | Environments | Locked, reproducible Python environments in seconds | Essential |
| Docker documentation (docs.docker.com) | Docs | Intermediate | Containers | Reproducible test and pipeline environments; substrate for Phase 03+ stacks | Essential |
| DuckDB documentation (duckdb.org) | Docs | Intermediate | Local analytics | In-place SQL over files; this curriculum's default analytics engine | Essential |
| SEC, "Beginner's Guide to Financial Statements" (sec.gov) | Guide | Beginner | Financial documents | The regulator's own walkthrough of the statements inside a 10-K | Recommended |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Chacon & Straub, *Pro Git* (free online, git-scm.com/book) | Book | Intermediate | Git mastery | Branch, rebase, and review discipline beyond cheat-sheet level | Recommended |
| pytest documentation (docs.pytest.org) | Docs | Intermediate | Testing | Tests are the evidence contract every later phase depends on | Essential |
| ruff documentation (docs.astral.sh/ruff) | Docs | Beginner | Lint/format | One fast tool for lint plus format in CI | Recommended |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| GitHub Actions documentation (docs.github.com) | Docs | Intermediate | CI/CD | Where your lint-and-test workflow actually runs | Essential |
| GitHub Skills (skills.github.com) | Course | Beginner | GitHub workflow | Hands-on reps for issues, PRs, and projects if git is rusty | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Khan Academy, "Finance and capital markets" | Course | Beginner | Finance basics | Warm-up videos if finance vocabulary is entirely new before Phase 01 | Optional |
| Investopedia | Reference | Beginner | Terminology | Fast lookups; always verify against Tier 1 sources | Reference |

## 10. Practical Exercises

1. - [ ] Take the finance baseline quiz from `/notes/templates/`; score it; write the gap map routing each weak area to a phase; store in `/notes/artifacts/`.
2. - [ ] Create the `fintech-lab` repository with the scaffold (src/tests/notes/data/notebooks) and a README stating its purpose and conventions.
3. - [ ] Add GitHub Actions CI: ruff check + pytest on push and PR; put the badge in the README.
4. - [ ] Write `uv.lock` (or a conda environment export) and a Dockerfile; make CI run the test suite inside the container.
5. - [ ] DuckDB warm-up: download a FRED CSV (e.g., CPIAUCSL), run a window-function query over it, and assert the results in a test.
6. - [ ] Fetch one real 10-K from SEC EDGAR; annotate Business / Risk Factors / MD&A / Statements / Notes in a case note; quote one Notes disclosure that surprised you.
7. - [ ] Set up the project board with Backlog / In progress / Review / Done; open issues for the Phase 01-03 mini projects.
8. - [ ] Write ADR-001 (environment tooling) and ADR-002 (DuckDB as local warehouse) using the decision template.
9. - [ ] Commit your 12/18/24-month pacing plan to `/PROGRESS.md` with per-stage target dates and cut rules.
10. - [ ] File the transfer matrix from lesson 00.1 under `/notes/experiments/` as experiment 000, with three predicted surprises.

## 11. Mini Projects

**M1 — The fintech-lab repository.** Data: none (scaffold + one FRED CSV smoke test). Task: repo with CI (ruff + pytest), note templates from `/notes/templates/`, conventions doc, and two ADRs. Deliverable: the green-badge repository you will use for the next 12+ months. Difficulty: ★☆☆☆☆.

**M2 — Self-verifying environment.** Data: one FRED series. Task: Docker image that runs lint, tests, and a DuckDB smoke query; CI builds and runs it on every push. Deliverable: one-command proof that a clean checkout produces a verified analysis. Difficulty: ★★☆☆☆.

## 12. Major Project Hook

None — this phase IS the setup; deliver the fintech-lab repo. Every later flagship build (`/projects/flagship/`, first touched around Phase 06) inherits its scaffolding, CI, and conventions.

## 13. Case Studies & Industry Examples

- **Knight Capital (2012)**: publicly reported loss of roughly $440M in under an hour from a deployment and process failure — the standing argument for CI, release discipline, and kill switches; revisited in Phase 17.
- **Reinhart-Rogoff (2013)**: publicly reported spreadsheet and formatting errors in an influential public-debt study — the standing argument for scripted, version-controlled, reviewable analysis over ad-hoc files.
- **Your baseline quiz score**: the least famous and most useful case study — record it today; the delta after Phase 06 is your evidence that the loop works.

## 14. Interview Questions

**Why does an AI engineer in finance need to understand double-entry bookkeeping?** Ledgers are the data model beneath nearly every financial dataset; knowing that every transaction has two sides and that sums must reconcile is what lets you detect broken pipelines, design correct features, and earn trust from finance teams.

**What does a central bank actually do?** It sets short-term policy rates, supplies or absorbs reserves and liquidity, acts as lender of last resort, and supervises parts of the system — and its decisions reprice everything from mortgages to the discount rates inside your models.

**What is the difference between a stock and a bond?** A bond is a contractual claim (coupons plus principal, default risk, seniority); a stock is a residual claim (variable cash flows, unlimited downside participation in losses, upside participation). Volatility, models, and data frequency differ accordingly.

**What is a 10-K and why would an ML engineer read one?** The audited annual report: business description, risk factors, MD&A, financial statements, and notes. Document-AI, credit, and sentiment work consume 10-K content, and the notes are where accounting reality hides.

**Walk me through your personal workflow from idea to shipped artifact.** Issue → branch → small PRs → CI (lint + tests) → merge → notes/decision record → PROGRESS.md update — the same reviewability regulated teams expect.

**How do you make a data analysis reproducible six months from now?** Pinned environment, checksummed raw snapshots, scripted transformations, seeded randomness, and a decision record linking data version to figure version.

**When would you reach for DuckDB instead of pandas or a warehouse?** Local, file-based analytics over larger-than-memory data with real SQL — the tier between pandas (small, imperative) and cloud warehouses (shared, governed); it is the default for prototyping financial pipelines.

**What is a basis point and why do finance people use it?** 0.01%; a stable unit that removes the "one percent of what" ambiguity when rates and fees move by small amounts.

**Why do financial institutions care so much about audit trails for ML?** Because models drive regulated decisions (credit, AML, trading); reconstructing who ran what, on which data, producing which decision is a supervisory expectation (SR 11-7, BCBS 239), not a nice-to-have.

**How will you know this self-directed curriculum is working?** Evidence accrues: green CI, artifact counts per phase, checkpoint recordings, re-taken baseline scores, and PROGRESS.md history — process metrics you defined before starting, not vibes.

## 15. Assessment — Can You Pass the Bar?

- [ ] `fintech-lab` exists with green CI (ruff + pytest) on every push, badge visible.
- [ ] CI runs the test suite inside a Docker build (M2 complete).
- [ ] Notes tree contains at least one concept note, one experiment note, one decision record, and one case note, all from templates.
- [ ] Baseline quiz scored; gap map routes at least ten weak areas to specific phases.
- [ ] Pacing plan (12/18/24 months) committed to `/PROGRESS.md` with dates and cut rules.
- [ ] One real 10-K annotated: you can name what lives in each of the five sections without opening it.
- [ ] Explain the repo's learning loop and evidence conventions to a peer in ten minutes (record it; store in `/notes/artifacts/`).

## 16. Mastery Checkpoint

You may proceed to Phase 01 when:

1. The fintech-lab repo is live with green CI, and the M2 containerized pipeline passes end to end.
2. The notes tree is populated from templates and you have actually used all four note types.
3. The gap map and pacing plan are logged in `/PROGRESS.md`.
4. The annotated 10-K case note exists and cites one non-obvious Notes disclosure.
5. The recorded loop-explainer exists — the first of many spoken artifacts, because finance careers are verbal.

Evidence: repo URL + CI badge, PROGRESS.md entry, artifact links. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Skipping the phase because "setup is boring," then running Phase 03 pipelines with no CI and unreproducible inputs.
- Week-one over-engineering (Kubernetes, monorepo tooling, a pile of SaaS accounts) instead of the minimum lovable lab.
- Notes as diaries: long, unlinkable, unfindable — if a note cannot be cited by another note, rewrite it.
- Treating Investopedia as a primary source — fine as Tier 4 lookup, but from Phase 01 the sources of record are central banks and regulators.
- Choosing the 12-month plan for status rather than calendar realism; the cut rules exist for exactly this moment.
- Committing API keys or raw datasets to git "just to test" — secrets and snapshot discipline start today.

## 18. Where This Goes Next

Phase 01 (`../01-financial-foundations/README.md`) immediately exercises your new lab: the FRED ingestion, the TVM/bond-pricing kernel, and the ledger simulator all live in fintech-lab and set the pattern that every concept must leave a tested artifact behind. By Phase 03 (`../03-financial-data-engineering/README.md`) the repo grows a lakehouse; by Phase 17 it is a production-grade financial ML platform.
