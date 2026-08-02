---
id: benchmark/nova-63
type: benchmark
slug: nova-63
schema_version: 1.0
title: "NOVA-63"
name: NOVA-63
category: Multilingual
description: "A native-authored, difficulty-controlled multilingual exam over 63 disciplines."
what_is_it: "NOVA-63 (Native Omni-lingual Versatile Assessments of 63 Disciplines) is a 93,536-question benchmark sourced from native-language exam material across 14 languages and 63 academic disciplines — built to avoid translationese by not translating."
why_people_use_it: "It fixes the two big flaws of translated multilingual tests: content is natively authored (no translationese) and every item passes multi-level difficulty screening, so scores stay low and discriminative even for the strongest models."
examples:
  - "A native-language medical-licensing or law question in Chinese, Arabic, or Russian, drawn from that country's own exams rather than translated from English."
method: "Multiple-choice evaluation over 93,536 native-sourced questions spanning 63 disciplines in 14 languages; a pipeline of LLM-assisted formatting, expert quality verification, and multi-level difficulty screening curates the set."
metric: "Accuracy (% correct), analyzed per language, per language-family, and per discipline."
format: "Multiple-choice; native-language questions, no cross-language translation of items."
saturation: low
limitations: "Coverage is deep (63 disciplines) but breadth is 14 languages, so it is not a long-tail low-resource test like INCLUDE or Global PIQA. Native sourcing means questions are not parallel across languages, so a per-language score reflects both that language's difficulty mix and the model's ability — cross-language gaps must be read with that caveat."
citation: "https://aclanthology.org/2025.emnlp-main.364/"
worked_examples:
  - task_type: "Civil law (Chinese)"
    input: "根据《中华人民共和国民法典》，男性的法定结婚年龄是多少周岁？(Under the Civil Code of the People's Republic of China, what is the legal minimum marriage age for men?)"
    options:
      - "18"
      - "20"
      - "21"
      - "22"
    success:
      answer: "D — 22"
      why: "The PRC Civil Code sets the minimum marriage age at 22 for men — knowledge native to Chinese law, sourced from a Chinese-authored exam."
    failure:
      answer: "A — 18"
      why: "Picks the near-universal Western legal-adult default instead of the PRC-specific statute; strong English knowledge does not carry the local rule — the native-knowledge gap NOVA-63 isolates."
    checks: "Expert knowledge as it exists in a language's own legal tradition, not Western-default recall."
    scoring: "Accuracy (% correct)."
  - task_type: "Arabic grammar (Arabic)"
    input: "في النحو العربي، ما إعراب الفاعل؟ (In Arabic grammar, what is the grammatical case of the subject/doer?)"
    options:
      - "مرفوع — الرفع. (Nominative / raf‘.)"
      - "منصوب — النصب. (Accusative / naṣb.)"
      - "مجرور — الجر. (Genitive / jarr.)"
      - "مجزوم — الجزم. (Jussive / jazm.)"
    success:
      answer: "A — Nominative (raf‘)"
      why: "The fā‘il (subject) is always in the nominative case — a foundational rule of native Arabic grammar."
    failure:
      answer: "B — Accusative (naṣb)"
      why: "The accusative marks the object (maf‘ūl bih), not the subject; confusing the two is a native-grammar distinction generalist models often miss."
    checks: "Command of a language's own grammatical tradition, not translated linguistics."
    scoring: "Accuracy (% correct)."
  - task_type: "Constitutional law (Hindi)"
    input: "भारतीय संविधान में कितने मौलिक कर्तव्य हैं? (How many Fundamental Duties are there in the Indian Constitution?)"
    options:
      - "7"
      - "10"
      - "11"
      - "9"
    success:
      answer: "C — 11"
      why: "Originally 10; the 86th Amendment (2002) added an 11th (the duty on education), so the current count is 11 — a live fact of Indian polity."
    failure:
      answer: "B — 10"
      why: "The pre-2002 count before the 86th Amendment — a near-miss that misses the current constitutional figure taught in Indian exams."
    checks: "Up-to-date native civic knowledge as tested in a country's own curriculum."
    scoring: "Accuracy (% correct)."
  - task_type: "History (Russian)"
    input: "Кто ввёл «Табель о рангах» в России? (Who introduced the Table of Ranks in Russia?)"
    options:
      - "Пётр I — Пётр Великий. (Peter I / Peter the Great.)"
      - "Иван IV — Грозный. (Ivan IV the Terrible.)"
      - "Екатерина II. (Catherine II.)"
      - "Александр II. (Alexander II.)"
    success:
      answer: "A — Peter I (Peter the Great)"
      why: "Peter the Great established the Table of Ranks in 1722 — standard Russian-history knowledge from a Russian-authored exam."
    failure:
      answer: "C — Catherine II"
      why: "A plausible reformist-ruler guess, but the Table of Ranks predates her reign by decades — native Russian-history detail a generalist may lack."
    checks: "Depth in a language's own national history rather than broad Western-canon recall."
    scoring: "Accuracy (% correct)."
  - task_type: "Civil law (Japanese)"
    input: "日本の成年年齢は現在何歳ですか？ (In Japan, what is the current age of adulthood?)"
    options:
      - "18歳 (18 years)"
      - "20歳 (20 years)"
      - "21歳 (21 years)"
      - "16歳 (16 years)"
    success:
      answer: "A — 18"
      why: "Japan lowered the age of adulthood from 20 to 18, effective April 2022 — a current fact of Japanese civil law."
    failure:
      answer: "B — 20"
      why: "The long-standing pre-2022 value; choosing it misses the recent statutory change that a Japanese exam expects."
    checks: "Current, jurisdiction-specific legal knowledge in the language's own tradition."
    scoring: "Accuracy (% correct)."
tags: [multilingual, knowledge, reasoning, multiple-choice, native]
updated: 2026-08-02
sources:
  - { url: "https://aclanthology.org/2025.emnlp-main.364/", title: "NOVA-63: Native Omni-lingual Versatile Assessments of 63 Disciplines (EMNLP 2025 Main)", trust: 0.95 }
  - { url: "https://aclanthology.org/2025.emnlp-main.364.pdf", title: "NOVA-63 paper PDF (ACL Anthology)", trust: 0.95 }
related: [benchmark/include, benchmark/mmlu-prox, benchmark/mmmlu]
---

## What It Tests
Expert-level knowledge across 63 academic disciplines — the "63" in the name is disciplines, not
languages. It spans STEM, medicine, law, humanities and professional fields, in 14 languages,
using questions written natively in each language rather than translated from English.

## How It Scores
Accuracy over 93,536 questions, broken down by language, by language family, and by discipline.
Because every item survives multi-level difficulty screening, top models sit far below the
ceiling — the strongest public results reported are in the high-50s percent — which keeps the
benchmark discriminative where translated tests have saturated.

## Format
Multiple-choice questions sourced from native exam material in each of the 14 languages. A
curation pipeline combines large-language-model-assisted formatting, human expert quality
verification, and difficulty screening; items are not parallel translations, so each language's
set is its own native content.

## Limitations
It trades breadth for depth: 14 languages (not the 40-plus of INCLUDE or the 100-plus of Global
PIQA) but 63 disciplines with hard, screened questions. Non-parallel native content means a
per-language accuracy blends the local question mix with model ability, so raw cross-language
gaps are less clean than on a parallel set like MMMLU.

## Example
Rather than translating a US bar-exam question into Chinese, NOVA-63 draws an actual
Chinese-law question authored in Chinese, and an actual Russian-medicine question authored in
Russian. This native sourcing is the point: it measures knowledge as it exists in each language's
own academic tradition, avoiding the translationese and Western framing of translated benchmarks.
