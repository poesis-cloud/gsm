---
layout: default
title: CNCF Sandbox Proposal
parent: GSM
nav_order: 7
---

# GSM — CNCF Sandbox Proposal (Draft)
{: .no_toc }

**Status:** Draft application — not submitted
{: .fs-5 .fw-300 }

This is a **draft** application proposing the Generative System Model (GSM) for the [CNCF](https://www.cncf.io/) **Sandbox** maturity level. It follows the structure of the CNCF Sandbox intake. It has **not** been submitted to the CNCF, and nothing here implies CNCF review or acceptance. Fields that depend on people or relationships not yet established are marked **TBD** rather than filled speculatively.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 1. Application contact

- **Originating maintainer:** Poesis — [github.com/poesis-cloud](https://github.com/poesis-cloud)
- **Primary contact:** TBD
- **Maintainers file:** to be added at contribution time (see [Charter §4](charter.md)).

## 2. Project name

**Generative System Model (GSM)** — an open standard for defining and governing software-intensive systems.

## 3. Project description

GSM is a small, fixed vocabulary for expressing **what a software-intensive system must be** — its purpose, obligations, and constraints — as **typed, versioned, machine-evaluable definitions** rather than prose. It standardizes the **THINK** layer of IT (definition and governance), complementing the BUILD layer (pipelines) and the RUN layer (observability). The standard specifies eight primitives, a three-tempo governance grammar (Directives, Norms, Ascriptions), an extensible Archetype type system, a normative lifecycle, two sandboxed expression-language profiles, and a canonical JSON interchange so that governed definitions are portable across conforming tools.

## 4. Statement on alignment with CNCF mission

The CNCF fosters and sustains an ecosystem of open, vendor-neutral projects for cloud-native systems. Cloud-native systems are governed systems: they carry compliance, security, reliability, and architectural obligations that today live in non-portable, tool-specific prose. GSM provides an **open, vendor-neutral, machine-evaluable** way to express those obligations and bind them to subjects across their lifecycle — directly serving CNCF's goals of interoperability and avoidance of lock-in. GSM is the definitional complement to existing CNCF runtime standards.

## 5. Cloud-native fit, alignment, and gap

- **Fit.** GSM is model-and-interchange first, infrastructure-independent, and implementable by many tools — the same shape as CloudEvents and OpenTelemetry.
- **Alignment.** It composes with CNCF projects rather than competing: OpenTelemetry produces the descriptive evidence a GSM Norm is evaluated against; a GSM Effector's output may be carried as a CloudEvent; workload-identity and feature-flag standards realize definitions GSM governs.
- **Gap it fills.** There is no open, vendor-neutral standard for the *definition/governance* (THINK) layer. Governance is fragmented across architecture wikis, compliance spreadsheets, and proprietary GRC tools whose models do not interoperate. GSM closes that gap.

**Representative cloud-native use cases** (full detail: [Cloud-Native Use Cases](cloud-native-use-cases.md)):

- **SLO governance** — Norms express SLOs; OpenTelemetry supplies the evidence.
- **Admission & policy** — the Directive/Norm is the authoritative obligation; OPA/Gatekeeper and Kyverno enforce.
- **Supply chain** — provenance/signing/SBOM obligations as DNA; sigstore, SLSA, in-toto, and TUF as evidence.
- **Zero-trust / mesh** — service coupling as Interactions governed by Norms; SPIFFE/SPIRE, Istio, and Linkerd realize them.
- **Progressive delivery** — rollout guardrails as Norms; Argo, Flux, and Flagger realize them.
- **Continuous compliance & data protection** — GDPR/NIS2/DORA sourced into Archetypes; the `$gsm:dataProtection` vocabulary governs data handling.
- **Platform & FinOps** — golden-path and cost obligations over application definitions; Backstage, Crossplane, and OpenCost realize/observe them.

## 6. Comparable / adjacent work (landscape)

- **OpenTelemetry** — RUN layer (telemetry). Complementary, not overlapping.
- **CloudEvents** — event interchange. GSM reuses it as a carrier, not a substitute.
- **Open Policy Agent / policy-as-code** — evaluates rules against current state. GSM governs the *definition of the state* (slow-moving identity and obligations) with a full lifecycle and governance grammar; the two are complementary.
- **Enterprise-architecture frameworks (TOGAF, ArchiMate) and quality/regulatory standards (ISO 25010, GDPR, NIS2, DORA)** — these are *sourced into* GSM as Archetypes and DNA, not replaced.

## 7. Vendor neutrality

The standard mandates no vendor, language, runtime, or storage technology. Conformance is defined for documents, producers, consumers, and processors so that independent implementations interoperate. The reference implementation is one conformant processor among potential others and holds no privileged status in the specification.

## 8. Why CNCF, and why Sandbox

- **Why CNCF.** Neutral stewardship is what turns a model into a standard. The THINK layer needs the same neutral home the RUN layer found in CNCF for OpenTelemetry; CNCF's IP policy, governance scaffolding, and ecosystem are the right environment for vendor-neutral adoption.
- **Why Sandbox.** GSM is young (pre-1.0). Sandbox is the appropriate entry maturity: it signals early-stage, invites community contribution, and sets expectations honestly. Incubation would be sought only after demonstrating multi-party adoption and a stable 1.0.

## 9. Roadmap (high level)

- **Near term:** stabilize the 0.x specification; publish the conformance catalog and a reference test fixture set; gather implementer feedback.
- **Mid term:** independent implementations beyond the reference; framework-mapping guides (TOGAF/ISO/GDPR → GSM); a 1.0 candidate with a frozen conformance surface.
- **Longer term:** broaden domain adoption beyond IT governance; formal interoperability tests with complementary CNCF projects.

Dates are intentionally omitted; this is an early-stage draft.

## 10. Governance and maintainers

Proposed governance is documented in the [Charter](charter.md): maintainer roles, lazy-consensus default with supermajority for normative change, a written change-proposal process, and semantic versioning. A `MAINTAINERS` file would be established at contribution time. Current steward: Poesis.

## 11. Contributing and Code of Conduct

Contribution would follow the hosting foundation's DCO/CLA and Code of Conduct. Until hosted, the source repositories' `CONTRIBUTING` and `CODE_OF_CONDUCT`/`CONTRIBUTING` guidance applies.

## 12. License

- **Specification text:** CC BY 4.0 (proposed).
- **Reference schemas and implementation:** Apache-2.0 (proposed, OSI-approved).

This relicensing is **proposed**; current repository licenses remain authoritative until formally updated by the rights holder.

## 13. Trademark and IP

The originating maintainer is willing, in principle, to transfer the project name/marks and to operate under the hosting foundation's IP Policy as part of contribution. Specific terms are **TBD** and subject to the rights holder's formal decision.

## 14. External dependencies

The standard depends only on open specifications: JSON ([RFC 8259](https://www.rfc-editor.org/rfc/rfc8259)), JSON Schema 2020-12, UUIDv7 ([RFC 9562](https://www.rfc-editor.org/rfc/rfc9562)), [CEL](https://github.com/google/cel-spec), and [Starlark](https://github.com/bazelbuild/starlark). It requires no proprietary component.

## 15. Adopters

- **SIE (Systemic Intelligence Engine)** — the reference implementation (a GSM Processor) within the Poesis project.
- **ITIP (IT Intelligence Platform)** — a domain application built on SIE within the Poesis project.

No external (third-party) adopters are claimed at this draft stage.

## 16. Existing sponsorship / funding

Developed within the **Poesis** project. No external CNCF or vendor funding. Requested TOC sponsors: **TBD**.

---

### Related documents

- [Specification](specification.md) — the normative standard.
- [Primer](primer.md) — non-normative introduction.
- [Conformance](conformance.md) — testable requirement catalog.
- [Charter](charter.md) — proposed project governance.
