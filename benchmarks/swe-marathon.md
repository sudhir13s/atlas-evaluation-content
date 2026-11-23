---
id: benchmark/swe-marathon
type: benchmark
slug: swe-marathon
schema_version: 3.1
title: "SWE-Marathon"
name: SWE-Marathon
category: Code Generation
description: "A benchmark of 20 project-scale software-engineering tasks, such as library rewrites and product clones, that need agent runs of 2 to 10 hours."
what_is_it: "SWE-Marathon (Desai et al., arXiv, June 2026; 26 authors) is a benchmark of 20 long-horizon tasks curated from real open-source and research codebases. It targets work that resists single-test verification, in four families: library clones and reproductions (8 tasks), product clones (5), ML engineering (5) and algorithmic optimization (2). Each task has a unique executable environment, a human-written reference solution and multi-layer verification. The paper evaluates 1,300 trajectories: 13 agent-model configurations x 20 tasks x 5 trials."
why_people_use_it: "Current agent benchmarks mostly cover short tasks such as single pull requests or small tickets. SWE-Marathon measures planning, long-context understanding and memory use over multi-hour rollouts in large codebases; logged attempts average 27.2 million tokens. The authors name poor self-verification, self-reported infeasibility and premature termination as common failure modes."
method: "Agents run for a per-task wall-clock limit of 2 to 10 hours inside the task's executable environment, with visible development feedback. Final scoring uses hidden verifiers of six kinds: dense test suites, behavioural parity with a reference implementation, performance gates, deterministic replay, integrity checks and computer-use agentic verifiers; some product clones add a browser-based UX stage. Trajectories are audited afterwards for reward hacking."
metric: "pass@1 (resolved rate) = trials whose submission passes the task verifier / trials run."
format: "Agentic, ultra-long-horizon; one executable environment per task with a reference solution and hidden multi-layer verification."
limitations: "The set is small: 20 tasks, so a few tasks swing results. Long runs are expensive, which limits repeats. Some tasks need verifiers that go beyond unit tests, such as computer-use checks for product clones, and the authors report reward-hacking behaviour in 13.8% of rollouts."
citation: "https://arxiv.org/abs/2606.07682"
openness: open
openness_note: "The authors state that the benchmark, its evaluation code and agent trajectories are released at swe-marathon.org; hidden verifiers are withheld from agents during runs, and no licence for the tasks is stated."
tags: [coding, agent, ultra-long-horizon, project-scale]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2606.07682", title: "SWE-Marathon: Can Agents Autonomously Complete Ultra-Long-Horizon Software Work? (Desai et al., 2026)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2606.07682", title: "SWE-Marathon, full text (arXiv HTML)", trust: 0.95 }
related: [benchmark/frontierswe, benchmark/deepswe]
---

## What It Tests
Whether an agent can sustain **project-scale software work** over hours.

- **Library clones and reproductions (8)** — rebuild or port an existing library or tool.
- **Product clones (5)** — recreate an application, some graded through its user interface.
- **ML engineering (5)** — machine-learning systems work, such as fast GPU kernels.
- **Algorithmic optimization (2)** — hit a performance target on a defined problem.

## How It Scores
**pass@1** = trials that pass the task's hidden verifier / trials run.

- **Hidden verifiers** — test suites, parity with a reference, performance gates, deterministic replay, integrity checks and computer-use agents.
- **UX stage** — for some product clones the trial reward is the minimum of the deterministic and agentic stages.
- **Audit** — trajectories are reviewed afterwards for reward hacking.

## Format
20 tasks, each with its own executable environment and a wall-clock limit of **2 to 10 hours**. The paper ran 13 agent-model configurations, 5 trials each: 1,300 trajectories.

## Limitations
- **Small set.** 20 tasks.
- **Cost.** Multi-hour runs limit repeats.
- **Gaming risk.** Reward hacking appeared in 13.8% of rollouts.

## Example
From the paper's task list:

- **biofabric-rust-rewrite** — port a 70,000-line Java graph-visualization tool to Rust.
- **excel-clone** — build an Excel-like spreadsheet, with the user experience checked by an agentic verifier.
- **trimul-cuda** — implement AlphaFold-3's TriMul operation in Triton under a latency gate.

Each is scored by its hidden verifier; the agent sees only the visible development feedback while it works.
