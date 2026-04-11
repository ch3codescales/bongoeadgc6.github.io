---
title: "Supply Chain Attacks Start in Your Build Cache"
date: 2026-04-11
draft: false
description: "When axios 1.14.1 introduced a malicious dependency, our the artifact repository cache kept serving it long after npm pulled it. Here's how we investigated, contained, and hardened against it."
tags: ["Security", "DevOps", "CI/CD", "Supply Chain", "npm", "the artifact repository", "the container orchestration platform"]
categories: ["Engineering"]
---

On March 25, 2025, a malicious version of the axios npm package — version 1.14.1 — was published to the npm registry. It contained a bundled dependency called `plain-crypto-js@4.2.0`, which was later confirmed to be malicious. The package was identified and removed from the npm registry within roughly three hours.

For most organizations, three hours sounds manageable. For us, it wasn't that simple.

<!--more-->

## How We Found Out

I came across John Hammond's breakdown of the incident through Huntress. After watching it, the severity was clear — this wasn't a theoretical risk. Axios is one of the most widely used HTTP client libraries in the JavaScript ecosystem, and our organization had pipelines using it across multiple teams.

I immediately looped in our product security team and central security architects. We aligned quickly on the need to investigate our exposure and got to work.

## The First Problem: the artifact repository Doesn't Know What It Doesn't Know

Our organization uses the artifact repository as a proxy and cache for third-party npm packages. The intent is to reduce external dependency, improve build reliability, and give us a controlled chokepoint for what enters our supply chain. In theory, it's exactly the right architecture for situations like this.

In practice, it had a blind spot.

When `axios@1.14.1` was published, some of our pipelines pulled it through the artifact repository during that three-hour window. the artifact repository cached it. When npm yanked the package, the artifact repository didn't get the memo — it continued serving the cached version to any pipeline that requested it. From the artifact repository's perspective, it was just doing its job.

We were able to check the download count for `axios@1.14.1` in the artifact repository relatively quickly. The numbers confirmed it had been pulled. The harder question was: *by whom?*

## The Second Problem: A Service Account Tells You Nothing

Our CI/CD pipelines authenticate to the artifact repository using a shared service account. That means every download from every pipeline shows up under the same username in the the artifact repository access logs. The logs are effectively web server access logs — they tell you what was downloaded, but not which team or pipeline requested it.

To answer that question, we needed to look at the build logs in our CI/CD platform. Those logs weren't piped into a SIEM or centralized logging system. Ingesting them at scale would be expensive, and potentially introduce additional data exposure risk. So our DFIR team did it the hard way — manually searching build logs for specific log lines indicating the `axios@1.14.1` download and the presence of `plain-crypto-js@4.2.0`.

## What We Found

The DFIR team searched for npm install log lines referencing `axios@1.14.1` and `plain-crypto-js@4.2.0`. They found hits.

The affected pipelines weren't directly depending on axios — they were pulling `datadog-ci` and `npm-groovy-lint`, both of which list axios as a dependency. Critically, neither package had pinned an explicit axios version in their `package.json`. They used a minimum version constraint, meaning npm would resolve to the latest matching version available at install time.

During that three-hour window, the latest matching version was `1.14.1`. Any pipeline that ran an `npm install` during that period — or afterward, while the cached version remained available in the artifact repository — pulled the malicious package.

## Containment

Once we confirmed which build agents had been affected, we traced them to their specific the container orchestration platform worker nodes. We treated those nodes as compromised.

The containerized workload model worked in our favor here. Because the build agents were isolated containers, the blast radius was constrained. We quarantined the affected nodes, captured the volumes for forensic inspection, removed the nodes from the cluster, and terminated them after completing our investigation. No malicious files or indicators of active compromise were found, but we weren't willing to leave that to chance.

We also manually purged `axios@1.14.1` and `plain-crypto-js@4.2.0` from the the artifact repository cache to eliminate the ongoing distribution risk.

## What We're Doing About It

This incident exposed a few gaps we're now actively closing:

**Dependency pinning enforcement** — We're implementing automated checks across CI/CD pipelines to flag unpinned dependency versions as a policy violation. Using minimum version constraints (`^`, `~`, `>=`) is a common practice that most developers don't think twice about. This incident is a good illustration of why it matters.

**Package publication delay** — We're investigating a configurable delay in the artifact repository before newly published package versions become available to pipelines. A 24-hour hold would have meant `axios@1.14.1` was already removed from npm before any of our pipelines could pull it. It adds friction to adopting new releases, but that tradeoff is worth it for the reduction in zero-day exposure.

**Automated malicious package scanning** — We're evaluating solutions that can scan packages in the artifact repository for known-malicious indicators and automatically quarantine them, rather than relying on manual response after the fact.

## The Broader Lesson

The axios incident is a good case study in how supply chain attacks actually work in practice. The malicious package was live for three hours. That's fast enough to fly under the radar of most security teams — but slow enough to get cached everywhere.

The real risk isn't the registry. It's everything downstream of it: your artifact proxy, your package cache, your pipelines that were running during the window. By the time npm acts, the package may already be sitting in your the artifact repository waiting for the next build to pull it.

If you use a package proxy or cache — and you should — make sure you have a plan for what happens when that cache becomes the attack vector rather than the defense.

---

*Credit to [Huntress](https://www.huntress.com/blog/supply-chain-compromise-axios-npm-package) for their rapid analysis and public disclosure of the axios supply chain compromise.*
