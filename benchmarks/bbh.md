---
id: benchmark/bbh
type: benchmark
slug: bbh
schema_version: 4.0
title: BBH (BIG-Bench Hard)
name: BBH
category: Knowledge & Academic Reasoning
description: 'BIG-Bench Hard: 23 BIG-Bench tasks on which earlier language model evaluations did not beat the average human rater.'
what_is_it: 'BIG-Bench Hard (BBH) was introduced in October 2022 by Suzgun, Scales, Schärli and colleagues as a suite of 23 challenging tasks from BIG-Bench, chosen because prior language model evaluations did not outperform the average human rater on them. It has 6,511 evaluation examples: a random 250 per task, except Causal Judgement (187), Penguins in a Table (146) and Snarks (178), which use all available test examples. Tasks include logical deduction, tracking shuffled objects, date understanding and causal judgement.'
why_people_use_it: The paper showed that chain-of-thought (CoT) prompting lifts performance on many BBH tasks, and that few-shot prompting without CoT underestimates what models can do on them, so BBH is used to measure multi-step reasoning.
method: Few-shot prompting with three exemplars per task, written by the authors with and without chain-of-thought reasoning; greedy decoding, with the final answer extracted after a phrase such as 'the answer is'. Results are reported per task and averaged.
metric: Exact-match accuracy per task (generated answer compared with the ground-truth label; correct answers divided by examples), averaged across the 23 tasks.
format: 3-shot, with or without chain-of-thought; multiple-choice or free-form exact-match answers depending on the task.
limitations: The 23 tasks are very different, so an average can hide a task on which a model fails. Scores depend strongly on whether chain-of-thought prompting is used, so the prompting setup must be stated. The task files are public and carry a canary string asking that they never appear in training corpora.
citation: https://arxiv.org/abs/2210.09261
openness: open
openness_note: All 23 task files, the chain-of-thought prompts and reference model outputs are public in the authors' GitHub repository under the MIT licence.
worked_examples:
- task_type: Logical deduction (multi-step ordering)
  input: Four books A, B, C, D sit in a row. A is to the left of B. C is to the right of B. D is to the left of A. What is the left-to-right order?
  options:
  - D, A, B, C
  - A, B, C, D
  - A, D, B, C
  - D, B, A, C
  success:
    answer: A — D, A, B, C
    why: Chaining the three constraints gives D < A < B < C, the only ordering that satisfies all of them.
  failure:
    answer: C — A, D, B, C
    why: It honors A<B and C rightmost but places D to the right of A, violating 'D is to the left of A' — a partial-constraint slip.
  checks: Composing several constraints into one consistent ordering via chain-of-thought, not partial constraint satisfaction.
  scoring: Exact-match accuracy on the selected option, averaged across the 23 tasks.
- task_type: Date understanding
  input: Yesterday was April 30, 2021. What is the date tomorrow, in MM/DD/YYYY?
  options:
  - 05/02/2021
  - 05/01/2021
  - 04/30/2021
  - 05/03/2021
  success:
    answer: A — 05/02/2021
    why: Yesterday = April 30, so today = May 1 and tomorrow = May 2; the reasoning must roll over the month end (April has 30 days).
  failure:
    answer: B — 05/01/2021
    why: It computes 'today' (May 1) instead of 'tomorrow' — an off-by-one error on the relative-date reference, the most common failure on this task.
  checks: Resolving a chain of relative-date references and handling the month boundary correctly.
  scoring: Exact-match accuracy on the selected option, averaged across the 23 tasks.
- task_type: Boolean expressions
  input: 'Evaluate the expression: not ( True and False ) or ( False and not True )'
  options:
  - 'True'
  - 'False'
  success:
    answer: A — True
    why: (True and False) = False, so not(False) = True; True or (anything) = True, so the whole expression is True.
  failure:
    answer: B — False
    why: It mis-scopes the operators — applying `not` to the final `or` result rather than binding `not` most tightly, which reverses the answer.
  checks: Applying Boolean operator precedence (not, then and, then or) correctly.
  scoring: Exact-match accuracy on the selected option, averaged across the 23 tasks.
- task_type: Tracking shuffled objects (swaps)
  input: 'Alice, Bob, and Claire each hold a ball: Alice has a red ball, Bob a green ball, Claire a blue ball. First Alice and Bob swap balls; then Bob and Claire swap balls. Which ball does Claire hold at the end?'
  options:
  - the red ball
  - the green ball
  - the blue ball
  success:
    answer: A — the red ball
    why: After Alice↔Bob, Bob holds red; the second swap Bob↔Claire then hands that red ball to Claire.
  failure:
    answer: C — the blue ball
    why: It reports Claire's starting ball, ignoring that the second swap moved the blue ball away — a failure to update state through both swaps.
  checks: Maintaining and updating object state across a sequence of swaps.
  scoring: Exact-match accuracy on the selected option, averaged across the 23 tasks.
- task_type: Word sorting
  input: 'Sort the following words in alphabetical order: epoch, echo, eager, edge.'
  success:
    answer: eager echo edge epoch
    why: 'All share ''e'', so compare the second letter: a < c < d < p gives eager, echo, edge, epoch.'
  failure:
    answer: eager edge echo epoch
    why: It transposes echo and edge; comparing left-to-right, echo (e-c) precedes edge (e-d), so the generated list does not match the label exactly.
  checks: Character-by-character lexicographic comparison, not approximate alphabetization.
  scoring: Exact match of the generated sorted list against the label (a free-form task with no options), averaged across the 23 tasks.
tags:
- reasoning
- chain-of-thought
- mixed-format
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2210.09261
  title: Challenging BIG-Bench Tasks and Whether Chain-of-Thought Can Solve Them (Suzgun et al., 2022)
  trust: 0.95
- url: https://ar5iv.labs.arxiv.org/html/2210.09261
  title: BBH paper, full text (ar5iv)
  trust: 0.95
- url: https://raw.githubusercontent.com/suzgunmirac/BIG-Bench-Hard/HEAD/README.md
  title: suzgunmirac/BIG-Bench-Hard — official repository README
  trust: 0.9
- url: https://raw.githubusercontent.com/suzgunmirac/BIG-Bench-Hard/main/LICENSE
  title: BIG-Bench-Hard repository licence (MIT)
  trust: 0.9
- url: https://raw.githubusercontent.com/suzgunmirac/BIG-Bench-Hard/main/bbh/date_understanding.json
  title: BBH date_understanding task file (with canary string)
  trust: 0.9
- url: https://raw.githubusercontent.com/suzgunmirac/BIG-Bench-Hard/main/bbh/word_sorting.json
  title: BBH word_sorting task file (free-form targets)
  trust: 0.9
related:
- benchmark/mmlu-pro
- benchmark/drop
---

## What It Tests

**Multi-step reasoning** across 23 varied tasks, such as logical deduction, date understanding, tracking shuffled objects and causal judgement.

- **Origin** — tasks come from BIG-Bench, a large community evaluation suite.
- **Selection** — the authors kept tasks with human-rater baselines, multiple-choice or exact-match metrics and enough examples, where the best prior model had not beaten the average human rater.

## How It Scores

**Exact-match accuracy** per task: the extracted answer must equal the label. Task scores are then averaged.

- **Size** — 6,511 evaluation examples: 250 for most tasks, all available ones for Causal Judgement (187), Penguins in a Table (146) and Snarks (178).
- **Answer extraction** — greedy decoding; the answer is read after a phrase such as "the answer is".
- **Per-task view** — the breakdown says more than the average.

## Format

Three exemplars per task, which the authors wrote both as answer-only and with chain-of-thought (step-by-step) reasoning.

- **Answer types** — some tasks are multiple choice with lettered options; others, such as word sorting, need a free-form answer.
- **Data** — task files in `/bbh` and prompts in `/cot-prompts` of the GitHub repository `suzgunmirac/BIG-Bench-Hard`, under the **MIT licence**.

## Limitations

- **Heterogeneous.** One average covers very different tasks.
- **Prompt-dependent.** Chain-of-thought changes scores markedly.
- **Contamination guard.** Each task file carries a canary string asking that the data never appear in training corpora.

## Example

No item is quoted here, because the task files carry a canary string asking that the data stay out of training corpora. The format, as the date understanding file shows it:

- **Input** — a sentence that fixes a reference date (for example, a holiday in a given year), then asks for the date tomorrow in MM/DD/YYYY.
- **Options** — six lettered dates, (A) to (F).
- **Target** — one letter, such as "(B)"; the model's extracted answer must match it exactly.
