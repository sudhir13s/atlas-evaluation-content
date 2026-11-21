---
id: benchmark/swe-marathon
type: benchmark
slug: swe-marathon
schema_version: 1.0
title: "SWE-Marathon"
name: SWE-Marathon
category: Code Generation
description: "A benchmark of 20 project-scale software-engineering tasks, such as library reproduction and full-stack app cloning, that need multi-hour agent rollouts."
what_is_it: "SWE-Marathon is a benchmark of 20 long-horizon tasks curated from real open-source and research codebases. Instead of lengthening existing patch tasks, it targets categories that resist single-test verification: full library reproduction, full-stack application cloning, ML systems and post-training, and algorithmic optimization. Each task has a unique executable environment, a human-written reference solution and multi-layer verification. The paper evaluates 1,300 trajectories."
why_people_use_it: "Current agent benchmarks mostly cover short tasks such as single pull requests, small tickets or 5-to-10-minute exercises. SWE-Marathon measures planning, long-context understanding and memory use over multi-hour rollouts and large codebases. The authors note that failures often involve weak self-verification, poor recovery, premature termination, or attempts to exploit the evaluation environment."
method: "Agents run for multiple hours inside a task's executable environment, and layered checks verify the result. The paper reports that some failures were attempts to exploit the evaluation environment."
metric: "Pass@1 across agent-model configurations; the paper reports the share of attempts that pass."
format: "Agentic, ultra-long-horizon; one executable environment per task with a reference solution and multi-layer verification."
limitations: "The set is small: 20 tasks, so a few tasks swing results. Long runs are expensive, which limits repeats. Some tasks need verifiers that go beyond unit tests, such as computer-use checks for full-stack apps, and any verifier can be gamed, which is why the authors discuss exploit attempts."
citation: "https://www.swe-marathon.org/swe-marathon-paper.pdf"
tags: [coding, agent, ultra-long-horizon, project-scale]
updated: 2026-11-21
sources:
  - { url: "https://www.swe-marathon.org/swe-marathon-paper.pdf", title: "SWE-Marathon (paper, swe-marathon.org)", trust: 0.9 }
related: [benchmark/frontierswe, benchmark/deepswe]
---

## What It Tests
Whether an agent can sustain **project-scale software work** over hours.

- **Library reproduction** — rebuild an existing library.
- **Full-stack cloning** — recreate an application.
- **ML systems and post-training** — engineering around model training.
- **Algorithmic optimization** — improve performance on a defined problem.

## How It Scores
**Multi-layer verification** per task, with a human-written reference solution.

- **Pass@1** — the paper's headline across 1,300 trajectories.
- **Failure modes** — weak self-verification, poor recovery, early stopping, environment exploits.

## Format
20 tasks, each with its own executable environment. Rollouts take multiple hours across large codebases and multi-stage objectives.

## Limitations
- **Small set.** 20 tasks.
- **Cost.** Multi-hour runs limit repeats.
- **Gaming risk.** The authors report agents trying to exploit evaluation environments.

## Example
The four categories are the examples the paper gives: a task might ask the agent to **reproduce a whole library** or **clone a full-stack application** from scratch, with layered checks and a human reference solution defining what counts as done.