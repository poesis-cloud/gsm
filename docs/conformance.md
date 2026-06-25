---
layout: default
title: Conformance
parent: GSM
nav_order: 5
---

# GSM Conformance
{: .no_toc }

**Status:** Normative where it restates the [Specification](specification.md); adds stable assertion IDs
{: .fs-5 .fw-300 }

This document catalogs the **testable conformance requirements** of GSM. Each requirement has a stable identifier (e.g. `GSM-PROC-17`) so test suites, audits, and conformance claims can cite it. The requirement levels (MUST / SHOULD / MAY) and their meaning are defined in [Specification §3](https://docs.poesis.cloud/gsm/specification/#3-notational-conventions). Where this document and the Specification differ, the **Specification prevails**.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 1. Conformance targets

GSM defines four targets (see [Specification §16](https://docs.poesis.cloud/gsm/specification/#16-conformance)):

| Target | What it is |
|---|---|
| **GSM Document** | A canonical-JSON serialization of Definitions and their Ascriptions. |
| **GSM Producer** | Software that emits GSM Documents. |
| **GSM Consumer** | Software that ingests GSM Documents. |
| **GSM Processor** | Software that authors, validates, and governs Ascriptions (the engine). |

A Processor that emits or ingests documents MUST additionally satisfy the Producer / Consumer requirements for those documents.

## 2. Conformance profiles

| Profile | Targets it bundles | Intended implementer |
|---|---|---|
| **Interchange** | Document + Producer + Consumer | Tools that exchange governed definitions but do not run the lifecycle. |
| **Governance Processor** | Processor + Extension rules (+ Interchange for any I/O) | Engines that author and govern Ascriptions (e.g. a Definition Manager). |

An implementation claims conformance by naming a **profile**, the **specification version** it targets, and the list of requirement IDs it satisfies, with any exceptions explicitly noted (§7).

## 3. Document requirements (`GSM-DOC-*`)

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-DOC-1` | MUST | Be well-formed JSON in the canonical interchange representation. | §18 |
| `GSM-DOC-2` | MUST | Carry a top-level `gsm` member naming the targeted specification version. | §18 |
| `GSM-DOC-3` | MUST | Give every Definition an `id` and a sealed `subjectType`. | §6.1, §6.3 |
| `GSM-DOC-4` | MUST | Give every Ascription the required envelope members and a valid `status`. | §6.2, §12.1 |
| `GSM-DOC-5` | MUST | Ensure each `statement` validates against its typing Archetype's schema. | §6.2, §9 |
| `GSM-DOC-6` | MUST | Keep identity-bound values identical across Ascriptions of the same Definition. | §6.2, §11 |
| `GSM-DOC-7` | MUST | Spell `subjectType` and `status` exactly as the enumerations define. | §6.3, §12.1, §18 |

## 4. Producer requirements (`GSM-PRD-*`)

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-PRD-1` | MUST | Emit only conforming GSM Documents. | §16.2 |
| `GSM-PRD-2` | MUST | Assign Archetype `$id` as `gsmarc://gsm/{title}/v{version}`. | §9.1, §16.2 |
| `GSM-PRD-3` | MUST NOT | Emit a rootless Archetype in the `archetypeId` (typing) position. | §9.2, §16.2 |
| `GSM-PRD-4` | MUST | Preserve identity-bound values across versions. | §16.2 |

## 5. Consumer requirements (`GSM-CON-*`)

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-CON-1` | MUST | Reject documents that violate any `GSM-DOC-*` requirement. | §16.1, §16.3 |
| `GSM-CON-2` | MUST | Treat `statement` as authoritative and resolve references by `definitionId` / `archetypeId`. | §16.3 |
| `GSM-CON-3` | MUST NOT | Require non-standard extensions to interpret the core model. | §16.3 |
| `GSM-CON-4` | MUST | Select the in-effect Ascription (`ACTIVE`, else `DEPRECATED`) where one is required. | §18 |

## 6. Processor requirements (`GSM-PROC-*`)

### 6.1 Identity & versioning

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-PROC-1` | MUST | Keep at most one in-effect Ascription per Definition. | §6.1 |
| `GSM-PROC-2` | MUST | Treat Definition `id` and `subjectType` as immutable after creation. | §6.1, §6.3 |
| `GSM-PROC-3` | MUST | Enforce the identity invariant (`$gsm:identityBound`) across versions. | §6.2, §11 |
| `GSM-PROC-4` | MUST | Assign `version` at the `APPROVE` transition, monotonic per Definition. | §12.3 |
| `GSM-PROC-5` | MUST | Forbid a second `APPROVED` Ascription within one Definition (approval convergence). | §12.3 |
| `GSM-PROC-6` | MUST | Auto-deprecate the prior `ACTIVE` Ascription on activation handoff. | §12.3 |

### 6.2 Typing

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-PROC-7` | MUST | Enforce `allOf` convergence to exactly one GSM base for typing Archetypes. | §9.2 |
| `GSM-PROC-8` | MUST | Reject a rootless Archetype used as `archetypeId`. | §9.2 |
| `GSM-PROC-9` | MUST | Bundle the seed Archetype and validate every incoming Archetype against it. | §9.4 |
| `GSM-PROC-10` | MUST | Assign the Archetype `$id`; ignore tenant-supplied `$id`. | §9.1 |
| `GSM-PROC-11` | MUST | Enforce global uniqueness of Archetype `title` among in-effect Ascriptions. | §9.1 |

### 6.3 Schema vocabulary

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-PROC-12` | MUST | Reject unknown `$gsm:*` keywords (sealed vocabulary). | §11 |
| `GSM-PROC-13` | MUST | Apply `$gsm:*` validation at every schema node, not only the top level. | §11 |
| `GSM-PROC-14` | MUST | Reject `$gsm:queryable` together with `atRest.encryption` on the same property. | §11 |
| `GSM-PROC-15` | MUST | Treat the `$gsm:identityBound` property set as immutable per Archetype Definition. | §11 |
| `GSM-PROC-16` | MUST | Honor `$gsm:sealed` by rejecting tenant `allOf` extension. | §11 |

### 6.4 Lifecycle & cascades

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-PROC-17` | MUST | Implement the nine statuses and twelve transitions exactly; permit no other edges. | §12.1, §12.2 |
| `GSM-PROC-18` | MUST | Enforce referee preconditions before every transition. | §12.4 |
| `GSM-PROC-19` | MUST | Apply the constitutive cascade (Mechanism → ports), blocking the source on failure. | §12.5 |
| `GSM-PROC-20` | MUST | Apply the governing cascade (Structure → elements) as a no-op on failure. | §12.5 |
| `GSM-PROC-21` | MUST | Apply the dependent cascade (ports → Interaction) on degradation and terminal transitions. | §12.5 |
| `GSM-PROC-22` | MUST | Treat terminal statuses (`RETIRED`, `ABANDONED`, `REJECTED`) as permanent. | §12.2 |
| `GSM-PROC-23` | MUST | Record an immutable audit entry (pre-status, post-status, timestamp) per transition. | §12.2, §16.4 |

### 6.5 Governance grammar

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-PROC-24` | MUST | Reject Directive contradictions (`MUST` + `MUST_NOT`; `ENSURE` + `PREVENT`) on the same `verb`/`qualifier`/`purpose`. | §8.1.1 |
| `GSM-PROC-25` | MUST | Resolve opposing verb directions by modal precedence (`MUST` > `SHOULD` > `MAY`). | §8.1.1 |
| `GSM-PROC-26` | MUST | Reject floating Norms (no opening Directive). | §8.2 |
| `GSM-PROC-27` | MUST NOT | Apply referee preconditions to `Directive.purpose` (a string, not a reference). | §8.1, §12.4 |
| `GSM-PROC-28` | MUST | Validate a Norm's `qualifier` matches or descends from the opening Directive's `qualifier`. | §8.2 |

### 6.6 Expressions & rules

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-PROC-29` | MUST | Evaluate Norm `applicability` under the restricted CEL profile. | §14.1.1 |
| `GSM-PROC-30` | MUST | Evaluate Norm `assertion` under the assertion CEL profile. | §14.1.2 |
| `GSM-PROC-31` | MUST | Auto-derive Effectors/Receptors from the Mechanism rule AST and reuse port Definitions. | §14.2.2 |
| `GSM-PROC-32` | MUST NOT | Accept independently authored Mechanism ports. | §7.2, §16.4 |
| `GSM-PROC-33` | MUST | Sandbox Starlark rules: no `load()`, string-literal Archetype arguments, bounded steps. | §14.2.3 |
| `GSM-PROC-34` | MUST NOT | Resolve tenant-supplied `$schema` URIs; validate against the bundled seed Archetype. | §19 |
| `GSM-PROC-35` | MUST | Keep all expression evaluation deterministic and side-effect-free. | §14 |

## 7. Extension requirements (`GSM-EXT-*`)

| ID | Level | Requirement | Spec ref |
|---|---|---|---|
| `GSM-EXT-1` | MUST NOT | Add, remove, or rename `DefinitionSubjectType` values. | §16.5 |
| `GSM-EXT-2` | MUST NOT | Add `$gsm:*` vocabulary keywords. | §16.5 |
| `GSM-EXT-3` | MUST NOT | Introduce stored primitives eliminated in §15. | §15, §16.5 |
| `GSM-EXT-4` | MUST NOT | Alter the lifecycle state machine. | §16.5 |
| `GSM-EXT-5` | MAY | Define domain Archetypes that extend GSM bases via `allOf`. | §9, §16.5 |

## 8. Claiming conformance

A conformance claim SHOULD state:

1. The **profile** (Interchange or Governance Processor).
2. The **specification version** targeted (e.g. `gsm 0.1`).
3. The **requirement IDs** satisfied, and any **exceptions** with rationale.
4. The **evidence** (test suite, audit, or self-certification).

Exceptions to a MUST requirement disqualify a full-profile claim; an implementation MAY still report partial conformance by enumerating the IDs it meets.

## 9. Versioning of conformance

Requirement IDs are **stable**: once published, an ID is never reassigned to a different requirement. A removed requirement is marked *withdrawn* and its ID is retired. New requirements take new IDs. This lets a conformance claim remain interpretable across specification versions.

## 10. Self-certification checklist

The condensed checklist lives in [Specification Appendix A](https://docs.poesis.cloud/gsm/specification/#appendix-a-conformance-checklist-informative). The catalog above is its authoritative, ID-stable expansion.
