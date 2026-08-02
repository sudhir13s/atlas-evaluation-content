---
id: benchmark/c-eval
type: benchmark
slug: c-eval
schema_version: 1.0
title: "C-Eval"
name: C-Eval
category: Knowledge & Academic Reasoning
description: "The first comprehensive Chinese-language knowledge-and-reasoning suite — 13,948 exam questions across 52 disciplines and four difficulty levels."
what_is_it: "A Chinese analog of MMLU: 13,948 multiple-choice questions drawn from real Chinese exams (middle school, high school, college, professional), spanning 52 disciplines, plus a 'C-Eval Hard' subset of the most reasoning-heavy science subjects."
why_people_use_it: "It is the de facto measure of a model's knowledge and reasoning in Chinese, exposing the gap between English-centric and Chinese-oriented models that English benchmarks miss."
examples:
  - "A college-level advanced-mathematics item asking to evaluate a limit or choose the correct derivative, presented in Chinese with four options (A-D) and one correct answer."
method: "Answer-only and chain-of-thought prompting, few-shot (typically 5-shot), over 13,948 4-way multiple-choice questions; the predicted letter is parsed and matched to the key."
metric: "Accuracy (% correct), reported overall and per difficulty level; C-Eval Hard reported separately."
format: "4-way multiple-choice in Chinese; answer-only or few-shot chain-of-thought."
saturation: high
limitations: "Released in 2023 and now largely saturated — strong bilingual models exceed 90%, well above GPT-4's original ~68.7%. Exam-sourced questions raise contamination risk, and the multiple-choice format tests recognition, not Chinese generation or long-form reasoning."
citation: "https://arxiv.org/abs/2305.08322"
example:
  task_type: "College advanced mathematics (item presented in Chinese; shown here in English)"
  input: "Evaluate the definite integral ∫₀^π sin(x) dx. (A) 0 (B) 1 (C) 2 (D) -2"
  success:
    answer: "(C) 2"
    why: "The antiderivative is -cos(x); [-cos x]₀^π = (-cos π) - (-cos 0) = 1 + 1 = 2."
  failure:
    answer: "(D) -2"
    why: "It uses +cos(x) as the antiderivative instead of -cos(x), flipping the sign to give cos π - cos 0 = -2 — a classic sign error."
  checks: "Correct calculus reasoning on a Chinese-language exam item; decoding the language is not enough, the answer turns on the math."
  scoring: "Exact-match accuracy on the selected option."
tags: [knowledge, multiple-choice, chinese, multilingual, reasoning]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2305.08322", title: "C-Eval: A Multi-Level Multi-Discipline Chinese Evaluation Suite for Foundation Models (Huang et al., 2023)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/ceval/ceval-exam", title: "C-Eval dataset (ceval/ceval-exam)", trust: 0.9 }
  - { url: "https://cevalbenchmark.com/", title: "C-Eval official leaderboard", trust: 0.9 }
related: [benchmark/mmlu, benchmark/mmlu-pro]
---

## What It Tests
Broad knowledge and reasoning **in Chinese**, sourced from authentic Chinese exams. The 52
disciplines span STEM, social science, humanities, and other professional fields, and each item is
tagged with one of four difficulty levels: middle school, high school, college, and professional.

Because the content and language are Chinese-native, it surfaces capability gaps that English
benchmarks like MMLU cannot — an English-tuned model can score well on MMLU yet stumble here.

## How It Scores
Accuracy across all 13,948 questions, reported overall and broken down by difficulty. A companion
**C-Eval Hard** subset isolates the most reasoning-intensive science subjects (e.g. advanced math,
physics) as a harder signal.

At release only GPT-4 cleared 60% average; today strong bilingual models sit above 90%, which is why
the suite is treated as largely solved for frontier systems.

## Format
Four-way multiple-choice (A-D) in Chinese. Models are run answer-only or with few-shot chain-of-
thought (5-shot is common); the emitted letter is parsed and compared to the key. The public split
withholds test answers, which are scored via the official leaderboard.

## Limitations
It is saturated for frontier models, so it no longer discriminates at the top. Drawing from public
exams means real contamination exposure, and the multiple-choice recognition format says nothing
about Chinese generation, dialogue, or long-context ability. It is knowledge-centric by design.

## Example
A college-level item might present, in Chinese, a definite integral to evaluate with four candidate
values as options A-D. The distractors correspond to common algebra-sign or bound-substitution
mistakes, so a model that sets up the integral but slips on a sign lands on a plausible wrong option
rather than the correct one — the score turns on the reasoning, not on decoding the language alone.
