---
id: benchmark/mmmlu
type: benchmark
slug: mmmlu
schema_version: 1.0
title: "MMMLU"
name: MMMLU
category: Multilingual
description: "OpenAI's professionally human-translated MMLU test set in 14 languages."
what_is_it: "The Multilingual Massive Multitask Language Understanding (MMMLU) set is the full MMLU test split translated by professional human translators into 14 languages, so the same 14,042 questions can be scored in each language."
why_people_use_it: "It is the most-cited apples-to-apples multilingual knowledge test: because every language is a human translation of the identical questions, a model's score gap between English and, say, Yoruba isolates language ability from question difficulty."
examples:
  - "A US-foreign-policy MMLU item such as 'The Monroe Doctrine was primarily concerned with...' presented in Arabic, Swahili, or Japanese, with the same four answer options translated."
method: "0-shot or few-shot multiple-choice over 14,042 parallel questions per language across 57 subjects; the predicted answer letter is parsed and compared to the gold key."
metric: "Accuracy (% correct), reported per language and averaged."
format: "4-way multiple-choice, one correct option; typically run 0-shot or 5-shot."
saturation: high
limitations: "It is a translation of English-origin MMLU, so it carries MMLU's US/Western cultural framing and any of MMLU's original label errors into every language — it measures multilingual knowledge recall, not culturally-native knowledge. High-resource languages are near-saturated; the signal now lives in the low-resource tail (e.g. Yoruba, Swahili)."
citation: "https://huggingface.co/datasets/openai/MMMLU"
worked_examples:
  - task_type: "US government / history (Yoruba)"
    input: "Ẹ̀kọ́ Monroe ní pàtàkì dá lórí kín? (The Monroe Doctrine was primarily concerned with what?)"
    options:
      - "Ìtakò ìjọba amúnisìn Yúróòpù ní Amẹ́ríkà. (Opposing further European colonization in the Americas.)"
      - "Ìṣòwò òmìnira pẹ̀lú Britain. (Free trade with Britain.)"
      - "Ìparí ẹrú. (The abolition of slavery.)"
      - "Ìtànkálẹ̀ sí ìwọ̀-oòrùn. (Westward expansion.)"
    success:
      answer: "A — Opposing further European colonization in the Americas"
      why: "Correct on the identical human-translated item — the same question English models get right."
    failure:
      answer: "D — Westward expansion"
      why: "Conflates the Monroe Doctrine with Manifest Destiny; a plausible distractor. Scoring this wrong in Yoruba but right in English reveals a language gap, not a knowledge gap."
    checks: "Language competence isolated from question difficulty — the item is identical across all 14 languages by construction."
    scoring: "Accuracy (% correct)."
  - task_type: "High-school biology (Swahili)"
    input: "Usanisinuru hutokea katika sehemu gani ya seli ya mmea? (In which part of a plant cell does photosynthesis occur?)"
    options:
      - "Kloroplasti. (Chloroplast.)"
      - "Mitokondria. (Mitochondrion.)"
      - "Kiini cha seli. (Nucleus.)"
      - "Ukuta wa seli. (Cell wall.)"
    success:
      answer: "A — Kloroplasti (Chloroplast)"
      why: "Correct on the identical translated item — photosynthesis occurs in the chloroplast."
    failure:
      answer: "B — Mitokondria (Mitochondrion)"
      why: "Swaps the site of respiration for photosynthesis — a common in-language biology misconception; failing this in Swahili but not English exposes a language gap."
    checks: "Isolating language competence on a fixed factual item shared across all 14 languages."
    scoring: "Accuracy (% correct)."
  - task_type: "US government (Arabic)"
    input: "كم عدد أعضاء مجلس الشيوخ الأمريكي؟ (How many members are in the United States Senate?)"
    options:
      - "100"
      - "50"
      - "435"
      - "538"
    success:
      answer: "A — 100"
      why: "Two senators per state across 50 states — the same fact regardless of the question's language."
    failure:
      answer: "C — 435"
      why: "That is the size of the House of Representatives; confusing the two chambers is a plausible in-language mix-up, and getting it wrong only in Arabic signals a language gap."
    checks: "Recall of a fixed Western-civics fact under a non-Latin script."
    scoring: "Accuracy (% correct)."
  - task_type: "Introductory psychology (Japanese)"
    input: "古典的条件づけ（classical conditioning）の例はどれですか？ (Which of these is an example of classical conditioning?)"
    options:
      - "ベルの音で犬がよだれを流す。 (A dog salivates at the sound of a bell.)"
      - "餌をもらってレバーを押す頻度が増える。 (A rat presses a lever more often after food rewards.)"
      - "親のしぐさを子どもがまねる。 (A child imitates a parent's gesture.)"
      - "褒められて勉強時間が増える。 (Studying increases after being praised.)"
    success:
      answer: "A — A dog salivates at the sound of a bell"
      why: "Classical (Pavlovian) conditioning — an involuntary response to an associated stimulus."
    failure:
      answer: "B — A rat presses a lever more often after food rewards"
      why: "That is operant conditioning — the classic confusion with classical conditioning; failing it only in Japanese reveals a language gap, not a knowledge gap."
    checks: "Distinguishing classical from operant conditioning on the identical translated item."
    scoring: "Accuracy (% correct)."
  - task_type: "College physics (German)"
    input: "Welche ist die SI-Einheit der Kraft? (Which is the SI unit of force?)"
    options:
      - "Newton (N)"
      - "Joule (J)"
      - "Watt (W)"
      - "Pascal (Pa)"
    success:
      answer: "A — Newton (N)"
      why: "Force is measured in newtons — a unit fact unchanged by the question's language."
    failure:
      answer: "B — Joule (J)"
      why: "The joule is the unit of energy/work, not force — a plausible unit mix-up; missing it only in German points to a language gap."
    checks: "Recall of SI units on a parallel item common to all 14 languages."
    scoring: "Accuracy (% correct)."
tags: [multilingual, knowledge, multiple-choice, translation]
updated: 2026-08-02
sources:
  - { url: "https://huggingface.co/datasets/openai/MMMLU", title: "openai/MMMLU — Datasets at Hugging Face", trust: 0.95 }
  - { url: "https://www.marktechpost.com/2024/09/23/openai-releases-multilingual-massive-multitask-language-understanding-mmmlu-dataset-on-hugging-face-to-easily-evaluate-multilingual-llms/", title: "OpenAI Releases MMMLU Dataset on Hugging Face (2024)", trust: 0.7 }
related: [benchmark/mmlu, benchmark/mmlu-pro, benchmark/mmlu-redux]
---

## What It Tests
The same broad academic knowledge as MMLU (57 subjects, from elementary math to professional
law and medicine) but read and answered in a non-English language. Because the questions are
identical across languages, it isolates *language competence* from *subject difficulty*.

## How It Scores
Accuracy over 14,042 questions per language, reported for each of the 14 languages and averaged.
The 14 languages span Europe (German, Spanish, French, Italian, Portuguese), Asia (Chinese,
Japanese, Korean, Hindi, Bengali, Indonesian), the Middle East (Arabic), and Africa (Swahili,
Yoruba). The per-language spread — not the average — is the headline number.

## Format
Four-way multiple choice, one correct option, usually run 0-shot or 5-shot. Crucially, every
item is a *human* translation: OpenAI paid professional translators for one translation per
question per language rather than machine-translating, which removes translationese as a
confound — a first for a benchmark of this size.

## Limitations
The questions originate from English MMLU, so their cultural and topical framing stays Western
regardless of the target language — MMMLU asks "can you answer US-centric questions in Swahili?",
not "what does a Swahili speaker's world contain?". It also inherits MMLU's known label noise,
and high-resource languages are already near the ceiling, leaving useful signal mainly in
low-resource languages.

## Example
An MMLU item like "In the context of US foreign policy, the Monroe Doctrine primarily aimed to..."
appears verbatim-translated into each of the 14 languages, with the same four options. A model
that scores 88% in English but 61% in Yoruba reveals a language gap, not a knowledge gap — the
question set is by construction the same.
