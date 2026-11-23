---
id: benchmark/gdpval
type: benchmark
slug: gdpval
schema_version: 4.0
title: GDPval
name: GDPval
category: Professional Work
description: An OpenAI benchmark of real work products across 44 occupations in the top 9 industries by US GDP, graded by blind expert comparison with human deliverables.
what_is_it: GDPval, released by OpenAI on 2025-09-25 (paper by Patwardhan et al.), measures model performance on economically valuable, real-world tasks. It covers the majority of US Bureau of Labor Statistics work activities for 44 occupations in the 9 sectors that contribute most to US GDP. The full set has 1,320 tasks, 30 per occupation; a gold subset of 220 tasks, 5 per occupation, is published on Hugging Face. Every task is based on a real work product written by professionals with 14 years of experience on average.
why_people_use_it: It ties model progress to the work people are paid to do instead of exam questions. The paper says frontier performance improved roughly linearly over time and is approaching expert quality, and that more reasoning effort, task context and scaffolding each help. Artificial Analysis runs its own variant, GDPval-AA, as a standalone evaluation.
examples:
- 'From the public gold set (Accountants and Auditors): an auditor reviews a spreadsheet of Anti-Financial Crime Risk Metrics and tests its accuracy, then returns the requested deliverable.'
method: Experienced graders from the same occupations compare a model's deliverable with the one written by the task's professional, without knowing which is which, then rank them and mark the AI deliverable better than, as good as, or worse than the human one. OpenAI also built an automated grader that estimates how experts would judge a deliverable and offers it as an experimental service at evals.openai.com; OpenAI says it is not yet as reliable as expert graders.
metric: 'Share of tasks where the model''s deliverable is rated better than (win) or as good as (tie) the expert''s, from blind pairwise comparison: (wins + ties) / tasks graded. GDPval-AA reports an Elo rating instead.'
format: Open-ended professional tasks, each with a prompt and often reference files, answered with a deliverable file; graded by blind pairwise comparison with the professional's version.
limitations: Expert grading is slow and costly, and OpenAI says its automated grader is not yet as reliable as expert graders. Only the 220-task gold set is public, so outside results use that subset, not the 1,320-task full set. OpenAI describes an internal GDPval v2 for long-horizon workflows, and Artificial Analysis' GDPval-AA v2.1 is its own pairwise-Elo variant on 220 tasks, so numbers from different versions and graders are not comparable. Tasks are one-shot deliverables, not whole jobs.
citation: https://arxiv.org/abs/2510.04374
openness: partly-open
openness_note: The method and a 220-task gold subset (prompts, reference files, expert deliverables and rubrics) are public on Hugging Face; the rest of the 1,320-task full set is not published.
tags:
- agent
- knowledge-work
- occupations
- economic-value
- openai
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2510.04374
  title: 'GDPval: Evaluating AI Model Performance on Real-World Economically Valuable Tasks (arXiv:2510.04374)'
  trust: 0.95
- url: https://openai.com/index/gdpval
  title: Measuring the performance of our models on real-world tasks (OpenAI, 2025-09-25)
  trust: 0.95
- url: https://huggingface.co/datasets/openai/gdpval
  title: openai/gdpval — gold subset on Hugging Face (220 tasks)
  trust: 0.9
- url: https://artificialanalysis.ai/methodology/intelligence-benchmarking
  title: Artificial Analysis Intelligence Benchmarking Methodology (GDPval-AA)
  trust: 0.85
- url: https://openai.com/index/introducing-gpt-6-2-astra-sol
  title: Introducing GPT-6.2 Astra and Sol (OpenAI, mentions internal GDPval v2)
  trust: 0.9
related:
- benchmark/remote-labor-index
- leaderboard/artificial-analysis
---

## What It Tests

Whether a model can produce **work a professional would accept** in real occupations.

- **Breadth** — 44 occupations across the top 9 US sectors by GDP contribution, such as accountants, lawyers, registered nurses and mechanical engineers.
- **Real work products** — each task is based on something a professional actually produced.
- **Experienced authors** — task writers average **14 years** in their field.

## How It Scores

**Blind expert comparison** against the professional's deliverable. "Blind" means the grader does not know which file the model wrote.

- **Verdicts** — the AI deliverable is better than, as good as, or worse than the expert's.
- **Headline** — **win-or-tie rate** = (wins + ties) / tasks graded.
- **Automated grader** — experimental, offered by OpenAI at evals.openai.com, and not a replacement for expert graders.

## Format

| Set | Tasks | Per occupation | Public? |
|---|---|---|---|
| Full set | 1,320 | 30 | No |
| Gold subset | 220 | 5 | Yes, on Hugging Face |

- **Gold-set rows** — each carries the sector, occupation, prompt, reference files, the expert's deliverable files and a scoring rubric.
- **Content note** — the dataset card warns that some tasks keep mature themes because they reflect real occupational work.

## Limitations

- **Partly open.** Only the 220-task gold set is public; the full 1,320 tasks are not.
- **Expert grading is slow.** The automated grader is not yet as reliable as experts.
- **Versions.** GDPval v2 (OpenAI-internal) and GDPval-AA v2.1 (Artificial Analysis) are separate setups; do not mix their numbers.
- **One-shot.** A task is a single deliverable, not a whole job.

## Example

From the public gold set on the Hugging Face dataset card (sector: Professional, Scientific, and Technical Services; occupation: **Accountants and Auditors**):

- **Prompt opening** — "You are an auditor and as part of an audit engagement, you are tasked with reviewing and testing the accuracy" of a spreadsheet of Anti-Financial Crime Risk Metrics.
- **Inputs** — the reference spreadsheet attached to the task.
- **Grading** — an expert compares the model's file with the professional's file, blind, and the row's rubric lists point-scored checks.
