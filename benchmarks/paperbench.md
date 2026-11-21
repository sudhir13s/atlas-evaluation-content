---
id: benchmark/paperbench
type: benchmark
slug: paperbench
schema_version: 1.0
title: "PaperBench"
name: PaperBench
category: Science & Research
description: "A benchmark in which an agent must replicate 20 recent machine-learning papers from scratch, graded against rubrics co-developed with the papers' authors."
what_is_it: "PaperBench evaluates the ability of AI agents to replicate state-of-the-art AI research. Agents must replicate 20 ICML 2024 Spotlight and Oral papers from scratch, including understanding the paper's contributions, developing a codebase and successfully running experiments. Rubrics hierarchically break each replication into smaller sub-tasks with clear grading criteria, giving 8,316 individually gradable tasks in total."
why_people_use_it: "It measures AI engineering and research ability on a concrete, objective task: reproduce published results. The rubrics were written together with the author or authors of each paper, which supports accuracy and realism. The authors also had top ML PhDs attempt a subset and report that models do not yet outperform that human baseline."
method: "An LLM-based judge grades each replication attempt against the rubric. To check the judge, the authors built a separate benchmark for judges. The code is open source."
metric: "Average replication score in percent: how much of the paper's rubric an attempt satisfies."
format: "Agentic, long-running: one paper per task, with the agent writing a codebase and executing experiments from scratch."
limitations: "The set is small at the paper level: 20 papers, all from one conference year (ICML 2024 Spotlight and Oral). Grading relies on an LLM judge, whose accuracy the authors measure on a separate judge benchmark. Results depend on the agent scaffold, and the paper's best agent used open-source scaffolding. Replication of a paper is one narrow kind of research skill."
citation: "https://arxiv.org/abs/2504.01848"
tags: [agent, research-replication, machine-learning, rubric]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2504.01848", title: "PaperBench: Evaluating AI's Ability to Replicate AI Research (arXiv:2504.01848)", trust: 0.95 }
---

## What It Tests
Whether an agent can **reproduce a published ML paper's results** without the original code.

- **Understand** the paper's contributions.
- **Build** a codebase.
- **Run** experiments successfully.

## How It Scores
A **hierarchical rubric** per paper, applied by an LLM judge.

- **Granularity** — 8,316 individually gradable tasks across 20 papers.
- **Authorship** — rubrics co-developed with each paper's authors.
- **Judge check** — a separate benchmark tests the judge.

## Format
One ICML 2024 Spotlight or Oral paper per run, replicated from scratch. A human baseline from top ML PhDs on a subset gives a reference.

## Limitations
- **Small, single-venue set.** 20 papers from one conference year.
- **LLM judge.** Its agreement with humans is measured but not perfect.
- **Narrow skill.** Replication is one part of research work.

## Example
For each paper, the agent must replicate the work **from scratch**: understand what the paper contributes, write a codebase and run the experiments. A rubric **hierarchically decomposes** the replication into smaller sub-tasks, each with clear grading criteria, and the LLM judge scores the attempt against it.
