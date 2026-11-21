---
id: benchmark/agentdojo
type: benchmark
slug: agentdojo
schema_version: 1.0
title: "AgentDojo"
name: AgentDojo
category: Truthfulness & Safety
description: "An extensible environment for testing prompt-injection attacks and defenses on tool-using agents: 97 realistic tasks and 629 security test cases."
what_is_it: "AgentDojo is an evaluation framework for agents that run tools over untrusted data, where tool output can hijack the agent into a malicious task. It is a dynamic environment, not a static test suite, designed for new agent tasks, defenses and adaptive attacks. The environment holds 97 realistic tasks, such as managing an email client, navigating an e-banking website or making travel bookings, 629 security test cases, and attack and defense methods from the literature. The authors are at ETH Zurich and Invariant Labs."
why_people_use_it: "Prompt injection is a central risk for agents, and static suites age as attacks and defenses evolve. AgentDojo lets researchers add tasks, defenses and attacks. The authors report that it challenges both sides: models fail many tasks even without attacks, and existing injection attacks break some security properties but not all. US and UK AI safety institutes used it to show a model's vulnerability to prompt injection."
method: "Tasks are grouped into suites, and the benchmark script runs a chosen model with a chosen defense and attack, for example the tool filter defense and an attack with tool knowledge. Results are gathered per suite and task."
metric: "Task success and attack outcomes: whether the agent completes the user's task and whether an injected malicious goal succeeds, checked by the security test cases."
format: "Tool-using agent suites such as workspace; tasks with injection test cases; run from a command-line script with model, defense and attack options."
limitations: "The environment is extensible, so task and test counts describe the published release, and the package API is marked as under development. Attack coverage is limited to the paper's attack paradigms unless users add new ones. The maker's site says its package may change, so pin the version when comparing."
citation: "https://arxiv.org/abs/2406.13352"
tags: [safety, prompt-injection, agents, security, eth-zurich]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2406.13352", title: "AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents (arXiv:2406.13352)", trust: 0.95 }
  - { url: "https://agentdojo.spylab.ai", title: "AgentDojo — project site", trust: 0.9 }
related: [benchmark/gray-swan-ipi-arena, benchmark/agentharm]
---

## What It Tests
How well agents **resist prompt injection** hidden in data returned by their tools.

- **Tasks** — 97 realistic ones, such as email, e-banking and travel booking.
- **Security cases** — 629 test cases.
- **Extensible** — new tasks, defenses and adaptive attacks can be added.

## How It Scores
**Task outcomes and security test cases.**

- **Without attack** — does the agent complete the user's task?
- **Under attack** — does the injected malicious goal succeed?
- **Runner** — a script combines a model, a defense and an attack.

## Format
Suites such as workspace, each with user tasks and injection cases, run by `agentdojo.scripts.benchmark`.

## Limitations
- **Evolving environment.** The package API is still under development.
- **Attack coverage.** Limited to included attack paradigms unless extended.
- **Version pinning.** Needed for comparable results.

## Example
The authors' own examples: an agent **managing an email client**, **navigating an e-banking website** or **making travel bookings**. While doing the user's task it reads tool output that may contain a planted instruction; a security test case then checks whether the agent carried out that attacker-chosen action.
