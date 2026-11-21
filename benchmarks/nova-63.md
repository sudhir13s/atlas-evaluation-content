---
id: benchmark/nova-63
type: benchmark
slug: nova-63
schema_version: 1.0
title: "NOVA-63"
name: NOVA-63
category: Multilingual
description: "A multiple-choice multilingual benchmark of questions sourced from native speakers in 14 languages, divided into 63 academic disciplines."
what_is_it: "NOVA-63 (Native Omni-lingual Versatile Assessments of 63 Disciplines; Zhang et al., EMNLP 2025) is a multiple-choice benchmark of 89,107 native questions across 14 languages, covering 8 language families. Questions in each language are divided into 13 primary and 63 secondary academic disciplines."
why_people_use_it: "Its questions are native content rather than translations of English material, and a pipeline of LLM-assisted formatting, expert quality verification and multi-level difficulty screening balances them across disciplines with consistent difficulty standards."
examples:
  - "A question written natively in one of the 14 languages, with a question stem, answer options and one correct answer, filed under one of 63 secondary disciplines."
method: "Multiple choice. To keep the set difficult, the authors selected questions on which large LLMs had accuracy below 50%, generated wrong options from incorrect LLM solutions, and reviewed all candidate questions manually for relevance, fluency and originality."
metric: "Accuracy: the proportion of correctly answered questions, analysed by language, language family and discipline."
format: "Multiple-choice questions written natively in each language; items are not parallel across languages."
limitations: "Questions differ from language to language, so a per-language score mixes that language's question set with the model's ability. The published paper gives 89,107 questions, while the ACL Anthology abstract page gives 93,536."
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
updated: 2026-11-21
sources:
  - { url: "https://aclanthology.org/2025.emnlp-main.364/", title: "NOVA-63: Native Omni-lingual Versatile Assessments of 63 Disciplines (EMNLP 2025 Main)", trust: 0.95 }
  - { url: "https://aclanthology.org/2025.emnlp-main.364.pdf", title: "NOVA-63 paper PDF (ACL Anthology)", trust: 0.95 }
related: [benchmark/include, benchmark/mmlu-prox, benchmark/mmmlu]
---

## What It Tests
Academic knowledge in **14 languages**, on questions sourced from native speakers rather than translated.

- **63 disciplines** — the "63" counts secondary disciplines, under 13 primary ones.
- **8 language families** — the paper says they cover about 69% of the global population.

## How It Scores
**Accuracy**, the proportion of questions answered correctly, broken down by language, language family and discipline.

## Format
Multiple-choice questions in each language's own content. The pipeline keeps questions large LLMs answered with under 50% accuracy, builds wrong options from incorrect LLM solutions, and has every candidate reviewed manually.

## Limitations
- **Not parallel.** Each language has its own questions, so cross-language gaps mix content and ability.
- **Two counts.** 89,107 questions in the paper; 93,536 on the Anthology abstract page.

## Example
Each item is a native-language question with its options and one correct answer, extracted from source material by an LLM and then checked by native-speaking reviewers. The model picks an option, and accuracy is reported per language, language family and discipline.
