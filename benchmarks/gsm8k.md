---
id: benchmark/gsm8k
type: benchmark
slug: gsm8k
schema_version: 1.0
title: "GSM8K"
name: GSM8K
category: Math & Logic
description: "Grade School Math 8K: 8.5K linguistically diverse grade-school math word problems that take 2 to 8 steps of basic arithmetic."
what_is_it: "GSM8K was introduced by Cobbe et al. (2021) as 8.5K high-quality grade-school math word problems, split into 7.5K training and 1K test problems. Each problem takes between 2 and 8 steps to solve, mostly with elementary arithmetic, and comes with a natural-language solution."
why_people_use_it: "The authors built it to diagnose why language models fail at multi-step mathematical reasoning on problems that are conceptually simple; the final numeric answer can be checked exactly."
examples:
  - "Janet's ducks lay 16 eggs/day. She eats 3 and bakes 4 into muffins. She sells the rest at $2 each. How much does she make daily? (answer: 9 * $2 = $18)"
method: "The model solves each test problem, usually with step-by-step reasoning and a few worked examples in the prompt; the final number is extracted and compared with the gold answer."
metric: "Accuracy (% of test problems with the correct final answer)."
format: "Few-shot or zero-shot, usually with chain-of-thought; exact match on the final number."
limitations: "Only the final number is graded, so a correct answer reached by faulty working still counts. The problems cover elementary arithmetic only. The GSM-Symbolic study (Mirzadeh et al., 2024) reports that the performance of all models it tested declines when only the numerical values in GSM8K-style questions are altered."
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2110.14168", title: "Training Verifiers to Solve Math Word Problems (Cobbe et al., 2021)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/2110.14168", title: "GSM8K paper, full text (ar5iv)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2410.05229", title: "GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models (Mirzadeh et al., 2024)", trust: 0.85 }
  - { url: "https://huggingface.co/datasets/openai/gsm8k", title: "GSM8K dataset (Janet's ducks test item)", trust: 0.9 }
related: [benchmark/math, benchmark/aime]
---

## What It Tests
**Multi-step arithmetic reasoning** in word problems: read the scenario, plan the operations, carry them out in order.

## How It Scores
**Exact match** on the final numeric answer over the test split; the working is not graded.

## Format
Few-shot or zero-shot prompts, usually with step-by-step reasoning; the final number is extracted from the response.

## Limitations
- **Answer-only grading.** Faulty working can still score.
- **Elementary scope.** Basic arithmetic, not competition math.
- **Fragility.** GSM-Symbolic (2024) reports performance declines when only the numbers change.

## Example
"Janet's ducks lay 16 eggs per day. She eats 3 for breakfast and bakes 4 into muffins. She sells the
remainder at $2 per egg. How much does she make daily?" Reasoning: 16 − 3 − 4 = 9 eggs; 9 × $2 =
**$18**. Emitting just "$18" with no steps scores correct too — a weakness of final-answer grading.
