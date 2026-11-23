---
id: benchmark/remote-labor-index
type: benchmark
slug: remote-labor-index
schema_version: 3.1
title: "Remote Labor Index"
name: Remote Labor Index (RLI)
category: Professional Work
description: "A CAIS and Scale AI benchmark of 240 real, paid remote-work projects that measures how often an AI agent's deliverable would be accepted as commissioned work."
what_is_it: "The Remote Labor Index (RLI) is a multi-sector benchmark of real, economically valuable freelance projects, built to evaluate end-to-end agent performance. The authors are at the Center for AI Safety (CAIS) and Scale AI (paper submitted 2025-10-30). It has 240 projects from 23 Upwork subcategories, such as video and animation, 3D modelling and CAD, audio production, web development and data analysis, representing over 6,000 hours of human work valued at over $140,000."
why_people_use_it: "Knowledge and reasoning benchmarks do not show how gains turn into economic value or automation. RLI grounds that discussion in whole projects that a client actually paid a freelancer to do, and gives a common basis for tracking AI automation over time."
method: "For each project, trained human evaluators compare the AI deliverable with the human reference deliverable and choose one of three ratings: not acceptable to a reasonable client, as good and acceptable, or better than the reference. Separate pairwise comparisons between deliverables produce an Elo score. The evaluation platform code is open source."
metric: "Automation rate = projects whose AI deliverable is rated at least as good as the human deliverable (rating 2 or 3) / projects evaluated. Elo is a relative score from pairwise comparisons with the human baseline fixed at 1,000; a 400-point gap means 10-to-1 odds of winning."
format: "End-to-end projects with a brief and input files; the agent submits deliverables (such as videos, CAD models, audio or code) that a human evaluator reviews."
limitations: "Grading depends on trained human evaluators, so it is slow and costly. 230 projects are private for quantitative evaluation and only 10 are public, so outside parties cannot rerun the official score. Automation rate counts only projects accepted as commissioned work, so it is a strict measure. Elo scores are relative to the agents tested."
citation: "https://arxiv.org/abs/2510.26787"
openness: partly-open
openness_note: "The method, evaluation platform code and a public set of 10 projects are released; the 230 projects used for the scored evaluation are private to protect personal data and prevent contamination."
tags: [agent, remote-work, automation, economic-value, human-evaluation]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2510.26787", title: "Remote Labor Index: Measuring AI Automation of Remote Work (arXiv:2510.26787)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2510.26787", title: "Remote Labor Index, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://www.remotelabor.ai", title: "Remote Labor Index — project site and leaderboard", trust: 0.9 }
related: [benchmark/gdpval]
---

## What It Tests
Whether an agent can **complete a whole freelance-style project** well enough that a client would accept it.

- **Multi-sector** — 240 projects from 23 Upwork subcategories, from game development and product design to architecture and video animation.
- **End to end** — the agent produces the final deliverable itself.
- **Economic framing** — projects took a mean of **28.9 hours** (median 11.5) and cost a mean of **$632.60** (median $200) when done by people.

## How It Scores
Trained human evaluators rate each AI deliverable against the human reference.

- **Rating 1** — worse, not acceptable to a reasonable client.
- **Rating 2** — as good, acceptable.
- **Rating 3** — acceptable and better than the reference.

The **automation rate** = projects rated 2 or 3 / projects evaluated. **Elo** comes from pairwise comparisons, with the human baseline at 1,000.

## Format
A project brief plus input files in, a deliverable out.

| Set | Projects | Use |
|---|---|---|
| Private | 230 | Quantitative evaluation (the score) |
| Public | 10 | Qualitative inspection |

## Limitations
- **Human grading.** Trained evaluators make it slow and costly.
- **Mostly private.** Only 10 of 240 projects are public.
- **Strict bar.** Only commissioned-work quality counts.
- **Relative Elo.** Elo shifts with the set of agents compared.

## Example
The paper's Figure 9 shows a successful case: an agent wrote code to build a **simple web data visualisation** that met the brief. The authors note that this kind of work is well within current agents' reach but makes up a small share of remote labour. Most projects instead ask for videos, 3D models, audio or design files, judged against the paid human deliverable.
