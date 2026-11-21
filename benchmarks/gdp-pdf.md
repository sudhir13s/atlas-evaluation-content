---
id: benchmark/gdp-pdf
type: benchmark
slug: gdp-pdf
schema_version: 1.0
title: "GDP.pdf"
name: GDP.pdf
category: Multimodal
description: "A Surge AI benchmark of 100 question-document pairs from ten professional fields, graded with rubrics of atomic criteria over long real PDFs."
what_is_it: "GDP.pdf measures whether a model can answer a realistic question about a specific professional PDF, such as a benefits packet, lease, datasheet, clinical guideline or construction plan. It has 100 items written by working professionals in ten fields. A candidate question was kept only when at least two frontier multimodal models failed it in a way that mattered: a wrong answer, missed decisive evidence or a fabricated claim. Each item is tagged against a taxonomy of eleven capabilities in three tiers."
why_people_use_it: "Document-AI benchmarks usually test OCR, layout, chart reasoning or table question answering in isolation. GDP.pdf tests the combination on realistic questions. Artificial Analysis includes it in its Intelligence Index, with a fixed five attempts per task."
method: "Each item has a rubric of atomic criteria, so results can be a graded rubric score or a strict task-level pass rate. Surge uses one judge model; Artificial Analysis sends page images plus extracted text with a different judge, and says its scores are not comparable with Surge's."
metric: "Strict task-level pass rate (all criteria pass) and a graded rubric score in percent. Artificial Analysis reports All-pass as headline over 500 attempts (100 tasks times 5) and Mean Pass as secondary."
format: "A question plus a long PDF; one free-form answer in a single turn without tools in Artificial Analysis' run. Artificial Analysis lists 4,592 PDF pages and 1,275 criteria."
limitations: "Only items that stumped at least two frontier models were kept, so the set is hard by construction and tied to those models. Scores depend on how the PDF is delivered (raw document input versus extracted text plus page images) and which judge grades. The paper is from July 2026 and the benchmark has a short track record."
citation: "https://arxiv.org/abs/2607.11192"
tags: [multimodal, documents, pdf, professional, surge-ai]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2607.11192", title: "GDP.pdf: Benchmarking Grounded Multimodal Reasoning over Professional PDF Documents (arXiv:2607.11192)", trust: 0.95 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (GDP.pdf)", trust: 0.9 }
related: [leaderboard/artificial-analysis]
---

## What It Tests
Whether a model can find and use **decisive evidence in a long professional PDF**.

- **Eleven capabilities** — from text extraction and grounding to table and chart comprehension, cross-referencing, spatial reasoning and abstention on unsupported queries.
- **Ten fields** — questions written by working professionals.
- **Hard by selection** — kept only if two frontier models failed it meaningfully.

## How It Scores
Each item has a **rubric of atomic criteria**.

- **Strict** — task-level pass rate, all criteria pass.
- **Graded** — rubric score across criteria.
- **Artificial Analysis** — All-pass as headline, Mean Pass as secondary.

## Format
One question, one long PDF, one free-form answer. The paper finds recurring loss patterns such as misaligned tables, misread charts and skipped footnotes.

## Limitations
- **Selection effect.** Items were chosen because frontier models failed them.
- **Delivery matters.** Raw PDF versus extracted text with page images changes results.
- **Judge-specific.** Surge's and Artificial Analysis' scores are not comparable.

## Example
A typical item pairs a **question a professional would ask** with a specific PDF, for example a lease or datasheet. Its rubric lists atomic checks such as using the right figure, honouring a footnote exclusion or declining when the document does not support an answer. An answer passes strictly only if every criterion passes.
