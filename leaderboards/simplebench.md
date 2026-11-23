---
id: leaderboard/simplebench
type: leaderboard
slug: simplebench
schema_version: 4.0
title: SimpleBench
name: SimpleBench
maintainer: SimpleBench Team
url: https://simple-bench.com
tagline: Multiple-choice questions where untrained people set the baseline.
ranks: models by average accuracy over five runs on multiple-choice questions
update_cadence: Not stated
coverage: 117 models
free_access: true
score_origin: independent
tags:
- leaderboard
updated: '2026-11-21'
sources:
- url: https://simple-bench.com
  title: SimpleBench
  trust: 0.9
- url: https://github.com/simple-bench/SimpleBench
  title: SimpleBench code and public dataset
  trust: 0.9
---

## What It Ranks

SimpleBench is a multiple-choice text benchmark of over 200 questions on spatio-temporal reasoning, social intelligence and trick questions, which the page calls linguistic adversarial robustness. The page names the SimpleBench Team as authors.

- **Human baseline**: 83.7%, from what the page calls a small sample of nine participants; the table also lists a highest human score of 95.4%.
- **Score**: average over five runs (AVG@5).

## How To Read It

### Methodology

- Temperature 0.7 and top-p 0.95, except for the o1 series, per the page
- The public dataset in the GitHub repository holds 10 questions; the rest are not published
- The leaderboard has further views: Perf/$, Timeline and Open-Ended

### Best For

- Comparing models against a stated human baseline on short reasoning and trick questions

## Caveats

- Nine human participants set the baseline
- Most questions are unpublished, so scores cannot be reproduced outside the team
- The introduction on the page names a top model and says the human baseline beats every tested model, while the table lists several models above 83.7%
- The page states no update schedule
