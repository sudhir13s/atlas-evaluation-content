---
id: evaluation-framework/<slug>
type: evaluation-framework
slug: <slug>
schema_version: 2.1
title: "<Framework Name>"
name: "<Framework Name>"
maintainer: "<who maintains it>"
repository_url: "<address of the source repository>"
docs_url: "<address of the documentation>"        # optional
license: "<SPDX identifier, e.g. Apache-2.0>"
scope: application                                 # application | agent | model
metrics: [evaluation-metric/<slug>]                # optional: metrics the framework implements
languages: [Python]
tags: [<tag>]
updated: <YYYY-MM-DD>
sources:
  - { url: "<url>", title: "<title>", trust: 0.9 }
related: [evaluation-framework/<slug>]
---

## What It Is
<What the framework evaluates and who maintains it.>

## When To Use
<The situations it fits and the ones where another tool is the better choice.>

## How It Works
<The mechanism: datasets, metrics, judge models, tracing, how a run is configured.>

## Limitations
<Costs, judge bias, coverage gaps, lock-in, maturity.>

## References
<Documentation and papers it is built on, as cited pointers.>
