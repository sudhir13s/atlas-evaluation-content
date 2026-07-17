---
id: benchmark/drop
type: benchmark
slug: drop
schema_version: 1.0
title: "DROP"
name: DROP
category: Math & Logic
description: "Discrete Reasoning Over Paragraphs — reading comprehension that requires computation over the text."
what_is_it: "A 96,000-question reading-comprehension benchmark where answering requires discrete operations — addition, subtraction, counting, sorting, comparison — over information stated in a passage. Questions were adversarially crowdsourced against a strong QA model, so shallow span-extraction fails."
why_people_use_it: "It bridges reading comprehension and numerical reasoning: the model must both find the right facts in a paragraph and compute over them. It is part of several leaderboard suites and tests a skill neither pure math nor pure QA benchmarks capture."
examples:
  - "Passage: 'In 2010 the city had 250,000 residents; by 2015 it grew 12%; in 2020 it fell 5% from 2015.' Q: population in 2020? (answer: 250,000 * 1.12 * 0.95 = 266,000)"
method: "Few-shot or 0-shot; free-form answers (numbers, spans, or sets of spans) are compared to gold using a numeracy-aware F1 plus exact match."
metric: "F1 (numeracy-aware) and exact match, 0-100."
format: "Few-shot; free-response with F1/EM scoring."
saturation: medium
limitations: "Some questions have ambiguous or multiple valid answers, capping achievable F1. F1 gives partial credit, which can flatter near-misses. Answer normalization (numbers, dates, span sets) is fiddly and a source of scoring noise."
citation: "https://arxiv.org/abs/1903.00161"
tags: [reading-comprehension, numerical-reasoning, f1]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/1903.00161", title: "DROP: A Reading Comprehension Benchmark Requiring Discrete Reasoning Over Paragraphs (Dua et al., 2019)", trust: 0.95 }
related: [benchmark/gsm8k, benchmark/bbh]
---

## What It Tests
Combined comprehension and computation: extract the relevant numbers, dates, or entities from a
passage and perform a discrete operation (arithmetic, counting, sorting, comparison) to produce the
answer. Neither reading nor math alone is enough.

## How It Scores
A numeracy-aware **F1** (which understands that "266,000" and "266000" match, and gives partial
credit on multi-span answers) alongside **exact match**, both on a 0–100 scale. F1's partial credit
is why it, not accuracy, is the headline — it rewards getting the right numbers even if the final
combination is slightly off.

## Format
Few-shot or 0-shot; the model emits a free-form answer (a number, a text span, or a set of spans)
that is normalized and compared to the gold answer(s).

## Limitations
**Ambiguity**: some items admit more than one defensible answer, which caps the achievable F1 and
adds label noise. F1's partial credit can make a model look better than exact reasoning would show.
Answer normalization across numbers, dates, and span sets is error-prone.

## Example
Passage: "In 2010 the city had 250,000 residents. By 2015 the population grew by 12%. In 2020 it
decreased by 5% from the 2015 figure." Q: "What was the population in 2020?" Correct chain:
250,000 × 1.12 = 280,000, then 280,000 × 0.95 = **266,000**. Adding the percentages directly
(250,000 × 1.07 = 267,500) is the classic error the item catches.
