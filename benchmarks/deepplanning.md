---
id: benchmark/deepplanning
type: benchmark
slug: deepplanning
schema_version: 1.0
title: "DeepPlanning"
name: "DeepPlanning"
category: Agent & Tool Use
description: "A long-horizon planning benchmark with code-verifiable constraints over travel-itinerary and shopping-cart tasks."
what_is_it: "DeepPlanning tests whether an agent can produce a globally coherent plan under many interlocking hard constraints. Built by the Qwen team, it uses realistic multi-day travel-planning and multi-product shopping tasks that demand proactive information gathering (via tools), local constrained reasoning, and global constrained optimization over budgets, time windows, and cross-item dependencies — then verifies the whole plan with code, not an LLM judge."
why_people_use_it: "Most agent benchmarks reward step-level tool use but rarely test whether the *final plan* satisfies a web of global constraints. DeepPlanning isolates that skill and verifies it deterministically, exposing a real gap: even frontier agents produce plans that look locally fine but violate global budgets or dependencies."
examples:
  - "Travel: plan a multi-day itinerary that respects transport connections, attraction opening hours, minute-level scheduling, per-day activity diversity, and a total cost budget — pulling live options from a sandboxed database via tool calls."
  - "Shopping: assemble a cart that meets brand and size preferences, shipping-time limits, and coupon-combination rules while minimizing cost, matched against a ground-truth optimal cart."
method: "240 standalone tasks (120 travel, 120 shopping) each in an isolated sandbox database the agent queries with tools. Evaluation is code-based, not LLM-judged: travel plans are checked by rule-based verifiers across eight dimensions (route consistency, sandbox compliance, itinerary structure, time feasibility, business hours, duration rationality, cost accuracy, activity diversity); shopping plans are compared to a ground-truth cart."
metric: "Accuracy — the fraction of tasks whose final plan passes all code-based constraint checks. Even GPT-5.2-high reaches only ~44.6% average (Travel ~35.0%, Shopping ~54.2%)."
format: "Single-turn planning with tool-based information gathering against a sandboxed database."
saturation: low
limitations: "Only two domains (travel and shopping), so it is a focused planning probe rather than a general agent test. Queries are synthesized, which can create a distribution shift from real user requests. It targets single-turn planning, not multi-turn interactive replanning with a user. Code-based checkers are exact and reproducible but only score the dimensions they encode."
citation: "https://arxiv.org/abs/2601.18137"
tags: [agent, planning, tool-use, long-horizon, constraints]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2601.18137", title: "DeepPlanning: Benchmarking Long-Horizon Agentic Planning with Verifiable Constraints (Qwen Team, Alibaba, 2026)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/Qwen/DeepPlanning", title: "Qwen/DeepPlanning dataset (Hugging Face)", trust: 0.9 }
  - { url: "https://aclanthology.org/2026.acl-long.335/", title: "DeepPlanning — ACL 2026 Anthology", trust: 0.9 }
related: [benchmark/tau2-bench, benchmark/vita-bench, benchmark/bfcl-v4]
---

## What It Tests
Whether an agent can plan — really plan — under hard, interlocking constraints. The benchmark splits the
skill into three parts an agent must do together: **proactive information acquisition** (query a
sandboxed database with tools to find viable options), **local constrained reasoning** (each choice must
respect its own rules — opening hours, sizes, shipping times), and **global constrained optimization**
(the whole plan must fit a total time and money budget with cross-item dependencies). The insight is that
step-level correctness is not enough; the final plan has to be *globally* coherent.

## How It Scores
The metric is **accuracy** — the fraction of tasks whose final plan passes every constraint check —
computed by **code, not an LLM judge**, so it is exact and reproducible. Travel itineraries are run
through rule-based verifiers across eight dimensions: route consistency, sandbox compliance, itinerary
structure, time feasibility, business hours, duration rationality, cost accuracy, and activity
diversity. Shopping plans are compared against a ground-truth optimal cart. The results are stark: the
strongest configuration reported, **GPT-5.2-high, averages only ~44.6%** (~35.0% travel, ~54.2%
shopping), and non-reasoning models fare far worse — high component-level scores routinely mask failures
in global coherence.

## Format
Single-turn planning. The agent gathers information through tool calls against an isolated sandbox
database, then submits one plan that the checkers evaluate. There are **240 tasks total** — 120 travel
(Chinese and English variants) and 120 shopping.

## Limitations
The benchmark covers just two domains, travel and shopping, so it is a sharp probe of planning rather
than a general agent evaluation. Its user queries are synthesized, which can introduce a distribution
shift from real-world phrasing. It focuses on single-turn planning and does not test multi-turn
interactive replanning with a user in the loop. Code-based verification is a strength for
reproducibility, but it only measures the constraint dimensions the authors encoded — a plan could be
"good" in ways the checkers do not score, or pass the checks while being unnatural.

## Example
A travel task asks for a three-day city itinerary under a fixed budget. A correct plan queries the
sandbox for transport, lodging, attractions, and dining; schedules them at minute-level granularity so
travel times connect and every venue is visited within its business hours; keeps daily activities
diverse; and lands total cost under budget. An agent that picks individually plausible attractions but
double-books a time slot, routes impossibly between them, or overshoots the budget fails the code
checkers — which is exactly why even frontier models score below 50%.
