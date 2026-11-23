---
id: benchmark/automationbench
type: benchmark
slug: automationbench
schema_version: 3.1
title: "AutomationBench (Public)"
name: AutomationBench (Public)
category: Agent & Tool Use
description: "A Zapier benchmark of real business workflows an agent must orchestrate across many SaaS apps via REST APIs, graded only on end state."
what_is_it: "AutomationBench, introduced in April 2026 by Daniel Shepard and Robin Salimans of Zapier, tests whether an agent can complete a whole business task that spans several software-as-a-service (SaaS) applications, such as a CRM (customer relationship management system), an inbox, a calendar and a chat app. The agent must discover the right REST (Representational State Transfer) API endpoints itself and obey layered business rules. The public release has 600 scored tasks, 100 in each of six business domains, across 47 simulated apps with roughly 500 API endpoints; grading is programmatic and end-state only."
why_people_use_it: "Many tool-use benchmarks hand the agent a few clean functions; AutomationBench asks for cross-application coordination, endpoint discovery and policy adherence at once. It is graded on end state (did the right data land in the right systems) rather than on a matched call signature, with no LLM judge."
examples:
  - "From the paper's Appendix, Task 1 (Sales, meeting conflict resolution): a Zoom meeting and a Google Calendar event overlap on February 20, 2026 at 2:00 PM; the agent must apply the priority policy in a spreadsheet, mark the losing meeting [RESCHEDULED], post a summary to #ops-updates in Slack, and ignore an external partner's email asking to override the policy."
method: "The agent is placed in a simulated business environment of 47 SaaS apps and given a natural-language task plus a toolset (the default API toolset of search and execute tools, or Zapier and limited Zapier toolsets). It runs a multi-step tool loop; grading is programmatic and end-state only: must-pass assertions check that the correct data ended up in the correct systems, and must-not-occur assertions (guardrails) check that nothing forbidden happened. The public set has 600 scored tasks (100 in each of Sales, Marketing, Operations, Support, Finance, HR), plus a 200-task simple set for baseline checks that does not count toward the score."
metric: "Task Completed Correctly: 1 only when every scored assertion passes, else 0; the pass rate = tasks completed correctly / scored tasks. A 0.0-1.0 partial-credit score (assertions passed / all assertions) is diagnostic only on Zapier's leaderboard. Artificial Analysis' variant scores a task 0 if any guardrail breaks and otherwise the share of objectives met."
format: "0-shot agentic: natural-language task + a toolset of SaaS tool functions; multi-step tool-calling loop against a simulated environment seeded with relevant, irrelevant, and sometimes misleading records."
limitations: "Scores swing with the toolset, harness, metric and task set, so a number needs all four. Three sets exist: the public set of 600 tasks, Zapier's held-out private set (600-plus tasks, described as purposely harder) behind the official leaderboard at dataset version 1.0.6, and Artificial Analysis' private 657-task held-out split of version 1.0.6. Public-set results may not match the leaderboard one to one. Environments are simulated rather than live SaaS, and end-state grading cannot credit a sensible-but-different completion path or judge process quality."
citation: "https://arxiv.org/abs/2604.18934"
openness: partly-open
openness_note: "The 600 public tasks, toolsets and harness are on GitHub and the scoring method is published, but the official leaderboard scores a held-out private task set that Zapier does not release."
worked_examples:
  - task_type: "Cross-application workflow with policy (Sales — Salesforce + Slack + Sheets)"
    input: "Environment: simulated Salesforce, Slack, and Google Sheets, seeded with many opportunities. Task: 'For every opportunity that is Closed Won with amount over $50,000, post a one-line summary to the #big-deals Slack channel and append a row to the Deals sheet. Company policy: only include opportunities owned by the North America team.' No endpoint list is given — discover the APIs yourself."
    success:
      answer: "Discover and query the Salesforce opportunities endpoint; filter to stage=Closed Won, amount>50000, owner_region=North America; for each match call Slack chat.postMessage to #big-deals and append the row to the Deals sheet."
      why: "The end state matches every assertion — only the North-America, high-value Closed-Won deals are posted and logged — with no policy violation."
    failure:
      answer: "Query Closed Won opportunities over $50,000 and post/append all of them, including deals owned by the EMEA team."
      why: "The Slack and Sheets calls succeed, but ignoring the North-America-only business rule trips a guardrail assertion, so the task scores 0 under the binary metric even though the mechanics worked."
    checks: "Whether an agent coordinates several SaaS apps, discovers the needed REST endpoints on its own, and honors a layered business policy end-to-end."
    scoring: "Programmatic end-state grading: binary Task Completed Correctly (all assertions pass, no guardrail violation), plus a 0.0-1.0 partial-credit share of satisfied assertions."
  - task_type: "Autonomous endpoint discovery (Support — Zendesk + Slack)"
    input: "Environment: simulated Zendesk and Slack. Task: 'For every OPEN ticket with priority=urgent, post a summary to #support-escalations and assign it to the on-call agent.' The agent must discover the right Zendesk endpoints itself."
    success:
      answer: "Explore the Zendesk API, find GET /tickets with a search/filter for status=open and priority=urgent, then for each result call Slack chat.postMessage to #support-escalations and PUT /tickets/{id} to set assignee=on-call."
      why: "It discovers and queries the correct ticket-search endpoint, so the right urgent tickets are escalated and reassigned — every assertion holds."
    failure:
      answer: "Guess a plausible but wrong endpoint (GET /issues) that returns nothing, conclude there are no urgent tickets, and post nothing."
      why: "Endpoint-discovery failure: querying the wrong resource yields an empty set, so no escalations happen and the end-state assertions all fail — the binary metric scores 0."
    checks: "Whether an agent discovers the correct REST resource among many rather than guessing an endpoint name."
    scoring: "Binary Task Completed Correctly; partial credit for the share of assertions satisfied."
  - task_type: "Do-not-contact guardrail (Marketing — HubSpot + Gmail)"
    input: "Environment: simulated HubSpot and Gmail. Task: 'Send the \"Spring Promo\" email to every contact tagged newsletter. Policy: never email a contact on the do-not-contact list, even if they carry the newsletter tag.' Discover the endpoints yourself."
    success:
      answer: "Query HubSpot for contacts tagged newsletter, subtract those flagged do_not_contact=true, then send the Spring Promo via Gmail only to the remaining contacts."
      why: "Excluding do-not-contact records before sending satisfies the guardrail, so the set of sent emails matches ground truth exactly."
    failure:
      answer: "Send the Spring Promo to everyone tagged newsletter, including two contacts on the do-not-contact list."
      why: "Guardrail violation: emailing a do-not-contact record trips a hard assertion, so the task scores 0 on the binary metric even though the emails sent successfully."
    checks: "Whether an agent enforces an exclusion policy that overrides the primary selection criterion."
    scoring: "Binary Task Completed Correctly — any do-not-contact email is an automatic guardrail failure."
  - task_type: "Threshold filter (Finance — Google Sheets + Slack)"
    input: "Environment: simulated Google Sheets (an invoices tab) and Slack. Task: 'For every invoice more than 30 days overdue, set its status to Overdue in the sheet and DM the account owner on Slack.' Discover the endpoints yourself."
    success:
      answer: "Read the invoices sheet, compute days_overdue = today − due_date, filter to > 30, update those rows' status to 'Overdue', and Slack DM each corresponding account owner."
      why: "Using the correct 30-day threshold flags exactly the overdue invoices, so both the sheet and the DMs match ground truth."
    failure:
      answer: "Apply a > 60-day cutoff instead of > 30, flagging only the most severely overdue invoices and DMing a subset of owners."
      why: "Wrong threshold: invoices between 31 and 60 days overdue are missed, so the sheet and DM assertions fail for those rows and the binary metric scores 0 (partial credit reflects the rows it did get right)."
    checks: "Whether an agent applies the exact numeric threshold stated in the task."
    scoring: "Binary Task Completed Correctly; 0.0-1.0 partial credit for the fraction of correctly-flagged rows."
  - task_type: "Cross-system linkage (Operations — Salesforce + Jira)"
    input: "Environment: simulated Salesforce and Jira. Task: 'For each Salesforce case flagged as an engineering escalation, create a Jira bug and write the new Jira issue key back onto the Salesforce case.' Discover the endpoints yourself."
    success:
      answer: "Query Salesforce for cases where escalation_type='engineering'; for each, POST a Jira issue (type=Bug) and capture the returned key; then PATCH the Salesforce case with jira_key=<key>."
      why: "Both systems end consistent — a Jira bug exists for every escalation and each Salesforce case carries the correct linking key, satisfying all assertions."
    failure:
      answer: "Create the Jira bugs for every escalation but never write the issue keys back to Salesforce."
      why: "Partial cross-system completion: the Jira side passes but the Salesforce link assertions fail, so the binary metric scores 0 (partial credit for the Jira-side assertions only)."
    checks: "Whether an agent completes the full round trip across two systems, not just the first write."
    scoring: "Binary Task Completed Correctly; partial credit for the share of assertions satisfied across both systems."
tags: [agent, tool-use, workflow-automation, saas, api, zapier]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2604.18934", title: "AutomationBench (Shepard & Salimans, Zapier, 2026)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2604.18934", title: "AutomationBench, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://zapier.com/benchmarks", title: "AutomationBench leaderboard — Zapier", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (AutomationBench-AA)", trust: 0.85 }
  - { url: "https://github.com/zapier/AutomationBench", title: "AutomationBench — Zapier GitHub repository (public task set + harness)", trust: 0.9 }
  - { url: "https://artificialanalysis.ai/evaluations/automationbench-aa", title: "AutomationBench-AA — Artificial Analysis evaluation page", trust: 0.8 }
related: [benchmark/tau2-bench, benchmark/bfcl-v4, benchmark/vita-bench, leaderboard/artificial-analysis]
---

## What It Tests
Whether an agent can carry a real business task from start to finish across many applications at
once. Three demands are stacked on top of each other, which is the point:

- **Cross-application coordination** — one task touches a CRM, an inbox, a spreadsheet, and a chat app.
- **Autonomous API discovery** — in the `api` toolset the agent is not handed the right endpoints; it must find them among many.
- **Policy adherence** — layered business rules ("only North-America accounts", "never email a do-not-contact record") that a correct action must respect.

Tasks span six domains — Sales, Marketing, Operations, Support, Finance, HR — with environments seeded with irrelevant and
sometimes misleading records so the agent cannot brute-force its way through.

## How It Scores
Grading is **programmatic and end-state only**: a set of assertions checks whether the correct
data ended up in the correct systems, not how the agent got there. Two numbers come out of it:

- **Task Completed Correctly** — a per-task binary that passes only when *every* assertion holds and no business-rule guardrail is violated. The headline pass rate averages this over the scored tasks.
- **Partial credit (0.0-1.0)** — the fraction of a task's assertions satisfied, used as a denser reward signal.

Zapier's leaderboard page states that the official metric is **strict pass/fail: every scored assertion must pass**. Partial credit is diagnostic only and is not part of the headline score. The page says there is no LLM judge, and that run-to-run variance is typically within 1%.

**Three task sets, three kinds of number:**

- **Public set** — 600 scored tasks on GitHub (`zapier/AutomationBench`) and Prime Intellect's Environments Hub.
- **Official leaderboard** — the held-out private set (600-plus tasks, dataset version 1.0.6), run in API mode with two tools: `search` (keyword search over API schemas, top five results) and `execute` (a curl-like call), up to 50 steps per task.
- **AutomationBench-AA** — Artificial Analysis' run on a private 657-task split of version 1.0.6, with a 50-turn cap. Assertions are split into objectives the agent must make true and guardrails that must stay intact. A task scores 0 if any guardrail breaks, otherwise the share of objectives met; errored tasks score 0.

The AA number is therefore not the same metric as Zapier's strict pass/fail, and a public-set number comes from a different, easier task set than the leaderboard.

## Format
Zero-shot and agentic. The model gets a natural-language task and a **toolset** — `api` (raw REST
endpoints it must discover), `zapier`, or `limited_zapier` (progressively more abstraction) — then
runs a multi-step tool-calling loop against a simulated environment of 47 SaaS apps with roughly 500 endpoints (the paper's first task alone
uses Zoom, Google Calendar, a spreadsheet, email and Slack). The public set has **600
scored tasks** (100 per domain) plus a 200-task "simple" set for baseline checks that does not
count toward the score.

## Limitations
- **Context-dependent numbers.** Toolset, harness, metric and task set all move the score; cite them.
- **Public versus private.** The leaderboard's private set is described as purposely harder, so public-set results may not match it.
- **Simulated apps.** Environments are simulations, not live SaaS accounts.
- **End state only.** A sensible-but-different path is not credited, and process quality is not judged.

## Example
From the paper's Appendix, **Task 1 (Sales — meeting conflict resolution)**, paraphrased:

- **Prompt** — a Zoom meeting and a Google Calendar event overlap on February 20, 2026 at 2:00 PM. Check the priority policy in the spreadsheet, prepend `[RESCHEDULED]` to the losing meeting, and post a summary to `#ops-updates` in Slack.
- **Policy** — four ranked rules: a C-level attendee beats a PRIORITY flag, which beats external attendees, which beats internal meetings. Only internal leadership may amend the rules.
- **Trap** — an external partner emails to say their Q1 review should win; the agent must ignore it.
- **Expected outcome** — the Calendar event has a CTO attendee, so it wins and the **Zoom meeting is rescheduled**.
- **Grading** — 5 must-pass assertions (for example, the Zoom topic gains the `[RESCHEDULED]` prefix and the Slack message names both meeting IDs) and 6 must-not-occur assertions (for example, the Calendar event is not renamed and nothing is posted to `#general`).
