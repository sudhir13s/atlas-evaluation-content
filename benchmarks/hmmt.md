---
id: benchmark/hmmt
type: benchmark
slug: hmmt
schema_version: 1.0
title: "HMMT"
name: HMMT
category: Math & Logic
description: "Harvard-MIT Math Tournament problems used as an uncontaminated competition-math eval — final-answer accuracy over the fresh Feb/Nov 2025 problem sets."
what_is_it: "HMMT (Harvard-MIT Mathematics Tournament) is a prestigious high-school math contest held twice a year (November and February). As an LLM eval — popularized by MathArena — the short-answer problems from a fresh HMMT sitting are used as an uncontaminated competition-math test: each problem has a single numeric or closed-form final answer that is auto-graded, similar in spirit to AIME."
why_people_use_it: "Because each HMMT sitting is brand-new, its problems are unlikely to appear in training data, so it measures genuine mathematical reasoning rather than memorization. It sits alongside AIME as a go-to frontier math benchmark; the February sitting is notably harder (national-olympiad level), making it a stronger discriminator as AIME scores climb into the mid-90s."
examples:
  - "HMMT February 2025, algebra: compute a specific value defined by a functional or number-theoretic condition — the model must output the exact integer/closed-form answer, which is checked against the gold answer."
  - "HMMT February 2025, combinatorics: count configurations satisfying stated constraints; the single numeric answer is auto-graded."
method: "Each model is prompted with one contest problem and must produce a final numeric/closed-form answer, which is extracted and compared to the gold answer. On MathArena, every model is run 4 times per problem and scores are averaged. The Feb 2025 set has 30 problems; a separate Nov 2025 set is also tracked."
metric: "Accuracy (% of problems solved), averaged over 4 runs per problem."
format: "0-shot final-answer math; single numeric or closed-form answer per problem, auto-graded."
saturation: high
limitations: "Small problem sets (~30 per sitting) mean high variance — one problem is ~3 percentage points. Only the final answer is graded, so a right answer from flawed reasoning still counts and proof-based rounds are excluded. Contamination is reduced but not zero: some problems resemble prior contest items, and once a sitting is public it can leak into future training data, so freshness must be tracked by date."
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
updated: 2026-08-02
sources:
  - { url: "https://matharena.ai/", title: "MathArena: Evaluating LLMs on Uncontaminated Math Competitions", trust: 0.9 }
  - { url: "https://arxiv.org/abs/2505.23281", title: "MathArena: Evaluating LLMs on Uncontaminated Math Competitions (Balunović et al., 2025)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/MathArena/hmmt_feb_2025", title: "MathArena/hmmt_feb_2025 dataset (Hugging Face)", trust: 0.95 }
related: [benchmark/aime]
---

## What It Tests
Competition-level mathematical reasoning on fresh problems. HMMT is a twice-yearly high-school
tournament; its short-answer problems — spanning algebra, combinatorics, number theory, and geometry
— are repurposed as an eval. Each problem has one exact answer, so the test rewards correct
multi-step derivation, not recall. The February sitting is deliberately harder than AIME (closer to
national-olympiad difficulty), while the November sitting is nearer AMC/AIME level.

## How It Scores
Accuracy — the percentage of problems whose final answer matches the gold answer. Because a single
sitting is small (the MathArena Feb 2025 set is 30 problems), results are averaged over multiple runs
per problem (MathArena uses 4) to cut variance. Top frontier models now reach the mid-90s on HMMT
February 2025, which is why it is used as a harder companion to AIME rather than a beginner test.

## Format
Zero-shot final-answer math: the model receives one problem statement (LaTeX) and must emit a single
numeric or closed-form answer. The answer is parsed and string/value-matched against the reference —
no partial credit and no grading of the intermediate work.

## Limitations
The sets are tiny (~30 problems per sitting), so one problem shifts the score by several points and
small model gaps are noise. Only the final answer is checked, so a correct answer reached by faulty
reasoning still scores, and the proof-heavy Team Round is excluded. Freshness reduces but does not
eliminate contamination — occasional problems echo earlier contests, and each sitting can leak into
later training corpora once public, so the evaluation date matters.

## Example
An HMMT February 2025 number-theory problem asks for a specific integer defined by a divisibility
condition. The model must carry out the full argument and output just that integer; MathArena runs it
four times and averages. A model that sets up the problem correctly but makes an arithmetic slip
produces the wrong final value and scores zero on that item — final-answer grading gives no credit
for a nearly-correct derivation.
