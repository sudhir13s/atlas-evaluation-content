---
id: leaderboard-entry/gemma-2-27b-it
type: leaderboard-entry
slug: gemma-2-27b-it
schema_version: 1.0
title: "Gemma 2 27B IT — leaderboard row"
model_slug: gemma-2-27b-it
model_name: "Gemma 2 27B IT"
organization: "Google"
params_b: 27
license: "Gemma License"
self_reported: true
mmlu: 75.2
mmlu_caveat: "Vendor-reported, 5-shot (Gemma 2 technical report, base model)."
humaneval: 51.8
humaneval_caveat: "Vendor-reported pass@1 (Gemma 2 technical report)."
gsm8k: 74.0
gsm8k_caveat: "Vendor-reported, 5-shot / 8-shot (Gemma 2 technical report)."
truthfulqa: 51.0
arc_challenge: 71.4
tags: [google, gemma, open-weight]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2408.00118", title: "Gemma 2: Improving Open Language Models at a Practical Size (Google DeepMind, 2024)", trust: 0.9 }
  - { url: "https://huggingface.co/google/gemma-2-27b-it", title: "gemma-2-27b-it model card (Hugging Face)", trust: 0.85 }
---

## Notes
MMLU (75.2), HumanEval (51.8 pass@1), and GSM8K (74.0) come from Google's Gemma 2 technical report
(several are reported on the base model, which the report tabulates alongside the IT variant).
TruthfulQA (~51) and ARC-Challenge (~71.4) are the report's/HF-archive reference figures and should be
verified before citation. Gemma 2 is notable for punching above its size on human-preference boards
(LMArena) relative to these static numbers.
