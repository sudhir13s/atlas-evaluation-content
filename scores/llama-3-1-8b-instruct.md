---
id: score-sheet/llama-3-1-8b-instruct
type: score-sheet
slug: llama-3-1-8b-instruct
schema_version: 2.0
title: Llama 3.1 8B Instruct — scores
model: model/llama-3-1-8b-instruct
scores:
- benchmark: benchmark/arc-challenge
  value: 83.0
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-07-17'
  self_reported: true
- benchmark: benchmark/gsm8k
  value: 0.845
  scale: fraction
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-19'
  self_reported: true
  method: 8-shot CoT
- benchmark: benchmark/humaneval
  value: 0.726
  scale: fraction
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-19'
  self_reported: true
  method: 0-shot
- benchmark: benchmark/mmlu
  value: 0.694
  scale: fraction
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-11-19'
  self_reported: true
  method: 5-shot
- benchmark: benchmark/mmlu
  value: 68.4
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported, 5-shot (Llama 3.1 model card / paper).
- benchmark: benchmark/truthfulqa
  value: 54.0
  scale: percent
  source: https://arxiv.org/abs/2407.21783
  observed_on: '2026-07-17'
  self_reported: true
tags:
- llm
- meta
- llama
- dense
- gqa
- instruct
updated: '2026-11-19'
sources:
- url: https://arxiv.org/abs/2407.21783
  title: https://arxiv.org/abs/2407.21783
- url: https://huggingface.co/meta-llama/Llama-3.1-8B-Instruct
  title: Llama-3.1-8B-Instruct model card (Hugging Face)
  trust: 0.85
---

## Notes

MMLU (68.4, 5-shot), HumanEval (72.6 pass@1), and GSM8K (84.5, 8-shot CoT) are Meta's reported
figures. TruthfulQA (~54) and ARC-Challenge (~83) are not in Meta's headline instruct table; they are
reference approximations aligned with the Llama-3-8B range on the archived HF Open LLM Leaderboard v1
and should be verified before citation.
