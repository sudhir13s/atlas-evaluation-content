---
id: benchmark/gsm8k
type: benchmark
slug: gsm8k
schema_version: 1.0
title: "GSM8K"
name: GSM8K
category: Math & Logic
description: "8,500 grade-school math word problems requiring multi-step arithmetic reasoning."
what_is_it: "Grade School Math 8K: 8,500 linguistically diverse word problems (7.5K train / 1.3K test) that a bright middle-schooler could solve, each needing 2–8 steps of basic arithmetic. The dataset ships with step-by-step solutions, which made it the birthplace of chain-of-thought evaluation."
why_people_use_it: "It is the canonical entry-level math-reasoning benchmark — cheap, ground-truth scored, and reported by essentially every model. It shows whether a model can chain arithmetic steps, not just recall facts."
examples:
  - "Janet's ducks lay 16 eggs/day. She eats 3 and bakes 4 into muffins. She sells the rest at $2 each. How much does she make daily? (answer: 9 * $2 = $18)"
method: "Few-shot (often 5-shot or 8-shot) with chain-of-thought; the final numeric answer is extracted and exact-matched to the gold value. maj@k (self-consistency) is sometimes reported."
metric: "Accuracy (% of problems whose final numeric answer is correct)."
format: "5–8-shot chain-of-thought; exact-match on the final number."
saturation: high
limitations: "Frontier models exceed 95%, so it is saturated. Chain-of-thought can inflate scores via pattern-matching that does not generalize to harder math. The GSM-Symbolic study showed accuracy drops when surface numbers/names are perturbed, revealing fragility. Only tests elementary arithmetic — says nothing about competition math."
citation: "https://arxiv.org/abs/2110.14168"
worked_examples:
  - task_type: "Multi-step word problem (referent parsing)"
    input: "Tom reads 12 pages on Monday, twice as many on Tuesday, and 5 fewer than Tuesday on Wednesday. How many pages did he read in total?"
    success:
      answer: "55"
      why: "Monday 12, Tuesday 2*12 = 24, Wednesday 24 - 5 = 19; total 12 + 24 + 19 = 55."
    failure:
      answer: "43"
      why: "Attached the Wednesday clause to the wrong referent, computing 5 fewer than Monday (12 - 5 = 7) instead of Tuesday, giving 12 + 24 + 7 = 43."
    checks: "Parsing the referent of each clause and chaining the arithmetic steps in order."
    scoring: "Exact-match accuracy on the final numeric answer."
  - task_type: "Money / order of operations"
    input: "Notebooks cost $3 each. Maria buys 5 of them and pays with a $20 bill, using a $2 coupon on her purchase. How much change does she get?"
    success:
      answer: "7"
      why: "Cost = 5*3 = 15, minus the $2 coupon = 13; change = 20 - 13 = 7."
    failure:
      answer: "5"
      why: "Forgot to apply the $2 coupon, computing change as 20 - 15 = 5."
    checks: "Applying every adjustment (discount before change) in the right order."
    scoring: "Exact-match accuracy on the final numeric answer."
  - task_type: "Rate and unit conversion"
    input: "A car travels at 60 miles per hour. How far does it travel in 2 hours and 30 minutes?"
    success:
      answer: "150"
      why: "2 hours 30 minutes = 2.5 hours; distance = 60 * 2.5 = 150 miles."
    failure:
      answer: "138"
      why: "Converted 30 minutes to 0.30 hours instead of 0.5, computing 60 * 2.3 = 138."
    checks: "Converting minutes to a fraction of an hour before multiplying by the rate."
    scoring: "Exact-match accuracy on the final numeric answer."
  - task_type: "Fractions of a whole"
    input: "A pizza has 12 slices. John eats 1/3 of the pizza and Mary eats 1/4 of the pizza. How many slices are left?"
    success:
      answer: "5"
      why: "John eats 12*(1/3) = 4 and Mary eats 12*(1/4) = 3; 7 eaten, so 12 - 7 = 5 remain."
    failure:
      answer: "6"
      why: "Applied Mary's 1/4 to the 8 remaining slices instead of the original 12: (1/4)*8 = 2, giving 4 + 2 = 6 eaten and 6 left."
    checks: "Taking each fraction of the original whole, not of the running remainder."
    scoring: "Exact-match accuracy on the final numeric answer."
  - task_type: "Percentages"
    input: "A shirt costs $40. It is discounted 25%, then $5 in tax is added. What is the final price?"
    success:
      answer: "35"
      why: "25% off leaves 40 * 0.75 = 30; adding $5 tax gives 30 + 5 = 35."
    failure:
      answer: "20"
      why: "Read '25%' as a flat $25, subtracting 40 - 25 = 15 and adding $5 to get 20."
    checks: "Interpreting a percentage discount as a proportion of the price, not a dollar amount."
    scoring: "Exact-match accuracy on the final numeric answer."
tags: [math, reasoning, chain-of-thought, word-problems]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2110.14168", title: "Training Verifiers to Solve Math Word Problems (Cobbe et al., 2021)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2410.05229", title: "GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in LLMs (Mirzadeh et al., 2024)", trust: 0.85 }
related: [benchmark/math, benchmark/aime]
---

## What It Tests
Multi-step arithmetic reasoning over natural-language word problems. Each problem needs the model to
parse the scenario, plan a sequence of arithmetic operations, and execute them in order — the
elementary form of mathematical reasoning.

## How It Scores
Exact-match accuracy on the final numeric answer over the 1,319-item test set. The intermediate
reasoning is not graded, only the last number. Human (careful adult) performance is effectively 100%.
maj@k, which takes a majority vote over k sampled solutions, is a common higher-variance-reducing
variant.

## Format
5- to 8-shot with chain-of-thought is standard; the model is shown worked examples, reasons step by
step, and emits a final number that is regex-extracted and compared to gold.

## Limitations
**Saturated** (95%+) and **fragile**: the GSM-Symbolic follow-up showed that renaming entities or
changing numeric values drops accuracy, indicating some models pattern-match rather than reason.
Grading only the final number means a lucky wrong-reasoning answer counts as correct. It only covers
elementary arithmetic.

## Example
"Janet's ducks lay 16 eggs per day. She eats 3 for breakfast and bakes 4 into muffins. She sells the
remainder at $2 per egg. How much does she make daily?" Reasoning: 16 − 3 − 4 = 9 eggs; 9 × $2 =
**$18**. Emitting just "$18" with no steps scores correct too — a weakness of final-answer grading.
