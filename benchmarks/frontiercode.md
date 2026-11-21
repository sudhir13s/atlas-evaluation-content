---
id: benchmark/frontiercode
type: benchmark
slug: frontiercode
schema_version: 1.0
title: "FrontierCode"
name: FrontierCode (v2.0)
category: Code Generation
description: "Cognition's coding benchmark built with open-source maintainers: an agent changes a real repository, and grading asks whether a maintainer would merge the change."
what_is_it: "FrontierCode is a series of coding benchmarks from Cognition, the company behind Devin. In each task an agent makes a change to a real open-source repository, and the change is graded on whether it is good enough for a maintainer to merge. Maintainers of those repositories wrote the tasks and the grading criteria. Version 2.0 draws on more repositories, adds security patches and performance work, and raises the bar on thoroughness."
why_people_use_it: "Most coding benchmarks fully specify the approach and saturate quickly. FrontierCode tries to measure what users want from an autonomous agent: take an informal spec, respect unstated constraints, and finish without repeated corrections. It is one of the coding inputs to the Artificial Analysis Intelligence Index."
method: "Each task has held-out tests plus a rubric of blocking and non-blocking criteria written by maintainers. Failing a blocking criterion zeroes the attempt. Artificial Analysis, which runs a variant on the same 132 tasks with its own harness and judge, also zeroes attempts whose trajectory shows the agent looking up the solution; otherwise an attempt gets a weighted rubric score, averaged over attempts and then over tasks."
metric: "Weighted rubric score in percent (higher is better), with a zero for any failed blocking check. Cognition's post does not restate the exact formula for 2.0; the scoring described here is Artificial Analysis' account of its own run."
format: "Agentic coding on a real repository checkout, graded by held-out tests and an LLM-judged rubric; no worked examples are given to the agent."
limitations: "Cognition says FrontierCode 2.0 largely keeps the 1.0 method, and the FrontierCode 1.0 post holds the grading details. The task set is described as private by Artificial Analysis, so outside parties cannot inspect it. Scores can fall when a model uses more reasoning effort, because extra code can trip scope criteria; Cognition treats that as a feature. Numbers from 1.1 and 2.0 are not comparable, and Artificial Analysis' variant differs in harness, judge and attempt count from Cognition's own."
citation: "https://cognition.com/blog/frontier-code-2.0"
tags: [coding, agent, mergeability, open-source-repositories, versioned]
updated: 2026-11-21
sources:
  - { url: "https://cognition.com/blog/frontier-code-2.0", title: "FrontierCode 2.0 (Cognition)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (FrontierCode-AA 2.0)", trust: 0.85 }
  - { url: "https://deepmind.google/models/gemini", title: "Gemini — Google DeepMind (evaluation table)", trust: 0.7 }
related: [benchmark/terminal-bench, benchmark/cursorbench, leaderboard/artificial-analysis]
---

## What It Tests
Whether an agent's code change is **good enough to merge**, not just whether tests pass.

- **Functional correctness** of the change.
- **Interpreting and expanding** a short user prompt.
- **Design and scope** choices that fit the existing codebase.

The tasks come from repositories that the authors maintain. More than 20 open-source developers each spent over 40 hours per task building them.

## How It Scores
Grading combines **held-out tests** with a **rubric of blocking and non-blocking criteria**.

- **Blocking criteria** — failing any one scores the attempt zero.
- **Thoroughness criteria** — a cluster of checks that an agent can infer from the prompt and codebase, such as fixing every instance of a bug.
- **Scope criteria** — pass rates on these fall as reasoning effort rises, so more effort can lower a score.

## Format
One real repository per task and an informal request. In Artificial Analysis' run there are **132 tasks** with **3 attempts** each, a 500-turn cap and a wall-clock limit.

## Limitations
- **Version drift.** 1.1 and 2.0 differ, so cite the version. Against 1.1, Cognition reports reference solutions that change 67% more files and 89% more lines, codebases 25% larger, tasks from 55% more repositories and 43% more blocking criteria.
- **Private set.** Artificial Analysis describes the benchmark as private; no public dataset was found.
- **Different runs.** Artificial Analysis' harness, judge and attempts differ from Cognition's, and vendor tables label scores 'main score' without defining it.

## Example
Cognition's post gives two kinds of thoroughness check. In **fixing every instance**, the prompt names a bug in one code path, but a sibling path has the same bug; a thorough agent fixes both. In **matching existing support**, the prompt mentions one input type while similar code handles others; a thorough agent supports them too. A change that satisfies only the literal prompt can still fail a blocking criterion and score zero.
