---
id: benchmark/<slug>
type: benchmark
slug: <slug>
schema_version: 1.0
title: "<Benchmark Name>"
name: <BENCHMARK NAME>
category: Knowledge & Academic Reasoning   # see schema enum for the eight categories
description: "<one line: what the benchmark is>"
what_is_it: "<plain-English explanation of what it measures>"
why_people_use_it: "<why it matters / what decision it informs>"
examples:
  - "<a representative question or prompt>"
method: "<evaluation methodology: few-shot/0-shot, judge, harness>"
metric: "<scoring metric, e.g. accuracy, pass@1, F1, ELO>"
format: "<prompt format, e.g. 5-shot multiple-choice>"
saturation: medium            # high | medium | low
limitations: "<honest limitations, contamination, saturation>"
citation: "<canonical paper/site URL>"
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<url>", title: "<title>", trust: 0.9 }
related: [benchmark/<slug>]
---

## What It Tests
<One or two sentences: the capability under test.>

## How It Scores
<The metric, the baseline, and how to read a number.>

## Format
<How the model is prompted; few-shot count; answer extraction.>

## Limitations
<Contamination, saturation, narrow scope, gaming — be honest.>

## Example
<A concrete, worked example item with the expected answer.>
