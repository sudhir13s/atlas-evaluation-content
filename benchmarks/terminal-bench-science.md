---
id: benchmark/terminal-bench-science
type: benchmark
slug: terminal-bench-science
schema_version: 1.0
title: "Terminal-Bench-Science"
name: Terminal-Bench-Science (v0.1)
category: Science & Research
description: "A Stanford-led benchmark of 70 expert-written scientific research workflows that an agent completes in a terminal, each checked by its own tests."
what_is_it: "Terminal-Bench-Science extends Terminal-Bench from software tasks to science. Researchers contribute workflows from their own work in the life, physical, Earth, mathematical and engineering sciences. Release 0.1 has 70 tasks covering data analysis, statistical inference, simulation, optimization, theorem proving, image reconstruction, signal processing, inverse problems, sensor calibration, model fitting, classification and scientific machine learning. The project is hosted by Stanford University and the Laude Institute."
why_people_use_it: "It lets scientists, not model makers or data vendors, set the bar for scientific capability in AI. Tasks were calibrated during review to challenge the newest frontier models, and it is a continuous benchmark with regular releases. Artificial Analysis includes it in its Intelligence Index."
method: "An agent works in a terminal environment on a task and produces concrete artifacts such as analyses, simulations, proofs, code or data products. Reproducible task-specific tests grade them. A task passes only if its tests pass. Artificial Analysis runs three trials per task with the mini-swe-agent harness and a 1,000-step cap."
metric: "Resolution rate in percent: the share of task trials that pass every test. The official announcement reports it over three independent trials per task."
format: "Agentic, 0-shot, in a Harbor-style containerised terminal; one expert-written instruction per task and deterministic tests."
limitations: "Release 0.1 is the first of a continuous series and tasks will be added, retired or fixed, so cite the version. Vendor tables use different protocols: Google's evaluation methodology for Gemini 4 Neon reports its own mini-swe-agent run as an average reward over 5 attempts, while the official board reports resolution rate, so those numbers differ in kind. Selecting tasks that stay hard for current models means the pool shifts with the frontier used for calibration. Only 70 tasks are included, so per-domain splits are small."
citation: "https://terminal-bench-science.ai/announcement"
tags: [agent, science, terminal, research-workflows, harbor]
updated: 2026-11-21
sources:
  - { url: "https://terminal-bench-science.ai/announcement", title: "Terminal-Bench-Science 0.1 announcement", trust: 0.95 }
  - { url: "https://github.com/harbor-framework/terminal-bench-science", title: "harbor-framework/terminal-bench-science — repository (Apache-2.0)", trust: 0.9 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology", trust: 0.85 }
  - { url: "https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf", title: "Gemini 4 Neon model evaluation (Google DeepMind)", trust: 0.7 }
related: [benchmark/terminal-bench, leaderboard/artificial-analysis]
---

## What It Tests
Whether an agent can carry out a **real scientific workflow** from a researcher's own work.

- **Domains** — life, physical, Earth, mathematical and engineering sciences.
- **Task types** — analysis, simulation, optimisation, theorem proving, reconstruction, calibration, model fitting.
- **Origin** — tasks are contributed by practising scientists and reviewed by domain and technical experts.

Of 920 proposals, 464 were approved, 386 pull requests opened and only 70 tasks made release 0.1.

## How It Scores
Each task has **its own deterministic tests**; a task passes only if all of them pass.

- **Headline** — resolution rate, the share of trials that pass.
- **Official runs** — three independent trials per task across all 70 tasks.
- **Artificial Analysis** — 19 life, 17 physical, 17 mathematical, 9 engineering and 8 Earth-science tasks.

## Format
A containerised terminal task with an instruction and tests, run through Harbor. Tasks are versioned so past trials can be re-graded or re-run.

## Limitations
- **First release.** 0.1 is the first of a continuous benchmark, so cite the version.
- **Mixed protocols.** Google reports an average reward over 5 attempts from its own run; the official board reports resolution rate.
- **Frontier-calibrated.** Tasks were selected to challenge the models available at release time.

## Example
Task files live in the project repository, `harbor-framework/terminal-bench-science`. A task has the same shape throughout: an expert-written instruction, a terminal environment, and tests that inspect the produced artifact, for instance an analysis or simulation output, against **reproducible expectations**.
