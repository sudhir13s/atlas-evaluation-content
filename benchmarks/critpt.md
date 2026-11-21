---
id: benchmark/critpt
type: benchmark
slug: critpt
schema_version: 1.0
title: "CritPt"
name: CritPt (Complex Research using Integrated Thinking - Physics Test)
category: Science & Research
description: "A physics-research benchmark of 71 unpublished composite challenges, written by over 50 active researchers, with machine-verifiable answers."
what_is_it: "CritPt, pronounced 'critical point', tests language models on unpublished, research-level physics reasoning. It has 71 composite research challenges designed to simulate entry-level research projects, plus 190 simpler checkpoint tasks derived from them for finer-grained analysis. All problems were newly written by more than 50 active physics researchers from their own work. Areas include condensed matter, quantum physics, astrophysics, high-energy physics, fluid dynamics and biophysics."
why_people_use_it: "It asks whether models can reason through open-ended challenges of the kind found in frontier physics research, not high-school competitions. Each problem is hand-curated to admit a guess-resistant, machine-verifiable answer. Artificial Analysis includes a hard subset of the second version in its Intelligence Index."
method: "An automated grading pipeline customised for advanced physics output formats checks the answers, which can be numbers, symbolic expressions or code. Artificial Analysis runs models with a code-execution tool in an offline Linux sandbox and uses the official grading server."
metric: "Accuracy (pass@1) on the composite challenges; checkpoint tasks give a finer view. Artificial Analysis reports mean accuracy across repeats."
format: "Physics research problems answered as Python functions, symbolic expressions or numerical answers. Version 2 has 70 challenges, and Artificial Analysis' index uses a fixed private hard subset of 40 of them."
limitations: "The authors describe the benchmark as a diagnostic: current models do well only on isolated checkpoints. Version 2 differs from version 1 (71 versus 70 challenges per Artificial Analysis), so cite the version. The hard subset used in the Intelligence Index removed 30 problems that seven frontier models solved in all five attempts, so it is tied to the models of that moment. Physics research areas are broad but a set of this size is small."
citation: "https://arxiv.org/abs/2509.26574"
tags: [science, physics, research-level, machine-verifiable]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2509.26574", title: "Probing the Critical Point (CritPt) of AI Reasoning: a Frontier Physics Research Benchmark (arXiv:2509.26574)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (CritPt v2)", trust: 0.85 }
related: [leaderboard/artificial-analysis]
---

## What It Tests
Whether a model can carry out **entry-level physics research** problems that nobody has published.

- **Composite challenges** — 71 projects, each decomposed into checkpoints.
- **Checkpoint tasks** — 190 simpler pieces for finer diagnosis.
- **Breadth** — condensed matter, quantum, astrophysics, high-energy, fluids and more.

## How It Scores
An **automated, physics-aware grader** checks each answer.

- **Answer types** — numerical values, symbolic expressions, Python functions.
- **Guess-resistant** — each problem was curated so lucky guesses fail.
- **Artificial Analysis** — official grading server, pass@1.

## Format
Open-ended problems from 50-plus researchers' own work. In Artificial Analysis' run, models use a code tool in an offline Linux sandbox.

## Limitations
- **Versions.** CritPt v2 changed the set; cite which one.
- **Moment-bound subset.** The hard subset depends on which models solved what at launch.
- **Small.** Dozens of problems, however deep.

## Example
The paper describes the structure rather than a single problem here: a **composite challenge** is a full small research project, and its checkpoints break it into steps that can be graded separately. A model may return a symbolic expression or a numerical value, and the pipeline checks it against the researcher's verified answer.
