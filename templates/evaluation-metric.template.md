---
id: evaluation-metric/<slug>
type: evaluation-metric
slug: <slug>
schema_version: 3.0
title: "<Metric Name>"
name: "<Metric Name>"
category: retrieval          # retrieval | generation | agent | classification | ranking | safety
measures: "<One sentence: what a high score means.>"
needs: [<input>, <input>]    # what must be supplied, e.g. question, retrieved contexts, answer, reference answer
judged_by: code              # code | model | human: what decides the score in the defining source
range: { min: 0, max: 1 }    # optional; only as the defining source states it; null for an open bound
direction: higher-is-better  # higher-is-better | lower-is-better
defined_in: { title: "<defining paper or documentation page>", url: "<url>" }
implemented_by: [framework/<slug>]            # optional; framework records whose docs implement it as defined
related_metrics: [evaluation-metric/<slug>]   # optional
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<url>", title: "<title>", trust: 0.9 }
---

## What It Measures
<Two or three lines: the property of a system's output the metric scores.>

## How It Is Computed
<The definition from the source, as bullets or a plain-text formula. No invented variants.>

## Limits
<Only limits the defining source itself or a cited paper states.>
