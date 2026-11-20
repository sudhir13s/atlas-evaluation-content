---
id: score-sheet/deepseek-v3
type: score-sheet
slug: deepseek-v3
schema_version: 2.0
title: DeepSeek-V3 — scores
model: model/deepseek-v3
scores:
- benchmark: benchmark/arc-challenge
  value: 90.0
  scale: percent
  source: https://arxiv.org/abs/2412.19437
  observed_on: '2026-07-17'
  self_reported: true
- benchmark: benchmark/gpqa-diamond
  value: 0.565341
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
- benchmark: benchmark/gsm8k
  value: 0.893
  scale: fraction
  source: https://huggingface.co/deepseek-ai/DeepSeek-V3
  observed_on: '2026-11-19'
  self_reported: true
  method: vendor-reported
- benchmark: benchmark/humaneval
  value: 82.6
  scale: percent
  source: https://arxiv.org/abs/2412.19437
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported pass@1 / HumanEval-Mul (DeepSeek-V3 technical report).
- benchmark: benchmark/mmlu
  value: 0.885
  scale: fraction
  source: https://huggingface.co/deepseek-ai/DeepSeek-V3
  observed_on: '2026-11-19'
  self_reported: true
  method: EM
- benchmark: benchmark/mmlu-pro
  value: 0.759
  scale: fraction
  source: https://huggingface.co/deepseek-ai/DeepSeek-V3
  observed_on: '2026-11-19'
  self_reported: true
  method: EM
- benchmark: benchmark/truthfulqa
  value: 68.0
  scale: percent
  source: https://arxiv.org/abs/2412.19437
  observed_on: '2026-07-17'
  self_reported: true
tags:
- llm
- deepseek
- moe
- sparse
- mla
- frontier
- fp8
updated: '2026-11-20'
sources:
- url: https://huggingface.co/deepseek-ai/DeepSeek-V3
  title: https://huggingface.co/deepseek-ai/DeepSeek-V3
- url: https://arxiv.org/abs/2412.19437
  title: DeepSeek-V3 Technical Report (DeepSeek-AI, 2024)
  trust: 0.9
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

DeepSeek-V3 is a 671B-total Mixture-of-Experts model with ~37B active parameters per token. MMLU
(88.5), HumanEval (~82.6 pass@1), and GSM8K (89.3, CoT) are DeepSeek's reported figures. The report
leads with MMLU-Pro, GPQA, MATH-500, and code benchmarks rather than TruthfulQA/ARC-Challenge, so
TruthfulQA (~68) and ARC-Challenge (~90) here are reference approximations and should be verified
before citation.
