---
id: leaderboard/huggingface-open-llm-leaderboard
type: leaderboard
slug: huggingface-open-llm-leaderboard
schema_version: 2.1
title: HuggingFace Open LLM Leaderboard
name: HuggingFace Open LLM Leaderboard
maintainer: Hugging Face
url: https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard
tagline: The only fair-apples ranking of open-weight models at scale.
update_cadence: Continuous (community submissions)
coverage: 3000+ open-weight models
free_access: true
tags:
- leaderboard
updated: '2026-11-19'
sources:
- url: https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard
  title: HuggingFace Open LLM Leaderboard
---

## What It Ranks

Runs `lm-evaluation-harness` (v2 suite: IFEval, BBH, MATH Lvl 5, GPQA, MuSR, MMLU-Pro) against thousands of open-weight models on identical infra. Community can submit any model on HuggingFace Hub and it is re-run from scratch — no author-reported numbers.

### Why You Need It

The open-weight ecosystem has 3000+ models. Paper-reported scores cannot be compared across papers (different prompts, different few-shot counts, different post-processing). This is the one place that runs everyone on the same code.

### Why It Matters

Before this existed, open-model claims were impossible to verify. Now a Llama-3-8B fine-tune that claims to beat the base model has to prove it on the same harness — you can see exactly how much it gained (or regressed).

## How To Read It

### Methodology

- Fixed `lm-evaluation-harness` config, version-pinned
- Every model re-run from scratch — no self-reported numbers accepted
- v2 suite (June 2024) replaced v1 after v1 saturation
- Per-task scores plus a normalized average

### Strengths

- Reproducible — harness is open-source, configs are public
- Massive coverage (3000+ open-weight models)
- Fine-tune comparison vs. base model is trivial
- Free and publicly hosted, no paywall

### Best For

- Picking the best open-weight model at a given size (7B / 13B / 70B)
- Verifying fine-tune claims
- Tracking the open-weight frontier

## Caveats

- Harness-specific quirks can penalize certain prompt formats
- v1 was aggressively gamed before retirement — v2 is harder but not immune
- Hosted / closed models (GPT, Claude, Gemini) not included
- Does not measure instruction-following style or long-context ability
