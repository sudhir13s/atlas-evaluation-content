---
id: benchmark/jailbreakbench
type: benchmark
slug: jailbreakbench
schema_version: 1.0
title: "JailbreakBench"
name: JailbreakBench (JBB-Behaviors)
category: Truthfulness & Safety
description: "An open robustness benchmark for jailbreaking: 100 misuse behaviours, a repository of adversarial prompts, a standard evaluation library and leaderboards."
what_is_it: "JailbreakBench is a centralized benchmark for jailbreak attacks and defenses on language models. It has four parts: a repository of state-of-the-art adversarial prompts called jailbreak artifacts, a dataset named JBB-Behaviors, a standardized evaluation library with a defined threat model, system prompts, chat templates and scoring functions, and leaderboards for attacks and defenses. JBB-Behaviors has 100 distinct misuse behaviours in ten categories matching OpenAI's usage policies, 55% original and the rest from AdvBench and TDC/HarmBench, plus 100 benign behaviours."
why_people_use_it: "The authors name three problems: no clear standard for judging whether a response is harmful, incomparable ways of computing attacker cost and success rate, and irreproducible work that withholds prompts or relies on changing proprietary APIs. JailbreakBench requires artifacts for submissions to ensure reproducibility. The benign behaviours let users check how often a model over-refuses."
method: "Submissions provide the adversarial prompts as jailbreak artifacts. The evaluation library fixes the threat model, system prompts, chat templates and scoring functions, so attacks and defenses are measured the same way."
metric: "Attack success rates and costs tracked by the leaderboards for attacks and defenses; over-refusal rates on the benign behaviours."
format: "100 misuse behaviours (plus 100 benign) in ten categories; adversarial prompts submitted as artifacts and run through a standard library."
limitations: "Success depends on the scoring functions and judge in the library, and behaviours are limited to 100, so coverage of harm types is limited. The maker says the benchmark will evolve over time. Releasing jailbreak artifacts has ethical trade-offs; the authors considered this and believe release is a net positive. Over-refusal checks use only 100 benign behaviours."
citation: "https://arxiv.org/abs/2404.01318"
tags: [safety, jailbreak, robustness, leaderboard, reproducibility]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2404.01318", title: "JailbreakBench: An Open Robustness Benchmark for Jailbreaking Large Language Models (arXiv:2404.01318)", trust: 0.95 }
  - { url: "https://jailbreakbench.github.io", title: "JailbreakBench — project site and leaderboards", trust: 0.9 }
related: [benchmark/harmbench, benchmark/strongreject]
---

## What It Tests
How robust language models are to **jailbreak attacks**, and how effective attacks and defenses are.

- **Misuse behaviours** — 100, in ten categories matching OpenAI's usage policies.
- **Benign behaviours** — 100, for measuring over-refusal.
- **Reproducibility** — artifacts are required with submissions.

## How It Scores
A **standard library** with a fixed threat model and scoring functions.

- **Leaderboards** — track attacks and defenses across models.
- **Costs and success** — computed the same way for everyone.

## Format
Behaviours plus adversarial prompts as artifacts. Chat templates and system prompts are fixed by the library.

## Limitations
- **Small behaviour set.** 100 misuse behaviours.
- **Judge-dependent.** Scores depend on the library's scoring functions.
- **Evolving.** The maker plans to expand it.

## Example
JBB-Behaviors mixes **original examples (55%)** with ones from AdvBench and TDC/HarmBench. An attack submits adversarial prompts for these behaviours as jailbreak artifacts; the library then runs them against a model under the fixed threat model and scores the outcome. The benign set checks the opposite failure: refusing requests that should be answered.
