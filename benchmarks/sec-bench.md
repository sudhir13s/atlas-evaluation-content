---
id: benchmark/sec-bench
type: benchmark
slug: sec-bench
schema_version: 1.0
title: "SEC-bench"
name: SEC-bench (including SEC-bench Pro)
category: Agent & Tool Use
description: "A security-engineering benchmark for LLM agents: the original SEC-bench covers proof-of-concept generation and vulnerability patching, and SEC-bench Pro adds long-horizon bug hunting in browsers and Linux."
what_is_it: "SEC-bench describes itself as the first fully automated framework for evaluating LLM agents on authentic security-engineering tasks. The original release (June 2025, accepted to NeurIPS 2025) has two tasks, proof-of-concept (PoC) generation and vulnerability patching. SEC-bench Pro launched on 2026-05-01 with a V8 board and is described as a self-evolving benchmark that measures agents' ability to hunt security bugs in critical software. Its Overall board combines V8, Firefox and Linux into 344 instances."
why_people_use_it: "The tasks use real vulnerabilities and are checked by running the result, not by judging text. SEC-bench Pro adds large, hard targets with long runs, and its boards grow over time. Google DeepMind cites SEC-bench Pro among its cybersecurity benchmarks."
method: "Each Pro instance ships a Docker image, metadata, a rendered prompt and a harness that starts the container, runs the agent with a timeout and collects files for checkers. In the original benchmark a PoC succeeds only if it creates a working artifact that triggers a valid sanitizer error. Three modes vary the information given: repository only, plus description, plus sanitizer report."
metric: "Success rate in percent: the SEC-bench score on the original boards, and success out of instances on the Pro boards, shown with how many runs completed and how many timed out."
format: "Agentic, 0-shot: PoC generation or patching on a real project; the live original leaderboard uses the repository, description and sanitizer-report mode ('poc-san')."
limitations: "The site's pages show timeouts as a major factor for some systems, so a score reflects the agent's runtime as well as skill. A footnote says newer results for some Claude Opus and Mythos models are not available because of safeguard restrictions. Google's evaluation methodology for Gemini 4 Neon describes a '183-task May 2026 JavaScript engine split' scored with an LLM judge as an extra success route; that split does not appear on the maker's site, so vendor numbers cannot be matched to the board. The maker's site states no licence for the data."
citation: "https://sec-bench.github.io"
tags: [security, vulnerability-discovery, poc-generation, patching, long-horizon]
updated: 2026-11-21
sources:
  - { url: "https://sec-bench.github.io", title: "SEC-bench and SEC-bench Pro — project site and leaderboards", trust: 0.9 }
  - { url: "https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf", title: "Gemini 4 Neon model evaluation (Google DeepMind)", trust: 0.7 }
related: [benchmark/cybergym, benchmark/cwe-bench]
---

## What It Tests
Whether an agent can do **real security-engineering work** on real software.

- **PoC generation** — craft a proof of concept that triggers a specific vulnerability.
- **Vulnerability patching** — given a CVE description and the code, fix the root cause without breaking functionality.
- **Pro bug hunting** — find security bugs in V8, Firefox and Linux with no pre-labelled target.

## How It Scores
Checkers run the result and report **success rate**.

- **Original** — a PoC counts only if it triggers a valid sanitizer error.
- **Pro** — a board shows successes out of instances, plus completed and timed-out counts.
- **Boards** — V8 (103), Firefox (104), Linux (137) and Overall (344); a SpiderMonkey board launched on 2026-05-05.

## Format
The original has three information modes: `poc-repo`, `poc-desc` and `poc-san`. Current original results use `poc-san`. OSS-Fuzz instances were added in December 2025.

## Limitations
- **Timeouts.** A large share of runs for some systems time out.
- **Missing models.** Some newer Claude results are unavailable due to safeguard restrictions.
- **Vendor splits.** Google's 183-task JavaScript-engine split is not visible on the site.
- **No licence stated.** The maker's site gives no data licence.

## Example
In the original benchmark's **poc-san** mode the agent receives the vulnerable repository, a short description and the crash report from a sanitizer, and must produce a PoC input. It succeeds only if running that PoC makes the target build report a **valid sanitizer error**. In **poc-repo** mode it gets only the code and must find the bug itself.
