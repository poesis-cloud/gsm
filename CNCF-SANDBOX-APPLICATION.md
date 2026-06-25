# GSM — CNCF Sandbox Application (ready-to-paste)

> **How to use this file.** This is the submission body for the GSM Sandbox application. When the project is ready (see the checklist at the bottom), open an application in the [`cncf/sandbox`](https://github.com/cncf/sandbox) repository and paste the fields below, adjusting any item still marked _TBD_. Confirm the current template/criteria in `cncf/sandbox` and `cncf/toc` before submitting, as the process evolves.
>
> **Status:** Draft — not submitted. Nothing here implies CNCF review or acceptance.

---

## Application contact details

- **Primary contact:** Clément Cazaud — clement.cazaud@outlook.com (interim)
- **Organization / steward:** Poesis — https://github.com/poesis-cloud
- **Maintainer:** Clément Cazaud (see `MAINTAINERS.md`)

## Project name

**Generative System Model (GSM)**

## Project description

GSM is an open, vendor-neutral standard for **defining and governing** software-intensive systems — the **THINK** layer of IT (definition and governance), complementing the **BUILD** layer (pipelines) and the **RUN** layer (observability). It specifies a small fixed core of eight primitives, a three-tempo governance grammar (Directives, Norms, Ascriptions), an extensible Archetype type system, a normative lifecycle, two sandboxed expression-language profiles, and a canonical JSON interchange — so that governed definitions (security posture, SLOs, supply-chain provenance, data-protection rules, delivery guardrails, cost ceilings) are portable across conforming tools instead of trapped in proprietary silos.

## Project repository

- https://github.com/poesis-cloud/gsm

## Project website / documentation

- https://docs.poesis.cloud/gsm/
- Specification: https://docs.poesis.cloud/gsm/specification/
- Conformance: https://docs.poesis.cloud/gsm/conformance/
- Cloud-native use cases: https://docs.poesis.cloud/gsm/cloud-native-use-cases/

## Additional repositories

- Reference implementation (GSM Processor): SIE — within the Poesis project.
- First domain application (adopter): ITIP — within the Poesis project.

## Preferred maturity level

**Sandbox.**

## License

- **Current:** specification/theory content under **CC BY-SA 4.0**.
- **Proposed for CNCF:** relicense specification text to **CC BY 4.0** and reference schemas/code to **Apache-2.0** (OSI-approved). This relicensing is a decision reserved to the rights holder and would be executed as part of contribution.

## Source of project / IP and trademark

Developed within the Poesis project; copyright Clément Cazaud. The steward is willing, in principle, to transfer the project name/marks and operate under the **CNCF IP Policy** as part of contribution. Specific terms are _TBD_.

## Sponsors (TOC)

_TBD._ TOC sponsors are not mandatory for Sandbox since the 2021 process reform; a TAG/TOC champion will be sought during community engagement.

## Cloud-native fit

Cloud-native systems are governed systems: every workload carries security, reliability, supply-chain, compliance, and cost obligations that today live in non-portable, tool-specific config and prose. GSM provides an open, machine-evaluable way to express those obligations and bind them to subjects across their lifecycle. It **composes with** existing cloud-native projects rather than competing:

- **SLO governance** — Norms express SLOs; **OpenTelemetry**/Prometheus supply the evidence.
- **Admission & policy** — the Directive/Norm is the authoritative obligation; **OPA/Gatekeeper** and **Kyverno** enforce.
- **Supply chain** — provenance/signing/SBOM obligations as DNA; **sigstore**, **SLSA**, **in-toto**, **TUF** as evidence.
- **Zero-trust / mesh** — service coupling as Interactions governed by Norms; **SPIFFE/SPIRE**, **Istio**, **Linkerd** realize them.
- **Progressive delivery** — rollout guardrails as Norms; **Argo**, **Flux**, **Flagger** realize them.
- **Continuous compliance & data protection** — GDPR/NIS2/DORA sourced into Archetypes; the `$gsm:dataProtection` vocabulary governs at-rest/in-transit handling.
- **Platform & FinOps** — golden-path and cost obligations over application definitions; **Backstage**, **Crossplane**, **OpenCost** realize/observe them.

Full detail: https://docs.poesis.cloud/gsm/cloud-native-use-cases/

## Why CNCF, and why now

The RUN layer found a neutral home at CNCF in **OpenTelemetry**, ending telemetry fragmentation. The THINK layer — the definitions those runtimes enforce and measure against — has no equivalent open standard and is fragmenting across proprietary GRC and architecture tools. Neutral stewardship is what turns a model into a standard; CNCF's IP policy, governance, and ecosystem are the right environment for vendor-neutral adoption. Sandbox is the appropriate entry maturity for a pre-1.0 standard inviting community contribution.

## Vendor neutrality

The standard mandates no vendor, language, runtime, or storage technology. Conformance is defined for documents, producers, consumers, and processors so independent implementations interoperate. The reference implementation holds no privileged status in the specification.

## Comparable / adjacent projects

- **OpenTelemetry** — RUN layer (telemetry); complementary.
- **CloudEvents** — event interchange; reused as a carrier, not replaced.
- **Open Policy Agent / policy-as-code** — evaluate rules against current state; GSM governs the *definition of the state* with a lifecycle and governance grammar. Complementary.
- **TOGAF / ArchiMate, ISO 25010, GDPR/NIS2/DORA** — sourced into GSM as Archetypes and DNA, not replaced.

## Governance, contributing, and Code of Conduct

- Governance: `GOVERNANCE.md` (lazy consensus; supermajority for normative change; written change proposals; semantic versioning).
- Maintainers: `MAINTAINERS.md`.
- Contributing (DCO + CLA): `CONTRIBUTING.md`.
- Code of Conduct: `CODE_OF_CONDUCT.md` (adopts the CNCF Community Code of Conduct).

## Roadmap

- **Near term:** stabilize the 0.x specification; publish conformance test fixtures; engage TAG App Delivery and TAG Security & Compliance; gather implementer feedback.
- **Mid term:** independent implementations beyond the reference; framework-mapping guides (TOGAF/ISO/GDPR → GSM); a 1.0 candidate with a frozen conformance surface.
- **Longer term:** broaden adoption beyond IT governance; interoperability tests with complementary CNCF projects.

## External dependencies

Open specifications only: JSON (RFC 8259), JSON Schema 2020-12, UUIDv7 (RFC 9562), CEL, and Starlark. No proprietary components.

## Adopters

- **SIE** — reference implementation (GSM Processor), within Poesis.
- **ITIP** — domain application built on SIE, within Poesis.

No external third-party adopters are claimed at this stage.

---

## Pre-submission checklist

- [ ] Public `gsm` repo contains `GOVERNANCE.md`, `MAINTAINERS.md`, `CODE_OF_CONDUCT.md`, `CONTRIBUTING.md`, `LICENSE`.
- [ ] Open-license posture confirmed (spec text → CC BY 4.0; code → Apache-2.0) **or** rationale prepared for keeping CC BY-SA.
- [ ] DCO sign-off enforced on commits.
- [ ] At least one external maintainer or independent implementer engaged.
- [ ] Presented to TAG App Delivery and/or TAG Security & Compliance; feedback incorporated.
- [ ] Conformance test fixtures published.
- [ ] `_TBD_` contacts and reporting address filled in.
