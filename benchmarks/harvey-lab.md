---
id: benchmark/harvey-lab
type: benchmark
slug: harvey-lab
schema_version: 4.0
title: Harvey LAB
name: Harvey LAB (Legal Agent Benchmark)
category: Professional Work
description: An open-source benchmark of legal agent tasks across 24 practice areas plus contracting, graded all-pass against expert-written rubric criteria.
what_is_it: 'The Legal Agent Benchmark (LAB) from Harvey, announced on 2026-05-06, evaluates agents on real legal work. At launch Harvey described 1,250 agent tasks across 24 practice areas with more than 75,000 expert-written rubric criteria; the repository README now lists 2,010 tasks across 24 practice areas plus contracting. Tasks are matter-style: they give an agent instructions, documents and a workspace, and it must produce a deliverable. The dataset and an execution harness are open source under the MIT licence.'
why_people_use_it: 'It gives model providers, agent builders, researchers and law firms a shared way to measure progress on long-horizon legal agents. Harvey says all-pass grading reflects how high-stakes legal work is reviewed: there is no partial credit for catching most of the issues. Variants exist at Artificial Analysis (Harvey LAB-AA) and Vals AI (HLAB-Vals), which feeds the Vals Index.'
method: LAB consists of a dataset of tasks (agent instructions, documents and rubrics) and an execution harness, the lab_core package, that runs agents, grades them and builds reports. An LLM judge checks every rubric criterion. Artificial Analysis' variant runs 120 private tasks, adds a check of each deliverable for material hallucinations, and uses a panel of three judges.
metric: All-pass rate = tasks where every rubric criterion passes / tasks. Artificial Analysis' headline is a hallucination-gated all-pass rate, in which a task with a material hallucination gets no credit.
format: 'Agentic: the task is a filesystem workspace with source documents and a short instruction; the agent returns deliverables (for example a .docx memo); separate grading and reporting steps score them.'
limitations: All-pass grading is strict, so most partial work scores zero. The task set grows between releases (1,250 tasks at launch, 2,010 in the current README), and the README warns that scores from different releases are not always comparable, so a score must name its release. Artificial Analysis' version has 120 private tasks with a hallucination gate, and Vals AI uses its own HLAB-Vals-1.1, so numbers across variants are not comparable. Harvey runs the project and sells legal AI, so its task mix reflects its view of legal work.
citation: https://www.harvey.ai/blog/introducing-harveys-legal-agent-benchmark
openness: open
openness_note: The tasks (instructions, documents, rubrics) and the grading harness are published on GitHub under the MIT licence; only third-party variants such as Artificial Analysis' 120-task set are private.
tags:
- agent
- legal
- rubric
- all-pass
- open-source
updated: '2026-11-23'
sources:
- url: https://www.harvey.ai/blog/introducing-harveys-legal-agent-benchmark
  title: Introducing Harvey's Legal Agent Benchmark (Harvey, 2026-05-06)
  trust: 0.95
- url: https://github.com/harveyai/harvey-labs
  title: harveyai/harvey-labs — dataset and harness (MIT)
  trust: 0.9
- url: https://artificialanalysis.ai/methodology/intelligence-benchmarking
  title: Artificial Analysis Intelligence Benchmarking Methodology (Harvey LAB-AA v1.1)
  trust: 0.85
- url: https://www.vals.ai/benchmarks/vals_index
  title: Vals Index (HLAB-Vals-1.1 component)
  trust: 0.85
related:
- leaderboard/vals-index
---

## What It Tests

Whether an agent can do **real legal work** across a wide range of practice areas.

- **Breadth** — 24 practice areas plus contracting, covering transactional, advisory, regulatory and litigation work.
- **Long-horizon tasks** — matters with instructions, a data room of documents and a deliverable.
- **Depth of grading** — more than 75,000 expert-written criteria at launch.

## How It Scores

**All-pass**: a task counts only when **every criterion** passes.

- **Rationale** — Harvey says missing one issue can change deal economics or force rework, so catching most issues is not enough.
- **Judge** — an LLM judge applies the binary rubric criteria.
- **Artificial Analysis variant** — zeroes a task with a material hallucination, then averages over a three-judge panel.

## Format

A dataset of tasks plus an execution harness, both open source.

| Release | Tasks | Practice areas |
|---|---|---|
| Launch announcement (May 2026) | 1,250 | 24 |
| Current repository README | 2,010 | 24 plus contracting |

- **Harness** — the `lab_core` package runs an agent, scores it and builds comparison reports.
- **Release pinning** — the README asks users to name the release or commit evaluated.

## Limitations

- **Strict.** Partial work scores zero.
- **Moving set.** Task count changed between releases; compare within one release.
- **Variants.** Open LAB, Artificial Analysis' 120-task variant and Vals AI's HLAB-Vals-1.1 are different setups.
- **Vendor-built.** Harvey designs the task mix and sells legal AI.

## Example

From Harvey's announcement, a **change-of-control task**:

- **Instruction** — framed as a partner's request to an associate: review the data room, identify change-of-control provisions, assess deal risk, recommend next steps and draft a memo for the deal team and Board.
- **Matter** — a fictional $458 million acquisition of 100% of the equity of Crestview Software Solutions.
- **Data room** — eight material contracts plus adjacent files such as a 10-K, some of them irrelevant.
- **Rubric** — 57 binary criteria over nine planted issues, for example that a licence turns from exclusive to non-exclusive after a change of control, and that the memo calculates aggregate financial exposure.

Under all-pass grading, a memo that catches eight issues but misses the ninth still fails.
