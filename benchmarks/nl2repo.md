---
id: benchmark/nl2repo
type: benchmark
slug: nl2repo
schema_version: 1.0
title: "NL2Repo"
name: NL2Repo-Bench
category: Code Generation
description: "A long-horizon benchmark: build a complete, installable Python repository from scratch given only a natural-language requirements document and an empty workspace."
what_is_it: "NL2Repo-Bench (Ding et al., 2025, ByteDance Seed, M-A-P and university collaborators) measures whether a coding agent can turn a single natural-language requirements document into a complete, installable Python library, starting from an empty workspace. The agent must design the architecture, manage dependencies and implement multi-module logic itself. It has 104 tasks in nine categories of Python libraries, with input documents averaging about 18,800 tokens."
why_people_use_it: "The authors say prior evaluations focus on localized code generation, scaffolded completion or short repairs. In their experiments (December 2025), even the strongest agents had average test pass rates below 40% and rarely completed an entire repository correctly; the failure modes they name include premature termination, loss of global coherence, fragile cross-file dependencies and inadequate planning."
examples:
  - "A requirements document for one of the 104 Python libraries; the agent builds the library in an empty workspace, and the target repository's official test suite is run on it."
method: "Each task gives the agent one requirements document and an empty workspace. The generated library is evaluated in a dedicated Docker environment per task against the official test suite of the target repository."
metric: "Average test pass rate, used as the task score."
format: "A single specification document, an autonomous multi-step agent run, and execution-based grading against the target repository's test suite."
limitations: "It covers Python libraries only (nine categories). A partly built repository can still pass some tests, so the pass rate is not the same as a working library."
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2512.12730", title: "NL2Repo-Bench: Towards Long-Horizon Repository Generation Evaluation of Coding Agents (Ding et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2512.12730", title: "NL2Repo-Bench, full text (arXiv HTML)", trust: 0.95 }
related: [benchmark/swe-bench, benchmark/livecodebench, benchmark/humaneval, benchmark/mbpp]
---

## What It Tests
Whether a coding agent can build a **whole Python library from a single specification**, starting from an empty workspace.

The 104 tasks span nine library categories:

- **System Tools (24)**, **Data Analysis & Processing (18)**, **Testing (13)**, **Utility Libraries (11)**
- **Web Development (10)**, **Networking Tools (9)**, **Machine Learning (7)**, **Database Interaction (7)**, **Batch File Processing (5)**

Input documents average about **18,800 tokens**.

## How It Scores
The generated library runs against the **official test suite of the target repository** in a dedicated Docker environment per task. The task score is the **average test pass rate**.

- **Paper's results (December 2025)** — the strongest agents stayed below 40% and rarely reproduced a whole repository.

## Format
One requirements document and an empty workspace, then an autonomous agent run. Grading is execution-based, with no LLM judge.

## Limitations
- **Python libraries only.** Nine categories; no applications or other languages.
- **Partial credit.** A partly built repository can pass some tests.

## Example
The agent receives the requirements document for one library, designs its modules and dependencies, and writes the code. The target repository's official tests then run on the result in that task's Docker environment, and the share of tests passed is the task score.
