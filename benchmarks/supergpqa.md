---
id: benchmark/supergpqa
type: benchmark
slug: supergpqa
schema_version: 1.0
title: "SuperGPQA"
name: SuperGPQA
category: Knowledge & Academic Reasoning
description: "A 26,529-question graduate-level benchmark spanning 285 disciplines, with up to 10 answer options per question to defeat guessing."
what_is_it: "A large-scale successor to GPQA and MMLU-Pro that scales expert-authored, graduate-level questions to 285 subfields — including long-tail disciplines like agronomy and light industry — with an average of ~9.67 options per item to make lucky guessing nearly worthless."
why_people_use_it: "It restores headroom at the frontier: where GPQA has only 448 questions and MMLU-Pro ~12K over 14 fields, SuperGPQA covers 285 subfields at 26K+ questions, so it discriminates among top reasoning models that saturate narrower knowledge tests."
examples:
  - "A graduate materials-science item asking which crystal defect mechanism dominates creep at a given temperature range, presented with roughly ten expert-written options where several are subtly plausible."
method: "Zero-shot evaluation over 26,529 multiple-choice questions with up to 10 options each; built via a Human-LLM collaborative pipeline (80+ expert annotators) that adds distractors and filters trivial or ambiguous items."
metric: "Accuracy (% correct), reported overall and sliced by discipline, field, subfield, and difficulty (easy/medium/hard)."
format: "Multiple-choice with an average of ~9.67 options (up to 10); zero-shot, chain-of-thought optional."
saturation: low
limitations: "New (Feb 2025) so contamination is limited but not zero. Many-option distractors make answer parsing and prompt template matter. Some long-tail subfields have thin coverage, and it remains a knowledge-and-reasoning multiple-choice test — no generation, tools, or long context."
citation: "https://arxiv.org/abs/2502.14739"
tags: [knowledge, multiple-choice, reasoning, graduate-level, frontier]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2502.14739", title: "SuperGPQA: Scaling LLM Evaluation across 285 Graduate Disciplines (M-A-P & ByteDance, 2025)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/m-a-p/SuperGPQA", title: "SuperGPQA dataset (m-a-p/SuperGPQA)", trust: 0.9 }
  - { url: "https://github.com/SuperGPQA/SuperGPQA", title: "SuperGPQA GitHub repository", trust: 0.85 }
related: [benchmark/gpqa, benchmark/gpqa-diamond, benchmark/mmlu-pro]
---

## What It Tests
Graduate-level knowledge and reasoning at breadth no prior benchmark reached: **13 disciplines, 72
fields, and 285 subfields**. Crucially it reaches long-tail areas — agronomy, light industry,
service science, military science — that mainstream tests ignore.

Each of the 26,529 questions is expert-authored and multiple-choice, but with an average of **~9.67
options** (up to 10). That wide option set drops the random baseline near 10% and blunts process-of-
elimination, forcing genuine reasoning.

## How It Scores
Accuracy over the full set, reported overall and sliced by discipline, field, subfield, and a
difficulty label (easy / medium / hard) attached to every item. The difficulty slices let you see
whether a model's edge is uniform or concentrated in easy questions.

At release the strongest reasoning models scored only in the low-60s percent, leaving large headroom
— which is the point of building it.

## Format
Zero-shot multiple-choice; the model reads the question plus up to ten options and emits a letter,
optionally with chain-of-thought. Questions were produced by a Human-LLM collaborative pipeline —
80+ expert annotators write and refine items, add plausible distractors, and iteratively filter out
trivial or ambiguous ones.

## Limitations
It is knowledge-and-reasoning multiple-choice only — it says nothing about generation, agentic
behavior, or long context. Being new (Feb 2025) its contamination is low today but will grow. The
many-option format makes scores sensitive to answer parsing and prompt template, and some long-tail
subfields carry relatively few questions, so those per-subfield estimates are noisier.

## Example
A graduate metallurgy item might ask which deformation mechanism governs creep in a nickel
superalloy over a stated temperature and stress regime, offering roughly ten mechanisms as options.
Several distractors (dislocation glide, grain-boundary sliding, Coble vs. Nabarro-Herring diffusion
creep) are each correct in *some* regime, so a model must reason about the specific conditions rather
than recognize a keyword — and with ten options, guessing after eliminating two or three still leaves
it badly exposed.
