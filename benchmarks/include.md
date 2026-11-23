---
id: benchmark/include
type: benchmark
slug: include
schema_version: 4.0
title: INCLUDE
name: INCLUDE
category: Multilingual
description: Four-option multiple-choice questions from local academic, professional and licence exams in 44 languages, keeping regional and cultural knowledge.
what_is_it: INCLUDE (Romanou et al., 2024; EPFL, Cohere For AI, Cohere For AI Community, ETH Zurich and the Swiss AI Initiative) collected 197,243 question-answer pairs from 1,926 local exams in 44 languages and 15 scripts, from sources in 52 countries, instead of translating English benchmarks. The released INCLUDE-base set has 22,637 four-option questions (dataset card; the paper gives 22,635), and INCLUDE-lite has 10,770.
why_people_use_it: The authors note that multilingual benchmarks are often translated from English and so ignore the regional and cultural knowledge of the places where models are used; INCLUDE evaluates models in those language environments (2024 paper).
examples:
- 'Albanian, Social Science / Sociology, region implicit (Hugging Face CohereLabs/include-base-44, Apache 2.0): Si është krijuar Unioni Europian? (How was the European Union created?) A: Me ndërmjetësim (Through mediation) B: Me zgjedhje demokratike (Through democratic elections) C: Me anë të evoluimit (Through evolution) D: Me anë të revolucionit (Through revolution). Answer index 2 (C).'
method: The paper reports 5-shot and zero-shot chain-of-thought accuracy, with prompts in the question's language or in English, with or without a regional prefix. Questions are labelled region-agnostic or region-specific; region-specific splits into region-explicit, cultural and region-implicit.
metric: Accuracy = correct answers / total questions, overall and by language, academic area and regional label; random guessing scores 25%.
format: Four-option multiple choice in each question's original language; INCLUDE-base caps each language at 550 questions (500 regional-knowledge, 50 STEM).
limitations: The authors note that regional labels were assigned per exam topic rather than per question, and that performance across regional labels is not separated from subject difficulty. Questions come from different exams in each language, so per-language scores are not a matched comparison. Multiple choice only.
citation: https://arxiv.org/abs/2411.19799
openness: open
openness_note: INCLUDE-base and INCLUDE-lite questions with answers are public on Hugging Face (Apache 2.0 on the base dataset card), and the paper describes sampling, prompts and scoring.
worked_examples:
- task_type: Driving law (Turkish)
  input: Türkiye'de yerleşim yeri içinde otomobiller için azami hız sınırı kaç km/s'dir? (In Turkey, what is the maximum speed limit for cars within a built-up area?)
  options:
  - '30'
  - '50'
  - '70'
  - '90'
  success:
    answer: B — 50 km/h
    why: Turkish Highway Traffic Law sets the in-town car limit at 50 km/h — a fact local to that country's rules.
  failure:
    answer: A — 30 km/h
    why: Picks a value shaped by Western residential/school-zone defaults instead of the Turkish statute — the regional-knowledge gap INCLUDE isolates.
  checks: Genuinely local, region-explicit knowledge versus Western-default recall in the target language.
  scoring: Accuracy (% correct).
- task_type: Driving law (Polish)
  input: W Polsce jaki jest dopuszczalny limit alkoholu we krwi dla kierowcy? (In Poland, what is the legal blood-alcohol limit for a driver?)
  options:
  - 0,2 promila (0.02%)
  - 0,5 promila (0.05%)
  - 0,8 promila (0.08%)
  - 0,0 promila (0.0%)
  success:
    answer: A — 0.2‰ (0.02%)
    why: Polish law sets a strict 0.2 g/L blood-alcohol limit for drivers — a country-specific statute.
  failure:
    answer: B — 0.5‰ (0.05%)
    why: Defaults to the common EU/Western 0.05% limit; Poland is stricter, so the Western prior gives the wrong local answer.
  checks: Country-specific legal thresholds versus a generic Western default.
  scoring: Accuracy (% correct).
- task_type: Civics (Indonesian)
  input: Berapa jumlah sila dalam Pancasila? (How many principles are in the Pancasila?)
  options:
  - '3'
  - '4'
  - '5'
  - '6'
  success:
    answer: C — 5
    why: The Pancasila, Indonesia's founding philosophy, has five principles — core national civic knowledge.
  failure:
    answer: B — 4
    why: A plausible near-count for a model lacking Indonesian civic grounding; the correct, culturally-specific figure is five.
  checks: Core civic knowledge local to a nation's own curriculum.
  scoring: Accuracy (% correct).
- task_type: Constitutional history (Hindi)
  input: भारत में मतदान की न्यूनतम आयु कितनी है? (What is the minimum voting age in India?)
  options:
  - '18'
  - '21'
  - '25'
  - '16'
  success:
    answer: A — 18
    why: The 61st Amendment (1989) lowered India's voting age from 21 to 18 — a regional constitutional fact.
  failure:
    answer: B — 21
    why: The pre-1989 age; a regional-history near-miss for a model without the local constitutional update.
  checks: Jurisdiction-specific legal history taught in a country's own exams.
  scoring: Accuracy (% correct).
- task_type: Driving law (German)
  input: Wie hoch ist die Richtgeschwindigkeit auf deutschen Autobahnen? (What is the advisory speed on German motorways?)
  options:
  - 100 km/h
  - 120 km/h
  - 130 km/h
  - keine (none)
  success:
    answer: C — 130 km/h
    why: Germany's Autobahn advisory speed (Richtgeschwindigkeit) is 130 km/h — a specific regulatory value.
  failure:
    answer: D — keine (none)
    why: Conflates the advisory speed with the famous unrestricted stretches; the advisory figure exists even where no hard limit is posted — a regional legal nuance.
  checks: Precise local regulatory knowledge versus a popular stereotype about German motorways.
  scoring: Accuracy (% correct).
tags:
- multilingual
- knowledge
- regional
- multiple-choice
- culture
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2411.19799
  title: 'INCLUDE: Evaluating Multilingual Language Understanding with Regional Knowledge (Romanou et al., 2024)'
  trust: 0.95
- url: https://arxiv.org/html/2411.19799
  title: INCLUDE, full text (arXiv HTML)
  trust: 0.95
- url: https://huggingface.co/datasets/CohereLabs/include-base-44
  title: CohereLabs/include-base-44 — Datasets at Hugging Face
  trust: 0.9
related:
- benchmark/nova-63
- benchmark/mmmlu
- benchmark/global-piqa
---

## What It Tests

**Knowledge and reasoning in 44 languages, as local exams test it**.

- **Sources** — academic exams, professional certifications and licences, and regional licence exams.
- **Regional labels** — region-agnostic (34.4% of collected questions, e.g. mathematics), region-explicit (18.8%), cultural (16.4%) and region-implicit (30.4%).
- **Makers** — Romanou and colleagues (EPFL, Cohere For AI, ETH Zurich, Swiss AI Initiative), 2024.
- **Scale** — 197,243 questions from 1,926 exams; 39.8% repurposed from published local benchmarks such as ArabicMMLU and TurkishMMLU.

## How It Scores

**Accuracy** = correct answers / total questions, reported overall and by language, academic area and regional label. Random guessing scores 25%.

- **Paper's settings** — 5-shot, and zero-shot chain-of-thought (a translated "let's think step by step" is appended).
- **Prompt language** — in the question's language or in English, with or without a regional prefix.

## Format

Four-option multiple choice in the original language. Questions with fewer options were dropped and those with more were pruned to four.

- **INCLUDE-base** — 22,637 questions (dataset card), at most 550 per language.
- **INCLUDE-lite** — 10,770 questions, at most 250 per language, region-specific domains only.
- **Data** — Hugging Face `CohereLabs/include-base-44` (Apache 2.0) and `include-lite-44`.

## Limitations

- **Confounded labels.** Regional labels were set per exam topic, not per question, and results are not separated from subject difficulty, as the authors note.
- **Not parallel.** Different exams per language.
- **Multiple choice.** No generation is tested.

## Example

From the Albanian subset on the Hugging Face dataset card for INCLUDE-base (Apache 2.0), Social Science / Sociology, labelled **region implicit**:

- **Question** — "Si është krijuar Unioni Europian?" (How was the European Union created?)
- **Options** — A: Me ndërmjetësim (through mediation), B: Me zgjedhje demokratike (through democratic elections), C: Me anë të evoluimit (through evolution), D: Me anë të revolucionit (through revolution).
- **Key** — answer index 2, i.e. **C**. The key comes from the source exam; "region implicit" means the expected answer may depend on how that region teaches the topic.
