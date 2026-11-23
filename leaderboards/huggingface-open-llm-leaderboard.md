---
id: leaderboard/huggingface-open-llm-leaderboard
type: leaderboard
slug: huggingface-open-llm-leaderboard
schema_version: 4.0
title: Open LLM Leaderboard (Hugging Face)
name: Open LLM Leaderboard (Hugging Face)
maintainer: Hugging Face
url: https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard
tagline: An archived ranking of open models run on one shared evaluation harness.
ranks: open models on six benchmarks run with the same harness
update_cadence: Retired
free_access: true
score_origin: independent
tags:
- leaderboard
updated: '2026-11-21'
sources:
- url: https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard
  title: Open LLM Leaderboard (Hugging Face Space)
  trust: 0.9
- url: https://huggingface.co/docs/leaderboards/open_llm_leaderboard/about
  title: Open LLM Leaderboard, About (Hugging Face documentation)
  trust: 0.9
- url: https://huggingface.co/spaces/open-llm-leaderboard/open_llm_leaderboard/discussions/1135
  title: End of the Open LLM Leaderboard (maintainers' post, 2025-03-13)
  trust: 0.9
- url: https://huggingface.co/collections/OpenEvals/archived-open-llm-leaderboard-2024-2025
  title: Archived Open LLM Leaderboard (2024-2025)
  trust: 0.9
---

## What It Ranks

Hugging Face evaluated open models on six benchmarks with the EleutherAI Language Model Evaluation Harness. The maintainers announced on 2025-03-13 that the leaderboard was retiring; its results remain on the Hub as an archive.

- **Benchmarks**: IFEval, BBH, MATH Lvl 5, GPQA, MuSR and MMLU-Pro, in use from June 2024.
- **Earlier version**: a first version ran from 2023 to June 2024 and is archived separately.

## How To Read It

### Methodology

- Every model ran on the same harness; the documentation gives the shots and metric per task, for example 3-shot normalized accuracy for BBH and 5-shot accuracy for MMLU-Pro
- Instruction models ran with their chat template
- Detailed results are in the results dataset on the Hub, and the documentation gives the command to reproduce them with Hugging Face's fork of the harness

### Best For

- Looking up archived scores of open models on the six benchmarks

## Caveats

- Retired in March 2025, so models released since then are absent
- The documentation notes that results can vary slightly with batch size because of padding
- Models flagged by the community carry "Flagged" in their name, and the documentation says to ignore them
