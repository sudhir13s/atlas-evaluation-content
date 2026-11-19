---
id: score-sheet/qwen2-5-7b-instruct
type: score-sheet
slug: qwen2-5-7b-instruct
schema_version: 2.0
title: Qwen2.5 7B Instruct — scores
model: model/qwen2-5-7b-instruct
scores:
- benchmark: benchmark/arc-challenge
  value: 63.0
  scale: percent
  source: https://arxiv.org/abs/2412.15115
  observed_on: '2026-07-17'
  self_reported: true
- benchmark: benchmark/gsm8k
  value: 0.916
  scale: fraction
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-19'
  self_reported: true
  method: vendor-reported
- benchmark: benchmark/gsm8k
  value: 85.4
  scale: percent
  source: https://arxiv.org/abs/2412.15115
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported (Qwen2.5 technical report).
- benchmark: benchmark/humaneval
  value: 0.848
  scale: fraction
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-19'
  self_reported: true
  method: 0-shot
- benchmark: benchmark/math
  value: 0.755
  scale: fraction
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-19'
  self_reported: true
  method: vendor-reported
- benchmark: benchmark/mmlu
  value: 74.2
  scale: percent
  source: https://arxiv.org/abs/2412.15115
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported (Qwen2.5 technical report).
- benchmark: benchmark/mmlu-pro
  value: 0.563
  scale: fraction
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-19'
  self_reported: true
  method: 5-shot
- benchmark: benchmark/truthfulqa
  value: 64.0
  scale: percent
  source: https://arxiv.org/abs/2412.15115
  observed_on: '2026-07-17'
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
updated: '2026-11-19'
sources:
- url: https://qwenlm.github.io/blog/qwen2.5-llm/
  title: https://qwenlm.github.io/blog/qwen2.5-llm/
- url: https://arxiv.org/abs/2412.15115
  title: Qwen2.5 Technical Report (Qwen Team, 2024)
  trust: 0.9
- url: https://huggingface.co/Qwen/Qwen2.5-7B-Instruct
  title: Qwen2.5-7B-Instruct model card (Hugging Face)
  trust: 0.85
---

## Notes

MMLU (74.2), HumanEval (84.8 pass@1), and GSM8K (85.4) are the Qwen team's reported figures.
TruthfulQA (~64) and ARC-Challenge (~63) are reference approximations not headlined in the Qwen2.5
report and should be verified before citation.
