---
id: evaluation-metric/task-success-rate
type: evaluation-metric
slug: task-success-rate
schema_version: 4.0
title: Task Success Rate (WebArena)
name: Task Success Rate
category: agent
measures: A high score means the agent completed a large share of tasks, judged by checks on its final answer or on the end state of the environment.
needs:
- task intent
- agent trajectory or final answer
- task-specific checks
judged_by: code
direction: higher-is-better
defined_in:
  title: 'WebArena: A Realistic Web Environment for Building Autonomous Agents (Zhou et al., 2023)'
  url: https://arxiv.org/abs/2307.13854
tags:
- agent
- web-agent
- functional-correctness
updated: '2026-11-21'
sources:
- url: https://arxiv.org/abs/2307.13854
  title: 'WebArena: A Realistic Web Environment for Building Autonomous Agents (arXiv:2307.13854)'
  trust: 0.95
- url: https://arxiv.org/html/2307.13854
  title: WebArena (HTML version)
  trust: 0.95
---

## What It Measures

Task success rate, **as WebArena defines it**, scores an agent on **functional correctness**: whether a task was actually accomplished, not whether the agent followed a reference sequence of actions.

## How It Is Computed

Each task has its own reward function returning success or failure:

- **Information-seeking tasks:** the agent's answer is compared with an annotated answer by `exact_match`, `must_include` (the answer contains the reference) or `fuzzy_match` (a language model, gpt-4-0613 in the paper, judges semantic equivalence).
- **Navigation and content or configuration tasks:** a programmatic check locates the relevant content in the environment's end state and applies `exact_match` or `must_include` to it.
- **Unachievable tasks:** labelled N/A; the agent is expected to say so.
- **Success rate (SR):** the percentage of tasks completed, also reported separately for achievable and unachievable tasks.

## Limits

- **Human baseline:** the authors note that human performance may differ for annotators from other backgrounds; their annotators completed 78.24% of sampled tasks.
- **Fuzzy matching** rests on a language-model judge, validated in the paper on a small set of examples.
