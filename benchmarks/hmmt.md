---
id: benchmark/hmmt
type: benchmark
slug: hmmt
schema_version: 1.0
title: "HMMT"
name: HMMT
category: Math & Logic
description: "Final-answer problems from the Harvard-MIT Mathematics Tournament, run by MathArena on each new contest (HMMT February 2025, November 2025, February 2026)."
what_is_it: "HMMT (Harvard-MIT Mathematics Tournament), founded in 1998, is a student-organized high school mathematics competition run by students at Harvard, MIT and nearby schools, with tournaments in November and February. MathArena (ETH Zurich and INSAIT; Balunović et al., 2025) evaluates models on the final-answer problems of each new HMMT contest; the HMMT February 2025 set has 30 problems."
why_people_use_it: "MathArena evaluates models as soon as a competition's problems are released, which its authors say effectively eliminates the risk of contamination (2025 paper)."
examples:
  - "HMMT February 2025, algebra: compute a specific value defined by a functional or number-theoretic condition — the model must output the exact integer/closed-form answer, which is checked against the gold answer."
  - "HMMT February 2025, combinatorics: count configurations satisfying stated constraints; the single numeric answer is auto-graded."
method: "MathArena prompts the model with one problem, extracts the final answer from \\boxed{} with a custom rule-based parser that turns LaTeX into structured values, and compares it with the official answer. Each model runs four times per problem."
metric: "Accuracy (pass@1, % of problems solved), averaged over the four runs."
format: "Final-answer competition mathematics; one numeric or closed-form answer per problem."
limitations: "A contest set is small (30 problems for HMMT February 2025), so one problem is about 3.3 percentage points. Only the final answer is graded, not the reasoning. Each contest is a separate set, so cite which one (for example HMMT February 2025) with a score."
citation: "https://matharena.ai/"
worked_examples:
  - task_type: "Number theory (modular casework)"
    input: "Find the number of positive integers n < 100 such that n^2 + n + 1 is divisible by 3."
    success:
      answer: "33"
      why: "n^2 + n + 1 mod 3 equals 0 only when n is congruent to 1 mod 3; among 1..99 those are 1, 4, ..., 97, i.e. 33 values."
    failure:
      answer: "66"
      why: "When checking n congruent to 2 mod 3, dropped the +1 constant (2^2 + 2 = 6, congruent to 0) instead of 2^2 + 2 + 1 = 7, wrongly counting that residue class too and doubling to 66."
    checks: "Correct residue-class casework with exact modular arithmetic, where a single dropped term flips the count."
    scoring: "Accuracy (final answer matches gold), averaged over 4 runs per problem."
  - task_type: "Combinatorics (circular permutations)"
    input: "In how many distinct ways can 8 people be seated around a circular table, where seatings that differ only by rotation are considered the same?"
    success:
      answer: "5040"
      why: "Fixing one person to remove rotational symmetry leaves (8-1)! = 7! = 5040 arrangements of the rest."
    failure:
      answer: "40320"
      why: "Used 8! = 40320, the count for a row, without dividing by the 8 rotations that give identical circular seatings."
    checks: "Accounting for rotational symmetry by fixing a reference seat, (n-1)! not n!."
    scoring: "Accuracy (final answer matches gold), averaged over 4 runs per problem."
  - task_type: "Number theory (last two digits)"
    input: "Find the last two digits of 7^2025."
    success:
      answer: "07"
      why: "Powers of 7 mod 100 cycle with period 4: 07, 49, 43, 01. Since 2025 = 4*506 + 1, 7^2025 is congruent to 7^1 = 07 mod 100."
    failure:
      answer: "43"
      why: "Miscomputed 2025 mod 4 as 3 instead of 1, landing on the third term of the cycle (43) rather than the first."
    checks: "Finding the multiplicative cycle length and reducing the exponent modulo it correctly."
    scoring: "Accuracy (final answer matches gold), averaged over 4 runs per problem."
  - task_type: "Algebra (Vieta's formulas)"
    input: "The roots of x^2 - 7x + 10 = 0 are r and s. Find r^2 + s^2."
    success:
      answer: "29"
      why: "By Vieta, r + s = 7 and rs = 10; r^2 + s^2 = (r + s)^2 - 2rs = 49 - 20 = 29."
    failure:
      answer: "49"
      why: "Took r^2 + s^2 = (r + s)^2 = 49, dropping the -2rs cross term from the identity."
    checks: "Using the identity r^2 + s^2 = (r+s)^2 - 2rs with the subtracted product term intact."
    scoring: "Accuracy (final answer matches gold), averaged over 4 runs per problem."
  - task_type: "Combinatorics (digit constraints)"
    input: "How many 4-digit numbers have all distinct digits and are even?"
    success:
      answer: "2296"
      why: "Split on the last digit. If it is 0: 9*8*7 = 504. If it is 2,4,6, or 8 (4 choices): first digit has 8 options (nonzero, not the last), then 8 and 7, giving 4*8*8*7 = 1792. Total 504 + 1792 = 2296."
    failure:
      answer: "2240"
      why: "Failed to separate the last-digit-0 case, treating all 5 even last digits uniformly with 8 choices for the leading digit (8*8*7*5 = 2240), which under-counts the arrangements when the units digit is 0."
    checks: "Case-splitting on the units digit because a trailing 0 changes how many leading-digit choices remain."
    scoring: "Accuracy (final answer matches gold), averaged over 4 runs per problem."
tags: [math, competition-math, reasoning, final-answer]
updated: 2026-11-21
sources:
  - { url: "https://matharena.ai/", title: "MathArena: Evaluating LLMs on Uncontaminated Math Competitions", trust: 0.9 }
  - { url: "https://arxiv.org/abs/2505.23281", title: "MathArena: Evaluating LLMs on Uncontaminated Math Competitions (Balunović et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2505.23281", title: "MathArena, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://www.hmmt.org/", title: "HMMT — official site", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/MathArena/hmmt_feb_2025", title: "MathArena/hmmt_feb_2025 dataset (Hugging Face)", trust: 0.95 }
related: [benchmark/aime]
---

## What It Tests
**Competition mathematics** on newly released HMMT problems, each with one exact final answer.

- **Source** — HMMT, a student-organized high school competition (Harvard, MIT and nearby schools).
- **Sets on MathArena** — HMMT February 2025, November 2025 and February 2026, each a separate set.

## How It Scores
**Accuracy** (pass@1): the share of problems whose extracted final answer matches the official one, averaged over four runs per problem. No partial credit.

## Format
One problem per prompt; the model puts its final answer in `\boxed{}`. A rule-based parser converts the LaTeX answer to a structured value before comparison.

## Limitations
- **Small sets.** 30 problems in HMMT February 2025: one problem is about 3.3 points.
- **Final answer only.** Reasoning is not graded.
- **Per contest.** Scores on different HMMT contests are not interchangeable.

## Example
A problem shown in the MathArena paper's pipeline figure: "Compute the sum of the positive divisors of 9! that have units digit 1." The official answer is 103; the model's boxed answer is parsed and compared with it.
