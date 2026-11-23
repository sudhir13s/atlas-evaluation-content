---
id: benchmark/frontierswe
type: benchmark
slug: frontierswe
schema_version: 4.0
title: FrontierSWE
name: FrontierSWE (v2)
category: Code Generation
description: A Proximal benchmark of ultra-long-horizon technical challenges, from performance engineering to AI research, where agents work up to 20 hours per task.
what_is_it: 'FrontierSWE, from Proximal, tests coding agents on ultra-long-horizon technical challenges collected with academic and industry partners. Version 2 (September 2026, four months after version 1) has 34 tasks: it adds 21 new challenges and retires four from version 1, in areas such as visual reasoning, scientific computing, AI research, implementation and performance optimisation. Named tasks include snooker prediction, a vision-only racing bot, a Quantum ESPRESSO solver port to Rust, MEG speech decoding and medium-range weather forecasting.'
why_people_use_it: Most coding benchmarks end within minutes. FrontierSWE measures how far an agent gets on open-ended challenges that take many hours. It is also available as a Prime Intellect Environment.
method: Version 2 measures how far agents get within a 20-hour budget per task, using proximus, a minimal harness built on mini-swe-agent with context compaction, vision and a submit tool. Workspaces include a self-check tool, final verification uses hidden verifier assets, and a post-hoc anti-cheat audit sets flagged trials to 0.
metric: Per-task score from 0 to 1, averaged over 5 trials (mean@5) and then across tasks. Implementation tasks score correctness; performance tasks score 0.5 x correctness, or 0.5 + 0.5 x speedup once fully correct; ML-research tasks use the raw reward.
format: Agentic, ultra-long-horizon; open-ended technical challenges with a 20-hour budget in version 2, run five times each.
limitations: Version 2 changed the methodology and task set, so version 1 and version 2 results are not comparable. Runs are long and costly. The leaderboard awards partial credit from verifier components, while the published verifier score is stricter, so check which number a table uses. Vendor tables give one number without the harness, so check how a model was run.
citation: https://www.frontierswe.com/blog/v2
openness: partly-open
openness_note: The repository publishes 17 task directories and the scoring code, but version 2 has 34 tasks and the maker describes hidden verifier assets and hidden test cases, so the full scored set cannot be rerun from public material.
tags:
- coding
- agent
- ultra-long-horizon
- research-engineering
- proximal
updated: '2026-11-23'
sources:
- url: https://www.frontierswe.com/blog/v2
  title: FrontierSWE v2 (Proximal blog)
  trust: 0.95
- url: https://github.com/Proximal-Labs/frontier-swe
  title: Proximal-Labs/frontier-swe — repository (tasks directory)
  trust: 0.9
- url: https://raw.githubusercontent.com/Proximal-Labs/frontier-swe/main/SCORING.md
  title: FrontierSWE SCORING.md (Proximal-Labs/frontier-swe)
  trust: 0.9
- url: https://raw.githubusercontent.com/Proximal-Labs/frontier-swe/main/tasks/git-to-zig/instruction.md
  title: FrontierSWE task git-to-zig, instruction.md
  trust: 0.9
related:
- benchmark/swe-marathon
- benchmark/deepswe
---

## What It Tests

Whether an agent can make progress on **very long, open-ended engineering and research challenges**.

- **Areas** — visual reasoning, scientific computing, AI research, implementation, performance optimisation.
- **Version 2** — 34 tasks: 21 new, four version-1 tasks retired.
- **Time budget** — up to **20 hours** per task.
- **Maker** — Proximal, with academic and industry partners; v2 post dated 2026-09-02.

## How It Scores

Each trial gets a score from **0 to 1**, defined in the repository's `SCORING.md`.

- **Implementation tasks** — correctness, a value from 0 to 1 derived from the verifier's components.
- **Performance tasks** — 0.5 x correctness, or 0.5 + 0.5 x speedup once correctness is 1.0.
- **ML-research tasks** — the raw reward the verifier reports.
- **Aggregation** — mean over 5 trials (mean@5), then across tasks; the best trial is also shown.
- **Anti-cheat** — a trial flagged in the post-hoc audit scores 0.

## Format

Open-ended challenges run inside **proximus**, a harness built on mini-swe-agent that keeps agents aware of remaining time and discourages early submission.

- **Self-check** — workspaces include a tool for checking progress.
- **Hidden verification** — final scoring uses verifier assets the agent does not see.

## Limitations

- **Version break.** v1 and v2 differ in tasks and methodology.
- **Cost.** Hours of agent time per task, five trials each.
- **Partial public set.** 17 task directories are public; the v2 set has 34.
- **Two scores.** The leaderboard gives partial credit; the raw verifier score is stricter.

## Example

From the public repository, task **git-to-zig** (`tasks/git-to-zig/instruction.md`; the repository states no licence, so only a short excerpt):

> "Reimplement git in Zig as a drop-in replacement for the `git` binary. The C source for git v2.47.0 is at `/app/git-src/` — read it, understand it, and rewrite it."

- **Checked by** — comparing the Zig binary's commands, output and exit codes with the real git.
- **Other public tasks** — cranelift-codegen-opt, libexpat-to-x86asm, frogsgame-rl (scored as board count / 500).
