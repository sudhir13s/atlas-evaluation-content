---
id: score-sheet/mixtral-8x7b-instruct-v0-1
type: score-sheet
slug: mixtral-8x7b-instruct-v0-1
schema_version: 2.0
title: Mixtral 8x7B Instruct v0.1 — scores
model: model/mixtral-8x7b-instruct-v0-1
scores:
- benchmark: benchmark/gpqa-diamond
  value: 0.305871
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
tags:
- llm
- mistral
- moe
- sparse
- gqa
- instruct
- apache-2-0
updated: '2026-11-22'
sources:
- url: https://arxiv.org/abs/2401.04088
  title: https://arxiv.org/abs/2401.04088
- url: https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1
  title: Mixtral-8x7B-Instruct-v0.1 model card (Hugging Face)
  trust: 0.8
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

Mixtral 8x7B is a sparse Mixture-of-Experts model in which each token uses 13B active parameters
out of 47B. The Mixtral of Experts paper reports academic benchmarks (MMLU, ARC-Challenge, GSM8K,
HumanEval) only for the pretrained Mixtral 8x7B base model; for Mixtral 8x7B – Instruct it reports
MT-Bench (8.30) and human-preference results. The one score listed is an independent Epoch AI run.
