---
id: benchmark/riemann-bench
type: benchmark
slug: riemann-bench
schema_version: 1.0
title: "Riemann-bench"
name: Riemann-bench
category: Math & Logic
description: "A private Surge AI benchmark of research-level mathematics problems with unique closed-form answers, solved by AI systems acting as unconstrained research agents."
what_is_it: "Riemann-bench is a benchmark from Surge AI of expert-written problems aimed at research-level mathematics, well beyond olympiad level. Authors include Ivy League mathematics professors, graduate students and PhD-holding International Mathematical Olympiad (IMO) medalists, and problems took their authors weeks to solve. Two independent experts must each solve every problem from scratch, and each problem has one closed-form answer that programmatic verifiers check. The set is kept private."
why_people_use_it: "Competition mathematics is a narrow slice of mathematical reasoning and can reward tricks over deep theory. Riemann-bench measures the harder end. Keeping the problems private is meant to make scores reflect real ability instead of memorised training data. Google DeepMind lists it in the science-and-math group of its Gemini 4 Neon table, citing the official leaderboard."
method: "Models run as research agents with coding tools, search and open-ended reasoning. The paper uses an unbiased statistical estimator over 100 independent runs per problem. Verification is programmatic against a single closed-form answer. Surge's blog says its pipeline later gave models unlimited time per problem, replacing an earlier one-hour timeout."
metric: "Score in percent: the estimated chance of solving a problem, averaged over problems, from 100 runs each. The leaderboard page shows it as a percentage."
format: "Agentic, with full tool access (code, search); one closed-form answer per problem, checked automatically."
limitations: "The problem set is private, so outsiders cannot inspect or rerun it, and the maker's paper abstract and benchmark page state no problem count. Scores moved when the maker removed the one-hour timeout, so a score depends on the time and tool setup. The paper reports early results below 10%, while the later leaderboard shows far higher numbers, so always cite the date and the pipeline version. Every problem must have a unique closed-form solution, so the set covers only that kind of problem."
citation: "https://arxiv.org/abs/2604.06802"
tags: [math, research-level, private-benchmark, agent, surge-ai]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2604.06802", title: "Riemann-Bench: A Benchmark for Moonshot Mathematics (arXiv:2604.06802)", trust: 0.95 }
  - { url: "https://www.surgehq.ai/benchmarks/riemann-bench", title: "Riemann-bench — Surge AI benchmark page and leaderboard", trust: 0.9 }
  - { url: "https://surgehq.ai/blog/riemann-bench-a-benchmark-for-moonshot-mathematics", title: "Riemann-bench: A Benchmark for Moonshot Mathematics (Surge AI blog)", trust: 0.85 }
related: [benchmark/aime]
---

## What It Tests
Whether an AI system can solve **research-level mathematics** that needs deep theory and novel synthesis.

- **Hard sources** — problems from professors, graduate students and IMO-medalist PhDs.
- **Double-blind checks** — two independent experts solve each problem from scratch before it is accepted.
- **One right answer** — each problem has a unique closed-form solution.

The blog's example is rated at 40 to 50 hours of work for an expert in the field.

## How It Scores
A program compares the model's final answer with the **closed-form answer**.

- **Estimator** — an unbiased estimate computed over 100 independent runs per problem.
- **Reported value** — a percentage on the leaderboard.
- **Privacy** — the benchmark stays private to limit memorisation.

## Format
Models act as unconstrained research agents with coding tools, search and long reasoning. The blog says the pipeline gives unlimited time per problem; an earlier version used a one-hour timeout.

## Limitations
- **Private and uninspectable.** No public problem set, and the maker states no problem count.
- **Setup-sensitive.** Removing the timeout changed results, so quote the pipeline version.
- **Narrow answer type.** Only problems with a unique closed form are included.
- **Moving numbers.** Early paper results and later leaderboard numbers differ by a wide margin.

## Example
The maker's blog shows one sample problem, rated at 40 to 50 hours of work for an expert in its field. In general, the agent may write code, search and reason at length, then submits a **single closed-form answer**. A verifier checks it, and the process repeats 100 times per problem to estimate the solve rate.
