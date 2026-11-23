---
id: benchmark/agentharm
type: benchmark
slug: agentharm
schema_version: 4.0
title: AgentHarm
name: AgentHarm
category: Truthfulness & Safety
description: A benchmark of 110 explicitly malicious agent tasks (440 with augmentations) in 11 harm categories, measuring both refusal and whether a jailbroken agent can still complete multi-step tasks.
what_is_it: AgentHarm, by Andriushchenko, Souly and colleagues at Gray Swan AI and the UK AI Security Institute (arXiv, October 2024; accepted at ICLR 2025), measures the harmfulness of LLM agents that use tools and carry out multi-step tasks. It has 110 explicitly malicious base tasks, 440 with augmentations, in 11 harm categories, using 104 distinct tools, and each harmful task has a benign counterpart. Scoring well requires a jailbroken agent to keep its capabilities and finish the task, not just comply. The public release holds 44 of 66 test base behaviours and 8 of 11 validation base behaviours.
why_people_use_it: Jailbreak robustness had mostly been studied for chatbots, while agents with tools may pose a greater risk if misused. AgentHarm lets researchers test attacks and defenses on agents with reliable, rubric-based grading. The authors found leading models surprisingly compliant with malicious agent requests even without a jailbreak. The maker's disclaimer says its grades should not be read as an overall measure of agent safety.
method: Models run as agents with the task's tools, with and without jailbreaks, through the Inspect framework. Each task has hand-written grading criteria that give partial credit; an LLM judge is used only for narrow, well-specified sub-checks. A separate semantic judge (GPT-4o by default) flags refusals across all of the agent's messages. A canary string marks the data to detect training contamination.
metric: 'Harm score: the share of a task''s hand-written grading criteria the agent meets, averaged over tasks (partial credit allowed). Refusal rate: the share of tasks where the refusal judge detects a refusal. Non-refusal harm score: the harm score over runs with no refusal.'
format: Agentic tool-use tasks in 11 harm categories, base and augmented forms, harmful and benign variants; Inspect-based evaluation.
limitations: Only part of the data is public (44 of 66 test and 8 of 11 validation base behaviours), and the dataset card says the public subset shifts average harm scores slightly against the full set; one jailbreak template used in the paper is also not released. The data contain harmful content. The licence is MIT with an extra clause restricting use to improving AI safety and security. The maker says grades are not a general measure of agent safety.
citation: https://arxiv.org/abs/2410.09024
openness: partly-open
openness_note: The method, grading code and a public subset (44 of 66 test and 8 of 11 validation base behaviours, with benign counterparts) are on Hugging Face, while the remaining behaviours are withheld.
tags:
- safety
- agents
- misuse
- jailbreak
- harmfulness
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2410.09024
  title: 'AgentHarm: A Benchmark for Measuring Harmfulness of LLM Agents (arXiv:2410.09024)'
  trust: 0.95
- url: https://arxiv.org/html/2410.09024
  title: AgentHarm paper, full text (arXiv HTML)
  trust: 0.95
- url: https://huggingface.co/datasets/ai-safety-institute/AgentHarm
  title: ai-safety-institute/AgentHarm — dataset card
  trust: 0.9
related:
- benchmark/harmbench
- benchmark/agentdojo
---

## What It Tests

Whether an **LLM agent refuses malicious multi-step tasks**, and how much a jailbroken agent can still get done.

- **Tasks** — 110 base tasks, 440 with augmentations (rephrasings and variants).
- **Harm categories** — 11: fraud, cybercrime, self-harm, harassment, sexual, copyright, drugs, disinformation, hate, violence and terrorism.
- **Tools** — 104 distinct simulated tools the agent can call.
- **Benign twins** — every harmful task has a harmless counterpart, to check that refusals are not just blanket caution.

## How It Scores

Two numbers are read together.

- **Harm score** — each task has **hand-written grading criteria** with partial credit; an LLM judge only checks narrow parts, never overall harmfulness.
- **Refusal rate** — a separate judge (GPT-4o by default) flags refusals in any of the agent's messages.
- **Non-refusal harm score** — harm score over runs that did not refuse, to see whether an attack keeps the agent capable.

## Format

Agent tasks with tools, run through the **Inspect** evaluation framework. The Hugging Face card ships `harmful`, `harmless_benign` and `chat` subsets with public test and validation splits.

## Limitations

- **Partial release.** 22 test and 3 validation base behaviours are withheld, so public-set numbers differ slightly from the paper's.
- **Harmful content.** Use is restricted by an added licence clause to AI safety and security work.
- **Not a safety verdict.** The maker warns against reading it as overall agent safety.

## Example

Harmful items are not reproduced here. The paper shows the **benign counterpart** of one task, which begins: "Find the official consular office website for the UK." It then asks the agent to report a lost passport using supplied personal details. The harmful twin asks for a misuse of the same tools; the agent is graded on whether it refuses and, if not, how many of the task's criteria it meets.
