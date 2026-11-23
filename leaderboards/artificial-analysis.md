---
id: leaderboard/artificial-analysis
type: leaderboard
slug: artificial-analysis
schema_version: 4.0
title: Artificial Analysis
name: Artificial Analysis
maintainer: Artificial Analysis
url: https://artificialanalysis.ai
tagline: Its own evaluations of models, shown next to cost, tokens and speed.
ranks: language models by an Intelligence Index, plus cost, tokens and speed
benchmarks:
- benchmark/automationbench
- benchmark/gdp-pdf
- benchmark/frontiercode
- benchmark/terminal-bench
- benchmark/terminal-bench-science
- benchmark/critpt
- benchmark/aa-omniscience
- benchmark/hle-diamond
update_cadence: Not stated
free_access: true
score_origin: independent
tags:
- leaderboard
- composite-index
- independent
- agentic
updated: '2026-11-21'
sources:
- url: https://artificialanalysis.ai
  title: Artificial Analysis
  trust: 0.85
- url: https://artificialanalysis.ai/about
  title: About Artificial Analysis
  trust: 0.85
- url: https://artificialanalysis.ai/methodology/intelligence-benchmarking
  title: Artificial Analysis Intelligence Benchmarking Methodology (Index v5.0.1)
  trust: 0.9
- url: https://artificialanalysis.ai/evaluations/mka
  title: MKA benchmark — Artificial Analysis evaluation page
  trust: 0.85
related:
- benchmark/automationbench
- benchmark/gdp-pdf
- benchmark/frontiercode
- benchmark/terminal-bench
- benchmark/terminal-bench-science
- benchmark/critpt
- benchmark/aa-omniscience
- benchmark/hle-diamond
- benchmark/mka
- benchmark/gdpval
- benchmark/harvey-lab
- benchmark/healthbench
- benchmark/mmmu-pro
---

## What It Ranks

Artificial Analysis, which calls itself the independent benchmarking company for AI, runs its own evaluations of models and publishes the scores next to cost per task, tokens used and speed. Its headline is the Intelligence Index, currently v5.0.1.

## How To Read It

### Intelligence Index v5.0.1

A **weighted average of nine evaluations in four categories**, 25% each. The methodology page describes it as primarily text-based and English.

- **Knowledge Work (25%)** — AA-Briefcase v1.1 (15%), AutomationBench-AA (5%), GDP.pdf (5%).
- **Coding (25%)** — FrontierCode-AA 2.0 (15%), Terminal-Bench 4.0 (10%).
- **Science (25%)** — Terminal-Bench-Science 0.1 (15%), CritPt v2 Hard Subset (10%).
- **General (25%)** — AA-Omniscience v1.1 (15%, split 10% accuracy and 5% non-hallucination rate), HLE-Diamond (10%).

Only some components have their own records here; AA-Briefcase does not.

### Methodology

- The methodology page describes Artificial Analysis' own implementation of each evaluation
- Temperature 0 for non-reasoning models and 0.6 for reasoning models, unless the lab recommends another
- Several agentic evaluations use the open-source Stirrup harness, and the terminal-based ones use mini-swe-agent
- Evaluations outside the Index, such as MKA and GDPval-AA, are reported separately
- The version history lists dated Index releases; the page states no update schedule

### Best For

- Comparing models on the Intelligence Index alongside cost, tokens and speed

## Caveats

- An aggregate score does not show where a model is weak
- Benchmark selection and weights are the board's own choice, and they change by version; for example, the Index uses Terminal-Bench 4.0 while the benchmark has reached 4.1
- Several components are private or Artificial Analysis variants of public benchmarks, so their numbers differ from the makers' own boards
- Vendor tables quote Index scores at specific thinking levels; check the level
