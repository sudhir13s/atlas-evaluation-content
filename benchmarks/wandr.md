---
id: benchmark/wandr
type: benchmark
slug: wandr
schema_version: 4.0
title: WANDR
name: WANDR (Wide ANd Deep Research)
category: Agent & Tool Use
description: A Perplexity benchmark of 500 data-collection tasks in which a research agent must find many items and back each with cited evidence.
what_is_it: WANDR, short for Wide ANd Deep Research, evaluates research agents that must search wide and deep. Perplexity Research published it on 14 July 2026 with 500 public task packages. Breadth is the number of members an agent must find, and depth is the number of records required per member. The median task asks for 50 members at 4 records each, or 245 records in total. The agent must build a collection and support every record with a cited page.
why_people_use_it: A fixed answer key is expensive to build and goes stale for open-ended research questions. WANDR grades each submitted claim against the evidence the agent cites, which makes failures traceable to discovery, enrichment, identity handling, page qualification or evidence extraction. The tasks and grading harness are public under Apache-2.0, so results can be rerun.
examples:
- 'From Perplexity''s write-up, task ceo_cfo_appointments: ''Find at least 70 US-based companies with a CEO or CFO appointment first announced between March 1 and April 30, 2026.'''
method: 'A grading pipeline normalises each system''s output (solve), fetches every cited page and retries broken fetches in a browser (fetch), has a judge resolve identities and evaluate every record (judge), then rolls verdicts up into task-level precision, recall and F1 (score). Grading is reference-free: no answer key is used.'
metric: Precision (quality of what was submitted), recall (quality-adjusted completion against the requested volume, with shortfalls counted as zero) and F1, the harmonic mean of the two. Each comes in a soft version, which gives partial credit for incomplete members, and a hard version, which counts only members whose full required set of records is correct.
format: 'Open-ended web research: a request for a large collection, with records and cited sources as the output.'
limitations: Grading depends on re-fetching live pages and on a judge model, so pages that change or disappear can affect results; the write-up does not name the judge model. Perplexity built the benchmark and also competes on it with its own system. Some comparisons in the write-up cover subsets (a 45-task effort comparison; diagnostics for 350 tasks for one system). Anthropic's release cites a 'relaxed score' for WANDR without defining it, a different measure from the hard F1 in Perplexity's write-up, so cite which score a number is.
citation: https://www.perplexity.ai/hub/blog/wandr-benchmark-evaluating-research-agents-that-must-search-wide-and-deep
openness: open
openness_note: The 500 tasks, their source configs and the evaluation harness are public in the perplexityai/wandr repository under the Apache-2.0 licence, and grading needs no hidden answer key.
tags:
- agent
- research
- web-search
- citations
- perplexity
updated: '2026-11-23'
sources:
- url: https://www.perplexity.ai/hub/blog/wandr-benchmark-evaluating-research-agents-that-must-search-wide-and-deep
  title: 'WANDR Benchmark: Evaluating Research Agents That Must Search Wide and Deep (Perplexity)'
  trust: 0.95
- url: https://github.com/perplexityai/wandr
  title: perplexityai/wandr — tasks and harness (Apache-2.0)
  trust: 0.9
- url: https://www.anthropic.com/claude-fable-and-mythos-5-2
  title: Introducing Claude Fable 5.2 and Claude Mythos 5.2 (Anthropic, WANDR relaxed score)
  trust: 0.8
---

## What It Tests

Whether a research agent can **find large sets of items and prove each one**.

- **Wide** — many members to discover (median 50).
- **Deep** — several records per member (median 4).
- **Evidence** — each record must be supported by a cited page.

## How It Scores

**Reference-free, evidence-verified grading** in four steps:

| Step | What happens |
|---|---|
| Solve | The system's output is normalised |
| Fetch | Every cited page is retrieved; broken fetches are retried in a browser |
| Judge | A judge resolves identities and checks every record |
| Score | Verdicts roll up into precision, recall and F1 per task |

- **Soft** — partial credit for incomplete members.
- **Hard** — only members whose full required set of records is correct count.
- **Diagnosis** — a soft-to-hard drop shows partial progress without full completion.

## Format

500 tasks; the median asks for 50 members with 4 records each (245 records). The output is a list of records with cited pages.

- **Repository** — `perplexityai/wandr`: editable task sources in `reference/wandr_tasks/`, generated Harbor task packages in `datasets/wandr/`, **Apache-2.0**.
- **Third-party pages** — cited source material stays under its own terms.

## Limitations

- **Live pages.** Grading re-fetches cited pages, which can change.
- **Judge model.** Identity and record checks use a model the write-up does not name.
- **Vendor interest.** Perplexity's own system is among those compared.
- **Score variants.** Anthropic cites an undefined 'relaxed score' that differs from hard F1.

## Example

From Perplexity's write-up, the task `ceo_cfo_appointments` begins:

> "Find at least 70 US-based companies with a CEO or CFO appointment first announced between March 1 and April 30, 2026."

- **Breadth** — at least 70 companies (members).
- **Depth** — each company needs the task's required records, each backed by a cited page.
- **Grading** — the grader opens every cited page and checks that it supports the record; recall counts any shortfall below 70 as zero.
