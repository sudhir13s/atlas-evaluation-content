---
id: benchmark/mmlu-redux
type: benchmark
slug: mmlu-redux
schema_version: 1.0
title: "MMLU-Redux"
name: MMLU-Redux
category: Knowledge & Academic Reasoning
description: "A hand-re-annotated, error-corrected subset of MMLU used to measure knowledge without the noise of the original's wrong answer keys."
what_is_it: "A 5,700-question subset of MMLU that 14 experts manually re-checked against a formal error taxonomy, correcting or flagging items whose ground-truth answer was wrong, missing, or ambiguous — so scores reflect knowledge rather than label noise."
why_people_use_it: "It quantified that MMLU itself is buggy (~6.49% of questions have errors) and gives a cleaner surface for comparing near-saturated models, where a few percent of bad labels can flip rankings."
examples:
  - "A re-audit of an original MMLU virology item flagged as 'Wrong Ground Truth' — the answer key names an option that the domain expert re-annotator marks as incorrect, so the item is corrected or excluded rather than scored against the faulty key."
method: "Re-annotate 5,700 MMLU questions (100 per subject × 57 subjects) under a two-level error taxonomy, then re-score models on the corrected set and compare against original MMLU numbers."
metric: "Accuracy (% correct) on the corrected items."
format: "Same 4-way multiple-choice as MMLU; typically 0-shot or 5-shot."
saturation: high
limitations: "It corrects labels but keeps MMLU's 4-option multiple-choice format, so guessing headroom and the knowledge-only scope remain. Only 100 questions per subject are re-annotated, so per-subject estimates are noisy, and it inherits MMLU's contamination exposure."
citation: "https://arxiv.org/abs/2406.04127"
tags: [knowledge, multiple-choice, data-quality, mmlu]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2406.04127", title: "Are We Done with MMLU? (Gema et al., 2024)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/edinburgh-dawg/mmlu-redux", title: "MMLU-Redux dataset (Edinburgh DAWG)", trust: 0.9 }
  - { url: "https://github.com/aryopg/mmlu-redux", title: "MMLU-Redux GitHub repository", trust: 0.85 }
related: [benchmark/mmlu, benchmark/mmlu-pro, benchmark/gpqa]
---

## What It Tests
The same broad academic knowledge as MMLU across 57 subjects — but the point is measurement
quality, not new content. MMLU-Redux asks whether a model's MMLU score is even trustworthy once
the wrong answer keys are removed.

The authors found roughly **6.49% of MMLU questions contain errors**, with some subjects far worse
(over half of the sampled virology items were flawed). On near-saturated leaderboards that error
floor is enough to reorder models.

## How It Scores
Accuracy over the 5,700 re-annotated questions (100 per subject × 57 subjects). Each item was
audited under a two-level error taxonomy:

- **Question Assessment** — bad question clarity, or bad/ambiguous answer options.
- **Ground-Truth Verification** — no correct answer, multiple correct answers, or a wrong ground
  truth in the original key.

Items with correct labels are scored normally; flawed items are corrected or excluded, so the
resulting accuracy is a cleaner signal than raw MMLU.

## Format
Unchanged from MMLU: 4-way multiple-choice, run 0-shot or 5-shot depending on the harness. The
contribution is the corrected labels and the annotation protocol, not a new prompt format.

## Limitations
It fixes the answer keys but not the design: four options keep the random baseline at 25%, and the
scope stays knowledge-only (no generation, tools, or long context). Only 100 questions per subject
are re-checked, so per-subject error estimates carry real sampling noise, and the underlying items
remain as exposed to training-data contamination as MMLU.

## Example
Take an original MMLU item whose answer key marks option C as correct. A domain-expert re-annotator
reviewing it under the taxonomy determines that C is actually wrong and B is the defensible answer —
tagging it **Wrong Ground Truth**. In plain MMLU, a model that answered B is penalized; in
MMLU-Redux the key is corrected (or the item dropped), so the score reflects knowledge instead of
the benchmark's own mistake.
