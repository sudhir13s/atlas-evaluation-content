---
id: score-sheet/llama-3-1-8b-instruct
type: score-sheet
slug: llama-3-1-8b-instruct
schema_version: 2.0
title: Llama 3.1 8B Instruct — scores
model: model/llama-3-1-8b-instruct
scores:
- benchmark: benchmark/arc-challenge
  value: 83.4
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-22'
  self_reported: true
  method: 0-shot
- benchmark: benchmark/gpqa-diamond
  value: 0.269571
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2026-08-27'
  self_reported: false
- benchmark: benchmark/gsm8k
  value: 84.5
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-22'
  self_reported: true
  method: 8-shot, CoT
- benchmark: benchmark/humaneval
  value: 72.6
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-22'
  self_reported: true
  method: 0-shot
- benchmark: benchmark/mmlu
  value: 69.4
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-22'
  self_reported: true
  method: 5-shot
tags:
- llm
- meta
- llama
- dense
- gqa
- instruct
updated: '2026-11-22'
sources:
- url: https://arxiv.org/abs/2407.21783
  title: https://arxiv.org/abs/2407.21783
- url: https://huggingface.co/meta-llama/Llama-3.1-8B-Instruct
  title: Llama-3.1-8B-Instruct model card (Hugging Face)
  trust: 0.85
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

MMLU (69.4, 5-shot), HumanEval (72.6, 0-shot), GSM8K (84.5, 8-shot CoT) and ARC-Challenge (83.4,
0-shot) are Meta's figures for the fine-tuned 8B model in Table 2 of the Llama 3 paper, which states
that all its results are for the Llama 3.1 models; the instruction-tuned table of the Llama 3.1
model card gives the same values. Meta does not report TruthfulQA for this model.
