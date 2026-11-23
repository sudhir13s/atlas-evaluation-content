---
id: benchmark/aa-omniscience
type: benchmark
slug: aa-omniscience
schema_version: 3.1
title: "AA-Omniscience"
name: AA-Omniscience (v1.1)
category: Truthfulness & Safety
description: "An Artificial Analysis benchmark of factual recall and hallucination: it rewards correct answers, penalises wrong guesses and treats abstaining as neutral."
what_is_it: "AA-Omniscience measures factual recall and knowledge calibration. It was built by Artificial Analysis (paper by Jackson, Keating, Cameron and Hill-Smith, November 2025), whose original version has 6,000 questions covering 42 economically relevant topics in six domains. Version 1.1 has 3,000 questions: it dropped saturated questions, removed the 600 published in the v1 public set, added new ones and fixed ambiguous or erroneous ones. A 300-question public subset (10%) is released; the other 2,700 stay private. Scores are not comparable with version 1."
why_people_use_it: "Most evaluations measure general capability, but reliable use needs factual accuracy and knowing what you do not know. AA-Omniscience separates a model that guesses from one that abstains when unsure. Artificial Analysis includes it in its Intelligence Index through two components, accuracy (10%) and 1 minus the hallucination rate (5%)."
examples:
  - "From the public v1.1 subset on Hugging Face (Apache 2.0), domain Science Engineering and Mathematics, subtopic Biology: a question asking which two microtubules are linked by an electron-dense strut or wing. Gold answer: 'One of the uppermost microtubules and the subtending singlet microtubule.'"
method: "Each answer is graded CORRECT, INCORRECT, PARTIAL_ANSWER or NOT_ATTEMPTED against the gold answer by a grading model (Artificial Analysis names GPT-5.6 Luna, medium, on its methodology page). The score awards points for correct answers, subtracts points for hallucinated ones and keeps abstentions neutral. Only the 300-question public subset can be run outside Artificial Analysis."
metric: "Omniscience Index, bounded between -100 and 100, where 0 means as many correct as incorrect answers. Artificial Analysis also reports accuracy and 1 minus the hallucination rate separately."
format: "Open-answer questions, answered without tools, one attempt each (3,000 questions in v1.1)."
limitations: "Version 1.1 and version 1 are not comparable. Grading uses a model, so grader behaviour affects scores. Ninety percent of the questions are private, so outsiders can check only the public subset and cannot reproduce official scores. The Intelligence Index uses accuracy and non-hallucination rate as two components of different weights, so a headline index number hides the split."
citation: "https://arxiv.org/abs/2511.13029"
openness: partly-open
openness_note: "A 300-question public subset of v1.1 (10%) is on Hugging Face under Apache 2.0 and the scoring is described in the paper, while the other 2,700 questions are kept private by Artificial Analysis."
tags: [truthfulness, hallucination, factual-recall, calibration, artificial-analysis]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2511.13029", title: "AA-Omniscience: Evaluating Cross-Domain Knowledge Reliability in Large Language Models (arXiv:2511.13029)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (AA-Omniscience v1.1)", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/ArtificialAnalysis/AA-Omniscience-Public-v1.1", title: "ArtificialAnalysis/AA-Omniscience-Public-v1.1 — Hugging Face dataset card (Apache 2.0)", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/ArtificialAnalysis/AA-Omniscience-Public", title: "ArtificialAnalysis/AA-Omniscience-Public — v1 public set (600 questions)", trust: 0.9 }
related: [leaderboard/artificial-analysis]
---

## What It Tests
Whether a model **knows facts and knows when it does not**.

- **Coverage** — 42 economically relevant topics in six domains, such as business, health, law and software engineering.
- **Calibration** — abstaining beats guessing wrong. Calibration means a model's willingness to answer matches what it actually knows.
- **Maker** — Artificial Analysis, which runs it for its own leaderboard and Intelligence Index.

## How It Scores
The **Omniscience Index** runs from -100 to 100.

- **Correct** — adds points.
- **Hallucinated** — a wrong answer stated as fact subtracts points.
- **Not attempted** — neutral.
- **Zero** — as many right answers as wrong ones.
- **Grader** — a grading model labels each answer correct, incorrect, partial or not attempted.

## Format
Open answer, no tools, one attempt per question.

- **Size** — version 1.1 has 3,000 questions, down from 6,000 in version 1.
- **Public subset** — 300 questions (10%, same domain mix) on Hugging Face under **Apache 2.0**; the other 2,700 are private.
- **Earlier public set** — version 1 released 600 questions; those were removed from v1.1.

## Limitations
- **Version break.** v1.1 scores cannot be compared with v1.
- **Mostly private.** Outsiders can run only the 300 public questions, not the official set.
- **Model grader.** A grading model decides the labels.

## Example
From the public v1.1 subset on Hugging Face (Apache 2.0), domain Science Engineering and Mathematics, subtopic Biology:

- **Question** — asks which two microtubules are linked by an electron-dense strut or wing.
- **Gold answer** — "One of the uppermost microtubules and the subtending singlet microtubule."
- **Scoring** — that answer earns points, a confident wrong pair loses points, and "I don't know" scores zero.
