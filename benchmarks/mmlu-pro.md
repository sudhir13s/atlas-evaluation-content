---
id: benchmark/mmlu-pro
type: benchmark
slug: mmlu-pro
schema_version: 1.0
title: "MMLU-Pro"
name: MMLU-Pro
category: Knowledge & Academic Reasoning
description: "A harder, reasoning-focused MMLU with 10 answer choices and cleaned questions."
what_is_it: "A rebuilt MMLU that expands each item to 10 options (instead of 4), removes trivial and noisy questions, and adds more reasoning-heavy problems so the test discriminates again at the frontier."
why_people_use_it: "MMLU saturated; MMLU-Pro restores headroom. It has become the de facto knowledge-reasoning benchmark on leaderboards like HF Open LLM v2 and Artificial Analysis."
examples:
  - "In quantum mechanics, which best describes the Born rule? (one correct answer among 10 options, e.g. 'it gives the probability of a measurement outcome from the squared amplitude of the wavefunction')"
method: "5-shot chain-of-thought multiple-choice over ~12,000 questions across 14 disciplines; the answer letter is parsed from the CoT response."
metric: "Accuracy (% correct)."
format: "5-shot CoT, 10-way multiple-choice."
saturation: medium
limitations: "Newer, so less contamination data exists — but it is not immune. The 10-option format and CoT dependence make scores sensitive to answer-parsing and prompt template. Still knowledge-centric, so it under-tests generation, tool use, and long-context skills."
citation: "https://arxiv.org/abs/2406.01574"
example:
  task_type: "College physics (quantum mechanics)"
  input: "In quantum mechanics, which statement best describes the Born rule? (representative options from a 10-choice item) (A) It gives the probability of a measurement outcome as the squared modulus of the corresponding wavefunction amplitude. (B) It determines the allowed energy levels of a bound state. (C) It sets the commutation relation between position and momentum. (D) It describes the deterministic time evolution of the wavefunction. ... (J) ..."
  success:
    answer: "(A) probability equals the squared modulus of the amplitude."
    why: "The Born rule maps |ψ|² (the squared amplitude) to the probability density of a measurement outcome."
  failure:
    answer: "(B) It determines the allowed energy levels of a bound state."
    why: "That is the time-independent Schrödinger equation's eigenvalue problem, not the Born rule — a plausible mix-up of two core quantum-mechanics results."
  checks: "Distinguishing closely-related quantum-mechanics principles under a 10-option format that penalizes shallow keyword matching."
  scoring: "Exact-match accuracy on the selected option."
tags: [knowledge, multiple-choice, reasoning]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2406.01574", title: "MMLU-Pro: A More Robust and Challenging Multi-Task Language Understanding Benchmark (Wang et al., 2024)", trust: 0.95 }
related: [benchmark/mmlu, benchmark/gpqa]
---

## What It Tests
The same broad academic knowledge as MMLU but with a heavier reasoning load and far less room for
lucky guessing. Ten answer options drop the random baseline from 25% to ~10%, and distractors are
written to be plausible.

## How It Scores
Accuracy across ~12,000 questions in 14 disciplines. Because the choice set is wider and questions
are cleaner, a given percentage is meaningfully harder than on MMLU — roughly, 70% on MMLU-Pro is
comparable in difficulty to ~85% on MMLU.

## Format
Five-shot with chain-of-thought is the reference protocol; the model reasons then emits a final
letter that is regex-parsed. Scores shift several points if run 0-shot or with a different template,
so cross-report comparisons must match the protocol.

## Limitations
It is knowledge-and-reasoning only — it says nothing about coding, agentic behavior, or long
context. Contamination pressure is lower than MMLU's today but grows as the set ages. Answer parsing
under CoT is a real source of measurement noise.

## Example
"In the context of quantum mechanics, which of the following best describes the significance of the
Born rule?" among ten options. The correct choice states that the Born rule gives the probability of
a measurement outcome as the squared modulus of the corresponding amplitude of the wavefunction — a
distractor that "it determines the energy levels of bound states" (that is the Schrödinger equation)
is designed to catch shallow pattern-matching.
