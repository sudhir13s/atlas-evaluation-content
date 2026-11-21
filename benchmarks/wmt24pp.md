---
id: benchmark/wmt24pp
type: benchmark
slug: wmt24pp
schema_version: 1.0
title: "WMT24++"
name: WMT24++
category: Multilingual
description: "A machine-translation benchmark from English into 55 languages and dialects, built on the WMT24 English sources."
what_is_it: "WMT24++ (Deutsch et al., 2025, Google) extends the WMT24 dataset to 55 languages and dialects by collecting new human-written references and post-edits for 46 new languages and dialects, plus post-edits of the references in 8 of the 9 original WMT24 languages. The 998 English source paragraphs cover four domains: literary, news, social and speech."
why_people_use_it: "It gives English-to-many translation test data with human references across 55 languages. Using automatic metrics, the authors found LLMs to be the best-performing MT systems in all 55 languages, and say this should be confirmed by human evaluation (2025)."
examples:
  - "An English source paragraph from the literary, news, social or speech domain, translated into one of the 55 target languages and scored against the human reference."
method: "English-to-X translation of paragraph-level sources. The paper removed 38 low-quality sources (mostly URLs or emojis), so evaluation uses 960 of the 998; outputs are scored with automatic metrics."
metric: "Automatic MT metrics: MetricX-24 and MetricX-24-QE in the main paper; BLEU, chrF, XCOMET, XCOMET-QE and COMETKiwi-23 in the appendix."
format: "Paragraph-level translation from English into each target language (en to xx)."
limitations: "Results rest on automatic metrics, and human evaluation is left for future work. The authors caution that automatic metrics are known to be biased against human translations, so apparent super-human MT scores should not be taken at face value. Only the English-to-X direction is covered."
citation: "https://arxiv.org/abs/2502.12404"
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
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2502.12404", title: "WMT24++: Expanding the Language Coverage of WMT24 to 55 Languages & Dialects (Deutsch et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2502.12404", title: "WMT24++, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://huggingface.co/datasets/google/wmt24pp", title: "google/wmt24pp dataset (Hugging Face)", trust: 0.9 }
related: [benchmark/mmmlu, benchmark/global-piqa, benchmark/include]
---

## What It Tests
Machine-translation quality from English into 55 languages and dialects, across four content
domains — literary, news, social (social-media), and speech (transcripts). It is a
generation task, not multiple-choice: the model must produce a full translation, which is then
compared to human references.

## How It Scores
**Automatic metrics** against human references and post-edits.

- **Main paper** — MetricX-24 and its reference-free variant MetricX-24-QE.
- **Appendix** — BLEU, chrF, XCOMET, XCOMET-QE and COMETKiwi-23.
- **Paper's finding** — LLMs were the best-performing MT systems in all 55 languages under these metrics (2025).

## Format
Paragraph-level, English to target (en to xx). The 998 WMT24 English sources: literary 206, news 149, social 531, speech 111. The paper drops 38 low-quality sources, leaving 960.

## Limitations
- **No human evaluation yet.** The authors leave it to future work.
- **Metric bias.** Automatic metrics are known to score human translations too low.
- **One direction.** English source only.

## Example
An English source paragraph from one of the four domains is translated into a target language, and the output is scored with MetricX-24 against that language's human reference.
