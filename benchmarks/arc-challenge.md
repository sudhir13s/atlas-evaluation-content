---
id: benchmark/arc-challenge
type: benchmark
slug: arc-challenge
schema_version: 1.0
title: "ARC-Challenge"
name: ARC-Challenge
category: Knowledge & Academic Reasoning
description: "AI2 Reasoning Challenge — grade-school science questions that resist retrieval and word-association."
what_is_it: "The hard partition of the AI2 Reasoning Challenge: 2,590 grade-school-level science questions selected because simple retrieval and co-occurrence solvers get them wrong. It rewards multi-step reasoning over surface pattern matching."
why_people_use_it: "A long-standing, standardized reasoning benchmark included in the original HF Open LLM Leaderboard, so historical scores exist for almost every model — useful for apples-to-apples comparison across generations."
examples:
  - "A student leaves a cup of hot water and a cup of cold water in a room for two hours. What is true afterward? (correct: both reach room temperature via thermal equilibrium)"
method: "25-shot multiple-choice was the HF Open LLM Leaderboard v1 protocol; papers also report 0-shot. Answer = highest-likelihood option (usually 4 choices)."
metric: "Accuracy (% correct) on the Challenge partition."
format: "25-shot (Open LLM v1) or 0-shot multiple-choice."
saturation: high
limitations: "Now saturated for frontier models (95%+), so it no longer discriminates at the top. The Easy partition is meaningless for modern models. Scores depend on the shot count, so v1-era 25-shot numbers are not comparable to 0-shot numbers."
citation: "https://arxiv.org/abs/1803.05457"
tags: [reasoning, science, multiple-choice]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/1803.05457", title: "Think you have Solved Question Answering? Try ARC (Clark et al., 2018)", trust: 0.95 }
related: [benchmark/hellaswag, benchmark/winogrande]
---

## What It Tests
Multi-step grade-school science reasoning that cannot be solved by keyword retrieval or word
co-occurrence. Questions were filtered specifically to defeat an information-retrieval baseline and a
word-association baseline, isolating actual reasoning.

## How It Scores
Accuracy on the 2,590-question Challenge set. Human performance is ~95%. Random baseline is ~25%
(most items have four options). Report the Challenge partition — the Easy set is not informative for
capable models.

## Format
Two protocols dominate: the HF Open LLM Leaderboard v1 used **25-shot**, while papers frequently
report **0-shot**. The shot count moves scores by several points, so a 25-shot number and a 0-shot
number are not directly comparable.

## Limitations
The benchmark is **saturated**: frontier models score 95%+ and differences are within noise. That
makes it useful mainly as a regression check and for historical comparison, not for ranking today's
best models. Always note the shot count when quoting a score.

## Example
"A student places a cup of hot water and a cup of cold water in a room. After two hours, what is true
of the two cups?" The correct answer is that **both reach room temperature** — heat flows from the
hot water to the room and from the room to the cold water until thermal equilibrium. A tempting
distractor ("the hot water stays hotter because it started with more energy") targets the common
misconception the item was written to test.
