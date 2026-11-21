---
id: evaluation-metric/rouge
type: evaluation-metric
slug: rouge
schema_version: 3.0
title: "ROUGE"
name: "ROUGE (Recall-Oriented Understudy for Gisting Evaluation)"
category: generation
measures: "A high score means the generated text shares many n-grams or long in-order word sequences with the reference texts."
needs: [generated text, reference texts]
judged_by: code
direction: higher-is-better
defined_in: { title: "ROUGE: A Package for Automatic Evaluation of Summaries (Lin, 2004)", url: "https://aclanthology.org/W04-1013/" }
implemented_by: [framework/ragas, framework/promptfoo]
related_metrics: [evaluation-metric/bertscore]
tags: [generation, summarization, n-gram-overlap]
updated: 2026-11-21
sources:
  - { url: "https://aclanthology.org/W04-1013/", title: "ROUGE: A Package for Automatic Evaluation of Summaries (Lin, 2004)", trust: 0.95 }
  - { url: "https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/traditional/", title: "Ragas documentation: Traditional (non-LLM) metrics", trust: 0.9 }
  - { url: "https://www.promptfoo.dev/docs/configuration/expected-outputs/", title: "promptfoo documentation: Assertions and metrics", trust: 0.9 }
---

## What It Measures
ROUGE (Recall-Oriented Understudy for Gisting Evaluation) scores a generated summary by **counting units it shares with human reference summaries**: n-grams, word sequences and word pairs.
Ragas implements ROUGE-1 and ROUGE-L (precision, recall or F-measure); promptfoo offers a ROUGE-N assertion with a pass threshold.

## How It Is Computed
The paper defines four measures:

- **ROUGE-N:** n-gram recall. Sum, over the reference summaries, of the n-grams that co-occur in the candidate, divided by the total n-grams in the references.
- **ROUGE-L:** F-measure built from the longest common subsequence (LCS): recall = LCS / reference length, precision = LCS / candidate length. It is 1 when the texts are identical and 0 when they share nothing.
- **ROUGE-W:** a weighted LCS that gives more credit to consecutive matches.
- **ROUGE-S:** overlap of skip-bigrams, word pairs in sentence order with any gap.
- **Multiple references:** the best pairwise score is taken; the package averages over leave-one-out reference sets.

## Limits
The paper states:

- **ROUGE-N ignores meaning:** "police kill the gunman" and "the gunman kill police" get the same ROUGE-2 score against "police killed the gunman".
- **ROUGE-L counts only one LCS,** so other in-order matches do not add to the score.
- **Multi-document summaries:** high correlation with human judgments was hard to reach and remained an open question.
