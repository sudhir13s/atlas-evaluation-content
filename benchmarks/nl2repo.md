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
worked_examples:
  - task_type: "Data-analysis library from a spec"
    input: "An ~18,800-token requirements document for a small tabular-data utility library: it must expose `read_csv`, schema inference, row filtering, and `to_json` export, with a stated public API, error behavior, and packaging expectations. Workspace is empty; no tests are shown."
    success:
      answer: "Plan the package layout, create `pyproject.toml` + `src/tablekit/{__init__,io,schema,filter,export}.py` implementing the specified public API, declare only the needed dependencies, make it `pip install`-able, and match the documented error semantics."
      why: "The repository is complete, installable, and its modules honor the spec's public API and behavior, so the held-out upstream tests pass at a high rate."
    failure:
      answer: "Implement `read_csv` and `to_json` in a single `main.py`, declare done early — but leave `schema` inference and the package `__init__.py` exports missing, so `import tablekit` raises ImportError and half the API is absent."
      why: "Premature termination plus a broken package structure: the repo fails to install/import cleanly and misses specified modules, so most upstream tests error out — the classic long-horizon failure the benchmark targets."
    checks: "Whether an agent can sustain global coherence across a whole repository — design, implement, wire up, and package many files from one spec — rather than complete a single function."
    scoring: "Average test pass rate over the hidden upstream tests: high vs near-zero when import fails."
  - task_type: "Networking-tools library (packaging correctness)"
    input: "A spec for a small networking utility library `nethelp` with sub-packages `nethelp.dns`, `nethelp.http`, and `nethelp.ports`, each exposing named functions the spec lists; it must be pip-installable with a documented dependency set."
    success:
      answer: "Create a proper `src/nethelp/` package with an `__init__.py` in every sub-package that re-exports the listed public names, and a `pyproject.toml` whose `[tool.setuptools.packages.find]` discovers all sub-packages."
      why: "Every sub-package is discoverable and its public names import cleanly, so `from nethelp.dns import resolve` works and the upstream suite runs against the full API."
    failure:
      answer: "Write the modules but forget `__init__.py` in `nethelp/dns` and `nethelp/ports`, and leave package discovery at the default flat layout."
      why: "The missing `__init__.py` files mean the sub-packages are not packaged/importable — `import nethelp.dns` raises ModuleNotFoundError, so every test touching those namespaces errors out (broken package structure)."
    checks: "Getting multi-package layout and discovery right so the whole public surface is importable after install."
    scoring: "Average test pass rate: only the http tests pass vs the full suite green."
  - task_type: "Testing-helper library (cross-file wiring)"
    input: "A spec for a testing-assertions library where a public `assert_that(x)` fluent builder in `core.py` must dispatch to matcher classes defined in `matchers/` (e.g. `equal_to`, `close_to`, `has_item`), with the exact error-message format specified."
    success:
      answer: "Define the matcher protocol in `matchers/base.py`, implement each matcher, and have `assert_that` import and compose them so `assert_that(3).is_(close_to(3.1, 0.2))` routes to the right matcher and raises with the specified message."
      why: "The builder and matchers are wired through a shared interface, so the fluent chains resolve and the assertion messages match, passing the behavior tests."
    failure:
      answer: "Implement `assert_that` and the matcher classes in separate files but hard-code only `equal_to` in the builder's dispatch, leaving `close_to`/`has_item` unreachable."
      why: "The cross-file dependency is only half-wired: unrouted matchers raise AttributeError at call time, so any test using them fails — fragile cross-file dependency, a named long-horizon failure mode."
    checks: "Sustaining a consistent interface contract across files so components actually call one another as designed."
    scoring: "Average test pass rate: equal_to tests pass vs all-matcher suite green."
  - task_type: "Database-interaction library (global coherence)"
    input: "A spec for a lightweight ORM-style library: a `Model` base class, a `Field` descriptor system, a `QuerySet` with `.filter()/.order_by()`, and a SQLite backend — the spec fixes how `Field` metadata flows into generated SQL."
    success:
      answer: "Design a coherent object model: `Field` descriptors register on `Model` via a metaclass, `QuerySet` reads that registry to build parameterized SQL, and the backend executes it — one consistent data model threaded through every module."
      why: "Because the field registry, query builder, and backend agree on the same metadata contract, generated SQL is correct and the round-trip tests pass."
    failure:
      answer: "Build `Model`, `Field`, `QuerySet`, and the backend separately, each inventing its own notion of a column, so `QuerySet` reads attributes the `Field` metaclass never registered."
      why: "Loss of global coherence: the modules disagree on the column contract, so query generation emits wrong/empty SQL and most integration tests fail even though each file looks plausible alone (weak global planning)."
    checks: "Holding one consistent domain model across many files instead of locally-plausible but mutually-inconsistent components."
    scoring: "Average test pass rate: low, from contract mismatch, vs high when the model is coherent."
  - task_type: "ML-utility library (dependency management)"
    input: "A spec for a small feature-preprocessing library exposing `StandardScaler`, `OneHotEncoder`, and a `Pipeline`, with the spec stating it must depend only on numpy (no scikit-learn) and declare it in `pyproject.toml`."
    success:
      answer: "Implement the transforms directly on numpy arrays, expose the specified `fit`/`transform` API, and declare exactly `dependencies = [\"numpy\"]` in `pyproject.toml`."
      why: "The install pulls only the allowed dependency and the numpy implementations honor the API, so the environment builds and the tests pass."
    failure:
      answer: "Implement the transforms by importing `sklearn.preprocessing` under the hood and omit it from the declared dependencies."
      why: "Undeclared dependency: the isolated Docker install has no scikit-learn, so `import sklearn` raises ImportError at collection time and the whole suite errors — a dependency-management failure the harness surfaces immediately."
    checks: "Declaring an accurate, minimal dependency set that matches what the code actually imports and what the spec allows."
    scoring: "Average test pass rate: zero on missing dependency vs full suite green."
tags: [code-generation, coding-agent, long-horizon, repository, python]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2512.12730", title: "NL2Repo-Bench: Towards Long-Horizon Repository Generation Evaluation of Coding Agents", trust: 0.95 }
  - { url: "https://arxiv.org/html/2512.12730v2", title: "NL2Repo-Bench (arXiv HTML, full paper)", trust: 0.95 }
  - { url: "https://huggingface.co/papers/2512.12730", title: "NL2Repo-Bench — Hugging Face paper page", trust: 0.85 }
related: [benchmark/swe-bench, benchmark/livecodebench, benchmark/humaneval, benchmark/mbpp]
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
