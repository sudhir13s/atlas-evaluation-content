---
id: benchmark/tau2-bench
type: benchmark
slug: tau2-bench
schema_version: 1.0
title: "TAU2-Bench"
name: "TAU2-Bench (τ²-bench)"
category: Agent & Tool Use
description: "A tool-agent-user benchmark where the user can also act, testing agents in a shared dual-control environment."
what_is_it: "τ²-bench (tau-squared-bench) evaluates conversational agents that must coordinate with a simulated user to complete customer-service tasks. Unlike its predecessor τ-bench, the user is not passive: in the new Telecom domain both the agent and the user can call tools and change a shared world state, so the agent has to reason, ask, and guide the user through steps it cannot perform itself."
why_people_use_it: "It captures a failure mode single-control benchmarks miss — real support agents must instruct a user to do things (reboot a device, toggle a setting) rather than doing everything themselves. It is a standard reliability test for customer-facing agents and is maintained by Sierra alongside the original τ-bench."
examples:
  - "Telecom support: the user's mobile data is not working. The agent must diagnose across a shared device/network state, and guide the user to toggle airplane mode or reset APN settings — actions only the user's tools can perform — then verify the fix."
method: "The environment is modeled as a decentralized partially observable Markov decision process (Dec-POMDP): agent and user each have their own tools acting on one shared state. A reliable, tool-and-state-constrained user simulator plays the customer. Tasks are generated compositionally from atomic components with pre-annotated goal states, then each run is scored automatically."
metric: "pass^k — the probability that an agent solves the same task in all k independent trials (pass^1 is the plain success rate; higher k measures reliability/consistency). Success per run is checked by comparing the final database/world state to the annotated goal, plus action and communication (natural-language assertion) checks."
format: "Multi-turn dialogue with tool use, agent and user acting on a shared environment."
saturation: low
limitations: "The user is an LLM simulator; its behavior — though constrained by tools and observable state — is not a real human, so results depend on the simulator's fidelity. The headline dual-control results center on the Telecom domain (the framework also carries airline and retail from τ-bench), so it is customer-service-shaped and does not test coding, research, or open web agents. Compositional task generation can produce uneven difficulty."
citation: "https://arxiv.org/abs/2506.07982"
worked_examples:
  - task_type: "Dual-control customer-service agent (Telecom domain)"
    input: "User: 'My mobile data has stopped working.' The agent has read/diagnostic tools on the shared state; only the user's device tools can toggle settings like the APN or airplane mode."
    success:
      answer: "diagnose_line(user_id) → identify APN misconfiguration → instruct user: 'Please open Settings → Mobile Data → reset the APN, then toggle airplane mode off and on' → recheck_connection() to confirm the shared state now shows data restored."
      why: "The agent recognizes the fix needs the user's tools, guides the user through the steps, then verifies the shared world state before declaring success."
    failure:
      answer: "diagnose_line(user_id) → attempts reset_apn() on its own side (a tool the agent does not control) and replies 'Your data is fixed now' without rechecking the shared state."
      why: "Wrong action / wrong actor: the agent tried to perform a user-only step itself and declared success without verifying the shared state — so mobile data is still down."
    checks: "Whether an agent can guide a user to take actions it cannot perform itself and confirm the outcome in a shared environment."
    scoring: "pass^k — probability of solving the task across all k independent trials; each trial checked against the annotated goal state plus action and communication checks."
  - task_type: "Policy adherence — return window (Retail domain)"
    input: "User: 'I want to return these sneakers for a refund.' The order record shows the item was delivered 45 days ago; policy allows refunds only within 30 days."
    success:
      answer: "get_order(order_id) → read delivered_at, compute 45 days > 30-day window → decline the refund and offer the policy-allowed alternative (store credit / exchange) instead."
      why: "The agent checks the order date against the return policy before acting and refuses the out-of-window refund, matching the goal state."
    failure:
      answer: "process_refund(order_id) immediately because the user asked and the item is unworn."
      why: "Policy violation: it issues a refund outside the 30-day window without checking the delivery date — the annotated goal marks a refund here as incorrect."
    checks: "Whether an agent enforces a business policy against the shared record instead of complying with the user's request outright."
    scoring: "pass^k — trial fails if the final state contains a policy-violating refund."
  - task_type: "Flight change with fare rules (Airline domain)"
    input: "User: 'Move me to the morning flight tomorrow.' The booking is a changeable fare that carries a $75 change fee; policy requires confirming any fee with the user before charging."
    success:
      answer: "get_reservation(pnr) → check_change_eligibility() → tell the user 'This change costs $75 — shall I proceed?' → on 'yes', change_flight(pnr, new_segment) → confirm the updated itinerary in the shared state."
      why: "It verifies eligibility, confirms the fee before charging, and only then commits the change — satisfying both the action and communication checks."
    failure:
      answer: "change_flight(pnr, new_segment) right away and charge the $75 without mentioning the fee to the user."
      why: "Communication-check failure: policy requires confirming the fee first; charging silently fails the natural-language assertion even though the flight did change."
    checks: "Whether an agent surfaces and confirms a cost before taking a chargeable action."
    scoring: "pass^k — trial fails the communication check if the fee is not confirmed before charging."
  - task_type: "Eliciting information the agent cannot observe (Telecom domain)"
    input: "User: 'My router has a red light and no internet.' The agent cannot see the router's on-screen error code; only the user can read it aloud."
    success:
      answer: "Ask the user: 'What is the error code shown on the router display?' → user reports 'E-31' → look_up_error(code='E-31') → give the matching fix and recheck_connection()."
      why: "The agent elicits the observation it cannot make itself, then acts on the real code — correct dual-control information flow."
    failure:
      answer: "Assumes the most common code (E-01, 'no signal') without asking, and walks the user through the wrong reset procedure."
      why: "Acting without verifying: it guessed an unobserved value instead of eliciting it, so it applies a fix for the wrong fault and the shared state stays broken."
    checks: "Whether an agent asks the user for state it cannot observe rather than assuming a default."
    scoring: "pass^k — trial fails when the fix is applied to an unverified, assumed error code."
  - task_type: "Authentication guardrail (Telecom domain)"
    input: "User: 'Upgrade my plan to unlimited data.' Policy: identity must be verified (name + account PIN) before any account change."
    success:
      answer: "Request and verify identity: 'Please confirm your name and 4-digit account PIN' → verify_identity(name, pin) passes → change_plan(user_id, plan='unlimited') → confirm in the shared state."
      why: "It runs the mandated identity check before mutating the account, so the change is authorized and matches the goal state."
    failure:
      answer: "change_plan(user_id, plan='unlimited') immediately because the caller sounds like the account holder."
      why: "Guardrail violation / wrong order: it changed the account before verifying identity, which the action check flags as an unauthorized mutation."
    checks: "Whether an agent completes a required authentication step before performing a sensitive account change."
    scoring: "pass^k — trial fails if an account change occurs without the prior identity verification."
tags: [agent, tool-use, multi-turn, user-simulation, customer-service]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2506.07982", title: "τ²-Bench: Evaluating Conversational Agents in a Dual-Control Environment (Barres et al., 2025)", trust: 0.95 }
  - { url: "https://github.com/sierra-research/tau2-bench", title: "sierra-research/tau2-bench (official repo)", trust: 0.9 }
related: [benchmark/bfcl-v4, benchmark/vita-bench, benchmark/deepplanning]
---

## What It Tests
Whether a conversational agent can get a task done when it does *not* control everything. Earlier
tool-agent-user benchmarks (τ-bench) put the agent in a single-control world: only the agent uses tools,
the user just talks. τ²-bench adds **dual control** — in the Telecom domain the user has their own tools
and can change the shared state — so the agent must reason about what it can do directly versus what it
must *instruct the user* to do, then confirm the outcome. That mirrors real technical support.

## How It Scores
The metric is **pass^k**: the probability the agent succeeds on the same task across all *k* independent
trials. pass^1 is the ordinary success rate; larger *k* punishes flakiness, rewarding agents that are
consistently right rather than occasionally lucky. Each individual trial is judged automatically by
comparing the **final world/database state** against a pre-annotated goal state, combined with **action
checks** (were the right tool calls made) and **communication checks** (natural-language assertions that
the agent said or elicited the right things). The headline finding: moving from a passive-user setting
to dual-control causes a large drop — frontier agents that score well on conventional domains fall
sharply on dual-control Telecom, exposing weak ability to guide a user.

## Format
Multi-turn conversation. The environment is a Dec-POMDP where a tool-constrained user simulator and the
agent both act on one shared state until the task is resolved or the dialogue ends.

## Limitations
The "user" is an LLM simulator; it is deliberately constrained by tools and observable state, but it is
not a real customer, so scores inherit the simulator's fidelity. The dual-control results are anchored
in the Telecom domain (airline and retail carry over from τ-bench), making the benchmark
customer-service-shaped — it says nothing about coding, browsing, or research agents. Compositionally
generated tasks can vary in difficulty, and like any state-match evaluation it rewards reaching the goal
state over the quality of the path taken there.

## Example
A Telecom customer reports mobile data is down. The correct trajectory has the agent diagnose using its
own read tools, recognize that the fix requires the *user's* device tools, walk the user through
toggling the relevant setting (e.g. resetting the access-point configuration), and then re-check the
shared state to confirm connectivity. An agent that tries to "fix" it entirely on its own side, or that
declares success without verifying the shared state, fails the run — and failing on even one of *k*
trials lowers its pass^k.
