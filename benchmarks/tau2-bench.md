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
tags: [agent, tool-use, multi-turn, user-simulation, customer-service]
updated: 2026-07-20
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
