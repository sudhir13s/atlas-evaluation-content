---
id: benchmark/include
type: benchmark
slug: include
schema_version: 1.0
title: "INCLUDE"
name: INCLUDE
category: Multilingual
description: "Four-option multiple-choice questions from local academic, professional and licence exams in 44 languages, keeping regional and cultural knowledge."
what_is_it: "INCLUDE (Romanou et al., 2024; EPFL, Cohere For AI, ETH Zurich, Swiss AI Initiative and others) collected 197,243 question-answer pairs from local exam sources in 44 written languages, instead of translating English benchmarks. The released INCLUDE-base set has 22,637 four-option questions covering 57 topics (dataset card; the paper gives 22,635), and INCLUDE-lite has 10,770."
why_people_use_it: "The authors note that multilingual benchmarks are often translated from English and so ignore the regional and cultural knowledge of the places where models are used; INCLUDE evaluates models in those language environments (2024 paper)."
examples:
  - "A national driving-licence or civil-service exam question in Turkish, Tamil, or Basque, including items whose answer depends on that country's specific laws or customs."
method: "The paper reports 5-shot and zero-shot chain-of-thought accuracy, with prompts in the question's language or in English. Questions are labelled region-agnostic or region-specific; region-specific splits into region-explicit (laws, certifications, clinical guidelines, licensing), cultural and region-implicit."
metric: "Accuracy (% correct), overall and by language, academic area and regional label."
format: "Four-option multiple choice in each question's original language; INCLUDE-base caps each language at 550 questions (500 regional-knowledge, 50 STEM)."
limitations: "The authors note that performance across regional labels is not separated from topic difficulty and academic level. Questions come from different exams in each language, so per-language scores are not a matched comparison. Multiple choice only."
citation: "https://arxiv.org/abs/2411.19799"
worked_examples:
  - task_type: "Driving law (Turkish)"
    input: "Türkiye'de yerleşim yeri içinde otomobiller için azami hız sınırı kaç km/s'dir? (In Turkey, what is the maximum speed limit for cars within a built-up area?)"
    options:
      - "30"
      - "50"
      - "70"
      - "90"
    success:
      answer: "B — 50 km/h"
      why: "Turkish Highway Traffic Law sets the in-town car limit at 50 km/h — a fact local to that country's rules."
    failure:
      answer: "A — 30 km/h"
      why: "Picks a value shaped by Western residential/school-zone defaults instead of the Turkish statute — the regional-knowledge gap INCLUDE isolates."
    checks: "Genuinely local, region-explicit knowledge versus Western-default recall in the target language."
    scoring: "Accuracy (% correct)."
  - task_type: "Driving law (Polish)"
    input: "W Polsce jaki jest dopuszczalny limit alkoholu we krwi dla kierowcy? (In Poland, what is the legal blood-alcohol limit for a driver?)"
    options:
      - "0,2 promila (0.02%)"
      - "0,5 promila (0.05%)"
      - "0,8 promila (0.08%)"
      - "0,0 promila (0.0%)"
    success:
      answer: "A — 0.2‰ (0.02%)"
      why: "Polish law sets a strict 0.2 g/L blood-alcohol limit for drivers — a country-specific statute."
    failure:
      answer: "B — 0.5‰ (0.05%)"
      why: "Defaults to the common EU/Western 0.05% limit; Poland is stricter, so the Western prior gives the wrong local answer."
    checks: "Country-specific legal thresholds versus a generic Western default."
    scoring: "Accuracy (% correct)."
  - task_type: "Civics (Indonesian)"
    input: "Berapa jumlah sila dalam Pancasila? (How many principles are in the Pancasila?)"
    options:
      - "3"
      - "4"
      - "5"
      - "6"
    success:
      answer: "C — 5"
      why: "The Pancasila, Indonesia's founding philosophy, has five principles — core national civic knowledge."
    failure:
      answer: "B — 4"
      why: "A plausible near-count for a model lacking Indonesian civic grounding; the correct, culturally-specific figure is five."
    checks: "Core civic knowledge local to a nation's own curriculum."
    scoring: "Accuracy (% correct)."
  - task_type: "Constitutional history (Hindi)"
    input: "भारत में मतदान की न्यूनतम आयु कितनी है? (What is the minimum voting age in India?)"
    options:
      - "18"
      - "21"
      - "25"
      - "16"
    success:
      answer: "A — 18"
      why: "The 61st Amendment (1989) lowered India's voting age from 21 to 18 — a regional constitutional fact."
    failure:
      answer: "B — 21"
      why: "The pre-1989 age; a regional-history near-miss for a model without the local constitutional update."
    checks: "Jurisdiction-specific legal history taught in a country's own exams."
    scoring: "Accuracy (% correct)."
  - task_type: "Driving law (German)"
    input: "Wie hoch ist die Richtgeschwindigkeit auf deutschen Autobahnen? (What is the advisory speed on German motorways?)"
    options:
      - "100 km/h"
      - "120 km/h"
      - "130 km/h"
      - "keine (none)"
    success:
      answer: "C — 130 km/h"
      why: "Germany's Autobahn advisory speed (Richtgeschwindigkeit) is 130 km/h — a specific regulatory value."
    failure:
      answer: "D — keine (none)"
      why: "Conflates the advisory speed with the famous unrestricted stretches; the advisory figure exists even where no hard limit is posted — a regional legal nuance."
    checks: "Precise local regulatory knowledge versus a popular stereotype about German motorways."
    scoring: "Accuracy (% correct)."
tags: [multilingual, knowledge, regional, multiple-choice, culture]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2411.19799", title: "INCLUDE: Evaluating Multilingual Language Understanding with Regional Knowledge (Romanou et al., 2024)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2411.19799", title: "INCLUDE, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/CohereLabs/include-base-44", title: "CohereLabs/include-base-44 — Datasets at Hugging Face", trust: 0.9 }
related: [benchmark/nova-63, benchmark/mmmlu, benchmark/global-piqa]
---

## What It Tests
**Knowledge and reasoning in 44 languages, as local exams test it**.

- **Sources** — academic exams, professional certifications and licences, and regional licence exams.
- **Regional labels** — region-agnostic (34.4% of collected questions, e.g. mathematics), region-explicit (18.8%), cultural (16.4%) and region-implicit.

## How It Scores
**Accuracy**, reported overall and by language, academic area and regional label. The paper runs 5-shot and zero-shot chain-of-thought prompts.

## Format
Four-option multiple choice in the original language. Questions with fewer options were dropped and those with more were pruned to four.

- **INCLUDE-base** — 22,637 questions (dataset card), at most 550 per language.
- **INCLUDE-lite** — 10,770 questions, at most 250 per language, region-specific domains only.

## Limitations
- **Confounded labels.** Regional-label results are not separated from topic difficulty, as the authors note.
- **Not parallel.** Different exams per language.
- **Multiple choice.** No generation is tested.

## Example
The paper's Figure 1 describes a region-explicit question about local laws or licensing, and a history question in a Greek exam whose answer may reflect a regional perspective. The model reads the question and four options in the original language and answers with one letter.
