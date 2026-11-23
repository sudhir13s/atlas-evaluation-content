---
id: evaluation-metric/answer-relevancy
type: evaluation-metric
slug: answer-relevancy
schema_version: 4.0
title: Answer Relevancy
name: Answer Relevancy
category: generation
measures: A high score means the answer addresses the question directly, without missing parts or unneeded detail.
needs:
- question
- answer
judged_by: model
direction: higher-is-better
defined_in:
  title: 'Ragas documentation: Answer Relevancy (Response Relevancy)'
  url: https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/answer_relevance/
implemented_by:
- framework/ragas
related_metrics:
- evaluation-metric/faithfulness
tags:
- rag
- generation
- relevance
- embeddings
- llm-as-judge
updated: '2026-11-21'
sources:
- url: https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/answer_relevance/
  title: 'Ragas documentation: Answer Relevancy (Response Relevancy)'
  trust: 0.9
---

## What It Measures

Answer Relevancy, **as defined by Ragas** (also called Response Relevancy), scores how **directly an answer addresses the question**. It does not check factual accuracy.

## How It Is Computed

- **Step 1:** a language model writes N questions that the answer would respond to (N = 3 by default, set by `strictness`).
- **Step 2:** an embedding model embeds the original question and each generated question.
- **Formula:** Answer Relevancy = (1/N) × sum over i of cos(E_gi, E_o), where E_gi is the embedding of the i-th generated question and E_o the embedding of the original question.
- **Range:** the documentation says the score usually falls between 0 and 1 but is not guaranteed to, because cosine similarity ranges from -1 to 1.

## Limits

- **No accuracy check:** a wrong answer can score high if it is on topic.
- **Penalises** incomplete answers and answers with unnecessary detail.
- **Model-dependent:** the score depends on the chosen language model and embedding model.
