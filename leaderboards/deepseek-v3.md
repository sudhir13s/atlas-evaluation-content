---
id: leaderboard-entry/deepseek-v3
type: leaderboard-entry
slug: deepseek-v3
schema_version: 1.0
title: "DeepSeek-V3 — leaderboard row"
model_slug: deepseek-v3
model_name: "DeepSeek-V3"
organization: "DeepSeek"
params_b: 671
license: "DeepSeek License"
self_reported: true
mmlu: 88.5
mmlu_caveat: "Vendor-reported (DeepSeek-V3 technical report)."
humaneval: 82.6
humaneval_caveat: "Vendor-reported pass@1 / HumanEval-Mul (DeepSeek-V3 technical report)."
gsm8k: 89.3
gsm8k_caveat: "Vendor-reported, CoT (DeepSeek-V3 technical report)."
truthfulqa: 68.0
arc_challenge: 90.0
tags: [deepseek, moe, open-weight, frontier]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2412.19437", title: "DeepSeek-V3 Technical Report (DeepSeek-AI, 2024)", trust: 0.9 }
  - { url: "https://huggingface.co/deepseek-ai/DeepSeek-V3", title: "DeepSeek-V3 model card (Hugging Face)", trust: 0.85 }
---

## Notes
DeepSeek-V3 is a 671B-total Mixture-of-Experts model with ~37B active parameters per token. MMLU
(88.5), HumanEval (~82.6 pass@1), and GSM8K (89.3, CoT) are DeepSeek's reported figures. The report
leads with MMLU-Pro, GPQA, MATH-500, and code benchmarks rather than TruthfulQA/ARC-Challenge, so
TruthfulQA (~68) and ARC-Challenge (~90) here are reference approximations and should be verified
before citation.
