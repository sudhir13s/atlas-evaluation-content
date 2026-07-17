---
id: leaderboard-entry/llama-3-1-8b-instruct
type: leaderboard-entry
slug: llama-3-1-8b-instruct
schema_version: 1.0
title: "Llama 3.1 8B Instruct — leaderboard row"
model_slug: llama-3-1-8b-instruct
model_name: "Llama 3.1 8B Instruct"
organization: "Meta"
params_b: 8
license: "Llama 3.1 Community License"
self_reported: true
mmlu: 68.4
mmlu_caveat: "Vendor-reported, 5-shot (Llama 3.1 model card / paper)."
humaneval: 72.6
humaneval_caveat: "Vendor-reported pass@1 (Llama 3.1 paper)."
gsm8k: 84.5
gsm8k_caveat: "Vendor-reported, 8-shot CoT (Llama 3.1 paper)."
truthfulqa: 54.0
arc_challenge: 83.0
tags: [meta, llama, open-weight, small]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2407.21783", title: "The Llama 3 Herd of Models (Meta, 2024)", trust: 0.9 }
  - { url: "https://huggingface.co/meta-llama/Llama-3.1-8B-Instruct", title: "Llama-3.1-8B-Instruct model card (Hugging Face)", trust: 0.85 }
---

## Notes
MMLU (68.4, 5-shot), HumanEval (72.6 pass@1), and GSM8K (84.5, 8-shot CoT) are Meta's reported
figures. TruthfulQA (~54) and ARC-Challenge (~83) are not in Meta's headline instruct table; they are
reference approximations aligned with the Llama-3-8B range on the archived HF Open LLM Leaderboard v1
and should be verified before citation.
