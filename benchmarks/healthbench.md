---
id: benchmark/healthbench
type: benchmark
slug: healthbench
schema_version: 1.0
title: "HealthBench"
name: HealthBench
category: Truthfulness & Safety
description: "An OpenAI open-source benchmark of 5,000 realistic health conversations, each graded against a physician-written rubric."
what_is_it: "HealthBench measures the performance and safety of language models in healthcare. It has 5,000 multi-turn conversations between a model and either an individual user or a healthcare professional. Responses are scored against conversation-specific rubrics written by 262 physicians from 60 countries, with 48,562 unique criteria in total. The criteria cover contexts such as emergencies, transforming clinical data and global health, and behaviours such as accuracy, instruction following and communication."
why_people_use_it: "It replaces multiple-choice and short-answer medical tests with open-ended, realistic conversations. OpenAI says evaluations in health should be meaningful, so scores reflect real-world impact and go beyond exam questions. The evaluation and data are open on GitHub."
method: "A model-based grader checks each rubric criterion against the response. To check the grader, physicians reviewed responses in HealthBench Consensus, and OpenAI found model-to-physician agreement similar to physician-to-physician agreement. Artificial Analysis runs HealthBench and a 525-conversation HealthBench Professional, using a length-adjusted rubric score graded by a GPT model."
metric: "Rubric score in percent: the share of the physician-written rubric a response satisfies. Variants are HealthBench Consensus (34 dimensions validated by physician consensus) and HealthBench Hard."
format: "Multi-turn health conversations; free-text reply; rubric-graded by a model."
limitations: "Grading uses a model grader, so scores inherit its judgements; physician agreement checks reduce but do not remove this. Scores differ across variants (Full, Consensus, Hard) and across graders, because Artificial Analysis uses its own grader model and a length adjustment. It evaluates text responses, not clinical outcomes."
citation: "https://arxiv.org/abs/2505.08775"
tags: [safety, health, medical, rubric, open-source]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2505.08775", title: "HealthBench: Evaluating Large Language Models Towards Improved Human Health (arXiv:2505.08775)", trust: 0.95 }
  - { url: "https://openai.com/index/healthbench", title: "Introducing HealthBench (OpenAI)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (HealthBench)", trust: 0.85 }
---

## What It Tests
Whether a model's answers in **health conversations** are accurate, safe and well communicated.

- **Contexts** — emergencies, transforming clinical data, global health and more.
- **Behaviours** — accuracy, instruction following, communication.
- **Users** — both individuals and healthcare professionals.

## How It Scores
**Rubric criteria** written by physicians, checked by a model grader.

- **Scale** — 5,000 conversations, 48,562 unique criteria, 262 physicians.
- **Variants** — Consensus (34 dimensions agreed by physicians) and Hard.
- **Grader check** — physicians reviewed grading in the Consensus set.

## Format
A multi-turn chat with a final model reply, scored per criterion. Artificial Analysis' HealthBench Professional variant has 525 conversations.

## Limitations
- **Model grader.** Scores depend on the grader and, in some runs, a length adjustment.
- **Text only.** It scores replies, not patient outcomes.
- **Variant mix-ups.** Full, Consensus, Hard and Professional are not interchangeable.

## Example
Each conversation comes with a **custom rubric** written by physicians, made of criteria on the response. A grader marks every criterion met or not, and the results add up to the score. The conversations and rubrics are in OpenAI's GitHub repository.
