---
id: benchmark/deepswe
type: benchmark
slug: deepswe
schema_version: 3.1
title: "DeepSWE"
name: DeepSWE
category: Code Generation
description: "A Datacurve benchmark of 113 original, long-horizon software-engineering tasks from 91 active open-source repositories, graded by hand-written behavioural verifiers."
what_is_it: "DeepSWE, published by Datacurve in May 2026 (Huang, Lee, Tng and Ge), measures coding agents on original, long-horizon engineering tasks drawn from active open-source repositories. It has 113 tasks across 91 repositories and five languages: TypeScript (35 tasks), Go (34), Python (34), JavaScript (5) and Rust (5). Tasks use the Harbor format, each with an isolated environment, a reference solution and a program-based verifier. C++ and Java are not yet covered."
why_people_use_it: "Tasks are written from scratch, not adapted from existing commits or pull requests, so the maker says no model has seen the solution in pretraining. Datacurve reports prompts about half the length of SWE-bench Pro's (2,158 vs 4,614 characters) while reference solutions add about 5.5 times more lines of code."
method: "An agent works in an isolated environment on a task prompt, and a hand-written verifier tests the resulting behaviour. Datacurve says its verifiers check software behaviour, not implementation details, and that behavioural verification needs a minimum amount of specificity in the prompt."
metric: "Resolved rate in percent = tasks whose verifier passes / tasks attempted."
format: "Agentic coding in Harbor task format: a prompt (instruction.md), an isolated Docker environment, hidden verifier tests and a withheld reference patch per task."
limitations: "Only five languages are covered and most tasks are in three of them. Prompts are shorter than SWE-bench Pro's but still longer than the messages developers send, because behavioural tests need a floor of specificity. The maker's site lists a later v1.1 post while the launch post describes the 113-task set, so check which version a number refers to. Harness choice changes results; vendors run their own scaffolds."
citation: "https://deepswe.datacurve.ai/blog/deepswe"
openness: open
openness_note: "The 113 tasks with environments, verifiers and reference solutions can be downloaded from Hugging Face after accepting the maker's access conditions (the card calls them held-out evaluation data), and the repository is Apache-2.0."
tags: [coding, agent, long-horizon, open-source-repositories, datacurve]
updated: 2026-11-23
sources:
  - { url: "https://deepswe.datacurve.ai/blog/deepswe", title: "DeepSWE — Measuring frontier coding agents (Datacurve)", trust: 0.95 }
  - { url: "https://github.com/datacurve-ai/deep-swe", title: "datacurve-ai/deep-swe — repository (Apache-2.0)", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/datacurve/deep-swe", title: "datacurve/deep-swe — Hugging Face dataset (gated)", trust: 0.9 }
related: [benchmark/swe-bench]
---

## What It Tests
Whether an agent can make **useful, well-scoped changes** across varied real codebases.

- **Breadth** — 113 tasks, 91 repositories, five languages.
- **Original tasks** — written from scratch, not taken from commits or pull requests; some are motivated by unresolved GitHub issues.
- **Larger solutions** — reference solutions add 668 lines on average vs 120 for SWE-bench Pro, per Datacurve.
- **Makers** — Datacurve, launch post dated 2026-05-26.

## How It Scores
A **program-based verifier** per task, written by hand.

- **Behaviour first** — tests target what the software does, not how it is built.
- **Isolated** — each task has its own environment.
- **Result** — resolved or not; resolved rate = tasks passed / tasks attempted.

## Format
Harbor task format, as listed on the dataset card:

- **task.toml** — metadata.
- **instruction.md** — the prompt the agent sees.
- **environment/** — a Dockerfile.
- **tests/** — the verifier.
- **solution/** — the reference patch, withheld from the agent.

Data: gated Hugging Face dataset `datacurve/deep-swe`; code: GitHub `datacurve-ai/deep-swe`, **Apache-2.0**.

## Limitations
- **Language coverage.** TypeScript, Go, Python, JavaScript and Rust only.
- **Prompt floor.** Behavioural tests need some specificity in the prompt.
- **Versions.** The site lists a v1.1 post; the launch post describes the 113-task set.
- **Harness.** Vendor scaffolds differ, which moves scores.

## Example
No task text is quoted: the dataset is gated as held-out evaluation data. The launch post describes one real task, **Boa evaluation cancellation**, in the Rust JavaScript engine Boa.

- **Goal** — let a running evaluation be cancelled.
- **Trap** — an agent implemented most of the cancellation logic but missed that `Promise.then` callbacks were still queued without the cancellation handle, so they could run after cancellation.
- **Verdict** — the behavioural verifier catches that, so the task is not resolved.
