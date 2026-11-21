---
id: benchmark/voicebench
type: benchmark
slug: voicebench
schema_version: 1.0
title: "VoiceBench"
name: VoiceBench
category: Voice & Speech
description: "A multi-faceted benchmark for LLM-based voice assistants using real and synthetic spoken instructions that vary by speaker, environment and content."
what_is_it: "VoiceBench evaluates LLM-based voice assistants, the systems behind real-time speech interaction. Earlier evaluations focused on speech recognition or general knowledge with clean speech. VoiceBench adds spoken instructions, both real and synthetic, that include variations in speaker characteristics, environment and content. The dataset has subsets with a stated sample count: alpacaeval (199), alpacaeval_full (636), commoneval (200), wildvoice (1,000), openbookqa (455), mmsu (3,074), sd-qa (553), mtbench (46), ifeval (345), bbh (1,000) and advbench (520)."
why_people_use_it: "Voice assistants such as GPT-4o-style systems need benchmarks designed for speech interaction, which were missing. VoiceBench covers open-ended question answering, multiple-choice question answering, reference-based question answering, multi-turn conversation, instruction following, reasoning and safety. The paper appeared in TACL 2026 and the code is Apache-2.0."
method: "A voice assistant answers spoken or text instructions from a chosen subset. For the open-ended subsets alpacaeval, commoneval, wildvoice and sd-qa, the repository scores responses automatically with gpt-4o-mini as judge. The sd-qa subset is split by region code, such as aus or usa."
metric: "Scored per subset. The repository documents a gpt-4o-mini judge score for the open-ended subsets and does not detail the scoring of the others in its quick start. It states that alpacaeval_full is the subset used in the leaderboard."
format: "Spoken instructions, from Google text-to-speech or human recordings depending on the subset, across eleven subsets and task types including open-ended QA, multiple-choice QA, reference-based QA, multi-turn QA, instruction following, reasoning and safety."
limitations: "Several subsets use synthetic Google text-to-speech audio, so speech variety there is limited; human-recorded subsets are commoneval, wildvoice, sd-qa and bbh. Scores from the GPT judge depend on that judge model. Subsets vary in size from 46 to 3,074 samples. The paper is from October 2024, so it predates recent full-duplex voice agents."
citation: "https://arxiv.org/abs/2410.17196"
tags: [voice, speech, voice-assistants, spoken-instructions, apache-2]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2410.17196", title: "VoiceBench: Benchmarking LLM-Based Voice Assistants (arXiv:2410.17196)", trust: 0.95 }
  - { url: "https://github.com/matthewcym/voicebench", title: "MatthewCYM/VoiceBench — repository (Apache-2.0)", trust: 0.9 }
related: [benchmark/tau-voice, benchmark/vamos-bench]
---

## What It Tests
How well an **LLM voice assistant** handles spoken instructions in realistic conditions.

- **Variation** — speaker characteristics, environment and content.
- **Task types** — open-ended, multiple-choice and reference-based QA, multi-turn, instruction following, reasoning, safety.
- **Audio sources** — human recordings and Google text-to-speech.

## How It Scores
**Per-subset scores**, with a GPT judge for open-ended answers.

- **Judge** — gpt-4o-mini for alpacaeval, commoneval, wildvoice and sd-qa.
- **Leaderboard subset** — alpacaeval_full.
- **Regions** — sd-qa is evaluated by region code.

## Format
Eleven subsets, from 46 to 3,074 samples. A model can be tested on spoken audio or on the same instructions as text, using the `--modality` option.

## Limitations
- **Synthetic audio.** Many subsets use Google text-to-speech.
- **Judge-dependent.** Open-ended scores come from a language-model judge.
- **Age.** The 2024 paper predates full-duplex voice agents.

## Example
The `sd-qa` subset shows the design: the same questions are spoken by people from different regions, such as **Australia (aus) or the United States (usa)**, and the assistant's answers are compared against references, so differences expose accent sensitivity. Running with `--modality text` on the same data separates speech understanding from the underlying language skill.
