---
id: benchmark/math
type: benchmark
slug: math
schema_version: 1.0
title: "MATH"
name: MATH
category: Math & Logic
description: "12,500 competition mathematics problems (AMC/AIME level) with full worked solutions."
what_is_it: "A dataset of 12,500 problems from high-school math competitions (AMC, AIME and similar), spanning algebra, geometry, number theory, counting/probability, and precalculus, each labeled with a difficulty level 1–5 and a step-by-step solution. Far harder than GSM8K."
why_people_use_it: "It is the standard benchmark for genuine mathematical problem-solving beyond arithmetic. A widely reported subset, MATH-500, is used for faster, cleaner evaluation."
examples:
  - "Find the number of positive integers n <= 1000 such that 15n is a perfect square. (answer: 8, via n = 15m^2)"
method: "Few-shot (often 4-shot) chain-of-thought; the final boxed answer is extracted and checked, requiring LaTeX/expression normalization and equivalence checking."
metric: "Accuracy (% of problems with a correct final answer)."
format: "4-shot chain-of-thought; boxed-answer exact/equivalence match."
saturation: low
limitations: "Requires LaTeX parsing and mathematical-equivalence checking, so scoring is sensitive to formatting (e.g. 1/2 vs 0.5 vs \\frac{1}{2}) and answer-extraction bugs cause under-counting. Reasoning models have climbed sharply, and contamination of competition problems is plausible. Level-5 problems dominate the difficulty signal."
citation: "https://arxiv.org/abs/2103.03874"
example:
  task_type: "Competition math (boxed answer)"
  input: "Find the number of positive integers n <= 1000 such that 15n is a perfect square."
  success:
    answer: "8"
    why: "15 = 3*5, so n must supply the missing factors: n = 15m^2, giving 15n = (15m)^2. Then 15m^2 <= 1000 requires m^2 <= 66.7, so m <= 8, i.e. 8 values."
  failure:
    answer: "9"
    why: "Rounded sqrt(66.7) up to 9, including m = 9, but 15 * 9^2 = 1215 > 1000, so that boundary case does not qualify."
  checks: "The algebraic insight n = 15m^2 plus a correct integer bound, where an off-by-one on the boundary over-counts."
  scoring: "Accuracy on the final answer under mathematical-equivalence checking."
tags: [math, competition, reasoning, chain-of-thought]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2103.03874", title: "Measuring Mathematical Problem Solving With the MATH Dataset (Hendrycks et al., 2021)", trust: 0.95 }
related: [benchmark/gsm8k, benchmark/aime]
---

## What It Tests
Real mathematical problem-solving: choosing a strategy, doing algebraic manipulation, and arriving at
an exact answer for competition-level problems. Unlike GSM8K, brute-force arithmetic rarely suffices —
insight is required.

## How It Scores
Accuracy on the final answer, with difficulty levels 1–5 letting you see where a model breaks down.
The **MATH-500** subset is a 500-problem slice used for faster evaluation. Scoring requires
normalizing mathematical expressions and checking equivalence, not just string match.

## Format
Typically 4-shot with chain-of-thought; the model produces a solution ending in a boxed answer
(`\boxed{...}`) that the harness extracts and compares under mathematical equivalence.

## Limitations
Answer extraction and equivalence checking are the main measurement hazards — a correct answer
written in an unexpected form can be scored wrong. Competition problems may leak into training
corpora (contamination). Level-5 problems drive most of the discrimination, so the aggregate can hide
strength/weakness by level.

## Example
"Find the number of positive integers n ≤ 1000 such that 15n is a perfect square." Because 15 = 3·5,
n must supply the missing factors: n = 15m². Then 15n = (15m)². Require 15m² ≤ 1000 ⇒ m² ≤ 66.7 ⇒
m ≤ 8, so there are **8** valid n. A brute-force search that enumerates a few cases and misses some
fails — the algebraic insight is the point.
