---
id: leaderboard-entry/phi-3-mini-4k-instruct
type: leaderboard-entry
slug: phi-3-mini-4k-instruct
schema_version: 1.0
title: "Phi-3 Mini 4K Instruct — leaderboard row"
model_slug: phi-3-mini-4k-instruct
model_name: "Phi-3 Mini 4K Instruct"
organization: "Microsoft"
params_b: 3.8
license: "MIT"
self_reported: true
mmlu: 68.8
mmlu_caveat: "Vendor-reported, 5-shot (Phi-3 technical report)."
humaneval: 58.5
humaneval_caveat: "Vendor-reported pass@1 (Phi-3 technical report)."
gsm8k: 82.5
gsm8k_caveat: "Vendor-reported, 8-shot CoT (Phi-3 technical report)."
truthfulqa: 65.0
arc_challenge: 84.9
tags: [microsoft, phi, open-weight, small]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2404.14219", title: "Phi-3 Technical Report (Microsoft, 2024)", trust: 0.9 }
  - { url: "https://huggingface.co/microsoft/Phi-3-mini-4k-instruct", title: "Phi-3-mini-4k-instruct model card (Hugging Face)", trust: 0.85 }
---

## Notes
Phi-3-mini (3.8B) is the strongest small model in this row set, thanks to its heavily curated training
data. MMLU (68.8), HumanEval (58.5 pass@1), and GSM8K (82.5) are Microsoft's reported figures from the
Phi-3 technical report, which does tabulate TruthfulQA (~65) and ARC-Challenge (~84.9) — these two are
closer to vendor-reported here than in the other rows, but still verify against the report before
citation. Phi models are also the textbook caution on contamination given their synthetic-data
training recipe.
