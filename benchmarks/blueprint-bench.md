---
id: benchmark/blueprint-bench
type: benchmark
slug: blueprint-bench
schema_version: 1.0
title: "Blueprint-Bench"
name: Blueprint-Bench (including Blueprint-Bench 2)
category: Multimodal
description: "An Andon Labs benchmark that asks a model to turn about 20 photographs of an apartment into an accurate 2D floor plan, testing spatial reasoning."
what_is_it: "Blueprint-Bench tests spatial reasoning by converting apartment photographs into a 2D floor plan. A model examines about 20 interior photos and generates a plan showing room layouts, connections and relative sizes. Success requires identifying rooms, inferring spatial relationships, understanding scale and following strict output formatting. Blueprint-Bench 2 has agents process 50 apartments in sequence with a persistent notepad, so they can carry lessons from one apartment to the next."
why_people_use_it: "Photos are familiar training data, but reconstructing space from them needs spatial intelligence. Andon Labs built the original to track when such ability would appear. In the original, model outputs were essentially noise, and it reports that Blueprint-Bench 2 shows the first signs of genuine spatial reasoning from 2D photographs. A human baseline exists for the original on a subset of 12 apartments."
method: "The model's floor plan is compared with the true plan using similarity scoring. The Blueprint-Bench 2 page headlines a connectivity similarity score."
metric: "Similarity score between the generated and true floor plans; Blueprint-Bench 2 reports a connectivity similarity score."
format: "Photos in, structured floor plan out. Blueprint-Bench 2 is agentic, with 50 apartments and a notepad across apartments."
limitations: "The original and version 2 are different setups (single-pass floor plans versus an agent across 50 apartments with a notepad), so scores are not comparable. The page reports a human baseline only for the original, on a 12-apartment subset."
citation: "https://andonlabs.com/evals/blueprint-bench-2"
tags: [multimodal, spatial-reasoning, floor-plans, agent, andon-labs]
updated: 2026-11-21
sources:
  - { url: "https://andonlabs.com/evals/blueprint-bench", title: "Blueprint-Bench — Andon Labs", trust: 0.95 }
  - { url: "https://andonlabs.com/evals/blueprint-bench-2", title: "Blueprint-Bench 2 — Andon Labs", trust: 0.95 }
related: [benchmark/vending-bench]
---

## What It Tests
Whether a model can **reconstruct a space from photographs**.

- **Rooms** — identify them from about 20 interior photos.
- **Connections** — work out doorways and how rooms link.
- **Scale** — keep relative sizes consistent.

## How It Scores
A **similarity score** compares the generated floor plan with the true one.

- **Version 2** — headline is a connectivity similarity score.
- **Human baseline** — available for the original on a subset of 12 apartments.

## Format
Photos in, a floor plan in a strict structured format out. In version 2, an agent handles 50 apartments one after another and keeps a notepad across them.

## Limitations
- **Two setups.** Original and version 2 are not comparable.
- **Limited baseline.** Humans were tested on 12 apartments for the original only.

## Example
The maker's overview figure shows the task: apartment photographs on one side and the resulting **2D floor plan** on the other, with red dots marking rooms and green lines marking doorways.