---
id: leaderboard-entry/mistral-7b-instruct-v0-3
type: leaderboard-entry
slug: mistral-7b-instruct-v0-3
schema_version: 1.0
title: "Mistral 7B Instruct v0.3 — leaderboard row"
model_slug: mistral-7b-instruct-v0-3
model_name: "Mistral 7B Instruct v0.3"
organization: "Mistral AI"
params_b: 7.2
license: "Apache-2.0"
self_reported: true
mmlu: 62.5
mmlu_caveat: "Reference figure for the Mistral 7B base (Mistral 7B paper, 5-shot); v0.3 instruct is close."
humaneval: 40.0
humaneval_caveat: "Reference pass@1 from the Mistral 7B paper."
gsm8k: 52.0
gsm8k_caveat: "Reference figure (Mistral 7B paper, 8-shot)."
truthfulqa: 56.0
arc_challenge: 63.0
tags: [mistral, open-weight, small]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2310.06825", title: "Mistral 7B (Jiang et al., 2023)", trust: 0.9 }
  - { url: "https://huggingface.co/mistralai/Mistral-7B-Instruct-v0.3", title: "Mistral-7B-Instruct-v0.3 model card (Hugging Face)", trust: 0.8 }
---

## Notes
Mistral did not publish a full per-benchmark table for the v0.3 instruct point release; the MMLU
(~62.5), HumanEval (~40), and GSM8K (~52) figures here are the Mistral 7B paper's base-model numbers,
which the instruct variant tracks closely — treat them as reference approximations, not exact v0.3
instruct measurements. TruthfulQA (~56) and ARC-Challenge (~63) are HF Open LLM v1-archive-range
approximations and should be verified before citation. This row is the clearest example of why the
`self_reported` flag and independent re-runs matter.
