---
id: benchmark/lvbench
type: benchmark
slug: lvbench
schema_version: 4.0
title: LVBench
name: LVBench
category: Multimodal
description: 'An extreme long-video understanding benchmark: 1,549 multiple-choice questions on 103 videos that average about 68 minutes, covering six skills.'
what_is_it: LVBench is a benchmark for understanding and extracting information from long videos, introduced in a 2024 paper by Weihan Wang and colleagues at Zhipu AI and Tsinghua University. It has 103 videos totalling 117 hours and 1,549 question-answer pairs, with videos averaging 4,101 seconds. Videos come from public sources, including TV series, sports broadcasts and everyday surveillance footage; the project site counts 6 major categories and 21 subcategories. Annotators combined manual effort with model assistance, and the questions cover six core skills.
why_people_use_it: Earlier video benchmarks focus on short clips, but applications such as embodied intelligence, movie review and live sports commentary need hours-long understanding. LVBench tests long-term memory and extended comprehension, and the project keeps its own leaderboard.
method: 'Models answer questions about a whole long video. Questions average 24 per hour of video and are spread across the full duration. Each question has four answer choices: one correct and three distractors that share the format and length of the correct one. The model picks one choice, which is compared with the key.'
metric: Accuracy = questions answered correctly / questions asked, in percent, shown on the project leaderboard as an Overall score plus a score for each of the six skills.
format: Multiple choice with four options per question, over videos up to two hours long. The six skills are temporal grounding (TG), entity recognition (ER), event understanding (EU), key information retrieval (KIR), reasoning (Rea) and summarization (Sum). The leaderboard lists how many frames each model was given.
limitations: 'The dataset is licensed CC-BY-NC-SA-4.0 for academic research only, and the authors do not own the copyright of the video files. Videos are not shipped: users download them from YouTube with the identifiers in the dataset, so availability depends on that site. The paper finds accuracy changes with the number of input frames, so frame budget changes the score, and vendors sample frames differently. The paper says videos average about four times longer than existing benchmarks, while the dataset card says five times longer than the longest existing datasets; the two pages disagree.'
citation: https://arxiv.org/abs/2406.08035
openness: open
openness_note: The questions, answers and YouTube video identifiers are public on Hugging Face under CC-BY-NC-SA-4.0 for academic use, and users fetch the videos themselves with the provided download script.
tags:
- multimodal
- video
- long-context
- long-video
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2406.08035
  title: 'LVBench: An Extreme Long Video Understanding Benchmark (arXiv:2406.08035)'
  trust: 0.95
- url: https://arxiv.org/html/2406.08035
  title: LVBench paper, full text (arXiv HTML)
  trust: 0.95
- url: https://lvbench.github.io
  title: LVBench — project site and leaderboard
  trust: 0.95
- url: https://huggingface.co/datasets/zai-org/LVBench
  title: zai-org/LVBench — Hugging Face dataset card
  trust: 0.9
---

## What It Tests

Whether a multimodal model can **understand and recall information across hours of video**.

- **Scale** — 103 videos (117 hours), 1,549 question-answer pairs, average video length 4,101 seconds.
- **Six skills** — temporal grounding, entity recognition, event understanding, key information retrieval, reasoning and summarization.
- **Varied sources** — TV series, sports broadcasts and surveillance footage; the paper names six video categories: sports, documentary, event record, lifestyle, TV show and cartoon.

**Who built it:** Weihan Wang and colleagues at **Zhipu AI and Tsinghua University** (arXiv, June 2024). Code is linked from the project site at [THUDM/LVBench](https://github.com/THUDM/LVBench).

## How It Scores

**Accuracy** on four-option questions: questions answered correctly / questions asked, reported overall and per skill. Random guessing scores 25%.

- **Questions** — about 24 per hour of video, spread across the whole timeline.
- **Distractors** — three wrong choices per question, matched in format and length to the right one.
- **Leaderboard** — Overall plus ER, EU, KIR, TG, Rea and Sum columns, with the frame count each model used.

## Format

Each question has one correct answer and three distractors.

- **Skills overlap** — the paper notes that many questions also need entity recognition and event understanding, which is why those two skills make up the largest share.
- **Videos** — downloaded from YouTube with identifiers in `video_info.meta.jsonl`, using the provided `download.sh` script or video2dataset.
- **Frames** — the paper tests 0, 1, 4, 8 and 50 frames and 1 frame per second.

## Limitations

- **Licence.** CC-BY-NC-SA-4.0, academic research only; the authors do not own the video copyright.
- **Video availability.** Videos live on YouTube and are not redistributed.
- **Frame budget.** Accuracy changes with the number of input frames, and vendors sample differently.
- **Conflicting claim.** The paper says videos are about four times longer than existing benchmarks; the dataset card says five times.

## Example

From the paper, the question style for three skills:

- **Temporal grounding** — "What happened at 29:30?"
- **Key information retrieval** — "What revenue growth did the firm report at the conference?"
- **Reasoning** — "Why did the experiment fail?"

Each real question comes with four answer choices, and the model must pick the one that matches the video. These are the paper's style examples, not full dataset items with options.
