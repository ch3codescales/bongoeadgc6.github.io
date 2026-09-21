---
title: "Giving the Backlog Teeth: What Enforcing Remediation SLOs Actually Requires"
date: 2026-02-06
draft: false
description: "Running a vulnerability-management program across a 3,000+ developer organization taught me that you can't improve what you don't enforce. Here's what giving the backlog teeth actually requires."
tags: ["Security", "Vulnerability Management", "DevSecOps", "CISO"]
categories: ["Security"]
---

Every security leader has stared down a backlog of ignored vulnerabilities. You know the shape of it: thousands of open findings, tracked diligently, remediated rarely. The problem wasn't that people didn't care; it was that nothing was *at stake* for not fixing them.

I led a vulnerability-management program across a large engineering organization of thousands of developers and repositories. Early on I learned that you can't improve what you don't enforce. Here's what actually happens when you give the backlog teeth, and the uncomfortable truth about what enforcing it really takes.

<!--more-->

## Start by asking: where's the accountability gap?

Before touching any tooling, look for the structural reason things don't get fixed. In our case, teams weren't refusing to remediate. They were *prioritizing*. But with no forcing function, "fix it eventually" quietly became "fix it never." Vulnerability work always loses to feature work when both are optional.

The gap wasn't awareness. It was that nothing forced a decision on a deadline. That's where enforcement enters the picture, not as a punishment mechanism but as the accountability structure that makes prioritization honest.

## Enforcement is a product decision, not a policy decision

When I introduced remediation SLOs (critical vulnerabilities had a fixed window to be fixed before the team was blocked), I deliberately treated it less like a compliance rule and more like a product launch. The reasoning:

- **An SLO nobody can meet is just noise.** If the window is unrealistic, teams stop believing the system, and the metric becomes fiction everyone quietly ignores.
- **Enforcement changes behavior only if the consequence is real.** A block on day N that the system never actually fires is theater, and teams sense it immediately.
- **The design has to survive contact with reality.** This is where most enforcement programs die, not on principle but on the day-to-day friction of operating at scale.

The uncomfortable truth: putting enforcement in place is the *beginning* of the work, not the end. It surfaces problems everywhere, some you predicted and some you didn't. And your willingness to watch the backlash, learn from the friction, and iterate on the design is what separates an enforced program from a performative one.

## What I'd tell a security leader starting this

1. **Pick a metric that can't be gamed.** Time-to-fix is better than raw count, because it measures behavior, not volume.
2. **Expect the friction.** Enforcement reveals the friction in your process. That's data, not a failure.
3. **Measure before you enforce.** You need the baseline to prove later that you moved the needle.
4. **Be ready to iterate.** Your first enforcement design will be wrong in some way. The fix is learning from it, which is exactly where the next part of this story begins.

When we compressed the average critical-vulnerability lifecycle from weeks to days and cleared thousands of backlogged findings, the enforcement timer was the catalyst. But it wasn't the solution. The solution took two more iterations to find, and the middle one was the most instructive.

*Read Part 2: [the safety valve that taught us why enforcement alone isn't enough]({{< relref "/posts/2026-02-13-the-waiver-trap" >}}).*
