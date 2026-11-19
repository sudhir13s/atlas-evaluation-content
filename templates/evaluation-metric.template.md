---
id: evaluation-metric/<slug>
type: evaluation-metric
slug: <slug>
schema_version: 2.1
title: "<Metric Name>"
name: "<Metric Name>"
category: retrieval          # retrieval | generation | agent | classification | ranking | safety
range: { min: 0, max: 1 }    # null for a bound the metric does not have
direction: higher-is-better  # higher-is-better | lower-is-better
needs_reference_answer: false
needs_judge_model: false
related_metrics: [evaluation-metric/<slug>]   # optional
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<url>", title: "<title>", trust: 0.9 }
---

## What It Measures
<The property of a system's output the metric scores.>

## How It Is Computed
<The formula or procedure, with a small worked example.>

## When To Use
<The situations it fits and the ones where another metric is the better choice.>

## Failure Modes
<How the metric misleads: gaming, judge bias, sensitivity to phrasing, saturation.>

## References
<The paper or documentation that defines it, as cited pointers.>
