---
id: evaluation-metric/mean-reciprocal-rank
type: evaluation-metric
slug: mean-reciprocal-rank
schema_version: 4.0
title: Mean Reciprocal Rank (MRR)
name: Mean Reciprocal Rank
category: retrieval
measures: A high score means the first correct item usually appears at or near the top of each ranked list.
needs:
- ranked responses per question
- correctness judgement for each response
judged_by: human
range:
  min: 0
  max: 1
direction: higher-is-better
defined_in:
  title: The TREC-8 Question Answering Track (Voorhees and Tice, LREC 2000)
  url: https://aclanthology.org/L00-1018/
related_metrics:
- evaluation-metric/normalized-discounted-cumulative-gain
tags:
- retrieval
- ranking
- question-answering
- trec
updated: '2026-11-21'
sources:
- url: https://aclanthology.org/L00-1018/
  title: The TREC-8 Question Answering Track (Voorhees and Tice, LREC 2000)
  trust: 0.95
- url: https://trec.nist.gov/data/qa.html
  title: TREC Question Answering Collections (NIST)
  trust: 0.9
---

## What It Measures

Mean reciprocal rank (MRR) scores a ranked list by **how early the first correct item appears**. It was the scoring measure of the TREC-8 Question Answering track, run by the US National Institute of Standards and Technology (NIST).

## How It Is Computed

- **Input:** for each question, a ranked list of up to five responses; human assessors judged each response correct or not.
- **Per question:** the reciprocal of the rank of the first correct response, or 0 if none of the five is correct.
- **Per run:** MRR = the mean of the per-question reciprocal ranks.
- **Range:** 0 to 1, inclusive.
- **Example values:** with five ranks, a question can score only 0, 0.2, 0.25, 0.33, 0.5 or 1.

## Limits

The defining paper names these drawbacks:

- **Few distinct values per question,** so parametric significance tests are unlikely to be appropriate.
- **No credit for further correct answers** after the first.
- **No credit for knowing you do not know:** in TREC-8 every question required at least one response.
- **Assessors disagree** on correctness even for short factual answers, the paper's main evaluation finding.
