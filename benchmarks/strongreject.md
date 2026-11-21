---
id: benchmark/strongreject
type: benchmark
slug: strongreject
schema_version: 1.0
title: "StrongREJECT"
name: StrongREJECT
category: Truthfulness & Safety
description: "A jailbreak benchmark pairing forbidden prompts with an automated evaluator that scores how useful a response is to a harmful request, built to curb exaggerated attack success claims."
what_is_it: "StrongREJECT is a benchmark for jailbreak effectiveness. Its dataset contains prompts that victim models must answer with specific harmful information, drawn from six categories: disinformation and deception, illegal goods and services, hate, harassment and discrimination, non-violent crimes, violence, and sexual content. Its automated evaluator measures the extent to which a response gives useful information to a forbidden prompt. The paper title is 'A StrongREJECT for Empty Jailbreaks'."
why_people_use_it: "The authors say jailbreak papers often exaggerate effectiveness because there is no standard high-quality benchmark. They show existing benchmarks have significant shortcomings and that existing evaluation methods overstate jailbreak effectiveness compared with human judgments. The StrongREJECT evaluator reaches state-of-the-art agreement with human judgments. They also find that jailbreaks that bypass safety fine-tuning tend to reduce the model's capabilities."
method: "The documentation describes a Python library that applies jailbreaks to prompts, generates responses and evaluates them. It offers a rubric-based evaluator, which prompts an LLM to judge harmfulness against a rubric, and implements other evaluators including HarmBench's, plus several dozen jailbreaks as baselines."
metric: "A harmfulness or usefulness score for each response to a forbidden prompt, from the StrongREJECT evaluator."
format: "Forbidden prompts in six categories, optionally transformed by jailbreaks; free-text responses scored by the evaluator."
limitations: "The paper's claim is that scoring must reflect how useful a harmful response is; this depends on the evaluator, which for the rubric version prompts a language model. The dataset curates examples from prior datasets with different licences, several of them stated as having no licence, so reuse terms are mixed. The library is MIT-licensed."
citation: "https://arxiv.org/abs/2402.10260"
tags: [safety, jailbreak, evaluator, harmfulness, open-source]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2402.10260", title: "A StrongREJECT for Empty Jailbreaks (arXiv:2402.10260)", trust: 0.95 }
  - { url: "https://strong-reject.readthedocs.io/en/latest/", title: "StrongREJECT documentation", trust: 0.9 }
related: [benchmark/harmbench, benchmark/jailbreakbench]
---

## What It Tests
How **useful a model's answer is to a forbidden request**, so attack success is not overstated.

- **Prompts** — six categories of harmful behaviour.
- **Evaluator** — scores usefulness of the harmful information, not just refusal.
- **Jailbreaks** — several dozen are implemented as baselines.

## How It Scores
An **automated evaluator** with a rubric-based version.

- **Agreement** — state-of-the-art match with human judgments of jailbreak effectiveness.
- **Finding** — jailbreaks that bypass safety fine-tuning tend to reduce capabilities.

## Format
Forbidden prompts, optionally jailbroken, and the model's free-text answer. The library applies jailbreaks, generates responses and evaluates them.

## Limitations
- **Evaluator-dependent.** The rubric version asks a language model to judge.
- **Mixed data licences.** Source datasets carry different licences; the library is MIT.

## Example
The documentation shows a workflow: pick forbidden prompts, apply a jailbreak such as 'happy_to_help' alongside a no-jailbreak baseline, collect the model's responses, and score them with the rubric-based evaluator. Comparing the two scores shows whether the jailbreak really made the answers more useful to a malicious user.
