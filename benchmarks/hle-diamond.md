---
id: benchmark/hle-diamond
type: benchmark
slug: hle-diamond
schema_version: 3.1
title: "HLE-Diamond"
name: HLE-Diamond
category: Frontier / Hardest
description: "A refined 1,000-question subset of Humanity's Last Exam, split evenly into reasoning and knowledge questions, from the Center for AI Safety and Scale AI."
what_is_it: "HLE-Diamond is a cleaned and refined subset of Humanity's Last Exam (HLE), released on 22 September 2026 after a year-long process of cleaning and refinement with input from research communities. It has 1,000 questions: 500 reasoning and 500 knowledge, marked by a partition column. Artificial Analysis reports that 272 of the questions include an image. The maker positions it as a migration path for when frontier models reach the noise ceiling on the original HLE, and publishes recommended settings for runs with tools in addition to closed-book runs."
why_people_use_it: "It is a smaller, cleaner version of HLE that separates multi-step reasoning from stored expert knowledge, reported as two partitions. Artificial Analysis includes it in its Intelligence Index with a 10% weight."
method: "Models answer without tools by default; the maker also gives recommended settings for runs with tools in the HLE GitHub repository. Artificial Analysis uses the official HLE system prompt, which asks for an explanation, an answer and a confidence score, and the official judge prompt and response format with an LLM judge (GPT-5.6 Luna, medium)."
metric: "Accuracy in percent (correct answers / questions) on all 1,000 questions, plus accuracy on each partition (reasoning, knowledge), as pass@1 (one attempt per question)."
format: "0-shot, open answers, 1,000 questions; 272 include an image. Models without image input receive only the question text for those 272 in Artificial Analysis' run."
limitations: "The dataset on Hugging Face is gated and the maintainers ask people not to re-share it; it carries a canary string. Scores are not comparable with scores on the full HLE, which uses 2,500 questions (2,158 text-only in Artificial Analysis' run), nor between closed-book and with-tools runs. Image handling and judge model change the result, so cite both."
citation: "https://lastexam.ai/blog/hle-diamond"
openness: open
openness_note: "All 1,000 questions with answers are downloadable from the gated Hugging Face dataset under the MIT licence after accepting its conditions, and the maintainers ask that it not be re-shared."
tags: [frontier, expert-questions, multimodal, reasoning, knowledge]
updated: 2026-11-23
sources:
  - { url: "https://lastexam.ai/blog/hle-diamond", title: "Introducing HLE-Diamond (Humanity's Last Exam)", trust: 0.95 }
  - { url: "https://lastexam.ai", title: "Humanity's Last Exam — project site (HLE-Diamond release note)", trust: 0.9 }
  - { url: "https://huggingface.co/datasets/cais/hle-diamond", title: "cais/hle-diamond — Hugging Face dataset card (gated, MIT)", trust: 0.9 }
  - { url: "https://artificialanalysis.ai/methodology/intelligence-benchmarking", title: "Artificial Analysis Intelligence Benchmarking Methodology (HLE-Diamond)", trust: 0.85 }
related: [benchmark/hle, leaderboard/artificial-analysis]
---

## What It Tests
The same **expert-level closed questions** as HLE, in a cleaned subset that splits skills.

- **Reasoning partition** — 500 questions that measure reasoning.
- **Knowledge partition** — 500 questions that measure expert knowledge.
- **Images** — 272 questions include an image (per Artificial Analysis).
- **Makers** — the HLE team at the Center for AI Safety and Scale AI, released 22 September 2026.

## How It Scores
**Accuracy**, overall and per partition, judged by an LLM against the key.

- **Default** — no tools, closed book.
- **Optional** — the maker publishes recommended settings for runs with tools.
- **Judge** — Artificial Analysis uses the official judge prompt and response format, one attempt per question (pass@1).

## Format
1,000 zero-shot open-answer questions, answered with an explanation, answer and confidence under the official system prompt.

- **Data** — gated Hugging Face dataset `cais/hle-diamond` (**MIT licence**); a `partition` column marks reasoning or knowledge, and other columns follow the HLE format.

## Limitations
- **Gated data.** Do not re-upload the dataset; it contains a canary string.
- **Not comparable with HLE.** Different question set, and tool use changes the number.
- **Image handling.** Text-only models see only the text of the 272 image questions.

## Example
No question is quoted here: the dataset is gated and the maintainers ask that it not be shared publicly. As the maker describes the format:

- **Each item** — an HLE-format question, optionally with an image, marked **reasoning** or **knowledge**.
- **Answer** — the model returns an explanation, an answer and a confidence.
- **Report** — the judge checks the answer; results give the overall rate and the rate for each partition.
