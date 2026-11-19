---
id: score-sheet/mistral-7b-instruct-v0-3
type: score-sheet
slug: mistral-7b-instruct-v0-3
schema_version: 2.0
title: Mistral 7B Instruct v0.3 — scores
model: model/mistral-7b-instruct-v0-3
scores:
- benchmark: benchmark/arc-challenge
  value: 63.0
  scale: percent
  source: https://arxiv.org/abs/2310.06825
  observed_on: '2026-07-17'
  self_reported: true
- benchmark: benchmark/gsm8k
  value: 52.0
  scale: percent
  source: https://arxiv.org/abs/2310.06825
  observed_on: '2026-07-17'
  self_reported: true
  note: Reference figure (Mistral 7B paper, 8-shot).
- benchmark: benchmark/humaneval
  value: 40.0
  scale: percent
  source: https://arxiv.org/abs/2310.06825
  observed_on: '2026-07-17'
  self_reported: true
  note: Reference pass@1 from the Mistral 7B paper.
- benchmark: benchmark/mmlu
  value: 62.5
  scale: percent
  source: https://arxiv.org/abs/2310.06825
  observed_on: '2026-07-17'
  self_reported: true
  note: Reference figure for the Mistral 7B base (Mistral 7B paper, 5-shot); v0.3 instruct is close.
- benchmark: benchmark/truthfulqa
  value: 56.0
  scale: percent
  source: https://arxiv.org/abs/2310.06825
  observed_on: '2026-07-17'
  self_reported: true
tags:
- llm
- mistral
- dense
- gqa
- instruct
- apache-2-0
updated: '2026-11-19'
sources:
- url: https://arxiv.org/abs/2310.06825
  title: Mistral 7B (Jiang et al., 2023)
  trust: 0.9
- url: https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.3
  title: Mistral-7B-Instruct-v0.3 model card (Hugging Face)
  trust: 0.8
---

## Notes

Mistral did not publish a full per-benchmark table for the v0.3 instruct point release; the MMLU
(~62.5), HumanEval (~40), and GSM8K (~52) figures here are the Mistral 7B paper's base-model numbers,
which the instruct variant tracks closely — treat them as reference approximations, not exact v0.3
instruct measurements. TruthfulQA (~56) and ARC-Challenge (~63) are HF Open LLM v1-archive-range
approximations and should be verified before citation. This row is the clearest example of why the
`self_reported` flag and independent re-runs matter.
