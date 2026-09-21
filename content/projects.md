---
title: "Projects"
date: 2026-09-21
layout: "simple"
description: "Concrete engineering outcomes: platform design, incident response, and AI in production — with the numbers, not just the job titles."
---

Four projects, picked for outcomes rather than technology. Each links to the full write-up; this page is the five-second version.

**Track record:** ~20x artifact-processing throughput (25 → ~500/min) · SaaS deploy time cut from ~90 min to 25 min, success rate ~50% → ~80% · teams of 5–9 engineers led · FedRAMP High–compliant pipelines built and run in AWS GovCloud.

## Prompt Engineering in Production — Hours-to-Days Down to Minutes

Built an AWS Bedrock pipeline giving developers in a FedRAMP High environment autonomous access to their own build-failure analysis, without a human in the loop or direct production access. The old process — a ticket to a US-based team — took hours to days per failed build; the new one surfaces sanitized analysis in an internal developer portal within minutes. A Bedrock Knowledge Base of known failure patterns cuts hallucinations and token burn.

**Stack:** AWS Bedrock, AWS GovCloud, SQS, Lambda, CI/CD platform, internal developer portal

[Read the full case study →](/posts/2026-04-11-prompt-engineering-in-production/)

## Automated Artifact Factory — Turning a Recurring Support Pattern Into a Platform Boundary

Traced a recurring cluster of pipeline support tickets back to a structural problem: every team was independently wiring signing, vulnerability scanning, and antivirus checks into its own CI pipeline. Designed (not yet built) a centrally-owned, asynchronous artifact factory that runs these checks in parallel and gates promotion through a single status entity — so the next tool swap is one change behind a boundary, not a migration across every team.

**Stack:** AWS, Kubernetes/EKS, artifact repository/registry, CI/CD platform, internal developer portal

[Read the full case study →](/posts/2026-09-20-automated-artifact-factory/)

## Axios Supply Chain Incident — Finding the Blind Spot in a Trusted Cache

When a malicious axios release stayed live on npm for roughly three hours, our artifact repository cache kept serving it long after npm pulled the package. Traced exposure through a shared CI service account — which erases per-team attribution in access logs — by manually correlating build logs, then contained affected build nodes on the container orchestration platform and purged the cache. Fixes now in flight: dependency-pinning enforcement, a publication delay window, and automated malicious-package scanning.

**Stack:** Artifact repository/proxy, CI/CD platform, container orchestration platform (Kubernetes), npm

[Read the full case study →](/posts/2026-04-11-axios-supply-chain/)

## SCA Tool Outage — Making the Freeze-vs-Bypass Call

An SCA tool went down for about five hours, taking every required security gate with it and freezing all pull request merges org-wide. Within forty minutes, made the call — jointly with business leadership and security — to lift the gate and keep teams shipping, then closed the gap with a retroactive scan of every PR merged during the ~4.5-hour bypass window. Nothing was found, but the retrospective is now driving two efforts still in progress: more granular per-check bypass controls, and an evaluation of tooling redundancy so one vendor outage can't freeze the org again.

**Stack:** SCA tool, Git platform (required status checks), CI/CD platform, pipeline metrics/reporting

[Read the full case study →](/posts/2026-04-12-sca-tool-outage/)
