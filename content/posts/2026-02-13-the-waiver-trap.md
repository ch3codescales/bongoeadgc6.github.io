---
title: "The Waiver Trap: When Your Compliance Safety Valve Delays Real Remediation"
date: 2026-02-13
draft: false
description: "I introduced temporary exemptions to relieve enforcement friction — and learned the hard way that exceptions defer problems rather than solve them. Why the waiver is the most instructive security-program decision you'll make."
tags: ["Security", "Vulnerability Management", "Compliance", "Security Program Design"]
categories: ["Security"]
---

Enforcement worked. Critical vulnerabilities were suddenly getting fixed on deadlines, and the backlog that had sat ignored for years started to clear. But it worked *too well* in one sense: the volume of new work it created across thousands of developers turned enforcement itself into a blocker.

This is the part of the story almost nobody writes about, because it's the part that looks like a failure. It was the most instructive decision I made.

<!--more-->

## The friction problem

Every enforcement program generates friction. When you add a hard deadline to a category of work developers had been deferring, you create a surge of demand. Teams that were coping by ignoring vulnerabilities suddenly had to confront all of them at once.

The result: enforcement worked, but it became very difficult to manage across an org of thousands. The friction was real, and it threatened to burn people out.

## The instinct: add a relief valve

The natural first response is to give people a temporary way around the pressure (an exemption, a waiver, a business-justified exception). I introduced exactly that: a process where a team could temporarily bypass an enforcement block, with the requirement that they commit to repairing the underlying issue later.

On paper it was clean. In practice it taught me a hard lesson.

## What the waiver actually did

The waiver relieved the *pressure*, but it also relieved the *priority*. Once a team knew they could get a temporary bypass, the urgent work quietly became the deferred work again. The exemption I designed as a safety valve became a deferral mechanism.

The uncomfortable truth: **exceptions shift problems; they rarely solve them.** Every waiver was a moment where "fix it now" became "fix it later," and "later" has a very long half-life in a busy engineering org.

## What I tell leaders designing exception processes

1. **Assume the exception will be overused.** Whatever fraction you think will use it, it will be higher. Design for that.
2. **Make exceptions expensive enough to be rare.** If bypassing is as easy as complying, you've built a path of least resistance to *not* fixing things.
3. **Every exception should have an owner and a deadline.** An open-ended waiver is a permanent vulnerability with extra paperwork.
4. **Watch what happens with the exceptions; it's your most honest signal.** The waiver rate isn't a compliance stat; it's a UX stat for your security program.

## Why this failure unlocked the real answer

The lesson wasn't "remove the waivers." It was **diagnostic**: the waivers revealed the fundamental problem with enforcement-based security. Enforcement creates urgency, but urgency alone doesn't make remediation *easy*. If the only two options are "suffer the friction" or "use a loophole," people will pick the loophole, and you can't blame them.

That's when I stopped asking *how do we enforce harder* and started asking *why is fixing this so hard in the first place?*

The answer to that question changed everything, and it's the subject of Part 3.

*This follows [Part 1 — giving the backlog teeth]({{< relref "/posts/2026-02-06-give-the-backlog-teeth" >}}).*

*Read Part 3: [the paved path — how making the secure choice the easy choice did what enforcement alone never could]({{< relref "/posts/2026-05-01-the-paved-path" >}}).*
