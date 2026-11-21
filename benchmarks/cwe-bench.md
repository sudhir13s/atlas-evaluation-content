---
id: benchmark/cwe-bench
type: benchmark
slug: cwe-bench
schema_version: 1.0
title: "CWE-Bench"
name: CWE-bench
category: Cybersecurity
description: "A Collinear AI defensive-security benchmark: 100 held-out tasks where a coding agent must audit a real repository, find a weakness and fix it without breaking anything."
what_is_it: "CWE-bench gives a coding agent a checkout of a real open-source repository, some reproducing disclosed CVEs, and one instruction: audit the code and fix what you find. The task does not reveal the vulnerabilities or how many exist. The held-out set has 100 tasks covering 54 distinct weakness types from CWE, the weakness classification maintained by MITRE, and all 10 OWASP 2025 categories with 8 to 11 tasks each. Agents are never asked to create exploits."
why_people_use_it: "It mimics a real security audit, where nobody hands over a CVE number for a flaw no one has reported. Collinear contrasts it with benchmarks that name the CVE and weakness class. Artificial Analysis independently evaluates and publishes the results, and Google DeepMind cites it in its cybersecurity table as 'CWE-Bench v2'."
method: "A programmatic verifier runs the exploit and the repository's existing tests. A task passes only if the exploit no longer works and every pre-existing test still passes. A second track, in which a judge gives partial credit per vulnerability, exists but the leaderboard uses the deterministic gate only."
metric: "Pass@1 in percent: each task's score is the share of its rollouts that pass the deterministic verifier, averaged over the 100 tasks. Google's evaluation methodology for Gemini 4 Neon adds that ties are broken by pass@4. The leaderboard also lists average cost per rollout."
format: "Agentic, 0-shot: repository checkout plus one instruction. Languages: C/C++ 34 tasks, Go 21, JS/TS 20, Java 18, Python 4, Rust 3."
limitations: "The 100-task evaluation set is private to Collinear and Artificial Analysis, so no outsider can inspect or download it. The maker's site does not state a version; the label 'v2' comes from Google's table and blog, and the score Google reports differs from the numbers on the maker's leaderboard. One weakness target per task and 8 to 11 tasks per OWASP category make per-category results noisy. The maker's roadmap lists rubric scoring, a rotating held-out set and multi-turn audits as not yet released. Collinear also sells a separate 1,000-plus task training corpus, so it has a commercial interest."
citation: "https://cwe-bench.com"
tags: [security, defensive, vulnerability-patching, held-out, collinear]
updated: 2026-11-21
sources:
  - { url: "https://cwe-bench.com", title: "CWE-bench — Collinear AI benchmark site and leaderboard", trust: 0.9 }
  - { url: "https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-neon/", title: "Introducing Gemini 4 Neon (Google DeepMind blog)", trust: 0.7 }
  - { url: "https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf", title: "Gemini 4 Neon model evaluation (Google DeepMind)", trust: 0.7 }
related: [benchmark/cybergym, benchmark/sec-bench]
---

## What It Tests
Whether a coding agent can **find and fully fix** a security weakness in a real repository.

- **Blank slate** — the task never names the flaw or how many there are.
- **Defence only** — agents are never asked to write exploits.
- **Fix without breaking** — the exploit must be blocked and existing tests must still pass.

Memorised fixes do not help: Collinear excludes tasks that can be solved from a remembered fix alone.

## How It Scores
A **deterministic verifier** gives pass or fail per rollout.

- **Task score** — the share of rollouts that pass.
- **Leaderboard** — the average over 100 tasks, pass@1.
- **Not used for ranking** — a judge-based partial-credit track.

## Format
A repository checkout plus an instruction to audit and fix. There are 54 distinct weakness types and 8 to 11 tasks in each of the 10 OWASP 2025 categories.

## Limitations
- **Private set.** The 100 evaluation tasks are not available to outsiders.
- **Unreconciled version.** Google reports 'CWE-Bench v2'; the maker's site shows no version and different numbers.
- **Thin categories.** Each OWASP category rests on 8 to 11 tasks.
- **Commercial interest.** Collinear licenses a separate training corpus.

## Example
The site's sample task hardens the Rust library **blurhash-rs** (CVE-2023-42447). Its decoder must always return pixels or a typed error and never panic, while valid hashes keep decoding the same. The verifier runs eight inputs: three with multibyte characters, two whose decoded values fall outside a lookup table, and three normal cases. Fixing only the character check still panics on the out-of-range values, so that patch scores **0**; checking both input characters and decoded values scores **1**.
