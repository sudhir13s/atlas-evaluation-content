---
id: benchmark/nl2repo
type: benchmark
slug: nl2repo
schema_version: 1.0
title: "NL2Repo"
name: NL2Repo-Bench
category: Code Generation
description: "A long-horizon benchmark: build a complete, installable Python repository from scratch given only a natural-language requirements document and an empty workspace."
what_is_it: "NL2Repo-Bench (natural-language-to-repository) measures whether an LLM or coding agent can turn a single requirements document into a fully functional Python library — no scaffolding, no source, no tests revealed. Starting from an empty workspace, the agent must do architectural design, dependency management, multi-file implementation, and packaging on its own. It comprises 104 tasks across nine library categories, with an average specification length of ~18,800 tokens, far longer than typical repository-level benchmarks."
why_people_use_it: "Most code benchmarks test localized generation, scaffolded completion, or short repair. NL2Repo-Bench targets the long-horizon capability of building a whole software system end-to-end, which stays largely unsolved: even the strongest agents pass below ~40% of the hidden tests and rarely produce a fully correct repository. It exposes failure modes — premature termination, loss of global coherence, fragile cross-file dependencies, weak planning — that short-task benchmarks never surface."
examples:
  - "Given an ~18k-token spec for a CSV/tabular utility library (reader, schema inference, filtering, export), build the full installable package so the upstream test suite passes."
  - "From a spec for a small networking-tools library, produce a pip-installable package with correct module layout and dependencies, no ImportError on install."
method: "Each task gives the agent one natural-language specification document in an empty workspace; the agent works autonomously to produce a complete Python repository. The generated code is installed and run against the original upstream test suite inside an isolated Docker environment; the task score is the fraction of those tests that pass."
metric: "Average test pass rate — the mean fraction of the hidden upstream tests that the generated repository passes, averaged over tasks. Full-repository correctness (all tests pass) is also reported and is rare."
format: "0-shot, single spec document; autonomous multi-file/multi-step agent run; execution-based grading against held-out upstream tests."
saturation: low
limitations: "The benchmark is Python-only and library-centric (nine categories), so it does not cover applications, services, or other languages. Grading is by upstream test pass rate, so a partially-built repo can score non-zero even when unusable, and a hidden test suite may under- or over-specify the intended behavior. Because inputs are long (~18.8k tokens) and runs span hundreds of steps, scores are sensitive to the agent scaffold and its context/step budget as much as to the base model."
citation: "https://arxiv.org/abs/2512.12730"
example:
  task_type: "Repository generation (Python library from a spec)"
  input: "An ~18,800-token requirements document for a small tabular-data utility library: it must expose `read_csv`, schema inference, row filtering, and `to_json` export, with a stated public API, error behavior, and packaging expectations. Workspace is empty; no tests are shown."
  success:
    answer: "Plan the package layout, create `pyproject.toml` + `src/tablekit/{__init__,io,schema,filter,export}.py` implementing the specified public API, declare only the needed dependencies, make it `pip install`-able, and match the documented error semantics — the upstream test suite then passes at a high rate."
    why: "The repository is complete, installable, and its modules honor the spec's public API and behavior, so the held-out upstream tests pass."
  failure:
    answer: "Implement `read_csv` and `to_json` in a single `main.py`, declare done early — but leave `schema` inference and the package `__init__.py` exports missing, so `import tablekit` raises ImportError and half the API is absent."
    why: "Premature termination plus a broken package structure: the repo fails to install/import cleanly and misses specified modules, so most upstream tests error out — the classic long-horizon failure the benchmark targets."
  checks: "Whether an agent can sustain global coherence across a whole repository — design, implement, wire up, and package many files from one spec — rather than complete a single function."
  scoring: "Average test pass rate over the hidden upstream test suite (fraction of tests passing), per task then averaged."
tags: [code-generation, coding-agent, long-horizon, repository, python]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2512.12730", title: "NL2Repo-Bench: Towards Long-Horizon Repository Generation Evaluation of Coding Agents", trust: 0.95 }
  - { url: "https://arxiv.org/html/2512.12730v2", title: "NL2Repo-Bench (arXiv HTML, full paper)", trust: 0.95 }
  - { url: "https://huggingface.co/papers/2512.12730", title: "NL2Repo-Bench — Hugging Face paper page", trust: 0.85 }
related: [benchmark/swe-bench, benchmark/deepswe, benchmark/livecodebench, benchmark/humaneval, benchmark/mbpp]
---

## What It Tests
Whether a coding agent can build a **whole Python library from a single specification** — not
complete a snippet, not patch an existing repo, but design and construct the entire thing from an
empty workspace. The agent alone handles architecture, dependency choices, multi-file implementation,
and packaging, with **no scaffolding, source, or tests revealed**.

The 104 tasks span nine library categories, weighted toward realistic breadth:

- **System Tools (24)**, **Data Analysis & Processing (18)**, **Testing (13)**, **Utility Libraries (11)**
- **Web Development (10)**, **Networking Tools (9)**, **Machine Learning (7)**, **Database Interaction (7)**, **Batch File Processing (5)**

Specs are long — **~18,800 tokens on average** — so the model must hold a large design in context
across hundreds of steps.

## How It Scores
The generated repository is **installed and run against the original upstream test suite** in an
isolated Docker environment. The task score is the **average test pass rate** — the fraction of those
hidden tests the repo passes — averaged across tasks. Whole-repository correctness (every test green)
is reported separately and is rare.

The signal is stark: **all evaluated models score below ~40.5% average pass rate**, and nearly half
fall under 20%. The strongest results (for example Claude-Sonnet-4.5 under the Claude Code and
OpenHands harnesses) land around **40%**, confirming that long-horizon repository generation is
largely unsolved.

## Format
Zero-shot from a single spec document, then a long autonomous agent run. Grading is purely
**execution-based** against held-out upstream tests — no LLM judge — so a repository either installs
and passes tests or it does not.

## Limitations
The benchmark is **Python-only and library-centric**, so it does not cover applications, services, or
other languages. Because grading is by **test pass rate**, a half-built repo can score non-zero while
being unusable, and a given upstream suite may not fully pin the intended behavior. And with
**~18.8k-token inputs over hundreds of steps**, the score reflects the agent scaffold's context and
step budget as much as the base model — swap the harness and the number moves.

## Example
The agent receives an ~18,800-token spec for a tabular-data utility library — `read_csv`, schema
inference, filtering, `to_json`, with a stated public API and error behavior — and an empty
workspace. A passing solution lays out a proper package (`pyproject.toml`, a `src/` tree, module
files), implements the full API, declares only the needed dependencies, and is pip-installable, so
the upstream tests pass at a high rate. A plausible failing solution stuffs a couple of functions into
one `main.py`, declares done early, and leaves the package `__init__.py` and schema module missing —
`import` fails and most tests error out. That gap between "some functions work" and "a coherent,
installable repo" is exactly what NL2Repo-Bench measures.
