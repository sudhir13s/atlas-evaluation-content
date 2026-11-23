---
id: benchmark/agents-last-exam
type: benchmark
slug: agents-last-exam
schema_version: 4.0
title: Agents' Last Exam
name: Agents' Last Exam (ALE)
category: Frontier / Hardest
description: A UC Berkeley-led benchmark of real professional workflows from 55 occupational subdomains, done by computer-use agents and graded against expert references.
what_is_it: Agents' Last Exam (ALE) is a benchmark led by the University of California, Berkeley (the rdi-berkeley group; first posted June 2026, with Yiyou Sun, Dawn Song and several hundred co-authors) that grades generalist computer-use agents on real professional workflows. The paper reports 1,490 task instances built from 960 expert-authored workflows, covering 55 subdomains grouped into 13 industry clusters and developed with 250+ industry experts. The agent must produce the deliverable inside a real desktop or command-line environment, and a script scores it against a hidden expert reference.
why_people_use_it: Tasks come from professional work across many occupations rather than from software tasks alone, and grading prefers deterministic checks over an LLM judge. Only about 10% of task instances are public, which the authors use to limit contamination.
examples:
- 'From the paper''s Section 2.1, given as a suitable task: move a running cheetah into another race video, which requires tracking, rotoscoping, compositing and colour matching.'
method: 'Generalist computer-use agents (GUI perception via screenshots plus command-line operation) run each workflow inside its environment and must reach a target deliverable or world state. Grading is gate-and-score: a binary precondition (the gate, for example ''file parses'' or ''no toolpath collision'') must pass before a continuous quality score is computed, and a failed gate forces the task score to 0. Deterministic checks are preferred; tasks whose only scoring path is an LLM judge are rejected at quality control, and where a judge is needed it answers narrow, evidence-anchored yes/no probes.'
metric: Full pass rate = tasks receiving full credit / all tasks; mean score = the mean of fine-grained task scores in [0, 1]. Results are reported per difficulty tier and overall.
format: Agentic, multi-step, long-horizon. The agent operates a real computer environment (GUI plus command line); no few-shot exemplars. It receives the task brief, input files and tool access, then works to the deliverable.
limitations: Scores depend heavily on the harness (screenshot handling, tool orchestration, runtime), not the base model alone. Deterministic grading against one expert reference can penalize a correct deliverable produced a different way. Only 150 of 1,490 instances are public, so most of the pool cannot be inspected or rerun outside the authors' group. The paper's own counts do not fully reconcile (its tier sizes sum to 160 while it also refers to a 152-task public set), and version 1.1 is a different, smaller set (122 active tasks after 30 retirements), so v1.0 and v1.1 numbers are not comparable.
citation: https://arxiv.org/abs/2606.05405
openness: partly-open
openness_note: The method, code (Apache-2.0) and a public slice of tasks (CC BY 4.0, about 150 of 1,490 instances) are released, while the remaining instances are kept private and reference outputs stay hidden until an agent finishes.
worked_examples:
- task_type: Financial-modeling workflow (professional deliverable)
  input: 'Client brief plus raw data files and a blank workbook template. Task: build the three-statement financial model the expert delivered — populate the driver assumptions, link the income statement, balance sheet, and cash-flow tabs, and output the requested valuation summary. The environment exposes a spreadsheet GUI and a shell.'
  success:
    answer: The agent links the tabs with cross-sheet formulas, computes the driver-based projections, and writes the valuation summary cells; the file parses with no formula errors and the computed values match the expert reference within tolerance.
    why: 'It clears the gate (parses, no #REF!/#DIV errors) and its numeric outputs match the reference deliverable, so both the binary gate and the continuous quality score pass.'
  failure:
    answer: The agent produces a plausible-looking workbook with the right tab layout and labels, but hard-codes several projection figures instead of linking them, leaving one tab with a broken cross-sheet reference (#REF!).
    why: The broken reference trips the gate, which forces the task score to 0 regardless of how much of the layout looks correct — gate-and-score gives no partial credit once a precondition fails.
  checks: Whether an agent can carry a long-horizon professional task to a deterministically-correct deliverable, not merely a superficially plausible one.
  scoring: 'Gate-and-score: binary precondition gate, then deterministic value/structure comparison against the expert reference; a failed gate scores 0.'
- task_type: CAD/CAM machining workflow
  input: Given a part drawing, stock dimensions, and a fixture layout, generate the CNC toolpath in the CAM application so the machined part matches the reference geometry. The environment exposes the CAM GUI and a shell for running the post-processor.
  success:
    answer: The agent selects the correct tool and stepover, generates a toolpath that reaches every feature within tolerance, and runs the collision simulation — which reports no collision — before posting G-code.
    why: It clears the 'no toolpath collision' gate and the resulting geometry matches the reference within the numeric/geometric tolerance, so both gate and quality score pass.
  failure:
    answer: The agent generates a toolpath that cuts the right profile but plunges through a clamp because it never checked the fixture, so the collision simulation flags a gouge into the workholding.
    why: The toolpath-collision gate fails, forcing the score to 0 — an end-state that would scrap the part or crash the machine, no matter how correct the cut profile looks.
  checks: Whether the agent respects a hard physical safety precondition (no collision) while still reaching the target geometry.
  scoring: 'Gate-and-score: collision-check gate, then geometric-distance comparison to the reference part.'
- task_type: Data-analysis / SQL reporting workflow
  input: Given a client database and a brief asking for quarterly revenue by region for returning customers only, produce the summary table the analyst delivered. The environment exposes a SQL client and a spreadsheet.
  success:
    answer: The agent filters to returning customers, joins orders to the customer dimension on the correct key, groups by region and quarter, and writes a table whose totals match the reference figures exactly.
    why: The output table parses and its aggregated values match the expert reference, so the deterministic value comparison passes.
  failure:
    answer: The agent runs an inner join that silently drops customers with no region record and forgets the returning-customer filter, producing a well-formatted table whose totals are inflated and off from the reference.
    why: The numbers fail the exact/tolerance value check against the reference — a subtle join-and-filter error yields a plausible but wrong deliverable, scoring near 0 on the quality metric.
  checks: Whether the agent produces analytically correct aggregates, not just a table with the right shape and headers.
  scoring: 'Gate-and-score: table-parses gate, then deterministic numeric comparison of aggregated values against the reference.'
- task_type: DevOps / service-configuration workflow
  input: Given a repository and a brief, configure and launch the web service so its /health endpoint returns 200 and it reads the required database URL from an environment variable. The environment exposes a shell and the app's config files.
  success:
    answer: The agent sets the DATABASE_URL environment variable, updates the config, restarts the service, and verifies /health returns 200 against the running process.
    why: The behavioral world-state check (service up, endpoint healthy, correct env wiring) passes, so the task is graded against the real reachable end state.
  failure:
    answer: The agent edits the config file with the correct value but never restarts the service, so the old process keeps running with the stale configuration and /health still errors.
    why: The world-state check queries the live service and sees an unhealthy endpoint — editing the file without applying it leaves the target state unreached, scoring 0.
  checks: Whether the agent drives the system to the intended runtime world state, not just to a correct-looking file on disk.
  scoring: 'Gate-and-score: behavioral world-state comparison (endpoint status and config wiring) against the target state.'
- task_type: Document-publishing workflow (GUI-heavy)
  input: Given a signed reference contract and a set of redline instructions, apply the requested clause edits in the word processor and export the final document with the specified structure (numbered sections, defined terms, signature block). The environment is GUI-driven.
  success:
    answer: The agent applies each redline, keeps the numbered-section and defined-term structure intact, and exports a document whose section tree and required clauses match the reference structure check.
    why: The structural comparison against the reference deliverable passes because every mandated clause and heading is present and correctly ordered.
  failure:
    answer: The agent edits the visible clause text correctly but deletes a required indemnification section and lets automatic renumbering shift the section labels, so the exported structure diverges from the reference.
    why: The structural world-state check detects the missing clause and mismatched numbering — a visually clean document that fails the deterministic structure comparison, scoring near 0.
  checks: Whether the agent preserves a mandated document structure end-to-end, not merely the surface wording of edited passages.
  scoring: 'Gate-and-score: document-parses gate, then structural comparison of sections/clauses against the expert reference.'
tags:
- agent
- frontier
- computer-use
- long-horizon
- real-world
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2606.05405
  title: Agents' Last Exam (arXiv:2606.05405)
  trust: 0.95
- url: https://arxiv.org/html/2606.05405
  title: Agents' Last Exam, full text (arXiv HTML)
  trust: 0.95
- url: https://agents-last-exam.org/leaderboard
  title: Agents' Last Exam — v1.1 leaderboard (data CC BY 4.0, code Apache-2.0)
  trust: 0.9
- url: https://github.com/rdi-berkeley/agents-last-exam
  title: rdi-berkeley/agents-last-exam — official GitHub repository
  trust: 0.9
related:
- benchmark/bfcl-v4
- benchmark/automationbench
- benchmark/dsbench
---

## What It Tests

Whether a **generalist computer-use agent** can finish a **real professional project** end-to-end.
Each task is a professional workflow authored with **250+ industry experts**, covering **55
subdomains** in **13 industry clusters**, anchored in the **SOC 2018** (Standard Occupational
Classification) and **O\*NET** occupational taxonomies. Tasks interleave clicking through an app
with running scripts and editing files, so the agent needs GUI perception and command-line skill.

**Task pool in the paper (v4, October 2026):**

| Part | Instances |
|---|---|
| All instances (from 960 workflows) | 1,490 |
| Public | 150 (about 10%) |
| Private | 1,017 |
| Pending quality control | 323 |

**Difficulty tiers** (paper): Near-Term 67, Full-Spectrum 55, Last-Exam 38. The paper also mentions a 152-task public set, so these counts do not fully reconcile.

**Version 1.1** (GitHub repository): 122 active tasks after 30 retirements, with a separate leaderboard; v1.0 stays on a legacy board.

## How It Scores

The mechanism is **gate-and-score**:

- **Gate** — a binary precondition such as "the file parses" or "no toolpath collision" must pass first.
- **Score** — only then is a continuous quality score in [0, 1] computed against the hidden reference.
- **Fail the gate** — the task scores **0**, however much partial progress there is.

**Checks** are deterministic where possible. Tasks that could only be scored by an LLM judge are rejected at quality control; where a judge is used, it answers narrow yes/no questions tied to evidence.

**Headline numbers:** full pass rate = tasks with full credit / all tasks, and mean score = average task score, per tier and overall.

## Format

Agentic and long-horizon, no few-shot exemplars. The agent is dropped into the task's real environment (application GUI plus a shell), given the brief and input files, and must drive to the deliverable itself.

- **ALE-CLI** — the paper evaluates command-line agents on 105 Linux-only instances; the repository's v1.1 CLI list has 86 tasks.
- **Repository** — each task is an executable `main.py` in `tasks/`; code under **Apache-2.0**, task data under **CC BY 4.0**.

## Limitations

- **System, not model.** The harness (screenshots, tool orchestration, runtime) shapes the score.
- **One reference.** A correct deliverable made a different way can be marked wrong.
- **Mostly private.** About 90% of instances are not public, which limits independent reproduction.
- **Shifting counts.** Paper revisions and v1.1 use different task sets; cite the version and tier.

## Example

The paper's Section 2.1 gives this as a suitable task: **"Move a running cheetah into another race video."**

- **Why it qualifies** — it needs tracking, rotoscoping, compositing and colour matching, a real professional video-effects workflow.
- **How a task like this is graded** — a gate (for example, the output file exists and parses) and then a deterministic comparison with the expert's hidden reference.

The public tasks themselves can be browsed in the repository's `tasks/` folder.
