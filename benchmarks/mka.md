---
id: benchmark/mka
type: benchmark
slug: mka
schema_version: 3.1
title: "MKA (Multimodal Knowledge Agent)"
name: Multimodal Knowledge Agent (MKA)
category: Multimodal
description: "A Google DeepMind benchmark of 200 professional tasks whose key information sits in visual files such as drawings and blueprints, graded against expert rubrics."
what_is_it: "The Multimodal Knowledge Agent (MKA) benchmark, announced by Google DeepMind on 2026-11-10 and developed with Artificial Analysis, tests agents on visually heavy knowledge work. It has 200 evaluation tasks across 10 verticals and 83 occupations, built with domain experts such as architects, biologists, underwriters and data scientists. Each task pairs a realistic prompt with a bundle of multimodal input files and takes a seasoned professional about three to eight hours."
why_people_use_it: "Google notes that most knowledge-work benchmarks can be solved with text alone; MKA makes the visual files load-bearing. Artificial Analysis runs the private 200-task set and publishes the leaderboard, and ten public tasks let anyone inspect the format."
examples:
  - "From the public example on Artificial Analysis' page (Construction, Architect): produce a slide deck of at most 10 slides that audits five floor plans against two building sections and four elevations, reporting only mismatches and items that cannot be determined, with drawing crops as evidence; 12 input files."
method: "Agents run in the open-source Stirrup harness with a 250-turn limit, in a Linux sandbox with code execution, internet access, web search and page fetch, plus an image viewer for vision models. An agentic judge (named by Artificial Analysis) inspects the submitted files and final response in its own sandbox, without seeing the input files, and marks each rubric criterion satisfied or not. Google reports that domain experts agreed with the judge 96% of the time."
metric: "Climb weighted satisfaction rate (Climb WSR), in percent. A task's WSR = weight of satisfied criteria / total weight of its criteria (weights 1 to 5). Authors tag each criterion Climb (harder) or Regression (baseline competence), and Climb WSR is the mean over tasks of WSR on Climb criteria."
format: "Agentic: a workspace of input files and a prompt; the agent submits files and a final response. Each task has an expert rubric of about 70 yes/no checks weighted 75% on factual accuracy and 25% on visual design."
limitations: "The 200-task evaluation set is private to Artificial Analysis, so only the 10 public tasks can be inspected. Scores depend on the harness (Stirrup), the turn cap and the judge model. A run that submits no files and no final response scores 0. The benchmark was announced on 2026-11-10, so its track record is short."
citation: "https://blog.google/innovation-and-ai/models-and-research/gemini-models/multimodal-knowledge-agent-benchmark/"
openness: partly-open
openness_note: "Ten tasks with their input files and rubrics are public on Hugging Face under Apache-2.0 and the Stirrup harness is open source, but the 200-task scored set is private and run only by Artificial Analysis."
tags: [multimodal, agent, knowledge-work, documents, google-deepmind]
updated: 2026-11-23
sources:
  - { url: "https://blog.google/innovation-and-ai/models-and-research/gemini-models/multimodal-knowledge-agent-benchmark/", title: "Introducing the Multimodal Knowledge Agent benchmark (Google DeepMind blog)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/evaluations/mka", title: "MKA benchmark — Artificial Analysis evaluation page", trust: 0.9 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (MKA)", trust: 0.85 }
  - { url: "https://huggingface.co/datasets/google/multimodal-knowledge-agent", title: "google/multimodal-knowledge-agent — 10 public tasks (Apache-2.0)", trust: 0.9 }
related: [leaderboard/artificial-analysis]
---

## What It Tests
Whether an agent can **read visually dense files and produce professional deliverables**.

- **Verticals** — 10 in all; the public card names Business, Construction, Education, Engineering, Finance, Law and government, Logistics and mapping, Science, and Software and data.
- **Visual inputs** — load-bearing visual files such as architectural CAD blueprints.
- **Deliverables** — work such as slide decks, reports and spreadsheets.

## How It Scores
An **agentic judge** checks each weighted rubric criterion against the submitted files.

- **WSR** — weight of satisfied criteria / total weight, per task; weights run 1 to 5.
- **Climb vs Regression** — Climb criteria are harder; Regression checks baseline competence.
- **Headline** — Climb WSR, the mean over tasks.
- **Judge agreement** — Google reports 96% agreement with domain experts.

## Format
Each task has a prompt, a workspace of input files and an expert rubric of about 70 yes/no checks, weighted **75% factual accuracy, 25% visual design**.

- **Harness** — open-source Stirrup, 250-turn limit, code, internet, search, fetch and an image tool.
- **Public tasks** — each folder holds `metadata.json` (prompt, vertical, role), `rubrics.json` and the `input/` files.

## Limitations
- **Private set.** Only 10 tasks are public; the 200-task set is held by Artificial Analysis.
- **Setup-dependent.** Harness, turn cap and judge model affect results.
- **Young benchmark.** Announced on 2026-11-10, so the track record is short.

## Example
From the public example on Artificial Analysis' MKA page, closely paraphrased (**Construction** vertical, role Architect):

- **Task** — build a slide deck of at most 10 slides that audits **five floor plans** against two building sections and four elevations for coordination problems.
- **Scope** — every exterior door and window opening, with its elevation representation, floor band and written width.
- **Output** — only mismatches and "cannot determine" findings, organised by floor and supported by crops from the drawings.
- **Inputs** — 12 files.
