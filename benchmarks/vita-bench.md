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
example:
  task_type: "Cross-scenario interactive agent (travel + food-delivery + in-store)"
  input: "User: 'I land in Kyoto Friday evening — find a well-reviewed ramen place near my hotel for that night, and have breakfast groceries delivered to the hotel for Saturday morning. Keep it under ¥8,000 total.' (66 tools available; no domain-specific policy scaffolding.)"
  success:
    answer: "resolve_hotel_location() → search_restaurants(near=hotel, cuisine='ramen', open_friday_evening=true) → reserve() → order_groceries(deliver_to=hotel, window='Sat AM') → running total kept under ¥8,000, asking the user to clarify the arrival time when it's under-specified."
    why: "The agent chains travel, in-store, and delivery tools over a long horizon, respects the shared budget and time/location context, and clarifies ambiguity instead of guessing."
  failure:
    answer: "search_restaurants(cuisine='ramen') → reserves a top-rated spot across the city from the hotel, then reports done — never places the grocery order and never sums the cost against the ¥8,000 budget."
    why: "Incorrect action: ignored the spatial constraint (restaurant not near the hotel), dropped the second sub-task (groceries), and never enforced the budget — a reasoning/interaction failure the rubric penalizes."
  checks: "Whether an agent composes tools across domains over a long horizon while tracking shifting intent, location, time, and budget."
  scoring: "Success rate — long-horizon trajectory graded by a rubric-based sliding-window evaluator that credits diverse valid solution paths."
tags: [agent, tool-use, multi-turn, interactive, real-world]
updated: 2026-07-20
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
