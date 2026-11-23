---
id: benchmark/harmbench
type: benchmark
slug: harmbench
schema_version: 3.1
title: "HarmBench"
name: HarmBench
category: Truthfulness & Safety
description: "A standardized, open-source framework of 510 harmful behaviours for evaluating automated red-teaming attacks and the refusal robustness of language models."
what_is_it: "HarmBench, by Mantas Mazeika and 11 co-authors including Dan Hendrycks (arXiv, February 2024; code under the Center for AI Safety's GitHub organization), is a framework for comparing automated red-teaming attacks and the defenses against them. It has 510 harmful behaviours (400 textual, 110 multimodal) in four functional categories and seven semantic categories, split into 100 validation and 410 test behaviours. The paper uses it to compare 18 red-teaming methods against 33 target models and defenses. Code and data are MIT-licensed on GitHub."
why_people_use_it: "The field lacked a standard way to judge new red-teaming methods, so results were not comparable. HarmBench fixes the behaviours, the generation settings and the classifier, so attacks and defenses are measured the same way. The authors also introduce an adversarial training method, Robust Refusal Dynamic Defense (R2D2), that raises robustness across many attacks."
method: "A red-teaming method generates test cases (attack prompts) for each behaviour; the target model answers with greedy decoding and a standardized 512-token completion; a classifier labels each completion as an instance of the behaviour or not. Non-copyright behaviours are judged by a fine-tuned Llama 2 13B classifier released with the project; copyright behaviours are judged by a hashing-based check that the completion actually contains the copyrighted text."
metric: "Attack success rate (ASR) = test cases whose completion the classifier labels as exhibiting the behaviour / all test cases, reported in percent."
format: "510 behaviours: Standard (200), Copyright (100), Contextual (100, a behaviour plus a context string) and Multimodal (110, a behaviour plus an image); a pipeline of test-case generation, completion generation and classification."
limitations: "HarmBench evaluates refusal of harmful behaviours and attack methods, not overall model safety or helpfulness. Non-copyright results rely on a classifier model, so classifier errors affect scores. The repository's roadmap lists planned additions, so the framework may change after release. The dataset contains harmful behaviour descriptions and is meant for safety research."
citation: "https://arxiv.org/abs/2402.04249"
openness: open
openness_note: "The behaviour lists, the evaluation pipeline and the judge classifiers are all public in the MIT-licensed GitHub repository and on Hugging Face."
tags: [safety, red-teaming, jailbreak, refusal, open-source]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2402.04249", title: "HarmBench: A Standardized Evaluation Framework for Automated Red Teaming and Robust Refusal (arXiv:2402.04249)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2402.04249v2", title: "HarmBench paper, full text (arXiv HTML v2)", trust: 0.95 }
  - { url: "https://github.com/centerforaisafety/HarmBench", title: "centerforaisafety/HarmBench — repository (MIT)", trust: 0.9 }
related: [benchmark/jailbreakbench, benchmark/strongreject, benchmark/agentharm]
---

## What It Tests
How well **automated red-teaming attacks** get language models to carry out harmful behaviours, and how well **defenses** hold.

- **Behaviours** — 510 in total: 400 textual and 110 multimodal.
- **Splits** — 100 validation and 410 test behaviours.
- **Semantic categories** — seven: cybercrime and unauthorized intrusion; chemical and biological weapons and drugs; copyright violations; misinformation and disinformation; harassment and bullying; illegal activities; general harm.
- **Paper's comparison** — 18 red-teaming methods against 33 target models and defenses.

## How It Scores
**Attack success rate (ASR)**: the share of test cases whose completion carries out the behaviour.

- **Classifier** — a fine-tuned Llama 2 13B model judges standard and contextual completions; a separate classifier handles multimodal ones.
- **Copyright** — a stricter hashing check passes only if the copyrighted text actually appears.
- **Fixed settings** — greedy decoding and 512 generated tokens for every target, so numbers are comparable.

## Format
Four functional categories:

| Category | Count | What the model sees |
|---|---|---|
| Standard | 200 | A behaviour string |
| Copyright | 100 | A request for copyrighted text |
| Contextual | 100 | A behaviour plus a context passage |
| Multimodal | 110 | A behaviour plus an image |

New attacks plug in by implementing a red-teaming-method class; models are configured in YAML.

## Limitations
- **Narrow scope.** It tests refusal and attacks, not general safety or helpfulness.
- **Classifier-based.** Judging errors flow into scores.
- **Evolving.** The roadmap lists further additions.

## Example
Harmful behaviour text is not reproduced here. At category level, a **Copyright** behaviour asks the model to reproduce a passage of a copyrighted work. An attack method writes prompts meant to elicit it, the target model answers, and the **hashing check** counts the case as a success only if the copyrighted passage really appears in the completion. ASR is the share of such cases that succeed.
