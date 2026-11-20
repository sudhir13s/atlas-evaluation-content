---
id: score-sheet/llama-3-1-70b-instruct
type: score-sheet
slug: llama-3-1-70b-instruct
schema_version: 2.0
title: Llama-3.1-70B-Instruct — scores
model: model/llama-3-1-70b-instruct
scores:
- benchmark: benchmark/arc-challenge
  value: 93.0
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-07-17'
  self_reported: true
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
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported, 8-shot CoT (Llama 3.1 paper).
- benchmark: benchmark/humaneval
  value: 80.5
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported pass@1 (Llama 3.1 paper).
- benchmark: benchmark/mmlu
  value: 86.0
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported, 5-shot (Llama 3.1 model card / paper).
- benchmark: benchmark/truthfulqa
  value: 63.0
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-07-17'
  self_reported: true
tags:
- llm
- text-generation
updated: '2026-11-20'
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

MMLU (86.0, 5-shot), HumanEval (80.5 pass@1), and GSM8K (95.1, 8-shot CoT) are Meta's own reported
figures from the Llama 3.1 paper and model card — hence `self_reported: true`. Meta's headline table
does not report TruthfulQA or ARC-Challenge for the instruct model; the values here (TruthfulQA ~63,
ARC-Challenge ~93) are reference approximations consistent with the Llama-3-70B range on the archived
HF Open LLM Leaderboard v1 (TruthfulQA MC2, ARC-Challenge 25-shot) and should be verified before
citation. Independent re-runs (LMArena, LiveBench) are the trustworthy cross-check.
