---
id: benchmark/vending-bench
type: benchmark
slug: vending-bench
schema_version: 3.1
title: "Vending-Bench"
name: Vending-Bench (including Vending-Bench 2 and Arena)
category: Agent & Tool Use
description: "An Andon Labs simulation where an agent runs a vending-machine business over a long period, testing whether it stays coherent for millions of tokens."
what_is_it: "Vending-Bench (Backlund and Petersson, arXiv, February 2025) is a simulated environment in which an LLM agent operates a vending machine business: it balances inventory, places orders, sets prices and handles daily fees. Each task is simple, but a run exceeds 20 million tokens, so the test is sustained, coherent decision-making. Vending-Bench 2 from Andon Labs asks models to run a simulated vending business in San Francisco for one year, starting with $500, and scores the bank balance at the end. Vending-Bench Arena adds competition, with several agents running machines at the same location."
why_people_use_it: "Models often fail over long horizons even when short tasks look easy. The paper reports high variance in performance across models, with runs that derail by misinterpreting delivery schedules, forgetting orders or descending into 'meltdown' loops, and finds no clear correlation between failures and the context window becoming full. It also tests the ability to acquire capital, which the authors call a necessity in many hypothetical dangerous-AI scenarios."
method: "The agent acts through tools in a simulated business. In Vending-Bench 2 a run lasts one simulated year unless the agent is terminated early for failing to pay the $2 daily fee for more than 10 consecutive days; the leaderboard averages the final money balance across runs and shows the standard error. In Arena, agents may collaborate and trade, price wars occur, and scoring stays individual."
metric: "Final money balance in dollars after one simulated year (higher is better), averaged across runs on the maker's leaderboard."
format: "Long-horizon agentic simulation: a year of simulated days in Vending-Bench 2, with tool calls throughout; no fixed question set."
limitations: "The environment code is not published, so only the maker can run the official leaderboard. Results vary between runs and models, so averages across runs matter. Version 1 and Vending-Bench 2 differ, and the paper's findings are about version 1; a dollar balance is only comparable within one version. The maker gives no measured human baseline, only an estimate of what a good strategy could earn. The environment is a simulation, so it measures long-run coherence, not real-world business skill."
citation: "https://arxiv.org/abs/2502.15840"
openness: closed
openness_note: "The paper and the Vending-Bench 2 page describe the simulation, its rules and results, but the environment code and run setup are not published, so outsiders cannot rerun it."
tags: [agent, long-horizon, simulation, business, andon-labs]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2502.15840", title: "Vending-Bench: A Benchmark for Long-Term Coherence of Autonomous Agents (arXiv:2502.15840)", trust: 0.95 }
  - { url: "https://andonlabs.com/evals/vending-bench-2", title: "Vending-Bench 2 — Andon Labs", trust: 0.95 }
---

## What It Tests
The environment is not published; Andon Labs runs it and reports results. It tests whether an agent can stay **coherent and effective over a very long run**.

- **Business tasks** — manage inventory, place orders, set prices, pay daily fees.
- **Long horizon** — over **20 million tokens** per run in version 1; one simulated year in version 2.
- **Capital** — the agent must grow its money balance from a $500 start.

## How It Scores
The score is the **bank balance at the end** of the simulation, averaged over runs.

- **Version 2** — a year-long run; the agent is terminated early if it cannot pay the **$2 daily fee** for more than 10 consecutive days.
- **Arena** — multi-agent version; agents can trade or collaborate, but scoring stays individual.
- **No human baseline** — the maker gives only an estimated "good strategy" figure, not a measured human result.

## Format
A simulated vending-machine business with tool use throughout and no fixed question set. The system prompt tells the agent it is judged only on its bank balance at the end of the year.

## Limitations
- **Closed environment.** Outsiders cannot rerun it.
- **High variance.** Runs and models differ widely.
- **Version-bound.** The paper covers version 1; Vending-Bench 2 has its own leaderboard.
- **Simulated.** It measures long-horizon coherence, not real retail ability.

## Example
As the maker describes it, a Vending-Bench 2 run gives the agent a simulated vending machine in San Francisco, **$500** and tools to act in the business. It must **balance inventory, place orders, set prices and handle daily fees** for a simulated year. The paper says failing runs derail by misinterpreting delivery schedules, forgetting orders or descending into tangential **'meltdown' loops**.
