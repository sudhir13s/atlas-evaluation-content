---
id: benchmark/aa-lcr
type: benchmark
slug: aa-lcr
schema_version: 1.0
title: "AA-LCR"
name: AA-LCR
category: Long Context
description: "Artificial Analysis Long Context Reasoning — 100 hard questions requiring multi-step reasoning across sets of real-world documents totaling ~100k tokens each."
what_is_it: "Artificial Analysis Long Context Reasoning (AA-LCR) is a benchmark of 100 human-crafted questions, each answered by reasoning across a set of real documents (~100k input tokens per set). Answers are never stated in the text; the model must integrate scattered facts and infer the result — designed to replicate real knowledge work rather than synthetic needle-in-a-haystack retrieval."
why_people_use_it: "Simple retrieval benchmarks (needle-in-a-haystack) dramatically overstate real long-context ability. AA-LCR measures reasoning over many long documents at once, so it discriminates between frontier models on the Artificial Analysis leaderboard where retrieval tests are already saturated."
examples:
  - "Given four company annual reports and an industry report (~100k tokens), determine which segment drove the largest year-over-year margin change — a figure not printed anywhere but derivable by combining revenue and cost tables across documents."
  - "Across a set of government consultation responses, identify the single position endorsed by a majority of respondents once conflicting terminology is reconciled."
method: "The model receives a document set (~100k input tokens, cl100k_base tokenizer) and one question; the free-text answer is graded against a reference for correctness. 100 questions span 30 document sets (234 documents) across 7 document types."
metric: "Accuracy (% of questions answered correctly)."
format: "Long-context, open-ended question answering over multi-document inputs of ~100k tokens each."
saturation: low
limitations: "Only 100 questions, so scores carry real sampling noise and small differences are not meaningful. Being open-ended, grading depends on answer-matching and can penalize correct-but-differently-phrased responses. Text-only (no tables-as-images, no code repos), and the ~100k-token target does not probe the 500k-2M-token regime some models now advertise."
citation: "https://artificialanalysis.ai/articles/announcing-aa-lcr"
example:
  task_type: "Multi-document long-context reasoning (~100k-token input)"
  input: "Given four company annual reports and one industry report (~100k tokens total), answer: which business segment drove the largest year-over-year operating-margin change? The figure is printed in no single table."
  success:
    answer: "Cloud Services — its margin fell from 31% to 22% YoY, the largest swing, computed by pairing the revenue and cost-of-revenue lines from the segment tables in Report B (p.44) and Report C (p.51)."
    why: "The answer is derived by locating revenue and cost figures scattered across two distant documents and computing the margins — no single passage states it."
  failure:
    answer: "Hardware — its revenue grew the most year over year, so it drove the largest margin change."
    why: "Incorrect answer from a nearby, easy-to-retrieve passage: the model grabbed a headline revenue-growth number from one document instead of combining revenue and cost across documents to compute the actual margin change."
  checks: "Whether a model integrates facts spread across several long documents through multi-step inference, versus retrieving one salient nearby number."
  scoring: "Accuracy — free-text answer graded against a reference answer for correctness."
tags: [long-context, reasoning, multi-document, open-ended]
updated: 2026-07-20
sources:
  - { url: "https://artificialanalysis.ai/articles/announcing-aa-lcr", title: "Announcing Artificial Analysis Long Context Reasoning (AA-LCR)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/ArtificialAnalysis/AA-LCR", title: "ArtificialAnalysis/AA-LCR dataset (Hugging Face)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/evaluations/artificial-analysis-long-context-reasoning", title: "AA-LCR Leaderboard (Artificial Analysis)", trust: 0.9 }
related: [benchmark/longbench-v2]
---

## What It Tests
Reasoning across several long documents at once. Each question is tied to a document set that
averages ~100k input tokens (measured with the cl100k_base tokenizer) and spans seven real-world
document types: company reports, industry reports, government consultations, academia, legal,
marketing materials, and survey reports. Crucially, the answer is never written down — the model
must locate relevant facts in different places and combine them through multi-step inference.

## How It Scores
Accuracy over 100 questions (drawn from 30 document sets, 234 documents total). Each free-text
answer is checked against a reference answer for correctness. Because the set is small, headline
numbers move with only a few questions, so it is read as a coarse ranking rather than a precise
percentage. On release, top systems clustered near 69% (OpenAI o3 at 69.3%, xAI Grok 4 at 68%,
Qwen3 235B at 67%), with weaker models far below (LG EXAONE 4.0 at 14%).

## Format
Long-context open-ended question answering: the full document set plus one question go into the
prompt, and the model returns a written answer. This makes output length vary enormously across
models — from tens of thousands to millions of output tokens depending on how much the model
reasons before answering.

## Limitations
The 100-question size means real sampling noise; small gaps between models are not significant.
Open-ended grading can mis-score answers that are correct but phrased unexpectedly. It is text-only
and targets the ~100k-token band, so it does not probe multimodal long context or the multi-hundred-
thousand-token regime. Human evaluators themselves answered only 40-60% correctly on a first
attempt, which sets a low absolute ceiling for interpreting scores.

## Example
A question over a set of company and industry reports (~100k tokens) asks for a quantity — say, the
business unit with the largest year-over-year margin swing — that appears in no single table. The
model must pull revenue and cost figures from separate documents, compute the margins itself, and
compare them. A model that merely retrieves the nearest-looking number without doing the arithmetic
across documents fails, which is exactly the gap AA-LCR is built to expose.
