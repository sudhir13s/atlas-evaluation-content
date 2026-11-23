---
id: benchmark/mmmlu
type: benchmark
slug: mmmlu
schema_version: 4.0
title: MMMLU
name: MMMLU
category: Multilingual
description: OpenAI's translation of the MMLU test set into 14 languages by professional human translators.
what_is_it: 'Multilingual Massive Multitask Language Understanding (MMMLU), released by OpenAI on Hugging Face, is MMLU''s test set translated into 14 languages by professional human translators: Arabic, Bengali, German, Spanish, French, Hindi, Indonesian, Italian, Japanese, Korean, Brazilian Portuguese, Swahili, Yoruba and Simplified Chinese. Each language has 14,042 questions across MMLU''s 57 subjects.'
why_people_use_it: Every language holds the same questions, so scores can be compared across languages. OpenAI says relying on human translators increases confidence in the translations, especially for low-resource languages such as Yoruba.
examples:
- 'Abstract algebra, Arabic (Hugging Face openai/MMMLU, MIT licence): أوجد درجة امتداد الحقل المحدد Q(sqrt(2)، sqrt(3)، sqrt(18)) على Q. (Find the degree of the given field extension Q(sqrt(2), sqrt(3), sqrt(18)) over Q.) A: 0, B: 4, C: 2, D: 6. Answer: B'
method: 'Multiple choice: the model picks one of four options and its letter is compared with the key. OpenAI publishes the evaluation code in its simple-evals repository alongside the translations.'
metric: Accuracy = correct answers / total questions, per language and averaged across the 14 languages; random guessing scores 25%.
format: Four-option multiple choice (A to D), one correct answer.
limitations: The questions are translations of the English MMLU, so they keep MMLU's content and framing rather than knowledge native to each language, and any error in an original MMLU question carries into every translation.
citation: https://huggingface.co/datasets/openai/MMMLU
openness: open
openness_note: All translated questions and answer keys for the 14 languages are public on Hugging Face under the MIT licence, and OpenAI publishes the evaluation code.
worked_examples:
- task_type: US government / history (Yoruba)
  input: Ẹ̀kọ́ Monroe ní pàtàkì dá lórí kín? (The Monroe Doctrine was primarily concerned with what?)
  options:
  - Ìtakò ìjọba amúnisìn Yúróòpù ní Amẹ́ríkà. (Opposing further European colonization in the Americas.)
  - Ìṣòwò òmìnira pẹ̀lú Britain. (Free trade with Britain.)
  - Ìparí ẹrú. (The abolition of slavery.)
  - Ìtànkálẹ̀ sí ìwọ̀-oòrùn. (Westward expansion.)
  success:
    answer: A — Opposing further European colonization in the Americas
    why: Correct on the identical human-translated item — the same question English models get right.
  failure:
    answer: D — Westward expansion
    why: Conflates the Monroe Doctrine with Manifest Destiny; a plausible distractor. Scoring this wrong in Yoruba but right in English reveals a language gap, not a knowledge gap.
  checks: Language competence isolated from question difficulty — the item is identical across all 14 languages by construction.
  scoring: Accuracy (% correct).
- task_type: High-school biology (Swahili)
  input: Usanisinuru hutokea katika sehemu gani ya seli ya mmea? (In which part of a plant cell does photosynthesis occur?)
  options:
  - Kloroplasti. (Chloroplast.)
  - Mitokondria. (Mitochondrion.)
  - Kiini cha seli. (Nucleus.)
  - Ukuta wa seli. (Cell wall.)
  success:
    answer: A — Kloroplasti (Chloroplast)
    why: Correct on the identical translated item — photosynthesis occurs in the chloroplast.
  failure:
    answer: B — Mitokondria (Mitochondrion)
    why: Swaps the site of respiration for photosynthesis — a common in-language biology misconception; failing this in Swahili but not English exposes a language gap.
  checks: Isolating language competence on a fixed factual item shared across all 14 languages.
  scoring: Accuracy (% correct).
- task_type: US government (Arabic)
  input: كم عدد أعضاء مجلس الشيوخ الأمريكي؟ (How many members are in the United States Senate?)
  options:
  - '100'
  - '50'
  - '435'
  - '538'
  success:
    answer: A — 100
    why: Two senators per state across 50 states — the same fact regardless of the question's language.
  failure:
    answer: C — 435
    why: That is the size of the House of Representatives; confusing the two chambers is a plausible in-language mix-up, and getting it wrong only in Arabic signals a language gap.
  checks: Recall of a fixed Western-civics fact under a non-Latin script.
  scoring: Accuracy (% correct).
- task_type: Introductory psychology (Japanese)
  input: 古典的条件づけ（classical conditioning）の例はどれですか？ (Which of these is an example of classical conditioning?)
  options:
  - ベルの音で犬がよだれを流す。 (A dog salivates at the sound of a bell.)
  - 餌をもらってレバーを押す頻度が増える。 (A rat presses a lever more often after food rewards.)
  - 親のしぐさを子どもがまねる。 (A child imitates a parent's gesture.)
  - 褒められて勉強時間が増える。 (Studying increases after being praised.)
  success:
    answer: A — A dog salivates at the sound of a bell
    why: Classical (Pavlovian) conditioning — an involuntary response to an associated stimulus.
  failure:
    answer: B — A rat presses a lever more often after food rewards
    why: That is operant conditioning — the classic confusion with classical conditioning; failing it only in Japanese reveals a language gap, not a knowledge gap.
  checks: Distinguishing classical from operant conditioning on the identical translated item.
  scoring: Accuracy (% correct).
- task_type: College physics (German)
  input: Welche ist die SI-Einheit der Kraft? (Which is the SI unit of force?)
  options:
  - Newton (N)
  - Joule (J)
  - Watt (W)
  - Pascal (Pa)
  success:
    answer: A — Newton (N)
    why: Force is measured in newtons — a unit fact unchanged by the question's language.
  failure:
    answer: B — Joule (J)
    why: The joule is the unit of energy/work, not force — a plausible unit mix-up; missing it only in German points to a language gap.
  checks: Recall of SI units on a parallel item common to all 14 languages.
  scoring: Accuracy (% correct).
tags:
- multilingual
- knowledge
- multiple-choice
- translation
updated: '2026-11-23'
sources:
- url: https://huggingface.co/datasets/openai/MMMLU
  title: openai/MMMLU — Datasets at Hugging Face
  trust: 0.95
- url: https://datasets-server.huggingface.co/size?dataset=openai/MMMLU
  title: openai/MMMLU — row counts per locale (Hugging Face dataset viewer)
  trust: 0.9
- url: https://github.com/openai/simple-evals
  title: openai/simple-evals — evaluation code linked from the dataset card
  trust: 0.9
related:
- benchmark/mmlu
- benchmark/mmlu-pro
- benchmark/mmlu-redux
---

## What It Tests

MMLU's academic knowledge (57 subjects, from elementary level to professional subjects such as law and physics), read and answered in **14 languages**.

- **Parallel** — the same 14,042 questions in every language.
- **Human translation** — professional translators, not machine translation.

- **Maker** — OpenAI, released on Hugging Face with the evaluation code (`openai/simple-evals`).

## How It Scores

**Accuracy** = correct answers / total questions, per language and averaged across the 14 languages. Random guessing scores 25%.

## Format

Four-option multiple choice with one correct answer, as in MMLU.

- **Locales** — AR_XY, BN_BD, DE_DE, ES_LA, FR_FR, HI_IN, ID_ID, IT_IT, JA_JP, KO_KR, PT_BR, SW_KE, YO_NG and ZH_CN.
- **Size** — one test split of 14,042 rows per locale (196,588 in total).
- **Data** — MIT licence on Hugging Face (`openai/MMMLU`).

## Limitations

- **Translated content.** MMLU's English-origin questions, not knowledge native to each language.
- **Inherited errors.** A wrong key in MMLU stays wrong in every translation.

## Example

From the first row shown on the Hugging Face dataset card (abstract algebra, in Arabic; MIT licence):

- **Question** — "أوجد درجة امتداد الحقل المحدد Q(sqrt(2)، sqrt(3)، sqrt(18)) على Q." (Find the degree of the given field extension Q(sqrt(2), sqrt(3), sqrt(18)) over Q.)
- **Options** — A: 0, B: 4, C: 2, D: 6.
- **Key** — **B (4)**: sqrt(18) = 3·sqrt(2) adds nothing new, so only sqrt(2) and sqrt(3) each double the degree.
