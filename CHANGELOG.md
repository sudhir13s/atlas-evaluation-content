# Changelog

All notable changes to `ai-evaluation-content`. Grouped by schema version / release. Follows
[Keep a Changelog](https://keepachangelog.com/) conventions.

## [Unreleased]

### Added
- Repository scaffold to the content-repo standard: `meta.json`, `schema/registry.json`, the four
  JSON Schemas (`_shared`, `benchmark`, `evaluation-dataset`, `leaderboard-entry`), templates, and
  docs (`README`, `AGENTS`, `CONVENTIONS`).
- Initial evaluation content library (schema version 1.0), all cited:
  - **16 benchmark cards** — MMLU, MMLU-Pro, GPQA, ARC-Challenge, HellaSwag, WinoGrande, BBH,
    TruthfulQA, IFEval, GSM8K, MATH, AIME, DROP, HumanEval, MBPP, SWE-bench — each with what it
    tests, its metric, prompt format, known limitations/contamination, and a worked example.
  - **9 evaluation datasets & harnesses** — lm-evaluation-harness, HELM, OpenAI Evals, BIG-bench,
    AGIEval, Chatbot Arena (LMArena), MT-Bench, AlpacaEval, LiveBench.
  - **10 leaderboard rows** — vendor-reported MMLU / HumanEval / GSM8K / TruthfulQA / ARC-Challenge
    for ten notable models, each marked `self_reported` and cited to its technical report.

## Schema versions

- **1.0** — initial schema for the three evaluation content types.
