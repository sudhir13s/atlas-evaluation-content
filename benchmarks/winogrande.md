---
id: benchmark/winogrande
type: benchmark
slug: winogrande
schema_version: 1.0
title: "WinoGrande"
name: WinoGrande
category: Knowledge & Academic Reasoning
description: "Large-scale Winograd-style pronoun-resolution problems testing common-sense coreference."
what_is_it: "A 44,000-item benchmark of fill-in-the-blank sentences where a pronoun or blank must be resolved to the correct noun using common sense. It scales up the classic Winograd Schema Challenge and uses adversarial filtering (AfLite) to remove dataset-specific word cues."
why_people_use_it: "It targets a narrow but important skill — common-sense coreference — and was part of the original HF Open LLM Leaderboard, so historical scores are broadly available."
examples:
  - "The trophy doesn't fit into the brown suitcase because _ is too large. (correct: the trophy)"
method: "5-shot two-way choice (Open LLM v1) or 0-shot; the model picks which of two candidate fillers makes the sentence coherent."
metric: "Accuracy (% correct)."
format: "5-shot (Open LLM v1) or 0-shot, binary choice."
saturation: high
limitations: "Binary choice means a 50% random floor and a compressed dynamic range. Frontier models score in the high 80s/90s, so it is saturating. AfLite filtering reduces but does not eliminate spurious cues."
citation: "https://arxiv.org/abs/1907.10641"
tags: [common-sense, coreference, multiple-choice]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/1907.10641", title: "WinoGrande: An Adversarial Winograd Schema Challenge at Scale (Sakaguchi et al., 2019)", trust: 0.95 }
related: [benchmark/hellaswag, benchmark/arc-challenge]
---

## What It Tests
Common-sense coreference: resolving an ambiguous pronoun or blank to the right entity based on world
knowledge rather than grammar. Each item comes as a twin pair that differ by one word and flip the
answer, which defeats models that latch onto surface cues.

## How It Scores
Accuracy on a binary choice. Random baseline is 50%; human performance is ~94%. The twin-pair design
means a model must get both members of a pair right to show real understanding, though the headline
metric scores items independently.

## Format
Open LLM v1 used **5-shot**; **0-shot** is also common. The task presents two candidate fillers and
the model chooses the coherent one.

## Limitations
The binary format compresses the score range and caps information per item. It is **saturating** —
top models are in the high 80s to 90s and clustered. AfLite adversarial filtering trims dataset
artifacts but cannot guarantee a model is reasoning rather than exploiting residual statistics.

## Example
"The trophy doesn't fit into the brown suitcase because _ is too large." The blank resolves to **the
trophy** (it is the large thing). Swap "large" for "small" and the answer flips to "the suitcase" —
the twin design that forces genuine reasoning.
