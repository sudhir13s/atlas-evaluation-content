---
id: benchmark/mka
type: benchmark
slug: mka
schema_version: 1.0
title: "MKA (Multimodal Knowledge Agent)"
name: Multimodal Knowledge Agent (MKA)
category: Multimodal
description: "A Google DeepMind benchmark of 200 professional tasks whose key information sits in visual files such as drawings, schematics and scans, graded against expert rubrics."
what_is_it: "The Multimodal Knowledge Agent (MKA) benchmark, announced by Google DeepMind on 2026-11-10 and developed with Artificial Analysis, tests agents on visually heavy knowledge work. It has 200 tasks across 10 verticals and 83 occupations. Each task bundles a realistic prompt with multimodal input files and asks for deliverables such as a report, spreadsheet, slide deck or marked-up drawing. A task takes a seasoned professional three to eight hours."
why_people_use_it: "Most knowledge-work benchmarks can be solved from text alone, but real work often depends on blueprints, schematics and charts. MKA forces an agent to perceive those files. Artificial Analysis runs the private 200-task set and publishes the leaderboard, and ten public tasks let anyone inspect the format."
method: "Agents run in Artificial Analysis' open-source Stirrup harness, in a Linux sandbox with code execution, web search and page fetch, and an image viewer for vision models, up to 250 turns and 8 hours per task. An agentic judge opens every submitted file and marks each rubric criterion pass or fail without seeing the input files. Google reports that domain experts agreed with the judge 96% of the time."
metric: "Climb weighted satisfaction rate (Climb WSR), in percent. A task's WSR is the weight of satisfied criteria divided by the total weight. Authors tag each criterion Climb (harder) or Regression (baseline competence), and Climb WSR averages WSR on Climb criteria over tasks."
format: "Agentic: a workspace of input files (PDFs, Office files, spreadsheets, images) and a prompt; the agent submits files and a final answer. Each task has an expert rubric of about 70 yes/no checks weighted 75% factual accuracy and 25% visual design."
limitations: "The 200-task evaluation set is private to Artificial Analysis, so only the 10 public tasks can be inspected. Scores depend on the harness (Stirrup), the turn and time caps, and the judge model, which Artificial Analysis names. Google's table labels one column 'Climb WSR' and another 'Climb WSR accuracy',. The benchmark was announced on 2026-11-10, so its track record is short."
citation: "https://blog.google/innovation-and-ai/models-and-research/gemini-models/multimodal-knowledge-agent-benchmark/"
tags: [multimodal, agent, knowledge-work, documents, google-deepmind]
updated: 2026-11-21
sources:
  - { url: "https://blog.google/innovation-and-ai/models-and-research/gemini-models/multimodal-knowledge-agent-benchmark/", title: "Introducing the Multimodal Knowledge Agent benchmark (Google DeepMind blog)", trust: 0.9 }
  - { url: "https://artificialanalysis.ai/evaluations/mka", title: "MKA benchmark — Artificial Analysis evaluation page", trust: 0.9 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (MKA)", trust: 0.85 }
  - { url: "https://huggingface.co/datasets/google/multimodal-knowledge-agent", title: "google/multimodal-knowledge-agent — 10 public tasks (Apache-2.0)", trust: 0.9 }
related: [leaderboard/artificial-analysis]
---

## What It Tests
Whether an agent can **read visually dense files and produce professional deliverables**.

- **Verticals** — Business, Construction, Education, Engineering, Finance, Healthcare, Law and government, Logistics and mapping, Science, Software and data.
- **Visual inputs** — architectural drawings, electrical schematics, geospatial rasters, plates and multi-axis charts.
- **Deliverables** — spreadsheets, slide decks, documents or structured reports.

## How It Scores
An **agentic judge** checks each weighted rubric criterion.

- **Criteria** — 14,284 in total in the 200-task set, weights 1 to 5 (Artificial Analysis).
- **Climb vs Regression** — Climb criteria are harder; Regression checks baseline competence.
- **Headline** — Climb WSR, the mean over tasks.

## Format
Each task has a prompt, a workspace of input files and an expert rubric of about 70 yes/no checks, weighted 75% on factual accuracy and 25% on visual design. The agent gets code, search, fetch and an image tool.

## Limitations
- **Private set.** Only 10 tasks are public; the 200-task set is held by Artificial Analysis.
- **Setup-dependent.** Harness, turn cap and judge model affect results.
- **Young benchmark.** Announced on 2026-11-10, so the track record is short.

## Example
A public example on Artificial Analysis' page is for the **Construction** vertical, role Architect. The prompt asks the agent to build a PowerPoint audit of at most 10 slides that compares **five floor plans** with two building sections and four elevations. It must check cut content, what is visible beyond the cut and every exterior door and window opening. Only mismatches and 'cannot determine' findings are reported, supported by crops from the drawings. Twelve input files include a coordination-review standard.
