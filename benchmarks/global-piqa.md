---
id: benchmark/global-piqa
type: benchmark
slug: global-piqa
schema_version: 1.0
title: "Global PIQA"
name: Global PIQA
category: Multilingual
description: "Hand-built physical commonsense reasoning across 100+ languages and cultures."
what_is_it: "Global PIQA is a physical-commonsense reasoning benchmark hand-written by 335 researchers from 65 countries, covering 116 language varieties — a multilingual, culturally-grounded successor to the English PIQA (Physical Interaction Question Answering) test."
why_people_use_it: "It tests everyday physical know-how, not expert knowledge, in over 100 languages including many low-resource ones — and because over half the questions reference local foods, customs, and traditions, it exposes cultural blind spots that translated benchmarks hide."
examples:
  - "Given a goal like preparing a local dish, choose which of two everyday physical actions correctly achieves it — with the objects and customs drawn from the question's own culture."
method: "Two-choice selection: given a goal, pick the correct physical solution over a plausible distractor; scored across 116 language varieties, in both a parallel split and a non-parallel culturally-specific split."
metric: "Accuracy (% correct); random baseline is 50%."
format: "2-way multiple-choice (goal + two candidate solutions); 0-shot."
saturation: medium
limitations: "Because it is two-choice, random guessing already scores 50%, so absolute numbers look high; the signal is the *gap* between high- and low-resource languages. Hand authoring by volunteers means per-language set sizes and difficulty vary, and coverage, while broad, is still uneven across the 116 varieties."
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
updated: 2026-08-02
sources:
  - { url: "https://arxiv.org/abs/2510.24081", title: "Global PIQA: Evaluating Physical Commonsense Reasoning Across 100+ Languages and Cultures (2025)", trust: 0.95 }
  - { url: "https://github.com/mrlbenchmarks/global-piqa", title: "mrlbenchmarks/global-piqa — official repository", trust: 0.9 }
related: [benchmark/hellaswag, benchmark/winogrande, benchmark/include]
---

## What It Tests
Everyday physical commonsense — knowing which of two mundane actions actually accomplishes a goal
(how to open, carry, cook, or fix ordinary things) — across 116 language varieties spanning five
continents, 14 language families, and 23 writing systems. It is the multilingual, culturally-aware
extension of the English PIQA (Physical Interaction Question Answering) benchmark.

## How It Scores
Accuracy against a 50% random baseline, since each item offers two candidate solutions. The
headline is the resource gap, not the average: the best model reaches only 80.2% on Sub-Saharan
African languages versus 95.6% on Western European ones — a 15-point gap — and up to 37 points
separate the strongest and weakest languages.

## Format
Each item gives a goal and two physical solutions, one correct; the model picks one, 0-shot. The
benchmark was built by hand by 335 researchers from 65 countries rather than machine-translated,
and ships a **parallel** split plus a **non-parallel** split where over 50% of items reference
local foods, customs, or traditions specific to the language's culture.

## Limitations
The two-choice format means 50% is free, so raw scores look inflated and only cross-language gaps
are informative. Volunteer hand-authoring gives uneven set sizes and difficulty across languages,
and although 100-plus varieties is exceptional coverage, it remains uneven — some varieties have
far fewer items than others.

## Example
A non-parallel item might state a goal tied to preparing a regional dish and offer two everyday
physical actions, only one of which works given local ingredients and utensils. A model trained
mostly on English web text may know the abstract physics yet miss the culturally-specific setup —
which is precisely the failure Global PIQA is designed to surface.
