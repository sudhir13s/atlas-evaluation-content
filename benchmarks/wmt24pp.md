---
id: benchmark/wmt24pp
type: benchmark
slug: wmt24pp
schema_version: 1.0
title: "WMT24++"
name: WMT24++
category: Multilingual
description: "Machine-translation benchmark from English into 55 languages and dialects."
what_is_it: "WMT24++ extends the WMT24 general machine-translation test set to 55 languages and dialects by collecting fresh human-written references and post-edits, covering four domains: literary, news, social, and speech."
why_people_use_it: "It is the current reference for measuring English-to-many translation quality with clean human references, and it showed that general-purpose LLMs now beat dedicated translation systems across all 55 languages."
examples:
  - "Translate an English news paragraph, a literary excerpt, a social-media post, or a speech transcript into each of the 55 target languages, scored against a human reference and post-edit."
method: "English-to-X translation of ~960 source segments per language across four domains; system outputs are scored automatically against human references and post-edits."
metric: "Automatic MT metrics — MetricX-24 (primary), chrF, and neural metrics such as XCOMET and CometKiwi; higher MetricX / chrF is better."
format: "Sentence/segment-level translation, English source into each target language (en to xx)."
saturation: medium
limitations: "The authors rely on automatic metrics and explicitly leave human evaluation to future work, so rankings can shift under human judgement. It is English-source only (one direction), the segment count per language is modest (~960), and metric-based scores are sensitive to reference quality — especially for low-resource dialects."
citation: "https://arxiv.org/abs/2502.12404"
example:
  task_type: "Machine translation (English to German, news domain)"
  input: "Translate into German: 'The committee did not approve the new budget.'"
  success:
    answer: "'Der Ausschuss hat den neuen Haushalt nicht genehmigt.' (The committee did not approve the new budget.)"
    why: "Fluent and faithful — the negation is preserved, so the meaning matches the source."
  failure:
    answer: "'Der Ausschuss hat den neuen Haushalt genehmigt.' (gloss: 'The committee approved the new budget.')"
    why: "Drops the negation 'not', flipping the sentence to its opposite meaning — a critical mistranslation despite fluent output."
  checks: "Meaning-preserving translation, including polarity/negation, scored automatically against a human reference."
  scoring: "Automatic MT metrics — MetricX-24 (primary) and chrF; higher is better."
tags: [multilingual, translation, machine-translation, generation]
updated: 2026-07-20
sources:
  - { url: "https://arxiv.org/abs/2502.12404", title: "WMT24++: Expanding the Language Coverage of WMT24 to 55 Languages & Dialects (Deutsch et al., 2025)", trust: 0.95 }
  - { url: "https://huggingface.co/papers/2502.12404", title: "WMT24++ paper page — Hugging Face", trust: 0.9 }
related: [benchmark/mmmlu, benchmark/global-piqa, benchmark/include]
---

## What It Tests
Machine-translation quality from English into 55 languages and dialects, across four content
domains — literary, news, social (social-media), and speech (transcripts). It is a
generation task, not multiple-choice: the model must produce a full translation, which is then
compared to human references.

## How It Scores
Automatic metrics against human references and post-edits. The paper's primary ranking uses
**MetricX-24** (a learned neural metric), alongside character-level **chrF** and other neural
metrics (**XCOMET**, **CometKiwi**). Higher is better. Under these metrics, large language models
were the best-performing translation systems in all 55 languages.

## Format
Segment-level, English-to-target (en to xx). WMT24++ reuses the 998 English source segments from
WMT24 — literary (206), news (149), social (531), speech (111) — and, after removing low-quality
items, evaluates roughly 960 segments shared across every language pair. The contribution is new
human references and post-edits for 46 additional languages plus improved post-edits for the
original set.

## Limitations
Scores come from automatic metrics; the authors leave human evaluation to future work, so
system rankings are provisional and could reorder under human judgement. It measures only the
English-to-X direction, the per-language segment count (~960) is modest, and neural-metric scores
depend heavily on reference quality, which is hardest to guarantee for low-resource dialects.

## Example
An English news sentence is translated into, say, Icelandic, Swahili, and Vallader Romansh; each
output is scored by MetricX-24 and chrF against a professional human reference. Because the same
~960 source segments are used for every language, per-language scores reflect translation
difficulty and model ability rather than differing test content.
