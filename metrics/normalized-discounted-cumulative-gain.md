---
id: evaluation-metric/normalized-discounted-cumulative-gain
type: evaluation-metric
slug: normalized-discounted-cumulative-gain
schema_version: 4.0
title: Normalized Discounted Cumulative Gain (nDCG)
name: Normalized Discounted Cumulative Gain
category: retrieval
measures: A high score means the most relevant items are ranked highest, close to the ideal ordering.
needs:
- graded relevance of each item
- system ranking or scores for the items
judged_by: code
range:
  min: 0
  max: 1
direction: higher-is-better
defined_in:
  title: 'scikit-learn documentation: sklearn.metrics.ndcg_score'
  url: https://scikit-learn.org/stable/modules/generated/sklearn.metrics.ndcg_score.html
related_metrics:
- evaluation-metric/mean-reciprocal-rank
- evaluation-metric/context-precision
tags:
- retrieval
- ranking
- graded-relevance
updated: '2026-11-21'
sources:
- url: https://scikit-learn.org/stable/modules/generated/sklearn.metrics.ndcg_score.html
  title: 'scikit-learn documentation: sklearn.metrics.ndcg_score'
  trust: 0.9
- url: https://scikit-learn.org/stable/modules/generated/sklearn.metrics.dcg_score.html
  title: 'scikit-learn documentation: sklearn.metrics.dcg_score'
  trust: 0.9
---

## What It Measures

Normalized discounted cumulative gain (nDCG) scores a ranking against **graded relevance labels**. Relevant items count for more the higher they are ranked, and the result is compared with the best possible ordering.

## How It Is Computed

As the scikit-learn reference implementation documents it:

- **DCG:** sum the true relevance scores in the order induced by the predicted scores, after a logarithmic discount by rank (logarithm base 2 by default; a lower base discounts more sharply).
- **Ideal DCG:** the DCG of a perfect ranking of the same items.
- **nDCG** = DCG / ideal DCG, averaged over samples; optionally only the top k positions count.
- **Range:** 0 to 1.
- **Origin:** the documentation cites Järvelin and Kekäläinen, "Cumulated gain-based evaluation of IR techniques", ACM Transactions on Information Systems 20(4), 2002, pp. 422-446 (not openly available).

## Limits

The reference documentation notes:

- **Negative relevance labels** may give values outside 0 to 1.
- **Ties in predicted scores** are averaged by default; assuming no ties is faster but gives wrong results when ties exist.
