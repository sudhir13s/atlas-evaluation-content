---
id: benchmark/ifeval
type: benchmark
slug: ifeval
schema_version: 1.0
title: "IFEval"
name: IFEval
category: Instruction Following & Chat
description: "Instruction-following measured by verifiable, programmatically checkable constraints."
what_is_it: "A benchmark of ~540 prompts carrying 'verifiable instructions' — constraints a script can check without human or model judgment, such as 'respond in exactly 3 paragraphs', 'do not use any commas', 'include the keyword X at least twice', or 'answer in JSON'. It isolates whether a model actually obeys formatting and constraint instructions."
why_people_use_it: "It removes judge-model subjectivity: correctness is decided by code, not opinion. That makes it a clean, cheap, reproducible measure of instruction adherence, and it anchors the HF Open LLM Leaderboard v2."
examples:
  - "Write a summary of the water cycle in exactly two paragraphs, and do not use the word 'water'. (checked programmatically for paragraph count and forbidden word)"
method: "0-shot; each response is checked by deterministic verifier functions. Reported at strict/loose granularity and at prompt vs instruction level."
metric: "Instruction-following accuracy (% of constraints satisfied); prompt-level and instruction-level, each in strict and loose variants."
format: "0-shot, programmatic verification."
saturation: medium
limitations: "Only measures verifiable-constraint following, not answer quality — a model can satisfy every format rule while giving a useless answer. Coverage of instruction types is finite. 'Loose' vs 'strict' scoring (which tolerates minor formatting like markdown wrappers) can differ by several points."
citation: "https://arxiv.org/abs/2311.07911"
tags: [instruction-following, formatting, deterministic]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2311.07911", title: "Instruction-Following Evaluation for Large Language Models (Zhou et al., 2023)", trust: 0.95 }
related: [benchmark/mmlu-pro]
---

## What It Tests
Literal instruction adherence: does the model honor explicit, machine-checkable constraints on its
output? It deliberately avoids anything requiring judgment, so the signal is pure compliance.

## How It Scores
Deterministic verifier functions check each constraint. Results are reported four ways:
prompt-level (all constraints in a prompt satisfied) vs instruction-level (each constraint counted),
each in **strict** (exact) and **loose** (tolerant of trivial wrappers like surrounding markdown)
variants. The four numbers together give a fuller picture than any one.

## Format
0-shot. No few-shot examples and no judge model — just the prompt, the response, and the checkers.

## Limitations
It measures **form, not substance**: a response can pass every check and still be unhelpful or wrong.
The instruction taxonomy is finite, so it does not cover every kind of directive. The strict/loose
gap is a real reporting subtlety — always state which you quote.

## Example
"Write a summary of the water cycle in exactly two paragraphs, and do not use the word 'water'." The
verifier counts paragraphs (must equal 2) and scans for the forbidden word. A beautifully written
three-paragraph summary fails on the paragraph-count constraint regardless of its quality — which is
the point.
