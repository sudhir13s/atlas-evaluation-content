---
id: leaderboard-entry/qwen2-5-72b-instruct
type: leaderboard-entry
slug: qwen2-5-72b-instruct
schema_version: 1.0
title: "Qwen2.5 72B Instruct — leaderboard row"
model_slug: qwen2-5-72b-instruct
model_name: "Qwen2.5 72B Instruct"
organization: "Alibaba (Qwen Team)"
params_b: 72.7
license: "Qwen License"
self_reported: true
mmlu: 86.1
mmlu_caveat: "Vendor-reported (Qwen2.5 technical report)."
humaneval: 86.6
humaneval_caveat: "Vendor-reported pass@1 (Qwen2.5 technical report)."
gsm8k: 95.8
gsm8k_caveat: "Vendor-reported (Qwen2.5 technical report)."
truthfulqa: 69.0
arc_challenge: 72.4
tags: [alibaba, qwen, open-weight]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2412.15115", title: "Qwen2.5 Technical Report (Qwen Team, 2024)", trust: 0.9 }
  - { url: "https://huggingface.co/Qwen/Qwen2.5-72B-Instruct", title: "Qwen2.5-72B-Instruct model card (Hugging Face)", trust: 0.85 }
---

## Notes
MMLU (86.1), HumanEval (86.6 pass@1), and GSM8K (95.8) are the Qwen team's reported figures from the
Qwen2.5 technical report. The report emphasizes MMLU/MMLU-Pro/math/code rather than TruthfulQA or
ARC-Challenge; TruthfulQA (~69) and ARC-Challenge (~72) are reference approximations and should be
verified before citation.
