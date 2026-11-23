---
id: benchmark/vita-bench
type: benchmark
slug: vita-bench
schema_version: 4.0
title: VitaBench
name: VitaBench
category: Agent & Tool Use
description: An interactive agent benchmark with 66 tools across food delivery, in-store consumption and online travel, scored by a rubric-based sliding-window evaluator.
what_is_it: 'VitaBench (He et al., September 2025, Meituan LongCat Team; ICLR 2026) tests LLM agents on interactive tasks drawn from food delivery, in-store consumption and online travel (OTA, online travel agency) services, using 66 tools. A framework without domain-specific policies lets scenarios and tools be composed into 400 tasks: 100 cross-scenario tasks (the main results) and 100 single-scenario tasks in each of delivery, in-store and travel. Each task is derived from multiple real user requests.'
why_people_use_it: Tasks require reasoning across time and place, using complex tool sets, clarifying ambiguous instructions and tracking shifting user intent over multi-turn conversations. The paper's error analysis of 76 failed rubric items from one model attributes 61.8% to reasoning, 21.1% to tool use, 7.9% to interaction and 9.2% to the user simulator. The tasks and code are public under the MIT licence.
examples:
- 'From the paper''s Appendix C example trajectory, a simulated user turn: ''We have 6 people total, and we want to book a table for 12 noon on the 27th.'''
method: The agent talks with a simulated user over multiple turns and calls tools in the environment. A rubric-based sliding-window evaluator reads the trajectory in overlapping windows (10 turns with 2 overlapping) and records which rubric items are satisfied; an item once satisfied stays satisfied, which allows diverse valid solution paths. Each task is run four times.
metric: 'Success rate: a task succeeds only if all its rubric items are satisfied. The paper reports Avg@4 (mean success over four runs), Pass@k (probability that at least one of k runs succeeds) and Pass^k (probability that all k runs succeed).'
format: Multi-turn interactive dialogue with tool use; 100 cross-scenario and 300 single-scenario tasks.
limitations: Grading relies on a rubric-based LLM evaluator, and the user is simulated by an LLM, so both affect results; the paper itself attributes 9.2% of analysed failures to the user simulator. The domains are life services (food delivery, in-store, travel). All-or-nothing scoring means one missed rubric item fails a task.
citation: https://arxiv.org/abs/2509.26490
openness: open
openness_note: The 400 tasks, tools, environment data and evaluation code are public on Hugging Face (meituan-longcat/VitaBench) and GitHub under the MIT licence, with no held-out set described.
worked_examples:
- task_type: Cross-scenario interactive agent (travel + food-delivery + in-store)
  input: 'User: ''I land in Kyoto Friday evening — find a well-reviewed ramen place near my hotel for that night, and have breakfast groceries delivered to the hotel for Saturday morning. Keep it under ¥8,000 total.'' (66 tools available; no domain-specific policy scaffolding.)'
  success:
    answer: resolve_hotel_location() → search_restaurants(near=hotel, cuisine='ramen', open_friday_evening=true) → reserve() → order_groceries(deliver_to=hotel, window='Sat AM') → running total kept under ¥8,000, asking the user to clarify the arrival time when it's under-specified.
    why: The agent chains travel, in-store, and delivery tools over a long horizon, respects the shared budget and time/location context, and clarifies ambiguity instead of guessing.
  failure:
    answer: search_restaurants(cuisine='ramen') → reserves a top-rated spot across the city from the hotel, then reports done — never places the grocery order and never sums the cost against the ¥8,000 budget.
    why: 'Incorrect action: ignored the spatial constraint (restaurant not near the hotel), dropped the second sub-task (groceries), and never enforced the budget — a reasoning/interaction failure the rubric penalizes.'
  checks: Whether an agent composes tools across domains over a long horizon while tracking shifting intent, location, time, and budget.
  scoring: Success rate — long-horizon trajectory graded by a rubric-based sliding-window evaluator that credits diverse valid solution paths.
- task_type: Shifting user intent (food delivery)
  input: 'User: ''Order me a large pepperoni pizza.'' Then mid-conversation: ''Actually make it two, and one of them needs to be gluten-free — and add garlic bread.'''
  success:
    answer: Start build_order(item='large pepperoni') → on the update, revise to two pizzas (one gluten-free crust) + garlic bread → confirm the revised cart → submit_order().
    why: The agent tracks the intent change, updates the in-progress order to all three items with the gluten-free variant, and confirms before submitting.
  failure:
    answer: submit_order() with the single original pepperoni pizza, treating the follow-up as noise.
    why: 'Failure to track shifting intent: it ignored the mid-conversation revision, so the final order misses a pizza, the gluten-free requirement, and the garlic bread — the rubric penalizes the dropped updates.'
  checks: Whether an agent updates an in-progress task when the user changes their request mid-conversation.
  scoring: Success rate — rubric credits the trajectory that reflects the user's latest intent.
- task_type: Clarifying an ambiguous request (in-store)
  input: 'User: ''Grab me a coffee from the cafe.'' Size, milk, and hot/iced are unspecified, and the menu offers several options.'
  success:
    answer: 'Ask: ''What size, and hot or iced — any milk preference?'' → user answers ''medium iced oat-milk latte'' → order_item(size=''medium'', drink=''iced latte'', milk=''oat'').'
    why: The agent resolves the under-specified request by asking, then orders exactly what the user wanted — the rubric rewards the clarification step.
  failure:
    answer: order_item(drink='hot latte', size='large', milk='whole') as a default guess without asking.
    why: 'Acting on assumptions: it guessed size, temperature, and milk instead of clarifying, and the rubric penalizes the unrequested defaults when they diverge from the user''s intent.'
  checks: Whether an agent asks a clarifying question on an ambiguous order instead of guessing defaults.
  scoring: Success rate — rubric credits eliciting the missing details before ordering.
- task_type: Budget tracking across bookings (travel)
  input: 'User: ''Book me a hotel and a return train for next weekend, total under ¥30,000.'' Several hotels and train fares are available in the environment.'
  success:
    answer: search_trains(round_trip) → pick a ¥9,000 fare → search_hotels(budget_remaining=21000) → reserve a ¥18,000 hotel → running total ¥27,000, under the cap → confirm both.
    why: The agent tracks a running total across two bookings and keeps the combined cost under the ¥30,000 budget.
  failure:
    answer: Reserves a ¥26,000 boutique hotel first because it is top-rated, then books a ¥9,000 train, ending at ¥35,000.
    why: 'Budget not tracked globally: each booking looked fine alone, but the combined total overshoots the ¥30,000 cap — a global-reasoning failure the rubric marks down.'
  checks: Whether an agent maintains a cross-booking budget rather than optimizing each booking in isolation.
  scoring: Success rate — rubric requires the final plan to respect the stated total budget.
- task_type: Error recovery (cross-scenario)
  input: 'User: ''Reserve the sushi place I picked for 8pm.'' The reservation tool returns ''fully booked at 8pm.'' Other nearby restaurants and times are available.'
  success:
    answer: 'reserve(restaurant, 20:00) → ''fully booked'' → tell the user and offer options: ''It''s full at 8 — I can do 8:45 there, or a similar sushi spot nearby at 8. Which?'' → reserve the chosen alternative.'
    why: The agent recognizes the failed action, recovers with concrete alternatives, and keeps the user in the loop — strong self-awareness and recovery.
  failure:
    answer: reserve(restaurant, 20:00) → 'fully booked' → replies 'Sorry, I couldn't make the reservation' and stops.
    why: 'Poor error recovery: it surfaces the failure but neither retries a nearby time nor proposes an alternative, so the task is left unresolved — the exact weakness the paper''s error analysis flags.'
  checks: Whether an agent recovers from a failed tool call by finding a valid alternative instead of giving up.
  scoring: Success rate — rubric credits recovering to a completed reservation over abandoning the task.
tags:
- agent
- tool-use
- multi-turn
- interactive
- real-world
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2509.26490
  title: 'VitaBench: Benchmarking LLM Agents with Versatile Interactive Tasks in Real-world Applications (He et al., 2025)'
  trust: 0.95
- url: https://arxiv.org/html/2509.26490
  title: VitaBench, full text (arXiv HTML)
  trust: 0.95
- url: https://vitabench.github.io/
  title: VitaBench project page (code, dataset, leaderboard)
  trust: 0.95
- url: https://huggingface.co/datasets/meituan-longcat/VitaBench
  title: meituan-longcat/VitaBench — Hugging Face dataset card (MIT licence)
  trust: 0.9
- url: https://github.com/meituan-longcat/vitabench
  title: meituan-longcat/vitabench — official code repository
  trust: 0.9
related:
- benchmark/tau2-bench
- benchmark/bfcl-v4
- benchmark/deepplanning
---

## What It Tests

Whether an agent can complete **interactive real-world tasks** in food delivery, in-store consumption and online travel.

- **66 tools**, composable across scenarios without domain-specific policies.
- **Skills** — reasoning over time and place, clarifying ambiguous instructions, tracking shifting intent.

## How It Scores

**Success rate**, all or nothing: a task succeeds only when **every rubric item** is satisfied.

- **Sliding window** — an LLM evaluator reads the trajectory in overlapping windows of 10 turns (2 overlapping) and keeps a running record of satisfied items, so different valid paths can pass.
- **Avg@4** — mean success over four runs per task.
- **Pass@k / Pass^k** — at least one of *k* runs succeeds / all *k* runs succeed.
- **Error analysis** — of 76 failed rubric items from one model: reasoning 61.8%, tool use 21.1%, interaction 7.9%, user simulator 9.2%.

## Format

Multi-turn dialogue with a simulated user and tool calls in the environment.

| Set | Tasks | Read / write / general tools |
|---|---|---|
| Cross-scenario (all tools combined) | 100 | 33 / 27 / 6 |
| Delivery | 100 | 10 / 4 / 6 |
| In-store | 100 | 10 / 9 / 5 |
| Online travel (OTA) | 100 | 19 / 14 / 5 |

**Data and code** — Hugging Face `meituan-longcat/VitaBench` and GitHub `meituan-longcat/vitabench`, **MIT licence**.

## Limitations

- **LLM in the loop.** A rubric-based LLM evaluator and an LLM-simulated user.
- **Strict scoring.** One missed rubric item fails the task.
- **Life services only.** Food delivery, in-store and travel.

## Example

From the paper's Appendix C, a full example trajectory. One simulated-user turn reads:

> "We have 6 people total, and we want to book a table for 12 noon on the 27th."

The agent must use the scenario tools to act on such requests, asking for any missing details. The evaluator then checks the whole trajectory against the task's rubric items, and the task succeeds only if all are met.
