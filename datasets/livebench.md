---
id: evaluation-dataset/livebench
type: evaluation-dataset
slug: livebench
schema_version: 1.0
title: "LiveBench"
name: "LiveBench"
category: Live / Contamination-Free
size: "Hundreds of questions across 6 categories, refreshed monthly"
url: "https://livebench.ai/"
description: "A contamination-free benchmark whose questions are drawn from sources released after model training cutoffs and refreshed on a schedule."
what_it_tests: "Reasoning, coding, math, data analysis, language, and instruction following — using freshly sourced questions (recent papers, competitions, news, datasets) so no model can have trained on them."
why_it_exists: "Contamination is the quiet flaw in static benchmarks: a model may have seen the test items during training, inflating scores. LiveBench eliminates this by continually sourcing new questions and retiring old ones, and by using ground-truth (not LLM-judge) scoring."
how_to_use: "Read the hosted leaderboard, or run the open harness. Scores use objective ground-truth answers, so there is no judge-model bias. Because question sets rotate monthly, compare models within the same snapshot, not across months."
known_issues: "Smaller sample per category than static giants like MMLU, so more variance. Category weighting is a design choice. New-model coverage is slightly slower than Arena. Scores across different monthly snapshots are not directly comparable (different questions)."
tags: [live, contamination-free, ground-truth, reasoning]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2406.19314", title: "LiveBench: A Challenging, Contamination-Free LLM Benchmark (White et al., 2024)", trust: 0.9 }
  - { url: "https://livebench.ai/", title: "LiveBench — leaderboard", trust: 0.85 }
related: [benchmark/gsm8k, benchmark/bbh]
---

## What It Is
A benchmark designed around contamination resistance: its questions come from sources published after
each model's training cutoff, new questions are added monthly, and old ones are retired. Scoring is
objective ground-truth, avoiding the judge-model bias of LLM-graded evals.

## How It Evaluates
Questions in six categories (reasoning, coding, math, data analysis, language, instruction following)
are scored against known correct answers — no model judge. As models are re-evaluated on newer
question sets, scores shift, so the leaderboard reflects performance on genuinely unseen items.

## What It Looks For
Raw capability uncontaminated by memorization. If a model's LiveBench ranking disagrees with its
static-benchmark ranking, the static benchmark is the more likely to be contaminated — that
cross-check is LiveBench's main value.

## Known Issues
Per-category samples are smaller than MMLU-scale sets, adding variance. The category weighting is a
curatorial choice, and new models appear a little slower than on Arena. Because questions rotate,
scores from different monthly snapshots measure different tests and should not be compared directly.
