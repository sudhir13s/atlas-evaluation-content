---
id: benchmark/cwe-bench
type: benchmark
slug: cwe-bench
schema_version: 3.1
title: "CWE-Bench"
name: CWE-bench (v2)
category: Cybersecurity
description: "A Collinear AI defensive-security benchmark: 250 tasks (100 public, 150 held out) where a coding agent must audit a real repository, find a weakness and fix it without breaking anything."
what_is_it: "CWE-bench, built by Collinear AI, gives a coding agent a checkout of a real open-source repository and one instruction: audit the code and fix what you find, without being told the weakness or how many exist. Version 2 (the site dates it November 2, after v1 on September 25 and v0 on September 1; its leaderboard is dated November 18, 2026) has 250 tasks, 100 public and 150 held out, covering 185 distinct weakness types from CWE (Common Weakness Enumeration, MITRE's catalogue of software weakness types), grouped into ten Collinear-defined classes. Each task targets one weakness, and agents are never asked to create exploits."
why_people_use_it: "It mimics a real security audit, where nobody hands over a CVE number for a flaw no one has reported. Collinear excludes tasks that can be solved from memorized upstream fixes alone. Google DeepMind cites it as 'CWE-Bench v2' and says the leaderboard is independently evaluated with Artificial Analysis."
method: "Each model gets four rollouts per task at high reasoning effort, with a one-hour limit per rollout; a rollout that hits the limit is graded on the code as it stands, and rollouts lost to infrastructure errors are rerun. A deterministic, programmatic verifier checks that the exploit is blocked and legitimate behaviour still holds. LLM judge panels were used only while building the verifiers; reported scores are programmatic."
metric: "Binary pass or fail per rollout, with no partial credit; the leaderboard reports pass@1 and pass@4 over the four rollouts in percent, plus average cost per rollout. Google's methodology says models are ranked by pass@1 with ties broken by pass@4. The site does not give a formal formula for pass@1."
format: "Agentic, 0-shot: repository checkout plus one audit-and-fix instruction. Languages over 250 tasks: C/C++ 58, JS/TS 54, Go 50, Java 40, Python 35, Rust 13."
limitations: "The 150 held-out tasks are not released, so leaderboard numbers cannot be fully reproduced outside the maker's runs. Version 2 changed the task set and taxonomy (v1 used the OWASP Top 10 2025 and 73 CWEs), so compare scores only within one version. The ten weakness classes are Collinear's own grouping, not an official taxonomy, and each class holds only 17 to 40 tasks. Collinear also sells a separate 4,000-plus task training corpus, so it has a commercial interest."
citation: "https://cwe-bench.com"
openness: partly-open
openness_note: "The 100-task public set, the grader and the harness are open on Hugging Face, while the 150-task held-out set used on the leaderboard is not released."
tags: [security, defensive, vulnerability-patching, held-out, collinear]
updated: 2026-11-23
sources:
  - { url: "https://cwe-bench.com", title: "CWE-bench — Collinear AI benchmark site and leaderboard", trust: 0.95 }
  - { url: "https://storage.googleapis.com/deepmind-media/gemini/gemini_4_neon_model_evaluation.pdf", title: "Gemini 4 Neon model evaluation (Google DeepMind)", trust: 0.7 }
related: [benchmark/cybergym, benchmark/sec-bench]
---

## What It Tests
Whether a coding agent can **find and fully fix** a security weakness in a real repository.

- **Blank slate** — the task never names the flaw or how many there are.
- **Defence only** — agents are never asked to write exploits.
- **Fix without breaking** — the exploit must be blocked and legitimate behaviour must still work.
- **No memorized fixes** — tasks solvable by copying the known upstream patch are excluded.

## How It Scores
A **deterministic verifier** gives pass (1) or fail (0) per rollout, with no partial credit.

- **Rollouts** — four per task, one hour each, at high reasoning effort.
- **Reported** — pass@1 and pass@4, plus average cost per rollout.
- **Ranking** — by pass@1, ties broken by pass@4 (per Google's methodology).

## Format
A repository checkout plus an instruction to audit and fix. The ten Collinear weakness classes and their task counts (all 250 tasks):

| Class | Tasks |
|---|---|
| Injection | 27 |
| Input validation and parsing | 25 |
| Memory and resource safety | 40 |
| Access control | 29 |
| Authentication and sessions | 23 |
| Cryptography and secrets | 28 |
| Trust boundaries and request handling | 18 |
| Supply chain and data integrity | 18 |
| Information exposure and logging | 25 |
| Error handling and logic flaws | 17 |

## Limitations
- **Held-out set.** 150 of the 250 tasks are private.
- **Version changes.** v2 replaced v1's OWASP grouping and widened coverage from 73 to 185 CWEs.
- **Own taxonomy.** The classes are Collinear's, "in the spirit of OWASP".
- **Commercial interest.** Collinear licenses a separate training corpus.

## Example
The site's sample task, labelled a **training example** from a real vulnerability (not an evaluation item), hardens the Rust library **blurhash-rs** (CVE-2023-42447). Its `decode` function must never panic on malformed input, while valid hashes decode exactly as before. The verifier runs eight inputs: three with multibyte characters, two whose decoded values fall out of range (such as `00~~~~`), two valid hashes and one invalid hash that must return a normal error. A fix that only checks characters still panics on the out-of-range values and scores **0**; one that also validates ranges scores **1**.
