---
id: benchmark/math
type: benchmark
slug: math
schema_version: 1.0
title: "MATH"
name: MATH
category: Math & Logic
description: "12,500 competition mathematics problems in seven subjects, each with a step-by-step solution and a final boxed answer."
what_is_it: "MATH (Mathematics Aptitude Test of Heuristics) was introduced by Hendrycks et al. (2021). It has 12,500 competition mathematics problems, 7,500 for training and 5,000 for test. The seven subjects are Prealgebra, Algebra, Number Theory, Counting and Probability, Geometry, Intermediate Algebra and Precalculus. Each problem is tagged with a difficulty level from 1 to 5 following Art of Problem Solving (AoPS): early AMC 8 problems are often level 1, AIME problems level 5."
why_people_use_it: "Each problem has a full step-by-step solution and a single final answer, so a model's answer can be checked automatically, and the difficulty levels and subjects show where a model breaks down."
examples:
  - "Find the number of positive integers n <= 1000 such that 15n is a perfect square. (answer: 8, via n = 15m^2)"
method: "The model generates a solution ending in a final boxed answer, which is extracted and compared with the reference answer, usually after normalising the mathematical expression."
metric: "Accuracy (% of test problems with a correct final answer)."
format: "Free response with a boxed final answer; zero-shot or few-shot."
limitations: "Answer checking needs expression normalisation (for example 1/2 versus 0.5), so extraction and equivalence rules change scores between harnesses. Subsets used in reports, such as the level-5 problems, are not comparable with the full test set; MATH Level 5 has its own record."
citation: "https://arxiv.org/abs/2103.03874"
worked_examples:
  - task_type: "Number theory (perfect squares)"
    input: "Find the number of positive integers n <= 1000 such that 15n is a perfect square."
    success:
      answer: "8"
      why: "15 = 3*5, so n must supply the missing factors: n = 15m^2, giving 15n = (15m)^2. Then 15m^2 <= 1000 requires m^2 <= 66.7, so m <= 8, i.e. 8 values."
    failure:
      answer: "9"
      why: "Rounded sqrt(66.7) up to 9, including m = 9, but 15 * 9^2 = 1215 > 1000, so that boundary case does not qualify (off-by-one on the boundary)."
    checks: "The algebraic insight n = 15m^2 plus a correct integer bound, where an off-by-one on the boundary over-counts."
    scoring: "Accuracy on the final answer under mathematical-equivalence checking."
  - task_type: "Algebra (absolute-value equation)"
    input: "Find the sum of all real solutions to x^2 - 5|x| + 6 = 0."
    success:
      answer: "0"
      why: "Let u = |x| >= 0: u^2 - 5u + 6 = 0 gives u = 2 or u = 3, so x = ±2, ±3. By symmetry the four roots sum to 0."
    failure:
      answer: "5"
      why: "Solved only the x >= 0 branch, taking x = 2 and x = 3 and summing to 5, dropping the negative roots that the absolute value also allows."
    checks: "Recognizing that |x| yields both signs, so every positive root has a negative twin."
    scoring: "Accuracy on the final answer under mathematical-equivalence checking."
  - task_type: "Number theory (divisor counting)"
    input: "How many positive divisors does 360 have?"
    success:
      answer: "24"
      why: "360 = 2^3 * 3^2 * 5^1, so the divisor count is (3+1)(2+1)(1+1) = 24."
    failure:
      answer: "6"
      why: "Multiplied the exponents directly (3 * 2 * 1 = 6), forgetting the +1 on each exponent in the divisor-count formula."
    checks: "Applying the (e_i + 1) product formula for the number of divisors."
    scoring: "Accuracy on the final answer under mathematical-equivalence checking."
  - task_type: "Counting / probability"
    input: "Two fair six-sided dice are rolled. What is the probability that the sum is 7? Express your answer as a common fraction."
    success:
      answer: "1/6"
      why: "Of 36 equally likely ordered outcomes, 6 give a sum of 7: (1,6),(2,5),(3,4),(4,3),(5,2),(6,1). So 6/36 = 1/6."
    failure:
      answer: "1/12"
      why: "Counted the unordered pairs {1,6},{2,5},{3,4} as only 3 outcomes, giving 3/36 = 1/12, ignoring that (1,6) and (6,1) are distinct rolls."
    checks: "Treating the two dice as distinguishable so ordered outcomes are counted."
    scoring: "Accuracy on the final answer under mathematical-equivalence checking."
  - task_type: "Geometry (circle)"
    input: "A circle has area 36*pi. What is its circumference? Express your answer in terms of pi."
    success:
      answer: "12*pi"
      why: "pi*r^2 = 36*pi gives r^2 = 36, so r = 6; circumference = 2*pi*r = 12*pi."
    failure:
      answer: "72*pi"
      why: "Set the radius equal to the area coefficient (r = 36 instead of solving r^2 = 36), giving circumference 2*pi*36 = 72*pi — confusing r with r^2."
    checks: "Solving r^2 = 36 for r before applying the circumference formula."
    scoring: "Accuracy on the final answer under mathematical-equivalence checking."
tags: [math, competition, reasoning, chain-of-thought]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2103.03874", title: "Measuring Mathematical Problem Solving With the MATH Dataset (Hendrycks et al., 2021)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/2103.03874", title: "MATH paper, full text (ar5iv)", trust: 0.95 }
related: [benchmark/gsm8k, benchmark/aime, benchmark/math-level-5]
---

## What It Tests
**Competition mathematics**: choosing a strategy and reaching an exact answer across seven subjects.

## How It Scores
**Accuracy** on the final boxed answer over the 5,000 test problems.

- **Difficulty levels** — 1 to 5, following AoPS.
- **Subjects** — seven, from Prealgebra to Precalculus.

## Format
The model writes a solution ending in `\boxed{...}`; the harness extracts and compares it.

## Limitations
- **Answer checking.** Extraction and equivalence rules move scores.
- **Subsets.** Level-5 or other subsets are not comparable with the full test set.

## Example
"Find the number of positive integers n ≤ 1000 such that 15n is a perfect square." Because 15 = 3·5,
n must supply the missing factors: n = 15m². Then 15n = (15m)². Require 15m² ≤ 1000 ⇒ m² ≤ 66.7 ⇒
m ≤ 8, so there are **8** valid n. A brute-force search that enumerates a few cases and misses some
fails — the algebraic insight is the point.
