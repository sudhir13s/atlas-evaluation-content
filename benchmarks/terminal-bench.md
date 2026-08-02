---
id: benchmark/terminal-bench
type: benchmark
slug: terminal-bench
schema_version: 1.0
title: "Terminal-Bench"
name: Terminal-Bench (v2.1)
category: Agent & Tool Use
description: "A command-line agent benchmark: an agent is dropped into a container and must complete real terminal tasks, graded by automated tests on the final state."
what_is_it: "Terminal-Bench evaluates an agent-model system on real technical tasks completed entirely through a command-line interface. Version 2.1 ships 89 curated tasks spanning software engineering, machine learning, security, data processing, scientific computing, and system administration. The agent is given a written instruction and a purpose-built Docker container with the relevant files and tools; it can inspect the environment, run commands, edit files, install packages, and revise based on execution feedback. Automated tests then inspect the container's final state to decide pass or fail."
why_people_use_it: "It measures end-to-end agentic competence in the environment engineers actually work in — the terminal — rather than one-shot code snippets. Because a run pairs a model with a harness (context management, terminal access, tool execution, error recovery), it surfaces how much the scaffold matters, not just the model. Built by Stanford and the Laude Institute, it has become a standard yardstick for CLI coding agents."
examples:
  - "Given a container with a broken Python package, fix the build so its test suite passes."
  - "Train a fastText classifier on the provided data until it reaches at least 0.62 accuracy on the held-out set."
method: "An agent-model system is placed in a task-specific Docker container with a natural-language instruction and interacts through the terminal over multiple steps. After the agent finishes, hidden automated tests inspect the final container state; the task counts as solved only if those tests pass."
metric: "Accuracy — the proportion of tasks whose automated tests pass across all trials (task-resolution success rate). Scores are reported with a confidence interval from repeated trials."
format: "Multi-step agentic loop, 0-shot; containerized terminal environment; pass/fail per task from final-state tests."
saturation: medium
limitations: "Version 2.1 is a correction of 2.0 that fixed 28 of the 89 tasks (broken external dependencies, too-tight resource budgets, instructions that did not match tests), so scores are not comparable across versions — always cite the version. With only 89 tasks the confidence intervals are wide, and results conflate model quality with harness quality (a strong model on a weak harness underperforms). The environment is Linux/CLI only, so it says nothing about GUI, multimodal, or long-context abilities."
citation: "https://www.tbench.ai/"
example:
  task_type: "Terminal task (system administration / build repair)"
  input: "Container has a Python package whose `pip install -e .` fails and whose test suite errors on import. Instruction: 'Make the package installable and get its pytest suite to pass.' The agent has a shell and can read files, edit, and install."
  success:
    answer: "Inspect the traceback, find a missing `package_dir`/`__init__.py` and a pinned-but-yanked dependency in `pyproject.toml`, fix the packaging metadata and relax the pin, run `pip install -e .`, then `pytest -q` — all tests green."
    why: "The final container state satisfies the instruction: the package installs and the hidden test suite passes, which is exactly what the automated grader checks."
  failure:
    answer: "Run `pip install -e . || pip install .` and `pytest`, see the import error, then `pip install <top-level-module-name>` from PyPI to satisfy the import and declare done."
    why: "Installing an unrelated same-named PyPI package masks the import error instead of fixing the local packaging; the final-state tests still fail (or import the wrong module), so the task is scored unsolved."
  checks: "Whether the agent can diagnose a real environment failure from execution feedback and drive the container to a test-passing final state, not just silence the first error."
  scoring: "Pass/fail per task from final-state automated tests; benchmark score is the fraction of tasks passing (accuracy)."
tags: [agent, tool-use, terminal, cli, coding-agent, leaderboard]
updated: 2026-08-02
sources:
  - { url: "https://www.tbench.ai/", title: "Terminal-Bench — official site and leaderboard", trust: 0.95 }
  - { url: "https://www.laude.org/updates/terminal-bench", title: "Terminal-Bench — Laude Institute", trust: 0.9 }
  - { url: "https://snorkel.ai/leaderboard/terminal-bench-2-1/", title: "Terminal-Bench 2.1: LLM Terminal Agent Benchmark (Snorkel AI)", trust: 0.85 }
  - { url: "https://snorkel.ai/blog/terminal-bench-2-0-raising-the-bar-for-ai-agent-evaluation/", title: "Terminal-Bench 2.0: Raising the bar for AI agent evaluation (Snorkel AI)", trust: 0.85 }
related: [benchmark/swe-bench, benchmark/bfcl-v4, benchmark/tau2-bench, benchmark/deepswe]
---

## What It Tests
Whether an agent can get real technical work done through a terminal — the environment engineers
actually use. Each of the 89 tasks (v2.1) hands the agent a container and an instruction and lets it
loop: inspect, run commands, edit files, install packages, react to errors.

The task pool is deliberately broad:

- **Software engineering** — build repair, refactors, failing test suites.
- **Machine learning** — train a model to a target metric.
- **Security** — capture-the-flag-style challenges, file/permission work.
- **Data processing, scientific computing, system administration** — pipelines, environment setup, ops.

The point is holistic competence: planning, tool use, and recovery over many steps, not a single
formatted answer.

## How It Scores
Every task ends with **hidden automated tests that inspect the container's final state**. A trial is
**successful only if those tests pass**; the headline number is **accuracy** — the proportion of
successful trials across all tasks — reported with a confidence interval from repeated runs.

Because a run is an **agent-model system**, the model supplies reasoning and generation while the
**harness** controls context management, terminal access, tool execution, and error recovery. Two
systems using the same model can score differently, so results are always credited to the pair.

## Format
Zero-shot, multi-step, containerized. The agent receives one natural-language instruction and a
Docker environment, then works through the terminal until it declares completion. No worked examples
are provided. Top agent-model systems currently score in the low-to-mid **80s percent**, leaving real
headroom on the hardest tasks.

## Limitations
Version 2.1 **fixed 28 of the 89 tasks** from 2.0 — cases with external dependencies that changed,
resource budgets too tight for valid solutions, or instructions that did not match their tests — and
introduced continuous validation. Because of those fixes, **scores are not comparable across
versions**; cite the version. With only 89 tasks, confidence intervals are wide. And every score
blends **model quality with harness quality**, so a headline number reflects the whole system, not the
model alone. The benchmark is Linux/CLI only, so it does not probe GUI, multimodal, or long-context
skills.

## Example
The agent gets a container with a Python package whose install and test suite are broken, and the
instruction "make it installable and get pytest to pass." A correct run traces the failure to broken
packaging metadata and a bad dependency pin, fixes both, reinstalls, and confirms the suite is green —
the final-state tests then pass. A plausible wrong run installs an unrelated same-named package from
PyPI to silence the import error; the symptom disappears but the graded tests still fail, so the task
is scored unsolved.
