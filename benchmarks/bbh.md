---
id: benchmark/bbh
type: benchmark
slug: bbh
schema_version: 1.0
title: "BBH (BIG-Bench Hard)"
name: BBH
category: Knowledge & Academic Reasoning
description: "The 23 hardest BIG-Bench tasks where prior models failed to beat the average human."
what_is_it: "BIG-Bench Hard is a curated 23-task subset of the 200+-task BIG-Bench suite, chosen because earlier language models scored below the average human rater on them. Tasks span logical deduction, multi-step arithmetic, causal judgment, word manipulation, and more."
why_people_use_it: "It concentrates BIG-Bench's signal into the tasks that still discriminate, and it was the benchmark that popularized chain-of-thought prompting (CoT dramatically improved BBH scores). It is part of the HF Open LLM Leaderboard v2 suite."
examples:
  - "(Logical deduction) A is left of B. C is right of B. D is left of A. Order left to right? (correct: D, A, B, C)"
method: "3-shot, usually with chain-of-thought; a mix of multiple-choice and exact-match free-response tasks, scored per task then averaged."
metric: "Average accuracy across the 23 tasks (exact-match or multiple-choice per task)."
format: "3-shot chain-of-thought."
saturation: medium
limitations: "Highly heterogeneous — averaging across very different tasks can hide that a model fails badly on a few. Chain-of-thought is essentially required, so scores conflate reasoning ability with prompt formatting. Some tasks are small, adding variance."
citation: "https://arxiv.org/abs/2210.09261"
worked_examples:
  - task_type: "Logical deduction (multi-step ordering)"
    input: "Four books A, B, C, D sit in a row. A is to the left of B. C is to the right of B. D is to the left of A. What is the left-to-right order?"
    options:
      - "D, A, B, C"
      - "A, B, C, D"
      - "A, D, B, C"
      - "D, B, A, C"
    success:
      answer: "A — D, A, B, C"
      why: "Chaining the three constraints gives D < A < B < C, the only ordering that satisfies all of them."
    failure:
      answer: "C — A, D, B, C"
      why: "It honors A<B and C rightmost but places D to the right of A, violating 'D is to the left of A' — a partial-constraint slip."
    checks: "Composing several constraints into one consistent ordering via chain-of-thought, not partial constraint satisfaction."
    scoring: "Exact-match accuracy on the selected option, averaged across the 23 tasks."
  - task_type: "Date understanding"
    input: "Yesterday was April 30, 2021. What is the date tomorrow, in MM/DD/YYYY?"
    options:
      - "05/02/2021"
      - "05/01/2021"
      - "04/30/2021"
      - "05/03/2021"
    success:
      answer: "A — 05/02/2021"
      why: "Yesterday = April 30, so today = May 1 and tomorrow = May 2; the reasoning must roll over the month end (April has 30 days)."
    failure:
      answer: "B — 05/01/2021"
      why: "It computes 'today' (May 1) instead of 'tomorrow' — an off-by-one error on the relative-date reference, the most common failure on this task."
    checks: "Resolving a chain of relative-date references and handling the month boundary correctly."
    scoring: "Exact-match accuracy on the selected option, averaged across the 23 tasks."
  - task_type: "Boolean expressions"
    input: "Evaluate the expression: not ( True and False ) or ( False and not True )"
    options:
      - "True"
      - "False"
    success:
      answer: "A — True"
      why: "(True and False) = False, so not(False) = True; True or (anything) = True, so the whole expression is True."
    failure:
      answer: "B — False"
      why: "It mis-scopes the operators — applying `not` to the final `or` result rather than binding `not` most tightly, which reverses the answer."
    checks: "Applying Boolean operator precedence (not, then and, then or) correctly."
    scoring: "Exact-match accuracy on the selected option, averaged across the 23 tasks."
  - task_type: "Tracking shuffled objects (swaps)"
    input: "Alice, Bob, and Claire each hold a ball: Alice has a red ball, Bob a green ball, Claire a blue ball. First Alice and Bob swap balls; then Bob and Claire swap balls. Which ball does Claire hold at the end?"
    options:
      - "the red ball"
      - "the green ball"
      - "the blue ball"
      - "the outcome cannot be determined"
    success:
      answer: "A — the red ball"
      why: "After Alice↔Bob, Bob holds red; the second swap Bob↔Claire then hands that red ball to Claire."
    failure:
      answer: "C — the blue ball"
      why: "It reports Claire's starting ball, ignoring that the second swap moved the blue ball away — a failure to update state through both swaps."
    checks: "Maintaining and updating object state across a sequence of swaps."
    scoring: "Exact-match accuracy on the selected option, averaged across the 23 tasks."
  - task_type: "Word sorting"
    input: "Sort the following words in alphabetical order: epoch, echo, eager, edge."
    options:
      - "eager, echo, edge, epoch"
      - "echo, eager, edge, epoch"
      - "eager, edge, echo, epoch"
      - "epoch, edge, echo, eager"
    success:
      answer: "A — eager, echo, edge, epoch"
      why: "All share 'e', so compare the second letter: a < c < d < p gives eager, echo, edge, epoch."
    failure:
      answer: "C — eager, edge, echo, epoch"
      why: "It transposes echo and edge; comparing left-to-right, echo (e-c) precedes edge (e-d), so this mis-orders the middle pair."
    checks: "Character-by-character lexicographic comparison, not approximate alphabetization."
    scoring: "Exact-match accuracy on the selected option, averaged across the 23 tasks."
tags: [reasoning, chain-of-thought, mixed-format]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2210.09261", title: "Challenging BIG-Bench Tasks and Whether Chain-of-Thought Can Solve Them (Suzgun et al., 2022)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2206.04615", title: "Beyond the Imitation Game (BIG-bench) (Srivastava et al., 2022)", trust: 0.9 }
related: [benchmark/mmlu-pro, benchmark/drop]
---

## What It Tests
A spread of hard reasoning skills — symbolic logic, multi-hop deduction, arithmetic word problems,
date and object tracking, sarcasm and causal judgment. The unifying theme is that pre-CoT models
underperformed humans on all of them.

## How It Scores
Each of the 23 tasks is scored on its own (exact match or multiple choice) and the results are
averaged. Because the tasks are so different, the per-task breakdown is far more informative than the
single average — a strong average can mask a task where the model scores near zero.

## Format
Three-shot with chain-of-thought is the canonical protocol; CoT is what makes BBH tractable and is
the reason the benchmark is famous. Without CoT, scores collapse.

## Limitations
The **heterogeneity** is the main caveat: aggregating logic puzzles with word-sorting and causal
judgment produces a number that is hard to interpret. CoT-dependence means the score partly measures
prompt engineering. Small per-task sizes add noise.

## Example
(Logical deduction) "A is to the left of B. C is to the right of B. D is to the left of A. What is
the left-to-right order?" Chaining the constraints gives D < A < B < C, so the answer is **D, A, B,
C**. A model that ignores the constraints and answers alphabetically ("A, B, C, D") fails.
