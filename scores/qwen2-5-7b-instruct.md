---
id: score-sheet/qwen2-5-7b-instruct
type: score-sheet
slug: qwen2-5-7b-instruct
schema_version: 2.0
title: Qwen2.5 7B Instruct — scores
model: model/qwen2-5-7b-instruct
scores:
- benchmark: benchmark/gpqa-diamond
  value: 0.354798
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2026-08-30'
  self_reported: false
- benchmark: benchmark/gsm8k
  value: 91.6
  scale: percent
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-22'
  self_reported: true
- benchmark: benchmark/humaneval
  value: 84.8
  scale: percent
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-22'
  self_reported: true
- benchmark: benchmark/math
  value: 75.5
  scale: percent
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-22'
  self_reported: true
- benchmark: benchmark/mmlu-pro
  value: 56.3
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
- apache-2-0
updated: '2026-11-22'
sources:
- url: https://qwenlm.github.io/blog/qwen2.5-llm/
  title: https://qwenlm.github.io/blog/qwen2.5-llm/
- url: https://arxiv.org/abs/2412.15115
  title: Qwen2.5 Technical Report (Qwen Team, 2024)
  trust: 0.9
- url: https://huggingface.co/Qwen/Qwen2.5-7B-Instruct
  title: Qwen2.5-7B-Instruct model card (Hugging Face)
  trust: 0.85
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

GSM8K (91.6), HumanEval (84.8), MATH (75.5) and MMLU-Pro (56.3) are the Qwen team's figures for
Qwen2.5-7B-Instruct in the "Qwen2.5-7B-Instruct Performance" table of the Qwen2.5 blog; the Qwen2.5
technical report (Table 8) gives the same values. The Qwen team publishes MMLU, ARC-Challenge and
TruthfulQA only for the Qwen2.5-7B base model, so none are listed for the instruct model.
