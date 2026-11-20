---
id: score-sheet/gemma-2-9b-it
type: score-sheet
slug: gemma-2-9b-it
schema_version: 2.0
title: Gemma 2 9B IT — scores
model: model/gemma-2-9b-it
scores:
- benchmark: benchmark/arc-challenge
  value: 68.4
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-07-17'
  self_reported: true
- benchmark: benchmark/gpqa-diamond
  value: 0.274621
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
- benchmark: benchmark/gsm8k
  value: 0.686
  scale: fraction
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-11-19'
  self_reported: true
  method: 5-shot (Gemma 2 9B pretrained-base report)
- benchmark: benchmark/humaneval
  value: 0.402
  scale: fraction
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-11-19'
  self_reported: true
  method: pass@1 (Gemma 2 9B pretrained-base report)
- benchmark: benchmark/math
  value: 0.366
  scale: fraction
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-11-19'
  self_reported: true
  method: 4-shot (Gemma 2 9B pretrained-base report)
- benchmark: benchmark/mmlu
  value: 0.713
  scale: fraction
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-11-19'
  self_reported: true
  method: 5-shot (Gemma 2 9B pretrained-base report)
- benchmark: benchmark/truthfulqa
  value: 45.3
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-07-17'
  self_reported: true
tags:
- llm
- gemma
- google
- dense
- gqa
- instruct
- sliding-window
updated: '2026-11-20'
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

MMLU (71.3), HumanEval (40.2 pass@1), and GSM8K (68.6) are from the Gemma 2 technical report.
TruthfulQA (~45.3) and ARC-Challenge (~68.4) are reference figures from the report/HF archive and
should be verified before citation.
