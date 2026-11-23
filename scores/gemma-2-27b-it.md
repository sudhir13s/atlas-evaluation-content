---
id: score-sheet/gemma-2-27b-it
type: score-sheet
slug: gemma-2-27b-it
schema_version: 4.0
title: gemma-2-27b-it — scores
model: model/gemma-2-27b-it
scores:
- benchmark: benchmark/gpqa-diamond
  value: 0.364899
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
- benchmark: benchmark/mmlu
  value: 76.2
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-11-22'
  self_reported: true
  method: few-shot
- benchmark: benchmark/truthfulqa
  value: 51.6
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-11-22'
  self_reported: true
  method: MC2Acc
tags:
- llm
- text-generation
updated: '2026-11-22'
sources:
- url: https://arxiv.org/abs/2408.00118
  title: 'Gemma 2: Improving Open Language Models at a Practical Size (Google DeepMind, 2024)'
  trust: 0.9
- url: https://huggingface.co/google/gemma-2-27b-it
  title: gemma-2-27b-it model card (Hugging Face)
  trust: 0.85
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

MMLU (76.2) is Google's figure for the instruction-tuned 27B model in Table 17 of the Gemma 2
technical report, which compares pre-trained (75.2) and instruction-tuned models on few-shot
benchmarks. TruthfulQA (51.60, MC2 accuracy) is from the report's safety academic benchmark table
(Table 18) for Gemma 2 IT 27B. The report gives ARC-Challenge, GSM8K and HumanEval only for the
pre-trained model, so none are listed here.
