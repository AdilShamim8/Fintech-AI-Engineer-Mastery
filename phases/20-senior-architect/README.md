# Phase 20 — Senior & Architect Level

> **Stage VI — Production & Leadership** · **Duration: ongoing — 8+ weeks** · **Mastery target: Leadership**
> **Position in path:** `19-specialist-tracks` ← **this phase** → graduation — the loop restarts at `/README.md`

## 1. Objective

Seniority in fintech AI is judged on judgment: designing systems under time pressure, writing decisions others can act on, choosing build-vs-buy with the vendor market actually in view, and operating inside the governance and org structures of regulated finance. This phase is the rehearsal room — six timed design problems, strategy and promotion-packet writing, stakeholder translation, product economics, and the mentoring practice that proves mastery. It never fully ends; that is the point.

## 2. Why It Matters in Finance

The staff/principal/architect trajectory in financial institutions is less about deeper models and more about consequences: your designs allocate risk budgets, your tradeoffs reach model councils and regulators, and your vocabulary must shift fluently between engineers, validators, and the C-suite. The market rewards this rare combination — engineers who can write a defensible architecture and survive a risk committee — with the field's scarcest roles. None of it is improvisable; all of it is rehearsable.

- Design interviews at senior levels test structure under time pressure: 45 minutes from blank page to defensible tradeoffs.
- Build-vs-buy decisions in fintech are vetted by procurement, risk, and audit — a vendor decision without a risk argument is a rejected decision.
- The three-lines-of-defense model determines where ML engineers sit and whose language they must speak (Phase 16's frame, now at org scale).
- Product thinking separates architects from technicians: approval rate × margin − loss rate is the equation your designs serve.
- Public artifacts — writing, talks, OSS — are how senior reputation compounds beyond one employer's walls.

## 3. Prerequisites

- [ ] Phases 01-18 complete; capstone defended (this phase rehearses at a higher altitude)
- [ ] Phase 19 specialization started (at least one track in progress)
- [ ] `/system-design/README.md` problems studied but not yet done under time pressure
- [ ] Real experience surviving one design review, incident review, or model council (any venue)

## 4. Learning Outcomes

- I can produce a structured, defensible design for any of the six `/system-design/` problems in 45 minutes, written first, then verbally.
- I can write ADRs and strategy docs that survive review by engineers, risk officers, and executives.
- I can run a build-vs-buy analysis for a fintech AI capability, including the vendor landscape and third-party risk frame.
- I can articulate platform strategy (central vs embedded teams; platform-as-product) and org placement of ML engineers within three lines of defense.
- I can manage stakeholders across risk, compliance, audit, legal, and validation functions as translation work, not friction.
- I can reason in unit economics: approval rate × margin − loss rate, cost per decision, and the NPS-vs-loss tradeoff.
- I can run ML interviews that probe what actually predicts senior performance.
- I can produce a promotion-packet-style self-review with evidence, and a teaching artifact that transfers a skill to others.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 20.1 | System-design mastery under time pressure | six problems, 45 minutes each | six timed design docs |
| 20.2 | ADRs & tradeoff writing | decisions as durable artifacts | ADR set (from 20.1) |
| 20.3 | Build-vs-buy in fintech | vendor landscape + risk framing | build-vs-buy analysis |
| 20.4 | Platform strategy | central vs embedded; platform-as-product | strategy note |
| 20.5 | Org design for financial AI | three lines of defense; model councils | org-position memo |
| 20.6 | Stakeholder management | translation layers across risk/compliance/audit | stakeholder map + scripts |
| 20.7 | Regulatory engagement basics | exams, findings, supervisory dialogue | engagement playbook |
| 20.8 | Product thinking for engineers | unit economics of decisioning | unit-economics model |
| 20.9 | Portfolio & brand | writing, OSS, talks; knowledge base | public artifact plan |
| 20.10 | Interviewing & hiring | running ML interviews; what to probe | interview kit |
| 20.11 | Mentoring & teaching | mastery tests through others | mentoring notes |
| 20.12 | Roadmaps, strategy docs & ethics leadership | the engineer's voice in governance | strategy doc draft |

**20.1 System-design mastery under time pressure.** Run all six problems in `/system-design/README.md` under a strict 45-minute clock: 5 minutes clarifying, 10 minutes requirements and constraints (latency, consistency, regulation), 20 minutes architecture with explicit tradeoffs, 10 minutes risk/degrade/evolution. Do each written first, then verbal to a recorder. The discipline — a defensible partial answer with named tradeoffs — is the deliverable, not completeness.

**20.2 ADRs & tradeoff writing.** Convert each timed design into 2-3 ADRs in Nygard format: context, options, decision, consequences. Senior writing is decision-dense and emotion-free: what you chose, what you rejected, what would change your mind. These ADRs feed your portfolio and your interview stories simultaneously.

**20.3 Build-vs-buy in fintech.** Frame every capability decision as: differentiating (build), commodity (buy), or compliance-critical (buy + validate hard). Map the vendor landscape as it publicly presents itself — FICO and SAS (analytics/scoring incumbents), Zest AI (ML underwriting), Feedzai and Featurespace (fraud/AML), Quantexa (entity resolution/contextual decisioning), ComplyAdvantage (screening/data), Chainalysis (blockchain analytics), DataRobot and H2O.ai (AutoGen ML platforms) — as market landscape, not endorsement; positions shift and claims are the vendors' own. Add the third-party risk lens (Phase 16): validation rights, exit plans, sub-processor chains.

**20.4 Platform strategy.** Central ML platform vs embedded domain teams is a false binary; the live question is which capabilities centralize (registry, monitoring, feature infrastructure, audit) and which stay local (domain features, policy logic). The platform-as-product mindset: internal customers, adoption metrics, roadmaps, and the humility that a platform nobody uses is a cost center. Write the strategy note for a realistic org shape.

**20.5 Org design for financial AI.** First line owns and operates models; second line (model risk/compliance) validates and challenges; third line audits. ML engineers usually sit in line one with accountability into line two's evidence demands; model councils and AI committees arbitrate tiering and exceptions. Draft where you and your team sit, what you owe each line, and where the friction is structural — not personal.

**20.6 Stakeholder management.** Each function has a native language: validators speak evidence and re-performance, compliance speaks obligations and controls, audit speaks findings and remediation, legal speaks liability, the business speaks money. Build translation layers: the same model change presented as metrics, as control evidence, and as P&L. Scripts for the recurring conversations (a challenged model, a delayed approval, a risk acceptance) are senior tooling.

**20.7 Regulatory engagement basics.** Exams and supervisory dialogues reward institutions that can produce artifacts on demand. The engineer's role: keep evidence retrievable, respond to findings with systemic fixes, and never let the first draft of a regulator-facing document be written under deadline. Know the escalation path when engineering reality conflicts with a submitted claim.

**20.8 Product thinking for engineers.** Decisioning economics: revenue ≈ approval rate × margin per approval − approval rate × loss rate × severity − operating cost; your model and policy live inside that equation, and "better model" must translate into a term that moves. Cost per decision and the NPS-vs-loss tradeoff (friction, false declines) complete the frame. Build the unit-economics model for one real system and argue one design change from it.

**20.9 Portfolio & brand.** A senior reputation compounds through artifacts: technical writing (blog posts from Phases 18-19 work), OSS contributions (tooling you actually use), talks (mock defenses make drafts), and a personal knowledge base — this curriculum's `/notes/` discipline is the career asset version. Plan one public artifact per quarter; consistency beats virality.

**20.10 Interviewing & hiring.** Running interviews is the mirror of passing them: design rounds scored on structure and tradeoffs, ML-depth rounds scored on judgment under uncertainty, and behavioral rounds probing ownership across governance. Write your own interview kit — questions, rubrics, and the follow-ups that separate rehearsed from real — and calibrate it against colleagues.

**20.11 Mentoring & teaching.** Mentoring is the cheapest mastery test: explaining Phase 06's calibration or Phase 15's watermarks to a strong junior exposes every gap in your own understanding. Keep mentoring notes: what confused them, which explanation landed, what you had to relearn. Teaching artifacts (tutorials, talk notes) double as portfolio evidence.

**20.12 Roadmaps, strategy docs & ethics leadership.** Strategy docs for fintech AI follow a shape: context → problems worth solving → options with tradeoffs → bets with kill criteria → what we will not do. Ethics leadership is the engineer's voice in model governance: raising proxy-discrimination findings, contesting metrics that flatter, and insisting the degradation path is fair — the Phase 16 instincts, now exercised with seniority rather than permission.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Decision economics | Approval rate × margin − loss rate − opex | Designs must argue in P&L terms | You optimize models while the business optimizes nothing |
| Cost per decision | Fully-loaded cost over decision volume | Platform and model choices priced honestly | Build-vs-buy argued by preference |
| Tradeoff quantification | Explicit numbers for latency/cost/risk axes | ADRs without numbers are vibes | Your "better" claims die in committee |
| Adoption & diffusion metrics | Platform usage, time-to-integrate | Platform-as-product needs product math | Platforms get built, not adopted |
| Risk-adjusted prioritization | Expected value × probability on roadmaps | Sequencing bets with kill criteria | Strategy docs full of wishes |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Design-doc discipline | Timed designs become ADRs become review skills; the loop is deliberate practice |
| Vendor evaluation rigor | Landscape claims verified against your requirements and risk frame, not demos |
| Evidence architecture | Your repos already produce artifacts; the senior move is making them retrievable on demand |
| Interview rubrics | Structured scoring beats intuition; write rubrics like tests |
| Knowledge-base tooling | Notes, ADRs, and papers linked in one searchable vault (this repo's `/notes/`) |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| `/system-design/README.md` problem set | The six timed exercises at the core of this phase |
| ADR templates (Nygard/MADR) | Decision-writing format for every design |
| Whiteboard or Excalidraw/draw.io | The verbal-design rehearsal medium |
| Blog/newsletter platform | Quarterly public artifacts |
| Conference/meetup CFP trackers | Talk pipeline from mock defenses outward |
| Rubric sheets (structured interview kits) | Calibration for hiring loops you run |
| `/PROGRESS.md` + `/notes/` vault | Promotion-packet evidence base |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| `/system-design/README.md` + six problems (this repo) | Problems | Advanced | Design mastery | The timed rehearsal set for this phase | Essential |
| Kleppmann, *Designing Data-Intensive Applications* (2017) | Book | Advanced | Systems | The vocabulary of tradeoffs you will write | Essential |
| Michael Nygard, "Documenting Architecture Decisions" (2011) | Article | All | ADRs | The format your ADRs follow | Essential |
| SR 11-7 + EU AI Act (as org-design context; see Phase 16) | Regulation | Advanced | Governance | The frame your org maps answer to | Reference |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Tanya Reilly, *The Staff Engineer's Path* (O'Reilly 2022) | Book | Intermediate | Senior roles | The broad-scope engineer's operating manual | Essential |
| Will Larson, *Staff Engineer* (2021) and *An Elegant Puzzle* (2019) | Books | Intermediate | Leadership | Staff-plus narratives and org-design case studies | Recommended |
| Google SRE books (sre.google, free) | Books | Intermediate | Reliability org | SLO/postmortem culture at organizational scale | Recommended |
| Marty Cagan, *Inspired* (2nd ed., 2017) | Book | Beginner-Intermediate | Product | Product thinking translated for engineers | Optional |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Vendor public documentation (FICO, Feedzai, Quantexa, DataRobot, and peers) | Docs/Reports | Intermediate | Build-vs-buy | What vendors claim; read as market landscape | Reference |
| Public postmortems & design docs (SRE book cases, fintech engineering blogs) | Case studies | Advanced | Judgment | Calibrate your tradeoff instincts against published practice | Recommended |
| Published model-governance frameworks from major banks | Reports | Advanced | Org design | Public structure for committees, tiers, and gates | Optional |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Engineering-leadership newsletters/podcasts (select one or two) | Media | Intermediate | Leadership | Ambient exposure to senior-level dilemmas | Optional |
| Conference talk archives (QCon, Strange Loop archives, KubeCon) | Talks | Intermediate | Communication | Talk craft worth imitating | Optional |

## 10. Practical Exercises

1. - [ ] Run all six `/system-design/` problems under 45 minutes each, written; then re-do two verbally to a recorder; compare structure drift between written and verbal.
2. - [ ] Convert each timed design into 2-3 ADRs (Nygard format); have one reviewed by an engineer and one by a risk/compliance persona.
3. - [ ] Write a full build-vs-buy analysis for one capability (e.g., fraud decisioning): requirements, vendor landscape scan (as of date noted), risk frame, recommendation.
4. - [ ] Draft the platform strategy note: what centralizes, what embeds, adoption metrics, and the migration path from today's state.
5. - [ ] Draw your org's three-lines-of-defense map with ML roles placed; annotate what each line owes the others and where friction is structural.
6. - [ ] Build the unit-economics model for your capstone system (approval × margin − loss − cost per decision); argue one design change from the numbers.
7. - [ ] Write the stakeholder scripts: challenged model, delayed approval, risk acceptance — each in three translations (engineering, risk, executive).
8. - [ ] Assemble the promotion-packet-style self-review: scope, impact, evidence links from `/PROGRESS.md`, artifacts, and the narrative arc.
9. - [ ] Produce the teaching artifact: a tutorial or talk notes for one specialist topic (Phase 19) or system pattern; deliver it once to a real audience.
10. - [ ] Write your interview kit: two design questions, two depth questions, two behavioral questions, with rubrics and follow-ups; run one mock interview and calibrate.

## 11. Mini Projects

**M1 — Six timed design docs.** The full `/system-design/` set under 45-minute pressure, each followed by an ADR pair. Deliverable: design-doc folder with ADRs. Difficulty: ★★★★☆.

**M2 — Build-vs-buy dossier.** One capability analyzed end to end with vendor landscape and third-party risk frame. Deliverable: decision-ready dossier. Difficulty: ★★★☆☆.

**M3 — Strategy document.** A "GenAI in [institution type] — what we will and will not do" strategy doc with bets, kill criteria, and governance gates. Deliverable: 5-8 page doc. Difficulty: ★★★☆☆.

**M4 — Public artifact + talk.** One published technical post and one delivered talk (meetup, internal guild, or recorded session). Deliverable: links + recording. Difficulty: ★★★☆☆.

**M5 — Promotion packet & teaching artifact.** Self-review with evidence base, plus one tutorial that transfers a skill to a junior. Deliverable: packet + tutorial. Difficulty: ★★★☆☆.

## 12. Major Project Hook

There is no new build here: the deliverables of this phase are made from the artifacts you already own — flagship designs (Phase 18), governance evidence (Phase 16), platform discipline (Phase 17), and specialist depth (Phase 19) — recast as design docs, strategy, and public proof. That conversion is the promotion.

## 13. Case Studies & Industry Examples

- **Publicly documented platform org designs (fintech and bank engineering blogs)**: how institutions split platform vs domain teams and where model governance sits — read structurally, not for brand names.
- **Vendor-landscape evolution, 2020-2025 (publicly reported)**: incumbent analytics vendors absorbing ML, ML platforms commoditizing, fraud/graph specialists consolidating — a live case for hedged, dated build-vs-buy claims.
- **Google SRE postmortem culture (published)**: the operations-culture benchmark your ML incident reviews should imitate.
- **Your own mock defenses (Phases 18-19)**: recorded, watchable evidence of your communication trajectory — the most personal case study in this phase.

## 14. Interview Questions

**Design an auth-time fraud decisioning system — 45 minutes, go.** Clarify constraints (latency budget, volumes, regulation), draw the spine (stream → features → scoring → policy → audit), name the two hardest tradeoffs (consistency vs availability of feature freshness; model vs rules fallback), and close with degradation and evolution. Structure and named tradeoffs beat completeness every time.

**Tell me about a time you were overruled by a risk or compliance function.** Show the disagreement in both languages (engineering evidence, regulatory duty), what you escalated, what you accepted, and what you changed in your process afterward. Senior answers demonstrate that overrule is sometimes correct.

**How do you decide build vs buy for a risk capability?** Classify the capability (differentiating/commodity/compliance-critical), scan the vendor landscape with dated claims, then decide with third-party risk framing: validation rights, exit costs, sub-processors. Never argue preference; argue the frame.

**How would you improve approval rate without increasing losses?** Work the equation: segment-specific policies, better calibration where thresholds bind, pre-approval data quality, fraud-loss leakage into credit losses, friction reduction for good applicants (NPS tradeoff). Show you can move terms, not just the model.

**How do you run ML interviews?** Structured design rounds with rubrics, depth questions that probe judgment under uncertainty ("what would change your mind"), and behavioral rounds on governance exposure. Calibrate with colleagues; kill questions that only test trivia.

**Your platform has 40% adoption. What do you do?** Treat it as product failure: interview non-adopters, find the parity gaps, cut setup cost, publish success metrics — and consider whether parts should never have been centralized. Platforms earn adoption or get dissolved.

**Write the outline of a strategy doc for GenAI in a bank.** Context and constraints → problems worth solving (with owners) → options and tradeoffs → a small number of bets with kill criteria → governance gates → what we will not do. The "will not do" section is where credibility lives.

**How do you give an engineer's voice in model governance without becoming a bystander?** Bring evidence (fairness findings, degradation paths, audit costs) into councils in decision-ready form, contest flattering metrics explicitly, and own the follow-through. Silence in governance is a design decision too.

**What does a promotion packet for an AI engineer contain?** Scope shift (self → team → org), artifacts as evidence, governance exposure (councils, validations, incidents), mentorship, and the narrative arc tying them. Written from `/PROGRESS.md` evidence, not adjectives.

**How do you know a design is good before it ships?** Tradeoffs are explicit and quantified, degradation paths exist and are tested, governance evidence is produced by construction, someone who disagrees has stress-tested it, and the second-order effects (cost per decision, on-call load) have owners.

## 15. Assessment — Can You Pass the Bar?

- [ ] Six timed design docs exist, each defensible in a 10-minute verbal drill without notes.
- [ ] ADR set shows quantified tradeoffs and named rejected alternatives.
- [ ] Build-vs-buy dossier survives a hostile review from both engineering and risk personas.
- [ ] Unit-economics model drives one real design argument with numbers.
- [ ] Stakeholder scripts delivered fluently in all three translations.
- [ ] One public artifact shipped and one talk delivered (or recorded).
- [ ] Promotion-packet self-review and teaching artifact completed and linked from `/PROGRESS.md`.

## 16. Mastery Checkpoint

This phase closes when — for now:

1. The six timed designs + ADRs, dossier, strategy doc, and packet are complete and stored (repo + `/notes/artifacts/`).
2. One public artifact and one teaching artifact exist outside your own head.
3. You can run any of the six designs verbally under pressure, and run a credible interview loop with your kit.
4. Your research agenda (Phase 19) and knowledge base remain alive — the loop restarts at `/README.md` with new depth each pass.

Seniority is maintained by practice cadence: one timed design and one public artifact per quarter keeps the edge.

## 17. Failure Modes & Gotchas

- Designing for the demo instead of the degradation path — committees always ask "what happens when it fails?"
- Vendor claims quoted without dates or validation rights; the landscape moves and procurement knows it.
- Platform strategy by ideology (always-centralize/never-centralize) instead of capability-by-capability analysis.
- Speaking engineer to everyone: validators, auditors, and executives each need their translation; refusing to translate reads as avoidance.
- Promotion packets full of adjectives; evidence links or it did not happen.
- Mentoring as status rather than mastery test — the confusions you hear are your own gaps talking.
- Letting the research agenda and knowledge base die after the capstone; senior judgment decays without fresh inputs.

## 18. Where This Goes Next

There is no Phase 21: the path loops. Revisit `/README.md` with your new altitude, keep `/research/` and `/docs/08-research-agenda.md` current, maintain the quarterly practice cadence (timed design, public artifact, teaching), and use your mentoring notes to identify which phase to re-run at depth. Mastery in fintech AI is not a destination you reach; it is a system you operate.
