---
id: standard/mitre-atlas
type: standard
slug: mitre-atlas
schema_version: 4.0
title: MITRE ATLAS
issuer: The MITRE Corporation
kind: framework
url: https://atlas.mitre.org/
applies_to: AI-enabled systems, and the adversary tactics and techniques that target or use them.
tags:
- security
- threat-modeling
- adversarial-ml
- red-teaming
- mitre
updated: '2026-11-21'
sources:
- url: https://atlas.mitre.org/
  title: MITRE ATLAS
  trust: 0.95
related:
- standard/owasp-top-10-for-llm-applications
---

## What It Is

A **living knowledge base of adversary tactics and techniques involving AI**: attacks against AI-enabled systems, abuse or manipulation of AI capabilities, and harmful autonomous behaviour enabled by AI.
It draws on observed real-world attacks and on demonstrations by AI red teams and security groups. Some tactics and techniques are adapted from MITRE ATT&CK.

## Structure

The site lists **16 tactics, 221 techniques, 44 mitigations and 77 case studies**.
Its matrix can be filtered by Predictive AI, Generative AI, Agentic AI and Enterprise. The tactics, in matrix order:

- Reconnaissance
- Resource Development
- AI Attack Adaptation
- Initial Access
- AI Model Access
- Execution
- Persistence
- Privilege Escalation
- Defense Evasion
- Credential Access
- Discovery
- Lateral Movement
- Collection
- Command and Control
- Exfiltration
- Impact

## Why It Matters For LLM Applications

The matrix has **Generative AI** and **Agentic AI** filters, so attacks on LLM applications and agents can be described with the same tactics and techniques.
Its case studies come from real attacks and red-team demonstrations.
