---
id: benchmark/mmmlu
type: benchmark
slug: mmmlu
schema_version: 1.0
title: "MMMLU"
name: MMMLU
category: Multilingual
description: "OpenAI's professionally human-translated MMLU test set in 14 languages."
what_is_it: "The Multilingual Massive Multitask Language Understanding (MMMLU) set is the full MMLU test split translated by professional human translators into 14 languages, so the same 14,042 questions can be scored in each language."
why_people_use_it: "It is the most-cited apples-to-apples multilingual knowledge test: because every language is a human translation of the identical questions, a model's score gap between English and, say, Yoruba isolates language ability from question difficulty."
examples:
  - "A US-foreign-policy MMLU item such as 'The Monroe Doctrine was primarily concerned with...' presented in Arabic, Swahili, or Japanese, with the same four answer options translated."
method: "0-shot or few-shot multiple-choice over 14,042 parallel questions per language across 57 subjects; the predicted answer letter is parsed and compared to the gold key."
metric: "Accuracy (% correct), reported per language and averaged."
format: "4-way multiple-choice, one correct option; typically run 0-shot or 5-shot."
saturation: high
limitations: "It is a translation of English-origin MMLU, so it carries MMLU's US/Western cultural framing and any of MMLU's original label errors into every language — it measures multilingual knowledge recall, not culturally-native knowledge. High-resource languages are near-saturated; the signal now lives in the low-resource tail (e.g. Yoruba, Swahili)."
citation: "https://huggingface.co/datasets/openai/MMMLU"
tags: [multilingual, knowledge, multiple-choice, translation]
updated: 2026-07-20
sources:
  - { url: "https://huggingface.co/datasets/openai/MMMLU", title: "openai/MMMLU — Datasets at Hugging Face", trust: 0.95 }
  - { url: "https://www.marktechpost.com/2024/09/23/openai-releases-multilingual-massive-multitask-language-understanding-mmmlu-dataset-on-hugging-face-to-easily-evaluate-multilingual-llms/", title: "OpenAI Releases MMMLU Dataset on Hugging Face (2024)", trust: 0.7 }
related: [benchmark/mmlu, benchmark/mmlu-pro, benchmark/mmlu-redux]
---

## What It Tests
The same broad academic knowledge as MMLU (57 subjects, from elementary math to professional
law and medicine) but read and answered in a non-English language. Because the questions are
identical across languages, it isolates *language competence* from *subject difficulty*.

## How It Scores
Accuracy over 14,042 questions per language, reported for each of the 14 languages and averaged.
The 14 languages span Europe (German, Spanish, French, Italian, Portuguese), Asia (Chinese,
Japanese, Korean, Hindi, Bengali, Indonesian), the Middle East (Arabic), and Africa (Swahili,
Yoruba). The per-language spread — not the average — is the headline number.

## Format
Four-way multiple choice, one correct option, usually run 0-shot or 5-shot. Crucially, every
item is a *human* translation: OpenAI paid professional translators for one translation per
question per language rather than machine-translating, which removes translationese as a
confound — a first for a benchmark of this size.

## Limitations
The questions originate from English MMLU, so their cultural and topical framing stays Western
regardless of the target language — MMMLU asks "can you answer US-centric questions in Swahili?",
not "what does a Swahili speaker's world contain?". It also inherits MMLU's known label noise,
and high-resource languages are already near the ceiling, leaving useful signal mainly in
low-resource languages.

## Example
An MMLU item like "In the context of US foreign policy, the Monroe Doctrine primarily aimed to..."
appears verbatim-translated into each of the 14 languages, with the same four options. A model
that scores 88% in English but 61% in Yoruba reveals a language gap, not a knowledge gap — the
question set is by construction the same.
