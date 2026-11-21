---
id: benchmark/lvbench
type: benchmark
slug: lvbench
schema_version: 1.0
title: "LVBench"
name: LVBench
category: Multimodal
description: "An extreme long-video understanding benchmark: 1,549 multiple-choice questions on 103 videos that average about 68 minutes, covering six skills."
what_is_it: "LVBench is a benchmark for understanding and extracting information from long videos, introduced in a 2024 paper by Weihan Wang and colleagues. It has 103 videos and 1,549 question-answer pairs, with videos averaging 4,101 seconds. Videos come from public sources, including TV series, sports broadcasts and everyday surveillance footage, in 6 major categories and 21 subcategories. Annotators combined manual effort with model assistance, and the questions cover six core skills."
why_people_use_it: "Earlier video benchmarks focus on clips under a minute, but applications such as embodied intelligence, movie review and live sports commentary need hours-long understanding. LVBench tests long-term memory and extended comprehension. Google DeepMind lists it in the multimodal group of its Gemini 4 Neon table, and the project keeps its own leaderboard."
method: "Models answer questions about a whole long video. Questions average 24 per hour of video and are spread across the full duration; every video must include questions on all six skills. Annotators write four answer choices per question, one correct and three distractors of similar format and length. The repository ships an accuracy script (test_acc.py)."
metric: "Accuracy in percent, shown on the project leaderboard as an Overall score plus a score for each skill."
format: "Multiple choice with four options per question, over videos up to two hours long. The six skills are temporal grounding (TG), entity recognition (ER), event understanding (EU), key information retrieval (KIR), reasoning (Rea) and summarization (Sum). The paper's frame-density test compares 0 to 50 frames with 1 frame per second."
limitations: "The dataset is licensed CC-BY-NC-SA-4.0 for academic research only, and the authors do not own the copyright of the video files. Videos are not shipped: users download them from YouTube with the identifiers in the dataset, so availability depends on that site. The paper finds accuracy rises with the number of input frames, so frame budget changes the score. Reports differ in setup: Google's evaluation methodology for Gemini 4 Neon runs it without tools, vision only, at 1 frame per second for Gemini, 500 frames for GPT-6.2 Sol and 600 for Claude Opus 5.5 because of API limits, so such numbers are not comparable. The paper says videos average about four times longer than existing benchmarks, while the dataset card says five times; the two pages disagree. Entity recognition and event understanding make up the largest share of questions."
citation: "https://arxiv.org/abs/2406.08035"
tags: [multimodal, video, long-context, long-video]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2406.08035", title: "LVBench: An Extreme Long Video Understanding Benchmark (arXiv:2406.08035)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2406.08035", title: "LVBench paper, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://lvbench.github.io", title: "LVBench — project site and leaderboard", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/zai-org/LVBench", title: "zai-org/LVBench — Hugging Face dataset card", trust: 0.9 }
  - { url: "https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf", title: "Gemini 4 Neon model evaluation (Google DeepMind)", trust: 0.7 }
---

## What It Tests
Whether a multimodal model can **understand and recall information across hours of video**.

- **Scale** — 103 videos, 1,549 question-answer pairs, average video length 4,101 seconds.
- **Six skills** — temporal grounding, entity recognition, event understanding, key information retrieval, reasoning and summarization.
- **Varied sources** — TV series, sports broadcasts and surveillance footage in 6 categories and 21 subcategories.

## How It Scores
**Accuracy** on four-option questions, reported overall and per skill.

- **Questions** — about 24 per hour of video, spread across the whole timeline.
- **Distractors** — three wrong choices per question, matched in format and length to the right one.
- **Script** — the repository's `test_acc.py` computes accuracy from an answer file.

## Format
Each question has one correct answer and three distractors. The skills combine: the paper notes that a question about why a player pushed another still needs entity recognition and event understanding, which is why those two skills dominate. Videos are downloaded from YouTube with identifiers in `video_info.meta.jsonl`.

## Limitations
- **Licence.** CC-BY-NC-SA-4.0, academic research only; the authors do not own the video copyright.
- **Video availability.** Videos live on YouTube and are not redistributed.
- **Frame budget.** Accuracy rises with more input frames, and vendors sample differently.
- **Conflicting claim.** The paper says videos are about four times longer than existing benchmarks; the dataset card says five times.

## Example
The paper gives question styles for each skill. A **temporal grounding** question asks 'What happened at 29:30?'. A **key information retrieval** question asks 'What revenue growth did the firm report at the conference?'. A **reasoning** question asks 'Why did the experiment fail?'. Each comes with four answer choices, and the model must pick the one that matches the video.
