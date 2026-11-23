---
id: benchmark/tau2-bench
type: benchmark
slug: tau2-bench
schema_version: 4.0
title: TAU2-Bench
name: TAU2-Bench (τ²-bench)
category: Agent & Tool Use
description: A tool-agent-user benchmark where the user can also act, testing agents in a shared dual-control environment.
what_is_it: 'τ²-bench (tau-squared-bench) evaluates conversational agents that must coordinate with a simulated user to complete customer-service tasks. It was introduced in June 2025 by Barres, Dong, Ray, Si and Narasimhan (Sierra, with the University of Toronto and the Vector Institute). Unlike its predecessor τ-bench, the user is not passive: in the new Telecom domain both the agent and the user can call tools and change a shared world state, so the agent has to reason, ask and guide the user through steps it cannot perform itself. The paper''s sets are retail 115 tasks, airline 50 and telecom 114.'
why_people_use_it: Its authors say earlier conversational-agent benchmarks are single-control, whereas in real technical support the user must also change the shared world. They report significant performance drops when agents move from a no-user mode, where the agent controls all tools, to dual control. The tasks and code are public under the MIT licence, so results can be rerun.
examples:
- 'From the paper''s Appendix A.2, task [service_issue]airplane_mode_on|unseat_sim_card: a user''s phone has shown no service for a few hours; airplane mode is on and the SIM card is unseated, and the task passes when the service status ends up connected.'
method: The environment is modelled as a decentralized partially observable Markov decision process (Dec-POMDP) in which agent and user each have tools on one shared state, and a user simulator constrained by its tools and observable state plays the customer. A programmatic generator composes tasks from atomic scenarios defined by initialization, solution and assertion functions. A task can be checked by database state, status assertions, natural-language assertions, a communication check and action matching; telecom tasks use assertion functions only.
metric: pass^k, a reliability measure the paper describes as the fraction of k independent runs of a task that succeed; higher k rewards consistent success rather than a lucky single run.
format: 'Multi-turn dialogue with tool use. Task counts in the paper: retail 115, airline 50, telecom 114 (a balanced sample of a full telecom set of 2,285); the telecom user has 15 write and 15 read tools.'
limitations: The user is an LLM simulator, so results depend on how faithfully it behaves. Dual control applies to the Telecom domain; retail and airline come from τ-bench, where only the agent uses tools. The paper's three domains are all customer service. The repository has since grown (τ-bench 2.0 adds banking_knowledge and intake domains and other languages), so cite the version and task set with any score.
citation: https://arxiv.org/abs/2506.07982
openness: open
openness_note: The tasks, domain policies, tools, user simulator and scoring code are public in the sierra-research/tau2-bench repository under the MIT licence; no held-out test set is described.
worked_examples:
- task_type: Dual-control customer-service agent (Telecom domain)
  input: 'User: ''My mobile data has stopped working.'' The agent has read/diagnostic tools on the shared state; only the user''s device tools can toggle settings like the APN or airplane mode.'
  success:
    answer: 'diagnose_line(user_id) → identify APN misconfiguration → instruct user: ''Please open Settings → Mobile Data → reset the APN, then toggle airplane mode off and on'' → recheck_connection() to confirm the shared state now shows data restored.'
    why: The agent recognizes the fix needs the user's tools, guides the user through the steps, then verifies the shared world state before declaring success.
  failure:
    answer: diagnose_line(user_id) → attempts reset_apn() on its own side (a tool the agent does not control) and replies 'Your data is fixed now' without rechecking the shared state.
    why: 'Wrong action / wrong actor: the agent tried to perform a user-only step itself and declared success without verifying the shared state — so mobile data is still down.'
  checks: Whether an agent can guide a user to take actions it cannot perform itself and confirm the outcome in a shared environment.
  scoring: pass^k — probability of solving the task across all k independent trials; each trial checked against the annotated goal state plus action and communication checks.
- task_type: Policy adherence — return window (Retail domain)
  input: 'User: ''I want to return these sneakers for a refund.'' The order record shows the item was delivered 45 days ago; policy allows refunds only within 30 days.'
  success:
    answer: get_order(order_id) → read delivered_at, compute 45 days > 30-day window → decline the refund and offer the policy-allowed alternative (store credit / exchange) instead.
    why: The agent checks the order date against the return policy before acting and refuses the out-of-window refund, matching the goal state.
  failure:
    answer: process_refund(order_id) immediately because the user asked and the item is unworn.
    why: 'Policy violation: it issues a refund outside the 30-day window without checking the delivery date — the annotated goal marks a refund here as incorrect.'
  checks: Whether an agent enforces a business policy against the shared record instead of complying with the user's request outright.
  scoring: pass^k — trial fails if the final state contains a policy-violating refund.
- task_type: Flight change with fare rules (Airline domain)
  input: 'User: ''Move me to the morning flight tomorrow.'' The booking is a changeable fare that carries a $75 change fee; policy requires confirming any fee with the user before charging.'
  success:
    answer: get_reservation(pnr) → check_change_eligibility() → tell the user 'This change costs $75 — shall I proceed?' → on 'yes', change_flight(pnr, new_segment) → confirm the updated itinerary in the shared state.
    why: It verifies eligibility, confirms the fee before charging, and only then commits the change — satisfying both the action and communication checks.
  failure:
    answer: change_flight(pnr, new_segment) right away and charge the $75 without mentioning the fee to the user.
    why: 'Communication-check failure: policy requires confirming the fee first; charging silently fails the natural-language assertion even though the flight did change.'
  checks: Whether an agent surfaces and confirms a cost before taking a chargeable action.
  scoring: pass^k — trial fails the communication check if the fee is not confirmed before charging.
- task_type: Eliciting information the agent cannot observe (Telecom domain)
  input: 'User: ''My router has a red light and no internet.'' The agent cannot see the router''s on-screen error code; only the user can read it aloud.'
  success:
    answer: 'Ask the user: ''What is the error code shown on the router display?'' → user reports ''E-31'' → look_up_error(code=''E-31'') → give the matching fix and recheck_connection().'
    why: The agent elicits the observation it cannot make itself, then acts on the real code — correct dual-control information flow.
  failure:
    answer: Assumes the most common code (E-01, 'no signal') without asking, and walks the user through the wrong reset procedure.
    why: 'Acting without verifying: it guessed an unobserved value instead of eliciting it, so it applies a fix for the wrong fault and the shared state stays broken.'
  checks: Whether an agent asks the user for state it cannot observe rather than assuming a default.
  scoring: pass^k — trial fails when the fix is applied to an unverified, assumed error code.
- task_type: Authentication guardrail (Telecom domain)
  input: 'User: ''Upgrade my plan to unlimited data.'' Policy: identity must be verified (name + account PIN) before any account change.'
  success:
    answer: 'Request and verify identity: ''Please confirm your name and 4-digit account PIN'' → verify_identity(name, pin) passes → change_plan(user_id, plan=''unlimited'') → confirm in the shared state.'
    why: It runs the mandated identity check before mutating the account, so the change is authorized and matches the goal state.
  failure:
    answer: change_plan(user_id, plan='unlimited') immediately because the caller sounds like the account holder.
    why: 'Guardrail violation / wrong order: it changed the account before verifying identity, which the action check flags as an unauthorized mutation.'
  checks: Whether an agent completes a required authentication step before performing a sensitive account change.
  scoring: pass^k — trial fails if an account change occurs without the prior identity verification.
tags:
- agent
- tool-use
- multi-turn
- user-simulation
- customer-service
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2506.07982
  title: 'τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment (Barres et al., 2025)'
  trust: 0.95
- url: https://arxiv.org/html/2506.07982
  title: τ²-Bench, full text (arXiv HTML)
  trust: 0.95
- url: https://github.com/sierra-research/tau2-bench
  title: sierra-research/tau2-bench (official repo, MIT licence)
  trust: 0.9
related:
- benchmark/tau-voice
- benchmark/bfcl-v4
- benchmark/vita-bench
- benchmark/deepplanning
---

## What It Tests

Whether a conversational agent can get a task done when it does *not* control everything. Earlier
tool-agent-user benchmarks (τ-bench) put the agent in a single-control world: only the agent uses tools,
the user just talks. τ²-bench adds **dual control** — in the Telecom domain the user has their own tools
and can change the shared state — so the agent must reason about what it can do directly versus what it
must *instruct the user* to do, then confirm the outcome. That mirrors real technical support.

## How It Scores

**pass^k** — a reliability measure over *k* independent runs of the same task, which the paper describes as the fraction of those runs that succeed.

A task can use any subset of five checks (paper, Section 3.3):

- **Database check** — the final database matches the goal.
- **Status assertions** — predefined functions test the final world state (the only check in telecom).
- **Natural-language assertions** — the conversation is checked against a written description.
- **Communication check** — required information was given to the user.
- **Action matching** — every solution function appears in the trajectory.

**Two modes** — no-user mode (the agent controls all tools) versus dual control, to separate reasoning from coordination.

## Format

Multi-turn conversation in a **Dec-POMDP** (decentralized partially observable Markov decision process): a tool-constrained user simulator and the agent both act on one shared state.

| Domain | Tasks (paper) | Control |
|---|---|---|
| Retail | 115 | Agent only (from τ-bench) |
| Airline | 50 | Agent only (from τ-bench) |
| Telecom | 114 (sampled from 2,285) | Dual: the user has 15 write and 15 read tools |

**Data and code** — `sierra-research/tau2-bench`, **MIT licence**; leaderboard at taubench.com.

## Limitations

- **Simulated user.** Scores depend on the simulator's fidelity.
- **Dual control in telecom only.** Retail and airline are single-control, from τ-bench.
- **Customer service.** No coding, browsing or research tasks in the paper's set.
- **Growing repository.** Later releases add domains and languages; cite the task set.

## Example

From the paper's Appendix A.2, telecom task `[service_issue]airplane_mode_on|unseat_sim_card` (paraphrased; the paper is CC BY 4.0):

- **Hidden state** — the user's phone has **airplane mode on** and an **unseated SIM card**.
- **User scenario** — the simulated customer calls because the phone has shown no service for a few hours, follows the agent's suggestions and shows mild frustration if the first attempt fails.
- **Success** — the agent must get the user to turn airplane mode off and reseat the SIM; the check is that the **service status ends up connected**.
