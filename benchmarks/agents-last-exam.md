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
limitations: "It targets computer-use agents, so scores depend heavily on the harness (screenshot quality, tool orchestration, runtime substrate), not the base model alone — a weak scaffold can mask a strong model. Deterministic grading against one expert reference can penalize a correct deliverable produced a different way. The public slice is small (~150 tasks), so open reproduction is limited and most evaluation runs against the private pool the authors control. Absolute numbers are so low today that tier composition and partial-credit weighting materially shift rankings. Version 1.1 is a different, smaller task set (122 tasks) with its own board, so v1.0 and v1.1 numbers are not comparable."
citation: "https://arxiv.org/abs/2606.05405"
worked_examples:
  - task_type: "Financial-modeling workflow (professional deliverable)"
    input: "Client brief plus raw data files and a blank workbook template. Task: build the three-statement financial model the expert delivered — populate the driver assumptions, link the income statement, balance sheet, and cash-flow tabs, and output the requested valuation summary. The environment exposes a spreadsheet GUI and a shell."
    success:
      answer: "The agent links the tabs with cross-sheet formulas, computes the driver-based projections, and writes the valuation summary cells; the file parses with no formula errors and the computed values match the expert reference within tolerance."
      why: "It clears the gate (parses, no #REF!/#DIV errors) and its numeric outputs match the reference deliverable, so both the binary gate and the continuous quality score pass."
    failure:
      answer: "The agent produces a plausible-looking workbook with the right tab layout and labels, but hard-codes several projection figures instead of linking them, leaving one tab with a broken cross-sheet reference (#REF!)."
      why: "The broken reference trips the gate, which forces the task score to 0 regardless of how much of the layout looks correct — gate-and-score gives no partial credit once a precondition fails."
    checks: "Whether an agent can carry a long-horizon professional task to a deterministically-correct deliverable, not merely a superficially plausible one."
    scoring: "Gate-and-score: binary precondition gate, then deterministic value/structure comparison against the expert reference; a failed gate scores 0."
  - task_type: "CAD/CAM machining workflow"
    input: "Given a part drawing, stock dimensions, and a fixture layout, generate the CNC toolpath in the CAM application so the machined part matches the reference geometry. The environment exposes the CAM GUI and a shell for running the post-processor."
    success:
      answer: "The agent selects the correct tool and stepover, generates a toolpath that reaches every feature within tolerance, and runs the collision simulation — which reports no collision — before posting G-code."
      why: "It clears the 'no toolpath collision' gate and the resulting geometry matches the reference within the numeric/geometric tolerance, so both gate and quality score pass."
    failure:
      answer: "The agent generates a toolpath that cuts the right profile but plunges through a clamp because it never checked the fixture, so the collision simulation flags a gouge into the workholding."
      why: "The toolpath-collision gate fails, forcing the score to 0 — an end-state that would scrap the part or crash the machine, no matter how correct the cut profile looks."
    checks: "Whether the agent respects a hard physical safety precondition (no collision) while still reaching the target geometry."
    scoring: "Gate-and-score: collision-check gate, then geometric-distance comparison to the reference part."
  - task_type: "Data-analysis / SQL reporting workflow"
    input: "Given a client database and a brief asking for quarterly revenue by region for returning customers only, produce the summary table the analyst delivered. The environment exposes a SQL client and a spreadsheet."
    success:
      answer: "The agent filters to returning customers, joins orders to the customer dimension on the correct key, groups by region and quarter, and writes a table whose totals match the reference figures exactly."
      why: "The output table parses and its aggregated values match the expert reference, so the deterministic value comparison passes."
    failure:
      answer: "The agent runs an inner join that silently drops customers with no region record and forgets the returning-customer filter, producing a well-formatted table whose totals are inflated and off from the reference."
      why: "The numbers fail the exact/tolerance value check against the reference — a subtle join-and-filter error yields a plausible but wrong deliverable, scoring near 0 on the quality metric."
    checks: "Whether the agent produces analytically correct aggregates, not just a table with the right shape and headers."
    scoring: "Gate-and-score: table-parses gate, then deterministic numeric comparison of aggregated values against the reference."
  - task_type: "DevOps / service-configuration workflow"
    input: "Given a repository and a brief, configure and launch the web service so its /health endpoint returns 200 and it reads the required database URL from an environment variable. The environment exposes a shell and the app's config files."
    success:
      answer: "The agent sets the DATABASE_URL environment variable, updates the config, restarts the service, and verifies /health returns 200 against the running process."
      why: "The behavioral world-state check (service up, endpoint healthy, correct env wiring) passes, so the task is graded against the real reachable end state."
    failure:
      answer: "The agent edits the config file with the correct value but never restarts the service, so the old process keeps running with the stale configuration and /health still errors."
      why: "The world-state check queries the live service and sees an unhealthy endpoint — editing the file without applying it leaves the target state unreached, scoring 0."
    checks: "Whether the agent drives the system to the intended runtime world state, not just to a correct-looking file on disk."
    scoring: "Gate-and-score: behavioral world-state comparison (endpoint status and config wiring) against the target state."
  - task_type: "Document-publishing workflow (GUI-heavy)"
    input: "Given a signed reference contract and a set of redline instructions, apply the requested clause edits in the word processor and export the final document with the specified structure (numbered sections, defined terms, signature block). The environment is GUI-driven."
    success:
      answer: "The agent applies each redline, keeps the numbered-section and defined-term structure intact, and exports a document whose section tree and required clauses match the reference structure check."
      why: "The structural comparison against the reference deliverable passes because every mandated clause and heading is present and correctly ordered."
    failure:
      answer: "The agent edits the visible clause text correctly but deletes a required indemnification section and lets automatic renumbering shift the section labels, so the exported structure diverges from the reference."
      why: "The structural world-state check detects the missing clause and mismatched numbering — a visually clean document that fails the deterministic structure comparison, scoring near 0."
    checks: "Whether the agent preserves a mandated document structure end-to-end, not merely the surface wording of edited passages."
    scoring: "Gate-and-score: document-parses gate, then structural comparison of sections/clauses against the expert reference."
tags: [agent, frontier, computer-use, long-horizon, real-world]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2606.05405", title: "Agents' Last Exam (Berkeley RDI, arXiv:2606.05405)", trust: 0.95 }
  - { url: "https://agents-last-exam.org/leaderboard", title: "Agents' Last Exam — v1.1 leaderboard", trust: 0.9 }
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

**Version 1.1 board** (agents-last-exam.org, results updated 2026-11-14):

- **Task set** — 122 tasks: 86 Linux and 36 Windows. The board cites a task-definitions commit dated 2026-10-27.
- **Metrics** — Pass Rate is the share of runs with a perfect score; Score is the average partial credit. Both average over all 122 tasks, and missing or failed runs count as zero.
- **Reading a row** — each row is a harness-and-model pair at its strongest effort level; vendor tables may report the averaged rubric Score, not the Pass Rate.
- **Licences** — dataset under CC BY 4.0, code under Apache-2.0.
- **Legacy board** — v1.0 stays on a separate page.

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
