# CONVENTIONS.md — authoring rules

The rules every file follows. Humans and agents follow the identical process. The authoritative
cross-repo spec is the
[content-repo standard](https://github.com/sudhir13s/TheAIAtlas/blob/main/docs/content-repo-standard.md);
this file is the `evaluation`-domain instance.

## Slugs and file names

- Slugs are **lowercase, hyphen-separated, ASCII** — no spaces, no underscores. Example:
  `mmlu`, `swe-bench`, `lm-evaluation-harness`, `llama-3-1-70b-instruct`.
- The file name is `<slug>.md` and **must equal** the `slug` frontmatter field.
- A file lives in the folder mapped to its `type` in `schema/registry.json`
  (`benchmark` → `benchmarks/`, `evaluation-dataset` → `datasets/`, `score-sheet` → `scores/`,
  `leaderboard` → `leaderboards/`, `evaluation-metric` → `metrics/`, `standard` → `standards/`).

## IDs

- `id` = `<type>/<slug>` (e.g. `benchmark/mmlu`). Globally unique, **immutable**.
- All cross-references use IDs, never titles: `related`, and a dataset that points at the
  benchmark(s) it runs.
- Every referenced ID must resolve to an existing file. Dangling references fail validation.

## Frontmatter

Base fields on **every** file: `id`, `type`, `slug`, `schema_version`, `title`, `tags`, `updated`,
`sources`. Optional base field: `related`. Each type adds its own fields — see
`schema/<type>.schema.json`. Put **structured/reference data in frontmatter** (metrics, scores,
examples) so it stays machine-parseable; the body carries explanation.

- `tags`: lowercase, hyphen-separated.
- `updated`: ISO date (`YYYY-MM-DD`).
- `sources[]`: `{ url, title, trust }` — `trust` in `[0,1]`. Open-source-first; cite, do not copy.
- `schema_version`: the `major.minor` the file was authored against. The repo is at `3.0`; `benchmark` and `evaluation-dataset` files were authored
  against `1.0` and stay valid, because their types did not change.

## Body sections

- Prose lives under `##` headings. The **required sections** and their order per type are declared
  in `schema/registry.json → types.<type>.sections`.
- The `Example` section on a `benchmark` is **mandatory** — a concrete, worked item is what makes the
  card usable. Never leave it empty or hand-wavy.

## Per-type specifics

- **benchmark** — carries `category` (one of the UI groups in the schema enum), `metric`, `method`, `format`,
  `saturation`, and `limitations`. `method`, `metric`, and `limitations` are required: **a score is
  never shown without them.** Body sections: What It Tests → How It Scores → Format → Limitations → Example.
- **evaluation-dataset** — the datasets and harnesses that *run* benchmarks (a harness like
  lm-evaluation-harness, a framework like HELM, a human-preference platform like LMArena, an
  LLM-as-judge set like MT-Bench, a live/contamination-free suite like LiveBench). Carries `category`,
  `url`, `what_it_tests`, `why_it_exists`, `how_to_use`, `known_issues`.
  Body sections: What It Is → How It Evaluates → What It Looks For → Known Issues.
- **score-sheet** — all reported scores of one model. The slug equals the model slug
  (`score-sheet/<model-slug>`). Carries `model` (a `model/<slug>` ID from the models repo) and
  `scores`, a list of `{ benchmark, value, scale, source, observed_on }` where `benchmark` is a
  `benchmark/<slug>` ID. Optional per score: `self_reported`, `method`, `note`. Optional body
  section: Notes (which report a number came from and which benchmarks the vendor did not publish).
  - `value` is copied exactly as the source states it and is **never rescaled**; `scale` says how
    to read it: `percent` (0 to 100) or `fraction` (0 to 1).
  - `source` is the address where the value was published; `observed_on` is the date the Atlas
    recorded it when the source gives none.
  - The same benchmark may appear twice when two sources report different values.
- **leaderboard** — an outside leaderboard as a thing, not its scores. Required: `name`,
  `maintainer`, `url`, `update_cadence`. Optional: `tagline`, `ranks`, `benchmarks` (`benchmark/<slug>`
  IDs the board reports), `coverage`, `data_licence`, `free_access`, `score_origin` (`self-reported`,
  `independent` or `mixed`). Body sections: What It Ranks → How To Read It → Caveats.
- **evaluation-metric** — one metric, as its defining source (paper or tool documentation)
  defines it. Required: `name`, `category` (`retrieval`, `generation`, `agent`, `classification`,
  `ranking`, `safety`), `measures` (one sentence: what a high score means), `needs` (what must be
  supplied), `judged_by` (`code`, `model` or `human`), `direction` (`higher-is-better` or
  `lower-is-better`), `defined_in` (`{title, url}`). Optional: `range` (`{min, max}`, `null` for an
  open bound; only when the source states it), `implemented_by` (`framework/<slug>` IDs whose docs
  implement it as defined), `related_metrics`. A tool-specific name is written "as defined by
  <tool>". Body sections: What It Measures → How It Is Computed → Limits (only limits the source
  or a cited paper states).
- **standard** — a standard, framework, regulation or guidance document that governs AI systems.
  Required: `issuer`, `kind` (`standard`, `framework`, `regulation`, `guidance`), `url` (the
  issuer's own page), `applies_to`. Optional: `version`, `published`, `status`, each only as the
  issuer states it. Body sections: What It Is → Structure (parts by the issuer's own names) → Why
  It Matters For LLM Applications. Paywalled texts are described only from the issuer's public
  page; no legal advice or compliance claims.

## Sourcing

Open-source-first, zero plagiarism. Prefer open papers, official model/technical reports, the
benchmark's own paper, and Papers-with-Code. Reference paywalled material only as a cited pointer,
never as copied text. Scores are **vendor-reported** and marked `self_reported: true`
unless the score states an independent re-run. Record everything in `sources[]`.
