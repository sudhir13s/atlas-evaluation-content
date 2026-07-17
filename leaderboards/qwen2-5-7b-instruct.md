---
id: leaderboard-entry/qwen2-5-7b-instruct
type: leaderboard-entry
slug: qwen2-5-7b-instruct
schema_version: 1.0
title: "Qwen2.5 7B Instruct — leaderboard row"
model_slug: qwen2-5-7b-instruct
model_name: "Qwen2.5 7B Instruct"
organization: "Alibaba (Qwen Team)"
params_b: 7.6
license: "Apache-2.0"
self_reported: true
mmlu: 74.2
mmlu_caveat: "Vendor-reported (Qwen2.5 technical report)."
humaneval: 84.8
humaneval_caveat: "Vendor-reported pass@1 (Qwen2.5 technical report)."
gsm8k: 85.4
gsm8k_caveat: "Vendor-reported (Qwen2.5 technical report)."
truthfulqa: 64.0
arc_challenge: 63.0
tags: [alibaba, qwen, open-weight, small]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2412.15115", title: "Qwen2.5 Technical Report (Qwen Team, 2024)", trust: 0.9 }
  - { url: "https://huggingface.co/Qwen/Qwen2.5-7B-Instruct", title: "Qwen2.5-7B-Instruct model card (Hugging Face)", trust: 0.85 }
---

## Notes
MMLU (74.2), HumanEval (84.8 pass@1), and GSM8K (85.4) are the Qwen team's reported figures.
TruthfulQA (~64) and ARC-Challenge (~63) are reference approximations not headlined in the Qwen2.5
report and should be verified before citation.
