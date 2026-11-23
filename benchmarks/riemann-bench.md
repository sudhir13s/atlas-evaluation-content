---
id: benchmark/riemann-bench
type: benchmark
slug: riemann-bench
schema_version: 4.0
title: Riemann-bench
name: Riemann-bench
category: Math & Logic
description: A private Surge AI benchmark of 25 research-level mathematics problems with unique closed-form answers, solved by AI systems acting as unconstrained research agents.
what_is_it: Riemann-bench is a benchmark from Surge AI (Garre, Knutsen, Mehta and Chen, 2026) of expert-written problems aimed at research-level mathematics, well beyond olympiad level. Surge's blog gives 25 problems, drawn from research problems its contributors met in their own work. Authors include Ivy League mathematics professors, graduate students and PhD-holding International Mathematical Olympiad (IMO) medalists, and problems routinely took their authors weeks to solve. Two independent experts must each solve every problem from scratch, and each problem has one closed-form answer that programs check. The set is kept private.
why_people_use_it: Competition mathematics is a narrow slice of mathematical reasoning. Riemann-bench measures research-level problems instead, and the authors say keeping the problems private makes scores reflect capability rather than memorised training data.
method: Models run as unconstrained research agents with coding tools, search and open-ended reasoning. The paper uses an unbiased statistical estimator over 100 independent runs per problem. Verification is programmatic against a single closed-form answer. Surge's blog says models first had a one-hour timeout per problem and the pipeline later moved to unlimited time.
metric: 'Score in percent: the estimated chance of solving a problem in one run, averaged over problems, from 100 runs each.'
format: Agentic, with tool access (code, search); one closed-form answer per problem, checked automatically.
limitations: The problem set is private, so outsiders cannot inspect or rerun it. Scores moved when the maker removed the one-hour timeout, so a score depends on the time and tool setup; always cite the date and pipeline version. With 25 problems, one problem is 4 percentage points. Every problem must have a unique closed-form solution, so the set covers only that kind of problem.
citation: https://arxiv.org/abs/2604.06802
openness: closed
openness_note: Surge AI publishes the method, a leaderboard and one sample problem as an image in its blog, but keeps the problem set strictly private.
tags:
- math
- research-level
- private-benchmark
- agent
- surge-ai
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2604.06802
  title: 'Riemann-Bench: A Benchmark for Moonshot Mathematics (arXiv:2604.06802)'
  trust: 0.95
- url: https://www.surgehq.ai/benchmarks/riemann-bench
  title: Riemann-bench — Surge AI benchmark page and leaderboard
  trust: 0.9
- url: https://surgehq.ai/blog/riemann-bench-a-benchmark-for-moonshot-mathematics
  title: 'Riemann-bench: A Benchmark for Moonshot Mathematics (Surge AI blog)'
  trust: 0.9
related:
- benchmark/aime
---

## What It Tests

Riemann-bench's problems are **not published**: Surge AI keeps the set strictly private. It tests whether an AI system can solve **research-level mathematics** that needs deep theory and novel synthesis.

- **Size** — 25 problems, per Surge AI's blog.
- **Authors** — Ivy League professors, graduate students and PhD-holding IMO medalists.
- **Double-blind checks** — two independent experts solve each problem from scratch before it is accepted.
- **One right answer** — each problem has a unique closed-form solution.

## How It Scores

A program compares the model's final answer with the **closed-form answer**.

- **Estimator** — an unbiased estimate of the solve rate from 100 independent runs per problem.
- **Reported value** — a percentage, averaged over problems.

## Format

Models act as unconstrained research agents with coding tools, search and long reasoning.

- **Time** — the blog says runs first had a one-hour timeout; the pipeline later gave unlimited time per problem.
- **Paper** — arXiv 2604.06802 (2026), accepted to an ICLR 2026 workshop on logical reasoning of large language models.

## Limitations

- **Private and uninspectable.** No public problem set, so results cannot be reproduced outside Surge AI.
- **Setup-sensitive.** Removing the timeout changed results, so quote the pipeline version and date.
- **Small.** 25 problems: one problem is 4 percentage points.
- **Narrow answer type.** Only problems with a unique closed form are included.

## Example

Surge AI's blog shows one sample problem as an image, rated at **40 to 50 hours of work** for an expert in its field; the Atlas does not reproduce it. As the maker describes it, a task is a research-level problem with a single closed-form answer. The agent may write code, search and reason at length, then submits that answer; a verifier checks it, and 100 runs per problem estimate the solve rate.
