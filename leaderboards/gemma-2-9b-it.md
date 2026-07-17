---
id: leaderboard-entry/gemma-2-9b-it
type: leaderboard-entry
slug: gemma-2-9b-it
schema_version: 1.0
title: "Gemma 2 9B IT — leaderboard row"
model_slug: gemma-2-9b-it
model_name: "Gemma 2 9B IT"
organization: "Google"
params_b: 9
license: "Gemma License"
self_reported: true
mmlu: 71.3
mmlu_caveat: "Vendor-reported, 5-shot (Gemma 2 technical report, base model)."
humaneval: 40.2
humaneval_caveat: "Vendor-reported pass@1 (Gemma 2 technical report)."
gsm8k: 68.6
gsm8k_caveat: "Vendor-reported (Gemma 2 technical report)."
truthfulqa: 45.3
arc_challenge: 68.4
tags: [google, gemma, open-weight, small]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2408.00118", title: "Gemma 2: Improving Open Language Models at a Practical Size (Google DeepMind, 2024)", trust: 0.9 }
  - { url: "https://huggingface.co/google/gemma-2-9b-it", title: "gemma-2-9b-it model card (Hugging Face)", trust: 0.85 }
---

## Notes
MMLU (71.3), HumanEval (40.2 pass@1), and GSM8K (68.6) are from the Gemma 2 technical report.
TruthfulQA (~45.3) and ARC-Challenge (~68.4) are reference figures from the report/HF archive and
should be verified before citation.
