---
id: benchmark/multichallenge
type: benchmark
slug: multichallenge
schema_version: 1.0
title: "MultiChallenge"
name: MultiChallenge
category: Instruction Following & Chat
description: "A realistic multi-turn conversation benchmark of 273 dialogues where frontier models still score under 50%, testing memory and consistency across turns."
what_is_it: "A Scale AI benchmark of 273 human-written multi-turn conversations (about 5 turns each) that stress four hard-to-fake skills — remembering earlier instructions, inferring user facts, editing content across versions, and staying self-consistent — scored by an LLM judge against per-item rubrics."
why_people_use_it: "Older multi-turn tests like MT-Bench are saturated, but MultiChallenge is not — even top frontier models stay below 50%, so it remains a discriminating measure of real conversational competence."
examples:
  - "Turn 1 the user mentions a peanut allergy; several turns later they ask for a cookie recipe. A passing model proactively avoids peanuts without being reminded — the 'Inference Memory' challenge."
method: "273 multi-turn conversations across four challenge categories; an LLM judge answers instance-level binary yes/no rubric questions to decide whether the final response passes, reaching ~93% agreement with human raters."
metric: "Accuracy (% of conversations the model passes, per the rubric judgment)."
format: "Multi-turn dialogue (avg. ~5 turns); LLM-as-judge with human-written instance-level rubrics."
saturation: low
limitations: "It relies on an LLM judge, so judge choice and rubric wording affect scores. At 273 conversations it is modest in size, English-only, and the four hand-designed categories, while realistic, do not exhaust the space of multi-turn failures."
citation: "https://arxiv.org/abs/2501.17399"
worked_examples:
  - task_type: "Inference Memory of user information"
    input: "Turn 1 — user, mid-conversation about travel: 'I have to be careful, I've got a serious peanut allergy.' ... Turn 4 — user, on a new thread: 'Can you give me a quick cookie recipe for tonight?'"
    success:
      answer: "Here's a quick oat-and-chocolate-chip cookie recipe — I've kept it completely peanut-free given your allergy: [recipe with no peanuts or peanut butter]."
      why: "The model retained the allergy stated four turns earlier and proactively avoided peanuts without being reminded."
    failure:
      answer: "Here's a fast favorite: classic peanut-butter cookies — just flour, sugar, an egg, and a cup of peanut butter. Bake 10 minutes."
      why: "✓ correct format, ✓ a genuinely good recipe, ✗ suggests peanut butter — the model dropped the peanut-allergy fact stated in Turn 1, failing the cross-turn Inference Memory constraint."
    checks: "Whether a model retains and reuses earlier user-stated facts across turns instead of answering each turn in isolation."
    scoring: "Accuracy — fraction of conversations the final response passes, judged by an LLM against per-item yes/no rubrics."
  - task_type: "Instruction Retention across turns"
    input: "Turn 1 — user: 'For everything in this chat, always give distances in kilometers, never miles.' ... Turn 5 — user: 'How far is it to drive from Los Angeles to San Francisco?'"
    success:
      answer: "It's roughly 615 km by the main highway route, usually about six hours of driving."
      why: "The model still honors the turn-1 unit instruction five turns later, reporting kilometers without a reminder."
    failure:
      answer: "It's roughly 380 miles by the main highway route, usually about six hours of driving."
      why: "✓ accurate distance, ✓ helpful detail, ✗ answers in miles — the model dropped the standing 'always use kilometers' instruction from Turn 1, failing Instruction Retention."
    checks: "Whether a model keeps applying a standing formatting instruction across later, unrelated turns."
    scoring: "Accuracy — the final response is judged against a rubric asking whether the earlier instruction still holds."
  - task_type: "Reliable versioned editing"
    input: "Turns 1-3 — user iterates on a cover-letter opening, ending with version 3 that adds a sentence about volunteer work. Turn 4 — user: 'Actually, go back to version 2 but keep the more formal greeting from version 3.'"
    success:
      answer: "[Version 2 body, unchanged, with the volunteer-work sentence removed] opening with 'Dear Hiring Committee,' — the formal greeting introduced in version 3."
      why: "The model reconstructs version 2 exactly, drops the version-3 volunteer sentence, and carries over only the requested formal greeting."
    failure:
      answer: "[Version 2 body] but still including the volunteer-work sentence, opening with 'Dear Hiring Committee,'."
      why: "✓ restored the formal greeting, ✓ fluent letter, ✗ it left in the volunteer-work sentence that only existed in version 3 — the model lost track of what version 2 actually contained, failing Reliable Versioned Editing."
    checks: "Whether a model tracks the exact state of content across multiple revisions, including a selective revert."
    scoring: "Accuracy — the rubric checks the final draft matches the specified merge of versions."
  - task_type: "Self-coherence"
    input: "Turn 2 — assistant stated: 'The novel Dune was published in 1965.' ... Turn 6 — user: 'Remind me, what year did Dune come out, and was that before or after the moon landing?'"
    success:
      answer: "Dune was published in 1965, which was before the July 1969 moon landing."
      why: "The model repeats the same 1965 date it gave earlier and reasons consistently from it — no self-contradiction."
    failure:
      answer: "Dune came out in 1963, so it was published a few years before the moon landing in 1969."
      why: "✓ correct ordering relative to the moon landing, ✗ it now says 1963 after asserting 1965 in Turn 2 — the model contradicts its own earlier statement, failing Self-Coherence."
    checks: "Whether a model stays consistent with facts it itself asserted earlier in the conversation."
    scoring: "Accuracy — the rubric asks whether the final answer contradicts the model's own prior turn."
  - task_type: "Instruction Retention (format constraint)"
    input: "Turn 1 — user: 'Please answer me in plain prose for this whole conversation — no bullet points or numbered lists.' ... Turn 4 — user: 'What are the main causes of the French Revolution?'"
    success:
      answer: "The French Revolution grew from several intertwined pressures. Deep fiscal crisis and unfair taxation strained ordinary people, while Enlightenment ideas questioned the monarchy's legitimacy, and a series of poor harvests pushed bread prices beyond reach — together tipping widespread grievance into revolt."
      why: "The model gives a substantive answer entirely in prose, honoring the turn-1 'no lists' instruction four turns later."
    failure:
      answer: "The main causes were:\n1. Fiscal crisis and heavy taxation\n2. Enlightenment ideas\n3. Food shortages and rising bread prices\n4. An inflexible monarchy"
      why: "✓ accurate causes, ✓ clearly organized, ✗ it uses a numbered list — the model dropped the standing 'plain prose, no lists' instruction from Turn 1, failing Instruction Retention."
    checks: "Whether a model keeps obeying an early formatting instruction when a later question naturally invites a list."
    scoring: "Accuracy — the rubric checks whether the final response still respects the earlier format constraint."
tags: [instruction-following, multi-turn, conversation, chat, llm-judge]
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2501.17399", title: "MultiChallenge: A Realistic Multi-Turn Conversation Evaluation Benchmark Challenging to Frontier LLMs (Sirdeshmukh et al., 2025)", trust: 0.95 }
  - { url: "https://scale.com/research/multichallenge", title: "MultiChallenge (Scale AI research page)", trust: 0.9 }
  - { url: "https://labs.scale.com/leaderboard/multichallenge", title: "MultiChallenge leaderboard (Scale Labs)", trust: 0.9 }
related: [benchmark/ifbench, benchmark/ifeval]
---

## What It Tests
Whether a model holds a **conversation** together over many turns, not just answers one prompt well.
It targets four skills that require instruction-following, context allocation, and in-context
reasoning simultaneously:

- **Instruction Retention** — keep obeying an instruction set early, even when later turns tempt a drift.
- **Inference Memory of User Information** — infer and reuse facts the user stated earlier (a stated allergy, a preference).
- **Reliable Versioned Editing** — track content state across revisions, including reverts and conflicting edits.
- **Self-Coherence** — avoid contradicting the model's own earlier statements.

## How It Scores
Accuracy = the fraction of the 273 conversations the model passes. Judgment is automated by an
**LLM-as-judge** answering human-written, instance-level **binary yes/no rubric questions** about the
final response.

That rubric design is what makes the judge reliable: it reaches ~93% agreement with experienced human
raters, versus ~37% for a naive judge without instance rubrics.

## Format
Each item is a realistic multi-turn dialogue averaging about five turns and ~1,200 words, written by
humans. The conversations are distributed across the four categories (Inference Memory is the largest;
Reliable Version Editing the smallest). Only the model's final turn is scored against the rubric.

## Limitations
Being LLM-judged, results depend on the judge model and rubric phrasing, so the harness must be pinned
for comparability. At 273 conversations it is modestly sized and English-only. The four categories are
realistic but hand-designed, so they capture common multi-turn failures rather than the full space —
and unlike IFBench, passing is judged, not programmatically verified.

## Example
Early in a chat the user mentions a peanut allergy while discussing travel. Several turns later, on an
unrelated thread, they ask for a quick cookie recipe. A model that passes the **Inference Memory**
challenge proactively omits peanuts and peanut butter without any reminder; a model that fails happily
suggests a peanut-butter cookie — technically a good recipe, but a conversational failure the rubric
catches.
