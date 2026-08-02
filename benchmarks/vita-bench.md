---
id: benchmark/vita-bench
type: benchmark
slug: vita-bench
schema_version: 1.0
title: "VITA-Bench"
name: "VitaBench"
category: Agent & Tool Use
description: "A hard, real-world agent benchmark with 66 tools across food delivery, in-store, and travel, scored by a rubric-based evaluator."
what_is_it: "VitaBench (Versatile Interactive Tasks Benchmark) tests LLM agents on messy, life-serving tasks drawn from food delivery, in-store consumption, and online travel. Each task hands the agent a rich environment — user profile, time-and-place context, and service databases — and 66 composable tools, then asks it to reason over long horizons, clarify ambiguous instructions, and track shifting user intent, with no domain-specific policy scaffolding to lean on."
why_people_use_it: "Most tool-use benchmarks are single-domain and policy-guided; VitaBench removes those crutches and composes cross-scenario tasks, so it is one of the most realistic and difficult interactive-agent tests available. Its error analysis pinpoints *where* agents fail (reasoning vs tool use vs interaction), which makes it useful for diagnosing agent weaknesses, not just ranking them."
examples:
  - "Cross-scenario: a user planning a trip asks the agent to book a restaurant near their hotel for the evening they arrive, order groceries delivered to the hotel, and stay within a budget — requiring the agent to chain travel, food-delivery, and in-store tools while reasoning over time, location, and the user's profile."
method: "Each task runs in an isolated environment with annotated user profiles, spatiotemporal context, and service databases; the agent interacts multi-turn with a simulated user and 66 tools. Long-horizon trajectories are graded by a rubric-based sliding-window evaluator that scores diverse valid solution paths rather than matching one gold trajectory. 100 cross-scenario tasks form the main results; 300 single-scenario tasks are also provided."
metric: "Success rate (task completion). Top models reach only ~30% on the 100 cross-scenario tasks and under 50% on the single-scenario sets."
format: "Multi-turn interactive dialogue with tool use in a simulated real-world environment."
saturation: low
limitations: "Scoring uses a rubric-based LLM evaluator, which is more flexible than exact match but introduces judge subjectivity and cost. The domains are life-services (food, retail, travel), so results do not transfer to coding, science, or web-research agents. The simulated user is an LLM, and the environments — though richly annotated — are synthetic, so real deployment behavior may differ."
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
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2509.26490", title: "VitaBench: Benchmarking LLM Agents with Versatile Interactive Tasks in Real-world Applications (He et al., 2025)", trust: 0.95 }
  - { url: "https://vitabench.github.io/", title: "VitaBench project page (code, dataset, leaderboard)", trust: 0.9 }
related: [benchmark/tau2-bench, benchmark/bfcl-v4, benchmark/deepplanning]
---

## What It Tests
Whether an agent can handle *versatile, real-world* interactive tasks — the kind of messy, multi-step
requests people actually make in food-delivery, in-store, and travel apps. Each task gives the agent a
lot to juggle at once: a user profile, spatiotemporal context (where and when), large service
databases, and **66 tools**, with the domain-specific policies deliberately stripped out so tools and
scenarios can be freely composed. The agent must reason over long horizons, ask clarifying questions
when instructions are ambiguous, and keep up as the user's intent shifts mid-conversation.

## How It Scores
The metric is **success rate**. Because a long interactive task has many valid solution paths, VitaBench
does not match against one gold trajectory; instead a **rubric-based sliding-window evaluator** grades
segments of the trajectory against criteria, accommodating diverse strategies and the stochasticity of a
simulated user. The results are humbling: leading models reach only about **30%** on the 100
cross-scenario tasks and **under 50%** on the single-scenario sets. The paper's error breakdown finds
reasoning errors dominate (~62%), followed by tool-usage errors (~21%) and interaction-management
failures (~8%), with agents showing poor self-awareness and weak error recovery.

## Format
Multi-turn interactive dialogue. The agent talks to a simulated user and operates the 66 tools inside an
isolated, richly annotated environment until the task is done or abandoned. 100 cross-scenario tasks are
the headline set; 300 single-scenario tasks provide finer-grained coverage.

## Limitations
Grading relies on a rubric-based LLM evaluator — more forgiving of alternate solutions than exact match,
but it adds judge subjectivity and evaluation cost. The scope is life-services (food, in-store, travel),
so scores do not generalize to coding, scientific, or open-web agents. The user is LLM-simulated and the
environments, while detailed, are synthetic, so behavior in real deployments can diverge.

## Example
A user says: "I land in Kyoto Friday evening — find me a well-reviewed ramen place near my hotel for
that night, and have breakfast groceries delivered to the hotel for Saturday morning, keep it under
¥8,000 total." A strong agent chains travel tools (resolve the hotel and arrival time), in-store/food
tools (search and reserve the restaurant, respecting opening hours and location), and delivery tools
(place a grocery order within the window and budget), asking for clarification when the request is
under-specified. The rubric evaluator credits partial progress and alternate valid orderings — and the
~30% top-model success rate shows how rarely agents get the whole chain right.
