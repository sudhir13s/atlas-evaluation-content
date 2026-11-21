---
id: benchmark/agentharm
type: benchmark
slug: agentharm
schema_version: 1.0
title: "AgentHarm"
name: AgentHarm
category: Truthfulness & Safety
description: "A benchmark of 110 explicitly malicious agent tasks in 11 harm categories, measuring both refusal and whether a jailbroken agent can still complete multi-step tasks."
what_is_it: "AgentHarm measures the harmfulness of LLM agents, which use external tools and execute multi-stage tasks. It includes 110 explicitly malicious agent tasks, 440 with augmentations, covering 11 harm categories including fraud, cybercrime and harassment. Scoring well requires a jailbroken agent to keep its capabilities and complete the multi-step task, not just comply. The public release holds 44 of 66 test base behaviours (176 augmented) and 8 of 11 validation base behaviours (32 augmented)."
why_people_use_it: "Jailbreak robustness had mostly been studied for chatbots, while agents with tools may pose a greater risk if misused. AgentHarm lets researchers test attacks and defenses on agents with simple, reliable evaluation. The maker's disclaimer says its grades should not be read as an overall measure of agent safety."
method: "Models run as agents on the tasks, with and without jailbreaks, and results are graded for refusal and for completion of the harmful multi-step task. The repository uses the Inspect framework, and a canary string helps detect training contamination."
metric: "Harm score together with refusal; the dataset card reports average harm scores for direct requests without jailbreaks."
format: "Agentic tool-use tasks in 11 harm categories, with and without augmentations; Inspect-based evaluation."
limitations: "Only a subset is public (44 of 66 test base behaviours and 8 of 11 validation behaviours), and the card says the initial public set shifts average scores slightly. The data contain harmful content. The licence is MIT with an extra clause that restricts use to improving the safety and security of AI systems. Grades are not a general measure of agent safety, according to the maker."
citation: "https://arxiv.org/abs/2410.09024"
tags: [safety, agents, misuse, jailbreak, harmfulness]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2410.09024", title: "AgentHarm: A Benchmark for Measuring Harmfulness of LLM Agents (arXiv:2410.09024)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/ai-safety-institute/AgentHarm", title: "ai-safety-institute/AgentHarm — dataset card", trust: 0.9 }
related: [benchmark/harmbench, benchmark/agentdojo]
---

## What It Tests
Whether an **LLM agent refuses malicious multi-step tasks**, and what a jailbroken agent can still do.

- **Tasks** — 110 base tasks, 440 with augmentations.
- **Harm categories** — 11, including fraud, cybercrime and harassment.
- **Capability retention** — scoring well also needs coherent multi-step behaviour after an attack.

## How It Scores
A **harm score** plus refusal measurement.

- **Contamination watch** — a canary GUID marks the data.
- **Framework** — Inspect.

## Format
Agent tasks with tools, in base and augmented forms. The public release is partial: 44 of 66 test and 8 of 11 validation base behaviours.

## Limitations
- **Partial release.** Not all behaviours are public.
- **Harmful content.** Handle with care; use is restricted by an added licence clause.
- **Not a safety verdict.** The maker warns against reading it as overall agent safety.

## Example
The paper's findings show what the tasks reveal: leading models were **surprisingly compliant with malicious agent requests without jailbreaking**, and simple universal jailbreak templates adapted to agents enabled **coherent, malicious multi-step behaviour** that kept model capabilities. A task is therefore graded both on refusing and on how much of the harmful job gets done.
