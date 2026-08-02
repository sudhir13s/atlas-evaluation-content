---
id: benchmark/nova-63
type: benchmark
slug: nova-63
schema_version: 1.0
title: "NOVA-63"
name: NOVA-63
category: Multilingual
description: "A native-authored, difficulty-controlled multilingual exam over 63 disciplines."
what_is_it: "NOVA-63 (Native Omni-lingual Versatile Assessments of 63 Disciplines) is a 93,536-question benchmark sourced from native-language exam material across 14 languages and 63 academic disciplines — built to avoid translationese by not translating."
why_people_use_it: "It fixes the two big flaws of translated multilingual tests: content is natively authored (no translationese) and every item passes multi-level difficulty screening, so scores stay low and discriminative even for the strongest models."
examples:
  - "A native-language medical-licensing or law question in Chinese, Arabic, or Russian, drawn from that country's own exams rather than translated from English."
method: "Multiple-choice evaluation over 93,536 native-sourced questions spanning 63 disciplines in 14 languages; a pipeline of LLM-assisted formatting, expert quality verification, and multi-level difficulty screening curates the set."
metric: "Accuracy (% correct), analyzed per language, per language-family, and per discipline."
format: "Multiple-choice; native-language questions, no cross-language translation of items."
saturation: low
limitations: "Coverage is deep (63 disciplines) but breadth is 14 languages, so it is not a long-tail low-resource test like INCLUDE or Global PIQA. Native sourcing means questions are not parallel across languages, so a per-language score reflects both that language's difficulty mix and the model's ability — cross-language gaps must be read with that caveat."
citation: "https://aclanthology.org/2025.emnlp-main.364/"
tags: [multilingual, knowledge, reasoning, multiple-choice, native]
updated: 2026-07-20
sources:
  - { url: "https://aclanthology.org/2025.emnlp-main.364/", title: "NOVA-63: Native Omni-lingual Versatile Assessments of 63 Disciplines (EMNLP 2025 Main)", trust: 0.95 }
  - { url: "https://aclanthology.org/2025.emnlp-main.364.pdf", title: "NOVA-63 paper PDF (ACL Anthology)", trust: 0.95 }
related: [benchmark/include, benchmark/mmlu-prox, benchmark/mmmlu]
---

## What It Tests
Expert-level knowledge across 63 academic disciplines — the "63" in the name is disciplines, not
languages. It spans STEM, medicine, law, humanities and professional fields, in 14 languages,
using questions written natively in each language rather than translated from English.

## How It Scores
Accuracy over 93,536 questions, broken down by language, by language family, and by discipline.
Because every item survives multi-level difficulty screening, top models sit far below the
ceiling — the strongest public results reported are in the high-50s percent — which keeps the
benchmark discriminative where translated tests have saturated.

## Format
Multiple-choice questions sourced from native exam material in each of the 14 languages. A
curation pipeline combines large-language-model-assisted formatting, human expert quality
verification, and difficulty screening; items are not parallel translations, so each language's
set is its own native content.

## Limitations
It trades breadth for depth: 14 languages (not the 40-plus of INCLUDE or the 100-plus of Global
PIQA) but 63 disciplines with hard, screened questions. Non-parallel native content means a
per-language accuracy blends the local question mix with model ability, so raw cross-language
gaps are less clean than on a parallel set like MMMLU.

## Example
Rather than translating a US bar-exam question into Chinese, NOVA-63 draws an actual
Chinese-law question authored in Chinese, and an actual Russian-medicine question authored in
Russian. This native sourcing is the point: it measures knowledge as it exists in each language's
own academic tradition, avoiding the translationese and Western framing of translated benchmarks.
