---
id: benchmark/tau-voice
type: benchmark
slug: tau-voice
schema_version: 4.0
title: tau-Voice
name: tau-Voice (τ-Voice)
category: Voice & Speech
description: 'A Sierra benchmark extending tau2-bench to full-duplex voice agents: 278 grounded tasks in retail, airline and telecom, with noise and accents.'
what_is_it: 'τ-Voice evaluates full-duplex voice agents, which listen and speak at the same time, on grounded customer-service tasks. It was introduced in March 2026 by Soham Ray, Keshav Dhandhania and Victor Barres (Sierra) with Karthik Narasimhan (Princeton University). It extends τ²-bench into a voice benchmark with 278 tasks: retail 114, airline 50 and telecom 114. A controllable voice user simulator supplies diverse accents, realistic audio environments and turn-taking behaviour such as interruptions and backchannels.'
why_people_use_it: Earlier evaluations measured conversational dynamics and task completion separately. The authors say τ-Voice combines verifiable completion of complex grounded tasks, full-duplex interaction and realistic audio, and it allows direct comparison between voice and text agents on the same tasks. Decoupling the simulation from wall-clock time lets the user simulator use the most capable language model without real-time constraints.
examples:
- 'From the paper''s Figure 4: a retail customer calls about exchanging a jigsaw puzzle and correcting their delivery address.'
method: Voice agents talk with the simulated user over telephone-quality audio (G.711 at 8 kHz) under a Clean condition and a Realistic condition with accents, background and burst noise, frame drops, muffling and non-directed speech. Task success is checked deterministically by comparing the environment's end state, such as database records, with a gold standard; the agent's required communications are checked by an LLM evaluator. The simulation advances in 200 ms ticks.
metric: 'Task completion as pass@1 (share of tasks solved in one run), plus four voice interaction scores: Latency, Responsiveness, Interrupt and Selectivity. The paper also compares voice results with a text agent on the same tasks.'
format: Real-time, full-duplex voice conversations; 278 tasks in three domains; verifiable task outcomes in a grounded environment.
limitations: It builds on τ²-bench's tasks, so those domains and policies shape what is tested. Results differ between Clean and Realistic audio conditions, so cite the condition. The failure analysis covers 91 failed simulations in the authors' own setup, so its split between agent and simulator errors may not generalise. Communication checks rely on an LLM evaluator.
citation: https://arxiv.org/abs/2603.13686
openness: open
openness_note: The paper is CC BY 4.0 and the voice tasks, simulator and scoring code ship in the public sierra-research/tau2-bench repository under the MIT licence; no held-out test set is described.
tags:
- voice
- agent
- full-duplex
- customer-service
- sierra
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2603.13686
  title: 'τ-Voice: Benchmarking Full-Duplex Voice Agents on Real-World Domains (arXiv:2603.13686)'
  trust: 0.95
- url: https://arxiv.org/html/2603.13686v1
  title: τ-Voice, full text (arXiv HTML)
  trust: 0.95
- url: https://github.com/sierra-research/tau2-bench
  title: sierra-research/tau2-bench — repository with the voice tasks (MIT licence)
  trust: 0.9
related:
- benchmark/tau2-bench
- benchmark/vamos-bench
---

## What It Tests

Whether a **voice agent completes grounded tasks** while talking and listening at once.

- **Domains** — retail (114 tasks), airline (50) and telecom (114).
- **Interaction** — multi-turn, policy-bound, with interruptions, backchannels ("mm-hm") and speech not aimed at the agent ("hold on").
- **Audio** — a Clean and a Realistic condition.

| Condition | What the simulated caller sounds like |
|---|---|
| Clean | American personas, no noise, no frame drops, no interruptions |
| Realistic | Diverse accents, indoor and outdoor noise, burst noise, about 2% frame drops, muffling, coughs, interruptions |

## How It Scores

**Pass@1 task completion** = tasks whose end state matches the gold standard / all tasks.

- **End-state check** — deterministic comparison of the environment (for example database records) with the expected result.
- **Communication check** — an LLM evaluator judges whether the agent told the user what it had to.
- **Voice quality** — four aggregate scores: Latency, Responsiveness, Interrupt and Selectivity.
- **Voice versus text** — the same tasks run with a text agent show the gap.

## Format

278 tasks driven by a voice user simulator with controllable accents and audio environments, over telephone-quality audio.

- **Code and tasks** — in the `sierra-research/tau2-bench` repository (**MIT licence**), listed there as the `voice-v1` set.
- **Paper** — arXiv:2603.13686, submitted 14 March 2026.

## Limitations

- **Inherited scope.** Domains and policies come from τ²-bench.
- **Condition-specific.** Clean and Realistic audio give different results.
- **Own failure analysis.** 91 failed simulations, attributed mostly to the agent in the authors' setup.
- **LLM evaluator.** Communication checks use a model, not string matching.

## Example

From the paper's Figure 4 and Appendix F (retail domain, task 41):

- **Caller** — a customer phones in with **two problems**: exchanging a 1000-piece jigsaw puzzle they ordered, and **correcting their address**.
- **Agent's job** — follow the retail policy over audio and make the exchange and the address change in the environment.
- **Grading** — the final database state is compared with the gold standard; the same task can be given to a text agent for comparison.
