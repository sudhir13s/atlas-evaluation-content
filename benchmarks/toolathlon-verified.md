---
id: benchmark/toolathlon-verified
type: benchmark
slug: toolathlon-verified
schema_version: 4.0
title: Toolathlon-Verified
name: Toolathlon-Verified (The Tool Decathlon)
category: Agent & Tool Use
description: 'The verified final release of Toolathlon: 108 long-horizon tool-use tasks across 32 applications, each checked by its own evaluation script.'
what_is_it: Toolathlon (The Tool Decathlon; Li et al., October 2025, from the Hong Kong University of Science and Technology with All Hands AI, Carnegie Mellon University and Duke University) is a benchmark of 108 manually sourced or crafted tasks for language agents, spanning 32 software applications and 604 tools, from Google Calendar and Notion to WooCommerce, Kubernetes, BigQuery and Snowflake. The tools come from 32 Model Context Protocol (MCP) servers plus 7 local toolkits, and 72 of the 108 tasks start from a realistic initial state such as a Canvas course with dozens of students. Toolathlon-Verified, released on 30 June 2026, is the verified final version, with task prompts, ground truths and evaluators reviewed and aligned.
why_people_use_it: Tasks require interacting with multiple applications over around 20 turns on average, and each is verified by a dedicated, deterministic evaluation script rather than an LLM judge. The maker states that Toolathlon-Verified begins a new official score series, not directly comparable with scores from earlier versions.
examples:
- 'From the paper: an administrative agent monitors a Snowflake database for customer tickets, finds the PDF operation manual on handling overdue tickets, and sends the required emails to managers and customers.'
method: Each task has initial-state setup where needed and a hand-written evaluation script that compares the outcome with a static snapshot of the ground-truth environment or follows a reference workflow to fetch real-time data. The Verified release reviewed and aligned task prompts, ground truths and evaluators.
metric: 'Pass@1 success rate: tasks whose evaluation script passes on a single attempt / 108, in percent, shown with an error margin. The leaderboard also shows Pass@3 and Pass^3 over three attempts, without defining them on the page; Verified scores form a new series.'
format: Long-horizon agentic task over MCP-exposed applications and local toolkits; execution-based pass or fail per task.
limitations: Scores from the original Toolathlon and from Toolathlon-Verified are not directly comparable, so the version must be cited. With 108 tasks, the leaderboard reports Pass@1 with an error margin. Some evaluation scripts fetch real-time data to build the ground truth, so results can drift over time.
citation: https://arxiv.org/abs/2510.25726
openness: open
openness_note: Task definitions, initial-state setup and evaluation scripts are public in the hkust-nlp/Toolathlon repository (tasks/finalpool), and no held-out task set is described; the repository page fetched did not state a licence.
worked_examples:
- task_type: Multi-tool orchestration (Canvas gradebook + spreadsheet)
  input: 'Initial state: a Canvas course with dozens of students and a spreadsheet of raw scores. Task: ''Compute each student''s final grade using the syllabus rubric (drop the lowest quiz, weight exams 60%), then post the letter grades to Canvas.'' The agent has MCP tools for the spreadsheet and Canvas.'
  success:
    answer: Read the roster and scores, apply the rubric (drop lowest quiz, weight exams 60%), map to letter grades, and write each grade back via the Canvas tool — the final Canvas gradebook state matches the reference ground truth exactly.
    why: The agent sequenced the right tools across both apps and produced the exact target environment state the evaluation script checks.
  failure:
    answer: Compute the grades correctly in the spreadsheet but post raw percentages (not letter grades) to Canvas, or update only the students present on the first page and skip the rest due to unhandled pagination.
    why: The final Canvas state does not match ground truth (wrong grade format, or missing students), so the execution-based grader marks the task failed even though intermediate reasoning looked right.
  checks: Whether an agent can plan and execute a long, multi-app workflow and end in the exact target state, including edge cases like pagination and output format.
  scoring: 'Execution-based Pass@1: the final environment state must match reference ground truth.'
- task_type: Filtered follow-ups (Notion + Google Calendar)
  input: 'Initial state: a Notion database of invoices, some marked Paid and some Unpaid. Task: ''For every UNPAID invoice, create a Google Calendar follow-up event 3 business days from now.'' MCP tools for Notion and Calendar are available.'
  success:
    answer: 'query_notion(database=invoices) → filter status=''Unpaid'' → for each, calendar.create_event(title=''Follow up: <invoice>'', date=+3 business days) → the calendar ends with exactly one event per unpaid invoice.'
    why: Applying the Unpaid filter first means the final calendar state contains events only for the invoices the ground truth expects.
  failure:
    answer: Iterate over the whole Notion database and create a follow-up event for every invoice, paid and unpaid alike.
    why: 'Missing filter: the final calendar has extra events for Paid invoices, so the end state does not match ground truth and Pass@1 fails.'
  checks: Whether an agent applies the stated record filter before writing to a second app.
  scoring: 'Execution-based Pass@1: calendar end state must match the expected unpaid-only set.'
- task_type: Ordered write-back (WooCommerce + Google Sheets)
  input: 'Initial state: a WooCommerce store with several orders flagged for refund and a Refunds spreadsheet. Task: ''Issue the refund in WooCommerce for each flagged order, then log the refunded amount and date to the Refunds sheet.'' MCP tools for both apps.'
  success:
    answer: 'For each flagged order: woocommerce.create_refund(order_id, amount) → confirm it succeeded → sheets.append_row(order_id, amount, date). The store shows refunded orders AND the sheet logs match.'
    why: Refund first, then log — so both the WooCommerce state and the sheet reflect real, completed refunds, matching ground truth on both systems.
  failure:
    answer: Append all the refund rows to the sheet first, then attempt the WooCommerce refunds — but a couple of create_refund calls error out and are never retried.
    why: 'Wrong order and no verification: the sheet claims refunds that the store never actually issued, so the two systems disagree and the end-state check fails.'
  checks: Whether an agent orders dependent writes so the logged record reflects the actually-completed action.
  scoring: 'Execution-based Pass@1: both the WooCommerce and sheet end states must match ground truth.'
- task_type: Targeted infrastructure change (Kubernetes)
  input: 'Initial state: a cluster with a `web` deployment in both the `staging` and `default` namespaces. Task: ''Scale the web deployment in the STAGING namespace to 3 replicas.'' The agent has a kubectl-style MCP tool.'
  success:
    answer: kubectl_scale(deployment='web', namespace='staging', replicas=3) → verify get_deployment shows staging/web at 3 ready replicas.
    why: The change targets the correct namespace, so only staging/web reaches 3 replicas — exactly the target cluster state.
  failure:
    answer: kubectl_scale(deployment='web', replicas=3) without a namespace, which defaults to the `default` namespace.
    why: 'Wrong argument/target: omitting the namespace scales default/web instead of staging/web, leaving the intended deployment unchanged — the end-state check fails.'
  checks: Whether an agent supplies the correct scoping argument (namespace) for an infrastructure action.
  scoring: 'Execution-based Pass@1: the correct deployment must end at the target replica count.'
- task_type: Query-to-doc pipeline (BigQuery + Notion)
  input: 'Task: ''Query total revenue by region for last quarter in BigQuery, then write the top 3 regions by revenue (highest first) into the "Q-Review" Notion page.'' MCP tools for BigQuery and Notion.'
  success:
    answer: bigquery.run_query('SELECT region, SUM(revenue) ... GROUP BY region ORDER BY 2 DESC LIMIT 3') → take the three rows in order → notion.update_page('Q-Review', top-3 list highest-first).
    why: The aggregation, sort, and top-3 slice are correct, so the Notion page ends with exactly the expected ranked regions.
  failure:
    answer: Run the SUM-by-region query but write all regions in the database's default (unsorted) order, or list the bottom 3 by sorting ascending.
    why: 'Wrong aggregation/sort output: the Notion page content does not match the expected top-3-descending ground truth, so Pass@1 fails.'
  checks: Whether an agent correctly transforms a query result (aggregate, sort, slice) before writing it into a document.
  scoring: 'Execution-based Pass@1: the Notion page end state must match the expected ranked list.'
tags:
- agent
- tool-use
- mcp
- long-horizon
- multi-tool
- leaderboard
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2510.25726
  title: 'The Tool Decathlon: Benchmarking Language Agents for Diverse, Realistic, and Long-Horizon Task Execution (Li et al., 2025)'
  trust: 0.95
- url: https://arxiv.org/html/2510.25726
  title: The Tool Decathlon, full text (arXiv HTML)
  trust: 0.95
- url: https://github.com/hkust-nlp/Toolathlon
  title: Toolathlon — official repo (HKUST-NLP), includes Toolathlon-Verified
  trust: 0.9
- url: https://toolathlon.xyz/docs/leaderboard
  title: Toolathlon-Verified leaderboard (maker's page; new score series statement)
  trust: 0.9
related:
- benchmark/bfcl-v4
- benchmark/tau2-bench
- benchmark/vita-bench
- benchmark/terminal-bench
---

## What It Tests

Whether a language agent can carry out **long-horizon work across several real applications**.

- **Scale** — 108 tasks, 32 MCP servers with 604 tools, plus 7 local toolkits.
- **Real initial states** — 72 tasks start from a prepared state, such as a Canvas course with dozens of students.
- **Long horizons** — around 20 turns per task on average.

**Toolathlon-Verified** (30 June 2026) is the verified final release: task prompts, ground truths and evaluators were reviewed and aligned.

**Where tasks come from:** real-world websites, or crafted to match genuine user demands, in two stages — broad sourcing first, then targeted tasks for under-represented apps.

## How It Scores

Each task has a **dedicated, hand-written evaluation script**. It either compares the final environment with a **static ground-truth snapshot** or follows a reference workflow to fetch **real-time data**. No LLM judge.

- **Pass@1** = tasks passed on one attempt / 108, the main leaderboard number, shown with an error margin.
- **Pass@3 and Pass^3** — shown alongside, over three attempts.
- **New series** — the maker says Verified results are not directly comparable with earlier Toolathlon scores.

## Format

The agent gets a task, MCP tools and local toolkits, and works over many turns until it stops; the evaluation script then checks the resulting state.

- **Task files** — `tasks/finalpool` in `hkust-nlp/Toolathlon` (for example `find-alita-paper`).
- **Paper** — arXiv:2510.25726, CC BY 4.0, 21 authors.

## Limitations

- **Version matters.** Original and Verified scores are separate series.
- **Live data.** Some ground truths are fetched at evaluation time, so they can change.
- **Small set.** 108 tasks, so the leaderboard reports an error margin.

## Example

From the paper's Figure 1, Example #2 (paraphrased; the paper is CC BY 4.0):

- **Setting** — an administrative agent watches a **Snowflake database** of customer tickets.
- **Steps** — find the **PDF operation manual** on handling overdue tickets, apply it, and send the required **emails** to managers and customers.
- **Grading** — the task's evaluation script checks the emails and resulting state against ground truth.
