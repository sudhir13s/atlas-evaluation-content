---
id: evaluation-dataset/big-bench
type: evaluation-dataset
slug: big-bench
schema_version: 1.0
title: "BIG-bench"
name: "BIG-bench (Beyond the Imitation Game)"
category: Dataset Suite
size: "204 tasks contributed by 450+ authors from 130+ institutions"
url: "https://github.com/google/BIG-bench"
description: "A massive, collaboratively built suite of 200+ diverse tasks probing capabilities beyond standard benchmarks."
what_it_tests: "An extremely broad spread — logic, math, common sense, linguistics, bias, social reasoning, coding, and many novel or whimsical tasks — deliberately including problems believed to be beyond current models."
why_it_exists: "To move past narrow, saturating benchmarks by crowdsourcing a huge, heterogeneous task collection that stresses many capabilities at once and probes where models qualitatively fail."
how_to_use: "Run tasks (JSON or programmatic) via the repo; most consumers now run the BIG-Bench Hard (BBH) subset of 23 discriminating tasks. Tasks report task-specific metrics, often with a human baseline for context."
known_issues: "Enormous and heterogeneous, so a single aggregate is nearly meaningless and full runs are costly — most people use BBH instead. Task quality varies. Many original tasks have since saturated, which is why the Hard subset was carved out."
tags: [dataset-suite, diverse, collaborative]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2206.04615", title: "Beyond the Imitation Game: Quantifying and Extrapolating the Capabilities of Language Models (Srivastava et al., 2022)", trust: 0.95 }
  - { url: "https://github.com/google/BIG-bench", title: "google/BIG-bench (GitHub)", trust: 0.9 }
related: [benchmark/bbh]
---

## What It Is
A community-built benchmark of 204 tasks assembled by hundreds of researchers to test language models
across an unusually wide range of skills, including tasks intended to be hard or currently
unsolvable. It is the parent suite from which BIG-Bench Hard (BBH) is drawn.

## How It Evaluates
Each task defines its own inputs and scoring (multiple-choice, exact match, or a custom metric), often
with a measured human baseline for comparison. Consumers select tasks or task groups; in practice most
run the 23-task BBH subset that still discriminates between strong models.

## What It Looks For
Breadth and blind spots — capabilities that focused benchmarks never probe, and qualitative failure
modes. It was designed to reveal where models break, not just to produce a ranking.

## Known Issues
The scale and heterogeneity mean the aggregate is hard to interpret and full evaluation is expensive.
Contributed task quality is uneven, and many tasks have saturated over time — the reason BBH exists.
For routine model comparison, BBH is the practical entry point.
