---
id: benchmark/global-piqa
type: benchmark
slug: global-piqa
schema_version: 1.0
title: "Global PIQA"
name: Global PIQA
category: Multilingual
description: "A participatory commonsense reasoning benchmark in the format of PIQA, built by hand for 141 language varieties."
what_is_it: "Global PIQA (Chang et al., 2025) was constructed by hand by over 350 researchers from over 65 countries. Its 141 language varieties cover five continents, 19 language families and 24 writing systems. Each item follows English PIQA (Physical Interaction: Question Answering): a goal and two solutions, one correct. Answering needs commonsense reasoning such as physical or temporal reasoning, cultural knowledge or basic world knowledge."
why_people_use_it: "It tests everyday physical know-how, not expert knowledge, in over 100 languages including many low-resource ones — and because over half the questions reference local foods, customs, and traditions, it exposes cultural blind spots that translated benchmarks hide."
examples:
  - "Given a goal like preparing a local dish, choose which of two everyday physical actions correctly achieves it — with the objects and customs drawn from the question's own culture."
method: "Each item can be run in a cloze setting (comparing completion probabilities, for pretrained-only models) or a generation setting (prompted, for instruction-tuned models). The non-parallel split has items written in each language, many culture-specific; the parallel split translates culturally agnostic items into 131 language varieties for direct comparison."
metric: "Accuracy (% correct); with two solutions, random choice scores 50%."
format: "Two-way choice (a goal and two candidate solutions). The official non-parallel split has exactly 100 examples per language for 136 language codes; every example was verified by a native speaker."
limitations: "The authors note that each language has only 100 examples. The non-parallel split is not translated, so its difficulty can vary across languages. Two choices mean 50% by chance."
citation: "https://arxiv.org/abs/2510.24081"
worked_examples:
  - task_type: "Physical commonsense (Swahili)"
    input: "Lengo (goal): Kufanya ugali uwe mnene ili uweze kukatwa vipande. (Make the ugali stiff enough to cut into pieces.) Chagua kitendo sahihi: (Choose the correct action.)"
    options:
      - "Ongeza unga wa mahindi taratibu ukikoroga. (Add maize flour gradually while stirring.)"
      - "Mimina maji zaidi ndani na uache uchemke. (Pour in more water and let it boil.)"
    success:
      answer: "A — Add maize flour gradually while stirring"
      why: "Stirring in more flour is exactly how ugali is thickened; the physical action matches the local dish."
    failure:
      answer: "B — Pour in more water and let it boil"
      why: "Adding water makes ugali runnier, not stiffer — the model reasons from a generic 'boil to reduce' default and misses the culturally-specific preparation."
    checks: "Everyday physical reasoning grounded in a local, culturally-specific setup rather than generic textbook physics."
    scoring: "Accuracy (% correct); random baseline is 50%."
  - task_type: "Physical commonsense (Hindi)"
    input: "लक्ष्य (goal): रोटी को फुलाना। (Puff up a roti flatbread.) सही क्रिया चुनें: (Choose the correct action.)"
    options:
      - "रोटी को थोड़ी देर सीधे आँच पर रखें। (Place the roti directly on the open flame for a moment.)"
      - "रोटी को पानी में डुबो दें। (Soak the roti in water.)"
    success:
      answer: "A — Place the roti directly on the open flame for a moment"
      why: "Direct heat flashes the roti's moisture to steam, which puffs it up — the standard local technique."
    failure:
      answer: "B — Soak the roti in water"
      why: "Water makes it soggy and it never puffs; a generic 'add water' default that misses the flame method."
    checks: "Culinary physical reasoning tied to a regional cooking method, not a generic action."
    scoring: "Accuracy (% correct); random baseline is 50%."
  - task_type: "Physical commonsense (Spanish)"
    input: "Objetivo (goal): Evitar que el guacamole se ponga marrón. (Keep the guacamole from turning brown.) Elige la acción correcta: (Choose the correct action.)"
    options:
      - "Agregar jugo de limón y mezclar. (Add lime/lemon juice and mix.)"
      - "Agregar azúcar y dejarlo al sol. (Add sugar and leave it in the sun.)"
    success:
      answer: "A — Add lime/lemon juice and mix"
      why: "The acid and vitamin C in citrus slow the oxidation that browns avocado — the everyday kitchen fix."
    failure:
      answer: "B — Add sugar and leave it in the sun"
      why: "Sugar does nothing for oxidation and sun/heat speeds browning — a wrong guess that misses the culinary chemistry."
    checks: "Grasping the physical cause of browning and the local remedy, not a random pantry action."
    scoring: "Accuracy (% correct); random baseline is 50%."
  - task_type: "Physical commonsense (Vietnamese)"
    input: "Mục tiêu (goal): Làm mềm bánh tráng để cuốn gỏi cuốn. (Soften rice paper to roll fresh spring rolls.) Chọn hành động đúng: (Choose the correct action.)"
    options:
      - "Nhúng nhanh bánh tráng vào nước ấm. (Dip the rice paper briefly in warm water.)"
      - "Cho bánh tráng vào dầu nóng để chiên. (Put the rice paper in hot oil to fry.)"
    success:
      answer: "A — Dip the rice paper briefly in warm water"
      why: "A quick dip hydrates and softens the rice paper so it can be rolled — the standard preparation for fresh rolls."
    failure:
      answer: "B — Put the rice paper in hot oil to fry"
      why: "Frying makes it crisp and brittle (that is for fried rolls) — the opposite of softening for fresh rolls."
    checks: "Matching the physical action to the specific dish's preparation, not a generic 'cook it' default."
    scoring: "Accuracy (% correct); random baseline is 50%."
  - task_type: "Physical commonsense (Arabic)"
    input: "الهدف (goal): تحويل اللبن الزبادي إلى لبنة. (Turn yogurt into labneh, i.e. strained yogurt.) اختر الإجراء الصحيح: (Choose the correct action.)"
    options:
      - "تصفية اللبن في قماش لإزالة الماء ومصل اللبن. (Strain the yogurt through cloth to drain off the whey/water.)"
      - "إضافة المزيد من الماء وتحريكه. (Add more water and stir.)"
    success:
      answer: "A — Strain the yogurt through cloth to drain off the whey"
      why: "Draining the whey through cloth thickens yogurt into labneh — the traditional method."
    failure:
      answer: "B — Add more water and stir"
      why: "Adding water thins it further, the opposite of thickening — a generic misread of the goal."
    checks: "Reasoning about a culturally-specific food transformation, not a generic mixing action."
    scoring: "Accuracy (% correct); random baseline is 50%."
tags: [multilingual, commonsense, reasoning, culture, physical]
updated: 2026-11-21
sources:
  - { url: "https://arxiv.org/abs/2510.24081", title: "Global PIQA: Evaluating Commonsense Reasoning Across 100+ Languages and Cultures (Chang et al., 2025)", trust: 0.95 }
  - { url: "https://arxiv.org/html/2510.24081", title: "Global PIQA, full text (arXiv HTML)", trust: 0.95 }
  - { url: "https://github.com/mrlbenchmarks/global-piqa", title: "mrlbenchmarks/global-piqa — official repository", trust: 0.9 }
related: [benchmark/hellaswag, benchmark/winogrande, benchmark/include]
---

## What It Tests
**Everyday commonsense in many languages and cultures**, in the PIQA format.

- **Kinds of reasoning** — physical, temporal, cultural knowledge, basic world knowledge.
- **Culture-specific** — in the non-parallel split, over 50% of examples reference local foods, customs, traditions or other culture-specific elements.
- **Coverage** — 141 language varieties, five continents, 19 language families, 24 writing systems.

## How It Scores
**Accuracy**: the share of items where the model picks the correct solution. Random choice gives 50%. The paper finds weaker performance in lower-resource languages, with up to a 68% accuracy gap between languages in the parallel split (2025 paper).

## Format
A goal (or sentence beginning, or question) and two solutions, one correct. Pretrained models are scored by completion probability; instruction-tuned models are prompted to choose. Splits:

- **Non-parallel** — 100 examples per language for 136 language codes, written in each language.
- **Parallel** — culturally agnostic items translated into 131 language varieties.

## Limitations
- **Small per language.** 100 examples each, as the authors note.
- **Uneven difficulty.** Non-parallel items differ by language.
- **Chance level.** Two choices: 50% by guessing.

## Example
Figure 1 of the paper shows the format: one goal with two candidate solutions. A pretrained model is scored on which solution it finds more probable; an instruction-tuned model is asked which solution is correct.
