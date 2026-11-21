---
id: benchmark/chartography
type: benchmark
slug: chartography
schema_version: 1.0
title: "Chartography"
name: Chartography (v1.1)
category: Multimodal
description: "A Surge AI benchmark of 100 expert-written tasks on professional charts such as Kaplan-Meier curves, Sankey diagrams and Bode plots."
what_is_it: "Chartography tests whether models can read the specialised charts professionals use, including Kaplan-Meier curves, candlestick charts, contour maps, Sankey diagrams and Bode plots. It has 100 tasks, each pairing one chart with a question written by a professional who reads such charts for a living and verified by three more experts. The tasks cover 12 professional domains. Version 1.1 updated the setup to an agentic harness."
why_people_use_it: "Common chart benchmarks lean on bar, line and pie charts, use short reasoning chains and were nearing saturation. Chartography targets domain formats and multi-step graphical reasoning. Google DeepMind and other vendors cite it in multimodal tables."
method: "Each task has a chart and an expert-written question with a calculation walkthrough. Numerical answers carry expert-set acceptable ranges. The paper evaluates 30 frontier-model configurations with 20 scored trials per task. Version 1.1 uses an agentic harness."
metric: "Mean pass@1 over 100 tasks and 20 trials per model, in percent."
format: "Chart image plus question; free numeric or short answers graded against expert ranges; v1.1 runs in an agentic harness, and vendors may report runs with or without tools."
limitations: "Version matters a great deal: the paper's abstract reports a best score of 45.0% on the first release, while the v1.1 leaderboard, which uses the agentic harness, shows far higher numbers, so never mix them. Only 2 of the 100 tasks are public, and the full set is available on request, so reproduction is limited. Public-source charts are included for non-commercial research use and are not under the dataset's CC BY 4.0. Vendor tables may run it with or without tools, so check the setting."
citation: "https://arxiv.org/abs/2608.10677"
tags: [multimodal, charts, professional, expert-written, surge-ai]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2608.10677", title: "Chartography: A Benchmark for Professional Chart Understanding (arXiv:2608.10677)", trust: 0.95 }
  - { url: "https://www.surgehq.ai/benchmarks/chartography", title: "Chartography v1.1 — Surge AI benchmark page and leaderboard", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/surgeai/Chartography-harbor", title: "surgeai/Chartography-harbor — dataset card (2 public tasks)", trust: 0.9 }
  - { url: "https://github.com/surge-ai/chartography", title: "surge-ai/chartography — evaluation harness repository", trust: 0.9 }
---

## What It Tests
Whether a model can **read professional charts** and reason about them.

- **Perception** — reading values from sparse axes and nuanced features.
- **Domain interpretation** — knowing the conventions of each chart type.
- **Multi-step reasoning** — calculating an answer from what the chart shows.

The paper says failures concentrate in visual perception, including 3D geometry projected onto a plane.

## How It Scores
**Pass@1** averaged over tasks and trials.

- **Tasks** — 100 charts across 12 professional domains, spanning STEM (including five engineering subfields), finance and investing, manufacturing and supply chain, and healthcare.
- **Trials** — 20 scored trials per task and model.
- **Answers** — numbers get expert-set acceptable ranges, so exact pixels are not required.

## Format
A chart plus a question. Version 1.1 changed the evaluation to an agentic harness; vendor tables state whether tools were used.

## Limitations
- **Version split.** The paper's results are for the first release; v1.1 numbers are not comparable.
- **Mostly closed.** Two public tasks; the rest is on request (benchmarks@surgehq.ai).
- **Licence mix.** Prompts, answers and expert charts are CC BY 4.0; charts from public sources are not, and are for non-commercial research.
- **Held-out only.** The maker asks users to keep tasks out of training data.

## Example
The dataset card illustrates grading with an **acceptable range**: an answer of 150 mph is accepted anywhere between 140 and 150 mph. So an answer that is close enough to the chart's true value can pass, while a misread axis fails. The dataset card includes two public sample tasks that show the format, and the project repository holds the evaluation harness, built on the Inspect AI framework.
