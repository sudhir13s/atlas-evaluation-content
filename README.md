# atlas-evaluation-content

Portable, reusable reference knowledge about **AI evaluation** — benchmarks, the datasets and
harnesses that run them, and per-model score sheets. Authored as Markdown
with structured YAML frontmatter, validated by JSON Schema, and readable by both humans and AI
agents. This repo is **content only**: no code, no build step, no framework lock-in. Any project can
vendor it (as a git submodule or a plain checkout) and render it however it likes.

It is the `evaluation` domain of a reference library
(`atlas-models-content`, `atlas-evaluation-content`, `atlas-frameworks-content`,
`atlas-hardware-content`, `atlas-news-content`, `atlas-practices-content`, `atlas-research-content`), all following the same
[content-repo standard](CONTENT-STANDARD.md).

## What's inside

| Type | Folder | What it captures |
|------|--------|------------------|
| `benchmark` | `benchmarks/` | A standardized test: what it measures, the metric, the prompt format, known limitations/contamination, and a worked example |
| `evaluation-dataset` | `datasets/` | The underlying datasets and harnesses (lm-evaluation-harness, HELM, OpenAI Evals, LMArena, …): what they are, how they evaluate, what signal they isolate |
| `score-sheet` | `scores/` | One model's reported benchmark values, each with its scale, source and date, and marked self-reported or independent |
| `leaderboard` | `leaderboards/` | An outside leaderboard (LMArena, Artificial Analysis, …): who runs it, what it ranks, how to read it and where it misleads |
| `evaluation-metric` | `metrics/` | A metric: what a high score means, what it needs, what judges it, its defining source and the tools that implement it |
| `standard` | `standards/` | A standard, framework, regulation or guidance for AI systems (OWASP, NIST, ISO/IEC, EU AI Act, MITRE ATLAS): issuer, structure, and why it matters for LLM applications |

## Quick start

```bash
# Vendor it
git clone https://github.com/sudhir13s/atlas-evaluation-content.git
# or as a submodule of your project
git submodule add https://github.com/sudhir13s/atlas-evaluation-content.git content/evaluation-src
```

Every content file is a Markdown file with YAML frontmatter (the structured fields) and a Markdown
body (prose and worked examples). Enumerate the library by globbing `**/*.md` and reading each file's
frontmatter, or start from [`meta.json`](meta.json) and
[`schema/registry.json`](schema/registry.json).

## Structure

```
meta.json                 # repo descriptor (machine entrypoint 1)
schema/registry.json      # type → folder/schema/sections map (machine entrypoint 2)
schema/*.schema.json      # JSON Schema per content type
templates/*.template.md   # copy these to author new content
benchmarks/ datasets/ scores/
assets/                   # SVG diagrams
```

## A note on scores

Benchmark numbers age fast and are easy to misread. Two rules this repo enforces:

- **Never a score without its method and limitations.** A number with no methodology is marketing.
  Every `benchmark` file carries `method`, `metric`, and `limitations` at the same level as the score.
- **Score values are vendor-reported and marked as such.** Each score in a `score-sheet` sets
  `self_reported: true` and cites the source. Independent re-runs (LMArena, HF Open LLM Leaderboard,
  LiveBench, Artificial Analysis) are the trustworthy cross-checks — see the `evaluation-dataset` files.

## Contributing

See [CONVENTIONS.md](CONVENTIONS.md) for the authoring rules and [AGENTS.md](AGENTS.md) for the
machine-navigation guide. In short: copy a template, fill the frontmatter and required sections, use
IDs (not titles) for links, and open a PR. Content must be open-source-sourced and cited — never
copied from paywalled material.

## License

Content is licensed **CC-BY-4.0** (see [LICENSE](LICENSE)) — reuse freely with attribution.
