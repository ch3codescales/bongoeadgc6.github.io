---
title: "Designing an Automated Artifact Factory: A Case Study in Turning Pipeline Pain Into a Platform Boundary"
date: 2026-09-20
draft: false
description: "How we traced a recurring cluster of support pain back to a structural problem, and designed a platform service to fix it — including the decisions still unresolved."
tags: ["Security", "DevOps", "CI/CD", "Platform Engineering", "Artifact Management", "Automation"]
categories: ["Engineering"]
---

Every team wiring the same three security checks into its own pipeline isn't really a security problem. It's a platform problem wearing a security costume.

This is the story of how we traced that pattern back to a structural issue, and the design we landed on to fix it: an automated artifact factory that turns signing, vulnerability scanning, and antivirus scanning from something every team bolts onto their own pipeline into something the platform just does for them.

<!--more-->

## Where This Started

The signal came from support data, not a single dramatic incident. Code signing had quietly become a disproportionate share of our support ticket volume, with a recurring cluster of "signing job failures" that ate up a lot of engineering time across many tickets. Separately, a single offline signing node had silently queued far more jobs than anyone expected before anyone noticed and filed a ticket — the kind of blast-radius pattern that's easy to miss until you actually look at the aggregate data.

Neither of those, on its own, was an emergency. Together, they pointed at something structural: every team was independently gluing signing, vulnerability scanning, and antivirus checks into their own CI pipeline, and every one of those integrations was a slightly different, slightly fragile thing.

## Why We Looked at It Now

There was also a forcing function. We have a code-signing tool migration in discovery right now, and that's where the support-data pattern turned into an actual design question. Swapping a signing tool without a platform boundary means every team touches their own pipeline again. Swapping it *with* one means the change happens behind the scenes.

That reframed the whole problem. This wasn't really about signing specifically — it was about whether we had a boundary at all between "the checks that gate an artifact" and "the pipelines that produce artifacts." We didn't. So every future tool swap, in signing or anywhere else, was going to cost us a full migration across every team, forever.

## Why the Current Approach Doesn't Scale

Today, artifact signing, vulnerability scanning, and antivirus scanning are steps individual teams inject into their own CI pipelines using shared-library snippets. That gets you repetitive per-team setup, inconsistent coverage depending on who copied which version of the snippet, and security gating that's coupled to whatever each pipeline happens to implement — or forgets to.

The failure mode compounds it. These steps are error-prone, and when they're not built to fail gracefully, one failure forces the whole pipeline to restart from the beginning instead of resuming from where it left off. That's expensive in engineering time, repeatedly, across every team that hits it.

## The Design We Landed On

We designed a centrally-owned, asynchronous **artifact factory**: uploading an artifact to the staging tier of our artifact repository triggers signing, vulnerability scanning, and antivirus scanning automatically, in parallel, without any team wiring anything into their own pipeline. Results land on a single status entity per artifact version in our internal developer portal, and that status gates promotion through the pipeline.

{{< mermaid >}}
flowchart LR
    A["Volatile tiers\n(dev / latest)\nout of scope"] --> B["Staging tier\nupload triggers event"]
    B --> C["Orchestrator + developer portal\ncreates pending status entity"]
    C --> D["Code signing"]
    C --> E["Vulnerability scan"]
    C --> F["Antivirus scan"]
    D --> G{"All required\nchecks pass?"}
    E --> G
    F --> G
    G -- yes --> H["RC tier\nauto-promoted"]
    G -- no --> I["Quarantine + alert\naccess revoked, team notified"]
    H --> J["Release\nhuman-gated promotion"]
{{< /mermaid >}}

A few decisions shaped that design more than anything else:

**The factory only engages once an artifact stabilizes.** Artifacts are still volatile in the earliest tiers — actively changing, not yet meant to be checked. We deliberately kept the factory out of that stage entirely: no event, no status entity, no checks. It only begins once an artifact reaches staging, where it's meant to hold still long enough to be evaluated.

**Promotion out of staging is automatic, not another approval queue.** Once every *required* check passes, the artifact moves itself from staging to the release-candidate tier. No human clicks anything. If a required check fails, the artifact is quarantined — held back, download access revoked, the responsible team notified immediately. A failed *optional* check doesn't block anything; it's recorded on the status entity but doesn't stop the artifact from moving forward. We haven't settled which checks are required versus optional yet, or whether we need a waiver path for someone to override a required-check failure deliberately. That's an open question, not a design gap we glossed over.

**We drew the boundary at "checks," not at "everything downstream."** What a team does with an artifact once it reaches the release-candidate tier — further validation, integration testing, manual QA — stays entirely theirs. We considered pulling more of that into the platform and deliberately didn't: the value of this project is in owning the checks, not in owning every team's release process.

**Promotion to release stays human-gated.** We're not automating the last step. That promotion uses infrastructure we already have; the factory's job is only to make sure the status entity has good enough data that a human making that call actually has what they need.

## The Parts We Deliberately Didn't Solve Yet

A design case study is more honest if it says what's still unresolved, so:

- We haven't yet confirmed that the event mechanism we're relying on is actually reachable from our own infrastructure against our specific systems, or what its real delivery and retry guarantees look like in practice. That needs verification before this is buildable, not just designed.
- The download-revocation mechanism we picked for quarantine needs to operate per artifact version, not per repository — we ruled out an alternative that only worked at the repository level. Whether the per-version mechanism is fully licensed and enabled on our own systems is still unconfirmed.
- Antivirus tooling for non-Windows artifact kinds doesn't have an answer yet.
- The exact schema for the status entity — what it tracks, how it represents which tier a version currently sits in — is still a discussion, not a spec.
- We haven't decided how existing per-team pipeline steps get migrated onto the factory and then retired: all at once, gradually, team by team. That sequencing question is intentionally deferred until the core of the factory is built and proven.

None of these are fatal to the design. All of them need answers before it's a spec someone can build against.

## What This Taught Us

The obvious payoff here is developer experience: upload an artifact, and the checks just happen, consistently, without every team maintaining their own copy of the logic.

The less obvious payoff is architectural, and it's the one that actually justified the effort. By centralizing these checks behind one platform boundary, swapping any underlying tool — the signing tool, the vulnerability scanner, the antivirus engine — becomes a change made once, behind that boundary, instead of a migration that touches every team's pipeline. That's the property we were actually missing, and it's the reason a signing-tool migration already in discovery is what turned this from "would be nice" into "worth designing now."

There's a quieter efficiency argument too: today, the same artifact often gets pulled and re-checked multiple times across multiple places. A factory that pulls an artifact once and runs every check against that single pull is a smaller cost on both infrastructure load and cloud spend — not the headline reason to do this, but a real one.

## Where It Stands

The open questions above are the actual next step: resolve them, verify the two that need hands-on confirmation against our own infrastructure, and turn this from a design into something implementable. The interesting part of this project was never the individual checks — it's whether the boundary underneath them holds up the next time we need to swap a tool without anyone else having to notice.
