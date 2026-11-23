---
id: benchmark/terminal-bench-science
type: benchmark
slug: terminal-bench-science
schema_version: 4.0
title: Terminal-Bench-Science
name: Terminal-Bench-Science (v0.1)
category: Science & Research
description: A Stanford-led benchmark of 70 expert-written scientific research workflows that an agent completes in a terminal, each checked by its own tests.
what_is_it: Terminal-Bench-Science extends Terminal-Bench from software tasks to science. Researchers contribute workflows from their own work in the life, physical, Earth, mathematical and engineering sciences. Release 0.1, announced on 27 August 2026, has 70 tasks covering data analysis, statistical inference, simulation, optimization, theorem proving, image reconstruction, signal processing, inverse problems, sensor calibration, model fitting, classification and scientific machine learning. It is led by researchers at Stanford University (project leads Steven Dillmann, Sanmi Koyejo and Ludwig Schmidt), built by the Terminal-Bench team and hosted by Stanford and the Laude Institute, with 376 contributors from 22 countries.
why_people_use_it: It lets practising scientists set the bar for scientific capability in AI, and it is run as a continuous benchmark with regular releases. Tasks and their tests are public, so a result can be rerun. Artificial Analysis includes it in its Intelligence Index.
examples:
- 'From the public task internal-tide-flux: write a reusable Python script that analyses one ocean mooring deployment from raw instrument records and outputs tidal harmonic constants and baroclinic energy-flux vectors as JSON, scored on withheld deployments.'
method: An agent works in a containerised terminal environment on a task and produces concrete artifacts such as analyses, simulations, proofs, code or data products. Reproducible task-specific tests grade them, and a task passes only if its tests pass. The official 0.1 results use three independent trials per task. Artificial Analysis runs its own version with the mini-swe-agent harness, three repeats per task and a 1,000-step cap.
metric: 'Resolution rate in percent: passing trials / all task trials, over three independent trials per task in the official announcement.'
format: Agentic, 0-shot, in a Harbor containerised terminal; one expert-written instruction per task and deterministic tests.
limitations: Release 0.1 is the first of a continuous series, and the repository already holds more tasks than the release (96 in its tasks folder), so cite the version. Selecting tasks that stay hard for current models means the pool shifts with the frontier used for calibration. Only 70 tasks are included, so per-domain splits are small (8 to 19 tasks). Vendor tables may run their own protocol rather than the official one, so check how a number was produced.
citation: https://terminal-bench-science.ai/announcement
openness: open
openness_note: Task instructions, environments and tests are public in the harbor-framework/terminal-bench-science repository under the Apache-2.0 licence; some verifiers score on withheld data inputs, but the tasks themselves are published and rerunnable.
tags:
- agent
- science
- terminal
- research-workflows
- harbor
updated: '2026-11-23'
sources:
- url: https://terminal-bench-science.ai/announcement
  title: Terminal-Bench-Science 0.1 announcement
  trust: 0.95
- url: https://github.com/harbor-framework/terminal-bench-science
  title: harbor-framework/terminal-bench-science — repository (Apache-2.0)
  trust: 0.9
- url: https://raw.githubusercontent.com/harbor-framework/terminal-bench-science/main/tasks/earth-sciences/ocean-sciences/internal-tide-flux/instruction.md
  title: Terminal-Bench-Science task internal-tide-flux — instruction
  trust: 0.9
- url: https://artificialanalysis.ai/methodology/intelligence-benchmarking
  title: Artificial Analysis Intelligence Benchmarking Methodology
  trust: 0.85
related:
- benchmark/terminal-bench
- leaderboard/artificial-analysis
---

## What It Tests

Whether an agent can carry out a **real scientific workflow** from a researcher's own work.

- **Domains** — life, physical, Earth, mathematical and engineering sciences.
- **Task types** — analysis, simulation, optimisation, theorem proving, reconstruction, calibration, model fitting.
- **Origin** — tasks are contributed by practising scientists and reviewed by domain and technical experts.

**Selection funnel for 0.1:** 920 proposals, 464 approved, 386 pull requests opened, **70 tasks** accepted (7.6%).

| Domain | Tasks in 0.1 |
|---|---|
| Life sciences | 19 |
| Physical sciences | 17 |
| Mathematical sciences | 17 |
| Engineering sciences | 9 |
| Earth sciences | 8 |

## How It Scores

Each task has **its own reproducible tests**; a task passes only if all of them pass.

- **Resolution rate** = passing trials / all trials, in percent.
- **Official runs** — three independent trials per task across all 70 tasks.
- **Artificial Analysis** — its own run with the mini-swe-agent harness, three repeats and a 1,000-step cap.

## Format

A containerised terminal task with an instruction and tests, run through **Harbor**, which also lets past trials be re-graded or re-run.

- **Repository** — `harbor-framework/terminal-bench-science`, **Apache-2.0**, tasks grouped by domain folder.
- **Contributions** — a Propose, Build, Review flow for each new task.

## Limitations

- **First release.** 0.1 is the first of a continuous benchmark; the repository already holds more tasks, so cite the version.
- **Frontier-calibrated.** Tasks were selected to challenge the models available at release time.
- **Small splits.** 70 tasks in total, 8 to 19 per domain.
- **Mixed protocols.** Vendor tables may use their own harness and attempt counts.

## Example

From the public task `internal-tide-flux` in the ocean-sciences folder (paraphrased from its instruction file):

- **Input** — one mooring deployment: raw current-meter, ADCP (acoustic Doppler current profiler), thermistor, CTD (conductivity, temperature, depth) and sea-level records, with no quality control and possible clock offsets.
- **Task** — write `/app/tidal_flux.py`, a reusable script using only the standard library, numpy and scipy.
- **Output** — JSON with tidal harmonic constants for the M2, S2 and K1 constituents and depth-integrated energy-flux vectors.
- **Grading** — the script is run on **withheld deployments**; amplitude, phase and flux-vector errors must each fall within a tolerance.
