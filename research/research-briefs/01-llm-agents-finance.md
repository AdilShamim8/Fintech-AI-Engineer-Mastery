# Brief 01 — LLM Agents in Financial Workflows

> **Maturity: Emerging → early practice** (as of 2026). Banks publicly moved from pilots to governed production for narrow, auditable workflows; autonomous money-moving agents remain frontier. Verify current claims quarterly.

## Status Map
- **Established:** agent anatomy (LLM + tools + memory + planning); ReAct-style loops; evaluation via task success rates.
- **Current practice:** deterministic-workflow-first designs with LLM steps for judgment; human-approval gates; full trace logging. High-ROI archetypes: reconciliation, reporting, KYC refresh, alert triage (with sign-off).
- **Emerging:** multi-agent analyst/critic patterns; trajectory-based evals; simulation environments (simulated customers, paper-trading).
- **Frontier:** autonomous trading agents (FinMem, FinRobot line); agents writing/verifying their own audit narratives; cross-system orchestration with formal guarantees.

## Key Papers & Resources
- Yao et al., "ReAct: Synergizing Reasoning and Acting in Language Models" (2022) — the loop.
- Shinn et al., "Reflexion" (2023) — verbal self-correction.
- Wu et al., "AutoGen" (2023) — multi-agent conversation patterns.
- Anthropic, "Building Effective Agents" (2024-25) — the workflow-first engineering stance.
- "A Review of LLM Agent Applications in Finance and Banking" (SSRN, 2024-25) — finance-specific survey (verify latest version).
- OWASP Top 10 for LLM Applications — the threat catalog; MCP specification for tool integration.

## Open Problems the Field Actually Argues About
1. When is an agent better than a tuned deterministic pipeline? (Measurement is rare; the honest answer is "less often than demos suggest".)
2. Auditability: can agent trajectories be made replayable and regulator-acceptable?
3. Injection resilience when tools touch money and data simultaneously.
4. Evaluation: trajectory quality metrics that predict real-world incident rates.
5. Cost ceilings: when does agent overhead exceed analyst cost?

## Solo Experiments (runnable with public tools/data)
1. **Reconciliation agent vs rules:** synthetic ledger mismatches (inject errors into a payments table); compare deterministic rules, LLM-only, and hybrid workflow on resolution rate + audit completeness.
2. **Triage copilot:** rank IBM-AML-style alerts with an LLM rubric vs a learned ranker; measure precision@capacity and cost per alert.
3. **Trajectory eval harness:** 20 scripted scenarios with graded paths (tool-choice, escalation, hallucination flags); run two agent designs and diff.
4. **Injection red-team:** plant instructions in "retrieved documents"; measure compliance rates across 3 prompt-hardening strategies.

## Curriculum Hooks
Phase 14 (main), Phase 16 (governance), Flagship 06, interview track E6.

## What Would Change My Mind
A public, reproducible benchmark showing autonomous agents beating governed hybrid workflows on a money-adjacent task *with equal audit quality* — that would move "workflow-first" from doctrine to open question.
