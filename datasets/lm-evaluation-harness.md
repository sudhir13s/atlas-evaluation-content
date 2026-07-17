---
id: evaluation-dataset/lm-evaluation-harness
type: evaluation-dataset
slug: lm-evaluation-harness
schema_version: 1.0
title: "lm-evaluation-harness"
name: "EleutherAI lm-evaluation-harness"
category: Harness
size: "60+ benchmark groups, hundreds of individual tasks"
url: "https://github.com/EleutherAI/lm-evaluation-harness"
description: "The de facto open-source harness for running language-model benchmarks reproducibly."
what_it_tests: "Whatever task you configure — MMLU, ARC, HellaSwag, GSM8K, TruthfulQA, and hundreds more — via a unified, version-pinned request/scoring pipeline."
why_it_exists: "Before it, every paper reported benchmarks with different prompts, shot counts, and answer-extraction rules, making cross-paper numbers incomparable. The harness fixes the protocol so results are reproducible and comparable."
how_to_use: "pip install, then run `lm_eval --model hf --model_args pretrained=<model> --tasks mmlu,arc_challenge --num_fewshot 5`. It supports Hugging Face, vLLM, and API backends and emits per-task scores."
known_issues: "Task definitions and prompt templates evolve between versions, so scores are only comparable at a fixed harness version. Some tasks have multiple community variants. Likelihood-based multiple-choice scoring can disadvantage certain prompt formats."
tags: [harness, reproducibility, open-source]
updated: 2026-07-17
sources:
  - { url: "https://github.com/EleutherAI/lm-evaluation-harness", title: "EleutherAI/lm-evaluation-harness (GitHub)", trust: 0.95 }
  - { url: "https://doi.org/10.5281/zenodo.5371628", title: "lm-evaluation-harness (Zenodo, Gao et al.)", trust: 0.9 }
related: [benchmark/mmlu, benchmark/arc-challenge, benchmark/gsm8k]
---

## What It Is
An open-source Python framework from EleutherAI that runs standardized evaluations over language
models. It powers the Hugging Face Open LLM Leaderboard and is the reference implementation many
model reports cite for their numbers.

## How It Evaluates
For each task it constructs prompts (with the configured few-shot count), queries the model backend
(local Transformers, vLLM, or a remote API), and scores responses — by answer likelihood for
multiple-choice tasks or by exact/regex match for generative ones. Configs are version-pinned so a
run is reproducible.

## What It Looks For
Reproducible, apples-to-apples task scores. Its value is protocol consistency: the same prompts, shot
counts, and extraction logic for every model, so differences reflect the model, not the harness.

## Known Issues
Comparability holds only within a fixed harness version — templates and task sets change across
releases. Multiple community task variants can produce different numbers for "the same" benchmark.
Likelihood scoring can penalize models whose formatting differs from the template's assumptions.
