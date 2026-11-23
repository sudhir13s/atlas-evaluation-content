---
id: benchmark/osworld
type: benchmark
slug: osworld
schema_version: 3.1
title: "OSWorld"
name: OSWorld (OSWorld-Verified and OSWorld 2.0)
category: Agent & Tool Use
description: "A real-computer benchmark for computer-use agents: open-ended tasks across web and desktop apps, checked by execution-based scripts, with a long-horizon 2.0 successor."
what_is_it: "OSWorld is a real computer environment for multimodal agents, with task setup, execution-based evaluation and interactive learning. It was introduced in April 2024 by Tianbao Xie and colleagues at the University of Hong Kong, Salesforce Research, Carnegie Mellon University and the University of Waterloo. The original benchmark has 369 Ubuntu tasks involving real web and desktop apps, file input and output, and workflows spanning several applications, plus 43 supplementary Windows tasks. On 28 July 2025 it was upgraded to OSWorld-Verified, with fixed community-reported examples and cloud support that brings an evaluation within about an hour. OSWorld 2.0 (June 2026) is a separate set of 108 long-horizon workflows."
why_people_use_it: "It tests whether an agent can operate a computer the way a person does, from screenshots and actions instead of a purpose-built API. Each task has a custom script that inspects the end state, so results are reproducible. The original paper reports that humans complete 72.36% of tasks. OSWorld 2.0 was built because the original tasks are short and narrow; its authors say a median task takes a person about 1.6 hours."
examples:
  - "From the OSWorld paper's Table 1 (Chrome): 'Can you help me clean up my computer by getting rid of all the cookies that Amazon might have saved?' An evaluation script then checks the resulting browser state."
method: "The agent sees the computer's screen, acts through the environment, and a task-specific script checks the final state; the original paper's experiments cap a run at 15 steps. In OSWorld 2.0 the primary result is binary completion within a 500-step budget, with a partial score from task-specific checkpoints (27.25 per task on average); 2.0 also adds a simulated user with bounded task knowledge and separate safety reports."
metric: "Task success rate: tasks whose evaluation script passes / all tasks, in percent. OSWorld 2.0 reports binary task completion as the primary metric and a checkpoint-based partial score as a second number."
format: "Agentic, multi-step GUI control of a real or virtual computer. OSWorld 2.0 tasks span 31 self-hosted websites and desktop apps in seven professional domains; the 2.0 paper reports about 318 tool calls per task for one tested model, versus about 30 in OSWorld 1.0."
limitations: "Scores depend on the version: OSWorld, OSWorld-Verified and OSWorld 2.0 are different task sets, and the maker points users to the Verified results. OSWorld 1.0 tasks are short and narrow, which is why the 2.0 authors built a longer set. Eight Google Drive tasks may fail to set up for network reasons, so some evaluations run 361 tasks instead of 369, and the 43 Windows tasks need activation because of copyright restrictions. Vendor tables label some results a subset, so check which tasks were run."
citation: "https://os-world.github.io"
openness: open
openness_note: "All 369 task configurations and evaluation scripts are public in the xlang-ai/OSWorld repository (evaluation_examples), and the OSWorld 2.0 authors release its 108 tasks, environment and self-hosted websites; the Windows tasks need an activated Windows image."
tags: [agent, computer-use, gui, desktop, versioned]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2404.07972", title: "OSWorld: Benchmarking Multimodal Agents for Open-Ended Tasks in Real Computer Environments (arXiv:2404.07972)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2404.07972", title: "OSWorld paper, full text (arXiv HTML)", trust: 0.95 }
  - { url: "http://osworld-v1.xlang.ai/", title: "OSWorld — project site (OSWorld-Verified results, data links)", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2606.29537", title: "OSWorld 2.0: Benchmarking Computer Use Agents on Long-Horizon Real-World Tasks (arXiv:2606.29537)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2606.29537", title: "OSWorld 2.0, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://github.com/xlang-ai/OSWorld", title: "xlang-ai/OSWorld — official repository (tasks in evaluation_examples)", trust: 0.9 }
related: [benchmark/agents-last-exam, benchmark/terminal-bench]
---

## What It Tests
Whether an agent can **operate a real computer** to finish open-ended tasks.

| Set | Released | Tasks | Notes |
|---|---|---|---|
| OSWorld | Apr 2024 | 369 Ubuntu + 43 Windows | Web and desktop apps, files, multi-app workflows |
| OSWorld-Verified | Jul 2025 | 369 | Fixed community-reported examples, cloud evaluation |
| OSWorld 2.0 | Jun 2026 | 108 | Long-horizon workflows, median about 1.6 human hours |

2.0 targets what the original under-represents: streaming interaction, dynamic environments, cross-source reasoning, implicit-state inference and visual-spatial precision.

## How It Scores
A **task-specific execution script** inspects the final state of the computer.

- **Original** — each task has an initial-state configuration and a custom evaluation script; success rate = passed tasks / all tasks.
- **Human baseline** — 72.36% in the original paper.
- **2.0** — binary completion at a 500-step budget is primary; a partial score from task-specific checkpoints (27.25 per task on average) is secondary.
- **Safety** — 2.0 adds separate safety reports on safety-sensitive actions.

## Format
Screenshot-driven agent loops over many steps.

- **Original** — real apps such as Chrome and Thunderbird on Ubuntu; tasks in `xlang-ai/OSWorld` under `evaluation_examples`.
- **2.0** — authentic input files, stateful user profiles, 31 self-hosted websites so runs are reproducible, and a **simulated user** that answers clarifying questions only from its configured knowledge.

## Limitations
- **Three task sets.** Cite the version; Verified fixed examples in the original set.
- **Short tasks in 1.0.** That is the reason for the longer 2.0 set.
- **Subsets.** Eight Google Drive tasks can be dropped (361 tasks); the 43 Windows tasks need activation.
- **Vendor subsets.** Check which tasks a reported number used.

## Example
From the OSWorld paper's Table 1, a **Chrome** task:

> "Can you help me clean up my computer by getting rid of all the cookies that Amazon might have saved?"

- **Setup** — an initial-state configuration prepares the browser before the agent starts.
- **Agent** — works from screenshots, clicking and typing through the browser.
- **Grading** — the paper lists this task among its examples of annotated evaluation scripts: a script checks the resulting browser state, and the task passes only if that check holds.
