---
title: "Desktop Applications: Yes, We Still Exist in the Era of AI!!!"
date: 2025-08-05
draft: false
description: "Why desktop application security still matters as AI becomes part of traditional software."
summary: "A reflection on desktop security in the AI era, covering prompt injection, unsafe file parsing, fuzzing, and the need to revisit established security assumptions."
conference: "BSides Las Vegas 2025"
location: "Las Vegas, Nevada, USA"
venue: "Tuscany Suites & Casino — Firenze"
presentation_date: "August 5, 2025"
presentation_time: "10:30 AM – 10:55 AM PDT"
event_url: "https://archive.bsideslv.org/2025/talks"
tags:
  - Desktop Security
  - AI Security
  - Fuzzing
  - Application Security
---

When conversations turn to modern application security, the focus is often on cloud infrastructure, APIs, and increasingly, AI agents.

But there is an entire category of software that rarely receives the same attention: desktop applications.

At **BSides Las Vegas 2025**, I presented *Desktop Applications: Yes, We Still Exist in the Era of AI!!!* to bring that conversation back into focus.

The question I wanted to explore was straightforward: **what happens when we introduce AI capabilities into desktop applications that already have complex security challenges?**

## Desktop applications never disappeared

Desktop software continues to power engineering, design, creative workflows, financial modeling, and other specialized environments.

These applications frequently handle complex files, integrate with operating-system resources, support extensions, and interact with local data.

Their security challenges are not new.

Memory corruption, privilege escalation, excessive filesystem permissions, unsafe parsing, and configuration weaknesses have been part of desktop application security for years.

What has changed is the functionality we're introducing into these environments.

## AI adds capability—and attack surface

AI is increasingly being integrated into traditional software through local language models, intelligent assistants, predictive interfaces, and offline inference.

These features create opportunities for productivity and automation.

They also introduce new trust boundaries.

An AI component may interpret untrusted content, generate instructions, interact with plugins, or produce files that another component subsequently processes.

This creates an important intersection between AI-specific threats and conventional software vulnerabilities.

**AI doesn't replace old bugs. It can compound them.**

## Demonstrating the intersection

One part of the presentation explored prompt injection in AI-enabled applications.

When an application uses a model to interpret content or instructions, an attacker may attempt to influence the model through data that should not carry authority.

The security concern extends beyond an incorrect model response. It becomes more significant when the model can influence application behavior or interact with privileged capabilities.

The second demonstration focused on file-format parsing and memory corruption.

Desktop applications often process complex, externally supplied files. Introducing AI-generated content does not automatically make those parsers safer.

Malformed input can still expose weaknesses in the underlying implementation.

The lesson I wanted to emphasize was that adding an intelligent interface does not eliminate the risks in the software beneath it.

## Fuzzing still matters

Fuzzing remains one of the most practical techniques for finding vulnerabilities in desktop software.

The presentation discussed approaches and tools including libFuzzer, AFL++, and Peach Fuzzer, along with targets such as file formats, protocols, and plugin interfaces.

AI-enabled workflows can expand the kinds of inputs that reach these components.

That makes it important to revisit existing fuzzing strategies rather than assume that previously tested interfaces remain adequately covered.

The objective is not simply to test the AI model. It is to test the complete path from untrusted input to application behavior.

## Threat modeling across old and new boundaries

Traditional desktop threat models often focus on local privileges, files, processes, plugins, and operating-system interfaces.

AI introduces additional considerations: model inputs, generated actions, inference behavior, model updates, and interactions between AI components and legacy modules.

These concerns need to be evaluated together.

An AI feature may be secure in isolation while exposing an unsafe operation elsewhere in the application.

Security engineering must account for the system as a whole.

## What I wanted attendees to take away

My message at BSidesLV was that desktop security is not obsolete.

It is evolving.

We should continue investing in established practices—fuzzing, secure parsing, least privilege, threat modeling, and secure development—while extending those practices to AI-enabled workflows.

For me, the most important takeaway is simple:

**AI isn't secure if it's built on unsafe foundations.**

The arrival of AI makes desktop application security more relevant, not less.

---

Thank you to the BSides Las Vegas community for the opportunity to share this perspective and contribute to the discussion.

*The views expressed here are my own and do not necessarily represent those of my employer.*
