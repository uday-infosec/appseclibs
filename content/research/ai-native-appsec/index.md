---
title: "Engineering Verifiable AI-Native Application Security"
date: 2026-11-14
draft: false
description: "Research into security agent reliability, vulnerability discovery, remediation, and verifiable automation."
summary: "Engineering approaches to making AI-assisted security workflows reliable, reviewable, and verifiable."
tags:
  - AI Security
  - Application Security
  - Security Agents
  - Verification
---

## Overview

AI agents are increasingly capable of discovering vulnerabilities, reviewing code, and proposing security fixes.

But capability alone does not establish trust.

This ongoing engineering research investigates how AI-native application security systems can produce results that are complete, reproducible, reviewable, and supported by evidence.

## Research Questions

1. How can security findings be preserved across multiple agent executions?
2. How can we detect incomplete analysis caused by context limitations?
3. How should conflicting agent recommendations be evaluated?
4. How can we verify that a proposed remediation actually addresses a vulnerability?
5. How can security automation integrate into developer workflows without creating excessive review overhead?

## Engineering Observations

### Finding Preservation

Multi-agent orchestration can lose findings between stages of analysis.

A reliable workflow needs durable evidence and explicit tracking of findings throughout execution.

### Context Exhaustion

Large repositories and multi-repository analysis can exceed the effective context available to an agent.

This can result in missed vulnerabilities or incomplete analysis.

### Remediation Reliability

An agent-generated fix may appear correct while leaving the underlying vulnerability unresolved or introducing regressions.

Remediation therefore requires independent validation.

### Agent Disagreement

Different agents may recommend conflicting solutions to the same security issue.

These disagreements should be preserved and evaluated rather than silently resolved.

## Engineering Directions

The following are approaches under investigation, not claims of completed validation:

- Persistent finding and evidence records
- Deterministic completeness checks
- Explicit coverage reporting
- Independent remediation verification
- Human review for consequential security decisions

## Research Status

**Ongoing engineering research**

This page documents research questions, observed failure modes, and engineering approaches. Formal evaluation results and reproducible technical artifacts will be added as they become available.

## Related Work

- [Research overview](/research/)
- [Publications](/publications/)
- [Conference presentations](/speaking/)
