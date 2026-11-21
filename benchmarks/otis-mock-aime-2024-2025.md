---
id: benchmark/otis-mock-aime-2024-2025
type: benchmark
slug: otis-mock-aime-2024-2025
schema_version: 1.0
title: "OTIS Mock AIME 2024-2025"
name: OTIS Mock AIME 2024-2025
category: Math & Logic
description: "45 competition-style math problems with integer answers from 0 to 999, taken from three OTIS Mock AIME exams and run by Epoch AI."
what_is_it: "The OTIS Mock AIME is an unofficial annual exam meant to emulate the American Invitational Mathematics Examination (AIME): 15 problems in 3 hours, each with an integer answer from 000 to 999. Its problems are written by students of Olympiad Training for Individual Study (OTIS), a proof-based olympiad training programme, and Evan Chen runs the exam. Epoch AI's benchmark uses the 45 problems of three editions (2024, 2025 I and 2025 II) and publishes results at epoch.ai/benchmarks."
why_people_use_it: "Epoch AI describes the problems as harder than MATH Level 5 but easier than FrontierMath, which places the benchmark between the two. The exam's author says the Mock AIME is somewhat harder than the real AIME, by perhaps 2 to 4 problems."
method: "Each problem is sent with a prompt saying it is an AIME problem whose answer is an integer from 000 to 999, asking for step-by-step work ending in \"ANSWER: X\". Since September 2025 a model-based extractor pulls out the final answer without seeing the key, and that answer is checked for an exact match. Before then, a model scorer compared the response with the true answer. For the 3 problems that contain an image, the image is left out and the TikZ or Asymptote code that draws it is included instead."
metric: "Accuracy: the share of the 45 problems answered correctly (higher is better)."
format: "0-shot, step-by-step solution ending in \"ANSWER: X\", where X is an integer from 000 to 999."
limitations: "Epoch AI notes that its earlier model scorer had idiosyncrasies, including a tendency to grade blank answers as correct, which led to the September 2025 change; scores from before and after the change were produced by different graders. Image problems are given as drawing code, not as images."
citation: "https://epoch.ai/benchmarks/otis-mock-aime-2024-2025"
tags: [math, competition, aime, integer-answer, epoch-ai, independent-run]
updated: 2026-11-21
sources:
  - { url: "https://epoch.ai/benchmarks/otis-mock-aime-2024-2025", title: "OTIS Mock AIME 2024-2025 (Epoch AI benchmarks)", trust: 0.95 }
  - { url: "https://gist.github.com/tadamcz/faf4681e154be2e4c8a6579d67aca7d3", title: "Mock AIME 2024-2025 task definition used by Epoch AI (Inspect)", trust: 0.9 }
  - { url: "https://web.evanchen.cc/mockaime.html", title: "OTIS Mock AIME (Evan Chen)", trust: 0.95 }
  - { url: "https://web.evanchen.cc/otis.html", title: "Olympiad Training for Individual Study (OTIS)", trust: 0.9 }
  - { url: "https://epoch.ai/benchmarks", title: "Epoch AI, Capabilities & benchmarking", trust: 0.9 }
related: [benchmark/aime, benchmark/math-level-5]
---

## What It Tests
Whether a model can solve **AIME-style competition problems** that are new and somewhat harder than the real AIME.

- **Source** — three OTIS Mock AIME exams: 2024, 2025 I and 2025 II, 15 problems each.
- **Authors** — students of the OTIS olympiad programme; Evan Chen runs the exam.
- **Difficulty** — between MATH Level 5 and FrontierMath, by Epoch AI's description.

## How It Scores
**Exact match** on an integer answer.

- **Extraction** — since September 2025 a model pulls out the final answer without seeing the key.
- **Check** — the extracted integer must equal the answer key.
- **Headline** — the share of the 45 problems solved.

## Format
Zero-shot. The prompt states the 000 to 999 answer range and asks for step-by-step work ending in "ANSWER: X". Image problems come with their TikZ or Asymptote source.

## Limitations
- **Grader change.** The earlier model scorer could mark blank answers correct; it was replaced in September 2025.
- **No images.** Diagrams are given only as drawing code.

## Example
A model receives one problem from, for example, the 2025 I exam, with the instruction that the answer is an integer from 000 to 999. It shows its work and ends with the line "ANSWER: X". The extractor reads X without seeing the key, and the item counts as correct only if X matches the key exactly.
