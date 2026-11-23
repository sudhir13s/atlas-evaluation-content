---
id: benchmark/healthbench
type: benchmark
slug: healthbench
schema_version: 4.0
title: HealthBench
name: HealthBench
category: Knowledge & Academic Reasoning
description: An OpenAI open-source benchmark of 5,000 realistic health conversations, each graded against a physician-written rubric.
what_is_it: 'HealthBench, released by OpenAI in May 2025, measures the performance and safety of language models in healthcare. It has 5,000 multi-turn conversations between a model and either an individual user or a healthcare professional. Responses are scored against conversation-specific rubrics written by 262 physicians who have practised in 60 countries, with 48,562 unique criteria in total. Conversations fall into seven themes (such as emergency referrals, global health and health data tasks), and criteria cover five axes: accuracy, completeness, communication quality, context awareness and instruction following.'
why_people_use_it: It replaces multiple-choice and short-answer medical tests with open-ended, realistic conversations. OpenAI says evaluations in health should be meaningful, so scores reflect real-world impact and go beyond exam questions. The evaluation code and data are released in OpenAI's simple-evals repository on GitHub.
method: 'Each rubric criterion has a point value from -10 to 10. A model-based grader (GPT-4.1 by default) checks each criterion independently: a met positive criterion earns its points, a met negative criterion subtracts its penalty. To check the grader, physicians reviewed responses in HealthBench Consensus, and OpenAI found model-to-physician agreement similar to physician-to-physician agreement. Artificial Analysis runs HealthBench and a separate 525-conversation HealthBench Professional with its own length-adjusted rubric score and grader models.'
metric: 'Rubric score: per conversation, points earned divided by the maximum possible points; the overall score is the mean across conversations, clipped to 0 to 1 (often shown as a percent). Variants are HealthBench Consensus (34 physician-validated criteria, 3,671 conversations) and HealthBench Hard (1,000 conversations).'
format: Multi-turn health conversations; free-text reply; rubric-graded by a model.
limitations: Grading uses a model grader, so scores inherit its judgements; physician agreement checks reduce but do not remove this. Scores differ across variants (Full, Consensus, Hard) and across graders, because Artificial Analysis uses its own grader models and a length adjustment. It evaluates text responses, not clinical outcomes. The simple-evals repository says it will no longer be updated for new models or results, though it keeps hosting the HealthBench reference implementation.
citation: https://arxiv.org/abs/2505.08775
openness: open
openness_note: All 5,000 conversations, their rubrics and the grading code are public in OpenAI's MIT-licensed simple-evals repository, though the authors ask that examples not be reposted online in plain text or images.
tags:
- safety
- health
- medical
- rubric
- open-source
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2505.08775
  title: 'HealthBench: Evaluating Large Language Models Towards Improved Human Health (arXiv:2505.08775)'
  trust: 0.95
- url: https://arxiv.org/html/2505.08775
  title: HealthBench, full text (arXiv HTML)
  trust: 0.95
- url: https://openai.com/index/healthbench
  title: Introducing HealthBench (OpenAI)
  trust: 0.95
- url: https://github.com/openai/simple-evals
  title: openai/simple-evals — HealthBench reference implementation (MIT)
  trust: 0.9
- url: https://artificialanalysis.ai/methodology/intelligence-benchmarking
  title: Artificial Analysis Intelligence Benchmarking Methodology (HealthBench)
  trust: 0.85
---

## What It Tests

Whether a model's answers in **health conversations** are accurate, safe and well communicated.

- **Themes** — global health, responding under uncertainty, expertise-tailored communication, context seeking, emergency referrals, health data tasks and response depth.
- **Axes** — accuracy, completeness, communication quality, context awareness and instruction following.
- **Users** — both individuals and healthcare professionals.

## How It Scores

**Rubric criteria** written by physicians, checked one by one by a model grader (GPT-4.1 by default).

- **Points** — each criterion is worth -10 to 10; negative criteria penalise harmful or unwanted content.
- **Per conversation** — points earned divided by the maximum possible.
- **Overall** — the mean over conversations, clipped to the range 0 to 1.
- **Scale** — 5,000 conversations, 48,562 unique criteria, 262 physicians.

## Format

A multi-turn chat ending in a model reply, scored per criterion.

- **Variants** — Consensus (34 physician-validated criteria, 3,671 conversations) and Hard (1,000 conversations).
- **Data and code** — in OpenAI's GitHub repository `openai/simple-evals`, which is **MIT-licensed**.
- **Third-party runs** — Artificial Analysis grades HealthBench with GPT-4.1 and a length adjustment, and runs a 525-conversation HealthBench Professional graded by GPT-5.4 (low); neither counts toward its Intelligence Index.

## Limitations

- **Model grader.** Scores depend on the grader and, in some runs, a length adjustment.
- **Text only.** It scores replies, not patient outcomes.
- **Variant mix-ups.** Full, Consensus, Hard and Professional are not interchangeable.
- **Frozen repository.** simple-evals is no longer updated for new models, though HealthBench stays hosted.

## Example

No conversation is quoted here, because the authors ask that HealthBench examples not be posted online in plain text or images. As the paper describes the format:

- **Input** — a multi-turn conversation between a user (a layperson or a clinician) and a model, ending with a turn for the model to answer.
- **Rubric** — criteria written for that conversation, each with points, such as rewarding a referral to emergency care or penalising a harmful claim.
- **Scoring** — the grader marks each criterion met or not; the score is earned points over the maximum.
