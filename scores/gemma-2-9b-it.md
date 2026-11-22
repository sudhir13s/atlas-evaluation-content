---
id: score-sheet/gemma-2-9b-it
type: score-sheet
slug: gemma-2-9b-it
schema_version: 2.0
title: Gemma 2 9B IT — scores
model: model/gemma-2-9b-it
scores:
- benchmark: benchmark/gpqa-diamond
  value: 0.274621
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
- benchmark: benchmark/mmlu
  value: 72.3
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-11-22'
  self_reported: true
  method: few-shot
- benchmark: benchmark/truthfulqa
  value: 50.27
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-11-22'
  self_reported: true
  method: MC2Acc
tags:
- llm
- gemma
- google
- dense
- gqa
- instruct
- sliding-window
updated: '2026-11-22'
sources:
- url: https://arxiv.org/abs/2408.00118
  title: https://arxiv.org/abs/2408.00118
- url: https://huggingface.co/google/gemma-2-9b-it
  title: gemma-2-9b-it model card (Hugging Face)
  trust: 0.85
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

MMLU (72.3) is Google's figure for the instruction-tuned 9B model in Table 17 of the Gemma 2
technical report, which compares pre-trained (71.3) and instruction-tuned models on few-shot
benchmarks. TruthfulQA (50.27, MC2 accuracy) is from the report's safety academic benchmark table
(Table 18) for Gemma 2 IT 9B. The report gives ARC-Challenge, GSM8K, HumanEval and MATH only for the
pre-trained model, so none are listed here.
