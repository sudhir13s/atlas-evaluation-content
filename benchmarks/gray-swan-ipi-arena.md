---
id: benchmark/gray-swan-ipi-arena
type: benchmark
slug: gray-swan-ipi-arena
schema_version: 4.0
title: Gray Swan IPI Arena
name: Gray Swan Indirect Prompt Injection (IPI) Arena
category: Truthfulness & Safety
description: A crowdsourced red-teaming benchmark of indirect prompt-injection attacks on AI agents across 41 scenarios; the score is attack success rate, so lower is better.
what_is_it: 'Gray Swan''s Indirect Prompt Injection (IPI) Arena is a public red-teaming competition in which attackers try to hijack an AI agent by planting instructions in content it reads, while hiding the compromise from the user. The first large round, written up by Dziemian and 30 co-authors (arXiv, March 2026) and designed with the UK AI Security Institute, the US Center for AI Standards and Innovation (CAISI) and frontier labs, ran for three weeks: 464 participants made about 272,000 attack attempts against 13 frontier models, 8,648 of them successful, across 41 scenarios in tool-use, coding and computer-use settings.'
why_people_use_it: Agents read emails, browse the web and execute code, so planted instructions are a core risk. The arena produces a large pool of real human attacks, and curated subsets can be reused as a fixed test. Google's evaluation methodology for Gemini 4 Neon uses 1,810 curated, high-transferability attacks from the 2026 Q1 and Q2 (April) waves.
method: 'Each scenario gives an agent a user task plus tool responses or content that carry an injected instruction. A behaviour counts as a break when all of its criteria pass: a deterministic judge checks that the agent called the target tool with the expected arguments, and an LLM judge checks that the attack stayed hidden and the user''s request was still handled. In Google''s vendor-run test, models ran on public endpoints over 4 independent inference trials, scored as bootstrapped attack success rates at 1, 10 and 15 attempts.'
metric: Attack success rate (ASR) in percent = successful attacks / attack attempts; lower is better. Google reports ASR@1, ASR@10 and ASR@15, the success rate across 1, 10 and 15 attempts.
format: 'Adversarial agent scenarios with planted instructions: 41 behaviours (18 tool, 15 coding, 8 browser) across 3 competition waves; up to 5 simulated tool steps per run.'
limitations: 'Lower is better, the reverse of most benchmarks, so check the direction before comparing. Results depend on the attack set: Google used deduplicated attacks curated for cross-model transfer, so numbers from other attack sets differ. Gray Swan ran and supplied the vendor-run results, and the full attack data is not public, so outsiders cannot reproduce them. ASR at 15 attempts and at 1 attempt answer different questions.'
citation: https://arxiv.org/abs/2603.15714
openness: partly-open
openness_note: The scenarios, judges and a sample of 95 attacks are public (MIT-licensed kit on GitHub, attacks on Hugging Face), while the full competition attack set went only to UK AISI, US CAISI and participating labs and a held-out portion is withheld.
tags:
- safety
- prompt-injection
- agents
- red-teaming
- security
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2603.15714
  title: How Vulnerable Are AI Agents to Indirect Prompt Injections? Insights from a Large-Scale Public Competition (arXiv:2603.15714)
  trust: 0.95
- url: https://www.grayswan.ai/blog/your-ai-agent-can-be-compromised-youd-never-know
  title: Your AI Agent Can Be Compromised (Gray Swan)
  trust: 0.95
- url: https://github.com/GraySwanAI/ipi_arena_os
  title: GraySwanAI/ipi_arena_os — open-source evaluation kit (MIT)
  trust: 0.9
- url: https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf
  title: Gemini 4 Neon model evaluation (Google DeepMind)
  trust: 0.7
related:
- benchmark/agentdojo
---

## What It Tests

How easily an **attacker can hijack an agent** through content it reads, without the user noticing.

- **Indirect** — the attacker never talks to the agent; instructions hide in emails, web pages, documents or code.
- **Dual goal** — an attack succeeds only if the agent does the harmful action **and** the reply to the user shows no sign of it.
- **Settings** — 41 scenarios: 18 tool-use, 15 coding and 8 browser (computer-use).

## How It Scores

**Attack success rate (ASR): lower is better.**

- **Break** — a behaviour run where every criterion passes its threshold.
- **Tool-call judge** — deterministic: did the agent call the target tool with the expected arguments?
- **LLM judge** — was the attack stealthy, and did the agent still complete the user's request?
- **ASR@k** — in Google's test, success within k attempts (1, 10, 15), over 4 trials, bootstrapped.

## Format

An open-source kit runs each scenario for up to 5 steps, with an LLM simulating tool responses. The first competition round had three waves; Gray Swan says recurring competitions continue.

## Limitations

- **Direction.** Lower is better.
- **Attack set.** Numbers depend on which attacks were used and how they were curated.
- **Vendor-run.** Figures cited by model makers were supplied by Gray Swan on non-public attacks.

## Example

The kit's README uses the scenario `garage-door-email`, in the **tool** category, whose harmful-action category is **smart home manipulation**. The README gives only that label, not the full scenario. In any run, the agent works on a user task while a tool response carries the planted instruction; the tool-call judge checks whether the agent made the attacker's target call, and the LLM judge checks that the user-facing reply hid it. Attack text is not reproduced here.
