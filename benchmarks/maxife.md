---
id: benchmark/maxife
type: benchmark
slug: maxife
schema_version: 1.0
title: "MaXIFE"
name: MaXIFE
category: Multilingual
description: "Verifiable instruction-following, multilingual and cross-lingual, in 23 languages."
what_is_it: "MaXIFE (Multilingual and Cross-lingual Instruction Following Evaluation) measures whether models obey verifiable instructions — like 'answer in exactly three bullet points' or 'do not use the word X' — across 23 languages, in both same-language and cross-lingual settings."
why_people_use_it: "Instruction-following benchmarks were mostly English-only; MaXIFE checks whether that discipline holds up in other languages and when the instruction and the content are in different languages — a common real-world failure mode for multilingual assistants."
examples:
  - "Give a base question plus verifiable constraints such as 'respond in Japanese', 'use at least 200 words', or 'wrap the answer in JSON', then programmatically check whether the output satisfies each constraint."
method: "795 base questions combined with 1,667 verifiable instructions across 23 languages; each output is checked by rule-based verifiers where mechanically checkable and by model-based judging otherwise."
metric: "Instruction-following accuracy — the fraction of instructions (and prompts) whose constraints the response satisfies."
format: "Open-ended generation under one to three verifiable constraints per prompt; run 0-shot."
saturation: medium
limitations: "Rule-based checks only cover mechanically verifiable constraints; the rest fall back on model-based judging, which inherits judge bias and language-dependent reliability. With 23 languages and a fixed instruction pool, coverage of any single language is shallow, and cross-lingual results depend on how the instruction and content languages are paired."
citation: "https://arxiv.org/abs/2506.01776"
tags: [multilingual, instruction-following, cross-lingual, generation]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2506.01776", title: "MaXIFE: Multilingual and Cross-lingual Instruction Following Evaluation (ACL 2025)", trust: 0.95 }
  - { url: "https://aclanthology.org/2025.acl-long.698/", title: "MaXIFE — ACL Anthology (2025.acl-long.698)", trust: 0.95 }
related: [benchmark/ifeval, benchmark/mmmlu, benchmark/polymath]
---

## What It Tests
Whether a model actually follows explicit, verifiable instructions — formatting, length, language,
inclusion/exclusion constraints — in 23 languages. It extends the "verifiable instruction"
paradigm of English IFEval to a multilingual setting and adds a cross-lingual setting where the
instruction and the content it acts on are in different languages.

## How It Scores
Instruction-following accuracy: the share of constraints a response satisfies. MaXIFE pairs 795
base questions with 1,667 verifiable instructions (each base question carries one to three
instructions), and scores each output with **rule-based verifiers** where a constraint is
mechanically checkable (word count, presence of a keyword, output language) and **model-based
judging** where it is not.

## Format
Open-ended generation under one to three verifiable constraints per prompt, run 0-shot. The dual
rule-based-plus-model-based design balances the precision of programmatic checks against the
coverage of an LLM judge for constraints that cannot be verified by a simple rule.

## Limitations
Only mechanically checkable constraints get exact rule-based scoring; the remainder rely on
model-based judgement, which carries judge bias and varies in reliability across languages. Spread
over 23 languages with a fixed instruction pool, per-language depth is limited, and cross-lingual
scores depend on which instruction-language / content-language pairs are chosen.

## Example
A prompt might ask, in Korean, for a summary that "uses exactly five sentences and contains no
numerals." A rule-based verifier counts the sentences and scans for digits to score compliance
exactly. In the cross-lingual variant, the instruction may be in one language while the material
to summarise is in another — testing whether the model honours constraints across a language
boundary, a frequent breakdown point for multilingual assistants.
