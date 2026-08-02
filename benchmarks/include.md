---
id: benchmark/include
type: benchmark
slug: include
schema_version: 1.0
title: "INCLUDE"
name: INCLUDE
category: Multilingual
description: "Regional-knowledge multiple-choice from local exams in 44 languages."
what_is_it: "INCLUDE is a knowledge- and reasoning-centric benchmark of 22,637 four-option multiple-choice questions (from a larger 197,243-item collection) drawn from real academic and professional exams in 44 languages — deliberately keeping region-specific and cultural knowledge instead of translating English."
why_people_use_it: "It answers a question translated benchmarks cannot: how does a model do on knowledge that is actually local to a language's speakers — a Polish driving-law question, a Turkish civil-service exam — rather than US content rendered into their language?"
examples:
  - "A national driving-licence or civil-service exam question in Turkish, Tamil, or Basque, including items whose answer depends on that country's specific laws or customs."
method: "Multiple-choice evaluation over 22,637 four-option questions across 44 languages and 57 topics; items are labelled region-agnostic, region-implicit, or region-explicit to separate universal from culturally-specific knowledge."
metric: "Accuracy (% correct), reported overall and by language and region-sensitivity."
format: "4-way multiple-choice, questions in their original language; run 0-shot or few-shot."
saturation: medium
limitations: "Sourcing from real exams means uneven subject and difficulty distribution across the 44 languages, so per-language scores are not strictly comparable. It is knowledge-recall-centric (little generation or tool use), and because questions are native rather than parallel, it measures a different thing than a translated set like MMMLU — the two are complementary, not interchangeable."
citation: "https://arxiv.org/abs/2411.19799"
tags: [multilingual, knowledge, regional, multiple-choice, culture]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2411.19799", title: "INCLUDE: Evaluating Multilingual Language Understanding with Regional Knowledge (2024)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/CohereLabs/include-base-44", title: "CohereLabs/include-base-44 — Datasets at Hugging Face", trust: 0.9 }
related: [benchmark/nova-63, benchmark/mmmlu, benchmark/global-piqa]
---

## What It Tests
Regional, culturally-grounded knowledge — the kind that lives inside a language's own exams. The
core INCLUDE-base set is 22,637 four-option questions across 44 languages and 57 topics, spanning
STEM, humanities, social science, health, and professional certifications, collected from local
academic and professional examinations rather than translated from English.

## How It Scores
Accuracy overall and broken out by language and by how region-specific a question is. Items are
tagged **region-agnostic** (universally answerable), **region-implicit**, or **region-explicit**
(needs local legal, geographic, or cultural knowledge), which lets the benchmark separate general
competence from genuinely local knowledge.

## Format
Four-way multiple choice in each question's original language, run 0-shot or few-shot. The full
collection gathered 197,243 question-answer pairs from local exam sources; the released base
benchmark distils these to 22,637 clean multiple-choice items.

## Limitations
Because questions come from real, heterogeneous exams, subject mix and difficulty vary across the
44 languages, so raw per-language accuracy is not a clean apples-to-apples comparison. It is
recall-and-reasoning multiple-choice only — no generation, dialogue, or tool use — and its native,
non-parallel design measures local knowledge, a deliberately different axis from translated
parallel tests.

## Example
Where a translated benchmark would ask a US-centric question in Turkish, INCLUDE instead asks an
actual question from a Turkish national exam — for instance one whose correct answer hinges on
Turkish traffic law. A model can only get such an item right if it holds knowledge local to that
region, which is exactly the capability INCLUDE isolates.
