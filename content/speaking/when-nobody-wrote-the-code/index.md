---
title: "When Nobody Wrote the Code: Engineering Lessons from Building AI-Native Application Security"
date: 2026-11-06
draft: false
description: "What happens when AI agents generate, review, and fix software faster than we can verify it?"
summary: "Reflections from OWASP Global AppSec USA 2026 on AI-native application security, agent reliability, verification, and the engineering patterns needed to build trust."
conference: "OWASP Global AppSec USA 2026"
location: "San Francisco, California, USA"
presentation_date: "November 6, 2026"
venue: "Grand Ballroom B, Street Level"
presentation_time: "2:15 PM – 3:00 PM PST"
tags:
  - AI Security
  - Application Security
  - Agentic AI
  - Security Engineering
  - OWASP
---

Earlier this month, I presented at **OWASP Global AppSec USA 2026 in San Francisco**. My talk, *When Nobody Wrote the Code: Engineering Lessons from Building AI-Native Application Security*, explored what happens when AI agents increasingly generate, review, and fix software—and what it takes to trust the outcome.

This is a written reflection on the ideas behind the presentation, the engineering challenges we encountered, and the lessons that shaped how I think about the future of application security.

## A pull request with no author

Picture a pull request where a scanner found the issue and a model wrote the fix. A second model reviewed it, and a third generated the tests. The description says "Fixed with high confidence."

Then it lands in front of one human, whose job is to approve it.

That raises the question at the center of this discussion:

**When nobody wrote the code, who is the author of record?**

## The assumption that quietly broke

Every application security program runs on a few assumptions we rarely say out loud.

Writing software is expensive. Earlier fixes cost less. And perhaps most importantly, verifying a change is cheaper than producing it.

For a long time, those assumptions held reasonably well. A reviewer could read in an hour what took an engineer a week to write, and review capacity roughly kept pace with code volume.

AI changes that equation.

Producing code and fixes is becoming fast and inexpensive, but knowing whether a change is safe isn't getting proportionally cheaper.

Every generated change still needs answers:

- Did it do what we intended?
- Did it introduce unintended behavior?
- Did it actually eliminate the vulnerability?
- Where did the change come from?
- What evidence supports the decision to accept it?

The bottleneck is moving from writing code to verifying it.

And most application security programs weren't designed for that shift.

## What we learned from three experiments

As we experimented with AI-native application security workflows, three challenges stood out.

### 1. AI-generated remediation creates review debt

One of the most compelling applications of AI in AppSec is automated remediation.

An agent identifies a vulnerability, understands the surrounding code, proposes a fix, and potentially opens a pull request.

The speed is impressive.

But generating a fix is not the same as accepting a fix.

Every proposed change still requires some level of validation. The remediation may address the reported vulnerability while introducing regressions, overlooking related attack paths, or misunderstanding the application's security assumptions.

As remediation becomes faster, the volume of changes requiring verification can increase.

We risk moving the vulnerability backlog into a review backlog.

**The queue moves. The risk doesn't automatically disappear.**

### 2. AI reviewing AI produces agreement, not proof

A natural response is to introduce another agent.

If one agent generates a fix, why not have a second agent review it?

We explored multi-agent workflows where different agents contribute to vulnerability discovery, analysis, and remediation.

The challenge is that agreement between agents does not necessarily establish correctness.

They may share similar assumptions, reasoning patterns, or incomplete context.

This concern extends beyond security engineering. A [2025 research paper examining correlated errors among language models](https://arxiv.org/abs/2506.07962) highlights how models can produce similar incorrect answers.

In security, this distinction matters enormously.

A remediation agent may confidently propose a fix. A reviewing agent may confidently approve it. Yet both may have overlooked the same authorization requirement or an alternative exploitation path.

**Agreement is a signal. It isn't proof.**

### 3. Multiple AI workflows can collide

Consider an environment where different agents are independently responsible for security remediation, refactoring, and test generation.

Each workflow may appear correct when evaluated in isolation.

But software systems are not collections of isolated changes.

A security fix may depend on assumptions that a subsequent refactor changes. Generated tests may validate implementation details without protecting the underlying security requirement.

The resulting failure may not exist in any single change.

It can emerge from the interaction between otherwise reasonable changes.

This is why local correctness cannot be the only measure of success for autonomous software engineering.

## What proof actually looks like

If model confidence and agent agreement aren't sufficient, what should we rely on?

My answer is increasingly centered on **executable evidence**.

In software testing, an *oracle* is a mechanism for determining whether a system's behavior is correct.

For security remediation, that means establishing an independently verifiable check that demonstrates the vulnerability before the fix and validates the expected security behavior afterward.

The order matters.

First, reproduce the vulnerability.

Then establish a test that fails on the vulnerable implementation.

Only after that should an agent attempt remediation.

If a supposed security regression test passes on the vulnerable implementation, it hasn't demonstrated that it can detect the original security failure.

The fix must then earn its passing result.

And ideally, verification should extend beyond the exact payload or execution path that originally exposed the issue.

A fix may block one exploit while leaving related variants accessible through a different endpoint, object reference, or input representation.

**"Fixed with high confidence" is a claim. A passing security regression test is evidence.**

Even that evidence has limits, but it gives us something concrete to evaluate and improve.

## Shift Left isn't dead. It's checking the wrong unit.

Traditional application security programs rely on established practices:

- Threat modeling during design
- Static analysis during development
- Security checks in CI/CD pipelines
- Dynamic testing before release
- Human review of consequential changes

These practices remain valuable.

The problem is that AI-generated changes can introduce new design decisions without passing through an explicit design phase.

Imagine a service where tenant isolation is enforced through a particular authorization boundary.

An agent introduces a bulk export endpoint while addressing a performance requirement.

The implementation looks reasonable, but it accidentally bypasses the expected tenant restriction.

Static analysis may not recognize the missing authorization requirement. A reviewer may see an otherwise clean diff. Dynamic testing may exercise only a single tenant.

Each security control can operate as designed while the application still violates an essential security property.

This is especially important for authorization vulnerabilities, where correctness depends on application-specific rules rather than recognizable vulnerable code patterns.

The [OWASP Top 10](https://owasp.org/Top10/) and [OWASP API Security Top 10](https://owasp.org/API-Security/) both emphasize the importance of broken access control and authorization failures.

My takeaway is not that we should abandon existing security gates.

We should extend them with verification of the **security invariants** those gates are intended to protect.

An invariant is a rule that must remain true regardless of how the implementation evolves.

For example:

> A user must never be able to access resources belonging to another tenant without explicit authorization.

That requirement should remain enforceable whether the implementation is written by a developer, generated by an agent, or modified through a series of automated changes.

The security objective stays constant even when the code changes.

## Seven engineering patterns for AI-native application security

The broader lesson from these experiments is that trustworthy AI-native AppSec requires engineering patterns, not simply more capable models.

I organized the ideas into seven patterns.

### 1. Evidence over Confidence

AI-generated findings and fixes should be supported by independently evaluable evidence.

A confidence score can help prioritize investigation, but it should not substitute for proof that the intended security property holds.

### 2. Verification before Remediation

Establish how to reproduce and verify a vulnerability before attempting to fix it.

This creates a baseline against which remediation can be evaluated.

It also reduces the risk of accepting a plausible-looking change that doesn't resolve the original issue.

### 3. Intent over Implementation

Security review should focus on the properties that must remain true, not solely on the lines of code that changed.

As implementation becomes increasingly automated, preserving architectural intent and security invariants becomes more important.

### 4. Human Approval over Human Review

Humans cannot meaningfully inspect unlimited volumes of generated code.

We need to make human involvement more effective by presenting the relevant security context, verification evidence, residual uncertainty, and consequences of accepting a change.

The goal is not to remove human accountability.

It is to give humans better evidence for making decisions.

### 5. Security Memory over Repeated Analysis

Security agents need durable context.

Findings, assumptions, architectural decisions, previous verification results, and known limitations should survive individual executions.

Without that continuity, agents repeatedly rediscover information, lose context, or make decisions without understanding previous security conclusions.

### 6. Provenance as Security Evidence

We should be able to reconstruct how an automated security decision was reached.

That includes what triggered the analysis, what evidence was considered, which changes were proposed, what verification was performed, and who or what approved the outcome.

If a decision cannot be reconstructed, it becomes difficult to audit, investigate, or govern.

### 7. Governance through Measurable Trust

Trust should not be represented by a single model-generated score.

Instead, it should emerge from measurable criteria appropriate to the action being performed.

For example, automatically suggesting a fix and automatically merging a security-sensitive change should require different levels of assurance.

The greater the potential consequence, the stronger the evidence and approval requirements should be.

## Can an agent write the verification oracle?

This is an important question.

If humans must manually write every security verification test, the approach may struggle to scale alongside AI-generated code.

Agents can help generate verification tests, but the design needs safeguards against self-validation.

One approach is to separate the responsibilities of generating a fix and generating the checks that evaluate it.

The verification logic should be derived from the security requirement rather than simply mirroring the proposed implementation.

It should demonstrate failure on the vulnerable version before passing on the corrected version.

And the remediation agent should not be able to silently weaken the checks used to evaluate its own work.

Additional tests can be withheld from the fixing agent to help identify solutions that only satisfy the visible test cases.

This changes where human expertise is applied.

Instead of reviewing hundreds of lines of generated implementation code without sufficient context, a security engineer can focus on the security requirement, the verification strategy, and the evidence supporting the outcome.

That is a much more promising use of human judgment.

## Where I think teams should start

Adopting these ideas doesn't necessarily require rebuilding an entire application security program.

I would start with three practical steps.

**This week: Establish one executable security oracle.**

Choose a security finding with a proposed automated remediation.

Create a verification check that reproduces the vulnerability and fails on the original implementation.

Then evaluate whether the fix actually satisfies that check.

**Next month: Preserve remediation evidence.**

Capture the original finding, proposed change, verification results, relevant context, and approval decision.

Even a structured evidence file attached to a pull request is a useful starting point.

**Next quarter: Define assurance requirements for automated actions.**

Determine which activities can be automated with minimal oversight and which require stronger evidence or explicit human approval.

Base those decisions on consequence, uncertainty, and verification quality rather than model confidence alone.

## The author of record

Let's return to the question that started this discussion.

When nobody wrote the code, who is the author of record?

It isn't the model. A model doesn't carry organizational accountability.

And simply assigning responsibility to the last person who clicked Approve doesn't solve the underlying verification problem.

Accountability must remain with the people and organizations governing the system, supported by evidence that makes their decisions meaningful.

The larger shift is from trusting how confidently software was produced to establishing why its behavior should be trusted.

**AI can make code abundant. Trust remains engineered.**

When nobody wrote the code, evidence becomes the foundation of trust.

---

Thank you to OWASP and everyone who attended, asked thoughtful questions, and continued the conversation afterward.

I'm particularly interested in how other security engineering teams are approaching agent reliability, independent verification, and developer trust as AI becomes more deeply integrated into software development.

If you're working on similar problems, I'd love to connect and exchange ideas.

*The views expressed here are my own and do not necessarily represent those of my employer.*