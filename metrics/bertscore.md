---
id: evaluation-metric/bertscore
type: evaluation-metric
slug: bertscore
schema_version: 3.0
title: "BERTScore"
name: "BERTScore"
category: generation
measures: "A high score means the tokens of the generated text and the reference closely match in contextual-embedding space."
needs: [generated text, reference text, contextual embedding model]
judged_by: model
range: { min: -1, max: 1 }
direction: higher-is-better
defined_in: { title: "BERTScore: Evaluating Text Generation with BERT (Zhang et al., ICLR 2020)", url: "https://arxiv.org/abs/1904.09675" }
related_metrics: [evaluation-metric/rouge]
tags: [generation, embeddings, semantic-similarity]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/1904.09675", title: "BERTScore: Evaluating Text Generation with BERT (arXiv:1904.09675)", trust: 0.95 }
---

## What It Measures
BERTScore compares a candidate text with a reference **token by token using contextual embeddings** instead of exact string matches, so paraphrases can still match.

## How It Is Computed
- **Embed:** a contextual model (for example BERT or RoBERTa) embeds every token of the reference x and the candidate x̂; vectors are pre-normalised, so cosine similarity is an inner product.
- **Greedy matching:** each token is matched to its most similar token in the other text.
- **Recall:** R = (1/|x|) × sum over reference tokens of the best similarity to a candidate token.
- **Precision:** P = (1/|x̂|) × sum over candidate tokens of the best similarity to a reference token.
- **F1:** F = 2PR / (P + R).
- **Optional idf weighting:** token weights from inverse document frequency over the reference sentences.
- **Range:** -1 to 1, the range of cosine similarity. Optional **baseline rescaling**, (score - b) / (1 - b), makes values typically fall between 0 and 1 without changing rankings.

## Limits
The paper states:

- **No configuration is best everywhere;** the user has to choose the embedding model and settings for the domain and language.
- **Low-resource languages:** scores from the multilingual BERT model are less stable.
- **Raw scores occupy a narrow range** in practice, which is why the paper adds baseline rescaling for readability.
