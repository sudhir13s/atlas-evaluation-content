# CONVENTIONS.md — authoring rules

The rules every file follows. Humans and agents follow the identical process. The authoritative
cross-repo spec is the
[content-repo standard](https://github.com/sudhir13s/TheAIAtlas/blob/main/docs/standards/content-repo-standard.md);
this file is the `evaluation`-domain instance.

## Slugs and file names

- Slugs are **lowercase, hyphen-separated, ASCII** — no spaces, no underscores. Example:
  `mmlu`, `swe-bench`, `lm-evaluation-harness`, `llama-3-1-70b-instruct`.
- The file name is `<slug>.md` and **must equal** the `slug` frontmatter field.
- A file lives in the folder mapped to its `type` in `schema/registry.json`
  (`benchmark` → `benchmarks/`, `evaluation-dataset` → `datasets/`, `leaderboard-entry` → `leaderboards/`).

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
- `schema_version`: the `major.minor` the file was authored against (currently `1.0`).

## Body sections

- Prose lives under `##` headings. The **required sections** and their order per type are declared
  in `schema/registry.json → types.<type>.sections`.
- The `Example` section on a `benchmark` is **mandatory** — a concrete, worked item is what makes the
  card usable. Never leave it empty or hand-wavy.

## Per-type specifics

- **benchmark** — carries `category` (one of the eight UI groups), `metric`, `method`, `format`,
  `saturation`, and `limitations`. `method`, `metric`, and `limitations` are required: **a score is
  never shown without them.** Body sections: What It Tests → How It Scores → Format → Limitations → Example.
- **evaluation-dataset** — the datasets and harnesses that *run* benchmarks (a harness like
  lm-evaluation-harness, a framework like HELM, a human-preference platform like LMArena, an
  LLM-as-judge set like MT-Bench, a live/contamination-free suite like LiveBench). Carries `category`,
  `url`, `what_it_tests`, `why_it_exists`, `how_to_use`, `known_issues`.
  Body sections: What It Is → How It Evaluates → What It Looks For → Known Issues.
- **leaderboard-entry** — one model's row. Carries `model_slug`, `model_name`, `organization`,
  `params_b`, `license`, and the five score fields (`mmlu`, `humaneval`, `gsm8k`, `truthfulqa`,
  `arc_challenge`) plus optional per-score `*_caveat` strings. `self_reported` states whether the
  numbers are the vendor's own. The consumer computes the row `average`. Body section: Notes (source
  of each number, and which benchmarks the vendor did not publish).

## Sourcing

Open-source-first, zero plagiarism. Prefer open papers, official model/technical reports, the
benchmark's own paper, and Papers-with-Code. Reference paywalled material only as a cited pointer,
never as copied text. Leaderboard scores are **vendor-reported** and marked `self_reported: true`
unless the file states an independent re-run. Record everything in `sources[]`.
