---
id: benchmark/math-level-5
type: benchmark
slug: math-level-5
schema_version: 3.1
title: "MATH Level 5"
name: MATH Level 5
category: Math & Logic
description: "The 1,324 hardest (level 5) problems of the MATH test set, run and graded by Epoch AI; a subset of MATH, not the full benchmark."
what_is_it: "MATH Level 5 is the hardest tier of the MATH dataset (Hendrycks et al., 2021), whose problems come from competitions such as the AMC 10, AMC 12 and AIME. MATH has 12,500 problems (7,500 training, 5,000 test); this subset is the 1,324 level-5 problems of the test set. Epoch AI runs it and publishes the results at epoch.ai/benchmarks."
why_people_use_it: "Epoch AI says it uses the level-5 subset because current models have not saturated it, which gives informative results without running all 5,000 test questions. Every model is run by Epoch with the same prompt and the same scorers, so results are an independent re-run rather than vendor-reported figures."
method: "Each problem is sent with a prompt asking for a step-by-step solution whose last line is \"ANSWER: $ANSWER\"; a response without that line scores zero. Epoch AI runs three scorers: normalized string match, symbolic equivalence (SymPy), and a model-graded equivalence check in which gemini-1.5-flash-002 judges whether the answer matches the target. Epoch's charts use the model-graded scorer unless noted otherwise."
metric: "Accuracy = problems judged correct / 1,324, in percent (higher is better)."
format: "0-shot, step-by-step solution ending in a line \"ANSWER: $ANSWER\" (no \\boxed answer needed)."
limitations: "It covers only level-5 problems, so its numbers are not comparable with full-MATH or MATH-500 numbers. The result depends on the scorer: Epoch reports a string-match, a symbolic and a model-graded score, and a response that omits the \"ANSWER:\" line gets no credit even when the working is right."
citation: "https://epoch.ai/benchmarks/math-level-5"
openness: open
openness_note: "The problems are the public MATH test set and Epoch AI publishes its task code (prompt, level filter and scorers) as a public Inspect gist, so anyone can rerun it; only Epoch's own runs are official."
tags: [math, competition, subset, epoch-ai, independent-run]
updated: 2026-11-23
sources:
  - { url: "https://epoch.ai/benchmarks/math-level-5", title: "MATH Level 5 (Epoch AI benchmarks)", trust: 0.95 }
  - { url: "https://gist.github.com/tadamcz/e639d7df0663a9c2e15ac7f97275b01b", title: "MATH Level 5 task definition used by Epoch AI (Inspect)", trust: 0.9 }
  - { url: "https://arxiv.org/abs/2103.03874", title: "Measuring Mathematical Problem Solving With the MATH Dataset (arXiv:2103.03874)", trust: 0.95 }
  - { url: "https://epoch.ai/benchmarks", title: "Epoch AI, Capabilities & benchmarking", trust: 0.9 }
related: [benchmark/math, benchmark/otis-mock-aime-2024-2025]
---

## What It Tests
Whether a model can solve the **hardest competition problems in MATH**.

- **Level 5 only** — the top difficulty tier of the MATH test set, 1,324 problems.
- **Competition sources** — AMC 10, AMC 12, AIME and similar contests.
- **Parent benchmark** — MATH, which has its own record.

## How It Scores
**Accuracy** on the final answer.

- **Answer line** — only the last line, "ANSWER: $ANSWER", is read.
- **Three scorers** — string match, symbolic equivalence, and a model judge (gemini-1.5-flash-002).
- **Headline** — Epoch's charts use the model-judged score unless they say otherwise.

## Format
Zero-shot. The prompt asks for a step-by-step solution and an answer on its own final line; a `\boxed{}` answer is not needed.

- **Task code** — a public Inspect gist from Epoch AI keeps only records whose level is 5.
- **Data copy** — the gist loads a stand-in copy of the MATH test split, and its code comment says this is because of DMCA (copyright takedown) issues with the official dataset copy.

## Limitations
- **Subset.** Numbers are not comparable with full-MATH or MATH-500 numbers.
- **Scorer-dependent.** String, symbolic and model-graded scores can differ.
- **Strict format.** A missing "ANSWER:" line scores zero.

## Example
Epoch AI's page shows no individual problem, so this describes the format. A model receives one level-5 problem with the instruction to solve it step by step and end with the line "ANSWER: $ANSWER". It works through the solution and puts its final answer alone on the last line. The scorer compares that answer with the key; "5/2" and "2.5" count as the same under the symbolic check.
