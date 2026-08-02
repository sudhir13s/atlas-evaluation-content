---
id: benchmark/agents-last-exam
type: benchmark
slug: agents-last-exam
schema_version: 1.0
title: "Agents' Last Exam"
name: Agents' Last Exam (ALE)
category: Frontier / Hardest
description: "A UC Berkeley frontier benchmark of real professional workflows where the best computer-use agents pass only ~2.6% of tasks overall."
what_is_it: "Agents' Last Exam (ALE) is a benchmark from Berkeley RDI (the Center for Responsible Decentralized Intelligence) that grades generalist computer-use agents on 1,490 real, expert-authored professional workflows drawn from 55 occupational subdomains. Each task is a project a working professional actually completed; the agent must reproduce the deliverable inside a real desktop/CLI environment, and it is scored by deterministic scripts against the expert's own reference — not by an LLM judge."
why_people_use_it: "Existing agent benchmarks were saturating, so ALE was built to restore headroom at the professional frontier: even the strongest agent tested passes only ~2.6% of tasks overall, and the hardest tier is near 0%. Because ~90% of the task pool is held private and grounded in the O*NET occupational taxonomy across 250+ real experts, scores are hard to contaminate or game, which makes ALE a credible frontier signal for real-work agent readiness."
examples:
  - "Given a client brief, a folder of raw CSVs, and a spreadsheet template, produce the finished financial-model workbook the domain expert delivered — graded by comparing computed cell values and structure to the reference, with a gate that the file must parse and contain no formula errors."
  - "Reproduce a professional deliverable that interleaves GUI steps (navigating an app via screenshots) with CLI operations (running scripts, editing files) to reach a specified end state."
method: "Generalist computer-use agents (GUI perception via screenshots + CLI operation) run each real professional workflow inside its environment and must reach a target world state. Grading is gate-and-score: a binary precondition (the gate — e.g. 'file parses', 'no toolpath collision') must pass before a continuous quality metric is evaluated; failing the gate forces the task score to 0. Checks are deterministic (exact/hashed values, numeric tolerances, geometric distances, structural and behavioral world-state comparisons), avoiding an LLM judge wherever a deterministic alternative exists."
metric: "Full-pass rate (binary: did every gate and criterion pass) plus mean score (continuous, 0–1). Headline results are reported per difficulty tier and as an overall average."
format: "Agentic, multi-step, long-horizon. The agent operates a real computer environment (GUI + CLI); no few-shot exemplars — it receives the task brief, input files, and tool access, then works to the deliverable."
saturation: low
limitations: "It targets computer-use agents, so scores depend heavily on the harness (screenshot quality, tool orchestration, runtime substrate), not the base model alone — a weak scaffold can mask a strong model. Deterministic grading against one expert reference can penalize a correct deliverable produced a different way. The public slice is small (~150 tasks), so open reproduction is limited and most evaluation runs against the private pool the authors control. Absolute numbers are so low today that tier composition and partial-credit weighting materially shift rankings."
citation: "https://arxiv.org/abs/2606.05405"
example:
  task_type: "Financial-modeling workflow (professional deliverable)"
  input: "Client brief plus raw data files and a blank workbook template. Task: build the three-statement financial model the expert delivered — populate the driver assumptions, link the income statement, balance sheet, and cash-flow tabs, and output the requested valuation summary. The environment exposes a spreadsheet GUI and a shell."
  success:
    answer: "Agent constructs the linked workbook, computes the driver-based projections, and writes the valuation summary cells; the file parses with no formula errors and the computed values match the expert reference within tolerance."
    why: "It clears the gate (parses, no #REF!/#DIV errors) and its numeric outputs match the reference deliverable, so both the binary gate and the continuous quality score pass."
  failure:
    answer: "Agent produces a plausible-looking workbook with the right tab layout and labels, but hard-codes several projection figures instead of linking them, leaving one tab with a broken cross-sheet reference (#REF!)."
    why: "The broken reference trips the gate, which forces the task score to 0 regardless of how much of the layout looks correct — gate-and-score gives no partial credit once a precondition fails."
  checks: "Whether an agent can carry a long-horizon professional task to a deliverable that is deterministically correct end-to-end, not merely superficially plausible."
  scoring: "Gate-and-score: binary precondition gate then deterministic value/structure comparison against the expert reference; a failed gate scores 0."
tags: [agent, frontier, computer-use, long-horizon, real-world]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2606.05405", title: "Agents' Last Exam (Berkeley RDI, arXiv:2606.05405)", trust: 0.95 }
  - { url: "https://github.com/rdi-berkeley/agents-last-exam", title: "rdi-berkeley/agents-last-exam — official GitHub repository", trust: 0.9 }
  - { url: "https://rdi.berkeley.edu/blog/agents-last-exam/", title: "Agents' Last Exam — Berkeley RDI blog", trust: 0.9 }
related: [benchmark/bfcl-v4, benchmark/automationbench, benchmark/dsbench]
---

## What It Tests
Whether a **generalist computer-use agent** can finish a **real professional project** end-to-end.
Each task is a workflow a working professional actually completed, spanning **55 occupational
subdomains** across **13 top-level industry clusters**, grounded in the **SOC 2018 / O\*NET**
occupational taxonomy and authored with **250+ domain experts**. The agent needs the full stack —
LLM reasoning, GUI perception from screenshots, CLI operation, tool invocation, and control flow —
because tasks interleave clicking through an app with running scripts and editing files.

The pool is **1,490 task instances** (960 expert-authored workflows), split into difficulty tiers:

- **Near-Term** (59 tasks) — top agents reach ~30% pass rate.
- **Full-Spectrum** (55 tasks) — one instance per subdomain, a breadth sweep.
- **Last-Exam** (35 tasks) — the hardest workflows, where most agents score ~0%.

## How It Scores
The mechanism is **gate-and-score**. A **binary precondition (the gate)** — for example "the file
parses" or "no toolpath collision" — must pass first; only then is a **continuous quality metric**
evaluated. Fail the gate and the task scores **0**, no matter how much partial progress looks right.

Grading is **deterministic** wherever possible — exact or hashed values, numeric tolerances,
geometric distances, structural comparisons, and behavioral world-state checks — deliberately
avoiding an LLM-as-judge. Headline numbers are a **full-pass rate** (binary) plus a **mean score**
(0–1), reported per tier and overall.

Reported results show how brutal it is: the overall average pass rate is **2.6%**. Codex
(GPT-5.5) leads at **26.2% overall** (42.4% Near-Term, 20.0% Full-Spectrum, 8.6% Last-Exam); Claude
Code (Sonnet 4.6) reaches **17.1% overall**.

## Format
Agentic and long-horizon — no few-shot exemplars. The agent is dropped into the task's real
environment (spreadsheet/app GUI plus a shell), given the brief and input files, and must drive to
the deliverable itself. A focused **ALE-CLI** subset of **106 Linux-only tasks** lets CLI agents be
compared directly; there, Codex scores **25.2%** even though the same agent posts **82% on
Terminal-Bench** — a concrete measure of how much harder ALE is than prior agent tests.

## Limitations
Scores reflect the **agent harness** (screenshot fidelity, tool orchestration, runtime substrate),
not the base model alone, so scaffolding choices can mask or flatter a model. Deterministic grading
against a **single expert reference** can mark a correct-but-different deliverable wrong. The
**public slice is small (~150 of 1,490 tasks; ~90% stays private)**, which is what keeps the
benchmark contamination-resistant but also limits independent reproduction — most runs execute
against the authors' private pool. With absolute pass rates this low, tier composition and
partial-credit weighting move rankings noticeably, so always cite the tier.

## Example
Give an agent a client brief, raw data files, and a blank workbook template, and ask it to rebuild
the three-statement financial model the expert delivered. A **passing** run links the tabs, computes
the driver-based projections, writes the valuation summary, and produces a file that parses cleanly
with values matching the reference. A **failing** run returns a plausible workbook with the right
labels but hard-codes figures and leaves a broken cross-sheet `#REF!` — which trips the gate and
forces the score to 0, regardless of how finished the layout appears.
