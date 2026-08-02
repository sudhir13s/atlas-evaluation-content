---
id: benchmark/aime
type: benchmark
slug: aime
schema_version: 1.0
title: "AIME"
name: AIME
category: Math & Logic
description: "American Invitational Mathematics Examination problems — a hard, contamination-resistant math frontier."
what_is_it: "Problems from the AIME, a selective 15-question US high-school competition whose answers are always integers from 0 to 999. Because a new exam is released each year (AIME I and II), post-cutoff editions serve as a fresh, low-contamination test of advanced mathematical reasoning. The 2024/2025 sets (30 problems) are the common reporting slice."
why_people_use_it: "It has become the headline math benchmark for reasoning models. The integer-only answer format makes scoring trivial and unambiguous, and using the newest exams sidesteps the contamination that plagues GSM8K and MATH."
examples:
  - "Every morning Aya walks 9 km and stops for coffee; the walk takes different total times at speeds s and s+2 km/h... find the number of minutes the walk takes at speed s + 1/2. (AIME 2024 I, Problem 1; integer answer 0-999)"
method: "0-shot or few-shot chain-of-thought; the final integer (0-999) is extracted and exact-matched. Often reported as avg@k / pass@1 over many samples because per-exam variance is high (only 15 problems per exam)."
metric: "Accuracy (% of problems solved) or avg@k over sampled attempts."
format: "0-shot CoT; integer exact-match."
saturation: low
limitations: "Very small per exam (15 problems), so single-run scores are extremely noisy — averaging over many samples and multiple exams is essential. Integer-only answers mean a right number from wrong reasoning still counts. Once a year's exam is public it becomes contaminable, so only post-cutoff editions are trustworthy."
citation: "https://maa.org/maa-invitational-competitions/"
worked_examples:
  - task_type: "Repdigit / algebra (integer answer 0-999)"
    input: "The sum of the first n positive integers is a three-digit number whose three digits are all identical. Find n."
    success:
      answer: "36"
      why: "n(n+1)/2 = 666 = 111*6; clearing the fraction gives n(n+1) = 1332 = 36*37, so n = 36."
    failure:
      answer: "25"
      why: "Dropped the factor of 2 when clearing the fraction, solving n(n+1) = 666 instead of 1332, then rounded n = 25.3 down to 25 (whose sum 325 is not a repdigit)."
    checks: "Sustained multi-step reasoning with correct algebraic manipulation, not merely landing on a plausible integer."
    scoring: "Exact-match on the integer answer (0-999); avg@k over sampled attempts."
  - task_type: "Inclusion-exclusion counting"
    input: "How many positive integers less than 1000 are divisible by neither 5 nor 7?"
    success:
      answer: "686"
      why: "Multiples of 5 up to 999: floor(999/5) = 199; of 7: floor(999/7) = 142; of 35: floor(999/35) = 28. Divisible by 5 or 7 = 199 + 142 - 28 = 313, so neither = 999 - 313 = 686."
    failure:
      answer: "658"
      why: "Double-subtracted the multiples of 35: computed 999 - (199 + 142) = 658, forgetting to add back the 28 numbers counted in both sets (the intersection term in inclusion-exclusion)."
    checks: "Correct inclusion-exclusion, including restoring the doubly-counted overlap."
    scoring: "Exact-match on the integer answer (0-999); avg@k over sampled attempts."
  - task_type: "Symmetric functions / algebra"
    input: "Positive real numbers x and y satisfy x + y = 30 and x^2 + y^2 = 500. Find xy."
    success:
      answer: "200"
      why: "(x + y)^2 = x^2 + 2xy + y^2, so 900 = 500 + 2xy, giving 2xy = 400 and xy = 200."
    failure:
      answer: "400"
      why: "Dropped the factor of 2 in the middle term, treating (x + y)^2 = x^2 + xy + y^2, so xy = 900 - 500 = 400."
    checks: "Using the square-of-a-sum identity with the correct 2xy cross term."
    scoring: "Exact-match on the integer answer (0-999); avg@k over sampled attempts."
  - task_type: "Geometry (Pythagorean triple)"
    input: "A right triangle has legs of length 20 and 21. Its altitude to the hypotenuse has length m/n in lowest terms. Find m + n."
    success:
      answer: "449"
      why: "The hypotenuse is sqrt(20^2 + 21^2) = sqrt(841) = 29. Altitude = (product of legs)/hypotenuse = (20*21)/29 = 420/29, already in lowest terms, so m + n = 420 + 29 = 449."
    failure:
      answer: "461"
      why: "Computed the hypotenuse as 20 + 21 = 41 (added the legs instead of applying the Pythagorean theorem), giving 420/41 and m + n = 461."
    checks: "Correctly deriving the hypotenuse before applying the area-based altitude formula."
    scoring: "Exact-match on the integer answer (0-999); avg@k over sampled attempts."
  - task_type: "Number theory (Legendre's formula)"
    input: "The largest power of 2 dividing 100! is 2^k. Find k."
    success:
      answer: "97"
      why: "Legendre's formula sums floor(100/2^i): 50 + 25 + 12 + 6 + 3 + 1 = 97."
    failure:
      answer: "93"
      why: "Truncated Legendre's formula at 2^4, summing only 50 + 25 + 12 + 6 = 93 and dropping the floor(100/32) = 3 and floor(100/64) = 1 terms."
    checks: "Carrying Legendre's formula through every power of 2 up to the argument, not stopping early."
    scoring: "Exact-match on the integer answer (0-999); avg@k over sampled attempts."
tags: [math, competition, frontier, reasoning]
updated: 2026-08-02
sources:
  - { url: "https://maa.org/maa-invitational-competitions/", title: "MAA Invitational Competitions (AIME) — official", trust: 0.9 }
  - { url: "https://arxiv.org/abs/2103.03874", title: "MATH dataset (context for competition-math evaluation)", trust: 0.8 }
related: [benchmark/math, benchmark/gpqa]
---

## What It Tests
Advanced, multi-step mathematical problem-solving at a level well above MATH's average — olympiad
feeder difficulty. Solving requires sustained symbolic reasoning, casework, and verification, not
retrieval.

## How It Scores
Exact-match on the integer answer (0–999). Because each exam has only 15 problems, the community
reports averaged metrics — **avg@k** (mean pass rate over k samples) or pass@1 over multiple exams —
to tame variance. A single 30-problem year is still a small sample; treat 1–2 problem differences as
noise.

## Format
0-shot chain-of-thought is typical for reasoning models; the model works the problem and outputs a
final integer that is compared to the official answer key.

## Limitations
**Small samples** make it noisy — always average over many generations and both exams in a year.
Integer-only grading rewards lucky guesses and correct-answer/wrong-work solutions. Contamination
appears the moment an exam is published, so only editions after a model's training cutoff give a
clean read.

## Example
AIME 2024 I, Problem 1 (Aya's walk): given that a 9 km walk plus a fixed coffee stop takes 4 hours at
speed s and 2 hours 24 minutes at speed s+2, find the total minutes at speed s + ½. Setting up
1/s and 1/(s+2) from the two times gives s = 2.5 km/h and a 24-minute coffee stop; at 3 km/h the walk
is 180 + 24 = **204** minutes. The integer answer format makes grading unambiguous.
