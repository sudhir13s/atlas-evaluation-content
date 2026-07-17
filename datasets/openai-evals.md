---
id: evaluation-dataset/openai-evals
type: evaluation-dataset
slug: openai-evals
schema_version: 1.0
title: "OpenAI Evals"
name: "OpenAI Evals"
category: Framework
size: "Open registry of community + first-party evals; extensible"
url: "https://github.com/openai/evals"
description: "An open framework and registry for writing and running custom evals, including model-graded ones."
what_it_tests: "Anything you define — the framework ships templates for exact-match, pattern-match, and model-graded evals, and a registry of community-contributed evals covering reasoning, coding, safety, and domain tasks."
why_it_exists: "Product teams need to measure task-specific quality, not just academic benchmarks. Evals makes it easy to codify 'does the model do X well for my use case?' as a reusable, versioned test — and to use a model as the grader when there is no single right answer."
how_to_use: "Define an eval as YAML + JSONL samples (or reuse one from the registry), then run it against a model via the CLI/SDK. Model-graded templates let a strong model score open-ended outputs against a rubric."
known_issues: "Model-graded evals inherit the grader model's biases and can be gamed by verbosity or style. Community evals vary in quality and rigor. Building a trustworthy custom eval (good coverage, low label noise) is real work — a bad eval gives false confidence."
tags: [framework, custom-evals, model-graded, product]
updated: 2026-07-17
sources:
  - { url: "https://github.com/openai/evals", title: "openai/evals (GitHub)", trust: 0.9 }
related: [benchmark/humaneval]
---

## What It Is
An open-source framework from OpenAI for authoring and running evaluations, plus a registry of shared
evals. It is oriented toward practitioners building product-specific tests rather than reproducing
academic leaderboards.

## How It Evaluates
An eval is a dataset of samples plus a grading method. Built-in methods cover exact match and pattern
match; the distinctive one is **model-graded** evaluation, where a capable model scores open-ended
answers against a rubric or reference. Everything is versioned so tests are repeatable.

## What It Looks For
Task-specific quality signals that generic benchmarks miss — whether a model handles your prompts,
your formats, and your edge cases. It is designed to turn "the model feels worse on our task" into a
measurable, regression-testable number.

## Known Issues
Model-graded scoring is only as good as the grader: it carries the grader's biases and can be swayed
by verbose or confidently-styled answers. Registry quality is uneven. A poorly designed custom eval
(narrow coverage, noisy labels) can mislead more than it informs.
