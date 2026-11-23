---
id: benchmark/blueprint-bench
type: benchmark
slug: blueprint-bench
schema_version: 3.1
title: "Blueprint-Bench"
name: Blueprint-Bench (including Blueprint-Bench 2)
category: Multimodal
description: "An Andon Labs benchmark that asks a model to turn about 20 photographs of an apartment into an accurate 2D floor plan, testing spatial reasoning."
what_is_it: "Blueprint-Bench, from Andon Labs (paper by Lukas Petersson and colleagues, September 2025), tests spatial reasoning by converting apartment photographs into a 2D floor plan. It has 50 apartments; for each, a model examines about 20 interior photos and draws a plan showing rooms, doorways and relative room sizes under strict drawing rules. Ground-truth plans were adapted from the official floor plan images of apartment listings. Blueprint-Bench 2 (May 2026) has agents process the 50 apartments in sequence with a persistent notepad, so they can carry lessons from one apartment to the next."
why_people_use_it: "Photos are familiar training data, but reconstructing space from them needs spatial intelligence. The paper evaluated language models, image-generation models and coding agents, and found all of them well below human performance. Andon Labs says that in Blueprint-Bench 2 it sees the first signs of genuine spatial reasoning from 2D photographs."
method: "The model's floor-plan image is turned into a graph by code: red dots mark room centres, flood fill finds each room, and green door pixels define connections between rooms. That graph is compared with the ground-truth graph by a weighted similarity score. Blueprint-Bench 2 matches plans under rotations and reflections and normalises scores so the random baseline is 0 and a perfect plan is 1."
metric: "Similarity score from 0 to 1, a weighted sum of six parts: Jaccard overlap of room connections (50%), degree correlation (20%), graph density (10%), room count accuracy (10%), door count accuracy (5%) and door orientation distribution (5%). Blueprint-Bench 2 headlines this as a connectivity similarity score."
format: "About 20 photos in, one floor-plan image out: black 3-pixel straight walls, green doorways, one 10-by-10-pixel red dot per room, pure white background, no furniture or windows. Blueprint-Bench 2 is agentic, with 50 apartments in a row and a notepad across them."
limitations: "Most of the data is kept private to prevent overfitting, so outside reproduction is limited to the public sample. Rooms are matched by size rank rather than room type, so a size-ranking error also costs connectivity points, and room shape is not scored. The original and version 2 are different setups (single-pass floor plans versus an agent across 50 apartments with a notepad), so scores are not comparable. The human baseline covers only a 12-apartment subset of the original."
citation: "https://arxiv.org/abs/2509.25229"
openness: partly-open
openness_note: "The paper, the scoring method and the generation code with a sample of the dataset are public on GitHub, while most apartments are kept private to prevent overfitting."
tags: [multimodal, spatial-reasoning, floor-plans, agent, andon-labs]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2509.25229", title: "Blueprint-Bench paper (Petersson et al., 2025, arXiv:2509.25229)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2509.25229", title: "Blueprint-Bench paper, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://andonlabs.com/evals/blueprint-bench", title: "Blueprint-Bench — Andon Labs", trust: 0.95 }
  - { url: "https://andonlabs.com/evals/blueprint-bench-2", title: "Blueprint-Bench 2 — Andon Labs", trust: 0.95 }
  - { url: "https://github.com/AndonLabs/Blueprint-Bench-generation", title: "AndonLabs/Blueprint-Bench-generation — code and dataset sample", trust: 0.9 }
related: [benchmark/vending-bench]
---

## What It Tests
Whether a model can **reconstruct a space from photographs**.

- **Rooms** — identify them from about 20 interior photos.
- **Connections** — work out doorways and how rooms link.
- **Scale** — rank rooms by relative size.

**Who built it:** Andon Labs; the paper's authors are Lukas Petersson, Axel Backlund, Axel Wennström, Hanna Petersson, Callum Sharrock and Arash Dabiri (arXiv, September 2025).

## How It Scores
A **similarity score** from 0 to 1 compares the graph drawn by the model with the true one.

| Part | Weight |
|---|---|
| Jaccard overlap of room connections | 50% |
| Degree correlation (how many doors each room has) | 20% |
| Graph density | 10% |
| Room count accuracy | 10% |
| Door count accuracy | 5% |
| Door orientation distribution | 5% |

- **Jaccard overlap** — shared connections / all connections in either plan.
- **Random baseline** — plans drawn by models given no photos; version 2 rescales so this baseline is 0.
- **Human baseline** — a person drew plans from the photo folder; connectivity was always right, but size rankings were not always right.

## Format
Photos in, a floor-plan image out, drawn to strict colour and line rules so code can read it.

- **Paper's models** — language models, two image-generation models and two coding agents in a Linux container.
- **Version 2** — an agent handles 50 apartments one after another and keeps a notepad across them.

## Limitations
- **Mostly private.** Only a sample of apartments is public.
- **Size-rank matching.** Rooms are matched by size, not type; shape is not scored.
- **Two setups.** Original and version 2 are not comparable.
- **Limited baseline.** Humans were tested on 12 apartments for the original only.

## Example
As the paper and maker describe it, a task is one apartment:

- **Input** — about 20 interior photos.
- **Output** — a plan with black walls, a **red dot** in each room and **green lines** for doorways, as in the maker's overview figure.
- **Scoring** — code reads the rooms and doors from the image, builds a connection graph and compares it with the plan from the apartment's listing.

No individual apartment is reproduced here; the maker keeps most of them private.
