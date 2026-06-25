# GSM Project Governance

This document defines how the **Generative System Model (GSM)** standard is governed. It is the operational governance for this repository. A narrative companion is published as the [Charter](https://docs.poesis.cloud/gsm/charter/) on the documentation site; where the two differ, **this file prevails** for repository operations.

> **Status.** This repository is maintained today by a single steward (see `MAINTAINERS.md`). The governance below is written to be ready for neutral, multi-organization stewardship — including potential contribution to a foundation such as the [CNCF](https://www.cncf.io/) — and applies as the project grows.

## 1. Mission

To define and steward GSM as an open, vendor-neutral standard for *defining and governing* software-intensive systems — the THINK-layer complement to runtime standards such as OpenTelemetry — so that governed definitions are portable across tools instead of locked in proprietary silos.

## 2. Scope

**In scope.** The normative model (eight primitives, the DNA grammar, the Archetype type system, the `$gsm:*` vocabulary, the Ascription lifecycle, the expression-language profiles), the canonical JSON interchange, the conformance catalog, and supporting non-normative material (primer, use cases, mappings).

**Out of scope.** Specific storage engines, transports, UIs, the descriptive/observability counterpart, and any single vendor's product. A reference implementation may be stewarded separately under its own license.

## 3. Values

1. **Small, coherent core** — additions are resisted by default; the burden of proof is on growth.
2. **Vendor neutrality** — no requirement may privilege one vendor, language, or platform.
3. **Backwards compatibility** — breaking changes are rare, versioned, and migration-documented.
4. **Decisions in the open** — substantive change happens through public proposals and review.
5. **Specification over implementation** — conformance is defined so many implementations interoperate; none is privileged by the standard.

## 4. Roles

| Role | Responsibility |
|---|---|
| **Lead maintainer** | Final steward; breaks ties; accountable for releases and IP. Currently **Clément Cazaud** (see `MAINTAINERS.md`). |
| **Maintainers** | Merge rights; release management; uphold this governance. |
| **Specification editors** | Maintain normative documents for clarity and internal consistency; editors do not, by themselves, decide normative changes. |
| **Contributors** | Anyone proposing changes via the process in §5, under the contribution gates in `CONTRIBUTING.md`. |

Maintainers are added or removed by maintainer supermajority and recorded in `MAINTAINERS.md`. The project actively seeks additional maintainers from other organizations.

## 5. Decision-making

- **Lazy consensus** is the default: a proposal open for the stated review window with no sustained objection is accepted.
- **Normative changes** — anything affecting conformance (primitives, vocabulary, lifecycle, expression profiles, interchange) — require an explicit **maintainer supermajority** and a review window no shorter than the editorial-change window.
- **Editorial changes** (wording, examples, non-normative docs) may proceed by lazy consensus.
- **Sustained objection** escalates to a recorded maintainer vote; ties are decided by the lead maintainer; absent a lead-maintainer decision, the status quo wins.

### 5.1 GSM Change Proposals

Substantive changes are raised as a written **GSM Change Proposal** (a tracked issue or PR) stating: motivation, the normative delta, conformance impact (which `GSM-*` requirement IDs change — see the [Conformance catalog](https://docs.poesis.cloud/gsm/conformance/)), backwards-compatibility analysis, and migration notes. Editors integrate accepted proposals and update the conformance catalog with stable IDs.

## 6. Releases and versioning

- The specification uses **semantic versioning**. Pre-1.0 releases may break; from 1.0, breaking changes increment the major version.
- The eight base Archetype schemas and the seed meta-schema are versioned **with** the specification.
- Each release records its changes and any withdrawn or added conformance IDs.

## 7. Backwards-compatibility policy

Within a major version: no removal or semantic change of an existing primitive, enumeration value, `$gsm:*` keyword, lifecycle edge, or interchange member. Additions must be optional and must not alter the meaning of existing documents. Deprecations are announced at least one minor release before removal at the next major version.

## 8. Contribution gates, licensing, and IP

- **Contribution gates.** Provenance via **DCO** and rights via **CLA**, as defined in `CONTRIBUTING.md`.
- **Current license.** This repository's content is licensed **CC BY-SA 4.0** (see `LICENSE`).
- **Proposed CNCF-alignment posture.** Foundation contribution would likely require relicensing the **specification text to CC BY 4.0** (dropping ShareAlike, which the CNCF IP Policy generally prefers for specifications) and licensing **reference schemas and code under Apache-2.0**. This relicensing is a **decision reserved to the rights holder** and is **not** executed by this document; current licenses remain authoritative until formally changed.

## 9. Code of Conduct

Participation is governed by `CODE_OF_CONDUCT.md`.

## 10. Amending this document

Amendments follow the **normative-change** process (§5): a written proposal, a review window, and maintainer supermajority. After any foundation hosting, amendments also follow the hosting foundation's requirements.
