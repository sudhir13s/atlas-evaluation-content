---
id: score-sheet/gemma-2-27b-it
type: score-sheet
slug: gemma-2-27b-it
schema_version: 2.0
title: gemma-2-27b-it — scores
model: model/gemma-2-27b-it
scores:
- benchmark: benchmark/arc-challenge
  value: 71.4
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-07-17'
  self_reported: true
- benchmark: benchmark/gpqa-diamond
  value: 0.364899
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
- benchmark: benchmark/gsm8k
  value: 74.0
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported, 5-shot / 8-shot (Gemma 2 technical report).
- benchmark: benchmark/humaneval
  value: 51.8
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported pass@1 (Gemma 2 technical report).
- benchmark: benchmark/mmlu
  value: 75.2
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported, 5-shot (Gemma 2 technical report, base model).
- benchmark: benchmark/truthfulqa
  value: 51.0
  scale: percent
  source: https://arxiv.org/abs/2408.00118
  observed_on: '2026-07-17'
  self_reported: true
tags:
- llm
- text-generation
updated: '2026-11-20'
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

MMLU (75.2), HumanEval (51.8 pass@1), and GSM8K (74.0) come from Google's Gemma 2 technical report
(several are reported on the base model, which the report tabulates alongside the IT variant).
TruthfulQA (~51) and ARC-Challenge (~71.4) are the report's/HF-archive reference figures and should be
verified before citation. Gemma 2 is notable for punching above its size on human-preference boards
(LMArena) relative to these static numbers.
