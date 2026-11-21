---
id: benchmark/mmmu-pro
type: benchmark
slug: mmmu-pro
schema_version: 1.0
title: "MMMU-Pro"
name: MMMU-Pro
category: Multimodal
description: "A harder version of MMMU: text-answerable questions are filtered out, answer options are expanded, and a vision-only setting embeds the question in the image."
what_is_it: "MMMU-Pro is a robust version of the Massive Multi-discipline Multimodal Understanding and Reasoning (MMMU) benchmark. It rebuilds MMMU in three steps: it filters out questions that text-only models can answer, it augments the candidate options, and it adds a vision-only input setting in which the question itself is embedded in an image. Artificial Analysis reports 1,730 questions with ten answer options across 30 academic disciplines."
why_people_use_it: "It removes shortcuts and guessing strategies, so it tests whether a model can see and read at once. The vision-only setting mimics real scenarios in which text and image arrive together. The authors report that models score much lower on MMMU-Pro than on MMMU."
method: "Models answer multiple-choice questions in two settings: the standard setting, with text plus image, and the vision-only setting. The paper also tests OCR prompts and chain-of-thought reasoning, finding that OCR prompts have minimal effect while chain of thought generally improves results. Artificial Analysis extracts the answer with a regex."
metric: "Accuracy in percent."
format: "Multiple choice with ten options; 0-shot; image-based, including a vision-only version in which the question is part of the image."
limitations: "The set builds on MMMU questions, so any problems in that source carry over. Scores differ between the standard and vision-only settings and between prompting styles, so a number needs its setting. Multiple choice with a fixed key cannot test open-ended answers. The question and discipline counts (1,730 questions, 30 disciplines) are Artificial Analysis' description of its own run."
citation: "https://arxiv.org/abs/2409.02813"
tags: [multimodal, vision, multiple-choice, multidiscipline]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2409.02813", title: "MMMU-Pro: A More Robust Multi-discipline Multimodal Understanding Benchmark (arXiv:2409.02813)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (MMMU Pro)", trust: 0.85 }
  - { url: "https://artificialanalysis.ai/evaluations/mka", title: "Artificial Analysis evaluations list (MMMU-Pro entry)", trust: 0.8 }
---

## What It Tests
Whether a model **truly sees and reads** an image-based academic question.

- **Filtered** — questions that text-only models can answer were removed.
- **Harder options** — candidate options were augmented.
- **Vision-only** — the question text sits inside the image.

## How It Scores
**Accuracy** on multiple-choice answers.

- **Settings** — standard (text and image) and vision-only.
- **Prompting** — the paper compares OCR prompts and chain of thought.
- **Artificial Analysis** — 1,730 questions with ten options, answers read by a regex.

## Format
Zero-shot multiple choice over images. In the vision-only version the model gets only an image that contains both question and options.

## Limitations
- **Inherits MMMU.** Issues in the source set carry over.
- **Setting matters.** Standard and vision-only scores differ.
- **Counts.** The 1,730-question figure is Artificial Analysis' count for its run.

## Example
The vision-only setting is the distinctive case: instead of passing a question as text plus a picture, the benchmark **embeds the question in the image** and the model must read it from pixels. The other two steps, filtering out text-answerable questions and augmenting the options, make the standard setting harder too.
