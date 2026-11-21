---
id: benchmark/vamos-bench
type: benchmark
slug: vamos-bench
schema_version: 1.0
title: "VAmoS Bench"
name: VAmoS Bench (Voice Agent Simulation Bench, including VAmoS Pro Bench)
category: Agent & Tool Use
description: "A Veris AI benchmark that runs complete voice-agent systems through simulated phone calls and grades what the agent did, not only what it said."
what_is_it: "VAmoS Bench, the Voice Agent Simulation Bench, measures whole voice-agent systems end to end on a stateful customer-support task. The agent, Riley, is a credit-card support representative for a fictional bank who can freeze, cancel, replace or activate a card. There are 100 scenarios; each gives a simulated caller a private goal and a seeded PostgreSQL database, and about one third of callers apply adversarial pressure. The agent has five tools that run real SQL. VAmoS Pro Bench is Veris AI's second, harder set for utility billing and payment assistance."
why_people_use_it: "Voice-agent benchmarks usually measure component quality such as word error rate, latency and turn-taking. VAmoS measures whether the call was handled correctly: the paper calls this 'containment'. It compares cascaded, speech-to-speech and hybrid stacks on one task. The board is a living one where new implementations of Riley can be submitted."
method: "A simulation platform opens an isolated call in which the caller reaches the agent over audio. Each scenario defines binary assertions. A grader checks them against the full trace: what was said, and the tool calls with arguments and returned rows. This catches an agent that claims a card was changed without updating the database, and one that changes the right record but discloses protected information."
metric: "Task completion in percent: the share of calls where the scenario's assertions pass. The board also shows connect rate, latency, turns, barge-ins per call and cost per call."
format: "Simulated phone calls over audio, with a graded trace. Pro: every call chains two to four caller requests, over street, cafe or television noise, with a calm or angry caller; every world effect and spoken figure is graded against the trace."
limitations: "The paper cautions that, given run-to-run variability and unmeasured scenario-set uncertainty, neighbouring rows are not a resolved ranking. The first version covers one financial-services task, and the paper's evaluation compares eleven systems while the live board lists more, so counts depend on the date. Veris AI sells simulation and testing products, so it has a commercial interest. Veris's own pages give different stack counts for the Pro set, so no count is given. Veris states no data licence and offers no downloadable score file."
citation: "https://arxiv.org/abs/2607.27453"
tags: [voice, agent, customer-support, simulation, veris-ai]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2607.27453", title: "VAmoS Bench: Voice Agent Simulation Bench (arXiv:2607.27453)", trust: 0.95 }
  - { url: "https://veris.ai/leaderboard/vamos", title: "VAmoS Bench — Veris AI leaderboard", trust: 0.9 }
  - { url: "https://veris.ai/leaderboard/vamos-pro", title: "VAmoS Pro Bench — Veris AI leaderboard", trust: 0.9 }
related: [benchmark/tau2-bench]
---

## What It Tests
Whether a **voice agent resolves a phone call correctly** by saying and doing the right things.

- **Stateful task** — freeze, cancel, replace or activate a credit card in a seeded database.
- **Pressure** — roughly one third of callers push adversarially.
- **Right outcome** — sometimes the right result is a refusal or a redirect.

## How It Scores
**Binary assertions** per scenario, graded on the whole trace.

- **Said vs done** — tool calls, arguments and returned rows are checked, not only the transcript.
- **Metric** — task completion, with latency, cost and barge-in rate beside it.
- **Pro set** — also grades every spoken figure.

## Format
Audio calls into an isolated simulation. The agent runs on any framework, platform or model; each board row is one implementation of Riley. Veris says it will run submitted implementations through the same scenarios.

## Limitations
- **Not a resolved ranking.** The paper says neighbouring rows are not clearly ordered.
- **One domain at first.** Version one is financial services; Pro covers utility billing.
- **Vendor-run.** Veris AI runs the benchmark and sells related products.
- **Counts vary.** Veris's pages give different Pro stack counts, so none are quoted.

## Example
Riley receives a call from a simulated cardholder whose private goal is to freeze a card. The scenario's assertions check that the card's **database row** really changed, and that no other customer's card was touched. A grader reads the trace, including the SQL calls. An agent that says 'your card is frozen' without updating the database fails, and so does one that updates the right row but reads out protected information.
