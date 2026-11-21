---
id: evaluation-metric/faithfulness
type: evaluation-metric
slug: faithfulness
schema_version: 3.0
title: "Faithfulness"
name: "Faithfulness"
category: generation
measures: "A high score means most claims in the answer can be inferred from the retrieved contexts."
needs: [question, answer, retrieved contexts]
judged_by: model
range: { min: 0, max: 1 }
direction: higher-is-better
defined_in: { title: "Ragas documentation: Faithfulness", url: "https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/faithfulness/" }
implemented_by: [framework/ragas]
related_metrics: [evaluation-metric/answer-relevancy]
tags: [rag, generation, groundedness, hallucination, llm-as-judge]
updated: 2026-11-21
sources:
  - { url: "https://docs.ragas.io/en/stable/concepts/metrics/available_metrics/faithfulness/", title: "Ragas documentation: Faithfulness", trust: 0.9 }
---

## What It Measures
Faithfulness, **as defined by Ragas**, scores how **factually consistent** an answer is with the retrieved contexts. Other tools call this property groundedness. An answer is faithful when every claim it makes is supported by the contexts.

## How It Is Computed
- **Step 1:** break the answer into individual claims.
- **Step 2:** a language model checks whether each claim can be inferred from the retrieved contexts.
- **Formula:** Faithfulness = (claims in the answer supported by the retrieved contexts) / (total claims in the answer).
- **Range:** 0 to 1.
- **Variant:** a version that uses Vectara's HHEM-2.1-Open classifier for the verification step.

## Limits
- **Scope set by the definition:** the score is consistency with the retrieved contexts, not a check against facts outside them.
- **The documentation states no further limits;** in the default variant both claim splitting and verification are done by a language model.
