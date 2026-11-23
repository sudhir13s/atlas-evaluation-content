---
id: benchmark/sec-bench
type: benchmark
slug: sec-bench
schema_version: 3.1
title: "SEC-bench"
name: SEC-bench (including SEC-bench Pro)
category: Cybersecurity
description: "A security-engineering benchmark for LLM agents: the original SEC-bench covers proof-of-concept generation and vulnerability patching, and SEC-bench Pro adds proof-of-concept tasks on the V8 and SpiderMonkey JavaScript engines."
what_is_it: "SEC-bench, by Hwiwon Lee, Ziqi Zhang, Hanxiao Lu and Lingming Zhang (arXiv, June 2025; accepted to NeurIPS 2025), is an automated framework that builds security-engineering tasks for LLM agents from real vulnerabilities, at a reported cost of $0.87 per instance. It has two tasks, proof-of-concept (PoC) generation and vulnerability patching; OSS-Fuzz instances were added in December 2025. SEC-bench Pro, by Lee, Liu, Kim, Zhang, Xia and Zhang, launched a V8 board on 2026-05-01 and a SpiderMonkey (Firefox) board on 2026-05-05; its Overall board combines the two into 183 instances."
why_people_use_it: "The tasks use real vulnerabilities and are checked by running the result, not by judging text. SEC-bench Pro targets large JavaScript engines and ships every instance as a container, so anyone can rerun it. Google DeepMind cites SEC-bench Pro among its cybersecurity benchmarks."
method: "In PoC tasks a submission succeeds only if it creates a working PoC artifact that triggers a valid sanitizer error (a report from memory-error detectors built into the program). The original benchmark has three information modes: repository only, plus a description, plus a sanitizer report. Each Pro instance ships a Docker image, metadata, a rendered prompt and a harness that starts the container, runs the agent under a timeout and collects outputs for the checkers."
metric: "Success rate in percent = successful instances / instances attempted, shown on the Pro boards as a fraction, for example 49/103."
format: "Agentic, 0-shot: PoC generation or patching on a real project. The original leaderboard uses the repository, description and sanitizer-report mode ('poc-san'). Pro boards: V8 (103 instances), SpiderMonkey/Firefox (80), Overall (183)."
limitations: "The harness runs each agent under a timeout, so a score reflects runtime limits as well as skill. Some board entries are marked as covering only a subset of a run, so check how many instances an entry solved out of. Google's evaluation methodology for Gemini 4 Neon reports a '183-task May 2026 JavaScript engine split' and adds an LLM judge as an extra success route when a PoC crashes both builds, which is not the maker's checker, so its numbers are not directly comparable with board entries."
citation: "https://sec-bench.github.io"
openness: open
openness_note: "The SEC-bench Pro instances (183, Apache-2.0 on Hugging Face) and the evaluation harness (GitHub) are public, so anyone can rerun the tasks."
tags: [security, vulnerability-discovery, poc-generation, patching, long-horizon]
updated: 2026-11-23
sources:
  - { url: "https://sec-bench.github.io", title: "SEC-bench and SEC-bench Pro — project site and leaderboards", trust: 0.95 }
  - { url: "https://arxiv.org/abs/2506.11791", title: "SEC-bench: Automated Benchmarking of LLM Agents on Real-World Software Security Tasks (arXiv:2506.11791)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/SEC-bench/SEC-bench-Pro", title: "SEC-bench/SEC-bench-Pro — Hugging Face dataset (Apache-2.0)", trust: 0.9 }
  - { url: "https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf", title: "Gemini 4 Neon model evaluation (Google DeepMind)", trust: 0.7 }
related: [benchmark/cybergym, benchmark/cwe-bench]
---

## What It Tests
Whether an agent can do **real security-engineering work** on real software.

- **PoC generation** — craft a proof of concept (an input or script) that triggers a specific vulnerability.
- **Vulnerability patching** — fix the root cause without breaking functionality.
- **Pro** — PoC tasks on two large JavaScript engines: V8 (Chromium) and SpiderMonkey (Firefox). The site also announced a Linux board on 2026-06-17 but shows no instance count for it.

## How It Scores
Checkers run the result and report **success rate**.

- **PoC rule** — a PoC counts only if it triggers a valid sanitizer error.
- **Pro boards** — successes out of instances, such as 49/103.
- **Subset entries** — some entries are marked as covering only part of a run.

## Format
The original has three information modes: `poc-repo` (code only), `poc-desc` (plus a description) and `poc-san` (plus a sanitizer report). The original leaderboard uses `poc-san`.

| Pro board | Instances | Launched |
|---|---|---|
| V8 | 103 | 2026-05-01 |
| SpiderMonkey (Firefox) | 80 | 2026-05-05 |
| Overall | 183 | 2026-06-17 |

Each Pro row lists the project, bug ID, container image, target source files, vulnerability type, error type, description and fix patches.

## Limitations
- **Timeouts.** Agents run under a time limit, which shapes scores.
- **Subset runs.** Read the solved-out-of count, not just the percentage.
- **Vendor judge.** Google's runs add an LLM-judge success route the maker's checker does not use.

## Example
From the public **SEC-bench Pro** dataset on Hugging Face: one **V8** instance is labelled with the vulnerability type **type confusion**. The agent gets the container with the vulnerable engine build and a description, and must write a PoC. It succeeds only if running that PoC makes the build report a **valid sanitizer error**. Exploit code is not reproduced here.
