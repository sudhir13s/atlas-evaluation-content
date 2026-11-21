---
id: benchmark/maxife
type: benchmark
slug: maxife
schema_version: 1.0
title: "MaXIFE"
name: MaXIFE
category: Multilingual
description: "An instruction-following benchmark with 1,667 verifiable instruction tasks in 23 languages, scored by rules or by a judge model."
what_is_it: "MaXIFE (Multilingual and Cross-lingual Instruction Following Evaluation; Liu et al., ACL 2025; Waseda University and OPPO AI Center) combines 795 basic questions with 47 instructions, translated in parallel into 23 languages. Each question carries one to three instructions, giving 1,667 instructions and 795 evaluation items per language (18,285 entries in all)."
why_people_use_it: "The authors note that existing instruction-following evaluations focus on a single language, and that the same model can follow instructions differently across languages (2025 paper)."
examples:
  - "Give a base question plus verifiable constraints such as 'respond in Japanese', 'use at least 200 words', or 'wrap the answer in JSON', then programmatically check whether the output satisfies each constraint."
method: "Instructions fall into 11 categories. Seven (format, repeat, keywords, marks, citation, length, emoji) are checked by rule-based evaluation; instructions that need semantic understanding are scored by model-based evaluation. Basic questions were translated by GPT-4o and Claude 3.5 Sonnet with native-speaker review; instructions by professional native-speaker translators."
metric: "Loose Score: the average of the graded scores over all 1,667 instructions. Strict Score: the same, with any score below 1 counted as 0."
format: "Open-ended answers to a basic question carrying one to three instructions, in each of 23 languages; the paper also reports cross-lingual experiments."
limitations: "The authors note that rule-based evaluation may not cover all possible outputs and model-based evaluation may not fully match human judgement. Loose and Strict Scores differ, so the variant must be stated."
citation: "https://arxiv.org/abs/2506.01776"
worked_examples:
  - task_type: "Verifiable instruction following — output language (Korean)"
    input: "다음 질문에 정확히 세 개의 불릿으로, 한국어로만 답하세요: 재생 에너지의 장점은 무엇입니까? (Answer in exactly three bullet points, in Korean only: what are the advantages of renewable energy?)"
    success:
      answer: "Three bullet points, all written in Korean, listing valid advantages."
      why: "Every verifiable constraint — bullet count and output language — is satisfied, and the content is correct."
    failure:
      answer: "A fluent, correct answer written as three bullets — but in English."
      why: "Violates the 'respond in Korean only' output-language constraint; the content is right, but a verifiable constraint is broken — exactly the failure MaXIFE checks."
    checks: "Obedience to mechanically-verifiable constraints (output language, bullet count), not just answer correctness."
    scoring: "Instruction-following accuracy — the fraction of constraints the response satisfies."
  - task_type: "Verifiable instruction following — sentence count (Spanish)"
    input: "Responde en exactamente cinco oraciones y sin usar ningún número: ¿cuáles son los beneficios del ejercicio? (Answer in exactly five sentences and without using any digits: what are the benefits of exercise?)"
    success:
      answer: "Exactly five sentences, no digits, each naming a valid benefit of exercise."
      why: "Both verifiable constraints — the exact sentence count and the no-numerals rule — are satisfied."
    failure:
      answer: "A correct, fluent answer, but written in six sentences."
      why: "Violates the exactly-five-sentence constraint; the content is right but a mechanically-checkable rule is broken, so the constraint scores as failed."
    checks: "Honouring an exact-count formatting constraint in a high-resource language."
    scoring: "Instruction-following accuracy — the fraction of constraints the response satisfies."
  - task_type: "Verifiable instruction following — output format (Japanese)"
    input: "次の質問にJSON形式で、キー \"pros\" と \"cons\" を用いて答えてください：リモートワークの利点と欠点は？ (Answer in JSON, using the keys \"pros\" and \"cons\": what are the pros and cons of remote work?)"
    success:
      answer: "A valid JSON object with \"pros\" and \"cons\" keys, each holding the relevant points."
      why: "The output-format constraint — valid JSON with the two named keys — is satisfied and parses cleanly."
    failure:
      answer: "A well-written answer in plain prose paragraphs listing the pros and cons."
      why: "Ignores the JSON-format constraint; a rule-based parser cannot load it, so the format constraint scores as violated despite correct content."
    checks: "Producing a machine-parseable format on demand, not just correct prose."
    scoring: "Instruction-following accuracy — the fraction of constraints the response satisfies."
  - task_type: "Cross-lingual instruction following — English instruction, Arabic answer"
    input: "Answer the following question in Arabic only (الإجابة بالعربية فقط): What is the capital of France?"
    success:
      answer: "باريس هي عاصمة فرنسا. (Paris is the capital of France.) — written entirely in Arabic."
      why: "The cross-lingual output-language constraint (answer in Arabic) is met even though the instruction itself was in English."
    failure:
      answer: "Paris is the capital of France. — a correct answer, but written in English."
      why: "Violates the 'answer in Arabic only' constraint; obeying an instruction whose language differs from the required output language is exactly the cross-lingual failure MaXIFE targets."
    checks: "Following a language constraint across a language boundary between instruction and response."
    scoring: "Instruction-following accuracy — the fraction of constraints the response satisfies."
  - task_type: "Verifiable instruction following — forbidden word (French)"
    input: "Explique l'utilité du recyclage sans jamais utiliser le mot « important ». (Explain the usefulness of recycling without ever using the word 'important'.)"
    success:
      answer: "A fluent explanation that conveys significance with words like « essentiel » or « crucial », never « important »."
      why: "Respects the keyword-exclusion constraint while still answering correctly."
    failure:
      answer: "A correct explanation that uses the word « important » twice."
      why: "Breaks the forbidden-word constraint; a rule-based scanner flags the banned word, so the constraint scores as violated even though the content is right."
    checks: "Obeying an exclusion (banned-word) constraint while keeping the answer correct."
    scoring: "Instruction-following accuracy — the fraction of constraints the response satisfies."
tags: [multilingual, instruction-following, cross-lingual, generation]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2506.01776", title: "MaXIFE: Multilingual and Cross-lingual Instruction Following Evaluation (Liu et al., ACL 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2506.01776", title: "MaXIFE, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://aclanthology.org/2025.acl-long.698/", title: "MaXIFE — ACL Anthology (2025.acl-long.698)", trust: 0.95 }
related: [benchmark/ifeval, benchmark/mmmlu, benchmark/polymath]
---

## What It Tests
Whether a model **follows verifiable instructions in 23 languages**.

- **47 instructions** in 11 categories, among them format, repeat, keywords, marks, citation, length and emoji.
- **Parallel data** — the same 795 questions and 47 instructions in every language.

## How It Scores
Each instruction gets a graded score from rule-based or model-based evaluation.

- **Loose Score** — the average over all 1,667 instructions.
- **Strict Score** — any score below 1 counts as 0.

## Format
A basic question plus one to three instructions; the model writes an open-ended answer. 795 items per language, 18,285 in total.

## Limitations
- **Checker coverage.** Rules may not cover every output; model-based scoring may not match human judgement, as the authors note.
- **Two scores.** State Loose or Strict with any number.

## Example
From the paper: a basic question such as "Can you help me come up with ideas for a birthday surprise for my mom?" is paired with a templated instruction such as "In the response, the word or phrase 'Computer' should appear at least 6 times." A rule-based checker counts the occurrences in the answer.
