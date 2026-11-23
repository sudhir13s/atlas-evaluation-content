---
id: score-sheet/<model-slug>
type: score-sheet
slug: <model-slug>
schema_version: 2.0
title: "<Model Name> — scores"
model: model/<model-slug>
scores:
  - benchmark: benchmark/<benchmark-slug>
    value: <number>
    scale: percent
    source: "<url of the report that publishes the value>"
    observed_on: <YYYY-MM-DD>
    reported_by: maker   # maker | independent | leaderboard
    self_reported: true  # older readers; omit when reported_by says it
    method: "<how it was measured, as the source states it>"
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<url>", title: "<title>", trust: 0.9 }
---

## Notes
<Optional. Which report the numbers came from, which benchmarks the vendor did NOT
publish, and any measurement caveats. Omit the heading when there is nothing to say.>
