---
id: benchmark/wandr
type: benchmark
slug: wandr
schema_version: 1.0
title: "WANDR"
name: WANDR (Wide ANd Deep Research)
category: Agent & Tool Use
description: "A Perplexity benchmark of 500 data-collection tasks in which a research agent must find many items and back each with cited evidence."
what_is_it: "WANDR, short for Wide ANd Deep Research, evaluates research agents that must search wide and deep. It has 500 benchmark tasks. Breadth is the number of members an agent must find, and depth is the number of records required per member. The median task asks for 50 members at about 4 records each. The agent must build a collection and support every record with evidence."
why_people_use_it: "A fixed answer key is expensive to build and goes stale for open-ended research questions. WANDR grades each submitted claim against the evidence the agent cites, which makes failures localisable to discovery, enrichment, identity handling, page qualification or evidence extraction. It also exposes the gap between partial and complete coverage."
method: "A grading pipeline normalises each system's output, fetches every cited page and retries broken fetches in a browser, has a judge resolve identities and evaluate every record, then rolls verdicts up into task-level precision, recall and F1. Grading is reference-free: no answer key is used."
metric: "Precision, recall and F1 per task, in soft and hard versions. The hard version demands complete credit for a record, so soft-to-hard drops show partial progress without full completion."
format: "Open-ended web research: a request for a large collection, with records and cited sources as the output."
limitations: "Grading depends on re-fetching live pages and on a judge model, so pages that change or disappear can affect results. Perplexity built and also competes on the benchmark with its own system, so the leaderboard has a vendor interest. Anthropic's release describes a 'relaxed score' for WANDR, a different measure from the hard F1 in Perplexity's write-up, so cite which score a number is."
citation: "https://www.perplexity.ai/hub/blog/wandr-benchmark-evaluating-research-agents-that-must-search-wide-and-deep"
tags: [agent, research, web-search, citations, perplexity]
updated: 2026-11-21
sources:
  - { url: "https://www.perplexity.ai/hub/blog/wandr-benchmark-evaluating-research-agents-that-must-search-wide-and-deep", title: "WANDR Benchmark: Evaluating Research Agents That Must Search Wide and Deep (Perplexity)", trust: 0.95 }
  - { url: "https://www.anthropic.com/claude-fable-and-mythos-5-2", title: "Introducing Claude Fable 5.2 and Claude Mythos 5.2 (Anthropic, WANDR relaxed score)", trust: 0.8 }
---

## What It Tests
Whether a research agent can **find large sets of items and prove each one**.

- **Wide** — many members to discover.
- **Deep** — several records per member.
- **Evidence** — each claim must be supported by a cited source.

## How It Scores
**Reference-free, evidence-verified grading.**

- **Pipeline** — solve, fetch, judge, score.
- **Metrics** — precision, recall and F1, soft and hard.
- **Diagnosis** — the score tree localises losses to a stage.

## Format
500 tasks; the median asks for 50 members with about 4 records each. The output is a list of records with cited pages.

## Limitations
- **Live pages.** Grading re-fetches cited pages, which can change.
- **Judge model.** Identity and record checks use a model.
- **Vendor interest.** Perplexity's own system is among those compared.
- **Score variants.** Anthropic cites a 'relaxed score' that differs from hard F1.

## Example
Perplexity's write-up describes the task shape: a request for a **large collection** of members with several records each, every record backed by a cited page. The grader opens each cited page and checks that it satisfies the task's requirements and supports what the record claims.
