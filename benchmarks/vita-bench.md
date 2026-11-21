---
id: benchmark/vita-bench
type: benchmark
slug: vita-bench
schema_version: 1.0
title: "VitaBench"
name: "VitaBench"
category: Agent & Tool Use
description: "An interactive agent benchmark with 66 tools across food delivery, in-store consumption and online travel, scored by a rubric-based sliding-window evaluator."
what_is_it: "VitaBench (He et al., 2025, Meituan LongCat Team) tests LLM agents on interactive tasks drawn from food delivery, in-store consumption and online travel services, using 66 tools. A framework without domain-specific policies lets scenarios and tools be composed into 100 cross-scenario tasks (the main results) and 300 single-scenario tasks. Each task is derived from multiple real user requests."
why_people_use_it: "Tasks require reasoning across time and place, using complex tool sets, clarifying ambiguous instructions and tracking shifting user intent over multi-turn conversations. In the paper (2025), the best model reached about 30% success on cross-scenario tasks and under 50% on single-scenario tasks; its error analysis attributes 61.8% of failures to reasoning, 21.1% to tool use and 7.9% to interaction management."
examples:
  - "A user request that spans scenarios (for example travel and food delivery), handled over several turns with a simulated user and the 66 tools."
method: "The agent talks with a simulated user over multiple turns and calls tools in the environment. A rubric-based sliding-window evaluator assesses the long trajectory, allowing diverse valid solution paths."
metric: "Success rate (task completion)."
format: "Multi-turn interactive dialogue with tool use; 100 cross-scenario and 300 single-scenario tasks."
limitations: "Grading relies on a rubric-based LLM evaluator, and the user is simulated by an LLM, so both affect results. The domains are life services (food delivery, in-store, travel)."
citation: "https://arxiv.org/abs/2509.26490"
worked_examples:
  - task_type: "Cross-scenario interactive agent (travel + food-delivery + in-store)"
    input: "User: 'I land in Kyoto Friday evening — find a well-reviewed ramen place near my hotel for that night, and have breakfast groceries delivered to the hotel for Saturday morning. Keep it under ¥8,000 total.' (66 tools available; no domain-specific policy scaffolding.)"
    success:
      answer: "resolve_hotel_location() → search_restaurants(near=hotel, cuisine='ramen', open_friday_evening=true) → reserve() → order_groceries(deliver_to=hotel, window='Sat AM') → running total kept under ¥8,000, asking the user to clarify the arrival time when it's under-specified."
      why: "The agent chains travel, in-store, and delivery tools over a long horizon, respects the shared budget and time/location context, and clarifies ambiguity instead of guessing."
    failure:
      answer: "search_restaurants(cuisine='ramen') → reserves a top-rated spot across the city from the hotel, then reports done — never places the grocery order and never sums the cost against the ¥8,000 budget."
      why: "Incorrect action: ignored the spatial constraint (restaurant not near the hotel), dropped the second sub-task (groceries), and never enforced the budget — a reasoning/interaction failure the rubric penalizes."
    checks: "Whether an agent composes tools across domains over a long horizon while tracking shifting intent, location, time, and budget."
    scoring: "Success rate — long-horizon trajectory graded by a rubric-based sliding-window evaluator that credits diverse valid solution paths."
  - task_type: "Shifting user intent (food delivery)"
    input: "User: 'Order me a large pepperoni pizza.' Then mid-conversation: 'Actually make it two, and one of them needs to be gluten-free — and add garlic bread.'"
    success:
      answer: "Start build_order(item='large pepperoni') → on the update, revise to two pizzas (one gluten-free crust) + garlic bread → confirm the revised cart → submit_order()."
      why: "The agent tracks the intent change, updates the in-progress order to all three items with the gluten-free variant, and confirms before submitting."
    failure:
      answer: "submit_order() with the single original pepperoni pizza, treating the follow-up as noise."
      why: "Failure to track shifting intent: it ignored the mid-conversation revision, so the final order misses a pizza, the gluten-free requirement, and the garlic bread — the rubric penalizes the dropped updates."
    checks: "Whether an agent updates an in-progress task when the user changes their request mid-conversation."
    scoring: "Success rate — rubric credits the trajectory that reflects the user's latest intent."
  - task_type: "Clarifying an ambiguous request (in-store)"
    input: "User: 'Grab me a coffee from the cafe.' Size, milk, and hot/iced are unspecified, and the menu offers several options."
    success:
      answer: "Ask: 'What size, and hot or iced — any milk preference?' → user answers 'medium iced oat-milk latte' → order_item(size='medium', drink='iced latte', milk='oat')."
      why: "The agent resolves the under-specified request by asking, then orders exactly what the user wanted — the rubric rewards the clarification step."
    failure:
      answer: "order_item(drink='hot latte', size='large', milk='whole') as a default guess without asking."
      why: "Acting on assumptions: it guessed size, temperature, and milk instead of clarifying, and the rubric penalizes the unrequested defaults when they diverge from the user's intent."
    checks: "Whether an agent asks a clarifying question on an ambiguous order instead of guessing defaults."
    scoring: "Success rate — rubric credits eliciting the missing details before ordering."
  - task_type: "Budget tracking across bookings (travel)"
    input: "User: 'Book me a hotel and a return train for next weekend, total under ¥30,000.' Several hotels and train fares are available in the environment."
    success:
      answer: "search_trains(round_trip) → pick a ¥9,000 fare → search_hotels(budget_remaining=21000) → reserve a ¥18,000 hotel → running total ¥27,000, under the cap → confirm both."
      why: "The agent tracks a running total across two bookings and keeps the combined cost under the ¥30,000 budget."
    failure:
      answer: "Reserves a ¥26,000 boutique hotel first because it is top-rated, then books a ¥9,000 train, ending at ¥35,000."
      why: "Budget not tracked globally: each booking looked fine alone, but the combined total overshoots the ¥30,000 cap — a global-reasoning failure the rubric marks down."
    checks: "Whether an agent maintains a cross-booking budget rather than optimizing each booking in isolation."
    scoring: "Success rate — rubric requires the final plan to respect the stated total budget."
  - task_type: "Error recovery (cross-scenario)"
    input: "User: 'Reserve the sushi place I picked for 8pm.' The reservation tool returns 'fully booked at 8pm.' Other nearby restaurants and times are available."
    success:
      answer: "reserve(restaurant, 20:00) → 'fully booked' → tell the user and offer options: 'It's full at 8 — I can do 8:45 there, or a similar sushi spot nearby at 8. Which?' → reserve the chosen alternative."
      why: "The agent recognizes the failed action, recovers with concrete alternatives, and keeps the user in the loop — strong self-awareness and recovery."
    failure:
      answer: "reserve(restaurant, 20:00) → 'fully booked' → replies 'Sorry, I couldn't make the reservation' and stops."
      why: "Poor error recovery: it surfaces the failure but neither retries a nearby time nor proposes an alternative, so the task is left unresolved — the exact weakness the paper's error analysis flags."
    checks: "Whether an agent recovers from a failed tool call by finding a valid alternative instead of giving up."
    scoring: "Success rate — rubric credits recovering to a completed reservation over abandoning the task."
tags: [agent, tool-use, multi-turn, interactive, real-world]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2509.26490", title: "VitaBench: Benchmarking LLM Agents with Versatile Interactive Tasks in Real-world Applications (He et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2509.26490", title: "VitaBench, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://vitabench.github.io/", title: "VitaBench project page (code, dataset, leaderboard)", trust: 0.9 }
related: [benchmark/tau2-bench, benchmark/bfcl-v4, benchmark/deepplanning]
---

## What It Tests
Whether an agent can complete **interactive real-world tasks** in food delivery, in-store consumption and online travel.

- **66 tools**, composable across scenarios without domain-specific policies.
- **Skills** — reasoning over time and place, clarifying ambiguous instructions, tracking shifting intent.

## How It Scores
**Success rate.** A rubric-based sliding-window evaluator assesses the long trajectory, so different valid solution paths can pass.

- **Paper's results (2025)** — about 30% on cross-scenario and under 50% on single-scenario tasks for the best model.
- **Error analysis** — reasoning 61.8%, tool use 21.1%, interaction management 7.9%.

## Format
Multi-turn dialogue with a simulated user and tool calls in the environment: 100 cross-scenario tasks (main results) and 300 single-scenario tasks.

## Limitations
- **LLM in the loop.** A rubric-based LLM evaluator and an LLM-simulated user.
- **Life services only.** Food delivery, in-store and travel.

## Example
A task derived from real user requests is given to the agent through the simulated user; the agent asks for missing details, calls the scenario tools, and the evaluator checks the trajectory against the task's rubric. Appendix C of the paper shows a full example trajectory.
