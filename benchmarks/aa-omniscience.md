---
id: benchmark/aa-omniscience
type: benchmark
slug: aa-omniscience
schema_version: 1.0
title: "AA-Omniscience"
name: AA-Omniscience (v1.1)
category: Truthfulness & Safety
description: "An Artificial Analysis benchmark of factual recall and hallucination: it rewards correct answers, penalises wrong guesses and treats abstaining as neutral."
what_is_it: "AA-Omniscience measures factual recall and knowledge calibration. The original paper describes 6,000 questions derived from authoritative academic and industry sources, covering 42 economically relevant topics in six domains. Version 1.1 has 3,000 questions: it dropped saturated questions, removed the 600 published in the v1 public set, added new ones and fixed ambiguous or erroneous ones. Scores are not comparable with version 1."
why_people_use_it: "Most evaluations measure general capability, but reliable use needs factual accuracy and knowing what you do not know. AA-Omniscience separates a model that guesses from one that abstains when unsure. Artificial Analysis includes it in its Intelligence Index through two components, accuracy and non-hallucination rate."
method: "Each answer is graded CORRECT, INCORRECT, PARTIAL_ANSWER or NOT_ATTEMPTED against the ground truth by a grading model. The Omniscience Index awards points for correct answers, subtracts points for hallucinated ones and keeps abstentions neutral. A public version of the v1.1 dataset is on Hugging Face."
metric: "Omniscience Index, bounded between -100 and 100, where 0 means as many correct as incorrect answers. Artificial Analysis also reports accuracy and 1 minus the hallucination rate separately."
format: "Open-answer questions, answered without tools, one attempt each (3,000 questions in v1.1)."
limitations: "Version 1.1 and version 1 are not comparable. Grading uses a model, so grader behaviour affects scores. The Intelligence Index uses accuracy and non-hallucination rate as two components of different weights, so a headline index number hides the split."
citation: "https://arxiv.org/abs/2511.13029"
tags: [truthfulness, hallucination, factual-recall, calibration, artificial-analysis]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2511.13029", title: "AA-Omniscience: Evaluating Cross-Domain Knowledge Reliability in Large Language Models (arXiv:2511.13029)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (AA-Omniscience v1.1)", trust: 0.9 }
related: [leaderboard/artificial-analysis]
---

## What It Tests
Whether a model **knows facts and knows when it does not**.

- **Coverage** — 42 economically relevant topics in six domains.
- **Sources** — authoritative academic and industry material.
- **Calibration** — abstaining beats guessing wrong.

## How It Scores
The **Omniscience Index** runs from -100 to 100.

- **Correct** — adds points.
- **Hallucinated** — subtracts points.
- **Not attempted** — neutral.
- **Zero** — as many right answers as wrong ones.

## Format
Open answer, no tools. Version 1.1 has 3,000 questions, down from 6,000; a public version of the v1.1 dataset is on Hugging Face.

## Limitations
- **Version break.** v1.1 scores cannot be compared with v1.
- **Published questions drop out.** The 600 questions in the v1 public set were removed from v1.1.
- **Model grader.** A grading model decides the labels.

## Example
The scoring labels show how it works. Asked a factual question, a model that **answers correctly** earns points, one that **states a wrong answer** loses points, and one that **declines** neither gains nor loses. A model that guesses on every question can therefore score below zero even with some right answers.
