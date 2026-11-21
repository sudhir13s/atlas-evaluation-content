---
id: benchmark/remote-labor-index
type: benchmark
slug: remote-labor-index
schema_version: 1.0
title: "Remote Labor Index"
name: Remote Labor Index (RLI)
category: Professional Work
description: "A CAIS and Scale AI benchmark of real, economically valuable remote-work projects that measures how often an AI agent's deliverable would be accepted as commissioned work."
what_is_it: "The Remote Labor Index (RLI) is a broadly multi-sector benchmark of real-world, economically valuable projects, designed to evaluate end-to-end agent performance in practical settings. The authors are at the Center for AI Safety and Scale AI (paper submitted 2025-10-30). The paper reports that agents perform near the floor on it."
why_people_use_it: "Research benchmarks of knowledge and reasoning do not show how gains turn into economic value or automation. RLI grounds that discussion in whole projects that a client might hire a freelancer to do. It gives a common basis for tracking AI automation over time."
method: "For each project, human evaluators compare the AI deliverable with the human reference deliverable. They follow a trained workflow and choose one of three ratings: not as good or not acceptable to a reasonable client, as good and acceptable, or better than the reference. Separate pairwise comparisons between two AI deliverables produce an Elo score."
metric: "Automation rate: the percentage of projects whose AI deliverable is judged to complete the project at least as well as the human deliverable (rating 2 or 3). The leaderboard also reports Elo, a relative score."
format: "End-to-end projects with a brief and project materials; the agent submits deliverables that a human evaluator reviews."
limitations: "Grading depends on trained human evaluators, so it is slow and costly. The number of projects is not stated in the paper abstract or the project page. Automation rate counts only projects accepted as commissioned work, so it is a strict measure. Elo scores are relative to the agents tested and shift when agents are added."
citation: "https://arxiv.org/abs/2510.26787"
tags: [agent, remote-work, automation, economic-value, human-evaluation]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2510.26787", title: "Remote Labor Index: Measuring AI Automation of Remote Work (arXiv:2510.26787)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2510.26787", title: "Remote Labor Index, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://www.remotelabor.ai", title: "Remote Labor Index — project site and leaderboard", trust: 0.9 }
related: [benchmark/gdpval]
---

## What It Tests
Whether an agent can **complete a whole freelance-style project** well enough that a client would accept it.

- **Multi-sector** — real projects from many kinds of remote work.
- **End to end** — the agent produces the final deliverable itself.
- **Economic framing** — success means the work could replace a commissioned deliverable.

## How It Scores
Human evaluators rate each AI deliverable against the human reference.

- **Rating 1** — worse, not acceptable to a reasonable client.
- **Rating 2** — as good, acceptable.
- **Rating 3** — acceptable and better than the reference.

The **automation rate** is the share of projects rated 2 or 3.

## Format
A project brief plus materials in, a deliverable out. Elo is computed from pairwise comparisons of two agents' deliverables.

## Limitations
- **Human grading.** Trained evaluators, so costs and speed are limiting.
- **Strict bar.** Only commissioned-work quality counts.
- **No stated size.** The project count is not given on the abstract or the project page.

## Example
The paper's grading instructions show how a project is judged. An evaluator reviews the project materials, uses the **human deliverable as the reference for what success looks like**, and rates the AI deliverable with one of the three ratings and a written justification. The AI file counts as an automated project only if a reasonable client would have **accepted it as the commissioned work**.
