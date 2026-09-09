# Phase 14 — Agentic FinTech Systems

> **Stage V — Advanced Financial AI** · **Duration: 3-4 weeks** · **Mastery target: Production**
> **Position in path:** `13-financial-rag-knowledge` ← **this phase** → `15-real-time-streaming`

## 1. Objective

Agents — LLM cores that plan, call tools, keep state, and act across multiple steps — are the most hyped and most dangerous pattern in financial AI: the same architecture that can draft a research brief can, if wired carelessly, move money or file a regulatory report unsupervised. In this phase you will learn the workflow-versus-agent decision framework, design finance-grade tools and guardrails, build human-in-the-loop approval gates, and evaluate agents with trajectory scoring in simulated environments. You will finish able to ship agentic systems whose every action is idempotent, auditable, replayable, and — where the risk requires it — human-approved.

## 2. Why It Matters in Finance

Finance is simultaneously the best and worst home for agents. Best, because enormous operational surface areas — reconciliation exceptions, KYC refresh backlogs, alert triage, research drafting — are high-volume judgment work that institutions have publicly reported staffing with thousands of people. Worst, because every agent action touches regulated processes, client money, or both, and because prompt injection turns "the model" into an attack surface that manipulators can steer toward your tools. The engineering bar is therefore not "the agent completes the task" but "the agent completes the task inside a cage that a risk committee can defend."

- Exception workflows (reconciliation, KYC periodic review, disputes) are the highest risk-adjusted-ROI agent targets: high volume, rule-heavy, with humans already in the loop as a fallback.
- The compliance-critical path (what gets executed, filed, or communicated externally) should stay a deterministic workflow; agency belongs in investigation and drafting, not in irreversible actions.
- Prompt injection is an availability-plus-integrity problem: a manipulated agent inherits your tool permissions, so tool allowlists and sandboxing are security controls, not conveniences.
- Auditability is non-negotiable: regulators and internal validators will ask "why did the system do that?" about a run from six months ago.
- Token-heavy agent loops have real unit economics; an agent that costs more per case than the human it assists is a negative-margin product.

## 3. Prerequisites

- [ ] Phase 11 — document intelligence pipelines (agents consume their outputs)
- [ ] Phase 12 — GenAI engineering: structured outputs, function calling, eval habits
- [ ] Phase 13 — RAG systems (research agents are RAG plus tools plus loops)
- [ ] Phase 08 — AML/fraud alert workflow concepts (triage agents plug into them)
- [ ] Working tool use: you have shipped at least one function-calling feature to a real user

## 4. Learning Outcomes

- I can decide, with a written framework, whether a given financial task should be a deterministic workflow, an agent, or a hybrid — and defend the boundary line.
- I can design typed, schema-validated finance tools (calculators, market data, compliance checks) whose failure modes are explicit.
- I can build a ReAct-style loop that cannot loop forever, overspend, or call non-allowlisted tools.
- I can implement orchestrator-worker and analyst-critic multi-agent patterns and say when each is worth the extra latency and cost.
- I can engineer approval gates with confidence-based escalation, and idempotency keys that make double execution of a financial action impossible.
- I can produce a fully traceable, replayable agent run (Langfuse or equivalent) sufficient for an audit six months later.
- I can defend a tool-using agent against the OWASP Top 10 for LLM Applications categories, especially injection and data exfiltration.
- I can evaluate agents with task success rate and trajectory scoring over simulated scenarios, not vibes.
- I can rank financial agent archetypes by risk-adjusted ROI and explain why trading autonomy stays a research topic.

## 5. Core Concepts (Lessons)

| # | Lesson | Focus | Output artifact |
|---|--------|-------|-----------------|
| 14.1 | Agent anatomy in finance | LLM core + tools + memory + planning; side effects | anatomy diagram + risk notes |
| 14.2 | Workflow vs agent framework | determinism where compliance demands it | written decision framework |
| 14.3 | Finance tool design | schemas, calculators, market data, compliance checkers | typed tool library |
| 14.4 | Tool-use loops & failure modes | ReAct, infinite loops, error cascades | instrumented agent loop |
| 14.5 | Multi-agent patterns | orchestrator-worker, analyst-critic peer review | two-pattern implementation |
| 14.6 | Reflection & self-correction | Reflexion-style critique with bounded retries | critic loop with stop conditions |
| 14.7 | Memory & client context | privacy-constrained state and retention | context store with PII policy |
| 14.8 | Human-in-the-loop | approval gates, confidence-based escalation | gate UI + escalation policy |
| 14.9 | Auditability | trace logs, replayable runs, re-execution | replayable run log |
| 14.10 | Idempotency & spend limits | double-execution prevention, action budgets | idempotent action layer |
| 14.11 | Guardrails & injection defense | allowlists, I/O validation, exfiltration blocking | guardrail suite |
| 14.12 | Sandboxed code execution | container isolation for code tools | Docker-gated code tool |
| 14.13 | Agent evaluation | success rate, trajectory scoring, simulations | 20-scenario eval harness |
| 14.14 | Frameworks, archetypes & cost | LangGraph/AutoGen/CrewAI/Agents SDK/MCP; ROI ranking | framework memo + archetype matrix |

**14.1 Agent anatomy in finance.** An agent is an LLM core that plans, calls tools, reads results, and repeats — plus memory and a policy for when to stop. Finance adds two things every other industry lacks: irreversible side effects (payments, filings, client communications) and a regulator who will read the logs. Map every component of your anatomy to its risk: tools to privilege, memory to privacy, planning to cost, stopping conditions to safety.

**14.2 Workflow vs agent framework.** THE design principle of this phase: deterministic workflows for compliance-critical paths, agency only where judgment is needed. Write the decision table explicitly — is the step reversible? does it need judgment? is it externally visible? — and let it classify every node. Most production "agents" in finance are actually workflows with one or two judgment nodes; that is a feature, not a failure of ambition.

**14.3 Finance tool design.** Tools are the agent's privilege boundary, so design them like a security API: strict input schemas (Pydantic), typed errors, read-only variants where possible, and no tool that bundles "look up" with "execute." Build the canonical finance set — market data fetch, present-value and day-count calculators, execution sandbox, compliance checker — and unit-test each with adversarial inputs, because the LLM will eventually supply one.

**14.4 Tool-use loops & failure modes.** ReAct-style interleaving of reasoning and action (Yao et al. 2022) is the base loop; its finance failure modes are concrete: infinite retry loops against a rate-limited API, hallucinated tool arguments, error messages the model ignores, and cascades where one bad value poisons five downstream calls. Cap steps, cap spend, and force the loop to emit a structured "I am stuck" state that routes to a human.

**14.5 Multi-agent patterns.** Orchestrator-worker fits research and document tasks (decompose, delegate, merge); analyst-critic fits anything memo-shaped (one agent drafts, one attacks the draft's evidence). Multi-agent is not free: it multiplies cost, latency, and failure surface. Ship single-agent until a measured bottleneck proves the pattern pays.

**14.6 Reflection & self-correction.** Reflexion (Shinn et al. 2023) formalizes "critique your own attempt, retry with the lesson." In finance, bound it: max two reflection rounds, critic prompts with checklists (citation exists? figure ties out? compliance language?), and a hard handoff to human review when reflection stops improving the artifact. Uncritiqued self-retry just burns tokens to reinforce the same mistake.

**14.7 Memory & client context.** Client memory improves continuity and creates a data-protection problem: what is stored, for how long, who can read it, and does it leak across clients? Design memory as a governed store — pseudonymized keys, retention policy, per-client isolation enforced at the store layer, never by prompt instructions alone.

**14.8 Human-in-the-loop.** In finance, HITL is not an optional softening; it is the control. Approval gates for irreversible actions, confidence-based escalation (low model confidence, high transaction value, unusual counterparty → human queue), and four-eyes principles for high-value steps. Design the UX like a trading desk blotter: queued items, full context, one-click approve/reject with a reason code, and SLAs so nothing silently ages.

**14.9 Auditability.** Every run produces a trace: inputs, tool calls with arguments and results, model versions, prompts, costs, and the human decisions along the way (Langfuse or an equivalent). Replayability means you can re-execute the run against pinned tool responses; where re-execution is nondeterministic, you store enough to explain exactly why. This is Phase 06's decision audit store, generalized to agents.

**14.10 Idempotency & spend limits.** Financial actions must be idempotent: client-generated idempotency keys, dedupe stores, and state machines that refuse double execution — the same discipline payment platforms use, applied to agent-initiated actions. Layer per-run and per-day spend limits, tool-level value caps, and circuit breakers that freeze the agent class when anomaly counters trip.

**14.11 Guardrails & injection defense.** Use the OWASP Top 10 for LLM Applications (owasp.org; verify current version) as your threat catalog: prompt injection, insecure output handling, excessive agency, sensitive data leakage among them. Defenses: input/output validation, tool allowlists per agent role, URL/domain allowlists to block exfiltration, instruction-hierarchy hygiene, and treating any untrusted text (email, web page, filing) as potential attacker input.

**14.12 Sandboxed code execution.** Any code-execution tool runs in an isolated container: no network by default, filesystem scoping, CPU/memory/time caps, and result sanitization on the way out. Container isolation is the difference between "agent writes code" and "agent writes code that reads your credential store."

**14.13 Agent evaluation.** Evaluate like an engineer: a fixed suite of scenarios (simulated customers, synthetic ledger mismatches, paper-trading environments) with automatic scoring — task success, trajectory quality (unnecessary steps, redundant tool calls, policy violations), and cost per task. Track pass rates across model and prompt versions like you track AUC; regression-test the suite in CI.

**14.14 Frameworks, archetypes & cost.** LangGraph (explicit state machines), AutoGen (conversation-driven multi-agent), CrewAI (role-based crews), and the OpenAI Agents SDK (lightweight handoffs) are the 2025-era landscape; MCP standardizes tool integration so tools outlive frameworks. Honest tradeoffs: abstraction saves days and hides the state transitions you most need to audit — many regulated deployments end up closer to the metal. Rank archetypes by risk-adjusted ROI: reconciliation agent (high ROI, low risk) > reporting agent > research agent > KYC refresh agent > dispute-handling copilot (human-approved) > risk-alert triage agent; trading agents (FinMem, FinRobot) stay research-frontier — never production without heavy controls.

## 6. Mathematics in This Phase

| Concept | What it is | Why finance uses it | Cost if you skip it |
|---|---|---|---|
| Expected cost of decisions | Error rate × cost per error vs automation cost | Autonomy thresholds are an economics problem | You automate by vibes and lose money on tail errors |
| Confidence calibration | Predicted confidence matching empirical correctness | Escalation gates only work if "0.9 confident" means 90% correct | Your confidence-based routing escalates the wrong cases |
| Percentile SLAs | p95/p99 latency and cost distributions | Agent runs are heavy-tailed; means hide the loop-stuck cases | Your "avg 40s" agent times out the p99 reviewer queue |
| Queueing basics (Little's law) | L = λ × W for human review queues | Size the approval-gate staffing from agent throughput | The human queue becomes the undiscussed bottleneck |
| Statistical comparison of variants | Bootstrap CIs on task success / cost | Proving agent v2 beats v1 on 50 scenarios | You ship "improvements" that are noise |
| Token economics | Cost as a function of loop depth and context | Unit economics of every agent use case | The P&L dies at scale, not in the demo |

## 7. Engineering in This Phase

| Topic | Why it matters here |
|---|---|
| Schema-first tool contracts | Tools are the privilege boundary; Pydantic schemas + adversarial tests keep the LLM out of undefined behavior |
| Idempotency keys & dedupe stores | Retries happen at every layer; a financial action must execute exactly once |
| Trace logging (Langfuse/OpenTelemetry) | The run log is the audit artifact; without it every incident is archaeology |
| Container isolation for code tools | Untrusted-model-written code is untrusted code; isolation is not optional |
| Secrets & permission scoping | Agents get scoped, short-lived credentials per tool class — never the service account |
| Graceful degradation | Agent down or over-budget → jobs queue for humans; the business process survives the outage |
| Replayability & pinning | Pinned tool responses and seeds make past runs explainable to validators |

## 8. Tools & Libraries

| Tool | Role |
|---|---|
| LangGraph | Explicit state-machine agent orchestration with checkpointing and human-in-the-loop nodes |
| OpenAI / Anthropic tool use | Native function-calling APIs; the base layer under every framework |
| MCP (Model Context Protocol) | Open protocol for tool integration; decouples tools from any one framework |
| Docker | Container isolation for code-execution and shell tools |
| Langfuse | LLM/agent trace logging, cost accounting, and run replay |
| Pydantic | Schema validation for tool inputs/outputs and guardrail checks |
| LangSmith / custom harness | Agent evaluation runs and trajectory scoring (or roll your own for control) |
| pytest + Locust | Adversarial tool unit tests and load behavior of gate queues |

## 9. Resources

### Tier 1 — Primary / Authoritative

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Anthropic, "Building Effective Agents" (2024) | Essay | All | Patterns | The clearest workflow-vs-agent framing; short and load-bearing | Essential |
| LangGraph documentation (langchain-ai.github.io) | Docs | Intermediate | Orchestration | State machines, checkpointing, human-in-the-loop nodes | Essential |
| Model Context Protocol specification (modelcontextprotocol.io) | Spec | Intermediate | Tool integration | The emerging standard for connecting tools to models | Essential |
| OWASP Top 10 for LLM Applications (owasp.org / genai.owasp.org) | Standard | All | Security | The threat catalog your guardrail suite must answer | Essential |
| Anthropic / OpenAI tool-use documentation | Docs | Intermediate | Function calling | Schema, error, and choice semantics from the source | Recommended |

### Tier 2 — Technical Education

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Yao et al. (2022), "ReAct: Synergizing Reasoning and Acting in Language Models" | Paper | Intermediate | Loops | The base loop; read the failure analyses | Essential |
| Shinn et al. (2023), "Reflexion: Language Agents with Verbal Reinforcement Learning" | Paper | Advanced | Self-correction | The formal version of bounded self-critique | Recommended |
| Wu et al. (2023), "AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation" | Paper | Intermediate | Multi-agent | The conversation-driven multi-agent reference | Recommended |

### Tier 3 — Practitioner

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| Yu et al. (2023), "FinMem: A Performance-Enhanced LLM Trading Agent with Layered Memory" | Paper | Advanced | Trading agents | Research-frontier read; note how far from production it is | Optional |
| FinRobot (Yang et al., 2024, AI4Finance Foundation) | Paper/Code | Advanced | Financial agents | Open agent platform for finance research; useful as a map, not a template | Optional |
| SSRN, "A Review of LLM Agent Applications in Finance and Banking" (search SSRN; verify latest version) | Survey | Intermediate | Landscape | Academic map of who is doing what in financial agents | Recommended |
| Langfuse documentation (langfuse.com) | Docs | Intermediate | Observability | Practical tracing/replay patterns you will actually implement | Recommended |

### Tier 4 — Supplementary

| Resource | Type | Level | Topic | Why Use It | Priority |
|---|---|---|---|---|---|
| CrewAI documentation | Docs | Beginner | Multi-agent | Role-based patterns; fine to skim once you know the fundamentals | Optional |
| OpenAI Agents SDK documentation | Docs | Intermediate | Frameworks | Lightweight handoff pattern worth comparing against LangGraph | Optional |

## 10. Practical Exercises

1. - [ ] Write the workflow-vs-agent decision table for one real process you know (e.g., disputes); classify every step and mark each "agency" node with its reversibility and risk tier.
2. - [ ] Build three schema-validated finance tools (PV calculator, market-data fetch with cache, sanctions/compliance checker stub); fuzz each with malformed inputs and log every schema rejection.
3. - [ ] Implement a ReAct loop with step cap, spend cap, and a structured stuck-state; deliberately trigger each failure mode (bad tool name, poisoned tool output, rate limit) and verify the caps hold.
4. - [ ] Add an analyst-critic pass to a memo generator; measure success-rate and cost delta across 20 scenarios with bootstrap CIs.
5. - [ ] Instrument an agent with Langfuse; then replay a recorded run offline against pinned tool responses and diff the trajectories.
6. - [ ] Implement idempotency: same idempotency key fired five times concurrently against your action layer must yield exactly one execution and four idempotent receipts (test with asyncio).
7. - [ ] Build a red-team set of 10 prompt-injection payloads (OWASP-inspired) aimed at a research agent; verify allowlists and output filters catch exfiltration attempts.
8. - [ ] Run a code-execution tool in a no-network Docker container with time/memory caps; attempt (ethically, locally) three escape or resource-exhaustion patterns and document what stops them.
9. - [ ] Calibrate model confidence on 100 scored scenarios (reliability curve); use it to set the escalation threshold that meets your human-review capacity from Little's law.
10. - [ ] Estimate unit economics: tokens, model prices, and human-minutes per case for two archetypes; find the break-even volume where the agent beats the human queue.

## 11. Mini Projects

**M1 — Reconciliation agent.** Deterministic matching workflow + LLM exception handler on synthetic ledger mismatches (PaySim-style or generated): propose resolution + evidence, human approval for write-offs, full audit trail. Deliverable: repo with workflow, gate UI, and replayable run logs. Difficulty: ★★☆☆☆.

**M2 — Cited research agent.** EDGAR filing tool + web search tool + citation checker; produces company briefs where every claim links to a source passage; injection payloads hidden in fetched pages must not redirect it. Deliverable: agent + red-team log + eval suite. Difficulty: ★★★☆☆.

**M3 — Alert-triage copilot.** IEEE-CIS or synthetic AML alerts → evidence-gathering agent → triage recommendation with confidence → human approval UI and escalation logic (value thresholds auto-escalate). Deliverable: working copilot with SLA dashboard. Difficulty: ★★★☆☆.

**M4 — Analyst-critic investment memo generator.** Multi-agent draft/attack/revise loop over EDGAR + market data; research-only disclaimer enforced in output filter. Deliverable: memo samples + trajectory traces. Difficulty: ★★★★☆.

**M5 — Agent eval harness.** 20 simulated scenarios (customers, mismatches, alerts) with trajectory scoring: success, steps, policy violations, cost; regression report comparing two model versions. Deliverable: harness + CI job. Difficulty: ★★★☆☆.

## 12. Major Project Hook

This phase is the engine room of **Flagship Project 6 — Agentic Compliance Operations Platform** (`/projects/flagship/`; see `/projects/flagship/README.md` for the index): reconciliation, KYC refresh, and alert triage behind approval gates and full audit trails.

## 13. Case Studies & Industry Examples

- **Knight Capital (2012)**: publicly reported ~$440m loss in under an hour from an automated system deployed with a dead control flag — the canonical argument for idempotency, spend limits, and kill switches long before LLM agents existed (see `/case-studies/README.md`).
- **Prompt-injection incidents in deployed LLM products** (publicly documented jailbreaks and data-leakage reports through 2024-2025): the pattern is consistent — untrusted text met powerful tools without allowlists.
- **Bank GenAI operations pilots** (publicly announced by several global banks as of 2025): agentic assistance in reconciliation, KYC document review, and developer workflows — announced with human-in-the-loop language, which is the tell of where trust actually sits.
- **FinMem / FinRobot (2023-2024)**: research-frontier trading agents — read them to understand the ceiling of autonomy claims, and why no serious desk runs them unconstrained.

## 14. Interview Questions

**When should you NOT use an agent?** When the step is deterministic, high-volume, compliance-critical, or irreversible: a rules engine with tests is cheaper, faster, and auditable. Use agency only where genuine judgment meets acceptable blast radius — and put a workflow around it.

**How do you audit an agent's decisions?** Persist every run as a trace — inputs, prompts, model version, tool calls with arguments and results, human approvals — then support replay against pinned tool responses. The audit question "why did it do that?" must be answerable from stored artifacts, not from memory.

**Design an approval-gate UX for financial agents.** Queue with SLA timers, full evidence bundle per item (what the agent found, what it wants to do, confidence), one-click approve/reject with reason codes, auto-escalation on aging, and a four-eyes rule above value thresholds. Optimize reviewer seconds per case, not clicks per feature.

**How do you prevent double-execution of a financial action?** Client-generated idempotency keys on every action request, a dedupe store checked transactionally before execution, and a state machine that only advances from pending to executed once. Test with concurrent retries — the bug shows up exactly there.

**How do you defend a tool-using agent against prompt injection?** Treat all external text as untrusted: tool allowlists per role, schema validation, domain allowlists on outbound fetches, no credentials in prompt-visible context, output filters on anything leaving the trust boundary, and approval gates on any irreversible tool. Assume some injection succeeds; design so success is contained.

**How do you evaluate agents rigorously?** Fixed scenario suite with automatic scoring: task success, trajectory quality (extra steps, policy violations), and cost per task; bootstrap CIs across versions; regression-gate releases on the suite. Simulated environments make it repeatable; production traces keep it honest.

**Single agent or multi-agent — how do you decide?** Default single-agent; add orchestrator-worker when tasks decompose cleanly and parallelism pays, analyst-critic when output quality risk exceeds the extra cost and latency. Every added agent multiplies failure surface and audit complexity.

**What does MCP actually give you?** A standard protocol so tools are written once and served to any MCP-aware model or framework — it decouples your tool investment from framework churn. It does not solve auth, rate limiting, or audit; you still own those.

**Where do agent costs bite first?** Context growth across loop iterations and reflection rounds. Budget per run, cap steps, compress memory, and route easy cases to cheaper paths — cost per task is a first-class metric, not an afterthought.

**What is your graceful-degradation design when the agent service is down?** The business process never depends on the agent: jobs fall back to a human queue with the same evidence bundle, and deterministic workflow legs keep running. Measure time-to-human-queue in your failover tests.

## 15. Assessment — Can You Pass the Bar?

- [ ] Write a workflow-vs-agent decision memo for a real process a risk officer would sign.
- [ ] Ship a tool-using agent whose every irreversible action is idempotent and gated.
- [ ] Demonstrate a concurrent-retry test where a financial action executes exactly once.
- [ ] Produce a full trace and offline replay of a past run, and explain a failure from the artifacts alone.
- [ ] Pass a 10-payload injection red-team with zero exfiltrations and a written control inventory mapped to the OWASP LLM Top 10.
- [ ] Show an eval report: success rate, trajectory violations, and cost per task with CIs across two versions.
- [ ] Explain to a regulator-in-the-role how your escalation thresholds were calibrated and staffed.

## 16. Mastery Checkpoint

You may proceed to Phase 15 when:

1. Repo evidence exists for: typed tool library, gated agent with idempotency, trace/replay store, guardrail suite, and the 20-scenario eval harness.
2. Your framework memo classifies at least one real financial process end-to-end, with the agency boundary justified in writing.
3. You can present the archetype ROI ranking and defend where you would — and would not — deploy autonomy (record it; store under `/notes/artifacts/`).

Evidence: repo links + red-team log + eval report + recorded decision defense. Log the checkpoint in `/PROGRESS.md`.

## 17. Failure Modes & Gotchas

- Agentifying a path because it is complex, when complexity was exactly the signal that it needed determinism first.
- Approval gates that approve everything because the evidence bundle takes longer to read than the task itself.
- Idempotency added at the HTTP layer but not the state machine — retries still double-execute under concurrency.
- Prompt-injection defenses that only filter input text while the agent's tool results (web pages, filings) carry the payload.
- Memory that leaks client context across sessions — a privacy incident no amount of capability justifies.
- Multi-agent systems deployed for the demo effect, with cost per task exceeding the human process and no trajectory evals to notice.
- Treating "the agent handled it" as evaluation; without a fixed scenario suite you have anecdotes, not evidence.

## 18. Where This Goes Next

Phase 15 makes the surrounding machinery real-time: the alert streams and payment events your agents triage arrive under latency budgets and exactly-once constraints that shape what agency can even reach. Phase 16 then turns the guardrail, audit, and governance instincts of this phase into a formal security, compliance, and responsible-AI stack.
