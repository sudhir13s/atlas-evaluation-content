---
id: score-sheet/mixtral-8x7b-instruct-v0-1
type: score-sheet
slug: mixtral-8x7b-instruct-v0-1
schema_version: 2.0
title: Mixtral 8x7B Instruct v0.1 — scores
model: model/mixtral-8x7b-instruct-v0-1
scores:
- benchmark: benchmark/arc-challenge
  value: 70.0
  scale: percent
  source: https://arxiv.org/abs/2401.04088
  observed_on: '2026-07-17'
  self_reported: true
- benchmark: benchmark/gsm8k
  value: 0.584
  scale: fraction
  source: https://arxiv.org/abs/2401.04088
  observed_on: '2026-11-19'
  self_reported: true
  method: 5-shot maj@1 (Mixtral base, arXiv 2401.04088)
- benchmark: benchmark/gsm8k
  value: 60.4
  scale: percent
  source: https://arxiv.org/abs/2401.04088
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported, 8-shot (Mixtral of Experts paper).
- benchmark: benchmark/humaneval
  value: 0.402
  scale: fraction
  source: https://arxiv.org/abs/2401.04088
  observed_on: '2026-11-19'
  self_reported: true
  method: pass@1 (Mixtral base, arXiv 2401.04088)
- benchmark: benchmark/mmlu
  value: 0.706
  scale: fraction
  source: https://arxiv.org/abs/2401.04088
  observed_on: '2026-11-19'
  self_reported: true
  method: 5-shot (Mixtral base, arXiv 2401.04088)
- benchmark: benchmark/truthfulqa
  value: 57.0
  scale: percent
  source: https://arxiv.org/abs/2401.04088
  observed_on: '2026-07-17'
  self_reported: true
tags:
- llm
- mistral
- moe
- sparse
- gqa
- instruct
- apache-2-0
updated: '2026-11-19'
sources:
- url: https://arxiv.org/abs/2401.04088
  title: https://arxiv.org/abs/2401.04088
- url: https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1
  title: Mixtral-8x7B-Instruct-v0.1 model card (Hugging Face)
  trust: 0.8
---

## Notes

Mixtral 8x7B is a sparse Mixture-of-Experts model: 46.7B total parameters but only ~12.9B active per
token. MMLU (70.6, 5-shot), HumanEval (~40.2 pass@1), and GSM8K (60.4, 8-shot) are from the Mixtral of
Experts paper (base-model table; the instruct variant tracks closely). TruthfulQA (~57) and
ARC-Challenge (~70) are HF-archive-range approximations and should be verified before citation.
