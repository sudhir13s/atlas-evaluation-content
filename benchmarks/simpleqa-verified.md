---
id: benchmark/simpleqa-verified
type: benchmark
slug: simpleqa-verified
schema_version: 1.0
title: "SimpleQA Verified"
name: SimpleQA Verified
category: Truthfulness & Safety
description: "A 1,000-prompt short-form factuality benchmark from Google DeepMind and Google Research, a cleaned and rebalanced version of OpenAI's SimpleQA, answered without tools."
what_is_it: "SimpleQA Verified measures short-form factuality from a model's own (parametric) knowledge. It is built from OpenAI's SimpleQA, a set of short fact-seeking questions written so that only one indisputable answer exists. The authors cut the original 4,326 questions to 1,000 to fix noisy labels, topic bias and redundant questions. Answers are dates, numbers, places or people, across topics such as art, sports, geography and music."
why_people_use_it: "It tests whether a model knows facts and holds back when it does not, on a set built to fix the original SimpleQA's noisy labels. The paper's filtering removes duplicates, reconciles answers against sources and keeps headroom by dropping questions that all leading frontier models already solve. The dataset, evaluation code and leaderboard are published on Kaggle."
method: "Models are run with their standard API parameters and without tools. An autorater (gpt-4.1-2025-04-14) grades each answer as CORRECT, INCORRECT or NOT_ATTEMPTED with a revised grading prompt: numeric answers carry an explicit acceptable range, only the part of the response that answers the question is graded, and a hedged answer must commit to one candidate or counts as not attempted."
metric: "F1: the harmonic mean of overall accuracy and accuracy given attempted (higher is better). The paper also reports accuracy, accuracy given attempted, attempted rate and hedged rate."
format: "0-shot short-form question answering; one short factual answer per question, no tools."
limitations: "The authors state it measures parametric factuality only: with tools such as search, models reach near-perfect scores. They also note that cleaning the labels makes the set easier, which they offset by keeping the hardest questions. It is a different question set and grader from the original SimpleQA (1,000 of its 4,326 questions, with a revised grading prompt), so the two are separate measurements."
citation: "https://arxiv.org/abs/2509.07968"
tags: [factuality, hallucination, short-form, parametric-knowledge, google-deepmind]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2509.07968", title: "SimpleQA Verified: A Reliable Factuality Benchmark to Measure Parametric Knowledge (arXiv:2509.07968)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2509.07968", title: "SimpleQA Verified, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://www.kaggle.com/benchmarks/deepmind/simpleqa-verified", title: "SimpleQA Verified leaderboard (Kaggle)", trust: 0.85 }
  - { url: "https://arxiv.org/abs/2411.04368", title: "Measuring short-form factuality in large language models (SimpleQA, arXiv:2411.04368)", trust: 0.95 }
---

## What It Tests
Whether a model **knows short facts** from its own training, and abstains when it does not.

- **Short answers** — a date, number, place or person.
- **One right answer** — each question has a single indisputable answer, as in SimpleQA.
- **No tools** — the model answers from memory only.

## How It Scores
An **autorater** (gpt-4.1-2025-04-14) labels each answer.

- **Labels** — correct, incorrect or not attempted.
- **Headline** — F1 of overall accuracy and accuracy given attempted.
- **Hedging** — an answer that lists several candidates without committing counts as not attempted.

## Format
1,000 short fact-seeking questions answered zero-shot with standard API settings. Numeric answers have an acceptable range.

## Limitations
- **Parametric only.** With search tools, models score near-perfect.
- **Easier labels.** Cleaning removed noise; the authors kept the hardest questions to compensate.
- **Not SimpleQA.** A different question set and grader from the original 4,326-question SimpleQA.

## Example
The paper gives this question: "On what day, month, and year was Algerian artist Mohammed Racim born?" The expected answer is **June 24, 1896**. A different date is graded incorrect; an answer that declines or hedges between dates without committing is graded not attempted.
