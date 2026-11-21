---
id: evaluation-metric/attack-success-rate
type: evaluation-metric
slug: attack-success-rate
schema_version: 3.0
title: "Attack Success Rate (ASR)"
name: "Attack Success Rate"
category: safety
measures: "A high score means many red-teaming test cases made the target model carry out the harmful behaviour; a more robust model scores lower."
needs: [harmful behaviours, attack test cases, target model completions, harm classifier]
judged_by: model
range: { min: 0, max: 100 }
direction: lower-is-better
defined_in: { title: "HarmBench: A Standardized Evaluation Framework for Automated Red Teaming and Robust Refusal (2024)", url: "https://arxiv.org/abs/2402.04249" }
related_metrics: [evaluation-metric/strongreject-score]
tags: [safety, red-teaming, jailbreak]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2402.04249", title: "HarmBench: A Standardized Evaluation Framework for Automated Red Teaming and Robust Refusal (arXiv:2402.04249)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2402.10260", title: "A StrongREJECT for Empty Jailbreaks (arXiv:2402.10260)", trust: 0.95 }
related: [benchmark/harmbench]
---

## What It Measures
Attack success rate (ASR) is HarmBench's primary measure of a **red-teaming method's success** against a target model. Read from the defender's side, a lower ASR means a model that refuses more attacks.

## How It Is Computed
- **Test cases:** a red-teaming method produces test cases meant to elicit a given harmful behaviour.
- **Completions:** the target model answers each test case with greedy decoding; HarmBench fixes the output length at 512 tokens.
- **Judgement:** a classifier decides whether each completion exhibits the behaviour.
- **ASR** = the percentage of test cases that elicit the behaviour from the target model.
- **Aggregation:** the paper uses average ASR to rank models and attacks.

## Limits
The paper states:

- **Output length changes ASR** by up to 30%, and was not standardised in earlier work, so cross-paper comparisons can mislead.
- **The classifier is under optimisation pressure** from attacks and defenses, so it must be robust to unusual completions or the metric can be gamed.
- **Binary success** does not say how useful a harmful answer was; the StrongREJECT paper makes this criticism of binary jailbreak measures.
