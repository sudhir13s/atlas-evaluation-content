---
id: leaderboard-entry/mixtral-8x7b-instruct-v0-1
type: leaderboard-entry
slug: mixtral-8x7b-instruct-v0-1
schema_version: 1.0
title: "Mixtral 8x7B Instruct v0.1 — leaderboard row"
model_slug: mixtral-8x7b-instruct-v0-1
model_name: "Mixtral 8x7B Instruct v0.1"
organization: "Mistral AI"
params_b: 46.7
license: "Apache-2.0"
self_reported: true
mmlu: 70.6
mmlu_caveat: "Vendor-reported, 5-shot (Mixtral of Experts paper; base model)."
humaneval: 40.2
humaneval_caveat: "Reference pass@1 (Mixtral of Experts paper)."
gsm8k: 60.4
gsm8k_caveat: "Vendor-reported, 8-shot (Mixtral of Experts paper)."
truthfulqa: 57.0
arc_challenge: 70.0
tags: [mistral, mixtral, moe, open-weight]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2401.04088", title: "Mixtral of Experts (Jiang et al., 2024)", trust: 0.9 }
  - { url: "https://huggingface.co/mistralai/Mixtral-8x7B-Instruct-v0.1", title: "Mixtral-8x7B-Instruct-v0.1 model card (Hugging Face)", trust: 0.8 }
---

## Notes
Mixtral 8x7B is a sparse Mixture-of-Experts model: 46.7B total parameters but only ~12.9B active per
token. MMLU (70.6, 5-shot), HumanEval (~40.2 pass@1), and GSM8K (60.4, 8-shot) are from the Mixtral of
Experts paper (base-model table; the instruct variant tracks closely). TruthfulQA (~57) and
ARC-Challenge (~70) are HF-archive-range approximations and should be verified before citation.
