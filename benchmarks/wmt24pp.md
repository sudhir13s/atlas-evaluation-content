---
id: benchmark/wmt24pp
type: benchmark
slug: wmt24pp
schema_version: 3.1
title: "WMT24++"
name: WMT24++
category: Multilingual
description: "A machine-translation benchmark from English into 55 languages and dialects, built on the WMT24 English sources."
what_is_it: "WMT24++ (Deutsch et al., February 2025, mostly Google authors) extends the WMT24 dataset to 55 languages and dialects by collecting new human-written references and post-edits for 46 new languages and dialects, plus post-edits of the references in 8 of the 9 original WMT24 languages (Icelandic keeps its original reference). Professional translators produced the references and post-edits. The 998 English source segments are paragraphs from four domains, literary, news, social and speech, plus one canary segment."
why_people_use_it: "It gives English-to-many translation test data with human references across 55 languages. Using automatic metrics, the authors found large language models (LLMs) to be the best-performing machine translation (MT) systems in all 55 languages, and say this should be confirmed by human evaluation (2025)."
examples:
  - "Siso's depictions of land, water center new gallery exhibition. (an English news source segment from the public Hugging Face data, to be translated into each target language)"
method: "English-to-X translation of paragraph-level sources. The paper removed 38 low-quality sources (mostly URLs, user tags, HTML and emojis, flagged is_bad_source in the data) from evaluation; outputs are scored with automatic metrics against the post-edited human reference."
metric: "Automatic MT metrics: MetricX-24 and MetricX-24-QE (a reference-free quality-estimation variant) in the main paper; BLEU, chrF, XCOMET, XCOMET-QE, COMETKiwi-23 and Gemini-DA scores in the appendix. MetricX scores are error scores, so lower is better."
format: "Paragraph-level translation from English into each target language (en to xx)."
limitations: "Results rest on automatic metrics, and human evaluation is left for future work. The authors caution that quality-estimation metrics rank human translations lower than human evaluators do, so apparent super-human MT scores should not be taken at face value. Only the English-to-X direction is covered. The data is public, so it can appear in training sets."
citation: "https://arxiv.org/abs/2502.12404"
openness: open
openness_note: "All 55 language pairs, with English sources, post-edited and original references and a bad-source flag, are public on Hugging Face under Apache-2.0."
worked_examples:
  - task_type: "Machine translation — negation (English → German, news)"
    input: "Translate into German: 'The committee did not approve the new budget.'"
    success:
      answer: "'Der Ausschuss hat den neuen Haushalt nicht genehmigt.' (The committee did not approve the new budget.)"
      why: "Fluent and faithful — the negation is preserved, so the meaning matches the source."
    failure:
      answer: "'Der Ausschuss hat den neuen Haushalt genehmigt.' (gloss: 'The committee approved the new budget.')"
      why: "Drops the negation 'not', flipping the sentence to its opposite meaning — a critical mistranslation despite fluent output."
    checks: "Meaning-preserving translation, including polarity/negation, scored automatically against a human reference."
    scoring: "Automatic MT metrics — MetricX-24 (primary; an error score, lower is better) and chrF (higher is better)."
  - task_type: "Machine translation — register (English → French, speech/announcement)"
    input: "Translate into French, as a formal public announcement: 'Please remain seated until the train has stopped.'"
    success:
      answer: "'Veuillez rester assis jusqu'à l'arrêt complet du train.' (Please remain seated until the train has completely stopped.)"
      why: "Uses the formal register (veuillez) appropriate to a public announcement and preserves the meaning."
    failure:
      answer: "'Reste assis jusqu'à ce que le train s'arrête.' (gloss: 'Stay seated until the train stops' — casual tu-form.)"
      why: "Correct meaning but the wrong register: the informal tu-imperative is inappropriate for a formal announcement, which the metrics penalize as an adequacy/register error."
    checks: "Matching the source's formality register, not just its literal meaning."
    scoring: "Automatic MT metrics — MetricX-24 (primary; an error score, lower is better) and chrF (higher is better)."
  - task_type: "Machine translation — false friend (English → Spanish, news)"
    input: "Translate into Spanish: 'The minister said the policy is actually working.'"
    success:
      answer: "'El ministro dijo que la política realmente está funcionando.' (The minister said the policy really is working.)"
      why: "'realmente' conveys 'in fact', preserving the source's claim."
    failure:
      answer: "'El ministro dijo que la política actualmente está funcionando.' (gloss: 'the policy is currently working'.)"
      why: "'actualmente' is a false friend meaning 'currently', not 'actually' — a mistranslation that shifts the meaning and worsens the MetricX-24 score."
    checks: "Avoiding false-friend traps that silently change meaning."
    scoring: "Automatic MT metrics — MetricX-24 (primary; an error score, lower is better) and chrF (higher is better)."
  - task_type: "Machine translation — politeness register (English → Japanese, social/business)"
    input: "Translate into Japanese, as a business email reply: 'Thank you for your email; I will reply by tomorrow.'"
    success:
      answer: "'メールをありがとうございます。明日までにご返信いたします。' (Thank you for your email; I will reply by tomorrow — polite keigo form.)"
      why: "Uses the polite keigo register expected in business correspondence while rendering the message faithfully."
    failure:
      answer: "'メールありがとう。明日までに返事するね。' (gloss: 'Thanks for the mail, I'll reply by tomorrow' — casual form.)"
      why: "The meaning is intact but the casual register is wrong for a business email; register mismatch is penalized as an adequacy/fluency error."
    checks: "Producing the correct politeness level for the text's context, not merely the literal content."
    scoring: "Automatic MT metrics — MetricX-24 (primary; an error score, lower is better) and chrF (higher is better)."
  - task_type: "Machine translation — idiom (English → Chinese, literary/dialogue)"
    input: "Translate into Chinese: 'Break a leg at your audition tonight!'"
    success:
      answer: "'祝你今晚试镜顺利！' (Wishing you a smooth audition tonight!)"
      why: "Renders the idiom's intended meaning — good luck — which is what the source conveys."
    failure:
      answer: "'今晚试镜时把腿摔断！' (gloss: 'Break your leg at the audition tonight!' — literal.)"
      why: "Translates the idiom word-for-word into a literal, alarming instruction, destroying the intended meaning — a mistranslation the metrics heavily penalize."
    checks: "Translating idiomatic meaning rather than a misleading literal rendering."
    scoring: "Automatic MT metrics — MetricX-24 (primary; an error score, lower is better) and chrF (higher is better)."
tags: [multilingual, translation, machine-translation, generation]
updated: 2026-11-23
sources:
  - { url: "https://arxiv.org/abs/2502.12404", title: "WMT24++: Expanding the Language Coverage of WMT24 to 55 Languages & Dialects (Deutsch et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2502.12404", title: "WMT24++, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/google/wmt24pp", title: "google/wmt24pp dataset card (Hugging Face, Apache-2.0)", trust: 0.9 }
related: [benchmark/mmmlu, benchmark/global-piqa, benchmark/include]
---

## What It Tests
**Machine-translation quality** from English into 55 languages and dialects.

- **Four domains** — literary, news, social (social-media posts) and speech (transcripts).
- **Generation** — not multiple choice: the model writes a full translation, which is compared with a human reference.
- **Coverage** — 46 new languages and dialects plus the 9 original WMT24 targets.

**Who built it:** Daniel Deutsch and colleagues, mostly at **Google** (one author at Unbabel), arXiv February 2025. Professional translators in each region wrote the references and post-edits. A **post-edit** is a translator's correction of an existing translation.

## How It Scores
**Automatic metrics** against the post-edited human references.

- **Main paper** — MetricX-24 and its reference-free variant MetricX-24-QE. MetricX gives an **error score**: lower is better.
- **Appendix** — BLEU, chrF, XCOMET, XCOMET-QE, COMETKiwi-23 and Gemini-DA scores.
- **Paper's finding** — LLMs were the best-performing MT systems in all 55 languages under these metrics (2025).

## Format
Paragraph-level, English to target (en to xx). The 998 English source segments:

| Domain | Segments |
|---|---|
| Social | 531 |
| Literary | 206 |
| News | 149 |
| Speech | 111 |
| Canary (marker segment) | 1 |

- **Removed** — the paper drops 38 low-quality sources from evaluation; the data flags them with `is_bad_source`.
- **Data** — one 998-row file per language pair on [Hugging Face](https://huggingface.co/datasets/google/wmt24pp) (Apache-2.0), with `source`, `target` (post-edit, the recommended reference) and `original_target`.

## Limitations
- **No human evaluation yet.** The authors leave it to future work.
- **Metric bias.** Quality-estimation metrics rank human translations lower than human evaluators do.
- **One direction.** English source only.
- **Public items.** Sources and references can leak into training data.

## Example
From the public data on the Hugging Face dataset card (Apache-2.0), segment 1 of the en-ar_EG pair is a **news** headline: "Siso's depictions of land, water center new gallery exhibition."

- **Task** — translate it into Egyptian Arabic (and, in other pairs, into each of the other targets).
- **Scoring** — the output is compared with the post-edited `target` reference using MetricX-24.
