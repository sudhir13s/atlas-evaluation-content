---
id: benchmark/arc-challenge
type: benchmark
slug: arc-challenge
schema_version: 1.0
title: "ARC-Challenge"
name: ARC-Challenge
category: Knowledge & Academic Reasoning
description: "The Challenge Set of the AI2 Reasoning Challenge (ARC): 2,590 grade-school science questions that both a retrieval method and a word co-occurrence method answered wrongly."
what_is_it: "ARC was released by the Allen Institute for AI (Clark et al., 2018) as 7,787 natural grade-school science questions written for human tests, split into a Challenge Set (2,590 questions) and an Easy Set (5,197). The Challenge Set keeps only questions that both a retrieval-based solver and a word co-occurrence solver got wrong. Questions are multiple choice, with three to five options and four on average."
why_people_use_it: "The authors built the Challenge Set so that questions cannot be answered by retrieving a matching sentence or by word association, to encourage work on questions that need more knowledge and reasoning."
examples:
  - "A student leaves a cup of hot water and a cup of cold water in a room for two hours. What is true afterward? (correct: both reach room temperature via thermal equilibrium)"
method: "Multiple choice: the model picks one option per question. Evaluations differ in the number of in-context examples (shots)."
metric: "Accuracy (% correct) on the Challenge Set."
format: "Multiple choice, 3 to 5 options (4 on average); zero-shot or few-shot."
limitations: "Scores depend on the number of shots and the answer-selection method, so numbers from different setups are not comparable. The questions are grade-school science, so the benchmark tests a narrow slice of knowledge."
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/1803.05457", title: "Think you have Solved Question Answering? Try ARC, the AI2 Reasoning Challenge (Clark et al., 2018)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/1803.05457", title: "ARC paper, full text (ar5iv)", trust: 0.95 }
related: [benchmark/hellaswag, benchmark/winogrande]
---

## What It Tests
**Grade-school science** questions that simple solvers fail.

- **Filtered** — each Challenge question defeated both a retrieval solver and a word co-occurrence solver.
- **Natural questions** — taken from human science tests, not written for the benchmark.

## How It Scores
**Accuracy** on the 2,590-question Challenge Set (train 1,119, dev 299, test 1,172).

## Format
Multiple choice with three to five options, four on average. Reports use zero-shot or few-shot prompts; always state which.

## Limitations
- **Setup-dependent.** Shot count and answer selection change the score.
- **Narrow.** Grade-school science only.

## Example
"A student places a cup of hot water and a cup of cold water in a room. After two hours, what is true
of the two cups?" The correct answer is that **both reach room temperature** — heat flows from the
hot water to the room and from the room to the cold water until thermal equilibrium. A tempting
distractor ("the hot water stays hotter because it started with more energy") targets the common
misconception the item was written to test.
