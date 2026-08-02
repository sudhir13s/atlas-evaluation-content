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
example:
  task_type: "Competition math (integer answer 0-999)"
  input: "The sum of the first n positive integers is a three-digit number whose three digits are all identical. Find n."
  success:
    answer: "36"
    why: "n(n+1)/2 = 666 = 111*6; clearing the fraction gives n(n+1) = 1332 = 36*37, so n = 36."
  failure:
    answer: "25"
    why: "Dropped the factor of 2 when clearing the fraction, solving n(n+1) = 666 instead of 1332, then rounded n = 25.3 down to 25 (whose sum 325 is not a repdigit)."
  checks: "Sustained multi-step reasoning with correct algebraic manipulation, not merely landing on a plausible integer."
  scoring: "Exact-match on the integer answer (0-999); avg@k over sampled attempts."
tags: [math, competition, frontier, reasoning]
updated: 2026-07-17
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
