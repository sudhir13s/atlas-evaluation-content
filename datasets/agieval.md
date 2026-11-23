---
id: evaluation-dataset/agieval
type: evaluation-dataset
slug: agieval
schema_version: 4.0
title: AGIEval
name: AGIEval
category: Dataset Suite
size: 20 tasks from human standardized/admission exams (English + Chinese)
url: https://github.com/ruixiangcui/AGIEval
description: A benchmark built entirely from real human standardized and admission exams.
what_it_tests: Human-centric reasoning and knowledge as measured by real exams — SAT, LSAT, GRE-style math, US and Chinese college-entrance and law/civil-service exams, and math competitions (AMC/AIME).
why_it_exists: To evaluate models on the same high-stakes tests humans take, giving an intuitive, human-anchored yardstick and avoiding synthetic or crowdsourced task artifacts.
how_to_use: Run the released task sets (multiple-choice and fill-in) in few-shot or 0-shot, with or without chain-of-thought; scores can be read directly against known human percentiles on those exams.
known_issues: Public human exams are prime contamination targets — many appear in training corpora, inflating scores. Bilingual coverage means language ability confounds reasoning on the Chinese subset. Fixed historical exams cannot refresh, so the set ages.
tags:
- dataset-suite
- human-exams
- reasoning
- bilingual
updated: '2026-07-17'
sources:
- url: https://arxiv.org/abs/2304.06364
  title: 'AGIEval: A Human-Centric Benchmark for Evaluating Foundation Models (Zhong et al., 2023)'
  trust: 0.9
related:
- benchmark/mmlu
- benchmark/aime
---

## What It Is

A benchmark assembled from official human exams rather than crowdsourced questions, spanning English
and Chinese tests used for college admission, law, and civil service, plus math competitions. The
goal is a human-anchored measure of general ability.

## How It Evaluates

Tasks are mostly multiple-choice (some fill-in), run few-shot or 0-shot, optionally with chain-of-
thought. Because the source exams have known human score distributions, a model's accuracy can be
interpreted against real human percentiles.

## What It Looks For

Whether models can pass the standardized tests that gate human opportunity — an intuitive, externally
valid signal of reasoning and knowledge that does not depend on synthetic task design.

## Known Issues

Contamination is the central risk: these exams are widely published and likely sit in training data.
The bilingual design entangles language proficiency with reasoning on the Chinese tasks. As a fixed
set of historical exams, it cannot be refreshed and slowly saturates.
