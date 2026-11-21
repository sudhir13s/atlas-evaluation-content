---
id: benchmark/tau-voice
type: benchmark
slug: tau-voice
schema_version: 1.0
title: "tau-Voice"
name: tau-Voice (τ-Voice)
category: Voice & Speech
description: "A Sierra benchmark extending tau2-bench to full-duplex voice agents: 278 grounded tasks in retail, airline and telecom, with noise and accents."
what_is_it: "τ-Voice evaluates full-duplex voice agents, which listen and speak at the same time, on grounded tasks with real-world complexity. Agents must handle multi-turn conversations, follow domain policies and interact with an environment. It extends τ²-bench into a voice benchmark with 278 tasks across retail, airline and telecom domains. A controllable voice user simulator supplies diverse accents, realistic audio environments and rich turn-taking dynamics."
why_people_use_it: "Earlier evaluations measured conversational dynamics and task completion separately. The authors say τ-Voice is the first to combine verifiable completion of complex grounded tasks, full-duplex interaction and realistic audio, and it allows direct comparison between voice and text agents on the same tasks. Decoupling the simulator from wall-clock time lets it use the most capable language model without real-time constraints."
method: "Voice agents are run against the simulated user under clean conditions and under realistic conditions with noise and diverse accents. The paper also analyses failures, attributing most of them to agent behaviour instead of simulator artifacts."
metric: "Task completion (pass@1) and voice interaction quality; the paper also compares voice results with a text-agent score on the same tasks."
format: "Real-time, full-duplex voice conversations; 278 tasks in three domains; verifiable task outcomes in a grounded environment."
limitations: "It builds on τ²-bench's tasks, so those domains and policies shape what is tested. The paper is a March 2026 preprint. Results differ between clean and realistic audio conditions, so cite the condition. The failure analysis in the paper concerns its own setup, so it may not generalise to other evaluation setups."
citation: "https://arxiv.org/abs/2603.13686"
tags: [voice, agent, full-duplex, customer-service, sierra]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2603.13686", title: "τ-Voice: Benchmarking Full-Duplex Voice Agents on Real-World Domains (arXiv:2603.13686)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2603.13686v1", title: "τ-Voice, full text (arXiv HTML)", trust: 0.95 }
related: [benchmark/tau2-bench, benchmark/vamos-bench]
---

## What It Tests
Whether a **voice agent completes grounded tasks** while talking and listening at once.

- **Domains** — retail, airline and telecom.
- **Interaction** — multi-turn, policy-bound, with turn-taking dynamics.
- **Audio** — clean and realistic conditions, with noise and diverse accents.

## How It Scores
**Pass@1 task completion** plus voice interaction quality.

- **Voice vs text** — the same tasks allow a direct comparison.
- **Failure analysis** — the paper attributes most failures to agent behaviour.

## Format
278 tasks driven by a voice user simulator with controllable accents and audio environments. Code is in the `sierra-research/tau2-bench` repository.

## Limitations
- **Inherited scope.** Domains come from τ²-bench.
- **Condition-specific.** Clean and realistic audio give different results.
- **Preprint.** Dated March 2026.

## Example
A task puts a voice agent in a **retail, airline or telecom** support conversation: it must follow the domain's policy, talk with the simulated customer over audio and take the right actions in the environment. Completion is checked in that environment, and the same task can be run with a text agent to measure the voice-text gap.
