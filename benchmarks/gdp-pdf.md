---
id: benchmark/gdp-pdf
type: benchmark
slug: gdp-pdf
schema_version: 4.0
title: GDP.pdf
name: GDP.pdf
category: Multimodal
description: A Surge AI benchmark of 100 question-document pairs from ten professional fields, graded with rubrics of atomic criteria over long real PDFs.
what_is_it: 'GDP.pdf (Garre, Ritchie, Mehta and Chen; arXiv, July 2026) measures whether a model can answer a realistic question about a specific professional PDF, such as a benefits packet, lease, datasheet, clinical guideline or construction plan. It has 100 items written by working professionals in ten fields. A candidate question was kept only when at least two frontier multimodal models failed it in a way that mattered: a wrong answer, missed decisive evidence or a fabricated claim. Each item is tagged against a taxonomy of eleven capabilities in three tiers.'
why_people_use_it: Document-AI benchmarks usually test OCR (optical character recognition), layout, chart reasoning or table question answering in isolation. GDP.pdf tests the combination on realistic questions. Artificial Analysis includes it in its Intelligence Index, with a fixed five attempts per task.
method: Each item has a rubric of atomic criteria, and a judge model checks each criterion. Surge's open harness runs on Inspect AI; its leaderboard configuration sets the judge model with a task option. Artificial Analysis sends extracted text plus page images, uses a different judge that sees neither the PDF nor the model's identity, and says its scores are not comparable with Surge's.
metric: All-pass rate (strict) = items where every rubric criterion passes / items; mean criteria score (graded) = average share of criteria passed. Artificial Analysis reports All-pass as headline over 500 attempts (100 tasks times 5) and Mean Pass as secondary.
format: A question plus a long PDF; one free-form answer in a single turn. Artificial Analysis lists 4,592 PDF pages and 1,275 criteria across the 100 tasks.
limitations: Only items that stumped at least two frontier models were kept, so the set is hard by construction and tied to those models. Scores depend on how the PDF is delivered (raw document input versus extracted text plus page images) and which judge grades. The maker links a Hugging Face dataset, but it returned an authorisation error when checked on 2026-11-23, so item availability is unconfirmed. The benchmark dates from July 2026 and has a short track record.
citation: https://arxiv.org/abs/2607.11192
openness: partly-open
openness_note: The method and evaluation harness are public on GitHub (with only a synthetic sample task); the maker says the 100 items are on Hugging Face, but that dataset was not publicly reachable when checked on 2026-11-23.
tags:
- multimodal
- documents
- pdf
- professional
- surge-ai
updated: '2026-11-23'
sources:
- url: https://arxiv.org/abs/2607.11192
  title: 'GDP.pdf: Benchmarking Grounded Multimodal Reasoning over Professional PDF Documents (arXiv:2607.11192)'
  trust: 0.95
- url: https://github.com/surge-ai/gdp-pdf
  title: surge-ai/gdp-pdf — evaluation harness (Inspect AI)
  trust: 0.9
- url: https://huggingface.co/datasets/surgeai/GDP.pdf
  title: surgeai/GDP.pdf — dataset link given by the maker (not reachable on 2026-11-23)
  trust: 0.5
- url: https://artificialanalysis.ai/methodology/intelligence-benchmarking
  title: Artificial Analysis Intelligence Benchmarking Methodology (GDP.pdf)
  trust: 0.9
related:
- leaderboard/artificial-analysis
---

## What It Tests

Whether a model can find and use **decisive evidence in a long professional PDF**.

- **Eleven capabilities** in three tiers — among them text extraction and grounding, table and chart comprehension, cross-referencing, spatial reasoning and **abstention** (declining when the document does not support an answer).
- **Ten fields** — questions written by working professionals.
- **Hard by selection** — kept only if at least two frontier models failed it in a way that mattered.

## How It Scores

Each item has a **rubric of atomic criteria**: small yes/no checks a judge model applies one at a time.

- **Strict (all-pass)** — the item counts only if every criterion passes.
- **Graded (mean criteria)** — the share of criteria passed, averaged over items.
- **Artificial Analysis** — All-pass over 500 attempts as headline, Mean Pass as secondary.

## Format

One question, one long PDF, one free-form answer.

- **Size** — 100 items; Artificial Analysis counts **4,592 pages** and **1,275 criteria**.
- **Common failures in the paper** — misaligned tables, misread charts, skipped footnotes and exclusions, miscounted floor-plan symbols, scan noise, and amendments that supersede earlier text.
- **Harness** — `surge-ai/gdp-pdf` on GitHub runs the benchmark with Inspect AI and supports repeated attempts.

## Limitations

- **Selection effect.** Items were chosen because frontier models failed them.
- **Delivery matters.** Raw PDF versus extracted text with page images changes results.
- **Judge-specific.** Surge's and Artificial Analysis' scores are not comparable.
- **Data access unconfirmed.** The linked Hugging Face dataset returned an authorisation error on 2026-11-23.

## Example

No item is quoted, because the dataset could not be reached and the public repository holds only a synthetic placeholder task. As the paper describes it, an item pairs a **question a professional would ask** with a specific PDF, such as a lease or datasheet. Its rubric lists atomic checks, for example using the right figure, honouring a footnote exclusion or declining when the document does not support an answer. The answer passes strictly only if every criterion passes.
