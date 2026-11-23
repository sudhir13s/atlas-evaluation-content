---
id: benchmark/vamos-bench
type: benchmark
slug: vamos-bench
schema_version: 4.0
title: VAmoS Bench
name: VAmoS Bench (Voice Agent Simulation Bench, including VAmoS Pro Bench)
category: Voice & Speech
description: A Veris AI benchmark that runs complete voice-agent systems through simulated phone calls and grades what the agent did, not only what it said.
what_is_it: VAmoS Bench, the Voice Agent Simulation Bench (paper by Joshua Meyer and colleagues, July 2026; leaderboard run by Veris AI), measures whole voice-agent systems end to end on a stateful customer-support task. The agent, Riley, is a credit-card support representative for a fictional bank who can freeze, cancel, replace or activate a card. There are 100 scenarios; each gives a simulated caller a private goal and a seeded PostgreSQL database, and roughly one third of callers apply adversarial pressure. The agent has five tools that run real SQL. VAmoS Pro Bench is a second, harder set of 100 tasks for utility billing and payment assistance.
why_people_use_it: 'Voice-agent benchmarks usually measure component quality such as word error rate, latency and turn-taking. VAmoS measures whether the call was handled correctly, which the paper ties to containment: the share of calls the automated system resolves without handing off to a human. It compares whole voice stacks on one task, and new implementations of Riley can be submitted to Veris.'
method: 'A simulation platform, operated by the authors'' organisation, opens an isolated call in which the simulated caller reaches the agent over audio. Each scenario defines binary assertions. A grader checks them against the full trace: what was said, and the tool calls with arguments and returned rows. This catches an agent that claims a card was changed without updating the database.'
metric: Task completion in percent = calls on which every scenario assertion passes / calls attempted; calls without a verdict count as misses. The board also shows connect rate, latency, turns, barge-ins per call and cost per call.
format: 'Simulated phone calls over audio, with a graded trace. Pro: every call chains two to four caller requests, over street, cafe or television noise, with a calm or angry caller; every world effect and spoken figure is graded against the trace.'
limitations: The scenarios and the simulator are not published, so nobody outside the maker can rerun the benchmark; only the Riley agent implementations are public. The paper treats row order as a snapshot of observed performance, not a definitive ranking, because of run-to-run variability. The first version covers one financial-services task, and the paper's evaluation compares eleven systems while the live board can list more, so counts depend on the date. The maker runs the benchmark on its own platform. No data licence or downloadable score file is offered.
citation: https://arxiv.org/abs/2607.27453
openness: closed
openness_note: The paper describes the method and the Riley agent code is on GitHub, but the 100 scenarios, their assertions and the simulator are not published, and runs are done by Veris AI.
tags:
- voice
- agent
- customer-support
- simulation
- veris-ai
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2607.27453
  title: 'VAmoS Bench: Voice Agent Simulation Bench (arXiv:2607.27453)'
  trust: 0.95
- url: https://arxiv.org/html/2607.27453
  title: VAmoS Bench paper, full text (arXiv HTML)
  trust: 0.95
- url: https://veris.ai/leaderboard/vamos
  title: VAmoS Bench — Veris AI leaderboard
  trust: 0.95
- url: https://veris.ai/leaderboard/vamos-pro
  title: VAmoS Pro Bench — Veris AI leaderboard
  trust: 0.95
- url: https://github.com/veris-ai/riley-agent
  title: veris-ai/riley-agent — agent implementations
  trust: 0.9
related:
- benchmark/tau2-bench
- benchmark/tau-voice
- benchmark/voicebench
---

## What It Tests

The scenarios are **not published**: the maker describes the task and posts results, and runs submitted agents itself. It tests whether a **voice agent resolves a phone call correctly** by saying and doing the right things.

- **Stateful task** — freeze, cancel, replace or activate a credit card in a seeded database.
- **Pressure** — roughly one third of callers push adversarially.
- **Right outcome** — sometimes the right result is following policy rather than doing what the caller asks.

**Who built it:** Joshua Meyer, Sahar Shayegan, Ritiz Tambi, Ali Khan, Sun Kim, Victor Shih, Mehdi Jamei and Andi Partovi (arXiv, 29 July 2026); Veris AI hosts the leaderboard.

## How It Scores

**Binary assertions** per scenario, graded on the whole trace.

- **Formula** — task completion = calls where every assertion passes / calls attempted; calls with no verdict count as failures.
- **Said vs done** — tool calls, arguments and returned rows are checked, not only the transcript.
- **Side metrics** — connect rate, latency, turns, barge-ins (caller interruptions) per call and cost per call.
- **Pro set** — also grades every spoken figure.

## Format

Audio calls into an isolated simulation.

- **Tools** — five, described by function: look up a user, find a card by its last four digits, change a card's status, request a replacement, update delivery status.
- **Any stack** — the agent can run on any framework, platform or model; each board row is one implementation of Riley.
- **Paper's systems** — eleven, from voice-agent frameworks such as Pipecat and LiveKit Agents to realtime speech models such as OpenAI Realtime.

## Limitations

- **Closed scenarios.** Only the agent code is public; results cannot be reproduced outside Veris.
- **Not a resolved ranking.** The paper says neighbouring rows are not clearly ordered.
- **One domain at first.** Version one is financial services; Pro covers utility billing.
- **Maker-run.** The authors' organisation operates the simulation platform that produces the scores.

## Example

As the paper describes one **adversarial scenario**:

- **Caller goal** — the caller has lost an active card and wants a replacement.
- **Pressure** — the caller pushes the agent to skip the required freeze.
- **Assertion** — the call passes only if the agent keeps the policy order, freezing the card before replacing it, as shown by the tool calls in the trace.

No scenario text is reproduced here because the maker does not publish the scenarios.
