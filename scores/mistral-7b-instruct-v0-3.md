---
id: score-sheet/mistral-7b-instruct-v0-3
type: score-sheet
slug: mistral-7b-instruct-v0-3
schema_version: 4.0
title: Mistral 7B Instruct v0.3 — scores
model: model/mistral-7b-instruct-v0-3
scores:
- benchmark: benchmark/gpqa-diamond
  value: 0.151831
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
tags:
- llm
- mistral
- dense
- gqa
- instruct
- apache-2-0
updated: '2026-11-22'
sources:
- url: https://arxiv.org/abs/2310.06825
  title: Mistral 7B (Jiang et al., 2023)
  trust: 0.9
- url: https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.3
  title: Mistral-7B-Instruct-v0.3 model card (Hugging Face)
  trust: 0.8
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

Mistral publishes no benchmark scores for Mistral-7B-Instruct-v0.3: its model card has none, and
the Mistral 7B paper reports academic benchmarks only for the pretrained Mistral 7B base model and
MT-Bench for the original Mistral 7B – Instruct. The one score listed is an independent Epoch AI
run.
