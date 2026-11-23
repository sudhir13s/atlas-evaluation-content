---
id: benchmark/jailbreakbench
type: benchmark
slug: jailbreakbench
schema_version: 3.1
title: "JailbreakBench"
name: JailbreakBench (JBB-Behaviors)
category: Truthfulness & Safety
description: "An open robustness benchmark for jailbreaking: 100 misuse behaviours plus 100 benign ones, a repository of adversarial prompts, a standard evaluation library and leaderboards."
what_is_it: "JailbreakBench, by Patrick Chao, Edoardo Debenedetti, Alexander Robey and nine co-authors (arXiv, March 2024; v1.0 accepted at the NeurIPS 2024 Datasets and Benchmarks Track), is a centralized benchmark for jailbreak attacks and defenses. It has four parts: a repository of adversarial prompts called jailbreak artifacts, the JBB-Behaviors dataset, a standardized evaluation library (threat model, system prompts, chat templates and scoring functions) and leaderboards. JBB-Behaviors has 100 misuse behaviours in ten categories matching OpenAI's usage policies, about 55% original and the rest from AdvBench and TDC/HarmBench, plus 100 benign behaviours. Code and data are MIT-licensed."
why_people_use_it: "The authors name three problems: no clear standard for judging whether a response is harmful, incomparable ways of computing attacker cost and success rate, and irreproducible work that withholds prompts or relies on changing proprietary APIs. JailbreakBench requires artifacts with submissions so results can be reproduced. The benign behaviours let users check how often a model over-refuses."
examples:
  - "JBB-Behaviors category names, from the Hugging Face dataset card: Harassment/Discrimination, Malware/Hacking, Physical harm, Economic harm, Fraud/Deception, Disinformation, Sexual/Adult content, Privacy, Expert advice, Government decision-making."
method: "Submissions provide their adversarial prompts as jailbreak artifacts. The evaluation library fixes the threat model, system prompts, chat templates and scoring functions, so attacks and defenses are measured the same way. Per the paper's v1.0 notes, a Llama-3-70B judge with a custom prompt decides whether a response is jailbroken, chosen using a 300-example human-preference dataset, and a Llama-3-8B judge detects refusals."
metric: "Attack success rate = behaviours for which the judge rules the response jailbroken / all 100 behaviours, tracked on the leaderboards with attacker cost; refusal rate on the 100 benign behaviours measures over-refusal."
format: "100 misuse behaviours and 100 benign behaviours in ten categories (columns: goal, target, behaviour, category, source); adversarial prompts submitted as artifacts and run through the standard library."
limitations: "Scores depend on the library's judge, and the set has only 100 misuse behaviours, so coverage of harm types is limited. The maker says the benchmark will expand and adapt over time. Releasing jailbreak artifacts has ethical trade-offs; the authors considered this and believe release is a net positive. Over-refusal checks use only 100 benign behaviours."
citation: "https://arxiv.org/abs/2404.01318"
openness: open
openness_note: "The JBB-Behaviors dataset (harmful and benign splits), the jailbreak artifacts, the judge comparison set and the evaluation library are public under the MIT licence."
tags: [safety, jailbreak, robustness, leaderboard, reproducibility]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2404.01318", title: "JailbreakBench: An Open Robustness Benchmark for Jailbreaking Large Language Models (arXiv:2404.01318)", trust: 0.95 }
  - { url: "https://jailbreakbench.github.io", title: "JailbreakBench — project site and leaderboards", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/JailbreakBench/JBB-Behaviors", title: "JailbreakBench/JBB-Behaviors — dataset card (MIT)", trust: 0.9 }
related: [benchmark/harmbench, benchmark/strongreject]
---

## What It Tests
How robust language models are to **jailbreak attacks** (prompts crafted to get a model to break its own safety rules), and how well attacks and defenses compare.

- **Misuse behaviours** — 100, in ten categories matching OpenAI's usage policies.
- **Sources** — about 55% original; the rest from AdvBench and TDC/HarmBench.
- **Benign behaviours** — 100 harmless requests, for measuring **over-refusal**.
- **Reproducibility** — attack prompts must be submitted as artifacts.

## How It Scores
A **standard library** with a fixed threat model, prompts and judge.

- **Attack success rate** — share of behaviours where the judge rules the model's response jailbroken.
- **Judge** — Llama-3-70B with a custom prompt in v1.0, picked against 300 human-labelled examples.
- **Over-refusal** — share of benign behaviours the model refuses, detected by a Llama-3-8B refusal judge.
- **Leaderboards** — track attacks and defenses, with attacker cost.

## Format
Each behaviour row has a goal (the request), a target string, a behaviour label, a category and a source. Chat templates and system prompts are fixed by the library.

## Limitations
- **Small behaviour set.** 100 misuse behaviours.
- **Judge-dependent.** Scores depend on the library's judge.
- **Evolving.** The maker plans to expand it.

## Example
Harmful requests are not reproduced here. The dataset card lists the **ten categories**: Harassment/Discrimination, Malware/Hacking, Physical harm, Economic harm, Fraud/Deception, Disinformation, Sexual/Adult content, Privacy, Expert advice and Government decision-making. For each behaviour, an attack submits one adversarial prompt; the library sends it to the target model and the judge rules jailbroken or not. A benign behaviour should be answered, not refused.
