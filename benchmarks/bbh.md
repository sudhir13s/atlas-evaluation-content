---
id: benchmark/bbh
type: benchmark
slug: bbh
schema_version: 1.0
title: "BBH (BIG-Bench Hard)"
name: BBH
category: Knowledge & Academic Reasoning
description: "BIG-Bench Hard: 23 BIG-Bench tasks on which earlier language model evaluations did not beat the average human rater."
what_is_it: "BIG-Bench Hard (BBH) was introduced by Suzgun et al. (2022) as a suite of 23 challenging tasks from BIG-Bench, chosen because prior language model evaluations did not outperform the average human rater on them. It has 6,511 evaluation examples: 250 per task, except for three tasks that use all available examples. Tasks include logical deduction, tracking shuffled objects, date understanding and causal judgement."
why_people_use_it: "The paper showed that chain-of-thought (CoT) prompting lifts performance on many BBH tasks, and that few-shot prompting without CoT underestimates what models can do on them, so BBH is used to measure multi-step reasoning."
examples:
  - "(Logical deduction) A is left of B. C is right of B. D is left of A. Order left to right? (correct: D, A, B, C)"
method: "Few-shot prompting with three exemplars per task, written by the authors with and without chain-of-thought reasoning; results are reported per task and averaged."
metric: "Accuracy per task, averaged across the 23 tasks."
format: "3-shot, with or without chain-of-thought."
limitations: "The 23 tasks are very different, so an average can hide a task on which a model fails. Scores depend strongly on whether chain-of-thought prompting is used, so the prompting setup must be stated."
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2210.09261", title: "Challenging BIG-Bench Tasks and Whether Chain-of-Thought Can Solve Them (Suzgun et al., 2022)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/2210.09261", title: "BBH paper, full text (ar5iv)", trust: 0.95 }
related: [benchmark/mmlu-pro, benchmark/drop]
---

## What It Tests
**Multi-step reasoning** across 23 varied tasks, such as logical deduction, date understanding, tracking shuffled objects and causal judgement.

## How It Scores
Each task is scored on its own and the results are averaged.

- **Size** — 6,511 evaluation examples, 250 for most tasks.
- **Per-task view** — the breakdown says more than the average.

## Format
Three exemplars per task, which the authors wrote both as answer-only and with chain-of-thought reasoning.

## Limitations
- **Heterogeneous.** One average covers very different tasks.
- **Prompt-dependent.** Chain-of-thought changes scores markedly.

## Example
(Logical deduction) "A is to the left of B. C is to the right of B. D is to the left of A. What is
the left-to-right order?" Chaining the constraints gives D < A < B < C, so the answer is **D, A, B,
C**. A model that ignores the constraints and answers alphabetically ("A, B, C, D") fails.
