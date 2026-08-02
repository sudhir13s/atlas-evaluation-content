---
id: benchmark/mmlu-prox
type: benchmark
slug: mmlu-prox
schema_version: 1.0
title: "MMLU-ProX"
name: MMLU-ProX
category: Multilingual
description: "MMLU-Pro's harder 10-option reasoning questions extended to 29 languages."
what_is_it: "MMLU-ProX takes MMLU-Pro — the reasoning-heavy, 10-choice successor to MMLU — and produces the same 11,829 questions in 29 typologically diverse languages via LLM translation plus expert review."
why_people_use_it: "It is the multilingual test that is still hard at the frontier: unlike translated MMLU (near-saturated), MMLU-ProX keeps MMLU-Pro's difficulty, so it exposes reasoning gaps across languages rather than just recall gaps."
examples:
  - "A university-level physics or economics MMLU-Pro item with 10 answer options, presented identically in Swahili, Thai, Hindi, and 26 other languages."
method: "Few-shot chain-of-thought multiple-choice over 11,829 parallel questions per language (a 658-question lite split also exists); the final answer letter is parsed from the reasoning output."
metric: "Accuracy (% correct), per language and averaged."
format: "10-way multiple-choice, chain-of-thought; run few-shot (inherits the MMLU-Pro protocol)."
saturation: medium
limitations: "Translations are LLM-generated then expert-reviewed rather than fully human-authored, so subtle translationese can remain. It is a translation of English-origin questions (no culturally-native content), and like MMLU-Pro its 10-option chain-of-thought format makes scores sensitive to answer parsing and prompt template."
citation: "https://arxiv.org/abs/2503.10497"
example:
  task_type: "Advanced reasoning MCQ (Swahili, economics)"
  input: "Kwa nini kampuni katika ushindani wa kimonopoli hupata faida sifuri ya kiuchumi kwa muda mrefu? (Why does a monopolistically competitive firm earn zero economic profit in the long run?) — chagua jibu bora kati ya chaguo kumi. (choose the best answer among ten options.)"
  success:
    answer: "Because free entry of new firms erodes profits until price equals average total cost."
    why: "Names the actual long-run zero-profit driver — entry competing away profit — reasoned correctly in-language."
  failure:
    answer: "Because the firm sets marginal revenue equal to marginal cost."
    why: "MR = MC sets the profit-maximizing output level, not the zero-profit condition; a plausible in-language distractor and a reasoning error, not a translation error."
  checks: "Reasoning through a distractor-hardened question in a lower-resource language, not mere fact recall."
  scoring: "Accuracy (% correct)."
tags: [multilingual, knowledge, reasoning, multiple-choice]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2503.10497", title: "MMLU-ProX: A Multilingual Benchmark for Advanced Large Language Model Evaluation (Xuan et al., 2025)", trust: 0.95 }
  - { url: "https://github.com/weihao1115/MMLU-ProX", title: "weihao1115/MMLU-ProX — official EMNLP 2025 repo", trust: 0.9 }
related: [benchmark/mmlu-pro, benchmark/mmmlu, benchmark/mmlu]
---

## What It Tests
Advanced, reasoning-heavy academic knowledge — the same 10-option, distractor-hardened questions
as MMLU-Pro — evaluated in 29 languages. Because it inherits MMLU-Pro's difficulty, it probes
whether a model can *reason* in a language, not merely recall facts in it.

## How It Scores
Accuracy over 11,829 identical questions per language, reported per language and averaged; a
658-question lite split supports cheap iteration. The paper's headline finding is the disparity:
strong performance in high-resource languages but drops of up to 24.3 percentage points in
low-resource ones.

## Format
Ten-way multiple choice with chain-of-thought, run few-shot — the MMLU-Pro reference protocol.
The ten options push the random baseline to ~10% and the reasoning requirement means the answer
is parsed from a generated chain, not a single token.

## Limitations
Questions are translated (LLM-then-expert-review, not fully human), so residual translationese
and English-origin cultural framing remain. As with any chain-of-thought multiple-choice test,
answer-letter parsing and prompt formatting introduce measurement noise, and cross-report
comparisons must fix the same shot count and template.

## Example
A graduate-level economics MMLU-Pro item — "Which of the following best explains why a
monopolistically competitive firm earns zero economic profit in the long run?" among ten
options — appears in each of the 29 languages. A model scoring 78% in German but 54% in Swahili
on the identical question set reveals a reasoning-in-language gap.
