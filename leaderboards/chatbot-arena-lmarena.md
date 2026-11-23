---
id: leaderboard/chatbot-arena-lmarena
type: leaderboard
slug: chatbot-arena-lmarena
schema_version: 4.0
title: Arena (formerly LMArena)
name: Arena (formerly LMArena)
maintainer: Arena Intelligence Inc.
url: https://arena.ai/leaderboard
tagline: Rankings from anonymous head-to-head votes by people using the site.
ranks: models by pairwise human votes, plus agent task signals
update_cadence: Irregular
coverage: 382M+ sessions
free_access: true
score_origin: independent
tags:
- leaderboard
updated: '2026-11-21'
sources:
- url: https://arena.ai/leaderboard
  title: Arena leaderboard
  trust: 0.9
- url: https://arena.ai/leaderboard/methodology
  title: Arena leaderboard methodology
  trust: 0.9
- url: https://arena.ai/company/about
  title: About Arena
  trust: 0.9
- url: https://arena.ai/company/leaderboard-changelog
  title: Arena leaderboard changelog
  trust: 0.9
- url: https://arxiv.org/abs/2403.04132
  title: 'Chatbot Arena: An Open Platform for Evaluating LLMs by Human Preference'
  trust: 0.9
---

## What It Ranks

Arena ranks models from votes cast by people using the site. Arena Intelligence Inc. runs it; the about page says Arena was created by researchers from UC Berkeley and was formerly LMArena, previously Chatbot Arena.

- **Battle leaderboards**: two anonymous models answer the same task and the user votes for the better response (A, B, tie or both bad); identities appear after the vote.
- **Agent leaderboards**: each session gets a randomly chosen orchestrator model, scored on a weighted average of signals such as task completion, praise versus complaint, steerability and bash reliability.
- **Boards**: Overall, Agent, Alignment Index, Text, WebDev, Image-to-WebDev, Text to Image, Image Edit, Text to Video, Image to Video, Video Edit, Vision, Document and Search.

## How To Read It

### Methodology

- Battle scores come from Bradley-Terry regression, which the methodology page calls closely related to the Elo model used in chess
- Style control adds response length and formatting to the statistical model, so ratings aim to reflect quality rather than style
- Confidence intervals narrow as more votes arrive
- The changelog records leaderboard updates several times a week, with no stated schedule

### Best For

- Comparing models by anonymous pairwise votes from the site's users

## Caveats

- The leaderboard page shows a total of sessions, not of votes
- The methodology page does not state how often the leaderboard updates
- The changelog says backfilled scores will appear in its public Hugging Face datasets; the leaderboard page states no data licence
