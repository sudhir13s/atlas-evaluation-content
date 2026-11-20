---
id: score-sheet/qwen2-5-72b-instruct
type: score-sheet
slug: qwen2-5-72b-instruct
schema_version: 2.0
title: Qwen2.5 72B Instruct — scores
model: model/qwen2-5-72b-instruct
scores:
- benchmark: benchmark/arc-challenge
  value: 72.4
  scale: percent
  source: https://arxiv.org/abs/2412.15115
  observed_on: '2026-07-17'
  self_reported: true
- benchmark: benchmark/gpqa-diamond
  value: 0.491477
  scale: fraction
  source: https://epoch.ai/benchmarks
  observed_on: '2025-01-27'
  self_reported: false
- benchmark: benchmark/gsm8k
  value: 0.958
  scale: fraction
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-19'
  self_reported: true
  method: vendor-reported
- benchmark: benchmark/humaneval
  value: 0.866
  scale: fraction
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-19'
  self_reported: true
  method: 0-shot
- benchmark: benchmark/math
  value: 0.831
  scale: fraction
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-19'
  self_reported: true
  method: vendor-reported
- benchmark: benchmark/mmlu
  value: 86.1
  scale: percent
  source: https://arxiv.org/abs/2412.15115
  observed_on: '2026-07-17'
  self_reported: true
  note: Vendor-reported (Qwen2.5 technical report).
- benchmark: benchmark/mmlu-pro
  value: 0.711
  scale: fraction
  source: https://qwenlm.github.io/blog/qwen2.5-llm/
  observed_on: '2026-11-19'
  self_reported: true
  method: 5-shot
- benchmark: benchmark/truthfulqa
  value: 69.0
  scale: percent
  source: https://arxiv.org/abs/2412.15115
  observed_on: '2026-07-17'
  self_reported: true
tags:
- llm
- qwen
- alibaba
- dense
- gqa
- instruct
- multilingual
- frontier
updated: '2026-11-20'
sources:
- url: https://qwenlm.github.io/blog/qwen2.5-llm/
  title: https://qwenlm.github.io/blog/qwen2.5-llm/
- url: https://arxiv.org/abs/2412.15115
  title: Qwen2.5 Technical Report (Qwen Team, 2024)
  trust: 0.9
- url: https://huggingface.co/Qwen/Qwen2.5-72B-Instruct
  title: Qwen2.5-72B-Instruct model card (Hugging Face)
  trust: 0.85
- url: https://epoch.ai/benchmarks
  title: Epoch AI, "Capabilities & benchmarking" (epoch.ai/benchmarks), CC BY 4.0; credit Epoch AI as the source.
---

## Notes

MMLU (86.1), HumanEval (86.6 pass@1), and GSM8K (95.8) are the Qwen team's reported figures from the
Qwen2.5 technical report. The report emphasizes MMLU/MMLU-Pro/math/code rather than TruthfulQA or
ARC-Challenge; TruthfulQA (~69) and ARC-Challenge (~72) are reference approximations and should be
verified before citation.
