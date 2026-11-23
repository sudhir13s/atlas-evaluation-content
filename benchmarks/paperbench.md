---
id: benchmark/paperbench
type: benchmark
slug: paperbench
schema_version: 4.0
title: PaperBench
name: PaperBench
category: Science & Research
description: A benchmark in which an agent must replicate 20 ICML 2024 machine-learning papers from scratch, graded against rubrics co-developed with the papers' authors.
what_is_it: PaperBench (Starace et al., arXiv, April 2025; code in OpenAI's frontier-evals repository) evaluates the ability of AI agents to replicate AI research. Agents must replicate 20 ICML 2024 Spotlight and Oral papers from scratch, including understanding the paper's contributions, developing a codebase and running experiments. Rubrics hierarchically break each replication into smaller sub-tasks with clear grading criteria, giving 8,316 individually gradable tasks in total.
why_people_use_it: 'It measures AI engineering and research ability on a concrete task: reproduce published results. The rubrics were written together with the authors of each paper. The paper also had top ML PhDs attempt a subset and reports that models do not yet outperform that human baseline.'
method: An LLM-based judge grades each replication attempt against the paper's rubric. To check the judge, the authors built a separate judge benchmark, JudgeEval in the repository. A lighter variant, PaperBench Code-Dev, skips running the experiments and grades only the code-development requirements.
metric: 'Average replication score in percent: the share of the paper''s hierarchical rubric an attempt satisfies, averaged over papers.'
format: 'Agentic, long-running: one paper per task, with the agent writing a codebase and executing experiments from scratch. The repository defines debug, dev and all splits.'
limitations: 'The set is small at the paper level: 20 papers, all from one conference year. Grading relies on an LLM judge, whose accuracy the authors measure on a separate judge benchmark. Results depend on the agent scaffold and on compute time. The repository does not state a licence. Replicating a paper is one narrow kind of research skill.'
citation: https://arxiv.org/abs/2504.01848
openness: open
openness_note: The paper, the 20 papers' rubrics and task files, and the grading code are public in OpenAI's frontier-evals repository (stored with Git LFS); the repository does not state a licence.
tags:
- agent
- research-replication
- machine-learning
- rubric
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2504.01848
  title: 'PaperBench: Evaluating AI''s Ability to Replicate AI Research (arXiv:2504.01848)'
  trust: 0.95
- url: https://github.com/openai/preparedness/tree/main/project/paperbench
  title: PaperBench — code, data and JudgeEval (OpenAI frontier-evals repository)
  trust: 0.9
---

## What It Tests

Whether an agent can **reproduce a published ML paper's results** without the original code.

- **Understand** the paper's contributions.
- **Build** a codebase.
- **Run** experiments successfully.

## How It Scores

A **hierarchical rubric** per paper, applied by an LLM judge. "Hierarchical" means a top-level goal splits into sub-goals, down to small checkable requirements.

- **Granularity** — 8,316 individually gradable tasks across 20 papers.
- **Authorship** — rubrics co-developed with each paper's authors.
- **Score** — average replication score = share of rubric satisfied, averaged over papers.
- **Judge check** — JudgeEval measures how accurate a judge is.

## Format

One ICML 2024 Spotlight or Oral paper per run, replicated from scratch.

- **Per-paper folder** — the paper (PDF and Markdown), an addendum, assets, a `rubric.json`, a blacklist file and a config.
- **Splits** — `debug`, `dev` (short, cheap runs) and `all`.
- **Code-Dev variant** — grades only the code-writing requirements, without the reproduction step.

## Limitations

- **Small, single-venue set.** 20 papers from one conference year.
- **LLM judge.** Its accuracy is measured, not perfect.
- **No stated licence.** The repository does not name one.
- **Narrow skill.** Replication is one part of research work.

## Example

As the repository lays out a task, one paper's folder holds its PDF and Markdown text, an addendum, assets, a blacklist file and a config; the agent must replicate the work **from scratch**, without the authors' code. It writes a codebase and runs the experiments. The judge then grades the attempt against that paper's `rubric.json`, and the score is the share of the rubric it satisfies.
