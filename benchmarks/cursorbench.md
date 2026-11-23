---
id: benchmark/cursorbench
type: benchmark
slug: cursorbench
schema_version: 4.0
title: CursorBench
name: CursorBench (v4.1)
category: Code Generation
description: 'Cursor''s internal coding-agent benchmark, built from real Cursor sessions: short, ambiguous, multi-file tasks graded by agentic graders; the tasks are not published.'
what_is_it: 'CursorBench is the offline evaluation suite Cursor uses to compare coding models; its tasks are not published. Tasks come from real sessions of Cursor''s own engineers: Cursor Blame traces committed code back to the agent request that produced it, which pairs a developer query with a ground-truth solution. Task descriptions are deliberately short and underspecified, like real requests, and agentic graders score the results. Cursor introduced it in March 2026 (version 3.0), refreshes it every few months, and its page listed version 4.1 (2026-11-18) as the latest on 2026-11-23.'
why_people_use_it: Public coding benchmarks tend to hand the agent a detailed issue, accept a narrow set of solutions, and draw from repositories that may sit in training data. Cursor built CursorBench to separate frontier models more clearly and to track what developers experience in its product. Its public page ranks models at several reasoning-effort settings and reports cost, tokens and steps per task next to the score.
method: Each task gives an agent a short request taken from a real Cursor session, in a multi-file or multi-workspace codebase. Agentic graders score the result. Cursor says it evaluates correctness, code quality, efficiency and interaction behaviour, and its published page and post focus on correctness. A separate online loop of controlled experiments on live traffic complements the offline suite.
metric: Score in percent (higher is better), reported per model and reasoning-effort setting on the maker's page, with average cost per task computed by applying each model's published per-million-token prices (input, cache read, cache write, output) to the tokens it used. The maker states that results vary and small differences may not be statistically meaningful.
format: 'Agentic, 0-shot: one short natural-language request per task, multi-step work in a real repository, scored by agentic graders. Version 4.0 added long-horizon tasks; 4.1 modified the instruction-following problems and raised the agent time limit.'
limitations: 'The suite is internal: the task set, its size and the grader prompts are not published, so nobody outside Cursor can rerun it. Scores are only comparable within one version; Cursor warns that a changed task mix shifts scores. Because Cursor both builds the benchmark and sells the product it tests, treat it as one vendor''s view of coding quality.'
citation: https://cursor.com/cursorbench
openness: closed
openness_note: Cursor publishes the leaderboard, changelog and a methodology post, but not the tasks, their number or the grader prompts.
tags:
- coding
- agent
- code-generation
- real-world
- versioned
updated: '2026-11-23'
sources:
- url: https://cursor.com/cursorbench
  title: CursorBench — Cursor's leaderboard and changelog
  trust: 0.95
- url: https://cursor.com/blog/cursorbench
  title: How we compare model quality in Cursor (Cursor blog)
  trust: 0.95
related:
- benchmark/swe-bench
- benchmark/terminal-bench
- benchmark/frontiercode
---

## What It Tests

CursorBench's **tasks are not published**; Cursor describes them and publishes results only. It tests whether a coding agent can finish **realistic developer requests** that arrive short and vague.

- **Real origin** — tasks trace back to sessions in Cursor's own engineering team, not to public repositories.
- **Underspecified prompts** — descriptions are intentionally brief, so the agent must infer intent.
- **Broad scope** — tasks span several files, and later versions add monorepos, log investigation and long-running work.

The changelog on Cursor's page shows how the suite grew:

| Version | Date | Change |
|---|---|---|
| 3.0 | 2026-03-11 | Initial edit, refactor and bug-fix tasks |
| 3.1 | 2026-05-19 | Codebase understanding, bug finding, planning, code review; better grading for some edit tasks |
| 3.2 | 2026-07-08 | Instruction following and advanced tool use |
| 4.0 | 2026-09-10 | Long-horizon edit, refactor, investigation, intent understanding, managing jobs, design adherence |
| 4.1 | 2026-11-18 | Modified instruction-following problems; longer agent time limit |

## How It Scores

Agentic graders score each task, and the headline is a **percentage score** per model and effort setting.

- **Cost** — average cost per task = the model's published per-million-token prices applied to its input, cache-read, cache-write and output tokens.
- **Effort levels** — one model appears several times, once per reasoning-effort setting.
- **Variance** — Cursor notes that small gaps may not be statistically meaningful.

## Format

Zero-shot and agentic: one short request, then multi-step work in a real codebase. Cursor says it measures correctness, code quality, efficiency and interaction behaviour, but its published page and post focus on **solution correctness**.

## Limitations

- **Closed suite.** The tasks, their count and the grader details are not public, so results cannot be reproduced.
- **Version-bound.** Compare scores only within one version; a new task mix moves the numbers.
- **Vendor interest.** The maker also sells the product the benchmark evaluates.

## Example

Cursor publishes the leaderboard and changelog but no individual tasks. As the maker describes it, a task is a **short request** such as a developer would type, a repository with many files, and an agentic grader that checks the outcome against a ground-truth solution from a real session. Compared with a detailed GitHub issue, the request leaves the agent to find the files, the intent and the acceptable design on its own.
