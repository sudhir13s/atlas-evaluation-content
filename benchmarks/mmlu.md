---
id: benchmark/mmlu
type: benchmark
slug: mmlu
schema_version: 1.0
title: "MMLU"
name: MMLU
category: Knowledge & Academic Reasoning
description: "Massive Multitask Language Understanding — 57 subjects of expert-level multiple-choice questions."
what_is_it: "A 4-choice knowledge exam spanning 57 subjects, from elementary math to professional law, medicine, and moral reasoning. It tests breadth of world and academic knowledge rather than a single skill."
why_people_use_it: "It became the default single-number proxy for general knowledge and is reported by nearly every model release, which makes it useful for rough cross-model comparison — as long as you remember the contamination caveat."
examples:
  - "Which of the following is an example of operant conditioning? (A) a dog salivating at a bell (B) a rat pressing a lever more often after food rewards (C) a child imitating a parent (D) blinking at a puff of air"
method: "Few-shot (typically 5-shot) multiple-choice; the answer is the option with the highest likelihood, or parsed from a generated letter. 15,908 questions across 57 tasks."
metric: "Accuracy (% correct), macro-averaged across the 57 subjects."
format: "5-shot multiple-choice (also reported 0-shot CoT for chat models)."
saturation: high
limitations: "Well-documented training-data contamination — many models have seen MMLU items during pre-training, inflating scores. A handful of questions have wrong gold answers. Frontier models cluster at 86–90%, so the top of the scale no longer discriminates; use MMLU-Pro or GPQA instead."
citation: "https://arxiv.org/abs/2009.03300"
tags: [knowledge, multiple-choice, reasoning, contamination]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2009.03300", title: "Measuring Massive Multitask Language Understanding (Hendrycks et al., 2021)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2310.18018", title: "Data Contamination Through the Lens of Time (Sainz et al. discussion of MMLU contamination)", trust: 0.75 }
related: [benchmark/mmlu-pro, benchmark/gpqa]
---

## What It Tests
Breadth of academic and professional knowledge plus light reasoning, across 57 subjects grouped
into STEM, humanities, social sciences, and "other" (business, health, misc). It is a knowledge
exam, not a reasoning or generation test.

## How It Scores
Plain accuracy: the fraction of the 15,908 four-choice questions answered correctly, macro-averaged
so each subject counts equally. Random guessing scores 25%. Human expert (domain-specialist)
performance is ~89.8%; unspecialized humans score far lower on out-of-field subjects.

## Format
Standardly 5-shot: five worked examples are prepended, then the model answers the sixth. Base models
are scored by comparing the likelihood of each answer letter; chat models are often run 0-shot with
chain-of-thought and the letter is parsed from the response. The two protocols are not directly
comparable — always check which was used.

## Limitations
Contamination is the headline problem: MMLU predates most models' training cutoffs, so items leak
into pre-training corpora and scores overstate true generalization. The benchmark is also near
**saturated** — frontier models sit at 86–90%, within noise of each other and of the human ceiling —
and a small number of questions have disputed or incorrect keys. Treat any score above ~90% with
suspicion.

## Example
"Which of the following is an example of operant conditioning?" with options (A) a dog salivating at
a bell, (B) a rat pressing a lever more often after receiving food, (C) a child imitating a parent,
(D) blinking at a puff of air. The correct answer is **(B)** — behavior changed by its consequences.
(A) is classical conditioning, (C) is observational learning, (D) is a reflex.
