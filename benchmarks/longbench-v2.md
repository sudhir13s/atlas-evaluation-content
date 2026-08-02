---
id: benchmark/longbench-v2
type: benchmark
slug: longbench-v2
schema_version: 1.0
title: "LongBench v2"
name: LongBench v2
category: Long Context
description: "503 hard multiple-choice questions over contexts from 8k to 2M words, testing deep understanding and reasoning across six real-world long-context task types."
what_is_it: "LongBench v2 is a long-context benchmark of 503 challenging multiple-choice questions written by ~100 highly educated annotators. Each question comes with a long context (8k to 2M words) and four answer options, and is designed so that skimming or retrieval is not enough — the answer requires genuine understanding and reasoning over the whole document."
why_people_use_it: "The original LongBench and simple retrieval tests saturated. LongBench v2 restores difficulty — human experts score only 53.7% under a 15-minute limit — so it separates models that truly reason over long inputs from those that merely locate a passage. It is a standard reference for long-context reasoning."
examples:
  - "Given a full novel as context, a single-document QA item asks which character's motivation best explains an event late in the book — answerable only by tracking information across chapters (one correct option of four)."
  - "Given an entire code repository, a code-repository-understanding item asks what a change to one function will break elsewhere (one correct option of four)."
method: "Zero-shot multiple-choice: the long context plus a four-option question go in the prompt; the chosen letter is parsed and compared to the gold answer. 503 questions span six task categories. Chain-of-thought prompting is commonly reported alongside direct answering."
metric: "Accuracy (% of questions answered correctly); random baseline is 25% for the 4-way choice."
format: "Zero-shot, 4-way multiple-choice over long contexts (8k-2M words)."
saturation: medium
limitations: "Multiple-choice with four options means a 25% guessing floor and sensitivity to option ordering and answer parsing. The very long contexts make evaluation expensive and force truncation on models with smaller windows, confounding comparisons. Being reasoning-over-choices, it under-tests long-form generation and faithful citation."
citation: "https://arxiv.org/abs/2412.15204"
example:
  task_type: "Long-context single-document QA (4-way multiple choice over a full novel)"
  input: "Context: the entire text of a novel (tens of thousands of words). Question: Which explanation best accounts for the protagonist's decision to abandon the inheritance in the final chapter? (A) … (B) … (C) … (D) …"
  success:
    answer: "(C) — a promise made to her dying mother in Chapter 2, reinforced by the guilt she voices in Chapter 19, which together explain the final-chapter choice."
    why: "The correct option requires integrating a setup introduced early with its payoff far later — reasoning across chapters, not locating one passage."
  failure:
    answer: "(B) — she abandons the inheritance because of the argument with her brother described just before the final chapter."
    why: "Incorrect answer from a locally plausible nearby passage: the distractor paraphrases the most recent scene, but the model missed that the true cause was set up chapters earlier and never combined the two distant passages."
  checks: "Whether a model reasons over information separated by long spans of context rather than picking the answer nearest the question."
  scoring: "Accuracy over 4-option questions (25% random baseline); the parsed letter is compared to the gold answer."
tags: [long-context, reasoning, multiple-choice]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2412.15204", title: "LongBench v2: Towards Deeper Understanding and Reasoning on Realistic Long-context Multitasks (Bai et al., 2024)", trust: 0.95 }
  - { url: "https://aclanthology.org/2025.acl-long.183/", title: "LongBench v2 (ACL 2025)", trust: 0.95 }
related: [benchmark/aa-lcr]
---

## What It Tests
Deep understanding and reasoning over realistic long documents — not retrieval. The 503 questions
span six task categories: single-document question answering (QA), multi-document QA, long
in-context learning, long-dialogue history understanding, code-repository understanding, and long
structured-data understanding. Contexts range from 8,000 to 2 million words, so a model must hold
and reason over far more than a single passage.

## How It Scores
Accuracy across the 503 four-option questions, with a 25% random baseline. The set was built to be
hard: ~100 highly educated annotators wrote and vetted the questions, and human experts reached only
53.7% under a 15-minute time limit. At release, the best model answering directly hit 50.1%, while a
reasoning model (o1-preview) reached 57.7% — the first to clear the human baseline, and evidence
that explicit reasoning, not just a bigger context window, drives the score.

## Format
Zero-shot multiple choice: the long context and a four-option question are placed in the prompt and
the model's chosen letter is parsed. Both direct-answer and chain-of-thought protocols are reported;
chain-of-thought generally helps, so cross-report comparisons must match the protocol.

## Limitations
Four options impose a 25% guessing floor and make scores sensitive to option ordering and answer
parsing. The extreme context lengths are costly to run and exceed some models' windows, forcing
truncation that muddies comparison. As a multiple-choice reasoning test it says nothing about
long-form generation quality or whether a model can cite the right span.

## Example
A single-document QA item supplies an entire novel and asks which of four explanations best accounts
for a late plot event. The correct option depends on details introduced chapters apart; a distractor
paraphrases a locally plausible but globally wrong reading. A model that retrieves only the nearest
relevant paragraph — without integrating the earlier setup — picks the distractor, which is the
failure mode LongBench v2 is designed to surface.
