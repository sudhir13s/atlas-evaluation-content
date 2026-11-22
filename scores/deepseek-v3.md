---
id: score-sheet/deepseek-v3
type: score-sheet
slug: deepseek-v3
schema_version: 2.0
title: DeepSeek-V3 — scores
model: model/deepseek-v3
scores:
- benchmark: benchmark/gpqa-diamond
  value: 0.565341
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
- benchmark: benchmark/mmlu
  value: 88.5
  scale: percent
  source: https://huggingface.co/deepseek-ai/DeepSeek-V3
  observed_on: '2026-11-22'
  self_reported: true
  method: EM
- benchmark: benchmark/mmlu-pro
  value: 75.9
  scale: percent
  source: https://huggingface.co/deepseek-ai/DeepSeek-V3
  observed_on: '2026-11-22'
  self_reported: true
  method: EM
tags:
- llm
- deepseek
- moe
- sparse
- mla
- frontier
- fp8
updated: '2026-11-22'
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

DeepSeek-V3 is a 671B-total Mixture-of-Experts model with 37B activated parameters. MMLU (88.5) and
MMLU-Pro (75.9) are DeepSeek's chat-model figures from the model card, matching Table 6 of the
DeepSeek-V3 technical report. DeepSeek reports ARC-Challenge, GSM8K and HumanEval only for
DeepSeek-V3-Base, reports code for the chat model as HumanEval-Mul rather than HumanEval, and does
not report TruthfulQA, so none of these are listed here.
