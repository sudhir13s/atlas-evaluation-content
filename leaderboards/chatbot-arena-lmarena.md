---
id: leaderboard/chatbot-arena-lmarena
type: leaderboard
slug: chatbot-arena-lmarena
schema_version: 2.1
title: Chatbot Arena (LMArena)
name: Chatbot Arena (LMArena)
maintainer: LMSYS Org + UC Berkeley SkyLab
url: https://lmarena.ai
tagline: Crowdsourced head-to-head — the closest thing to a real user vote.
update_cadence: Live
coverage: 150+ models, 2M+ votes
free_access: true
tags:
- leaderboard
updated: '2026-11-19'
sources:
- url: https://lmarena.ai
  title: Chatbot Arena (LMArena)
---

## What It Ranks

Users submit a prompt, get two anonymous model responses side-by-side, and pick the one they prefer (or tie). Votes accumulate into an ELO-style ranking, same math chess uses. Model identities are revealed only after the vote.

### Why You Need It

It is the only ranking that directly measures real human preference on open-ended, freely-chosen prompts. Every other benchmark uses a fixed test set with a fixed right answer — which is a tiny slice of how people actually use these models.

### Why It Matters

Hard to game. You cannot train on the test set because there is no test set. New prompts arrive every second from real users in the wild. If a model consistently wins against GPT-4 on diverse prompts, that signal is close to irreducible.

## How To Read It

### Methodology

- Blind pairwise voting — user sees responses A/B without model names
- Bradley-Terry / ELO aggregation across millions of votes
- Separate boards for Hard Prompts, Coding, Math, Multi-turn, Long Context, Vision, Style Control
- Style Control board regresses out response-length and markdown-formatting bias

### Strengths

- Real human judgment at scale (2M+ votes)
- Prompt diversity — covers long tail of real-world tasks
- Updated live, new models appear within days
- Multiple sub-boards let you isolate coding vs. reasoning vs. vision

### Best For

- Picking a general chatbot
- Gut-check against marketing claims
- Tracking frontier movement week-to-week

## Caveats

- Preference ≠ correctness — a confidently-wrong answer can win
- Voter demographic skews to AI-curious power users
- Verbose, well-formatted answers get upvoted (partially fixed by Style Control board)
- Cannot evaluate capabilities the crowd does not test for (deep domain expertise)
