---
id: leaderboard/<slug>
type: leaderboard
slug: <slug>
schema_version: 4.0              # the schema's current version; the schema refuses any other
title: "<Leaderboard Name>"
name: "<Leaderboard Name>"
maintainer: "<who runs it>"
url: "<address of the leaderboard>"
tagline: "<one line: what the board is for>"      # optional
ranks: "<what it ranks, in a few words>"           # optional
benchmarks: [benchmark/<slug>]                     # optional: benchmark records the board reports
update_cadence: Irregular            # Irregular | Retired | Not stated
coverage: "<models or votes covered, as the source states it>"   # optional
free_access: true                                  # optional
score_origin: independent                          # optional: self-reported | independent | mixed
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<url>", title: "<title>", trust: 0.9 }
related: [benchmark/<slug>]
---

## What It Ranks
<What the board ranks, who runs it, and how a ranking is produced.>

## How To Read It
<The methodology, what the numbers mean, and which decisions the board suits.>

## Caveats
<Where the board misleads: bias, coverage gaps, self-reported values, vendor interest.>
