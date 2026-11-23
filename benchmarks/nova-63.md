---
id: benchmark/nova-63
type: benchmark
slug: nova-63
schema_version: 4.0
title: NOVA-63
name: NOVA-63
category: Multilingual
description: A multiple-choice multilingual benchmark of questions sourced from native speakers in 14 languages, divided into 63 academic disciplines.
what_is_it: NOVA-63 (Native Omni-lingual Versatile Assessments of 63 Disciplines; Zhang, Yang, Wan et al., EMNLP 2025, work done at Alibaba Group) is a multiple-choice benchmark of 89,107 native questions across 14 languages, covering 8 language families and, by the paper's estimate, about 69% of the global population. Questions in each language are divided into 13 primary, 63 secondary and 262 tertiary academic disciplines, following SuperGPQA's discipline scheme.
why_people_use_it: Its questions are native content rather than translations of English material, and a pipeline of LLM-assisted formatting, expert quality verification and multi-level difficulty screening balances them across disciplines with consistent difficulty standards.
examples:
- 'As the paper describes it: a question collected from native-language source material in one of the 14 languages, with a stem, answer options and one correct answer, filed under one of 63 secondary disciplines.'
method: Multiple choice. LLMs extract the question, options and answer from native source material, and hired reviewers check the extraction. To keep the set difficult, the authors selected questions on which large LLMs had accuracy below 50% and used statistical checks to find suspect questions for review.
metric: Accuracy = questions answered correctly / questions, analysed by language, language family and discipline.
format: Multiple-choice questions written natively in each language; items are not parallel across languages.
limitations: 'The question set is not downloadable today: the paper says it is open-sourced on Hugging Face, but the dataset page is empty and marked under review. Questions differ from language to language, so a per-language score mixes that language''s question set with the model''s ability. The paper gives 89,107 questions, while the ACL Anthology abstract page gives 93,536.'
citation: https://aclanthology.org/2025.emnlp-main.364/
openness: closed
openness_note: The paper describes the method and results and names a Hugging Face dataset under the MIT licence, but on 2026-11-23 that dataset page was empty and marked under review, so no task items are public.
tags:
- multilingual
- knowledge
- reasoning
- multiple-choice
- native
updated: '2026-11-23'
sources:
- url: https://aclanthology.org/2025.emnlp-main.364/
  title: 'NOVA-63: Native Omni-lingual Versatile Assessments of 63 Disciplines (EMNLP 2025 Main)'
  trust: 0.95
- url: https://aclanthology.org/2025.emnlp-main.364.pdf
  title: NOVA-63 paper PDF (ACL Anthology)
  trust: 0.95
- url: https://huggingface.co/datasets/zjy1298/NOVA-63
  title: zjy1298/NOVA-63 dataset card (MIT, currently empty and under review)
  trust: 0.9
related:
- benchmark/include
- benchmark/mmlu-prox
- benchmark/mmmlu
---

## What It Tests

NOVA-63's questions are **not currently published**: the paper says the data is open-sourced, but the Hugging Face page is empty and marked under review. It tests academic knowledge in **14 languages**, on questions sourced from native speakers rather than translated.

- **Size** — 89,107 questions in the paper (93,536 on the Anthology abstract page).
- **Disciplines** — 13 primary, 63 secondary and 262 tertiary; the "63" counts secondary disciplines.
- **8 language families** — the paper says they cover about 69% of the global population.
- **Makers** — Jinyang Zhang, Kexin Yang, Yu Wan and co-authors; the work was done at Alibaba Group.

## How It Scores

**Accuracy** = questions answered correctly / questions, broken down by language, language family and discipline.

## Format

Multiple-choice questions in each language's own content, built in four stages:

- **Collection** — material from native speakers, to avoid translationese.
- **Extraction** — LLMs pull out question, options and answer; hired reviewers check the result.
- **Difficulty screening** — keep questions large LLMs answered with **under 50% accuracy**.
- **Validation** — statistical checks flag suspect questions, for example when wrong answers cluster on one option.

## Limitations

- **Not downloadable yet.** The dataset page is empty and under review, so results cannot be checked.
- **Not parallel.** Each language has its own questions, so cross-language gaps mix content and ability.
- **Two counts.** 89,107 questions in the paper; 93,536 on the Anthology abstract page.

## Example

No item is shown because none is currently public. As the paper describes it, a task is a **native-language multiple-choice question** with its options and one correct answer, extracted from source material by an LLM and checked by reviewers. The model picks an option, and accuracy is reported per language, language family and discipline.
