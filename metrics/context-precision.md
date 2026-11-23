---
id: evaluation-metric/context-precision
type: evaluation-metric
slug: context-precision
schema_version: 4.0
title: Context Precision
name: Context Precision
category: retrieval
measures: A high score means the relevant chunks among the retrieved contexts are ranked above the irrelevant ones.
needs:
- question
- retrieved contexts
- reference answer
judged_by: model
direction: higher-is-better
defined_in:
  title: 'Ragas documentation: Context Precision'
  url: https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/context_precision/
implemented_by:
- framework/ragas
related_metrics:
- evaluation-metric/context-recall
- evaluation-metric/normalized-discounted-cumulative-gain
tags:
- rag
- retrieval
- ranking
- llm-as-judge
updated: '2026-11-21'
sources:
- url: https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/context_precision/
  title: 'Ragas documentation: Context Precision'
  trust: 0.9
---

## What It Measures

Context Precision, **as defined by Ragas**, scores a retriever on **ranking**: whether the chunks that are relevant to the question sit near the top of the retrieved list.

## How It Is Computed

Ragas defines it as the mean of precision at each rank, counted only at ranks that hold a relevant chunk.

- **Formula:** Context Precision@K = sum over k = 1..K of (Precision@k × v_k), divided by the total number of relevant items in the top K results.
- **Precision@k** = true positives@k / (true positives@k + false positives@k).
- **K** is the number of chunks in the retrieved contexts; **v_k** is 1 when the chunk at rank k is relevant, else 0.
- **Relevance judge:** in the default variant a language model decides whether each chunk is relevant, using the question and the reference answer.
- **Other variants** in the same documentation: one judged against the generated response instead of a reference, one that compares chunks with reference contexts by string similarity (no model), and one that compares context IDs.

## Limits

- **Order-sensitive by design:** the documentation shows that moving one irrelevant chunk from second to first place lowers the score.
- **The non-model variant** needs reference contexts rather than a reference answer.
- **ID-based variant** compares IDs only, not content.
