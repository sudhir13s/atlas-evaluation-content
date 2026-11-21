# Changelog

All notable changes to `ai-evaluation-content`. Grouped by schema version / release. Follows
[Keep a Changelog](https://keepachangelog.com/) conventions.

## [Unreleased]

### Content (Professional Work category)

- **Added** the benchmark category "Professional Work" to the `category` enum of
  `benchmark.schema.json`. It is a content-only change: no field changed, so the schema stays at 3.0
  and existing records stay valid.
- **Changed** `gdpval`, `remote-labor-index` and `harvey-lab` move from "Agent & Tool Use" to
  "Professional Work": each is built from real professional work products judged for acceptance.
- **Changed** `healthbench` moves from "Truthfulness & Safety" to "Knowledge & Academic Reasoning":
  it scores answers to health conversations against physician-written rubrics, not hallucination or
  abstention behaviour.

### Schema 3.0 (evaluation metrics reshaped; standards added)

- **Changed (breaking, for `evaluation-metric` only)** the `evaluation-metric` type, which had no
  records. Added required `measures` (one sentence: what a high score means), `needs[]` (what must
  be supplied to compute it), `judged_by` (`code`, `model` or `human`: what decides the score in
  the defining source) and `defined_in` (`{title, url}` of the defining paper or documentation);
  added optional `implemented_by[]` (`framework/<slug>` IDs from the frameworks repo). Removed
  `needs_reference_answer` and `needs_judge_model`, which `needs` and `judged_by` replace. `range`
  became optional: it is written only when the defining source states one. Body sections are now
  What It Measures, How It Is Computed, Limits.
- **Added** 13 `evaluation-metric` records: context precision, context recall, faithfulness and
  answer relevancy (as defined by Ragas), mean reciprocal rank, normalized discounted cumulative
  gain, exact match, ROUGE, BERTScore, attack success rate (HarmBench), StrongREJECT score, task
  success rate (WebArena) and AST accuracy (Berkeley Function-Calling Leaderboard).
- **Added** the `standard` type (`standards/`, `standard/<slug>`): a standard, framework,
  regulation or guidance document that governs AI systems. `issuer`, `kind` (standard, framework,
  regulation, guidance), `url` and `applies_to` are required; optional `version`, `published` and
  `status`, each as the issuer states it. Body sections: What It Is, Structure, Why It Matters For
  LLM Applications. 5 records: OWASP Top 10 for LLM Applications (2025), NIST AI Risk Management
  Framework with its Generative AI Profile, the EU AI Act and MITRE ATLAS.
- **Added** `standard` to the shared `type` enum, `meta.json` and `registry.json`, with a template.
- **Removed** the `evaluation-framework` type (its schema, template, and `registry.json`, `meta.json`
  and shared `type` enum entries). It had no records: an evaluation tool is one `framework` record
  in the frameworks repo (category `evaluation` or `guardrails`), which the Evals Tools page lists.

### Schema 2.2 (benchmark categories; additive)

- **Added** three values to the `category` enum of the `benchmark` type: `Cybersecurity`,
  `Voice & Speech` and `Science & Research`. Adding values loosens the rule, so no other record
  changes schema version. `cybergym`, `cwe-bench`, `sec-bench` and `webexploitbench` moved to
  `Cybersecurity`, `vamos-bench` to `Voice & Speech`, and `terminal-bench-science` to
  `Science & Research`.

### Schema 2.1 (leaderboards, evaluation frameworks and metrics; additive)

- **Added** the `leaderboard` type (`leaderboards/`, `leaderboard/<slug>`): an outside leaderboard
  as a thing. `name`, `maintainer`, `url`, `update_cadence` are required; optional `tagline`,
  `ranks`, `benchmarks[]` (`benchmark/<slug>`), `coverage`, `data_licence`, `free_access` and
  `score_origin` (self-reported, independent, mixed). Body sections: What It Ranks, How To Read
  It, Caveats. 6 records, copied word for word from the `LEADERBOARDS` constant of the Evals page.
- **Added** the `evaluation-framework` type (`frameworks/`, `evaluation-framework/<slug>`): a tool
  that evaluates LLM applications or agents. `name`, `maintainer`, `repository_url`, `license`,
  `scope` (application, agent, model) and `languages[]` are required; optional `docs_url` and
  `metrics[]` (`evaluation-metric/<slug>`). Body sections: What It Is, When To Use, How It Works,
  Limitations, References. No records yet.
- **Added** the `evaluation-metric` type (`metrics/`, `evaluation-metric/<slug>`): `name`,
  `category` (retrieval, generation, agent, classification, ranking, safety), `range` (`{min,
  max}`, null for an open bound), `direction`, `needs_reference_answer` and `needs_judge_model`
  are required; optional `related_metrics[]`. Body sections: What It Measures, How It Is Computed,
  When To Use, Failure Modes, References. No records yet.
- **Added** the three types to the shared `type` enum, their templates, and `meta.json` /
  `registry.json` entries; all three schemas reject unknown fields.

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
- **2.1** — `leaderboard`, `evaluation-framework` and `evaluation-metric` types.
- **2.2** — three new benchmark categories (additive).
- **3.0** — `evaluation-metric` reshaped (breaking for that type, which had no records); `standard` type.
