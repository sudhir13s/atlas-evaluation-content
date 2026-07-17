---
id: evaluation-dataset/chatbot-arena
type: evaluation-dataset
slug: chatbot-arena
schema_version: 1.0
title: "Chatbot Arena (LMArena)"
name: "Chatbot Arena / LMArena"
category: Human Preference
size: "3M+ human pairwise votes across 150+ models (and growing)"
url: "https://lmarena.ai/"
description: "Crowdsourced blind pairwise voting that ranks models by real human preference using an ELO/Bradley-Terry rating."
what_it_tests: "Overall human-perceived response quality on open-ended, user-chosen prompts — the closest public proxy for 'which model do people actually prefer?'"
why_it_exists: "Fixed-answer benchmarks cover a narrow slice of real usage and can be gamed by training on the test set. Arena has no fixed test set — prompts come live from real users — so it measures preference in the wild and is very hard to game."
how_to_use: "Read the public leaderboard (overall and category boards: Hard Prompts, Coding, Math, Multi-turn, Long Context, Vision, Style Control). Or contribute votes by using the side-by-side battle interface; identities are revealed only after voting."
known_issues: "Preference is not correctness — a confident, well-formatted wrong answer can win. Voters skew toward AI-curious power users. Verbose, nicely formatted answers are favored (the Style Control board regresses this out). English-heavy prompt distribution."
tags: [human-preference, elo, crowdsourced, real-world]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2403.04132", title: "Chatbot Arena: An Open Platform for Evaluating LLMs by Human Preference (Chiang et al., 2024)", trust: 0.95 }
  - { url: "https://lmarena.ai/", title: "LMArena — leaderboard", trust: 0.9 }
related: [benchmark/ifeval]
---

## What It Is
An open platform (from LMSYS/LMArena, with UC Berkeley SkyLab) where users compare two anonymous
models side by side on their own prompts and vote for the better response. Millions of votes
aggregate into an ELO/Bradley-Terry ranking — the same math used to rate chess players.

## How It Evaluates
Blind pairwise voting: a user submits any prompt, sees responses A and B without model names, and
picks the winner (or a tie). Votes update the rating model. Separate category boards isolate coding,
math, multi-turn, long-context, and vision, and a Style Control board statistically removes length and
formatting bias.

## What It Looks For
Aggregate human preference on the long tail of real tasks. Because there is no fixed test set, models
cannot be trained on the questions, which makes the signal close to irreducible for general chat
quality.

## Known Issues
It measures preference, not truth — a persuasive wrong answer can win. The voter population is not
representative, and formatting/verbosity bias the raw scores (partly corrected by Style Control). The
prompt mix is English-heavy and cannot probe capabilities the crowd does not test.
