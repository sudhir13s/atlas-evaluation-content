---
id: benchmark/swe-sweep
type: benchmark
slug: swe-sweep
schema_version: 4.0
title: SWE-sweep
name: SWE-sweep
category: Code Generation
description: A Meta benchmark where a coding agent gets a real repository with many hidden bugs and no hints, and must find and fix as many as it can.
what_is_it: SWE-sweep, from Meta Superintelligence Labs (2026 preprint), asks an agent to explore a repository at a fixed past commit, discover the bugs in it and output a patch. It covers 100 repositories and about 4,100 known bugs. The agent receives only a broad instruction, with no issue text, file names, line ranges or bug type. Fixes are checked against hidden tests taken from the upstream pull requests that later fixed those bugs.
why_people_use_it: 'Most software-engineering benchmarks give the agent a bug report and measure repair. SWE-sweep measures the earlier step: deciding what is broken. The code is a thin wrapper around the Harbor evaluation framework, so labs can run it on their own agents.'
method: For each repository the authors collect issue and pull-request pairs and pick one commit where the most bugs exist together. They keep bugs whose fixes and tests reproduce at that state and that can be found from the repository alone. The agent's patch is run against the original test suite and, per bug, hidden fail-to-pass tests in a separate verifier environment.
metric: 'Fraction of all bugs resolved across all repositories, in percent. A bug counts when all its hidden tests pass and the original suite still passes; a task scores 0 if the patch breaks a previously passing test, otherwise bugs fixed / bugs in the task. The benchmark score is the micro-average: the sum over tasks of (bugs in task x task score) / total bugs.'
format: Agentic, 0-shot, one repository per task at a fixed base commit with a broad find-and-fix instruction; the agent returns a unified diff.
limitations: 'Any history-derived benchmark under-counts the bugs really present, and only bugs confirmed by an upstream fix and covered by tests are scored. Results depend on the harness: every entry on the official board uses the mini-SWE-agent scaffold, so runs in other scaffolds are not board scores. The paper is a preprint with no arXiv identifier listed, and the site said public leaderboard submissions were still ''coming soon'' (updated 2026-11-02).'
citation: https://swesweep.com
openness: open
openness_note: The repository publishes all 100 task directories, each with its instruction, environment, hidden tests and reference solution, plus the scoring code; its LICENSE file is not a standard licence GitHub can identify.
tags:
- coding
- agent
- bug-finding
- repository-level
- meta
updated: '2026-11-23'
sources:
- url: https://swesweep.com
  title: SWE-sweep — project site and leaderboard
  trust: 0.95
- url: https://github.com/facebookresearch/swe-sweep
  title: facebookresearch/swe-sweep — evaluation harness and tasks
  trust: 0.9
- url: https://github.com/facebookresearch/swe-sweep/blob/main/tasks/swe-sweep__dateutil/instruction.md
  title: SWE-sweep task swe-sweep__dateutil, instruction.md
  trust: 0.9
related:
- benchmark/swe-bench
- benchmark/terminal-bench
---

## What It Tests

Whether an agent can **discover** bugs on its own, then fix them without breaking the project.

- **Scale** — 100 repositories, about 4,100 bugs.
- **No hints** — no issue description, file name, line range or bug category.
- **Long-horizon work** — the agent must explore and understand a large codebase.
- **Interacting fixes** — repairs are not independent, and a regression zeroes the task.

## How It Scores

Each bug is a **fail-to-pass test set**, and the score is the share of bugs resolved.

- **Resolved bug** — every hidden test for it passes and the original suite still passes.
- **Task score** — fixed bugs / bugs in the task, or 0 if any previously passing test now fails.
- **Benchmark score** — a micro-average: total bugs solved / total bugs, so larger tasks weigh more.

## Format

One repository at a fixed base commit and a broad instruction to find and fix as many bugs as possible. The agent returns a unified diff.

- **Harness** — wraps Harbor; evaluation runs in a separate verifier environment.
- **Task layout** — `instruction.md`, `environment/`, `tests/`, `solution/`, `task.toml` per task.
- **Licence** — the repository's LICENSE file is not a standard licence GitHub identifies.

## Limitations

- **Under-counting.** Only bugs confirmed by an upstream fix, with executable tests, are scored; other real fixes earn nothing.
- **One board scaffold.** Board entries all use mini-SWE-agent; other harnesses are not comparable.
- **Open points.** Preprint with no arXiv identifier; public submissions "coming soon".

## Example

From the public task **swe-sweep__dateutil** (`instruction.md` in the repository), the agent is told:

> "Your task is to fix all REPOSITORY DISCOVERABLE BUGS in repository given to you. You MUST NOT make any OUT OF SCOPE edits."

- **Discoverable bug** — one whose expected behaviour is evident from the repository alone, such as its docs, docstrings or type annotations.
- **Rule** — breaking any test that passed at the start sets the score to zero.
- **Setting** — the repository is checked out at `/dateutil`, with no internet access.
