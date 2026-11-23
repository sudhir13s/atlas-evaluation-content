---
id: evaluation-dataset/<slug>
type: evaluation-dataset
slug: <slug>
schema_version: 4.0              # the schema's current version; the schema refuses any other
title: "<Dataset or Harness Name>"
name: "<Dataset or Harness Name>"
category: Harness            # Harness | Framework | Dataset Suite | Human Preference | LLM-as-Judge | Live / Contamination-Free
size: "<human-readable scale, e.g. '200+ tasks'>"
url: "<canonical repo/site URL>"
description: "<one line: what it is>"
what_it_tests: "<capabilities it measures>"
why_it_exists: "<the problem it was built to solve>"
how_to_use: "<how a practitioner runs / consumes it>"
known_issues: "<limitations, biases, gotchas>"
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<url>", title: "<title>", trust: 0.9 }
related: [benchmark/<slug>]
---

## What It Is
<What the dataset/harness is and who maintains it.>

## How It Evaluates
<The mechanism: fixed test set, harness run, human votes, judge model.>

## What It Looks For
<The signal it isolates and how to interpret its output.>

## Known Issues
<Biases, contamination risk, coverage gaps, reproducibility caveats.>
