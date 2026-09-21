---
title: "The Paved Path: Making the Secure Choice the Easy Choice at Scale"
date: 2026-05-01
draft: false
description: "Removing friction did what enforcement alone never could. The real lesson for security leaders: people choose the most convenient safe option, so make the secure path the easy path."
tags: ["Security", "DevSecOps", "Automation", "Security Program Design", "Change Management"]
categories: ["Security"]
---

*This is the final part of a series. Start with [Part 1 — giving the backlog teeth]({{< relref "/posts/2026-02-06-give-the-backlog-teeth" >}}), then [Part 2 — the waiver trap]({{< relref "/posts/2026-02-13-the-waiver-trap" >}}).*

The waiver experiment was a failure in the best possible way — it was diagnostic. It told me exactly where the real bottleneck was: **fixing a vulnerability was hard to *do*, not hard to *decide*.** Teams weren't ignoring the work because they were lazy or uncaring. The work to fix a dependency vulnerability was genuinely slow, manual, and painful. Every fix was a research project.

So I stopped trying to apply more pressure and started asking the question that changed everything:

> *What if fixing a vulnerability was the easiest possible thing to do?*

<!--more-->

## The insight: friction is a security decision

I've come to believe this is one of the most important ideas in security program design: **people choose the most convenient safe option available to them.** If doing the secure thing is harder than doing the insecure thing, you have not built a security problem — you have built a UX problem. Nobody deliberately does the wrong thing; they do the thing that's closest to hand.

This reframes the entire job of a security leader. You're not just setting policy and measuring compliance. You're designing the *paths* people will actually walk.

## Making the secure choice the easy choice

Instead of tightening the enforcement further — which would have just created more waiver demand — I invested in making the remediation itself nearly effortless. The core idea was automation that hands teams *almost-complete* work rather than *more* work:

- **Continuous, automated discovery** of new dependency versions, running across the fleet on a cadence.
- **Ready-to-review changes** — the automation opens a change with the fix already applied, so the team's job collapses from "research and implement" to "review and approve."
- **Assistance on the long tail** — teams with custom needs got guided support to configure the automation for their specific setup, rather than being told to handle it manually.

The result was that the secure action became the *default* action. Teams adopted it not because they were forced, but because it was the path of least resistance. And critically: **they kept it.** It became part of their own planning — something they carried themselves, not a mandate imposed on them.

## What changed when friction was removed

The numbers spoke for themselves:

- The remediation feedback loop went from a quarterly cycle to effectively biweekly — a step-change in responsiveness, not a marginal gain.
- Adoption of the automated path climbed steadily — and, importantly, it spread on its own merit because it was genuinely easier.
- Teams moved vulnerability management onto their own roadmaps, which is the signal I value most. When a team *chooses* to keep doing the secure thing unprompted, you've changed their culture, not just their compliance.

## The lessons for security leaders

1. **If adoption is a fight, look at your friction, not your people.** Resistance to security tooling is almost always a signal about ease-of-use, not about intent.
2. **Automation that hands people almost-finished work beats automation that adds more work.** The goal is to shrink the effort to comply, not pile on.
3. **Sustained adoption is the only metric that matters.** A rollout that reverts the moment you stop pushing isn't adoption — it's coercion.
4. **Sequence matters: make it easy before you make it mandatory.** Enforcement creates urgency, but it only works sustainably when the easy path exists first.
5. **Don't fall in love with your first design.** This whole program is the story of three iterations — enforce, waive, pave. The last one worked because the middle one taught me what to build.

## The takeaway

The most effective security programs aren't the ones with the strictest enforcement — they're the ones where **doing the right thing is the easiest thing**. When you remove the friction, secure behavior stops being a negotiation and becomes the obvious choice that engineers make on their own.

That's the real job of a security leader. Not to force compliance, but to design the conditions where compliance is the default.

## Next steps: the next-generation layer

The paved path got us to fast, sustainable remediation — but it was deliberately built with the ceiling in view. When I designed the system, I chose **not** to automate every part of it at once, specifically so I could watch how teams actually used the base loop before layering intelligence on top. That was the point of saving the major-dependency-updates automation for later.

That's the thread Part 4 picks up. The next-generation opportunity is **AI-driven remediation that complements the current-gen automation rather than replacing it — and does it at a cost that actually scales.**

The shape of the opportunity:

- The hardest remaining work isn't the routine bump — it's **major dependency upgrades**, where a version change can carry breaking changes, migration effort, and real risk. That's where teams still stall.
- The current generation of known, proven tooling already handles the bulk *cheaply* — token-efficient, low-cost workflows that cover the common cases reliably.
- The next-generation layer — AI — sits *on top of* that known-functional baseline. It's not about swapping the proven solution out; it's about using AI to handle the judgment-heavy tail that a fixed rule base can't, **on detection** of a vulnerable dependency.

The design principle that matters: **use the low-cost, known-good solution for everything it can do, and let AI extend it only where it adds value.** Done that way, adoption of AI across an enterprise isn't a big-bang project — it's a complement. You get genuinely new functionality (automating major upgrades at the moment they're detected) while *reducing* the total cost of the program, because the expensive AI token spend is only spent where the cheaper solution can't already do the job.

## The trap: "AI all the time"

There's an opposite risk to this, and it's the one I see most enterprises running toward: **"AI all the time."** The default posture in a lot of organizations right now is to assume every problem is an AI problem — and to throw a model at anything that moves, on every interaction, everywhere.

That's a recipe for blowing your budget. It's also usually *worse* at the task.

Here's the thinking that's missing from most of it: the majority of the tooling we already run is still genuinely good at what it does. It has been for a decade. These are known-functional, battle-tested solutions that solve real problems reliably and cheaply. The instinct to rip them out and replace them with an AI-driven version *for the sake of it being AI* isn't modernization — it's paying a premium to reintroduce fragility into something that already worked.

So the optimization that actually matters isn't "how much AI can we use." It's **optimizing for cost versus the specific deficiency you're actually trying to fix.** The right question isn't *can AI do this?* — it's *does AI do this better than the tool we already have, given that it costs more?*

That discipline is what separates a program that uses AI wisely from one that burns money to look current:

- **If the known solution already solves it well** — keep it. Don't pay for AI to do what the proven tool already does at near-zero marginal cost.
- **If there's a genuine gap** — a judgment-heavy case, a breaking change, a decision the rule base can't make reliably — *that's* where the AI token spend earns its keep.
- **Replace the proven solution only when the AI version is actually better**, not when it's merely different or newer.

Most enterprises are going to learn this lesson the expensive way. The ones that get it right are the ones that ask the hard question up front: *where does AI genuinely extend what we have, versus where would it just be an expensive substitute for something that's already working?* The answer to that is where you deploy next-gen capability — and where you keep your budget intact.

In other words: the future isn't AI *instead of* the paved path — it's AI on top of it, deployed surgically, so the marginal cost of each new capability stays low while the functionality keeps expanding.

That's the story of Part 4: how we started layering next-generation, AI-driven automation onto a current-gen foundation — and what it took to do it without blowing up the economics.

*Read Part 4: where next-generation, AI-driven remediation meets the paved path — and how to make the upgrade-on-detection opportunity scale.*

---

*I'd value your take on enforcement vs. friction — and on where AI belongs (and doesn't) in a security program. What's worked in your organization, and what taught you the most when it didn't?*
