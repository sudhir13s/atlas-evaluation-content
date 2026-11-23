---
id: benchmark/hle
type: benchmark
slug: hle
schema_version: 4.0
title: Humanity's Last Exam
name: Humanity's Last Exam (HLE)
category: Frontier / Hardest
description: A multi-modal benchmark of 2,500 expert-written, closed-ended academic questions, built so that frontier models score low.
what_is_it: Humanity's Last Exam (HLE) is from the Center for AI Safety and Scale AI, with a consortium of contributors (paper first posted in January 2025; Phan, Gatti, Han and many others). It has 2,500 questions across dozens of subjects, including mathematics, the humanities and the natural sciences. Questions are exact-match (76%) or multiple-choice (24%), each has a known unambiguous answer that automatic grading can check, and about 14% need both text and an image. The authors say questions came from nearly 1,000 subject experts at over 500 institutions in 50 countries.
why_people_use_it: The authors note that models exceed 90% accuracy on popular benchmarks such as MMLU, which limits informed measurement. HLE was designed to be the final closed-ended academic benchmark of its kind with broad subject coverage, with questions that cannot be quickly answered via internet retrieval. The paper was published in Nature (volume 649, 2026), and the authors maintain a private held-out set to assess overfitting.
method: A system prompt asks the model for an answer and a confidence from 0% to 100%. In the paper an LLM judge (o3-mini) checks the final answer against the key. Evaluations may run without tools (closed book) or with tools such as web search and code.
metric: 'Accuracy in percent (correct answers / total questions, higher is better), plus RMS (root mean square) calibration error: a well-calibrated model''s stated confidence matches its actual accuracy. Variants differ by tools, by text-only versus full set, and by judge.'
format: '0-shot question answering: exact-match questions (the model gives an exact string) and multiple-choice questions. Artificial Analysis runs only the 2,158 text-only questions for its separate HLE entry.'
limitations: HLE tests only closed-ended academic problems with verifiable answers, not open-ended research or creative work. Public questions can leak into training data; the authors keep a private held-out set to detect overfitting, and the gated dataset carries a canary string and asks users not to re-share it. Scores differ by tool use, subset and judge model, so a number needs those details. The refined 1,000-question subset HLE-Diamond is a separate record and its scores are not comparable with scores on the full set.
citation: https://arxiv.org/abs/2501.14249
openness: open
openness_note: All 2,500 scored questions with answers are downloadable from the gated Hugging Face dataset (MIT licence, accept conditions first), while a separate private held-out set is kept by the authors only to check for overfitting.
tags:
- frontier
- expert-questions
- multimodal
- closed-ended
- nature
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2501.14249
  title: Humanity's Last Exam (arXiv:2501.14249)
  trust: 0.95
- url: https://arxiv.org/html/2501.14249
  title: Humanity's Last Exam, full text (arXiv HTML)
  trust: 0.95
- url: https://lastexam.ai
  title: Humanity's Last Exam — project site
  trust: 0.9
- url: https://huggingface.co/datasets/cais/hle
  title: cais/hle — Hugging Face dataset card (gated, MIT)
  trust: 0.9
- url: https://artificialanalysis.ai/methodology/intelligence-benchmarking
  title: Artificial Analysis Intelligence Benchmarking Methodology (HLE entry)
  trust: 0.85
related:
- benchmark/hle-diamond
---

## What It Tests

Whether a model has **expert-level academic knowledge and reasoning** on closed questions at the edge of human knowledge.

- **Breadth** — dozens of subjects: mathematics, humanities, natural sciences.
- **Verifiable answers** — each question has a single known, unambiguous solution.
- **Not search-friendly** — the authors say answers cannot be found quickly online.
- **Makers** — the Center for AI Safety and Scale AI, with nearly 1,000 expert contributors.

## How It Scores

An **LLM judge** (a language model used as grader; o3-mini in the paper) checks each answer against the known solution.

- **Accuracy** — correct answers divided by total questions.
- **Calibration** — the paper reports RMS calibration error from the stated confidence; a well-calibrated model is right about as often as it claims.
- **Settings** — closed book or with tools; always cite which.

## Format

2,500 multi-modal questions answered zero-shot: 24% multiple-choice and the rest exact-match, with about 14% needing an image. The prompt asks for an answer and a confidence.

- **Data** — the gated Hugging Face dataset `cais/hle` (**MIT licence**); users must accept conditions and are asked not to re-share or re-upload it.
- **Held-out set** — the authors keep extra private questions to detect overfitting.
- **Third-party runs** — Artificial Analysis keeps to the 2,158 text-only questions for its own HLE entry.

## Limitations

- **Narrow construct.** Closed-ended academic questions only, not open-ended research or creative work.
- **Leak risk.** Questions are downloadable, so the authors hold a private test set and the data carries a canary string.
- **Setup-dependent.** Tool use, subset and judge change the number.
- **Variant.** HLE-Diamond, a refined 1,000-question subset, has its own record.

## Example

No question is quoted here: the dataset is gated and the maintainers ask that it not be shared publicly. As the maker describes the format:

- **Samples on lastexam.ai** — for instance, a Classics question asking for a translation of a Palmyrene inscription from a Roman tombstone, and an ecology question on hummingbird anatomy.
- **Each item** — question text (sometimes with an image), and either an exact-match answer or multiple-choice options with the right one marked.
- **Grading** — the model returns an answer and a confidence; the judge marks the answer right or wrong against the key.
