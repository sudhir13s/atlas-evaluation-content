---
id: leaderboard/vals-index
type: leaderboard
slug: vals-index
schema_version: 2.1
title: Vals Index
name: Vals Index
maintainer: Vals AI
url: https://www.vals.ai/benchmarks/vals_index
tagline: A GDP-weighted measure of agentic model performance in finance, coding, legal and tax work.
ranks: models by weighted accuracy across finance, coding, legal and tax tasks
benchmarks: [benchmark/terminal-bench]
update_cadence: Not stated; the page shows 'Updated 11/9/2026' and index releases on 2026-08-13, 2026-09-25 and 2026-10-19
tags: [leaderboard, composite-index, economic-weighting, agentic, private-benchmarks]
updated: 2026-11-21
sources:
  - { url: "https://www.vals.ai/benchmarks/vals_index", title: "Vals Index — Vals AI", trust: 0.9 }
related: [benchmark/terminal-bench]
---

## What It Ranks

The Vals Index ranks models on **agentic work in four sectors**, each weighted by its share of US gross domestic product (GDP). The version described here is **v2.2**, released on 2026-10-19.

- **Finance** — Finance Agent v2.1 and Excel Modeling Benchmark v1.1.
- **Coding** — Terminal-Bench 4.0, Vibe Code Bench v1.1 and Code Migration v1.1.
- **Legal** — Legal Research Bench and HLAB-Vals-1.1, Harvey's Legal Agent Benchmark.
- **Tax** — Tax Agent Bench.

The page says the index aggregates **six private and two public benchmarks**.

## How To Read It

Each sector is the average of its benchmarks, and the sectors combine as:

`Vals_Index = (8.0 × Finance + 5.6 × Coding + 1.2 × Legal + 0.5 × Tax) / 15.3`

- **Weights** — 8.0 (Finance and Insurance), 5.6 (Information sector, the closest proxy for software work), 1.2 (Legal services), 0.5 (accounting and tax preparation, NAICS 5412), per Bureau of Economic Analysis data.
- **Scale** — accuracy in percent; the page also lists cost, tokens and duration per test.
- **Code Migration** — the index uses a fixed subset: 50 of the 120 command-line migration tasks plus all 10 COBOL tasks, weighted 75% and 25%.
- **Release history** — v2 on 2026-08-13, v2.1 on 2026-09-25 (added Tax and moved to Terminal-Bench 4.0), v2.2 on 2026-10-19 (updated several components, switched Tax Agent Bench to all-pass accuracy).

## Caveats

- **Mostly private.** Most components are private Vals benchmarks that no other party has full access to, so results cannot be rerun.
- **Simplified weighting.** The page itself calls GDP weighting a vast oversimplification of AI's economic effect.
- **Version-bound.** Scores under v2 and v2.2 differ because components and Terminal-Bench versions changed.
- **Terms.** The page carries a 'Proprietary' label and states no licence or machine-readable file.
- **Single-maker view.** Vals AI builds and runs the evaluations itself; the page does not say whether scores are independently audited.
