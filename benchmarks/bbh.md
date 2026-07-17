---
id: benchmark/bbh
type: benchmark
slug: bbh
schema_version: 1.0
title: "BBH (BIG-Bench Hard)"
name: BBH
category: Knowledge & Academic Reasoning
description: "The 23 hardest BIG-Bench tasks where prior models failed to beat the average human."
what_is_it: "BIG-Bench Hard is a curated 23-task subset of the 200+-task BIG-Bench suite, chosen because earlier language models scored below the average human rater on them. Tasks span logical deduction, multi-step arithmetic, causal judgment, word manipulation, and more."
why_people_use_it: "It concentrates BIG-Bench's signal into the tasks that still discriminate, and it was the benchmark that popularized chain-of-thought prompting (CoT dramatically improved BBH scores). It is part of the HF Open LLM Leaderboard v2 suite."
examples:
  - "(Logical deduction) A is left of B. C is right of B. D is left of A. Order left to right? (correct: D, A, B, C)"
method: "3-shot, usually with chain-of-thought; a mix of multiple-choice and exact-match free-response tasks, scored per task then averaged."
metric: "Average accuracy across the 23 tasks (exact-match or multiple-choice per task)."
format: "3-shot chain-of-thought."
saturation: medium
limitations: "Highly heterogeneous — averaging across very different tasks can hide that a model fails badly on a few. Chain-of-thought is essentially required, so scores conflate reasoning ability with prompt formatting. Some tasks are small, adding variance."
citation: "https://arxiv.org/abs/2210.09261"
tags: [reasoning, chain-of-thought, mixed-format]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2210.09261", title: "Challenging BIG-Bench Tasks and Whether Chain-of-Thought Can Solve Them (Suzgun et al., 2022)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2206.04615", title: "Beyond the Imitation Game (BIG-bench) (Srivastava et al., 2022)", trust: 0.9 }
related: [benchmark/mmlu-pro, benchmark/drop]
---

## What It Tests
A spread of hard reasoning skills — symbolic logic, multi-hop deduction, arithmetic word problems,
date and object tracking, sarcasm and causal judgment. The unifying theme is that pre-CoT models
underperformed humans on all of them.

## How It Scores
Each of the 23 tasks is scored on its own (exact match or multiple choice) and the results are
averaged. Because the tasks are so different, the per-task breakdown is far more informative than the
single average — a strong average can mask a task where the model scores near zero.

## Format
Three-shot with chain-of-thought is the canonical protocol; CoT is what makes BBH tractable and is
the reason the benchmark is famous. Without CoT, scores collapse.

## Limitations
The **heterogeneity** is the main caveat: aggregating logic puzzles with word-sorting and causal
judgment produces a number that is hard to interpret. CoT-dependence means the score partly measures
prompt engineering. Small per-task sizes add noise.

## Example
(Logical deduction) "A is to the left of B. C is to the right of B. D is to the left of A. What is
the left-to-right order?" Chaining the constraints gives D < A < B < C, so the answer is **D, A, B,
C**. A model that ignores the constraints and answers alphabetically ("A, B, C, D") fails.
