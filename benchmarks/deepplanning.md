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
      why: "It respects the mutual-exclusion coupon rule while still minimizing cost, so the cart matches the ground-truth optimal within the checker's tolerance."
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
updated: 2026-08-02
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
