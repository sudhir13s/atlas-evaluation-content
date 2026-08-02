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
worked_examples:
  - task_type: "College advanced mathematics (item in Chinese; shown here in English)"
    input: "Evaluate the definite integral ∫₀^π sin(x) dx."
    options:
      - "0"
      - "1"
      - "2"
      - "−2"
    success:
      answer: "C — 2"
      why: "The antiderivative is −cos(x); [−cos x]₀^π = (−cos π) − (−cos 0) = 1 + 1 = 2."
    failure:
      answer: "D — −2"
      why: "It uses +cos(x) as the antiderivative instead of −cos(x), flipping the sign to give cos π − cos 0 = −2 — a classic sign error."
    checks: "Calculus reasoning on a Chinese-language exam item; decoding the language is not enough, the answer turns on the math."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "High-school chemistry (item in Chinese)"
    input: "How many moles of electrons are transferred when 1 mol of Cl₂ is completely reduced to Cl⁻?"
    options:
      - "1 mol"
      - "2 mol"
      - "0.5 mol"
      - "4 mol"
    success:
      answer: "B — 2 mol"
      why: "1 mol Cl₂ contains 2 mol of Cl atoms, and each gains one electron (Cl₂ + 2e⁻ → 2Cl⁻), so 2 mol of electrons are transferred."
    failure:
      answer: "A — 1 mol"
      why: "It counts one electron per Cl₂ molecule, forgetting that the molecule has two chlorine atoms each gaining an electron."
    checks: "Balancing electron transfer in a redox half-reaction, tracking atoms per molecule."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Chinese history (item in Chinese)"
    input: "Which dynasty first unified China and standardized its writing system, currency, and weights and measures?"
    options:
      - "Han dynasty"
      - "Qin dynasty"
      - "Tang dynasty"
      - "Zhou dynasty"
    success:
      answer: "B — Qin dynasty"
      why: "Qin Shi Huang unified China in 221 BCE and imposed standardized script, currency, and measures across the empire."
    failure:
      answer: "A — Han dynasty"
      why: "The Han consolidated and expanded the empire but came after the Qin, which was the first to unify and standardize — a common ordering slip."
    checks: "Recall of a foundational fact of Chinese history and the correct dynastic sequence."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Civics and law / moral cultivation (item in Chinese)"
    input: "Under China's Constitution, which body is the highest organ of state power and holds the authority to amend the Constitution?"
    options:
      - "The State Council"
      - "The National People's Congress"
      - "The Supreme People's Court"
      - "The Central Military Commission"
    success:
      answer: "B — The National People's Congress"
      why: "The NPC is defined as the highest organ of state power and is the body empowered to enact and amend the Constitution."
    failure:
      answer: "A — The State Council"
      why: "The State Council is the executive (administrative) organ, not the legislature — a common confusion of executive and legislative roles."
    checks: "Knowledge of the structure of the Chinese state and separation of governing bodies."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Middle-school biology (item in Chinese)"
    input: "Which organelle is the primary site of photosynthesis in a plant cell?"
    options:
      - "Mitochondrion"
      - "Chloroplast"
      - "Nucleus"
      - "Ribosome"
    success:
      answer: "B — Chloroplast"
      why: "Chloroplasts contain chlorophyll and carry out photosynthesis, converting light energy into chemical energy."
    failure:
      answer: "A — Mitochondrion"
      why: "The mitochondrion is the site of cellular respiration, not photosynthesis — the two energy pathways are frequently swapped."
    checks: "Matching an organelle to its function and distinguishing photosynthesis from respiration."
    scoring: "Exact-match accuracy on the selected option."
tags: [knowledge, multiple-choice, chinese, multilingual, reasoning]
updated: 2026-08-02
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
