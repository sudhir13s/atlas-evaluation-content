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
worked_examples:
  - task_type: "Physical science — thermal equilibrium"
    input: "A student places a cup of hot water and a cup of cold water in the same room and leaves them for two hours. Which statement is true afterward?"
    options:
      - "The hot water is still noticeably warmer than the cold water."
      - "Both cups have reached about the same room temperature."
      - "The cold water is now warmer than the hot water."
      - "Both cups have frozen solid."
    success:
      answer: "B — Both cups have reached about the same room temperature."
      why: "Heat flows from the hot water to the room and from the room to the cold water until both reach thermal equilibrium, so their temperatures converge."
    failure:
      answer: "A — The hot water is still noticeably warmer than the cold water."
      why: "It assumes the hotter cup 'keeps' its extra energy; in reality the cup loses heat to the room until equilibrium erases the difference."
    checks: "Multi-step causal reasoning about thermal equilibrium rather than surface word association."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Life science — energy flow in a food chain"
    input: "In the food chain grass → grasshopper → frog → snake, where does the energy that flows through all the organisms originally come from?"
    options:
      - "The Sun"
      - "The grasshopper"
      - "The soil"
      - "The snake"
    success:
      answer: "A — The Sun"
      why: "Producers (grass) capture sunlight through photosynthesis, so all the energy moving up the chain traces back to the Sun."
    failure:
      answer: "C — The soil"
      why: "Soil supplies water and minerals, not usable energy; this confuses a source of nutrients with the source of energy."
    checks: "Tracing energy in an ecosystem back to its true source rather than to a nearby physical input."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Earth science — cause of the seasons"
    input: "What is the main reason Earth experiences different seasons over the course of a year?"
    options:
      - "Earth's distance from the Sun changes dramatically through the year."
      - "Earth's axis is tilted relative to the plane of its orbit."
      - "The Sun's energy output rises and falls each year."
      - "The Moon periodically blocks sunlight from reaching Earth."
    success:
      answer: "B — Earth's axis is tilted relative to the plane of its orbit."
      why: "The ~23.5° axial tilt changes how directly sunlight strikes each hemisphere as Earth orbits, producing the seasons."
    failure:
      answer: "A — Earth's distance from the Sun changes dramatically through the year."
      why: "A widespread misconception: Earth's orbit is nearly circular and it is actually closest to the Sun during Northern-Hemisphere winter, so distance cannot explain the seasons."
    checks: "Overriding an intuitive distance explanation with the correct axial-tilt mechanism."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Physical science — states of matter"
    input: "When liquid water boils and turns into steam, what happens to the water molecules themselves?"
    options:
      - "They are destroyed and entirely new gas molecules form."
      - "They gain energy, move faster, and spread farther apart."
      - "They break down into smaller molecules."
      - "They combine into larger molecules."
    success:
      answer: "B — They gain energy, move faster, and spread farther apart."
      why: "Boiling is a physical change: the H₂O molecules stay the same but absorb energy and separate as the liquid becomes a gas."
    failure:
      answer: "A — They are destroyed and entirely new gas molecules form."
      why: "This treats a physical phase change as a chemical reaction; the molecules' identity does not change when water boils."
    checks: "Distinguishing a physical phase change from a chemical change at the molecular level."
    scoring: "Exact-match accuracy on the selected option."
  - task_type: "Astronomy — mass versus weight"
    input: "An astronaut weighs much less standing on the Moon than on Earth. What is the best explanation?"
    options:
      - "The Moon has no gravity at all."
      - "The Moon's gravitational pull is weaker than Earth's."
      - "The astronaut's mass becomes smaller on the Moon."
      - "There is no air on the Moon to press down on the astronaut."
    success:
      answer: "B — The Moon's gravitational pull is weaker than Earth's."
      why: "Weight is the force of gravity on an object; the Moon's smaller mass gives about one-sixth of Earth's surface gravity, so the same astronaut weighs less."
    failure:
      answer: "C — The astronaut's mass becomes smaller on the Moon."
      why: "Mass is the amount of matter and does not change with location; only weight (the gravitational force) changes — the classic mass/weight confusion."
    checks: "Separating mass (invariant) from weight (depends on local gravity)."
    scoring: "Exact-match accuracy on the selected option."
tags: [reasoning, science, multiple-choice]
updated: 2026-08-02
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
