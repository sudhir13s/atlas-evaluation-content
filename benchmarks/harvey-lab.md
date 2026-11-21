---
id: benchmark/harvey-lab
type: benchmark
slug: harvey-lab
schema_version: 1.0
title: "Harvey LAB"
name: Harvey LAB (Legal Agent Benchmark)
category: Agent & Tool Use
description: "An open-source benchmark of about 1,250 legal agent tasks across 24 practice areas, graded all-pass against expert-written rubric criteria."
what_is_it: "The Legal Agent Benchmark (LAB) from Harvey evaluates agents on real legal work. The first version has 1,250 agent tasks across 24 legal practice areas and more than 75,000 expert-written rubric criteria. Tasks are matter-style: they give an agent instructions, documents and a workspace, and it must produce a deliverable. Harvey open-sourced both the dataset and an execution harness, under the MIT licence per the repository."
why_people_use_it: "It gives model providers, agent builders, researchers and law firms a shared way to measure progress on long-horizon legal agents. Harvey says all-pass grading reflects how high-stakes legal work is reviewed: there is no partial credit for catching most of the issues. Variants exist at Artificial Analysis (Harvey LAB-AA) and Vals AI (HLAB-Vals), which feeds the Vals Index."
method: "LAB consists of a dataset of tasks (agent instructions, documents and rubrics) and an execution harness that runs and evaluates agents. An LLM judge checks every rubric criterion. Artificial Analysis' variant runs 120 private tasks over the same 24 practice areas, adds a check of each deliverable against source documents for hallucinations, and uses a panel of three judges."
metric: "All-pass rate: a task counts only when every rubric criterion passes. Artificial Analysis' headline is a hallucination-gated all-pass rate, in which a task with a material hallucination gets no credit."
format: "Agentic: the task is a filesystem workspace with source documents; the agent returns deliverables; separate evaluation and reporting steps score them."
limitations: "All-pass grading is strict, so most partial work scores zero. Variants differ: the open LAB has 1,250 tasks, Artificial Analysis' version has 120 private tasks with a hallucination gate, and Vals AI uses its own HLAB-Vals-1.1, so numbers are not comparable. Harvey runs the project and sells legal AI, so its task mix reflects its view of legal work. Harvey says LAB is ongoing and will be extended."
citation: "https://www.harvey.ai/blog/introducing-harveys-legal-agent-benchmark"
tags: [agent, legal, rubric, all-pass, open-source]
updated: 2026-11-21
sources:
  - { url: "https://www.harvey.ai/blog/introducing-harveys-legal-agent-benchmark", title: "Introducing Harvey's Legal Agent Benchmark (Harvey)", trust: 0.95 }
  - { url: "https://github.com/harveyai/harvey-labs", title: "harveyai/harvey-labs — dataset and harness (MIT)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/evaluations/aa-briefcase", title: "Artificial Analysis evaluations list (Harvey LAB-AA v1.1)", trust: 0.85 }
  - { url: "https://www.vals.ai/benchmarks/vals_index", title: "Vals Index (HLAB-Vals-1.1 component)", trust: 0.85 }
related: [leaderboard/vals-index]
---

## What It Tests
Whether an agent can do **real legal work** across a wide range of practice areas.

- **Breadth** — 24 practice areas, covering transactional, advisory, regulatory and litigation work.
- **Long-horizon tasks** — matters with instructions, documents and deliverables.
- **Depth of grading** — over 75,000 expert-written criteria.

## How It Scores
**All-pass**: every criterion must pass.

- **Rationale** — Harvey says legal work is reviewed without partial credit.
- **Judge** — an LLM judge applies the rubric.
- **Variant** — Artificial Analysis zeroes a task with a material hallucination.

## Format
A dataset of tasks plus an execution harness, both open source. Each task is a workspace of source documents and a brief.

## Limitations
- **Strict.** Partial work scores zero.
- **Variants.** Open LAB, Artificial Analysis' 120-task variant and Vals AI's HLAB-Vals-1.1 are different setups.
- **Vendor-built.** Harvey designs the task mix.

## Example
Harvey's announcement uses a **change-of-control task**, one of the 1,250, to show the format: instructions, source documents and rubric criteria checked on a deliverable. Under all-pass grading, a deliverable that catches most issues but misses one required item still fails.
