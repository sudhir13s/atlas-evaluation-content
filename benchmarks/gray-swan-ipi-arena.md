---
id: benchmark/gray-swan-ipi-arena
type: benchmark
slug: gray-swan-ipi-arena
schema_version: 1.0
title: "Gray Swan IPI Arena"
name: Gray Swan Indirect Prompt Injection (IPI) Arena
category: Truthfulness & Safety
description: "A crowdsourced red-teaming benchmark of indirect prompt-injection attacks on AI agents; the score is attack success rate, so lower is better."
what_is_it: "Gray Swan's Indirect Prompt Injection (IPI) Arena is a public red-teaming competition in which attackers try to hijack an AI agent without the user noticing, by planting instructions in content the agent reads. The first large round, designed with the UK AI Security Institute, the US Center for AI Standards and Innovation (CAISI) and frontier labs, tested 13 frontier models with 464 red teamers and more than 272,000 attacks. Scenarios span realistic coding, computer-use and tool-use tasks."
why_people_use_it: "Agents read emails, browse the web and execute code, so planted instructions are a core risk. The arena produces a large pool of real human attacks, and curated subsets can be reused as a fixed test. Google's evaluation methodology for Gemini 4 Neon uses 1,810 curated, high-transferability attacks from the 2026 Q1 and Q2 waves."
method: "In the vendor-run test, attacks target irreversible harmful actions, including private data exfiltration, data destruction, system compromise and unintended financial transactions. Models run on public endpoints over 4 independent inference trials. Scores are bootstrapped attack success rates at 1, 10 and 15 attempts (ASR@1, ASR@10, ASR@15)."
metric: "Attack success rate (ASR) in percent. Lower is better. ASR counts the share of attempts that succeed; a break rate counts the share of scenarios with at least one successful attempt, as another group's write-up explains."
format: "Adversarial agent scenarios with planted instructions; attacks drawn from competition waves; success judged by whether the harmful action happened."
limitations: "Lower is better, which is the reverse of most benchmarks, so check the direction before comparing. Results depend on the attack set: Google used attacks from 2026 Q1 and Q2 after deduplication and curation for cross-model transfer, so numbers from other attack sets differ. Gray Swan supplied the vendor-run results, so they are not independently reproduced. Attack success at 15 attempts and at 1 attempt answer different questions."
citation: "https://www.grayswan.ai/blog/your-ai-agent-can-be-compromised-youd-never-know"
tags: [safety, prompt-injection, agents, red-teaming, security]
updated: 2026-11-21
sources:
  - { url: "https://www.grayswan.ai/blog/your-ai-agent-can-be-compromised-youd-never-know", title: "Your AI Agent Can Be Compromised (Gray Swan)", trust: 0.9 }
  - { url: "https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf", title: "Gemini 4 Neon model evaluation (Google DeepMind)", trust: 0.7 }
  - { url: "https://reflection.ai/blog/advancing-open-model-safety-gray-swan", title: "Advancing open-model safety and agent security (Reflection)", trust: 0.7 }
related: [benchmark/agentdojo]
---

## What It Tests
How easily an **attacker can hijack an agent** through content it reads, without the user noticing.

- **Indirect** — instructions hide in web pages, emails, documents or code.
- **Harmful actions** — exfiltration, destruction, compromise and unintended payments.
- **Settings** — coding, computer use and tool use.

## How It Scores
**Attack success rate (ASR): lower is better.**

- **ASR@k** — success within k attempts (1, 10, 15 in Google's test).
- **Break rate** — scenarios with at least one success.
- **Trials** — four independent inference trials, bootstrapped.

## Format
Curated attacks from public competition waves, run against models on public endpoints. The competition was built with UK and US government institutes and frontier labs.

## Limitations
- **Direction.** Lower is better.
- **Attack set.** Numbers depend on which attacks were used and how they were curated.
- **Vendor-run.** The figures cited by vendors were supplied by Gray Swan.

## Example
A scenario places a hidden instruction inside content an agent processes, such as a web page, email, document or code file. The attack succeeds if the agent **performs the harmful action** the instruction asks for, for example sending private data out or making an unintended payment. ASR@15 asks how often at least one of fifteen attempts gets through.
