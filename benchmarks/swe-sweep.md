---
id: benchmark/swe-sweep
type: benchmark
slug: swe-sweep
schema_version: 1.0
title: "SWE-sweep"
name: SWE-sweep
category: Code Generation
description: "A Meta benchmark where a coding agent gets a real repository with many hidden bugs and no hints, and must find and fix as many as it can."
what_is_it: "SWE-sweep, from Meta Superintelligence Labs, asks an agent to explore a repository at a fixed past commit, discover the bugs in it and output a patch. It covers 100 repositories and about 4,100 known bugs. The agent receives only a broad instruction, with no issue text, file names, line ranges or bug type. Fixes are checked against hidden tests taken from the upstream pull requests that later fixed those bugs."
why_people_use_it: "Most software-engineering benchmarks give the agent a bug report and measure repair. SWE-sweep measures the earlier step: deciding what is broken. That matters as users delegate broader goals to agents. The code is a thin wrapper around the Harbor evaluation framework, so labs can run it on their own agents."
method: "For each repository the authors collect issue and pull-request pairs and pick one commit where the most bugs exist together. They keep bugs whose fixes and tests reproduce at that state and that can be found from the repository alone. The agent's patch is run against the original test suite and, per bug, hidden fail-to-pass tests."
metric: "Fraction of all bugs resolved across all repositories, in percent. A bug counts when all its hidden tests pass and the original suite still passes; a task scores 0 if the patch breaks the original suite, otherwise the share of its bugs fixed."
format: "Agentic, 0-shot, one repository per task at a fixed base commit with a broad find-and-fix instruction; the agent returns a patch."
limitations: "Any history-derived benchmark under-counts the bugs really present, and only bugs confirmed by an upstream fix and covered by tests are scored. Results depend on the harness: the official board uses the mini-SWE-agent scaffold, while vendor tables report 'provider harness' runs (each vendor's own tool) and 'mini-SWE harness' runs that they computed themselves, so they cannot be compared with the board. The paper is a preprint, and the site lists no arXiv identifier for it. The site says public leaderboard submissions were still 'coming soon'."
citation: "https://swesweep.com"
tags: [coding, agent, bug-finding, repository-level, meta]
updated: 2026-11-21
sources:
  - { url: "https://swesweep.com", title: "SWE-sweep — project site and leaderboard", trust: 0.95 }
  - { url: "https://github.com/facebookresearch/swe-sweep", title: "facebookresearch/swe-sweep — evaluation harness", trust: 0.9 }
  - { url: "https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf", title: "Gemini 4 Neon model evaluation (Google DeepMind)", trust: 0.7 }
related: [benchmark/swe-bench, benchmark/terminal-bench]
---

## What It Tests
Whether an agent can **discover** bugs on its own, then fix them without breaking the project.

- **No hints** — no issue description, file name, line range or bug category.
- **Long-horizon work** — the agent must explore and understand a large codebase.
- **Interacting fixes** — repairs are not independent, and a regression zeroes the task.

## How It Scores
Each bug is a **fail-to-pass test set**, and the score is the share of bugs resolved.

- **Resolved bug** — every hidden test for it passes and the original suite still passes.
- **Task score** — fixed bugs divided by bugs in the task, or 0 on any new failure.
- **Benchmark score** — a micro-average: total bugs solved over total bugs, so larger tasks weigh more.

## Format
One repository at a fixed base commit and a broad instruction to find and fix as many bugs as possible. The agent returns a unified diff. The repository wraps Harbor, and evaluation uses a separate verifier environment.

## Limitations
- **Under-counting.** Only bugs confirmed by an upstream fix, with executable tests, are scored; other real fixes earn nothing.
- **Harness labels.** Vendor tables mention 'provider harness' and 'mini-SWE harness'. Google's evaluation methodology for Gemini 4 Neon says it computed both itself, so they are not board scores.
- **Open points.** The site lists a preprint with no arXiv identifier, and the repository README refers to a LICENSE file without naming the licence.

## Example
The scoring rule is stated as pseudo-code on the repository. Suppose a task has **four** hidden bugs: if the agent's patch fixes one and the original tests still pass, the task score is **1/4**. If the same patch breaks any original test, the score is **0** whatever it fixed. This is an illustration of the stated formula, not a real task.
