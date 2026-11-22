---
id: score-sheet/llama-3-1-70b-instruct
type: score-sheet
slug: llama-3-1-70b-instruct
schema_version: 2.0
title: Llama-3.1-70B-Instruct — scores
model: model/llama-3-1-70b-instruct
scores:
- benchmark: benchmark/arc-challenge
  value: 94.8
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-22'
  self_reported: true
  method: 0-shot
- benchmark: benchmark/gpqa-diamond
  value: 0.441919
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
- benchmark: benchmark/gsm8k
  value: 95.1
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-22'
  self_reported: true
  method: 8-shot, CoT
- benchmark: benchmark/humaneval
  value: 80.5
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-22'
  self_reported: true
  method: 0-shot
- benchmark: benchmark/mmlu
  value: 83.6
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-22'
  self_reported: true
  method: 5-shot
tags:
- llm
- text-generation
updated: '2026-11-22'
sources:
- url: https://arxiv.org/abs/2407.21783
  title: The Llama 3 Herd of Models (Meta, 2024)
  trust: 0.9
- url: https://huggingface.co/meta-llama/Llama-3.1-70B-Instruct
  title: Llama-3.1-70B-Instruct model card (Hugging Face)
  trust: 0.85
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

MMLU (83.6, 5-shot), HumanEval (80.5, 0-shot), GSM8K (95.1, 8-shot CoT) and ARC-Challenge (94.8,
0-shot) are Meta's figures for the fine-tuned 70B model in Table 2 of the Llama 3 paper, which
states that all its results are for the Llama 3.1 models; the instruction-tuned table of the
Llama 3.1 model card gives the same values. The same paper table reports MMLU as 86.0 under 0-shot
chain-of-thought prompting. Meta does not report TruthfulQA for this model.
