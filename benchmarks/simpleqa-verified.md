---
id: benchmark/simpleqa-verified
type: benchmark
slug: simpleqa-verified
schema_version: 3.1
title: "SimpleQA Verified"
name: SimpleQA Verified
category: Truthfulness & Safety
description: "A 1,000-prompt short-form factuality benchmark from Google DeepMind and Google Research, a cleaned and rebalanced version of OpenAI's SimpleQA, answered without tools."
what_is_it: "SimpleQA Verified (2025, by Haas, Yona, D'Antonio, Goldshtein and Das of Google DeepMind and Google Research) measures short-form factuality from a model's own (parametric) knowledge. It is built from OpenAI's SimpleQA, a set of short fact-seeking questions with a single answer. The authors cut the original 4,326 questions to 1,000 to fix noisy and incorrect labels, topical biases and question redundancy. Answer types are dates, numbers, places or people, across topics such as art, sports, geography and music."
why_people_use_it: "It tests whether a model knows facts and holds back when it does not, on a set built to fix the original SimpleQA's noisy labels. The paper's filtering removes erroneous and ambiguous questions and then keeps headroom by discarding questions that all leading frontier models already answer correctly. The dataset, evaluation code and leaderboard are published on Kaggle, and the data is also on Hugging Face under the MIT licence."
examples:
  - "From the paper's Table 1: 'On what day, month, and year was Algerian artist Mohammed Racim born?' (answer: June 24, 1896)"
method: "Models are run with their standard API parameters and without tools. An autorater (gpt-4.1-2025-04-14) grades each answer as CORRECT, INCORRECT or NOT_ATTEMPTED with a revised grading prompt: numeric answers carry an explicit acceptable range, and a hedged answer counts only if it eventually commits to one candidate, otherwise it is not attempted."
metric: "F1: the harmonic mean of overall accuracy (correct answers / all questions) and accuracy given attempted (correct answers / attempted questions), higher is better. The paper also reports accuracy, accuracy given attempted, attempted rate and hedged rate."
format: "0-shot short-form question answering; one short factual answer per question, no tools."
limitations: "The authors state it measures parametric factuality only: with tools enabled, models reach near-perfect scores. They also note that removing erroneous and ambiguous questions makes the set easier, which they offset by selecting challenging questions. It is a different question set and grader from the original SimpleQA (1,000 of its 4,326 questions, with a revised grading prompt), so the two are separate measurements. All 1,000 questions and answers are public, so they can leak into training data."
citation: "https://arxiv.org/abs/2509.07968"
openness: open
openness_note: "All 1,000 questions with answers are public on Hugging Face under the MIT licence, and Kaggle hosts the evaluation code with the autorater prompt and the leaderboard."
tags: [factuality, hallucination, short-form, parametric-knowledge, google-deepmind]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2509.07968", title: "SimpleQA Verified: A Reliable Factuality Benchmark to Measure Parametric Knowledge (arXiv:2509.07968)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2509.07968", title: "SimpleQA Verified, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/google/simpleqa-verified", title: "google/simpleqa-verified — Hugging Face dataset card (MIT)", trust: 0.9 }
  - { url: "https://www.kaggle.com/benchmarks/deepmind/simpleqa-verified", title: "SimpleQA Verified leaderboard (Kaggle)", trust: 0.85 }
  - { url: "https://arxiv.org/abs/2411.04368", title: "Measuring short-form factuality in large language models (SimpleQA, arXiv:2411.04368)", trust: 0.95 }
---

## What It Tests
Whether a model **knows short facts** from its own training, and abstains when it does not.

- **Short answers** — a date, number, place or person.
- **One right answer** — each question has a single answer, as in SimpleQA.
- **No tools** — the model answers from memory only.
- **Makers** — Google DeepMind and Google Research, 2025.

## How It Scores
An **autorater** (an automatic grading model, gpt-4.1-2025-04-14) labels each answer.

- **Labels** — correct, incorrect or not attempted.
- **Headline** — F1, the harmonic mean of overall accuracy (correct / all questions) and accuracy given attempted (correct / attempted questions).
- **Hedging** — an answer that lists several candidates without committing to one counts as not attempted.
- **Numbers** — the gold answer gives an allowed range; any answer inside it is correct.

## Format
1,000 short fact-seeking questions answered zero-shot with standard API settings.

- **Data** — on Hugging Face (`google/simpleqa-verified`, **MIT licence**), with columns for the question, answer, topic, answer type and source URLs.
- **Code** — Kaggle hosts starter code with the autorater prompt, and the leaderboard.

## Limitations
- **Parametric only.** With tools enabled, models score near-perfect.
- **Easier labels.** Cleaning removed noise; the authors selected challenging questions to compensate.
- **Not SimpleQA.** A different question set and grader from the original 4,326-question SimpleQA.
- **Public items.** The questions are online and can leak into training data.

## Example
From the paper's Table 1:

- **Question** — "On what day, month, and year was Algerian artist Mohammed Racim born?"
- **Expected answer** — **June 24, 1896**.
- **Grading** — a different date is incorrect; declining, or hedging between dates without committing, is not attempted.
