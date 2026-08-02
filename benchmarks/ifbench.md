---
id: benchmark/ifbench
type: benchmark
slug: ifbench
schema_version: 1.0
title: "IFBench"
name: IFBench
category: Instruction Following & Chat
description: "A verifiable instruction-following benchmark of 300 prompts built on 58 new, held-out output constraints that models have not been trained to game."
what_is_it: "An Allen Institute for AI (AI2) and University of Washington benchmark that measures whether a model can follow precise, programmatically checkable output constraints it has never seen — 58 novel out-of-domain constraints paired with real user prompts, each verified by a Python function."
why_people_use_it: "It exposes overfitting: models ace older instruction tests like IFEval because they trained on those constraint types. IFBench's fresh, held-out constraints reveal whether instruction-following actually generalizes."
examples:
  - "Prompt: 'Explain photosynthesis. Every sentence must contain exactly one word in all capital letters, and the response must end with the exact phrase: That is all.' — a Python verifier checks both constraints mechanically."
method: "Combine 58 held-out verifiable constraints with prompts from a WildChat holdout to form 300 test items; a Python verification function per constraint checks the model's output automatically."
metric: "Prompt-level loose accuracy (fraction of prompts satisfying all constraints); strict and instruction-level variants also reported."
format: "Single-turn prompts, each carrying one or more verifiable output constraints; programmatic (no LLM judge)."
saturation: low
limitations: "Only constraints that a Python function can verify are in scope, so it tests format/structure compliance, not semantic quality or helpfulness. At 300 prompts it is small, and 'loose' vs 'strict' accuracy can differ by several points, so the reported variant matters."
citation: "https://arxiv.org/abs/2507.02833"
example:
  task_type: "Verifiable instruction following (stacked output constraints)"
  input: "Explain photosynthesis. Every sentence must contain exactly one word in all capital letters, and the response must end with the exact phrase: That is all."
  success:
    answer: "Plants use SUNLIGHT to turn water and carbon dioxide into sugar. This reaction releases OXYGEN as a byproduct. That is all."
    why: "Every sentence carries exactly one all-caps word and the response ends with the required phrase byte-for-byte — the Python verifier passes both checks."
  failure:
    answer: "Plants use SUNLIGHT to turn water and carbon dioxide into sugar. The GREEN pigment CHLOROPHYLL captures the light. This reaction releases OXYGEN as a byproduct. That is all."
    why: "✓ correct topic, ✓ ends with 'That is all.', ✗ the second sentence has two all-caps words (GREEN and CHLOROPHYLL) — the 'exactly one all-caps word per sentence' constraint is violated."
  checks: "Whether precise, held-out instruction-following generalizes to constraints the model was not trained to game."
  scoring: "Prompt-level loose accuracy — the prompt counts only if every constraint is satisfied."
tags: [instruction-following, verifiable, constraints, chat]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2507.02833", title: "Generalizing Verifiable Instruction Following (Pyatkin et al., 2025)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/allenai/IFBench_test", title: "IFBench test set (allenai/IFBench_test, 300 prompts)", trust: 0.9 }
  - { url: "https://github.com/allenai/IFBench", title: "IFBench GitHub repository", trust: 0.85 }
related: [benchmark/ifeval, benchmark/multichallenge]
---

## What It Tests
Whether precise instruction-following **generalizes** to constraints a model was not trained on. The
authors observed that models strongly overfit the small, fixed set of verifiable constraints in
benchmarks like IFEval, so high scores there no longer prove real skill.

IFBench answers that with **58 new, diverse output constraints** — collected from real user requests
or hand-written to cover core instruction-following skills — deliberately held out of common training
recipes.

## How It Scores
The primary metric is **prompt-level loose accuracy**: the fraction of prompts where the output
satisfies all of its constraints (with minor formatting leniency). Strict and per-instruction
variants are also reported.

Every constraint ships with a **Python verification function**, so scoring is fully programmatic and
reproducible — no LLM judge, no human rating, no ambiguity about whether a constraint was met.

## Format
Each of the 300 test items pairs a base prompt (drawn from a held-out slice of WildChat) with one or
more added output constraints — e.g. word counts, forbidden words, required phrases, casing or
structure rules. A constraint is only admitted if it can be checked by code, which is what keeps the
benchmark objective.

## Limitations
By construction it covers only **machine-verifiable** constraints — structure, format, lexical rules
— so it says nothing about whether the answer is correct, helpful, or well-written. At 300 prompts it
is small, and the gap between loose and strict accuracy can reach several points, so comparisons must
fix the same variant. It is also single-turn, unlike multi-turn tests such as MultiChallenge.

## Example
A prompt asks the model to explain a topic while obeying stacked constraints: "each sentence must
contain exactly one fully capitalized word" and "end with the exact phrase: That is all." The Python
verifier tokenizes the response, checks the all-caps count per sentence, and confirms the trailing
phrase byte-for-byte. A fluent, correct explanation still scores zero if it drops the closing phrase —
the benchmark rewards precise compliance, not content quality.
