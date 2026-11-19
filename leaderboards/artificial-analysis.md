---
id: leaderboard/artificial-analysis
type: leaderboard
slug: artificial-analysis
schema_version: 2.1
title: Artificial Analysis
name: Artificial Analysis
maintainer: Artificial Analysis (independent)
url: https://artificialanalysis.ai
tagline: Intelligence vs. price vs. speed — the production-decision board.
update_cadence: Weekly
coverage: ~30 hosted models, ~8 providers
free_access: true
tags:
- leaderboard
updated: '2026-11-19'
sources:
- url: https://artificialanalysis.ai
  title: Artificial Analysis
---

## What It Ranks

Runs a curated battery (MMLU-Pro, GPQA Diamond, MATH-500, HumanEval, MGSM, LiveCodeBench, SciCode, AIME) against ~30 hosted API models and publishes the scores alongside $ per million tokens, output tokens/second, and time-to-first-token. The 'Intelligence Index' is a weighted aggregate.

### Why You Need It

You need to pick a model for a product. Quality alone is not enough — cost per request, latency, and context window decide whether it ships.

### Why It Matters

Only public board that puts capability and economics on one page. A model that scores 2 points higher but costs 10× more rarely wins in production; this is where you see that tradeoff plainly.

## How To Read It

### Methodology

- Independent re-runs of public benchmarks (no author-reported numbers)
- Latency measured from their infra against each provider's public endpoint
- Prices pulled from provider pricing pages, updated weekly
- Intelligence Index is a normalized weighted mean across capability benchmarks

### Strengths

- All three dimensions (quality, cost, speed) on one screen
- Provider-level data — same model at different hosts can have different latency/price
- Historical charts show where the frontier is moving
- Non-partisan — funded independently, no affiliate bias

### Best For

- Choosing a production API model
- Cost optimization (same quality, cheaper)
- Understanding latency headroom for real-time UX

## Caveats

- API-hosted models only — no local / on-prem evaluation
- Aggregate Intelligence Index hides where a model is weak
- Benchmark selection is their curated choice, not community consensus
- Vision, audio, and agentic capabilities under-represented
