---
id: score-sheet/phi-3-mini-4k-instruct
type: score-sheet
slug: phi-3-mini-4k-instruct
schema_version: 2.0
title: Phi-3 Mini 4K Instruct — scores
model: model/phi-3-mini-4k-instruct
scores:
- benchmark: benchmark/arc-challenge
  value: 84.9
  scale: percent
  source: https://arxiv.org/abs/2404.14219
  observed_on: '2026-07-17'
  self_reported: true
- benchmark: benchmark/gsm8k
  value: 0.825
  scale: fraction
  source: https://arxiv.org/abs/2404.14219
  observed_on: '2026-11-19'
  self_reported: true
  method: 8-shot CoT
- benchmark: benchmark/humaneval
  value: 0.585
  scale: fraction
  source: https://arxiv.org/abs/2404.14219
  observed_on: '2026-11-19'
  self_reported: true
  method: 0-shot
- benchmark: benchmark/mmlu
  value: 0.688
  scale: fraction
  source: https://arxiv.org/abs/2404.14219
  observed_on: '2026-11-19'
  self_reported: true
  method: 5-shot
- benchmark: benchmark/truthfulqa
  value: 65.0
  scale: percent
  source: https://arxiv.org/abs/2404.14219
  observed_on: '2026-07-17'
  self_reported: true
tags:
- llm
- phi
- microsoft
- dense
- mha
- instruct
- small
- mit
updated: '2026-11-19'
sources:
- url: https://arxiv.org/abs/2404.14219
  title: https://arxiv.org/abs/2404.14219
- url: https://huggingface.co/microsoft/Phi-3-mini-4k-instruct
  title: Phi-3-mini-4k-instruct model card (Hugging Face)
  trust: 0.85
---

## Notes

Phi-3-mini (3.8B) is the strongest small model in this row set, thanks to its heavily curated training
data. MMLU (68.8), HumanEval (58.5 pass@1), and GSM8K (82.5) are Microsoft's reported figures from the
Phi-3 technical report, which does tabulate TruthfulQA (~65) and ARC-Challenge (~84.9) — these two are
closer to vendor-reported here than in the other rows, but still verify against the report before
citation. Phi models are also the textbook caution on contamination given their synthetic-data
training recipe.
