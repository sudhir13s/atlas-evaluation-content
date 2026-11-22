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
  observed_on: '2026-11-22'
  self_reported: true
  method: 10-shot
- benchmark: benchmark/gsm8k
  value: 82.5
  scale: percent
  source: https://arxiv.org/abs/2404.14219
  observed_on: '2026-11-22'
  self_reported: true
  method: 8-shot; CoT
- benchmark: benchmark/humaneval
  value: 58.5
  scale: percent
  source: https://arxiv.org/abs/2404.14219
  observed_on: '2026-11-22'
  self_reported: true
  method: 0-shot
- benchmark: benchmark/mmlu
  value: 68.8
  scale: percent
  source: https://arxiv.org/abs/2404.14219
  observed_on: '2026-11-22'
  self_reported: true
  method: 5-shot
- benchmark: benchmark/truthfulqa
  value: 65.0
  scale: percent
  source: https://arxiv.org/abs/2404.14219
  observed_on: '2026-11-22'
  self_reported: true
  method: 10-shot; MC2
tags:
- llm
- phi
- microsoft
- dense
- mha
- instruct
- small
- mit
updated: '2026-11-22'
sources:
- url: https://arxiv.org/abs/2404.14219
  title: https://arxiv.org/abs/2404.14219
- url: https://huggingface.co/microsoft/Phi-3-mini-4k-instruct
  title: Phi-3-mini-4k-instruct model card (Hugging Face)
  trust: 0.85
---

## Notes

Phi-3-mini has 3.8B parameters. All five values are Microsoft's figures for phi-3-mini in the
academic-benchmark table of the Phi-3 technical report: MMLU 68.8, GSM8K 82.5, HumanEval 58.5,
ARC-Challenge 84.9 and TruthfulQA 65.0. The report states that every model in that table was run
through the same Microsoft-internal evaluation pipeline at temperature 0, with the number of shots
listed per benchmark.
