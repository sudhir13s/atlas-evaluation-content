---
id: benchmark/toolathlon-verified
type: benchmark
slug: toolathlon-verified
schema_version: 1.0
title: "Toolathlon-Verified"
name: Toolathlon-Verified (The Tool Decathlon)
category: Agent & Tool Use
description: "A corrected, hardened release of Toolathlon: long-horizon, execution-verified multi-tool tasks across dozens of real apps, where the agent must reach a target environment state over ~20 tool-calling turns."
what_is_it: "Toolathlon (The Tool Decathlon) is a benchmark for general tool use by language agents in realistic environments — 108 tasks spanning 32 software applications and 600+ tools, from everyday platforms (Google Calendar, Notion) to professional ones (WooCommerce, Kubernetes, BigQuery, Canvas, Snowflake). Tools are exposed via Model Context Protocol (MCP) servers, and each task starts from a realistic initial state (e.g. a Canvas course with dozens of students, a real financial spreadsheet). Toolathlon-Verified, released 2026-06-30 by the HKUST-NLP team, is a fixed version: tasks corrected, graders aligned, per-task state isolated, and infrastructure hardened so failures reflect model capability rather than benchmark bugs — it starts a new official score series."
why_people_use_it: "It pushes agent evaluation beyond software engineering into diverse, long-horizon, real-application tool use — the kind of multi-app workflows a useful assistant must handle. Tasks require interacting with several apps over ~20 turns on average, and success is checked by executing the resulting environment state against ground truth, not by an LLM judge. It is hard and discriminating: on the original Toolathlon the best model, Claude-4.5-Sonnet, reached only 38.6% success. The Verified version exists so that remaining failures are meaningful signal about models, which is why labs adopt it as a cleaner successor."
examples:
  - "Across Canvas and a spreadsheet: read a roster, compute final grades per a stated rubric, and post them back so the resulting Canvas gradebook matches the expected state."
  - "Across Notion and a calendar/store app: reconcile order records and schedule follow-ups over ~20 tool calls until the final state matches ground truth."
method: "Each task is fully automated: a preprocess step sets up a realistic initial workspace state, the agent completes the task over many turns using MCP-exposed tools, and an evaluation script checks the final environment state against reference ground truth. Toolathlon-Verified adds corrected task specs, aligned evaluators, isolated per-task state, and hardened infrastructure; per task the run is bounded (e.g. a 5400-second cap)."
metric: "Pass@1 success rate — the fraction of tasks whose final environment state matches ground truth on the first attempt, verified by execution. Toolathlon-Verified is a new score series, not comparable to earlier Toolathlon numbers."
format: "0-shot, long-horizon agentic loop (~20 tool-calling turns on average) over MCP-exposed real apps; execution-based pass/fail per task."
saturation: medium
limitations: "Toolathlon-Verified restates the score series, so its numbers are NOT comparable to the original Toolathlon — always cite the Verified version. Success is judged by matching a target final state, so a task with multiple valid outcomes can penalize a correct-but-different solution unless the grader accounts for it (fixing exactly this drove the Verified release). It depends on live, third-party software and MCP servers, which adds setup and non-determinism, and the 108-task scale means confidence intervals are non-trivial. Long-horizon runs also blend model quality with the agent scaffold and turn/time budget."
citation: "https://arxiv.org/abs/2510.25726"
example:
  task_type: "Multi-tool orchestration (Canvas gradebook + spreadsheet)"
  input: "Initial state: a Canvas course with dozens of students and a spreadsheet of raw scores. Task: 'Compute each student's final grade using the syllabus rubric (drop the lowest quiz, weight exams 60%), then post the letter grades to Canvas.' The agent has MCP tools for the spreadsheet and Canvas."
  success:
    answer: "Read the roster and scores, apply the rubric (drop lowest quiz, weight exams 60%), map to letter grades, and write each grade back via the Canvas tool — the final Canvas gradebook state matches the reference ground truth exactly."
    why: "The agent sequenced the right tools across both apps and produced the exact target environment state the evaluation script checks."
  failure:
    answer: "Compute the grades correctly in the spreadsheet but post raw percentages (not letter grades) to Canvas, or update only the students present on the first page and skip the rest due to unhandled pagination."
    why: "The final Canvas state does not match ground truth (wrong grade format, or missing students), so the execution-based grader marks the task failed even though intermediate reasoning looked right."
  checks: "Whether an agent can plan and execute a long, multi-app workflow with the correct tool sequence and end in the exact target state, including edge cases like pagination and output format."
  scoring: "Execution-based Pass@1: the final environment state must match reference ground truth; benchmark score is the success rate over tasks."
tags: [agent, tool-use, mcp, long-horizon, multi-tool, leaderboard]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2510.25726", title: "The Tool Decathlon: Benchmarking Language Agents for Diverse, Realistic, and Long-Horizon Task Execution", trust: 0.95 }
  - { url: "https://github.com/hkust-nlp/Toolathlon", title: "Toolathlon — official repo (HKUST-NLP), includes Toolathlon-Verified", trust: 0.9 }
  - { url: "https://toolathlon.xyz/docs/leaderboard", title: "Toolathlon-Verified leaderboard", trust: 0.85 }
  - { url: "https://openreview.net/forum?id=z53s5p0qhf", title: "The Tool Decathlon — OpenReview (ICLR 2026)", trust: 0.85 }
related: [benchmark/bfcl-v4, benchmark/tau2-bench, benchmark/vita-bench, benchmark/terminal-bench]
---

## What It Tests
Whether a language agent can do **long-horizon, multi-app work in realistic environments** — the messy
tool use a genuinely useful assistant needs, well beyond software engineering. The benchmark covers
**108 tasks across 32 applications and 600+ tools**, exposed through **Model Context Protocol (MCP)
servers**, ranging from everyday apps (Google Calendar, Notion) to professional systems (WooCommerce,
Kubernetes, BigQuery, Canvas, Snowflake).

Two things make it realistic:

- **Real initial states** — a Canvas course with dozens of students, an actual financial spreadsheet —
  not blank sandboxes.
- **Long horizons** — a task requires interacting with several apps over **~20 tool-calling turns on
  average**.

**Toolathlon-Verified** (released **2026-06-30** by HKUST-NLP) is the cleaned successor: **tasks
corrected, graders aligned, per-task state isolated, infrastructure hardened**, so a failure reflects
the model rather than a benchmark bug.

## How It Scores
Every task is **fully automated and execution-verified**. A **preprocess** step sets up the initial
workspace state; the agent runs to completion using its MCP tools; then an **evaluation script checks
the final environment state against reference ground truth**. The metric is **Pass@1 success rate** —
the fraction of tasks whose end state matches ground truth on the first attempt. No LLM judge.

Because the Verified release realigned specs and graders, it **starts a new official score series that
is not comparable to earlier Toolathlon**. On the original benchmark the ceiling was low — the best
model, **Claude-4.5-Sonnet, reached 38.6% success** (averaging ~20 tool calls per task), with the top
open-weight model near 20% — underlining how far agents are from reliable multi-tool execution.

## Format
Zero-shot, long-horizon agentic loop over live apps, bounded per task (for example a 5400-second cap).
The agent gets a task and a set of MCP tools and must reach the target state on its own — no worked
examples.

## Limitations
Verified **restates the score series**, so its numbers are **not comparable** to original Toolathlon —
cite the version. Grading by **target-final-state match** can penalize a correct-but-different solution
unless the grader anticipates it (aligning exactly this drove the Verified release). The benchmark
leans on **live third-party software and MCP servers**, which adds setup burden and non-determinism, and
its **108-task** scale makes confidence intervals non-trivial. As with any long-horizon agent
benchmark, a score blends model quality with the scaffold and the turn/time budget.

## Example
Starting from a real Canvas course and a scores spreadsheet, the task is: apply the syllabus rubric
(drop the lowest quiz, weight exams 60%), then post letter grades to Canvas. A correct run reads the
roster, computes grades under the rubric, and writes the right letter grade back for every student, so
the final Canvas gradebook matches ground truth. A plausible wrong run computes the grades correctly but
posts raw percentages instead of letters — or updates only the first page of students and skips the rest
through unhandled pagination. The reasoning looked fine, but the **final state does not match**, so the
execution grader marks it failed. That gap between "looks done" and "the environment is actually in the
target state" is what Toolathlon-Verified measures.
