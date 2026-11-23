---
id: evaluation-dataset/mt-bench
type: evaluation-dataset
slug: mt-bench
schema_version: 4.0
title: MT-Bench
name: MT-Bench
category: LLM-as-Judge
size: 80 multi-turn questions across 8 categories (160 turns)
url: https://huggingface.co/spaces/lmsys/mt-bench
description: A curated set of 80 two-turn questions scored 1-10 by a strong judge model.
what_it_tests: Multi-turn conversational quality across writing, roleplay, reasoning, math, coding, extraction, STEM, and humanities — including whether a model follows up correctly on a second-turn instruction.
why_it_exists: To approximate human judgment of open-ended chat quality cheaply and quickly. It validated the 'LLM-as-a-judge' methodology (a strong model scoring answers) by showing high agreement with human preferences, later formalized alongside Chatbot Arena.
how_to_use: Generate answers to the 80 two-turn prompts, then have a judge model (originally GPT-4) score each answer 1-10 against a rubric; the model's score is the average. Single-answer grading and pairwise variants are both supported.
known_issues: 'The judge model has biases — it favors verbose, formal, and self-consistent answers and can exhibit position and self-preference bias. Only 80 questions, so scores are coarse. Judge quality caps the ceiling: a judge cannot reliably grade answers better than itself.'
tags:
- llm-as-judge
- multi-turn
- chat
updated: '2026-07-17'
sources:
- url: https://arxiv.org/abs/2306.05685
  title: Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena (Zheng et al., 2023)
  trust: 0.95
related:
- evaluation-dataset/chatbot-arena
- benchmark/ifeval
---

## What It Is

A small, curated benchmark of 80 challenging two-turn questions used to measure conversational
ability via LLM-as-a-judge scoring. It comes from the same team as Chatbot Arena and was the vehicle
for validating judge-model evaluation against human preference.

## How It Evaluates

For each of the 80 prompts the model answers both turns; a strong judge model then rates each answer
1–10 against a rubric (correctness, helpfulness, depth). The reported score is the average across
turns and questions. Both single-answer grading and pairwise comparison modes exist.

## What It Looks For

Open-ended chat quality that fixed-answer benchmarks miss — especially the second turn, which tests
whether a model can adapt to a follow-up instruction (e.g. "now make it more casual") rather than
repeating itself.

## Known Issues

Judge bias is the core caveat: the grader rewards length, formality, and confident consistency, and
shows position and self-preference effects. With only 80 questions the resolution is low. And the
judge sets the ceiling — it cannot reliably distinguish answers stronger than its own ability.
