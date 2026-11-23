---
id: benchmark/gsm8k
type: benchmark
slug: gsm8k
schema_version: 3.1
title: "GSM8K"
name: GSM8K
category: Math & Logic
description: "Grade School Math 8K: 8.5K linguistically diverse grade-school math word problems that take 2 to 8 steps of basic arithmetic."
what_is_it: "GSM8K was introduced by Cobbe et al. (2021) as 8.5K high-quality grade-school math word problems created by human problem writers, segmented into about 7.5K training and 1K test problems (the Hugging Face card lists 7,473 training and 1,319 test rows). Each problem takes between 2 and 8 steps to solve, mostly with elementary arithmetic, and comes with a natural-language solution whose final number follows the marker ####."
why_people_use_it: "The authors built it to diagnose why language models fail at multi-step mathematical reasoning on problems that are conceptually simple; the final numeric answer can be checked exactly."
examples:
  - "From the public test split: Janet's ducks lay 16 eggs per day. She eats 3 for breakfast and uses 4 to bake muffins, and sells the rest at the farmers' market for $2 each. How much does she make there daily? (answer: 18)"
method: "The model solves each test problem, usually with step-by-step reasoning and a few worked examples in the prompt; the final number is extracted and compared with the gold answer that follows #### in the reference solution."
metric: "Accuracy = test problems whose final number matches the gold answer / total test problems, in percent."
format: "Few-shot or zero-shot, usually with chain-of-thought; exact match on the final number."
limitations: "Only the final number is graded, so a correct answer reached by faulty working still counts. The problems cover elementary arithmetic only. The GSM-Symbolic study (Mirzadeh et al., 2024) reports that the performance of all models it tested declines when only the numerical values in GSM8K-style questions are altered."
citation: "https://arxiv.org/abs/2110.14168"
openness: open
openness_note: "All problems and step-by-step solutions (7,473 training and 1,319 test rows) are public on the Hugging Face dataset card under the MIT licence, and the final-answer check can be rerun by anyone."
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
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2110.14168", title: "Training Verifiers to Solve Math Word Problems (Cobbe et al., 2021)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/2110.14168", title: "GSM8K paper, full text (ar5iv)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2410.05229", title: "GSM-Symbolic: Understanding the Limitations of Mathematical Reasoning in Large Language Models (Mirzadeh et al., 2024)", trust: 0.85 }
  - { url: "https://huggingface.co/datasets/openai/gsm8k", title: "openai/gsm8k dataset card (MIT licence, 7,473 train / 1,319 test)", trust: 0.9 }
  - { url: "https://datasets-server.huggingface.co/rows?dataset=openai/gsm8k&config=main&split=test&offset=0&length=1", title: "GSM8K test split, first row (Janet's ducks)", trust: 0.9 }
related: [benchmark/math, benchmark/aime]
---

## What It Tests
**Multi-step arithmetic reasoning** in word problems: read the scenario, plan the operations, carry them out in order.

- **Makers** — Cobbe et al. (2021); the problems were written by **human problem writers**, first freelancers and later through Surge AI (paper, Appendix A).
- **Size** — 8.5K problems: about 7.5K training and 1K test (7,473 and 1,319 rows on the dataset card).
- **Depth** — each problem takes **2 to 8 steps**, mostly elementary arithmetic.

## How It Scores
**Accuracy** = test problems whose final number matches the gold answer / total test problems.

- **Gold answer** — the number after `####` at the end of each reference solution.
- **Not graded** — the working; only the final number counts.

## Format
Few-shot or zero-shot prompts, usually with step-by-step (chain-of-thought) reasoning; the final number is extracted from the response.

- **Reference solutions** — natural language with calculator annotations such as `<<48/2=24>>`.
- **Data** — public on the `openai/gsm8k` Hugging Face card under the **MIT licence**, in a `main` and a `socratic` configuration.

## Limitations
- **Answer-only grading.** Faulty working can still score.
- **Elementary scope.** Basic arithmetic, not competition math.
- **Fragility.** GSM-Symbolic (2024) reports performance declines when only the numbers change.

## Example
From the public test split on the Hugging Face dataset card (MIT licence), first row, closely paraphrased:

- **Problem** — Janet's ducks lay 16 eggs per day. She eats 3 for breakfast and uses 4 to bake muffins. She sells the rest at the farmers' market for $2 each. How much does she make there daily?
- **Reference working** — 16 − 3 − 4 = 9 eggs sold; 9 × $2 = $18.
- **Gold answer** — `#### 18`. A response ending in 18 scores correct, even with no working shown.
