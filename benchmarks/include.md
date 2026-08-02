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
updated: 2026-08-02
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
