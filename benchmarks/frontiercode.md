---
id: benchmark/frontiercode
type: benchmark
slug: frontiercode
schema_version: 3.1
title: "FrontierCode"
name: FrontierCode (v2.0)
category: Code Generation
description: "Cognition's coding benchmark built with open-source maintainers: an agent changes a real repository, and grading asks whether a maintainer would merge the change; the task set is not published."
what_is_it: "FrontierCode is a series of coding benchmarks from Cognition, the company behind Devin; its task set is not published. In each task an agent makes a change to a real open-source repository, and the change is graded on whether it is good enough for a maintainer to merge. More than 20 maintainers of those repositories each spent over 40 hours per task writing the tasks and grading criteria. Version 2.0 (2026-10-27) draws on more repositories, adds security patches and performance work, and raises the bar on thoroughness."
why_people_use_it: "Most coding benchmarks fully specify the approach and saturate quickly. FrontierCode tries to measure what users want from an autonomous agent: take an informal spec, respect unstated constraints, and finish without repeated corrections. Artificial Analysis runs a variant, FrontierCode-AA 2.0, as a 15%-weight input to its Intelligence Index."
method: "Each task has held-out behaviour tests plus a rubric of blocking and non-blocking criteria written by maintainers; blockers include deterministic scope checks such as files that must not be modified. Failing a blocking criterion zeroes the attempt; otherwise the score is a weighted aggregate of the rubric items. Artificial Analysis runs the same 132 tasks with its own harness and LLM judge, 3 attempts each, and also zeroes attempts whose trajectory shows the agent looking up the solution."
metric: "Weighted rubric score in percent (higher is better), with a zero for any failed blocking criterion. In Artificial Analysis' run, attempt scores are averaged per task and task scores are averaged with equal weight."
format: "Agentic coding on a real repository checkout from an informal request, graded by held-out tests and an LLM-judged rubric."
limitations: "The task set is not public, so results cannot be reproduced independently. Scores can fall when a model uses more reasoning effort, because extra code can trip scope criteria. Numbers from 1.x and 2.0 are not comparable, and Artificial Analysis' variant differs in harness, judge and attempt count from Cognition's own runs."
citation: "https://cognition.com/blog/frontier-code-2.0"
openness: closed
openness_note: "Cognition describes the method and sample task types but publishes no task set, and Artificial Analysis lists its 132-task FrontierCode-AA 2.0 run as a private dataset."
tags: [coding, agent, mergeability, open-source-repositories, versioned]
updated: 2026-11-23
sources:
  - { url: "https://cognition.com/blog/frontier-code-2.0", title: "FrontierCode 2.0 (Cognition)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (FrontierCode-AA 2.0)", trust: 0.85 }
related: [benchmark/terminal-bench, benchmark/cursorbench, leaderboard/artificial-analysis]
---

## What It Tests
FrontierCode's **tasks are not published**; Cognition describes the method and reports results. It tests whether an agent's code change is **good enough to merge**, not just whether tests pass.

- **Functional correctness** of the change.
- **Interpreting and expanding** a short user prompt.
- **Design and scope** choices that fit the existing codebase.

The tasks come from repositories the task authors maintain: more than 20 open-source developers, each spending over 40 hours per task.

## How It Scores
Grading combines **held-out tests** with a **rubric of blocking and non-blocking criteria**.

- **Blocking criteria** — failing any one scores the attempt zero; some are deterministic scope checks.
- **Weighted rubric** — otherwise the score is a weighted aggregate of rubric items.
- **Thoroughness criteria** — checks an agent can infer from the prompt and codebase; version 2.0 has 501 thoroughness blockers vs 168 in 1.1.
- **Scope criteria** — loosened in 2.0 (average scope failure rate 9.0% in 1.1, 3.8% in 2.0), yet more effort can still lower a score.

## Format
One real repository per task and an informal request. In Artificial Analysis' run: **132 tasks**, **3 attempts** each, a 500-turn cap and a wall-clock limit per attempt; at the limit the repository is graded as the agent left it.

## Limitations
- **Closed set.** No public task set, so results cannot be reproduced independently.
- **Version drift.** Cognition reports 2.0 reference solutions changing 67% more files and 89% more lines, in codebases 25% larger, than 1.1; its 55% more repositories and 43% more blocking criteria are measured against 1.0.
- **Different runs.** Artificial Analysis' harness, judge and attempts differ from Cognition's.

## Example
Cognition publishes no task text. As the maker describes it, two kinds of thoroughness check look like this:

- **Fixing every instance** — the prompt names a bug in one code path, but a sibling path has the same bug; a thorough agent fixes both.
- **Matching existing support** — the prompt mentions one input type while similar code handles others; a thorough agent supports them too.

A change that satisfies only the literal prompt can still fail a blocking criterion and score zero.
