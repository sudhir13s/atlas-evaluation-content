---
id: benchmark/mmmu-pro
type: benchmark
slug: mmmu-pro
schema_version: 3.1
title: "MMMU-Pro"
name: MMMU-Pro
category: Multimodal
description: "A harder version of MMMU: text-answerable questions are filtered out, answer options grow to up to ten, and a vision-only setting embeds the question in the image."
what_is_it: "MMMU-Pro (Yue et al., September 2024; arXiv revision May 2025) is a robust version of the Massive Multi-discipline Multimodal Understanding and Reasoning (MMMU) benchmark. It rebuilds MMMU in three steps: it filters out questions that text-only models can answer, it augments the candidate options from four to up to ten, and it adds a vision-only input setting in which the question itself is shown inside a screenshot or photo. The final set has 1,730 questions across 30 subjects in six disciplines, each available in the standard text-plus-image form and the vision-only form."
why_people_use_it: "It removes shortcuts and guessing strategies, so it tests whether a model can see and read at once. The vision-only setting mimics real scenarios in which text and image arrive together. The abstract reports that performance is substantially lower on MMMU-Pro than on MMMU, giving a range of 16.8% to 26.9% across models."
examples:
  - "Name the Artist of <image 1>: (an Art question from the public Hugging Face set; the image and up to ten options accompany it)"
method: "Models answer multiple-choice questions in two settings: standard (question text plus image, ten options) and vision-only (only an image that contains question and options). The overall MMMU-Pro score is the average of the two. The paper also tests OCR prompts and chain-of-thought reasoning, finding that OCR prompts have minimal effect while chain of thought generally improves results."
metric: "Accuracy = questions answered correctly / questions asked, in percent; the overall score averages the standard 10-option accuracy and the vision-only accuracy."
format: "Multiple choice with up to ten options; image-based, including a vision-only version in which the question is part of the image. The public set also offers a 4-option standard variant."
limitations: "The set builds on MMMU questions, so any problems in that source carry over. Scores differ between the standard and vision-only settings and between prompting styles, so a number needs its setting. Multiple choice with a fixed key cannot test open-ended answers. The questions and answers are public, so they can appear in training data."
citation: "https://arxiv.org/abs/2409.02813"
openness: open
openness_note: "All 1,730 questions, images and answer letters are public on Hugging Face under Apache-2.0, in standard 10-option, standard 4-option and vision-only subsets."
tags: [multimodal, vision, multiple-choice, multidiscipline]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2409.02813", title: "MMMU-Pro: A More Robust Multi-discipline Multimodal Understanding Benchmark (arXiv:2409.02813)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2409.02813", title: "MMMU-Pro paper, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/MMMU/MMMU_Pro", title: "MMMU/MMMU_Pro — Hugging Face dataset card (Apache-2.0)", trust: 0.9 }
---

## What It Tests
Whether a model **truly sees and reads** an image-based academic question.

- **Filtered** — questions that text-only models can answer were removed.
- **Harder options** — candidate options grew from four to up to ten.
- **Vision-only** — the question text sits inside a screenshot or photo.

**Who built it:** Xiang Yue, Tianyu Zheng, Yuansheng Ni and ten co-authors, including Wenhu Chen and Graham Neubig (arXiv, September 2024).

## How It Scores
**Accuracy** on multiple-choice answers: questions answered correctly / questions asked.

- **Overall score** — the average of the standard 10-option accuracy and the vision-only accuracy.
- **Random baseline** — random choice scores 12.8% (standard, 10 options) and 12.4% (vision-only) in the paper.
- **Prompting** — the paper compares OCR prompts (asking the model to transcribe the text first) and chain of thought (reasoning in writing before answering).

## Format
How the set was built, from the paper:

- **Text-only filter** — four open models answered each MMMU question without its image, ten times each; a question was dropped when at least three of the four answered it correctly in most trials. 1,800 questions were then sampled, 60 per subject.
- **Option augmentation** — GPT-4o proposed extra options, Claude 3.5 filtered them, and human experts reviewed twice; 70 questions without a clear link to their image were removed, leaving **1,730**.
- **Vision-only images** — annotators captured screenshots or photos of each question with varied backgrounds, fonts and font sizes.

**Data:** subsets `standard (10 options)`, `standard (4 options)` and `vision` on [Hugging Face](https://huggingface.co/datasets/MMMU/MMMU_Pro), each with 1,730 rows and an `answer` letter.

## Limitations
- **Inherits MMMU.** Issues in the source set carry over.
- **Setting matters.** Standard and vision-only scores differ; check which one, or the average, a number reports.
- **Public items.** Questions and answers can leak into training data.

## Example
From the public set on the Hugging Face dataset card (Apache-2.0), an Art question reads: "Name the Artist of <image 1>:"

- **Standard setting** — the model gets this text, the painting and up to ten artist names as options.
- **Vision-only setting** — the same question and options appear only as a screenshot or photo, and the model must read them from pixels.
- **Scoring** — the chosen letter is compared with the `answer` column.
