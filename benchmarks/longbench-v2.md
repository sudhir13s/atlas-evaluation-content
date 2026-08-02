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
worked_examples:
  - task_type: "Single-document QA over a full novel"
    input: "Context: the entire text of a novel (tens of thousands of words). Question: Which explanation best accounts for the protagonist's decision to abandon the inheritance in the final chapter?"
    options:
      - "She never wanted the money and says so in the opening scene."
      - "She abandons it because of the argument with her brother described just before the final chapter."
      - "A promise made to her dying mother in Chapter 2, reinforced by the guilt she voices in Chapter 19."
      - "The estate's lawyer tells her the inheritance is worthless."
    success:
      answer: "C — a promise made to her dying mother in Chapter 2, reinforced by the guilt she voices in Chapter 19, which together explain the final-chapter choice."
      why: "The correct option requires integrating a setup introduced early with its payoff far later — reasoning across chapters, not locating one passage."
    failure:
      answer: "B — she abandons the inheritance because of the argument with her brother just before the final chapter."
      why: "Incorrect answer from a locally plausible nearby passage: the distractor paraphrases the most recent scene, but the model missed that the true cause was set up chapters earlier and never combined the two distant passages."
    checks: "Whether a model reasons over information separated by long spans of context rather than picking the answer nearest the question."
    scoring: "Accuracy over 4-option questions (25% random baseline); the parsed letter is compared to the gold answer."
  - task_type: "Code-repository understanding"
    input: "Context: an entire Python repository (many files, tens of thousands of tokens). Question: If the signature of the internal helper `_normalize_path()` in utils/io.py is changed to return a list instead of a string, which part of the codebase breaks first at runtime?"
    options:
      - "The unit tests in tests/test_utils.py, which only check `_normalize_path` in isolation."
      - "The `load_config()` function in config/loader.py, which concatenates the return value with a string suffix."
      - "The CLI entrypoint in main.py, which never imports the helper."
      - "Nothing breaks; the change is backward-compatible."
    success:
      answer: "B — `load_config()` in config/loader.py concatenates the helper's return value with a string, which raises a TypeError once a list is returned."
      why: "Correct only by tracing the helper's call sites across files and finding the one that assumes a string return — a cross-file dependency, not a local read."
    failure:
      answer: "A — the unit tests in tests/test_utils.py break because they assert on the old string return."
      why: "Locally plausible but wrong: the tests are the nearest reference to the helper, but the question asks what breaks at runtime; the model missed the distant string-concatenation call site in config/loader.py."
    checks: "Whether a model follows a symbol's usage across many files rather than reasoning only about the file it is defined in."
    scoring: "Accuracy over 4-option questions (25% random baseline); the parsed letter is compared to the gold answer."
  - task_type: "Multi-document QA"
    input: "Context: five news articles from different dates covering the same corporate merger (tens of thousands of tokens). Question: What was the final approved acquisition price, and how did it change from the first offer?"
    options:
      - "$4.2 billion, unchanged from the initial offer reported in the first article."
      - "$3.8 billion, the figure quoted in the most recent article's headline."
      - "$4.5 billion, raised from an initial $4.2 billion after regulators required a sweetened bid, per the second and fifth articles."
      - "$4.2 billion, the only price mentioned anywhere in the coverage."
    success:
      answer: "C — $4.5 billion, raised from the initial $4.2 billion offer, after the regulatory demand described in the second article and the revised figure in the fifth."
      why: "Requires combining the original offer from one article with the revised, approved figure from another dated article — the change is stated in no single document."
    failure:
      answer: "B — $3.8 billion, the figure in the most recent article's headline."
      why: "Wrong nearby passage: the model grabbed a prominent headline number (which referred to a rejected counter-bid) instead of reconciling the offer and final price across the two relevant articles."
    checks: "Whether a model reconciles evolving facts across separately-dated documents rather than trusting one salient figure."
    scoring: "Accuracy over 4-option questions (25% random baseline); the parsed letter is compared to the gold answer."
  - task_type: "Long-dialogue history understanding"
    input: "Context: a very long customer-support chat log spanning weeks (tens of thousands of tokens). Question: What is the current status of the customer's refund request as of the final message?"
    options:
      - "Pending — the agent said early on it would be 'reviewed within 48 hours'."
      - "Approved and paid — an early message promised the refund would be processed."
      - "Denied, then reopened — it was rejected mid-thread for a missing receipt, but the customer later supplied it and an agent reinstated the request near the end."
      - "Never requested — the log only discusses a product exchange."
    success:
      answer: "C — denied for a missing receipt partway through, then reopened after the customer supplied the receipt and a later agent reinstated it."
      why: "The current status only emerges by following the status through several distant turns; an early message alone gives a stale answer."
    failure:
      answer: "A — pending, since an agent said it would be reviewed within 48 hours."
      why: "Anchored on an early passage: the model reported the initial status and missed the later denial-and-reopening turns that changed it, failing to integrate the distant updates."
    checks: "Whether a model tracks a state that evolves across a long dialogue rather than reporting an early, now-stale value."
    scoring: "Accuracy over 4-option questions (25% random baseline); the parsed letter is compared to the gold answer."
  - task_type: "Long structured-data understanding"
    input: "Context: a large spreadsheet-style table of quarterly sales by region and product line, rendered as text (tens of thousands of tokens). Question: Which region had positive sales growth in every quarter of the year?"
    options:
      - "North — it had the single highest quarter in the table."
      - "West — its sales rose in Q1, Q2, and Q3 but fell in Q4."
      - "South — its quarter-over-quarter figures increase in all four quarters, the only region with no decline."
      - "East — it had the highest annual total."
    success:
      answer: "C — South, whose quarter-over-quarter values rise in all four quarters, the only region never showing a decline."
      why: "Requires scanning every region's full row and checking the monotonic condition across all four columns — not reading a single cell."
    failure:
      answer: "A — North, because it contains the single highest quarterly value in the table."
      why: "Salient-cell error: the model picked the region with the biggest one-quarter number instead of testing the 'positive growth in every quarter' condition across all four columns, which North fails."
    checks: "Whether a model evaluates a condition across an entire structured row rather than seizing on one extreme value."
    scoring: "Accuracy over 4-option questions (25% random baseline); the parsed letter is compared to the gold answer."
tags: [long-context, reasoning, multiple-choice]
updated: 2026-08-02
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
