---
id: benchmark/frontierswe
type: benchmark
slug: frontierswe
schema_version: 1.0
title: "FrontierSWE"
name: FrontierSWE (v2)
category: Code Generation
description: "A Proximal benchmark of ultra-long-horizon technical challenges, from performance engineering to ML research, where agents work up to 20 hours per task."
what_is_it: "FrontierSWE tests coding agents on the hardest ultra-long-horizon technical challenges, collected with academic and industry partners from domains including performance engineering, computational science and ML research. Examples named by the maker include optimizing compilers and training state-of-the-art protein-prediction models. Version 2 has 34 tasks, 21 of them new, such as decoding speech from MEG brain recordings, predicting ball trajectories from video and training weather-forecasting models."
why_people_use_it: "Most coding benchmarks end within minutes. FrontierSWE measures how far an agent gets on open-ended challenges that take many hours; Prime Intellect says agents run for about 11 hours per task on average and fail almost all of them. It is also available as a Prime Intellect Environment."
method: "Version 2 measures how far models get within 20 hours: agents are kept aware of their remaining time and encouraged to continue instead of submitting early. These features are built into proximus, a minimal coding-agent harness built for ultra-long-horizon tasks. The repository documents scoring in SCORING.md."
metric: "Task score, as defined in the repository's scoring document; vendor tables report a single percentage."
format: "Agentic, ultra-long-horizon; open-ended technical challenges with a 20-hour budget in version 2."
limitations: "Version 2 changed the methodology and task count, so version 1 and version 2 results are not comparable. Runs are long and costly, so few repeats are practical. The scoring details sit in the repository, and vendor tables give one number without the harness, so check how a model was run."
citation: "https://www.frontierswe.com/blog/v2"
tags: [coding, agent, ultra-long-horizon, research-engineering, proximal]
updated: 2026-11-21
sources:
  - { url: "https://www.frontierswe.com/blog/v2", title: "FrontierSWE v2 (Proximal blog)", trust: 0.95 }
  - { url: "https://github.com/Proximal-Labs/frontier-swe", title: "Proximal-Labs/frontier-swe — repository", trust: 0.9 }
  - { url: "https://www.primeintellect.ai/blog/frontier-swe", title: "Launching FrontierSWE on the Environments Hub (Prime Intellect)", trust: 0.8 }
related: [benchmark/swe-marathon, benchmark/deepswe]
---

## What It Tests
Whether an agent can make progress on **very long, open-ended engineering and research challenges**.

- **Domains** — performance engineering, computational science, ML research.
- **Version 2** — 34 tasks, with 21 new ones in new domains.
- **Time budget** — up to 20 hours per task.

## How It Scores
Scoring is defined in the repository's **SCORING.md**.

- **Harness** — proximus keeps agents aware of remaining time.
- **Early stops** — the harness encourages agents not to submit prematurely.

## Format
Open-ended technical challenges with real-world problems from academic and industry partners. Version 1 appeared four months before version 2.

## Limitations
- **Version break.** v1 and v2 differ in tasks and methodology.
- **Cost.** Hours of agent time per task.
- **Single numbers.** Vendor tables rarely give the harness.

## Example
The maker's examples include **optimizing a compiler**, **training a model for protein prediction**, **decoding speech from MEG brain recordings** and **predicting ball trajectories from video**. Each is an open-ended challenge the agent attacks for hours, with the final result scored by the task's own checks.
