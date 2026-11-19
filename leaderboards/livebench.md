---
id: leaderboard/livebench
type: leaderboard
slug: livebench
schema_version: 2.1
title: LiveBench
name: LiveBench
maintainer: Abacus.AI + NYU + Meta
url: https://livebench.ai
tagline: Contamination-free by design — questions refresh every month.
update_cadence: Monthly
coverage: ~40 models
free_access: true
tags:
- leaderboard
updated: '2026-11-19'
sources:
- url: https://livebench.ai
  title: LiveBench
---

## What It Ranks

A benchmark whose questions are drawn from datasets, math competitions, code problems, and news articles released AFTER each model's training cutoff. New questions added monthly. Old questions retired. No model can have seen the current test set during training.

### Why You Need It

Contamination is the biggest quiet flaw in static benchmarks — a model may have seen the test questions during training, which inflates scores. LiveBench is designed to make that impossible.

### Why It Matters

If your ranking order on LiveBench disagrees with a static benchmark, the static one is probably contaminated. This is the cleanest signal of raw capability available today.

## How To Read It

### Methodology

- Monthly refresh — new questions from post-cutoff sources
- Categories: Reasoning, Coding, Math, Data Analysis, Language, Instruction Following
- Ground-truth answers (not LLM-as-judge)
- Scores decay as models are re-evaluated on new question sets

### Strengths

- Contamination effectively eliminated by design
- Ground-truth scoring — no judge-model bias
- Published methodology paper (Oct 2024)
- Free, reproducible, open harness

### Best For

- Honest reasoning ranking
- Sanity-check against MMLU/GPQA claims
- Research on contamination effects

## Caveats

- Smaller sample size per category than MMLU-style boards
- Category weighting is a choice, not a consensus
- New model coverage slightly slower than Arena
- Score trends across months are not directly comparable (different questions)
