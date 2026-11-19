# Changelog

All notable changes to `ai-evaluation-content`. Grouped by schema version / release. Follows
[Keep a Changelog](https://keepachangelog.com/) conventions.

## [Unreleased]

### Schema 2.0 (score sheets; breaking)

- **Added** the `score-sheet` type (`scores/`, `score-sheet/<model-slug>`): `model` (a
  `model/<slug>` ID) and `scores`, a list of `{ benchmark, value, scale, source, observed_on }`
  with optional `self_reported`, `method` and `note`. `scale` is `percent` or `fraction`; values
  are never rescaled. Optional body section: Notes.
- **Removed** the `leaderboard-entry` type (its `leaderboards/` folder, schema and template). It
  folds into `score-sheet`: the five score fields (`mmlu`, `humaneval`, `gsm8k`, `truthfulqa`,
  `arc_challenge`) became `scores` entries with `scale: percent`, each `*_caveat` became the
  score's `note`, `self_reported` moved onto each score, and the Notes section carried over.
  `model_name`, `organization`, `params_b` and `license` are dropped: they are facts of the
  `model` record.
- **Migrated** the 10 leaderboard entries and the benchmark scores held inside 7 model records
  (`scale: fraction`) into 10 `score-sheet` records (59 scores). Two scores of
  `model/deepseek-v3` (`HumanEval-Mul`, `MATH-500`) match no `benchmark` record and were skipped;
  15 leaderboard values equal to a model-record value were merged into it.
- **Changed** the shared base `type` list: `leaderboard-entry` out, `score-sheet` in.

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
- **2.0** — `leaderboard-entry` replaced by `score-sheet` (breaking).
