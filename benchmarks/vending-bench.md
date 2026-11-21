---
id: benchmark/vending-bench
type: benchmark
slug: vending-bench
schema_version: 1.0
title: "Vending-Bench"
name: Vending-Bench (including Vending-Bench 2 and Arena)
category: Agent & Tool Use
description: "An Andon Labs simulation where an agent runs a vending-machine business over a long period, testing whether it stays coherent for millions of tokens."
what_is_it: "Vending-Bench is a simulated environment in which an LLM agent operates a vending machine: it balances inventory, places orders, sets prices and pays daily fees. Each task is simple, but a run exceeds 20 million tokens, so the test is sustained, coherent decision-making. Vending-Bench 2 asks models to run the business for a simulated year and scores their bank balance at the end. Vending-Bench Arena adds competition, with several agents managing machines at the same location."
why_people_use_it: "Models often fail over long horizons even when short tasks look easy. The paper reports high variance across runs and 'meltdown' loops from which models rarely recover, and finds no clear link between failures and a full context window. It also tests whether a model can acquire capital, which the authors link to dangerous-AI scenarios."
method: "The agent acts through tools in a simulated business. In Vending-Bench 2 the leaderboard averages the final money balance across runs. In Arena, all scoring is individual, but agents may collaborate and trade, and price wars occur."
metric: "Final money balance in dollars (higher is better), averaged across runs on the maker's leaderboard."
format: "Long-horizon agentic simulation: a year of simulated days in Vending-Bench 2, with tool calls throughout; no fixed question set."
limitations: "Results vary a lot between runs of the same model, so averages across runs matter. Version 1 and Vending-Bench 2 differ, and the paper's findings are about version 1. Vendor tables cite a bank balance in dollars, which is only comparable within one version and run count. The environment is a simulation, so it measures coherence, not real-world business skill."
citation: "https://arxiv.org/abs/2502.15840"
tags: [agent, long-horizon, simulation, business, andon-labs]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2502.15840", title: "Vending-Bench: A Benchmark for Long-Term Coherence of Autonomous Agents (arXiv:2502.15840)", trust: 0.95 }
  - { url: "https://andonlabs.com/evals/vending-bench-2", title: "Vending-Bench 2 — Andon Labs", trust: 0.95 }
---

## What It Tests
Whether an agent can stay **coherent and effective over a very long run**.

- **Business tasks** — manage inventory, place orders, set prices, pay daily fees.
- **Long horizon** — over 20 million tokens per run in version 1; a simulated year in version 2.
- **Capital** — the agent must grow its money balance.

## How It Scores
The score is the **bank balance at the end** of the simulation, averaged over runs.

- **Version 2** — a year-long run, scored on the final balance.
- **Arena** — multi-agent version; scoring stays individual even though agents can trade.

## Format
A simulated vending-machine business with tool use throughout. Andon Labs says the best performers keep a steady rate of tool use all year and find good supplier prices.

## Limitations
- **High variance.** Runs of one model can differ widely.
- **Version-bound.** The paper covers version 1; Vending-Bench 2 has its own leaderboard.
- **Simulated.** It measures long-horizon coherence, not real retail ability.

## Example
The agent runs a vending machine: it must **balance inventories, place orders, set prices and handle daily fees**. Each step is simple, but the run is very long. The paper says failing runs derail by misinterpreting delivery schedules, forgetting orders or descending into tangential 'meltdown' loops from which they rarely recover. Models that keep a steady rate of tool use and source products at good prices score best on Vending-Bench 2.
