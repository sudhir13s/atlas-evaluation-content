---
id: evaluation-metric/context-recall
type: evaluation-metric
slug: context-recall
schema_version: 4.0
title: Context Recall
name: Context Recall
category: retrieval
measures: A high score means the retrieved contexts support most of the claims in the reference answer, so little needed information was missed.
needs:
- question
- retrieved contexts
- reference answer
judged_by: model
range:
  min: 0
  max: 1
direction: higher-is-better
defined_in:
  title: 'Ragas documentation: Context Recall'
  url: https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/context_recall/
implemented_by:
- framework/ragas
related_metrics:
- evaluation-metric/context-precision
tags:
- rag
- retrieval
- recall
- llm-as-judge
updated: '2026-11-21'
sources:
- url: https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/context_recall/
  title: 'Ragas documentation: Context Recall'
  trust: 0.9
---

## What It Measures

Context Recall, **as defined by Ragas**, scores a retriever on **coverage**: how much of the information needed to answer was actually retrieved. It penalises omissions.

## How It Is Computed

- **Default (model-judged):** split the reference answer into claims; a language model decides whether each claim can be attributed to the retrieved contexts.
- **Formula:** Context Recall = (reference claims supported by the retrieved contexts) / (total claims in the reference).
- **Why a reference answer:** Ragas uses it as a stand-in for annotated reference contexts, which take long to label.
- **Non-model variant:** |relevant contexts retrieved| / |total reference contexts|, with relevance decided by string comparison; needs reference contexts.
- **ID-based variant:** reference context IDs found among the retrieved IDs, divided by the total reference context IDs.
- **Range:** 0 to 1.

## Limits

- **A reference is always required,** since the metric checks for missing information.
- **The reference answer is a proxy** for reference contexts in the default variant.
- **ID-based variant** compares IDs only, not content.
