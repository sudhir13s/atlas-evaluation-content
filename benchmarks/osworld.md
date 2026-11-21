---
id: benchmark/osworld
type: benchmark
slug: osworld
schema_version: 1.0
title: "OSWorld"
name: OSWorld (OSWorld-Verified and OSWorld 2.0)
category: Agent & Tool Use
description: "A real-computer benchmark for computer-use agents: open-ended tasks across web and desktop apps, checked by execution-based scripts, with a long-horizon 2.0 successor."
what_is_it: "OSWorld is a scalable, real computer environment for multimodal agents, supporting task setup, execution-based evaluation and interactive learning on Ubuntu, Windows and macOS. The original benchmark has 369 computer tasks involving real web and desktop apps, operating-system file input and output, and workflows that span several applications. In July 2025 it was upgraded to OSWorld-Verified, with fixed community-reported examples and cloud support that cuts an evaluation to about an hour. OSWorld 2.0 is a separate set of 108 long-horizon workflows."
why_people_use_it: "It is a standard test of whether an agent can operate a computer the way a person does, from screenshots and actions instead of a purpose-built API. Each task has a custom script that inspects the end state, so results are reproducible. OSWorld 2.0 was built because the original tasks are short and narrow; the 2.0 authors say a median task takes a skilled person about 1.6 hours."
method: "The agent sees the computer's screen, acts through the environment, and a task-specific script checks the final state. In OSWorld 2.0 the primary result is binary completion within a step budget (500 steps in the paper), with a partial-credit score from checkpoints averaging 27.25 per task, according to Snorkel AI's leaderboard page. OSWorld 2.0 also adds a simulated user and a separate safety audit."
metric: "Task success rate in percent. OSWorld 2.0 reports binary task completion as the primary metric and a partial score as a second number."
format: "Agentic, multi-step GUI control of a real or virtual computer. OSWorld 1.0 agents average about 30 steps; OSWorld 2.0 tasks need an average of 318 tool calls for one model the paper tested, across 31 self-hosted websites and professional desktop apps in seven domains."
limitations: "Scores depend on the version: OSWorld, OSWorld-Verified and OSWorld 2.0 are different task sets and the maker tells users to compare against the Verified results table. OSWorld 1.0 tasks are short and narrow, which is why the 2.0 authors built a longer set. Eight Google Drive tasks may need manual setup, so some evaluations use 361 tasks instead of 369, and 43 extra Windows tasks are supplementary because of copyright limits. Vendor tables label some results an 'offline set' or a subset, so check which tasks were run."
citation: "https://os-world.github.io"
tags: [agent, computer-use, gui, desktop, versioned]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2404.07972", title: "OSWorld: Benchmarking Multimodal Agents for Open-Ended Tasks in Real Computer Environments (arXiv:2404.07972)", trust: 0.95 }
  - { url: "https://os-world.github.io", title: "OSWorld — project site (OSWorld-Verified results)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2606.29537", title: "OSWorld 2.0: Benchmarking Computer Use Agents on Long-Horizon Real-World Tasks (arXiv:2606.29537)", trust: 0.95 }
  - { url: "https://snorkel.ai/leaderboard/os-world-2-0", title: "OSWorld 2.0 — Snorkel AI leaderboard page", trust: 0.85 }
related: [benchmark/agents-last-exam, benchmark/terminal-bench]
---

## What It Tests
Whether an agent can **operate a real computer** to finish open-ended tasks.

- **Original OSWorld** — 369 tasks on web and desktop apps, file operations and multi-app workflows.
- **OSWorld-Verified** — the July 2025 upgrade with fixed examples and faster cloud evaluation.
- **OSWorld 2.0** — 108 long-horizon workflows across professional and everyday work.

2.0 targets phenomena that the original under-represents: streaming interaction, dynamic environments, cross-source reasoning, implicit-state inference and visual-spatial precision.

## How It Scores
A **task-specific execution script** inspects the final state of the computer.

- **Original** — each task has an initial-state configuration and a custom evaluation script.
- **2.0** — binary completion at a step budget is primary; partial credit from checkpoints is secondary.
- **Safety** — 2.0 includes separate safety reports on safety-sensitive actions.

## Format
Screenshot-driven agent loops over many steps. OSWorld 2.0 tasks use authentic input files and realistic stateful user profile data, with 31 self-hosted websites so runs are reproducible.

## Limitations
- **Three task sets.** Cite the version; Verified fixed examples in the original set.
- **Short tasks in 1.0.** That is the reason for the longer 2.0 set.
- **Subsets.** Eight Google Drive tasks can be dropped (361 tasks); 43 Windows tasks are extra.

## Example
The maker's site describes the shape of an original task: a **real application**, an initial state set up by configuration, and a script that checks the result. An OSWorld 2.0 workflow is longer: it gathers information from several sources, such as a website and a desktop application, and must leave the computer in the correct final state. Task files are on the project's data page.
