---
id: benchmark/gdpval
type: benchmark
slug: gdpval
schema_version: 1.0
title: "GDPval"
name: GDPval
category: Agent & Tool Use
description: "An OpenAI benchmark of real work products across 44 occupations in the top 9 industries by US GDP, graded by blind expert comparison with human deliverables."
what_is_it: "GDPval measures model performance on economically valuable, real-world tasks. It covers the majority of US Bureau of Labor Statistics work activities for 44 occupations in the 9 sectors that contribute most to US GDP. The full set has 1,320 tasks, 30 per occupation, and 220 of them form an open gold set. Every task is based on a real work product, such as a legal brief, an engineering blueprint, a customer-support conversation or a nursing care plan, written by professionals with 14 years of experience on average."
why_people_use_it: "It ties model progress to the work people are paid to do instead of exam questions. The paper says frontier performance improved roughly linearly over time and is approaching expert quality, and that more reasoning effort, context and scaffolding help. Artificial Analysis runs its own agentic version, GDPval-AA, as an additional evaluation."
method: "Experienced graders from the same occupations blindly compare a model's deliverable with the one written by the task writer, then rank them and mark the AI deliverable better, as good as, or worse. Task writers also wrote scoring rubrics. OpenAI built an automated grader that estimates how experts would judge a deliverable and offers it as an experimental service at evals.openai.com; the maker says it is not as reliable as expert graders."
metric: "Share of tasks where the model's deliverable is rated better than (win) or on par with (tie) the expert's, from blind pairwise comparison. GDPval-AA reports an Elo rating instead."
format: "Open-ended professional tasks, each based on a real work product, answered with a deliverable; graded by blind pairwise comparison with the expert's version."
limitations: "Human grading is costly, and OpenAI says its automated grader cannot replace expert graders. Only the 220-task gold set is open, so most results use that subset. OpenAI mentions an internal GDPval v2 in a later release, and Artificial Analysis' GDPval-AA v2.1 is its own pairwise-Elo variant on 220 tasks, so numbers from different versions and graders are not comparable. Tasks are one-shot deliverables, not full jobs."
citation: "https://arxiv.org/abs/2510.04374"
tags: [agent, knowledge-work, occupations, economic-value, openai]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2510.04374", title: "GDPval: Evaluating AI Model Performance on Real-World Economically Valuable Tasks (arXiv:2510.04374)", trust: 0.95 }
  - { url: "https://openai.com/index/gdpval", title: "Measuring the performance of our models on real-world tasks (OpenAI, 2025-09-25)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (GDPval-AA)", trust: 0.85 }
  - { url: "https://openai.com/index/introducing-gpt-6-2-astra-sol", title: "Introducing GPT-6.2 Astra and Sol (OpenAI, mentions GDPval v2)", trust: 0.9 }
related: [benchmark/remote-labor-index, leaderboard/artificial-analysis]
---

## What It Tests
Whether a model can produce **work a professional would accept** in real occupations.

- **Breadth** — 44 occupations across the top 9 US industries by GDP contribution.
- **Real work products** — legal briefs, blueprints, support conversations, care plans.
- **Experienced authors** — task writers average 14 years in their field.

## How It Scores
**Blind expert comparison** against the human deliverable.

- **Verdicts** — better, as good as, or worse than the expert's work.
- **Headline** — wins plus ties as a share of tasks.
- **Automated grader** — experimental, offered by OpenAI, and not a replacement for experts.

## Format
1,320 tasks in the full set, 30 per occupation, with 5 per occupation in the 220-task open gold set.

## Limitations
- **Mostly closed.** Only the gold set is open.
- **Expert grading is slow.** The automated grader is less reliable.
- **Versions.** GDPval v2 (OpenAI-internal) and GDPval-AA v2.1 (Artificial Analysis) are separate setups.

## Example
OpenAI's page names three example occupations: a **manufacturing engineer**, an **order clerk** and a **producer**. Each task is based on a real work product, and the model returns its own version. A blinded grader sees the AI file and the professional's file side by side, without knowing which is which, and ranks them.
