---
id: benchmark/deepplanning
type: benchmark
slug: deepplanning
schema_version: 3.1
title: "DeepPlanning"
name: "DeepPlanning"
category: Agent & Tool Use
description: "A long-horizon planning benchmark with code-verifiable constraints over travel-itinerary and shopping-cart tasks."
what_is_it: "DeepPlanning (Zhang, Jiang, Li, Tu and others, January 2026, Qwen Team, Alibaba Group) is a benchmark of multi-day travel planning and multi-product shopping tasks: 120 travel tasks, each in Chinese and English, and 120 English shopping tasks. Each task needs proactive information gathering through tools, local constrained reasoning and global constrained optimization, such as time and money budgets. Travel data come from real public sources for cities in China; shopping product data are synthesized. Plans are checked by code."
why_people_use_it: "The authors built it because most agent benchmarks emphasise local, step-level reasoning rather than global constrained optimization, and planning benchmarks underrepresent active information gathering and fine-grained local constraints (2026 paper)."
examples:
  - "From the paper's Appendix D travel prompt example: 'Can you create a travel plan for 2 people from Shanghai to Beijing, from Nov 4th to Nov 6th, 2025, one room, budget 10,000 RMB?'"
method: "Each task runs in an isolated sandbox the agent queries through APIs (9 for travel, 15 for shopping). Travel plans are checked by code on eight commonsense dimensions spanning 21 checkpoints (route consistency, sandbox compliance, itinerary structure, time feasibility, business hours compliance, duration rationality, cost calculation accuracy, activity diversity) plus the user's personalized constraints. Shopping carts are compared with a ground-truth cart."
metric: "Case accuracy: tasks that pass every check / all tasks (travel: perfect commonsense and personalized scores; shopping: every cart product exactly matches the ground truth). The paper also reports a commonsense score (8 dimensions worth 1/8 each), a personalized score and their average, the composite score, for travel, and a match score for shopping (matching products / ground-truth products). Results are averaged over four runs."
format: "Single-turn, multi-step planning with tool calls: 120 travel tasks (each in Chinese and English) and 120 English shopping tasks."
limitations: "The authors note that user queries are synthesized by adding constraints, which may shift them away from real user queries, and that the tasks are single-turn; multi-turn user-agent interaction is left for future work. Shopping product data are synthesized. Only two domains are covered."
citation: "https://arxiv.org/abs/2601.18137"
openness: open
openness_note: "The benchmark data are public, ungated, in the Qwen/DeepPlanning dataset on Hugging Face under the Apache-2.0 licence, and the checking method is described in the paper; no held-out set is described."
worked_examples:
  - task_type: "Long-horizon constrained planning (travel itinerary)"
    input: "Plan a 3-day city itinerary under a fixed total budget. Query the sandbox database (tools) for transport, lodging, attractions, and dining; every venue must be visited within its business hours and travel times must connect."
    success:
      answer: "query_transport(...) → query_attractions(...) → query_dining(...), then submit a minute-level schedule where each hop's travel time connects, every venue falls inside its opening hours, days stay diverse, and total cost lands under budget."
      why: "The plan is globally coherent — it passes every rule-based verifier (route consistency, time feasibility, business hours, cost accuracy, activity diversity)."
    failure:
      answer: "Picks four highly-rated attractions per day and a nice restaurant, but schedules two of them in the same 2–4pm slot and routes between venues 40km apart in 15 minutes; total cost also exceeds the budget by 12%."
      why: "Locally plausible choices, globally invalid: double-booked time slot, infeasible travel time, and over-budget total — the code checkers fail it on time feasibility, route consistency, and cost accuracy."
    checks: "Whether an agent produces a globally coherent plan under interlocking hard constraints, not just individually reasonable steps."
    scoring: "Accuracy — fraction of tasks whose final plan passes all code-based constraint checks."
  - task_type: "Constrained shopping cart (coupon-combination rules)"
    input: "Assemble a cart matching stated brand and size preferences and a shipping deadline, applying coupons to minimize cost — matched against a ground-truth optimal cart. Rule: coupon A ('15% off') and coupon B ('$10 off') cannot be stacked on the same item."
    success:
      answer: "query_products(brand, size) → compare per-item the better of coupon A vs B → apply exactly one coupon per item → submit the cart whose total equals the ground-truth optimal."
      why: "It respects the mutual-exclusion coupon rule while still minimizing cost, so every product in the cart matches the ground-truth cart."
    failure:
      answer: "Stacks both coupon A and coupon B on the priciest item to push the total lower than the reference cart."
      why: "Wrong action — violates the coupon-combination rule; the code checker invalidates the stack and the cart fails to match the ground-truth optimal."
    checks: "Whether an agent optimizes cost while obeying discrete combination rules on discounts."
    scoring: "Accuracy — cart must match the ground-truth optimal under the code checker."
  - task_type: "Proactive information gathering (sandbox compliance)"
    input: "Plan a day of sightseeing. All venues, prices, and opening hours must come from the sandbox database via tool queries — nothing may be invented."
    success:
      answer: "query_attractions(city) and query_dining(city) first, then build the schedule only from venues and hours returned by the sandbox."
      why: "Every item in the plan traces to a sandbox record, so it passes the sandbox-compliance verifier."
    failure:
      answer: "Skips the queries and fills the itinerary from the model's own knowledge of the city's famous museums and their 'usual' hours."
      why: "Acting without querying — the invented venues and hours are not in the sandbox, so the sandbox-compliance check fails even if the plan looks reasonable."
    checks: "Whether an agent grounds its plan in tool-retrieved facts rather than parametric guesses."
    scoring: "Accuracy — sandbox-compliance verifier must pass (all venues drawn from the database)."
  - task_type: "Constrained shopping cart (shipping-time limit)"
    input: "Buy a birthday gift matching a requested brand and size that must arrive within 3 days, minimizing cost. Query the sandbox for products and their shipping estimates."
    success:
      answer: "query_products(brand, size) → filter to items whose shipping estimate ≤ 3 days → pick the cheapest qualifying item → submit."
      why: "It enforces the hard shipping-deadline constraint before optimizing price, matching the ground-truth cart."
    failure:
      answer: "Picks the cheapest matching item overall, which ships in 6 days, because it is $8 less than the fastest qualifying option."
      why: "Wrong trade-off — optimizes cost while breaking the hard 3-day shipping constraint, so the checker fails the cart."
    checks: "Whether an agent treats a delivery deadline as a hard filter, not a soft preference to trade against price."
    scoring: "Accuracy — cart must satisfy the shipping-time constraint and match ground truth."
  - task_type: "Per-day activity diversity (travel)"
    input: "Plan a 2-day trip where each day must include a diverse mix of activity types (e.g. not two museums back-to-back), pulled from the sandbox and kept under budget."
    success:
      answer: "query_attractions(city) → schedule one museum, one outdoor site, and one dining spot per day, varying the type across slots so no day repeats a category."
      why: "The per-day mix satisfies the activity-diversity verifier while staying inside time and cost budgets."
    failure:
      answer: "Fills day one with three top-rated art museums in a row because they are the highest-rated venues available."
      why: "Violates the per-day activity-diversity constraint — locally optimal (highest ratings) but the diversity verifier fails the day."
    checks: "Whether an agent satisfies a diversity constraint instead of greedily picking the highest-rated items."
    scoring: "Accuracy — activity-diversity verifier must pass for every day."
tags: [agent, planning, tool-use, long-horizon, constraints]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2601.18137", title: "DeepPlanning: Benchmarking Long-Horizon Agentic Planning with Verifiable Constraints (Zhang et al., Qwen Team, Alibaba Group, 2026)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2601.18137", title: "DeepPlanning, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/Qwen/DeepPlanning", title: "Qwen/DeepPlanning dataset (Hugging Face, Apache-2.0)", trust: 0.9 }
related: [benchmark/tau2-bench, benchmark/vita-bench, benchmark/bfcl-v4]
---

## What It Tests
Whether an agent can make a **long-horizon plan that satisfies every constraint**. The paper names three competencies:

- **Proactive information acquisition** — find the facts through APIs instead of assuming them.
- **Local constrained reasoning** — each choice meets its own rules (opening hours, sizes, shipping times).
- **Global constrained optimization** — the whole plan fits time and money budgets.

## How It Scores
**Case accuracy**, computed by code: a task counts only when every check passes.

- **Travel** — eight commonsense dimensions with 21 checkpoints, each dimension worth 1/8 and awarded only if all its checkpoints pass, plus personalized constraints from the query.
- **Shopping** — every product in the cart must match the ground-truth cart; a match score (matching products / ground-truth products) is also reported.
- **Runs** — each task is run four times and results are averaged.

The eight travel dimensions: route consistency, sandbox compliance, itinerary structure, time feasibility, business hours, duration rationality, cost calculation accuracy and activity diversity.

## Format
Single-turn, multi-step planning in a sandbox. The agent queries, then submits one itinerary or cart.

| Domain | Tasks | APIs | Data source |
|---|---|---|---|
| Travel | 120, each in Chinese and English | 9 | Real public sources (Fliggy, Amap, web search), cities in China |
| Shopping | 120, English | 15 | Synthesized product data |

**Data** — Hugging Face `Qwen/DeepPlanning`, **Apache-2.0**.

## Limitations
- **Synthesized queries.** The authors say they may shift away from real user queries.
- **Single-turn.** Multi-turn interaction with a user is not tested.
- **Two domains.** Travel and shopping only; shopping product data are synthesized.

## Example
From the travel prompt example in the paper's Appendix D:

> "Can you create a travel plan for 2 people from Shanghai to Beijing, from Nov 4th to Nov 6th, 2025, one room, budget 10,000 RMB?"

- **Agent** — queries the sandbox for transport, hotels, attractions and restaurants, then submits a timed day-by-day plan.
- **Checkers** — code verifies, among other things, that each venue exists in the sandbox, is visited within its business hours and that costs add up within the budget.
- **Case accuracy** — the task counts only if every commonsense and personalized check passes.
