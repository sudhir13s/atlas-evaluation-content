---
id: leaderboard-entry/<slug>
type: leaderboard-entry
slug: <slug>
schema_version: 1.0
title: "<Model Name> — leaderboard row"
model_slug: <model-slug>
model_name: "<Model Name>"
organization: "<Vendor / Lab>"
params_b: <number>
license: "<license name or SPDX id>"
self_reported: true
mmlu: <number>
mmlu_caveat: "<how measured / who reported>"
humaneval: <number>
humaneval_caveat: "<how measured / who reported>"
gsm8k: <number>
gsm8k_caveat: "<how measured / who reported>"
truthfulqa: <number>
arc_challenge: <number>
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<url>", title: "<title>", trust: 0.9 }
---

## Notes
<Which report the numbers came from, which benchmarks the vendor did NOT publish
(and where the fallback figure was sourced), and any measurement caveats.>
