---
id: score-sheet/qwen2-5-72b-instruct
type: score-sheet
slug: qwen2-5-72b-instruct
schema_version: 2.0
title: Qwen2.5 72B Instruct — scores
model: model/qwen2-5-72b-instruct
scores:
- benchmark: benchmark/gpqa-diamond
  value: 0.491477
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
- benchmark: benchmark/gsm8k
  value: 95.8
  scale: percent
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-22'
  self_reported: true
- benchmark: benchmark/humaneval
  value: 86.6
  scale: percent
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-22'
  self_reported: true
- benchmark: benchmark/math
  value: 83.1
  scale: percent
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-22'
  self_reported: true
- benchmark: benchmark/mmlu-pro
  value: 71.1
  scale: percent
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-22'
  self_reported: true
tags:
- llm
- qwen
- alibaba
- dense
- gqa
- instruct
- multilingual
- frontier
updated: '2026-11-22'
sources:
- url: https://qwenlm.github.io/blog/qwen2.5-llm/
  title: https://qwenlm.github.io/blog/qwen2.5-llm/
- url: https://arxiv.org/abs/2412.15115
  title: Qwen2.5 Technical Report (Qwen Team, 2024)
  trust: 0.9
- url: https://huggingface.co/Qwen/Qwen2.5-72B-Instruct
  title: Qwen2.5-72B-Instruct model card (Hugging Face)
  trust: 0.85
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

GSM8K (95.8), HumanEval (86.6), MATH (83.1) and MMLU-Pro (71.1) are the Qwen team's figures for
Qwen2.5-72B-Instruct in the "Qwen2.5-72B-Instruct Performance" table of the Qwen2.5 blog; the
Qwen2.5 technical report (Table 6) gives the same values. The Qwen team publishes MMLU,
ARC-Challenge and TruthfulQA only for the Qwen2.5-72B base model, so none are listed for the
instruct model.
