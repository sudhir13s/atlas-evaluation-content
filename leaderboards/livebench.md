---
id: leaderboard/livebench
type: leaderboard
slug: livebench
schema_version: 2.1
title: LiveBench
name: LiveBench
maintainer: Abacus.AI
url: https://livebench.ai
tagline: Long-horizon tasks graded against hidden references, tests or rubrics.
ranks: models by a weighted mean of seven task suites, with cost per successful task
update_cadence: Irregular
coverage: 39 models
free_access: true
score_origin: independent
tags:
- leaderboard
updated: '2026-11-21'
sources:
- url: https://livebench.ai
  title: LiveBench 2.0
  trust: 0.9
- url: https://livebench.ai/#/methodology
  title: LiveBench 2.0 methodology
  trust: 0.9
- url: https://arxiv.org/abs/2406.19314
  title: 'LiveBench: A Challenging, Contamination-Limited LLM Benchmark'
  trust: 0.9
- url: https://github.com/LiveBench/LiveBench/blob/main/changelog.md
  title: LiveBench changelog
  trust: 0.9
---

## What It Ranks

LiveBench 2.0 gives models real work, such as decks, research reports, bug fixes and long-running apps, and grades what they hand back. The site names Abacus.AI as sponsor and says the results power Abacus AI's own agent product.

- **Suites**: Agentic (18 tasks), Design (25), Coding (24), Browser Use (25), Data Analysis (150 questions), Research (22) and Reasoning (202 questions).
- **Releases**: first release 2024-06-24; the latest, 2.0, on 2026-11-06.

## How To Read It

### Methodology

- Overall is a weighted mean of the suites with a score: Long Agentic counts twice, Design and Coding 1.5 times, the rest once
- One run per suite
- Grading depends on the suite: scripts against hidden reference data or tests, ground-truth answers, checks of an app's resulting data, a task-specific rubric for research reports, and visual and structural checks for design files
- Cost per successful task is total cost divided by total tasks and by the score, times 100
- The original LiveBench paper (2024) and its GitHub repository describe a design that adds new questions monthly; the changelog shows releases at irregular intervals

### Best For

- Comparing models on long-horizon task suites alongside cost per successful task

## Caveats

- The research suite is graded against a rubric, and the page does not say who or what applies it
- Scores come from one run per suite
- LiveBench 1.0, which the site still links, used different task categories, so its scores do not carry over to 2.0
- Abacus.AI sponsors the board and appears in its organization filter
