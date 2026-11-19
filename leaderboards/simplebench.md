---
id: leaderboard/simplebench
type: leaderboard
slug: simplebench
schema_version: 2.1
title: SimpleBench
name: SimpleBench
maintainer: Philip (AI Explained)
url: https://simple-bench.com
tagline: ~200 trick questions humans ace. LLMs mostly fail.
update_cadence: Irregular
coverage: ~15 frontier models
free_access: true
tags:
- leaderboard
updated: '2026-11-19'
sources:
- url: https://simple-bench.com
  title: SimpleBench
---

## What It Ranks

A private benchmark of 200+ multiple-choice questions probing spatial, temporal, and social intuition — the 'common sense' layer humans use without thinking. Designed so untrained humans score 83%+ while frontier LLMs score 40-60%.

### Why You Need It

Public benchmarks increasingly fail to separate a useful model from a plausible-sounding one. SimpleBench reveals how much of frontier performance is pattern-matched from training data vs. actual reasoning.

### Why It Matters

Cuts through hype cycles. If a new model claims AGI-adjacent reasoning but scores 45% where humans score 83%, that tells you what it actually is.

## How To Read It

### Methodology

- Private question set (prevents contamination)
- Multiple-choice with a single correct answer
- Human baseline established with 9 untrained graders
- Public leaderboard with just 10 questions shown as examples

### Strengths

- Private = contamination-proof
- Reveals common-sense gap invisible in other benchmarks
- Single curator = consistent difficulty
- Human baseline is explicit (83%+)

### Best For

- Reality check on reasoning claims
- Teaching what common-sense reasoning actually requires

## Caveats

- Private questions reduce reproducibility
- Small sample (~200 questions)
- Single-curator bias in what counts as 'common sense'
- Updates infrequent
