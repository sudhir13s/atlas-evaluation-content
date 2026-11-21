---
id: benchmark/deepswe
type: benchmark
slug: deepswe
schema_version: 1.0
title: "DeepSWE"
name: DeepSWE
category: Code Generation
description: "A Datacurve benchmark of 113 original, long-horizon software-engineering tasks from 91 active open-source repositories, graded by hand-written behavioural verifiers."
what_is_it: "DeepSWE measures frontier coding agents on original, long-horizon engineering tasks drawn from active open-source repositories. It has 113 tasks across 91 repositories and five languages: TypeScript, Go, Python, JavaScript and Rust, with most tasks in the first three. Tasks use the Harbor format, each with an isolated environment and a program-based verifier. C++ and Java are not yet covered."
why_people_use_it: "Tasks are written from scratch, not adapted from existing commits or pull requests, so the maker says no model has seen the solution in pretraining. Datacurve reports prompts about half the length of SWE-bench Pro's while solutions need 5.5 times more code and about twice the output tokens. Several vendors cite a 'DeepSWE v1.1' score in model releases."
method: "An agent works in an isolated environment on a task prompt, and a hand-written verifier tests the resulting behaviour. Datacurve says its verifiers check software behaviour, not implementation details, and that behavioural verification needs a minimum amount of specificity in the prompt."
metric: "Resolved rate in percent: the share of tasks whose verifier passes."
format: "Agentic coding in Harbor task format: a prompt, an isolated environment and a program-based verifier per task."
limitations: "Only five languages are covered and most tasks are in three of them. Prompts are shorter than SWE-bench Pro's but still longer than the messages developers send, because behavioural tests need a floor of specificity. Vendor tables cite version 1.1, while the maker's pages describe the 113-task set without a changelog, so check which version a number refers to. Harness choice changes results; vendors run their own scaffolds."
citation: "https://deepswe.datacurve.ai/blog/deepswe"
tags: [coding, agent, long-horizon, open-source-repositories, datacurve]
updated: 2026-11-21
sources:
  - { url: "https://deepswe.datacurve.ai/blog/deepswe", title: "DeepSWE — Measuring frontier coding agents (Datacurve)", trust: 0.95 }
  - { url: "https://github.com/datacurve-ai/deep-swe", title: "datacurve-ai/deep-swe — repository", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/datacurve/deep-swe", title: "datacurve/deep-swe — Hugging Face dataset", trust: 0.9 }
related: [benchmark/swe-bench]
---

## What It Tests
Whether an agent can make **useful, well-scoped changes** across varied real codebases.

- **Breadth** — 113 tasks, 91 repositories, five languages.
- **Original tasks** — written from scratch, not taken from commits or pull requests.
- **Larger solutions** — about 5.5 times more code than SWE-bench Pro, per Datacurve.

## How It Scores
A **program-based verifier** per task, written by hand.

- **Behaviour first** — tests target what the software does, not how it is built.
- **Isolated** — each task has its own environment.
- **Result** — resolved or not, aggregated as a rate.

## Format
Harbor task format: a prompt plus an isolated environment plus a verifier. Prompts are shorter than SWE-bench Pro's.

## Limitations
- **Language coverage.** TypeScript, Go, Python, JavaScript and Rust only.
- **Prompt floor.** Behavioural tests need some specificity in the prompt.
- **Versions.** Vendors cite v1.1; the maker describes the 113-task set.

## Example
The dataset card and repository show the task layout in Harbor format: an **instruction**, an **environment**, and a **verifier** that exercises the finished software. The task prompt tells the agent the outcome wanted, and the verifier decides by running behavioural checks, not by diffing code.
