---
id: benchmark/chartography
type: benchmark
slug: chartography
schema_version: 3.1
title: "Chartography"
name: Chartography
category: Multimodal
description: "A Surge AI benchmark of 100 expert-written tasks on professional charts such as Kaplan-Meier curves, Sankey diagrams and Bode plots."
what_is_it: "Chartography, from Surge AI (paper by Suhaas Garre, Chris Mutty, Sushant Mehta and Edwin Chen, August 2026), tests whether models can read the specialised charts professionals use, including Kaplan-Meier curves, candlestick charts, contour maps, Sankey diagrams and Bode plots. It has 100 tasks with 100 unique charts, each paired with a question written by a professional who reads such charts for a living and independently verified by three more experts. The tasks cover 12 professional domains."
why_people_use_it: "Surge AI built it to evaluate visual perception, domain-aware interpretation and multi-step graphical reasoning on chart formats that standard chart benchmarks rarely cover."
method: "Each task gives a model one chart image and an expert-written question. A judge model compares the model's final answer with the expert golden answer; it sees the question, the golden answer and the response, but not the chart. Numerical golden answers carry expert-set acceptable ranges. The paper evaluates 30 frontier-model configurations with 20 scored trials per task."
metric: "Mean pass@1 in percent: for each task, the share of trials whose answer the judge accepts, averaged over the 100 tasks. The repository reports it as accuracy/mean and can also report Pass@K and Pass^K over several runs."
format: "Chart image plus question; free numeric or short answers graded against expert ranges. The public tasks ship in Harbor task format (instruction, containerised environment with the chart, and tests), and the repository holds an Inspect AI evaluation harness."
limitations: "Only 2 of the 100 tasks are public, and the dataset card says the full set is available on request, so outside reproduction is limited. Public-source charts are included only for non-commercial research and evaluation and are not under the dataset's CC BY 4.0. Grading relies on a judge model, and the maker asks that tasks be kept out of all training data. Vendor tables may run it with or without tools, so check the setting."
citation: "https://arxiv.org/abs/2608.10677"
openness: partly-open
openness_note: "The paper and evaluation harness are public and 2 of the 100 tasks are on Hugging Face, while the full task set is held out and available from Surge AI on request."
tags: [multimodal, charts, professional, expert-written, surge-ai]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2608.10677", title: "Chartography: A Benchmark for Professional Chart Understanding (arXiv:2608.10677)", trust: 0.95 }
  - { url: "https://www.surgehq.ai/benchmarks/chartography", title: "Chartography — Surge AI benchmark page and leaderboard", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/surgeai/Chartography-harbor", title: "surgeai/Chartography-harbor — dataset card (2 public tasks)", trust: 0.9 }
  - { url: "https://github.com/surge-ai/chartography", title: "surge-ai/chartography — evaluation harness repository", trust: 0.9 }
---

## What It Tests
Whether a model can **read professional charts** and reason about them.

- **Perception** — reading values from sparse axes and nuanced features.
- **Domain interpretation** — knowing the conventions of each chart type.
- **Multi-step reasoning** — calculating an answer from what the chart shows.

**Who built it:** Surge AI; the paper's authors are Suhaas Garre, Chris Mutty, Sushant Mehta and Edwin Chen (arXiv, 11 August 2026).

## How It Scores
**Mean pass@1** averaged over tasks and trials. **Pass@1** is the chance that a single attempt is correct.

- **Tasks** — 100 charts across 12 professional domains, spanning STEM (including five engineering subfields), finance and investing, manufacturing and supply chain, and healthcare.
- **Trials** — 20 scored trials per task and model in the paper.
- **Judge** — one model-graded call per answer decides whether it matches the golden answer; the judge never sees the chart.
- **Ranges** — numbers get expert-set acceptable ranges matched to how precisely the chart can be read.

## Format
A chart plus a question; the model gives a final answer.

- **Public tasks** — Harbor format: `instruction.md`, `task.toml`, an `environment/` folder holding the chart and `tests/`.
- **Harness** — the GitHub repository's harness is built on the Inspect AI framework.
- **Licence** — prompts, golden answers, metadata and expert-made charts are CC BY 4.0.

## Limitations
- **Mostly held out.** Two public tasks; the rest is on request (benchmarks@surgehq.ai).
- **Licence mix.** Charts from public sources are not CC BY 4.0 and are for non-commercial research only.
- **Judge-dependent.** A model judge decides each match.
- **Training ban.** The maker asks users to keep tasks out of training, fine-tuning, reinforcement learning and distillation data.

## Example
The dataset card illustrates grading with an **acceptable range**: a golden answer of "150 mph, acceptable 140–150 mph" accepts any value in that range. So an answer close enough to the chart's true value passes, while a misread axis fails. The two public tasks (a healthcare task and a civil and environmental engineering task) are not quoted here because the maker asks that tasks stay out of training data.
