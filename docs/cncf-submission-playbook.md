---
layout: default
title: CNCF Submission Playbook
parent: GSM
nav_order: 10
---

# GSM → CNCF Submission Playbook
{: .no_toc }

**A short guide to how CNCF acceptance works and how GSM approaches it**
{: .fs-5 .fw-300 }

This page explains, at a high level, how a project joins the [CNCF](https://www.cncf.io/) and the path GSM follows. It is a public process reference. Detailed, dated execution tracking lives outside this site.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## The maturity ladder

CNCF projects progress through three levels. A project enters at the bottom and earns the rest.

| Level | Bar | Signal |
|---|---|---|
| **Sandbox** | Early-stage; open license, neutral governance, clear cloud-native relevance | "Promising; experiment in the open" |
| **Incubating** | Multiple production adopters and multi-organization contributors; due diligence | "Proven and growing" |
| **Graduated** | Large-scale adoption, security audit, governance maturity, OpenSSF best-practices | "Mature; CNCF-endorsed" |

GSM targets **Sandbox**.

## How Sandbox acceptance works

1. **Prerequisites.** An OSI-approved/open license, a public repository with governance (`GOVERNANCE.md`, `MAINTAINERS.md`), a Code of Conduct, contribution terms (DCO/CLA), and a clear vendor-neutrality story.
2. **Apply.** Open an application in the **`cncf/sandbox`** repository using its template. (Since the 2021 process reform, TOC *sponsors* are no longer mandatory for Sandbox, though a champion still helps.)
3. **Review.** The Technical Oversight Committee (**TOC**) reviews applications in periodic batches and decides by vote.
4. **Onboarding.** On acceptance: IP/trademark donation under the CNCF IP Policy, governance alignment, and listing in the CNCF landscape, followed by annual review.

> Process details evolve — confirm the current criteria in the `cncf/toc` and `cncf/sandbox` repositories before submitting.

## How GSM engages the community

Acceptance is built before the application is filed, in the open:

- **Technical Advisory Groups.** GSM's natural homes are **TAG App Delivery** (application definition and delivery) and **TAG Security & Compliance** (governance, policy, supply chain). The plan is to present GSM to these TAGs and incorporate feedback.
- **CNCF Slack and public calls.** Share the [Specification](specification.md) and [Cloud-Native Use Cases](cloud-native-use-cases.md); gather implementer interest.
- **Composition, not competition.** GSM is positioned as the definitional complement to OpenTelemetry, CloudEvents, and the policy/supply-chain projects — it gives them a shared, portable source of truth.

## What is already prepared

| Artifact | Where |
|---|---|
| Normative specification | [Specification](specification.md) |
| Conformance catalog (stable IDs) | [Conformance](conformance.md) |
| Non-normative onboarding | [Primer](primer.md) |
| Cloud-native relevance | [Cloud-Native Use Cases](cloud-native-use-cases.md) |
| Proposed governance | [Charter](charter.md) |
| Draft application | [CNCF Sandbox Proposal](cncf-sandbox-proposal.md) |
| Repo artifacts (`GOVERNANCE.md`, `MAINTAINERS.md`, `CODE_OF_CONDUCT.md`, ready-to-paste application) | [github.com/poesis-cloud/gsm](https://github.com/poesis-cloud/gsm) |

## The remaining path, in brief

1. Make the public repository submission-ready (governance, Code of Conduct, maintainers, DCO).
2. Confirm the open-license posture for CNCF (specification text and reference code).
3. Present to the relevant TAGs and gather feedback.
4. Grow contribution and adoption signals beyond a single organization.
5. Open the `cncf/sandbox` application and iterate in public.
