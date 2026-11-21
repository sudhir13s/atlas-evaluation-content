---
id: benchmark/aime
type: benchmark
slug: aime
schema_version: 1.0
title: "AIME"
name: AIME
category: Math & Logic
description: "Problems from the American Invitational Mathematics Examination (AIME), a 15-question competition whose answers are integers from 0 to 999."
what_is_it: "The AIME is run by the Mathematical Association of America (MAA) for students who earned a qualifying score on the AMC 10 or AMC 12. The MAA describes it as a 15-question exam given in two parts of 90 minutes each, with every answer an integer from 0 to 999, and harder than the AMC 10 and AMC 12. Model evaluations use the problems of past exams."
why_people_use_it: "Every answer is a single integer, so a model's final answer can be checked by exact match without a judge, and each year's exam adds new problems."
examples:
  - "Every morning Aya walks 9 km and stops for coffee; the walk takes different total times at speeds s and s+2 km/h... find the number of minutes the walk takes at speed s + 1/2. (AIME 2024 I, Problem 1; integer answer 0-999)"
method: "The model is given each problem, usually with step-by-step reasoning, and its final integer answer is compared with the official answer. Because there are few problems per exam, evaluators often average accuracy over several runs."
metric: "Accuracy (% of problems answered correctly), often averaged over several sampled runs."
format: "0-shot free response; integer exact match."
limitations: "The AIME is not a benchmark dataset with a paper: which exams and years are used, and how many runs are averaged, differ between reports, so a score needs those details. An exam has only 15 problems, so one problem moves the score by several points. Only the final integer is graded, not the working. Problems from a published exam can appear in later training data."
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
tags: [math, competition, reasoning]
updated: 2026-11-21
sources:
  - { url: "https://maa.org/maa-invitational-competitions/", title: "MAA Invitational Competitions (AIME description)", trust: 0.95 }
  - { url: "https://artofproblemsolving.com/wiki/index.php/2024_AIME_I_Problems/Problem_1", title: "2024 AIME I, Problem 1 (AoPS Wiki)", trust: 0.8 }
  - { url: "https://matharena.ai/", title: "MathArena (AIME evaluations, runs averaged per problem)", trust: 0.8 }
related: [benchmark/math, benchmark/otis-mock-aime-2024-2025]
---

## What It Tests
Multi-step **competition mathematics** at the level of the AIME, which the MAA describes as harder than the AMC 10 and AMC 12.

- **Exam format** — 15 questions in two 90-minute parts.
- **Answers** — always an integer from 0 to 999.

## How It Scores
**Exact match** of the final integer against the official answer.

- **Accuracy** — the share of problems answered correctly.
- **Averaging** — evaluators such as MathArena run each problem several times and report the mean.

## Format
Free-response problems answered without examples in the prompt; the model reasons, then gives one integer.

## Limitations
- **Not a fixed dataset.** Reports differ in which exams and years they use.
- **Small.** 15 problems per exam, so single runs are noisy.
- **Answer-only grading.** Correct working is not checked.
- **Leakage.** Published problems can enter later training data.

## Example
AIME 2024 I, Problem 1 (Aya's walk): given that a 9 km walk plus a fixed coffee stop takes 4 hours at
speed s and 2 hours 24 minutes at speed s+2, find the total minutes at speed s + ½. Setting up
1/s and 1/(s+2) from the two times gives s = 2.5 km/h and a 24-minute coffee stop; at 3 km/h the walk
is 180 + 24 = **204** minutes. The integer answer format makes grading unambiguous.
