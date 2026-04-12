---
title: "When Your Security Tool Goes Down: Surviving a the SCA tool Outage"
date: 2026-04-12
draft: false
description: "the SCA tool went down for 5 hours and took our entire development workflow with it. Here's the decision we made, why we made it, and what we learned."
tags: ["Security", "DevOps", "CI/CD", "the SCA tool", "Incident Response", "Supply Chain"]
categories: ["Engineering"]
---

Security tooling exists to protect your organization. But what happens when the security tool itself becomes the outage?

That's the situation we found ourselves in when the SCA tool experienced a service disruption that lasted approximately five hours. For us, it wasn't a degraded experience — it was a complete development freeze.

<!--more-->

## How We Use the SCA tool

the SCA tool is integrated into our standard CI/CD pipeline as a mandatory gate on every pull request. It handles three things: static code analysis for insecure patterns, dependency scanning for known vulnerabilities and licensing issues, and container image scanning. These checks are enforced as required status checks in our Git platform — a PR cannot merge until they pass.

This isn't optional tooling. It's business compliance and security enforcement baked into the development workflow. Every repository is enrolled, and historical analysis is maintained for audit purposes.

## How We Found Out

We didn't find out because a developer filed a ticket. We found out because our backend monitoring started firing — communication to the SCA tool's API had failed, and failed the SCA tool status checks were spiking across the board. Developers hit it almost simultaneously as they tried to merge work during normal business hours.

Within minutes, it was clear this wasn't a flaky test or a misconfigured repo. the SCA tool was down.

## The Catch-22

Forty minutes into the outage, we had a decision to make.

The technical situation was straightforward: the SCA tool's service was degraded, our required status checks couldn't complete, and no pull requests could merge. The business situation was also straightforward: development had stopped entirely. Not slowed — stopped.

The harder question was what to do about it.

Keeping the gates up was the safer security posture, but it meant accepting a full development freeze of unknown duration. Bypassing them meant restoring velocity, but it also meant any code merged during that window had skipped mandatory security scanning. In a regulated environment, that's not a decision one person makes unilaterally.

We looped in business leadership and the security team. The joint decision — made with full awareness of the tradeoff — was to temporarily remove the SCA tool as a required status check and allow teams to continue operating. The outage resolved approximately five hours after it started. We had introduced the bypass about forty minutes in, so roughly four and a half hours of merges occurred without the SCA tool gating.

## Closing the Gap

When the SCA tool came back online, we didn't just re-enable the gate and move on. We pulled the full list of PRs merged during the bypass window — something we were able to do quickly because we maintain pipeline metrics and can generate reports against them — and enforced a retroactive scan of every affected repository.

Nothing was found. The window was short enough that the exposure was limited. But "nothing was found this time" is not a risk management strategy.

## What the Retrospective Surfaced

During our retrospective, the conversation shifted from "what do we do next time this specific thing happens" to a broader question: how do we design our security posture to be more resilient to SaaS tool availability?

Two themes emerged:

**Granular bypass controls** — When the SCA tool went down, our only option was a binary one: all the SCA tool checks required, or none. We're investigating whether we can build more surgical controls — for example, temporarily disabling only the dependency scanning component while keeping code analysis active, or allowing bypasses scoped to specific repo classifications. The goal is to have options that don't require choosing between full security and full productivity.

**Tooling redundancy** — Most organizations don't self-host their security tooling, and we're no exception. That means our security posture has an availability dependency on third-party SaaS providers. One tool going down shouldn't mean zero coverage. We're evaluating whether distributing our scanning responsibilities across multiple tools provides meaningful resilience — not just redundancy for its own sake, but genuine defense-in-depth that can survive a single vendor outage.

## The Uncomfortable Reality

The axios supply chain incident and this the SCA tool outage happened within the same operational period. Back to back, they illustrate the same underlying tension: we build security controls into our pipelines, and then we discover that those controls can themselves become the risk.

A cached malicious package is a supply chain problem. A security tool outage blocking all development is a single point of failure problem. Both are worth taking seriously — and both require thinking beyond "add more tools" toward how those tools behave when something goes wrong.

The goal isn't to have more security gates. It's to have security controls that are resilient, observable, and designed with failure modes in mind.

---

*This post is part of an ongoing series on real-world engineering decisions — not the clean version, but how things actually go.*
