---
id: benchmark/harmbench
type: benchmark
slug: harmbench
schema_version: 1.0
title: "HarmBench"
name: HarmBench
category: Truthfulness & Safety
description: "A standardized, open-source framework for evaluating automated red-teaming methods and the refusal robustness of language models."
what_is_it: "HarmBench is a framework for comparing automated red-teaming attacks and the defenses against them. Its authors (Mantas Mazeika and colleagues, paper submitted 2024-02-06) identified desirable properties that earlier red-teaming evaluations lacked and designed the benchmark to meet them. In the paper they use it to compare 18 red-teaming methods against 33 target models and defenses. The repository covers standard, contextual and multimodal harmful behaviours."
why_people_use_it: "The field lacked a standard way to judge new red-teaming methods, so results were not comparable. HarmBench provides one fast, scalable, open-source pipeline for attacks and defenses. The authors also introduce an adversarial training method that raises robustness across many attacks, showing how attacks and defenses can be developed together."
method: "The pipeline generates test cases with a red-teaming method, generates completions from a target model, and classifies them. The project releases classifier models for standard and contextual behaviours and for multimodal behaviours, plus a validation classifier."
metric: "Classifier-judged outcomes of attacks against target models and defenses. The paper's abstract and the repository do not name a single headline number."
format: "Behaviours (standard, contextual, multimodal) plus a pipeline of test-case generation, completion generation and classification; new attacks implement a red-teaming-method class."
limitations: "HarmBench evaluates refusal of harmful behaviours and attack methods, not overall model safety or helpfulness. Results rely on classifier models to judge harm, so classifier errors affect scores. The repository's roadmap lists planned additions (more attacks, targets and behaviours, and system-prompt attacks), so the benchmark has changed since release. The dataset contains harmful behaviour descriptions and is meant for safety research."
citation: "https://arxiv.org/abs/2402.04249"
tags: [safety, red-teaming, jailbreak, refusal, open-source]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2402.04249", title: "HarmBench: A Standardized Evaluation Framework for Automated Red Teaming and Robust Refusal (arXiv:2402.04249)", trust: 0.95 }
  - { url: "https://github.com/centerforaisafety/HarmBench", title: "centerforaisafety/HarmBench — repository", trust: 0.9 }
related: [benchmark/jailbreakbench, benchmark/strongreject, benchmark/agentharm]
---

## What It Tests
How well **attacks and defenses** perform against harmful-behaviour requests to language models.

- **Red-teaming methods** — 18 compared in the paper.
- **Targets** — 33 models and defenses.
- **Behaviour types** — standard, contextual and multimodal.

## How It Scores
A **classifier** judges completions.

- **Classifiers** — released models for standard and contextual behaviours, multimodal behaviours, and a validation classifier.
- **Pipeline** — generate test cases, generate completions, evaluate.

## Format
An open-source framework. New red-teaming methods and models plug in through configuration and a method class.

## Limitations
- **Narrow scope.** It tests refusal and attacks, not general safety.
- **Classifier-based.** Judging errors flow into scores.
- **Evolving.** The roadmap lists further additions.

## Example
In a typical run, a **red-teaming method** produces test cases for a harmful behaviour, the **target model** writes responses, and a **classifier** decides whether each response carries out the behaviour. Comparing methods and models on identical behaviours is the point of the standardization.
