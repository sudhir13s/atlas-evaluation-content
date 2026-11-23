---
id: evaluation-metric/exact-match
type: evaluation-metric
slug: exact-match
schema_version: 4.0
title: Exact Match (EM)
name: Exact Match
category: generation
measures: A high score means a large share of predictions match one of the reference answers exactly.
needs:
- answer
- reference answers
judged_by: code
range:
  min: 0
  max: 100
direction: higher-is-better
defined_in:
  title: 'SQuAD: 100,000+ Questions for Machine Comprehension of Text (Rajpurkar et al., 2016)'
  url: https://arxiv.org/abs/1606.05250
tags:
- generation
- question-answering
- string-match
updated: '2026-11-21'
sources:
- url: https://arxiv.org/abs/1606.05250
  title: 'SQuAD: 100,000+ Questions for Machine Comprehension of Text (arXiv:1606.05250)'
  trust: 0.95
---

## What It Measures

Exact match (EM) scores short answers by **strict string agreement** with a reference. It was defined for the Stanford Question Answering Dataset (SQuAD), together with a token-overlap F1 score.

## How It Is Computed

- **Normalisation:** punctuation and the articles a, an and the are ignored.
- **Per prediction:** it counts as a match if it equals any one of the ground-truth answers exactly.
- **Score:** the percentage of predictions that match.

## Limits

- **Wording, not meaning:** in the paper, most disagreement between humans came from including or leaving out non-essential phrases, not from different answers. Human performance on the test set was 77.0% EM against 86.8% F1.
