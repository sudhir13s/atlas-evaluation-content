---
id: leaderboard-entry/llama-3-1-70b-instruct
type: leaderboard-entry
slug: llama-3-1-70b-instruct
schema_version: 1.0
title: "Llama 3.1 70B Instruct — leaderboard row"
model_slug: llama-3-1-70b-instruct
model_name: "Llama 3.1 70B Instruct"
organization: "Meta"
params_b: 70
license: "Llama 3.1 Community License"
self_reported: true
mmlu: 86.0
mmlu_caveat: "Vendor-reported, 5-shot (Llama 3.1 model card / paper)."
humaneval: 80.5
humaneval_caveat: "Vendor-reported pass@1 (Llama 3.1 paper)."
gsm8k: 95.1
gsm8k_caveat: "Vendor-reported, 8-shot CoT (Llama 3.1 paper)."
truthfulqa: 63.0
arc_challenge: 93.0
tags: [meta, llama, open-weight]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2407.21783", title: "The Llama 3 Herd of Models (Meta, 2024)", trust: 0.9 }
  - { url: "https://huggingface.co/meta-llama/Llama-3.1-70B-Instruct", title: "Llama-3.1-70B-Instruct model card (Hugging Face)", trust: 0.85 }
---

## Notes
MMLU (86.0, 5-shot), HumanEval (80.5 pass@1), and GSM8K (95.1, 8-shot CoT) are Meta's own reported
figures from the Llama 3.1 paper and model card — hence `self_reported: true`. Meta's headline table
does not report TruthfulQA or ARC-Challenge for the instruct model; the values here (TruthfulQA ~63,
ARC-Challenge ~93) are reference approximations consistent with the Llama-3-70B range on the archived
HF Open LLM Leaderboard v1 (TruthfulQA MC2, ARC-Challenge 25-shot) and should be verified before
citation. Independent re-runs (LMArena, LiveBench) are the trustworthy cross-check.
