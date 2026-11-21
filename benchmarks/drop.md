---
id: benchmark/drop
type: benchmark
slug: drop
schema_version: 1.0
title: "DROP"
name: DROP
category: Math & Logic
description: "Discrete Reasoning Over Paragraphs: a reading comprehension benchmark whose questions need discrete operations such as addition, counting or sorting over a passage."
what_is_it: "DROP was introduced by Dua et al. (2019). It is an English reading comprehension benchmark of 96,567 crowdsourced, adversarially created questions over Wikipedia passages, with an emphasis on sports game summaries and history. To answer, a system must resolve references in the question and perform discrete operations over the passage, such as addition, counting or sorting."
why_people_use_it: "The authors built DROP to need a more complete understanding of paragraphs than earlier reading comprehension datasets, combining finding facts in a passage with computing over them."
examples:
  - "Passage: 'In 2010 the city had 250,000 residents; by 2015 it grew 12%; in 2020 it fell 5% from 2015.' Q: population in 2020? (answer: 250,000 * 1.12 * 0.95 = 266,000)"
method: "The model reads a passage and a question and gives a free-form answer (a number, a date or text spans), compared with the gold answers."
metric: "Exact match and a numeracy-focused F1 (0-100); F1 is 0 when the numbers in the gold and predicted answers differ."
format: "Free-form answers over a passage; zero-shot or few-shot."
limitations: "F1 gives partial credit for word overlap on text answers. Answer normalisation for numbers, dates and multiple spans affects scores, so harnesses can disagree."
citation: "https://arxiv.org/abs/1903.00161"
worked_examples:
  - task_type: "Sequential percentage reasoning"
    input: "Passage: In 2010 the city had 250,000 residents. By 2015 the population grew by 12%. In 2020 it decreased by 5% from the 2015 figure. Question: What was the population in 2020?"
    success:
      answer: "266,000"
      why: "Applies the percentages sequentially: 250,000 * 1.12 = 280,000, then 280,000 * 0.95 = 266,000."
    failure:
      answer: "267,500"
      why: "Combined the two changes additively (+12% then -5% read as +7%), computing 250,000 * 1.07 instead of compounding them in order."
    checks: "Locating the right figures in text AND compounding operations in the correct order, not just extracting a span."
    scoring: "Numeracy-aware F1 and exact match (0-100)."
  - task_type: "Counting and comparison"
    input: "Passage: The home team scored in the 3rd, 14th, 41st, and 78th minutes. The visitors scored in the 22nd and 55th minutes. Question: How many more goals did the home team score than the visitors?"
    success:
      answer: "2"
      why: "The home team scored 4 goals and the visitors 2; the difference is 4 - 2 = 2."
    failure:
      answer: "6"
      why: "Answered the total number of goals (4 + 2 = 6) instead of the 'how many more' difference the question asks for."
    checks: "Counting each set of events AND performing the comparison the question specifies, not just tallying everything."
    scoring: "Numeracy-aware F1 and exact match (0-100)."
  - task_type: "Date-span duration"
    input: "Passage: Construction of the bridge began in March 1998 and was completed in November 2003. Question: How many years did the construction take?"
    success:
      answer: "5"
      why: "From 1998 to 2003 is a span of 2003 - 1998 = 5 years."
    failure:
      answer: "6"
      why: "Counted both endpoint years inclusively (2003 - 1998 + 1), over-counting the span by one."
    checks: "Computing an elapsed duration as a difference of years rather than an inclusive count."
    scoring: "Numeracy-aware F1 and exact match (0-100)."
  - task_type: "Argmax / longest-of selection"
    input: "Passage: In the quarter, Rossi ran for a 12-yard touchdown, Kim caught a 27-yard touchdown pass, and Lopez returned a kickoff 45 yards for a score. Question: Who scored on the longest play?"
    success:
      answer: "Lopez"
      why: "Comparing 12, 27, and 45 yards across all scoring plays, Lopez's 45-yard return is the longest."
    failure:
      answer: "Kim"
      why: "Restricted the comparison to passing plays, picking Kim's 27-yard reception and ignoring Lopez's longer kickoff return."
    checks: "Selecting the maximum across every relevant span in the passage, not within one sub-category."
    scoring: "Numeracy-aware F1 and exact match (0-100)."
  - task_type: "Difference with correct ordering"
    input: "Passage: The company reported revenue of $4.2 billion in 2019 and $3.6 billion in 2018. Question: By how much did revenue increase from 2018 to 2019?"
    success:
      answer: "0.6 billion"
      why: "The increase is the later figure minus the earlier: 4.2 - 3.6 = 0.6 billion."
    failure:
      answer: "-0.6 billion"
      why: "Subtracted in the wrong direction (2018 minus 2019, 3.6 - 4.2), reversing the sign of the change."
    checks: "Subtracting in the order the question implies (increase = later minus earlier)."
    scoring: "Numeracy-aware F1 and exact match (0-100)."
tags: [reading-comprehension, numerical-reasoning, f1]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/1903.00161", title: "DROP: A Reading Comprehension Benchmark Requiring Discrete Reasoning Over Paragraphs (Dua et al., 2019)", trust: 0.95 }
  - { url: "https://ar5iv.labs.arxiv.org/html/1903.00161", title: "DROP paper, full text (ar5iv)", trust: 0.95 }
related: [benchmark/gsm8k, benchmark/bbh]
---

## What It Tests
**Reading plus computation**: find the relevant numbers, dates or entities in a passage, then add, count, sort or compare them.

## How It Scores
Two metrics, both 0 to 100.

- **Exact match** — after simple normalisation, as in SQuAD.
- **Numeracy-focused F1** — bag-of-words overlap, set to 0 when the numbers differ.

## Format
A passage and a question; the answer is a number, a date or one or more text spans.

## Limitations
- **Partial credit.** F1 rewards overlap on text answers.
- **Normalisation.** Handling of numbers, dates and span sets affects scores.

## Example
Passage: "In 2010 the city had 250,000 residents. By 2015 the population grew by 12%. In 2020 it
decreased by 5% from the 2015 figure." Q: "What was the population in 2020?" Correct chain:
250,000 × 1.12 = 280,000, then 280,000 × 0.95 = **266,000**. Adding the percentages directly
(250,000 × 1.07 = 267,500) is the classic error the item catches.
