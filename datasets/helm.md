---
id: evaluation-dataset/helm
type: evaluation-dataset
slug: helm
schema_version: 1.0
title: "HELM"
name: "HELM (Holistic Evaluation of Language Models)"
category: Framework
size: "Dozens of scenarios x multiple metrics (accuracy, calibration, robustness, fairness, bias, toxicity, efficiency)"
url: "https://crfm.stanford.edu/helm/"
description: "Stanford CRFM's multi-metric framework that evaluates models across many scenarios and axes, not just accuracy."
what_it_tests: "A matrix of scenarios (question answering, summarization, sentiment, toxicity, reasoning, and more) each measured on several metrics — accuracy, calibration, robustness, fairness, bias, toxicity, and efficiency."
why_it_exists: "Single-accuracy leaderboards hide critical properties. HELM's thesis is that evaluation should be holistic and transparent — reporting many metrics on many scenarios so trade-offs (e.g. accuracy vs bias vs cost) are visible."
how_to_use: "Run the open-source framework locally, or read the hosted leaderboards (Classic, Lite, MMLU, and specialized editions like HELM Safety and MedHELM). Results are published with full per-scenario, per-metric breakdowns and prompt transparency."
known_issues: "Comprehensiveness makes it expensive to run in full and harder to summarize into one ranking. Scenario and metric selection is a curatorial choice. The hosted results can lag the newest models."
tags: [framework, holistic, transparency, safety, fairness]
updated: 2026-07-17
sources:
  - { url: "https://arxiv.org/abs/2211.09110", title: "Holistic Evaluation of Language Models (Liang et al., 2022)", trust: 0.95 }
  - { url: "https://crfm.stanford.edu/helm/", title: "HELM — Stanford CRFM", trust: 0.9 }
related: [benchmark/mmlu, benchmark/truthfulqa]
---

## What It Is
A framework and set of hosted leaderboards from the Stanford Center for Research on Foundation Models
that evaluates models holistically — many scenarios crossed with many metrics — with an emphasis on
transparency (every prompt and prediction is inspectable).

## How It Evaluates
It defines scenarios (a dataset + task) and measures each model on a standard battery of metrics.
Crucially, it reports accuracy alongside calibration, robustness (to perturbations), fairness, bias,
toxicity, and efficiency (compute/latency), producing a metric×scenario matrix rather than a single
score.

## What It Looks For
The full profile of a model, including where it is weak on non-accuracy axes. It surfaces trade-offs
that single-number leaderboards erase — e.g. a model that is accurate but poorly calibrated or more
toxic.

## Known Issues
Running the complete suite is compute-intensive, and its breadth resists compression into one ranking
(which is partly the point). The choice of scenarios and metrics is curatorial, and hosted results
can trail the latest releases.
