---
id: benchmark/aa-lcr
type: benchmark
slug: aa-lcr
schema_version: 4.0
title: AA-LCR
name: AA-LCR
category: Long Context
description: 'Artificial Analysis Long Context Reasoning: 100 text-based questions that need reasoning across sets of real documents averaging about 100k tokens each.'
what_is_it: 'Artificial Analysis Long Context Reasoning (AA-LCR), announced by Artificial Analysis on 5 August 2025, has 100 questions over 30 document sets (234 documents, about 2.98 million tokens in total, roughly 99k tokens per set). Each question needs information from several documents, and the answer is not stated directly: it has to be inferred from several pieces of information. Questions were written and validated by members of the Artificial Analysis research team and more than a dozen undergraduate students on short-term contracts.'
why_people_use_it: Artificial Analysis built it to replicate reasoning tasks knowledge workers expect models to do, unlike synthetic long-context tasks such as Needle in the Haystack. It is part of the Artificial Analysis evaluations.
examples:
- 'For the company and quarter where the company reported a 13.5% decline on the prior quarters operating income. What was their adjusted EBITDA? List the company name and adjusted EBITDA. (official answer: Equinix, $901 million; sample question from the dataset card)'
method: The prompt holds one document set, inserted between BEGIN and END INPUT DOCUMENTS markers in a fixed order, followed by the question (about 100k input tokens, counted with the cl100k_base tokenizer). An LLM-based equality checker decides whether the free-text answer matches the official answer; version 1.1 gives the checker a judge system prompt.
metric: Accuracy = questions judged correct / 100 questions, in percent.
format: Open-ended question answering over a multi-document input of about 100k tokens.
limitations: The set has 100 questions, so each question moves the score by one percentage point. The documents are text-only, across seven document types. Version 1.1 (September 2026) corrected sixteen answer keys and added the judge system prompt; the maker says scores on version 1.0.0 are not directly comparable with scores on version 1.1.
citation: https://artificialanalysis.ai/articles/announcing-aa-lcr
openness: open
openness_note: The questions and official answers are on Hugging Face under Apache-2.0, with pre-extracted document text and links to the original public documents, so anyone can rerun it.
worked_examples:
- task_type: Multi-document financial reasoning (~100k-token input)
  input: 'Given four company annual reports and one industry report (~100k tokens total), answer: which business segment drove the largest year-over-year operating-margin change? The figure is printed in no single table.'
  success:
    answer: Cloud Services — its margin fell from 31% to 22% YoY, the largest swing, computed by pairing the revenue and cost-of-revenue lines from the segment tables in Report B (p.44) and Report C (p.51).
    why: The answer is derived by locating revenue and cost figures scattered across two distant documents and computing the margins — no single passage states it.
  failure:
    answer: Hardware — its revenue grew the most year over year, so it drove the largest margin change.
    why: 'Incorrect answer from a nearby, easy-to-retrieve passage: the model grabbed a headline revenue-growth number from one document instead of combining revenue and cost across documents to compute the actual margin change.'
  checks: Whether a model integrates facts spread across several long documents through multi-step inference, versus retrieving one salient nearby number.
  scoring: Accuracy — free-text answer graded against a reference answer for correctness.
- task_type: Government-consultation aggregation (~100k-token input)
  input: Across a set of ~40 government consultation responses (~100k tokens), which single policy position is endorsed by a majority of respondents once differently-worded phrasings of the same position are treated as equivalent?
  success:
    answer: A phased carbon levy with rebates for low-income households — 23 of the 40 responses back it, though it is variously called a 'graduated emissions charge', 'staged carbon fee', and 'progressive levy'; reconciling those labels gives the majority.
    why: Requires reading positions scattered across many documents, normalizing synonymous terminology, then counting — the majority is stated nowhere and only emerges from combining the responses.
  failure:
    answer: An immediate flat carbon tax — it is the position argued most forcefully, in the longest and most detailed submission.
    why: 'Wrong passage weighting: the model latched onto the single most prominent submission instead of aggregating equivalent positions across all responses, so it reports a vocal minority as the majority.'
  checks: Whether a model reconciles synonymous claims across many documents and aggregates them, rather than over-weighting one salient response.
  scoring: Accuracy — free-text answer graded against a reference answer for correctness.
- task_type: Cross-contract legal reasoning (~100k-token input)
  input: 'Given three related commercial contracts and their amendments (~100k tokens), answer: under the current terms, may Party A subcontract delivery without Party B''s consent? The controlling clause is not in the main agreement.'
  success:
    answer: No — the master agreement permits subcontracting freely, but Amendment 2 to the side letter (§4.3) later revoked that right and now requires Party B's written consent, which the main contract's silence does not override.
    why: Correct only by tracing the clause through a later amendment in a separate document and recognizing it supersedes the original term — combining two distant passages.
  failure:
    answer: Yes — the master agreement's §12 expressly allows Party A to subcontract any obligation without restriction.
    why: 'Used the wrong passage: the model quoted the original master-agreement clause and missed that Amendment 2, in a different document, later revoked that freedom — it failed to combine the two distant provisions.'
  checks: Whether a model tracks how a later amendment in a distant document overrides an original clause.
  scoring: Accuracy — free-text answer graded against a reference answer for correctness.
- task_type: Academic cross-study synthesis (~100k-token input)
  input: 'Given five research papers on a drug''s efficacy (~100k tokens), answer: is the primary finding consistently replicated, and what is the main methodological reason any study diverges?'
  success:
    answer: Four of the five replicate the ~15% efficacy improvement; the outlier (Paper D) diverges because it used a 6-week follow-up window versus 12 weeks in the others — a difference stated in D's methods section and only meaningful when compared against the shared protocol described in Papers A and B.
    why: Requires confirming agreement across four documents and then explaining the outlier by cross-referencing its methods against the protocol defined in two other distant papers.
  failure:
    answer: No, the finding does not replicate — Paper D reports a much weaker effect, so the results are inconsistent across the literature.
    why: 'Stopped at one passage: the model reported Paper D''s diverging number without combining it with the other four papers or locating the follow-up-window difference that explains the discrepancy.'
  checks: Whether a model both confirms a result across multiple documents and diagnoses an outlier by combining distant methodological details.
  scoring: Accuracy — free-text answer graded against a reference answer for correctness.
- task_type: Survey-report reconciliation (~100k-token input)
  input: 'Given three market-survey reports covering overlapping regions (~100k tokens), answer: which product category shows the highest combined customer-satisfaction score across all regions, given each report weights regions differently?'
  success:
    answer: Home appliances — after re-weighting each report's regional scores to a common population basis (weights given in Report A's appendix, scores in Reports B and C), its blended score of 8.4 edges out electronics at 8.1.
    why: Correct only by pulling the weighting scheme from one report and the per-region scores from two others, then recomputing on a common basis — the ranking is stated in none of them.
  failure:
    answer: Electronics — it has the single highest satisfaction score, 8.9, reported in Report B's regional breakdown.
    why: 'Nearby-number error: the model took one report''s unweighted regional high without re-weighting across all three reports, so it missed that home appliances lead on the combined, comparably-weighted basis.'
  checks: Whether a model combines a weighting scheme and scores drawn from separate long documents instead of reporting one salient figure.
  scoring: Accuracy — free-text answer graded against a reference answer for correctness.
tags:
- long-context
- reasoning
- multi-document
- open-ended
updated: '2026-11-23'
sources:
- url: https://artificialanalysis.ai/articles/announcing-aa-lcr
  title: Announcing Artificial Analysis Long Context Reasoning (AA-LCR)
  trust: 0.95
- url: https://huggingface.co/datasets/ArtificialAnalysis/AA-LCR
  title: ArtificialAnalysis/AA-LCR dataset card (Hugging Face)
  trust: 0.9
related:
- benchmark/longbench-v2
---

## What It Tests

Reasoning across **several long documents at once**.

- **Multi-document** — each answer needs information from more than one document.
- **Multi-step** — the answer is not stated; it is inferred from several facts.
- **Real documents** — seven text-only types. Question counts per type, from the dataset card:

| Document type | Questions |
|---|---|
| Company documents | 63 |
| Government consultations | 11 |
| Industry reports | 8 |
| Legal | 6 |
| Marketing | 6 |
| Academia | 5 |
| Survey reports | 1 |

**Who built it:** the Artificial Analysis research team plus more than a dozen contracted undergraduate students, who checked difficulty against older, smaller models so that frontier models were not targeted.

## How It Scores

**Accuracy** over 100 questions from 30 document sets (234 documents).

- **Formula** — accuracy = questions judged correct / 100; each question is worth one percentage point.
- **Judge** — an **LLM-based equality checker** (a language model asked whether two answers mean the same thing) compares each answer with the official answer.
- **Human check** — the maker reports evaluators typically answered 40-60% correctly on a first attempt, and every question was answered correctly by at least one human tester.
- **Versions** — version 1.1 (September 2026) corrected sixteen answer keys (four of them substantive) and added a judge system prompt; 1.0.0 and 1.1 scores are not directly comparable.

## Format

The whole document set and one question go into the prompt; the model writes a **free-text answer**.

- **Prompt shape** — documents sit between BEGIN and END INPUT DOCUMENTS markers, then the question.
- **Length** — about 100k tokens per prompt, counted with the `cl100k_base` tokenizer.
- **Data** — `AA-LCR_Dataset.csv` (questions and answers, Apache-2.0) and `AA-LCR_extracted-text.zip` (document text) on [Hugging Face](https://huggingface.co/datasets/ArtificialAnalysis/AA-LCR). The maker places no licence over the document text, which comes from public documents.

## Limitations

- **Small set.** 100 questions: one question is one percentage point.
- **Text only.** No images or other modalities in the documents.
- **Version split.** Check whether a score used version 1.0.0 or 1.1.

## Example

The dataset card's sample question: "For the company and quarter where the company reported a 13.5% decline on the prior quarters operating income. What was their adjusted EBITDA? List the company name and adjusted EBITDA." The official answer is "Equinix, $901 million", which the equality checker compares with the model's answer.
