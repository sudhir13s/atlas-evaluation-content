---
id: leaderboard/vellum-llm-leaderboard
type: leaderboard
slug: vellum-llm-leaderboard
schema_version: 2.1
title: Vellum LLM Leaderboard
name: Vellum LLM Leaderboard
maintainer: Vellum AI
url: https://www.vellum.ai/llm-leaderboard
tagline: Curated, non-saturated benchmarks for the top ~25 production models.
update_cadence: Monthly
coverage: ~25 frontier models
free_access: true
tags:
- leaderboard
updated: '2026-11-19'
sources:
- url: https://www.vellum.ai/llm-leaderboard
  title: Vellum LLM Leaderboard
---

## What It Ranks

Hand-picked board that deliberately avoids saturated benchmarks (MMLU, HellaSwag). Uses GPQA, SWE-Bench, MATH-500, HumanEval+, and a few others where the frontier still has headroom. Pairs scores with latency, context window, and price.

### Why You Need It

When most models score 95%+ on MMLU, MMLU cannot tell you anything. You need benchmarks that still separate the top tier. Vellum's job is to curate that set.

### Why It Matters

A fast summary for practitioners who do not want to wade through HuggingFace's 3000 models or Arena's hundreds. The curation is the product.

## How To Read It

### Methodology

- Scores pulled from provider-published results + independent re-runs for key claims
- Benchmark selection refreshed when saturation hits
- Side-by-side comparison tool (pick 2-3 models, diff the columns)
- Cost and latency columns alongside capability

### Strengths

- Clean UI, fastest way to compare a shortlist
- Non-saturated benchmarks = signal not noise
- Covers hosted + leading open-weight together
- Updated when models ship — not on a fixed cadence

### Best For

- Quick comparison of 2-3 production candidates
- Understanding what still differentiates frontier models

## Caveats

- Curator-driven selection is opaque (why this benchmark, not that one?)
- Narrow coverage (~25 models) — misses long tail
- Some scores are provider-reported, not independently re-run
- Vellum is a vendor — watch for subtle conflicts of interest
