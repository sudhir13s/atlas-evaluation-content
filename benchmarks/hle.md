---
id: benchmark/hle
type: benchmark
slug: hle
schema_version: 1.0
title: "Humanity's Last Exam"
name: Humanity's Last Exam (HLE)
category: Frontier / Hardest
description: "A multi-modal benchmark of 2,500 expert-written, closed-ended academic questions, built so that frontier models score low."
what_is_it: "Humanity's Last Exam (HLE) is from the Center for AI Safety and Scale AI, with a consortium of contributors. It has 2,500 questions across dozens of subjects, including mathematics, the humanities and the natural sciences. Questions are exact-match (76%) or multiple-choice with five or more options (24%), each has a known unambiguous answer that automatic grading can check, and about 14% need both text and an image. The authors say questions came from nearly 1,000 subject experts at over 500 institutions in 50 countries."
why_people_use_it: "Popular benchmarks such as MMLU were beaten by large models, which limited informed measurement. HLE was written to be the last closed-ended academic exam of its kind. The questions cannot be answered quickly by an internet search. The paper appeared in Nature in 2026, and the authors maintain a private held-out set to assess overfitting."
method: "A system prompt asks the model for an answer and a confidence from 0% to 100%. In the paper an LLM judge (o3-mini with structured decoding) extracts the final answer and marks it correct or not against the key. Evaluations may run without tools (closed book) or with tools such as web search and code."
metric: "Accuracy in percent (higher is better), plus RMS calibration error: a well-calibrated model's stated confidence matches its actual accuracy. Variants differ by tools, by text-only versus full set, and by judge."
format: "0-shot question answering: exact-match questions (the model gives an exact string) and multiple-choice questions with five or more options. Artificial Analysis runs only the 2,158 text-only questions for its separate HLE entry."
limitations: "HLE tests structured academic problems with closed answers, so the maker notes it says little about open-ended research or creative work, or about general intelligence. Public questions can leak into training data; the authors keep a private held-out set to detect overfitting. Scores differ by tool use, subset and judge model, so a number needs those details. The refined 1,000-question subset HLE-Diamond is a separate record and its scores are not comparable with scores on the full set."
citation: "https://arxiv.org/abs/2501.14249"
tags: [frontier, expert-questions, multimodal, closed-ended, nature]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2501.14249", title: "Humanity's Last Exam (arXiv:2501.14249)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2501.14249", title: "Humanity's Last Exam, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://lastexam.ai", title: "Humanity's Last Exam — project site", trust: 0.9 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (HLE entry)", trust: 0.85 }
related: [benchmark/hle-diamond]
---

## What It Tests
Whether a model has **expert-level academic knowledge and reasoning** on closed questions at the edge of human knowledge.

- **Breadth** — dozens of subjects: mathematics, humanities, natural sciences.
- **Verifiable answers** — each question has a single known, unambiguous solution.
- **Not search-friendly** — the authors say answers cannot be found quickly online.

## How It Scores
An **LLM judge** checks each answer against the known solution.

- **Accuracy** — the share of correct answers.
- **Calibration** — the paper reports RMS calibration error from the stated confidence; it finds poor calibration across the models it tested.
- **Settings** — closed book or with tools; always cite which.

## Format
2,500 multi-modal questions answered zero-shot: 24% multiple-choice (five or more options) and the rest exact-match, with about 14% needing an image. The prompt asks for an answer and a confidence. Artificial Analysis keeps to the 2,158 text-only questions for its own HLE entry.

## Limitations
- **Narrow construct.** Closed-ended academic questions only, not open-ended research or creative work.
- **Leak risk.** Questions are public, so the authors hold a private test set.
- **Setup-dependent.** Tool use, subset and judge change the number.
- **Variant.** HLE-Diamond, a refined 1,000-question subset, has its own record.

## Example
Sample questions are shown on lastexam.ai. Each submitted question comes with its text, either an **exact-match answer** or **multiple-choice options** with the right one marked, a written rationale, an academic subject and the contributor's affiliation. A model returns its answer and a confidence, and the judge marks the answer right or wrong against the key.
