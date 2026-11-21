---
id: benchmark/hle-diamond
type: benchmark
slug: hle-diamond
schema_version: 1.0
title: "HLE-Diamond"
name: HLE-Diamond
category: Frontier / Hardest
description: "A refined 1,000-question subset of Humanity's Last Exam, split evenly into reasoning and knowledge questions, from the Center for AI Safety and Scale AI."
what_is_it: "HLE-Diamond is a cleaned and refined subset of Humanity's Last Exam (HLE), released after a year-long process with input from research communities. It has 1,000 questions: 500 reasoning and 500 knowledge, marked by a partition column. Artificial Analysis reports that 272 of the questions include an image. The authors recommend settings for runs with tools such as web search and code, in addition to closed-book runs."
why_people_use_it: "It is a smaller, cleaner version of HLE that is cheaper to run and separates reasoning from stored knowledge. Artificial Analysis includes it in its Intelligence Index, and Google lists it among its reasoning benchmarks. The reasoning and knowledge partitions are reported separately."
method: "Models answer without tools by default; the maker also gives recommended settings for runs with web and code tools. Artificial Analysis uses the official HLE system prompt, which asks for an explanation, an answer and a confidence score, and the official judge prompt with an LLM judge."
metric: "Accuracy in percent on all 1,000 questions, plus accuracy on each partition (reasoning, knowledge), as pass@1."
format: "0-shot, open answers, 1,000 questions; 272 include an image. Models without image input still face all 1,000 in Artificial Analysis' run."
limitations: "The dataset on Hugging Face is gated and the maintainers ask people not to re-share it; it carries a canary string. Scores are not comparable with scores on the full HLE, which uses 2,500 questions (2,158 text-only in Artificial Analysis' run), nor between closed-book and with-tools runs. Image handling and judge model change the result, so cite both."
citation: "https://lastexam.ai/blog/hle-diamond"
tags: [frontier, expert-questions, multimodal, reasoning, knowledge]
updated: 2026-11-21
sources:
  - { url: "https://lastexam.ai/blog/hle-diamond", title: "Introducing HLE-Diamond (Humanity's Last Exam)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/cais/hle-diamond", title: "cais/hle-diamond — Hugging Face dataset card (gated)", trust: 0.9 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (HLE-Diamond)", trust: 0.85 }
related: [benchmark/hle, leaderboard/artificial-analysis]
---

## What It Tests
The same **expert-level closed questions** as HLE, in a cleaned subset that splits skills.

- **Reasoning partition** — 500 questions that need multi-step reasoning.
- **Knowledge partition** — 500 questions that test expert knowledge.
- **Images** — 272 questions include an image (per Artificial Analysis).

## How It Scores
**Accuracy**, overall and per partition, judged by an LLM against the key.

- **Default** — no tools, closed book.
- **Optional** — the maker recommends settings for web and code tools.
- **Judge** — Artificial Analysis uses the official judge prompt and response format.

## Format
1,000 zero-shot open-answer questions, answered with an explanation, answer and confidence under the official system prompt.

## Limitations
- **Gated data.** Do not re-upload the dataset; it contains a canary string.
- **Not comparable with HLE.** Different question set, and tool use changes the number.

## Example
The questions are gated by the maker. Each question is marked **reasoning** or **knowledge**, optionally has an image, and the model returns an explanation, an answer and a confidence. The judge checks the answer, and the report gives the overall rate and the rate for each partition.
