---
id: benchmark/hellaswag
type: benchmark
slug: hellaswag
schema_version: 1.0
title: "HellaSwag"
name: HellaSwag
category: Knowledge & Academic Reasoning
description: "Common-sense sentence completion with adversarially filtered wrong endings."
what_is_it: "A sentence-completion test: given a short context (often from ActivityNet or WikiHow), the model picks the most plausible continuation from four options. The wrong endings are generated and adversarially filtered to fool weaker models while staying obviously wrong to humans."
why_people_use_it: "It was a standard common-sense benchmark on the original HF Open LLM Leaderboard, giving broad historical coverage. It isolates physical/temporal common sense rather than academic knowledge."
examples:
  - "A woman sits at a table with a piece of clay and begins to mold it. She... (correct: rolls it flat and cuts it with a knife; distractor: eats a piece of the clay)"
method: "10-shot multiple-choice was the HF Open LLM v1 protocol; 0-shot is also reported. Score = highest-likelihood ending among 4."
metric: "Accuracy (% correct)."
format: "10-shot (Open LLM v1) or 0-shot, 4-way multiple-choice."
saturation: high
limitations: "Near-saturated: frontier models exceed 95%, matching or beating the ~95.6% human baseline, so it no longer separates strong models. The adversarial filtering was tuned against 2019-era models and does not hold against modern ones. A few items have noisy labels."
citation: "https://arxiv.org/abs/1905.07830"
tags: [common-sense, multiple-choice, completion]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/1905.07830", title: "HellaSwag: Can a Machine Really Finish Your Sentence? (Zellers et al., 2019)", trust: 0.95 }
related: [benchmark/arc-challenge, benchmark/winogrande]
---

## What It Tests
Everyday physical and temporal common sense — knowing how a described activity plausibly continues.
The task is trivial for humans but was hard for pre-2020 models, which is exactly the gap it was
built to expose.

## How It Scores
Accuracy over the validation set. Human baseline is ~95.6%. Random baseline is 25%. Because the set
is large, scores are stable, but the ceiling is so close that top models are indistinguishable.

## Format
The HF Open LLM Leaderboard v1 used **10-shot**; papers also report **0-shot**. As with other
completion benchmarks, base models are scored by ending likelihood.

## Limitations
It is **saturated**. The adversarial-filtering trick (Adversarial Filtering) was calibrated against
2019 models, so its distractors are no longer adversarial to current systems. Use it as a smoke test
for weak common sense, not as a frontier discriminator.

## Example
"A woman is sitting at a table with a piece of clay. She begins to mold the clay into a shape.
She..." The plausible continuation is **"rolls the clay flat and cuts it into pieces with a knife"**;
an adversarial distractor like "eats a piece of the clay" is nonsensical to a human but was designed
to look statistically plausible to weaker models.
