---
layout: default
title: Specification
parent: GSM
nav_order: 4
---

# Generative System Model (GSM) Specification
{: .no_toc }

**Version:** 0.1 — Working Draft
{: .fs-5 .fw-300 }

<dl>
  <dt>Status of this document</dt>
  <dd><strong>Working Draft.</strong> This is a pre-1.0 draft of the GSM open standard, prepared as a candidate for contribution to the <a href="https://www.cncf.io/">Cloud Native Computing Foundation (CNCF)</a>. It has <em>not</em> been submitted to, reviewed by, or endorsed by the CNCF. Normative content is stable enough to implement against but MAY change before 1.0.</dd>
  <dt>Originating maintainer</dt>
  <dd>Poesis — <a href="https://github.com/poesis-cloud">github.com/poesis-cloud</a></dd>
  <dt>Editors</dt>
  <dd><em>TBD</em></dd>
  <dt>Latest published version</dt>
  <dd><a href="https://docs.poesis.cloud/gsm/specification/">docs.poesis.cloud/gsm/specification</a></dd>
  <dt>Reference implementation</dt>
  <dd>SIE — the Systemic Intelligence Engine (Definition Manager). See <a href="https://docs.poesis.cloud/sie-definition/">SIE</a>.</dd>
</dl>

> **Related documents.** This Specification is the normative core of the GSM document set: the non-normative [Primer](primer.md), the [Conformance](conformance.md) catalog (stable requirement IDs), the proposed [Charter](charter.md), and the draft [CNCF Sandbox Proposal](cncf-sandbox-proposal.md).

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 1. Abstract

The **Generative System Model (GSM)** is an open, vendor-neutral standard for **defining and governing** software-intensive systems. Where build pipelines standardize how systems are *built* and OpenTelemetry standardizes how systems are *observed*, GSM standardizes the **THINK** layer: how the *definition* of what a system must be — its purpose, its constraints, its obligations — is expressed, typed, versioned, and made machine-evaluable.

GSM specifies a small, fixed core of **eight primitives**, a three-tempo **governance grammar (DNA)** of Directives, Norms, and Ascriptions, an extensible **Archetype** type system, a normative **Ascription lifecycle**, two **expression-language profiles** for machine-evaluable governance, and a **canonical JSON interchange** representation so that governed definitions are portable across conforming tools instead of trapped in proprietary silos.

This document is the normative specification. It defines conformance for GSM documents, producers, consumers, and processors.

## 2. Introduction and motivation

Every IT organization runs three activities: it **builds** systems, it **runs** them, and it **thinks** them — governs, regulates, and aligns them. BUILD and RUN have won open toolchains. THINK has not: the definition of what a system *should be* lives in scattered prose — architecture wikis, compliance spreadsheets, review boards — that drifts from reality the moment it is written and is reconstructed only under audit pressure.

Prose cannot be machine-checked, composed, or enforced. As soon as software — or an AI agent — is asked to reason over "what the system should be," the implicit coupling a human mind carries effortlessly becomes an engineering problem. GSM closes this **automation gap** by making the definition **explicit, typed, versioned, and machine-evaluable**.

GSM is **generative**, not descriptive. A description records what a system *is*, after the fact. A definition declares what a system *must become* and carries the governance machinery to get there. This **Generative Inversion** is the core commitment of the model: a definition is the single source from which build-side execution is derived and against which run-side observation is compared.

> **Positioning.** GSM is to the THINK side of IT what OpenTelemetry is to the RUN side: one open standard so that governed definitions are portable, rather than fragmented across vendors. GSM is complementary to — not a replacement for — CNCF runtime standards (see [§17](#17-relationship-to-other-work)).

This specification standardizes the *model and its interchange*. It is intentionally independent of any storage technology, programming language, or deployment topology. The companion **[Manifesto](manifesto.md)** states the design intent and principles in non-normative form.

## 3. Notational conventions

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**, **SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL** in this document are to be interpreted as described in [BCP 14](https://www.rfc-editor.org/info/bcp14) ([RFC 2119](https://www.rfc-editor.org/rfc/rfc2119) and [RFC 8174](https://www.rfc-editor.org/rfc/rfc8174)) when, and only when, they appear in all capitals, as shown here.

> **Disambiguation — RFC 2119 keywords vs. modal values.** GSM defines a `DirectiveModal` enumeration whose values (`MUST`, `MUST_NOT`, `SHOULD`, `SHOULD_NOT`, `MAY`) deliberately echo deontic logic and visually resemble the RFC 2119 keywords. Throughout this document, **enumeration values are always rendered in `monospace`** and refer to data, never to the requirement level of this specification. Bare capitalized keywords in prose carry their RFC 2119 meaning.

JSON terms (object, member, array, string, number, boolean, null) are used as defined in [RFC 8259](https://www.rfc-editor.org/rfc/rfc8259). Schema terms (`$ref`, `allOf`, `$id`, `$defs`, `$dynamicAnchor`) are used as defined in [JSON Schema 2020-12](https://json-schema.org/specification). Identifiers are **UUIDv7** as defined in [RFC 9562](https://www.rfc-editor.org/rfc/rfc9562).

A **conforming implementation** is one that satisfies the requirements of [§16 Conformance](#16-conformance) for at least one conformance target.

## 4. Scope and design goals

### 4.1 In scope

- The GSM ontology: the eight primitives and the Definition/Ascription identity model.
- The DNA governance grammar (Directive, Norm, Ascription).
- The Archetype type system and two-layer typing.
- The `$gsm:*` schema-vocabulary keywords.
- The normative Ascription lifecycle (states, transitions, preconditions, cascades).
- The two expression-language profiles (Norm CEL profiles; Mechanism rule API).
- The canonical JSON interchange representation and conformance targets.

### 4.2 Out of scope

- Storage engines, indexing strategies, and database schemas (informative guidance only — see [§13](#13-persistence-informative)).
- Network protocols, transport bindings, and authentication mechanisms.
- User interfaces, visualization, and editing tools.
- The descriptive/observability counterpart of GSM (the *Descriptive System Model*), addressed by complementary standards such as OpenTelemetry.
- The academic-publication track (paper and book), which is non-normative and separate from this standard.

### 4.3 Design goals

1. **Small fixed core.** Eight primitives and one grammar. The core MUST remain small so it remains coherent and universal.
2. **Generative, not descriptive.** Primitives are active definitions, not passive records.
3. **Portability.** A governed definition MUST be expressible in a canonical, vendor-neutral form that any conforming tool can produce and consume.
4. **Machine-evaluability.** Governance assertions MUST be expressible in deterministic, sandboxable expression languages.
5. **Versioned governance.** Every governed binding MUST move through an explicit, auditable lifecycle.
6. **Extensibility without forking.** Domain vocabularies extend the core through Archetypes; they MUST NOT redefine the core classes.
7. **Composability.** Independent governance frameworks (e.g., TOGAF, ISO 25010, GDPR) MUST be able to *source into* the core and compose into one governance fabric.

## 5. Terminology

| Term | Definition |
|---|---|
| **Definition** | The stable identity of a governed subject; the referent that persists across all Ascriptions describing it. |
| **Ascription** | A governed, versioned snapshot that binds a Definition to a typed `statement` via an Archetype. |
| **Statement** | The governed JSON payload of an Ascription, validated against the typing Archetype's schema. |
| **Structure** | The foundational dynamic aggregate; constituted of Mechanisms; carries a `purpose`. |
| **Mechanism** | A logical causal unit inside a Structure: a `rule` plus its derived Effectors and Receptors. |
| **Effector / Receptor** | Output / input ports of a Mechanism, each typed by a data Archetype. |
| **Interaction** | A causal coupling from an Effector to a Receptor. |
| **Archetype** | A type definition expressed as a JSON Schema document; GSM's vehicle for static structure. |
| **Directive** | A constitutive governance act that opens a governance relationship along a viability dimension. |
| **Norm** | A measurable, machine-evaluable governance assertion operationalizing a Directive. |
| **DNA** | The three-tempo governance grammar: **D**irective (slow), **N**orm (medium), **A**scription (fast). |
| **Qualifier** | An Archetype referenced by a Directive/Norm that names the viability dimension being governed. |
| **In-effect** | An Ascription whose status is `ACTIVE` or `DEPRECATED`. |
| **GSM Processor** | An implementation that authors, validates, and governs Ascriptions against this specification. |
| **GSM Document** | A canonical-JSON serialization of one or more Definitions with their Ascriptions. |

## 6. Core model: Definition and Ascription

GSM separates **identity** from **governed content**.

### 6.1 Definition

A **Definition** is the stable identity of a governed subject. It answers *what thing is being governed*.

- A Definition **MUST** carry an `id` (UUIDv7) and a `subjectType`.
- `subjectType` **MUST** be one of the sealed `DefinitionSubjectType` values (§6.3). It **MUST NOT** change after creation.
- A Definition **MUST NOT** be modified once created; evolution occurs by attaching new Ascriptions.
- At most **one** in-effect Ascription **MUST** exist per Definition at any time.

### 6.2 Ascription

An **Ascription** is the governed snapshot — a versionable, lifecycle-managed binding of a Definition to a typed `statement`.

An Ascription **MUST** carry the following envelope members:

| Member | Type | Requirement |
|---|---|---|
| `id` | UUIDv7 string | REQUIRED. Identity of this snapshot. |
| `definitionId` | UUIDv7 string | REQUIRED. References the owning Definition. |
| `archetypeId` | UUIDv7 string | REQUIRED. References the **typing** Archetype's Definition (§9). |
| `version` | integer ≥ 1 | Assigned at the `APPROVE` transition; monotonically increasing per Definition. Absent before approval. |
| `status` | enum | REQUIRED. One of the lifecycle statuses (§12). |
| `statement` | object | REQUIRED. The governed payload, valid against the typing Archetype's schema. |
| `timestamp` | RFC 3339 string | REQUIRED. Authoring/ordering timestamp. |

**Identity invariant.** Properties annotated `$gsm:identityBound` (§11) in the typing Archetype **MUST NOT** change their value across Ascriptions of the same Definition. Changing an identity-bound value **MUST** be modeled as a new Definition, not a new version.

**Typing reference.** `archetypeId` **MUST** resolve to a *structural* Archetype whose `allOf` chain converges to exactly one GSM base (§9.2). The resolved base **MUST** match the Definition's `subjectType`.

### 6.3 `DefinitionSubjectType` (sealed enumeration)

```
STRUCTURE | MECHANISM | EFFECTOR | RECEPTOR | INTERACTION | ARCHETYPE | DIRECTIVE | NORM
```

This enumeration is **sealed**: conforming implementations **MUST NOT** add, remove, or rename values. Domain-level typing extensibility is provided exclusively through Archetypes (§9), never by extending this enumeration.

## 7. The eight primitives

GSM defines exactly eight structural classes. Six are *structural/meta* (Structure, Mechanism, Effector, Receptor, Interaction, Archetype) and two are *governance DNA* (Directive, Norm). All eight extend Ascription and therefore share the identity model, the typing surface, and the lifecycle.

> **"System" is not a primitive.** Whether a Structure *is a system* is a derived, contextual judgment — a Structure becomes a System when its Mechanisms become reflexive (their rules target the owning Structure's own Elements, producing governance DNA). Conforming implementations **MUST NOT** model "System" as a stored class; systemness, if surfaced, **MUST** be derived (§15).

### 7.1 Structure

The foundational dynamic aggregate, constituted of Mechanisms.

- `purpose` (string) — REQUIRED, `$gsm:identityBound`, and globally unique among in-effect Structures. It is both the Structure's identity and its governance address. It **MUST** be kebab-case.
- A Structure **MUST NOT** contain sub-Structures. There is no containment hierarchy; inter-Structure governance is expressed only through DNA (§8).

### 7.2 Mechanism

The logical causal unit inside a Structure.

- `structure` (reference) — REQUIRED, identity-bound. The owning Structure.
- `function` (string) — REQUIRED, identity-bound, unique within the owning Structure among in-effect Mechanisms.
- `rule` (string) — REQUIRED. Source in the Mechanism rule language (§14.2).
- Effectors and Receptors **MUST** be auto-derived by the processor from the rule's abstract syntax tree (§14.2.2). Implementations **MUST NOT** require ports to be authored independently of the rule.

### 7.3 Effector

An output port of a Mechanism.

- `mechanism` (reference) — REQUIRED, identity-bound.
- `archetype` (reference) — REQUIRED, identity-bound. The **data** Archetype typing the emitted payload.

### 7.4 Receptor

An input port of a Mechanism.

- `mechanism` (reference) — REQUIRED, identity-bound.
- `archetype` (reference) — REQUIRED, identity-bound. The **data** Archetype typing the consumed payload.

### 7.5 Interaction

The single primitive relation in GSM: a directed causal coupling.

- `effector` (reference) — REQUIRED, identity-bound. The emitting Effector.
- `receptor` (reference) — REQUIRED, identity-bound. The receiving Receptor.
- The emitting Effector's data Archetype and the receiving Receptor's data Archetype **MUST** be schema-compatible.
- An Interaction **MAY** couple ports of different Structures (inter-Structure causality).

### 7.6 Archetype

A type definition; GSM's vehicle for static structure. Normatively specified in [§9](#9-archetype-type-system).

### 7.7 Directive and Norm

Governance DNA; normatively specified in [§8](#8-dna-governance-grammar).

## 8. DNA governance grammar

Governance is expressed in three primitives stratified by rate of change:

| Layer | Primitive | Tempo | Role |
|---|---|---|---|
| **D** | Directive | Slow (identity) | Opens a governance relationship along a viability dimension. |
| **N** | Norm | Medium (constraints) | Operationalizes a Directive into a measurable assertion. |
| **A** | Ascription | Fast (binding) | The governed snapshot itself (§6). |

### 8.1 Directive

A Directive is a **constitutive act**: it does not merely constrain — it *opens* a governance relationship between a governing Structure and a governed Structure along a viability dimension.

A Directive `statement` **MUST** contain:

| Field | Type | Requirement |
|---|---|---|
| `structure` | reference | REQUIRED, identity-bound. The **governing** Structure (the governor, accountable). |
| `modal` | `DirectiveModal` | REQUIRED. One of `MUST`, `MUST_NOT`, `SHOULD`, `SHOULD_NOT`, `MAY`. |
| `verb` | `DirectiveVerb` | REQUIRED. One of `ENSURE`, `PREVENT`, `MAINTAIN`, `OPTIMIZE`, `MINIMIZE`, `MAXIMIZE`, `MONITOR`, `ENABLE`. |
| `qualifier` | reference | REQUIRED, identity-bound. The Archetype naming the viability dimension. |
| `purpose` | string | REQUIRED, identity-bound, kebab-case. Names the **governed** Structure. |

**Natural grammar:** `{structure} {modal} {verb} {qualifier} ON {purpose}` — e.g., *"order-processing `MUST` `ENSURE` SecurityProperties ON payment-service"*.

- `purpose` is a **string, not a reference**, to permit **anticipatory governance**: a Directive **MAY** name a `purpose` for which no Structure yet exists. Implementations **MUST NOT** perform referential-integrity checks on `purpose`.
- **Chain linkage:** the governed Structure named by `Directive.purpose` is the same Structure that bears the operationalizing Norms, i.e. `Directive.purpose == Norm.structure.purpose`.
- **Scope:** a Directive opens governance over the **internal Elements** (Mechanisms, Effectors, Receptors, Interactions) of the governed Structure only. Scope **MUST NOT** be accumulated across Directives nor inherited upward. Transitive governance is achieved through chains of distinct Directives.

#### 8.1.1 Conflict semantics

- A positive modal and its negation on the same `verb` + `qualifier` + `purpose` (e.g., `MUST` + `MUST_NOT`) is a **contradiction** and **MUST** be rejected.
- `ENSURE` and `PREVENT` on the same `qualifier` + `purpose` is a contradiction and **MUST** be rejected.
- Opposing verb directions on the same `qualifier` + `purpose` **MUST** be resolved by modal precedence: `MUST`/`MUST_NOT` > `SHOULD`/`SHOULD_NOT` > `MAY`.
- Different `purpose` values on the same `qualifier` are **NOT** conflicts.

### 8.2 Norm

A Norm operationalizes a Directive into a measurable assertion within the scope the Directive opened.

A Norm `statement` **MUST** contain:

| Field | Type | Requirement |
|---|---|---|
| `structure` | reference | REQUIRED, identity-bound. The **governed** Structure (= `Directive.purpose`). |
| `qualifier` | reference | REQUIRED, identity-bound. The dimension being constrained (mirrors the Directive's `qualifier`). |
| `applicability` | CEL string | OPTIONAL, default `"true"`. Activation condition (§14.1.1). |
| `assertion` | CEL string | REQUIRED, identity-bound. Compliance condition returning boolean (§14.1.2). |
| `toleranceMode` | `NormToleranceMode` | REQUIRED. One of `INSTANTANEOUS`, `AGGREGATED`, `SUSTAINED`. |
| `temporalWindow` | ISO 8601 duration | REQUIRED iff `toleranceMode` ∈ {`AGGREGATED`, `SUSTAINED`}. |
| `temporalAggregation` | `NormAggregationType` | REQUIRED iff `toleranceMode` ∈ {`AGGREGATED`, `SUSTAINED`}. One of `SUM`, `AVG`, `MIN`, `MAX`, `COUNT`, `P50`, `P90`, `P95`, `P99`. |
| `sustainedThreshold` | number in [0,1] | REQUIRED iff `toleranceMode` = `SUSTAINED`. |

**Natural grammar:** `{structure} ON {qualifier}: WHEN {applicability} ASSERT {assertion} ({toleranceMode})`.

- A processor **MUST** validate that a Norm's `qualifier` matches (or descends from) the `qualifier` of a Directive whose `purpose` equals the Norm's governed Structure. A Norm with no opening Directive **MUST** be rejected: there are no floating constraints.

### 8.3 Quality as viability (informative)

Quality is **not** a primitive. It is a derived proxy for viability — the balance between the value a Structure produces and the risk/cost of governing it. It is expressed through Norms targeting qualifier Archetypes that represent viability dimensions, and observed through the descriptive plane.

## 9. Archetype type system

### 9.1 Two-layer typing

GSM uses two layers:

1. **Fixed class hierarchy** — the eight `DefinitionSubjectType` values (§6.3). Sealed; never extended by tenants.
2. **Definition + Archetype + Ascription** — the extensible domain vocabulary. Tenant Archetypes extend GSM base schemas to add domain-specific properties.

An **Archetype** is an Ascription whose `statement` **IS** a JSON Schema document (draft 2020-12 or later).

- The schema's `title` **MUST** be present; it is the Archetype's human-readable identity, is `$gsm:identityBound`, **MUST** be globally unique among in-effect Archetypes, and is the root identifier used in Norm CEL expressions (§14.1).
- The processor **MUST** assign the schema `$id` as `gsmarc://gsm/{title}/v{version}`. Producers **MUST NOT** rely on a tenant-supplied `$id`.

### 9.2 Based vs. rootless Archetypes

- A **based** (structural) Archetype declares an `allOf` chain that converges to **exactly one** GSM base schema. Its `DefinitionSubjectType` is determined by that base. Only based Archetypes **MAY** be used as `archetypeId` (the typing position).
- A **rootless** Archetype declares no converging GSM base. Rootless Archetypes are valid first-class Archetypes but **MUST NOT** be used as `archetypeId`. They **MAY** be used in the qualifier and data roles (§9.3).
- A single Archetype schema **MAY** mix structural and rootless `allOf` entries. The convergence rule applies only to the subset of paths reaching a GSM base; **exactly one** structural base **MUST** be found among them.

### 9.3 The Archetype triple role

A single Archetype **instance** (one Definition, one schema, one lifecycle) **MAY** simultaneously occupy three relational positions:

| Role | FK position | Structural base required? |
|---|---|---|
| **Typing** | `archetypeId` on every Ascription | Yes |
| **Qualifier** | `qualifier` on Directive / Norm | No (rootless permitted) |
| **Data** | `archetype` on Effector / Receptor | No (rootless permitted) |

Implementations **MUST** represent these roles with a single Archetype construct. Splitting the construct per role is non-conforming, because it would duplicate one governed schema into divergent copies.

### 9.4 Autopoietic bootstrap

The type system is seeded by a single self-typing Archetype — the **seed Archetype** — whose `archetypeId` references its own `id`. This is the single axiomatic seed from which the entire type system grows. A conforming processor **MUST** bundle the seed Archetype (the GSM meta-schema) and **MUST** validate every incoming Archetype `statement` against it.

## 10. GSM base Archetype schemas

A conforming processor **MUST** provide GSM base Archetype schemas for the seven extensible classes — Structure, Mechanism, Effector, Receptor, Interaction, Directive, Norm — plus the **sealed** Archetype meta-schema. All seven extensible bases **MAY** be extended by tenant Archetypes via `allOf`. The Archetype meta-schema carries `$gsm:sealed` and **MUST NOT** be tenant-extended.

The identity-bound fields per base are:

| Base | Identity-bound fields |
|---|---|
| Structure | `purpose` |
| Mechanism | `structure`, `function` |
| Effector | `mechanism`, `archetype` |
| Receptor | `mechanism`, `archetype` |
| Interaction | `effector`, `receptor` |
| Directive | `structure`, `qualifier`, `purpose` |
| Norm | `structure`, `qualifier`, `assertion` |

## 11. Schema vocabulary (`$gsm:*`)

Archetype schemas carry GSM governance instructions as custom JSON Schema vocabulary keywords. The vocabulary is **sealed**: a processor **MUST** reject unknown `$gsm:*` keywords. Keyword validation **MUST** apply at every schema node (the meta-schema uses `$dynamicAnchor: "meta"`), not only at the top level.

| Keyword | Scope | Processor behavior |
|---|---|---|
| `$gsm:sealed` | Archetype (top-level) | Reject any tenant `allOf` extension of this schema. |
| `$gsm:identityBound` | Property (cross-version) | Reject an Ascription if the value differs from the first Ascription of the Definition. |
| `$gsm:queryable` | Property (query) | Provision an efficient query index for the property. Property type MUST be scalar or array-of-scalar. |
| `$gsm:unique` | Property (cross-Definition) | Enforce value uniqueness among in-effect Ascriptions of the same Archetype. |
| `$gsm:dataProtection` | Property (protection) | Apply a phase-first protection model: `atRest` and `inTransit`, each declaring at most one of `encryption`, `hash`, `mask`, `suppression`. |

**Mutual exclusion.** `$gsm:dataProtection` with `atRest.encryption` and `$gsm:queryable` on the same property **MUST** be rejected (ciphertext is not indexable). `$gsm:queryable` with `atRest.hash` is permitted.

**Immutability.** The set of properties marked `$gsm:identityBound` is itself immutable per Archetype Definition; changing it **MUST** require a new Archetype Definition.

## 12. Ascription lifecycle

Every Ascription moves through a normative state machine of **nine statuses** and **twelve transitions**. A conforming processor **MUST** implement this state machine exactly and **MUST NOT** permit transitions not listed here.

### 12.1 Statuses

| Category | Statuses | In-effect |
|---|---|---|
| Authoring | `DRAFT` | No |
| Review | `PROPOSED` | No |
| Approved | `APPROVED` (version assigned) | No |
| In-effect | `ACTIVE`, `DEPRECATED` | **Yes** |
| Suspended | `SUSPENDED` | No |
| Terminal | `RETIRED`, `ABANDONED`, `REJECTED` | No (permanent) |

### 12.2 Transitions

```
CREATE:                 ∅          → DRAFT
SUBMIT:                 DRAFT      → PROPOSED
APPROVE:                PROPOSED   → APPROVED      (assigns version)
ACTIVATE:               APPROVED   → ACTIVE
SUSPEND:                ACTIVE     → SUSPENDED
RESTORE:                SUSPENDED  → ACTIVE
DEPRECATE:              ACTIVE     → DEPRECATED
DEPRECATE:              SUSPENDED  → DEPRECATED
SUSPEND:                DEPRECATED → SUSPENDED
RETIRE:                 DEPRECATED → RETIRED
ABANDON:                DRAFT      → ABANDONED
REJECT:                 PROPOSED   → REJECTED
```

Terminal statuses (`RETIRED`, `ABANDONED`, `REJECTED`) are permanent; no further transition is permitted. Each transition **MUST** be recorded as an immutable audit entry (pre-status, post-status, timestamp).

### 12.3 Intra-Definition rules

- **Approval convergence.** A new Ascription **MUST NOT** be moved to `APPROVED` while another Ascription of the same Definition is already `APPROVED`.
- **Activation handoff.** When a new Ascription reaches `ACTIVE`, the previously `ACTIVE` Ascription of the same Definition **MUST** auto-transition to `DEPRECATED`.
- **Version assignment.** `version` **MUST** be assigned at the `APPROVE` transition and **MUST** be monotonically increasing per Definition.

### 12.4 Referee preconditions

A **reference** is any identity-bound field of one Ascription that points to another Definition's Ascription (e.g., `Mechanism.structure`). Before any transition, every reference **MUST** satisfy the precondition for the target status:

| Target status | Each reference MUST be in |
|---|---|
| `DRAFT` | any non-terminal |
| `PROPOSED` | any non-terminal |
| `APPROVED` | `APPROVED`, `ACTIVE`, `DEPRECATED` |
| `ACTIVE` | `ACTIVE`, `DEPRECATED` |
| `SUSPENDED` | `SUSPENDED`, `ACTIVE`, `DEPRECATED` |
| `DEPRECATED` | `ACTIVE`, `DEPRECATED` |

A Directive's `purpose` is a string, not a reference, and is therefore **exempt** from referee preconditions.

### 12.5 Cascades

| Cascade | Relationship | Scope | On failure |
|---|---|---|---|
| **Constitutive** | Mechanism → Effector / Receptor | All transitions | The source transition **MUST** be rejected (blocks). |
| **Governing** | Structure → Mechanism / Directive / Norm | All transitions | No-op for the failing target. |
| **Dependent** | Effector / Receptor → Interaction | Degradation + terminal only | No-op for the failing target. |

## 13. Persistence (informative)

GSM does not mandate a storage technology. The reference implementation materializes identity-bound references and `$gsm:*`-driven indexes from the `statement` for referential integrity and efficient reverse lookups, while keeping the `statement` as the source of truth. These are implementation concerns and are **not** required for conformance. Identifiers SHOULD be UUIDv7 so they are time-sortable.

## 14. Expression languages

GSM specifies two machine-evaluable expression surfaces. Both **MUST** be deterministic and side-effect-free.

### 14.1 Norm expressions — Common Expression Language (CEL)

Norm `applicability` and `assertion` **MUST** be expressed in [CEL](https://github.com/google/cel-spec). Two profiles apply.

#### 14.1.1 Applicability profile (restricted)

Returns boolean; default `"true"`. **Axis-predicate normal form:** `{ArchetypeTitle}.{property} {operator} {literal}`.

- The root identifier **MUST** be the qualifier Archetype's `title`.
- Operators are limited to `==`, `!=`, `<`, `<=`, `>`, `>=`, `in`, `matches`.
- Conjunction (`&&`) across axes is permitted; disjunction (`||`) is permitted **within a single axis** only.
- Function calls, compound-expression negation, and variable bindings **MUST NOT** be used.

#### 14.1.2 Assertion profile (permissive)

Returns boolean. References exactly one Archetype by `title` as the root identifier. Full CEL boolean expressions (arithmetic, string, list operations) are permitted. Side effects **MUST NOT** occur.

### 14.2 Mechanism rules

A Mechanism `rule` **MUST** be expressed in [Starlark](https://github.com/bazelbuild/starlark) and **MUST** execute in a sandbox. The `sys` namespace is the single API surface.

#### 14.2.1 Core API

```python
# Event trigger — MUST be the first executable statement
payload = sys.receive("ArchetypeTitle")
payload = sys.receive("ArchetypeTitle").on("ReceptorPortArchetype")

# Open-loop output (fire-and-forget)
sys.effect("ArchetypeTitle", data)
sys.effect("ArchetypeTitle", data).by("EffectorPortArchetype")

# Closed-loop output (with feedback)
response = sys.effect("D", data).receive("F")
response = sys.effect("D", data).by("E").receive("F").on("R")

# Chain order: effect → [by] → [receive → [on]]
```

#### 14.2.2 Auto-derivation

The processor **MUST** derive ports from the rule AST:

- `sys.receive("X")` derives a Receptor typed by Archetype `X`.
- `sys.effect("Y", data)` derives an Effector typed by Archetype `Y`.
- `.receive("Z")` on an effect derives a feedback Receptor typed by `Z`.

When deriving, the processor **MUST** match on the triple (Mechanism Definition, data Archetype, direction) and reuse the existing port Definition if one exists, preserving referential stability for Interactions.

#### 14.2.3 Sandbox constraints

- The only globals are `sys`, the injected host functions, and Starlark built-ins.
- `load()` statements **MUST NOT** be permitted.
- Every Archetype-name argument **MUST** be a string literal (to enable static analysis and port derivation).
- Execution **MUST** be bounded by a configurable step budget.

Injected host functions: `now()` (RFC 3339 timestamp), `uuid7()` (UUIDv7 string), `fullmatch(pattern, string)`, `search(pattern, string)`.

## 15. Eliminated constructs

To preserve coherence, GSM deliberately does **not** model the following. Conforming implementations **MUST NOT** reintroduce them as stored primitives:

| Eliminated | GSM alternative |
|---|---|
| **System** (class) | Derived from reflexive Mechanisms at inference time. |
| **Channel** | Interaction + Norms on the Interaction. |
| **Composition / Layers** | Flat Structures governed through DNA; cascades (§12.5) carry lifecycle coupling. |
| **VSM nested sub-systems** | Governance chains via `Directive.purpose`. |
| **Relationship / Role** | Derived from chains of Interactions. |
| **Interface** | Effectors and Receptors are the interface. |

## 16. Conformance

This specification defines four conformance targets. An implementation MAY claim conformance to one or more. The [Conformance](conformance.md) catalog restates these requirements as individually-citable, ID-stable assertions for test suites and audits.

### 16.1 GSM Document

A document is a **conforming GSM Document** if and only if:

1. It is well-formed JSON using the canonical interchange representation of [§18](#18-canonical-json-interchange).
2. Every Definition carries a valid `id` and a sealed `subjectType`.
3. Every Ascription carries the required envelope members (§6.2) and a `status` drawn from §12.1.
4. Every `statement` validates against the schema of its typing Archetype.
5. All identity-bound values are consistent across Ascriptions of the same Definition.

### 16.2 GSM Producer

A **conforming Producer** emits only conforming GSM Documents and:

1. **MUST** assign Archetype `$id` as `gsmarc://gsm/{title}/v{version}`.
2. **MUST NOT** emit rootless Archetypes in the `archetypeId` position.
3. **MUST** preserve identity-bound values across versions.

### 16.3 GSM Consumer

A **conforming Consumer** ingests conforming GSM Documents and:

1. **MUST** reject documents that violate §16.1.
2. **MUST** treat the `statement` as authoritative and resolve references by `definitionId`/`archetypeId`.
3. **MUST NOT** require non-standard extensions to interpret the core model.

### 16.4 GSM Processor

A **conforming Processor** authors and governs Ascriptions and:

1. **MUST** implement the lifecycle state machine of §12 exactly, including referee preconditions (§12.4) and cascades (§12.5).
2. **MUST** enforce the two-layer typing rules of §9, including base convergence and the seed Archetype validation (§9.4).
3. **MUST** enforce the `$gsm:*` vocabulary of §11, including sealed-keyword rejection and the mutual-exclusion rule.
4. **MUST** auto-derive Mechanism ports from rules (§14.2.2) and **MUST NOT** accept independently authored ports.
5. **MUST** evaluate Norm expressions under the CEL profiles of §14.1 and execute Mechanism rules under the sandbox of §14.2.3.
6. **MUST** reject Directive/Norm conflicts and floating Norms (§8).
7. **MUST** record an immutable audit entry for every transition.

### 16.5 Extension rules

Implementations **MAY** define domain Archetypes that extend GSM bases via `allOf`. Implementations **MUST NOT**:

- add, remove, or rename `DefinitionSubjectType` values;
- add `$gsm:*` vocabulary keywords;
- introduce stored primitives eliminated in §15;
- alter the lifecycle state machine.

A non-normative conformance checklist is provided in [Appendix A](#appendix-a-conformance-checklist-informative).

## 17. Relationship to other work

### 17.1 Within the cloud-native ecosystem (informative)

- **OpenTelemetry** standardizes the RUN side (telemetry). GSM standardizes the THINK side (definition/governance). They are complementary: observation produced under OpenTelemetry is the descriptive evidence a Norm is evaluated against.
- **CloudEvents** standardizes event interchange. A GSM Effector's emitted payload **MAY** be carried as a CloudEvent; the Effector's data Archetype types the CloudEvent `data`.
- **OpenFeature / SPIFFE** address flagging and workload identity; GSM governs the *definitions* that such runtime concerns realize.

### 17.2 Relationship to prior systemic models (informative)

GSM inherits viability, feedback, and requisite-variety insights from Beer's VSM, von Bertalanffy's GST, and Wiener/Ashby cybernetics, but inverts their **descriptive** stance into a **generative** one and replaces nested recursion with flat Structures governed through DNA.

## 18. Canonical JSON interchange

A GSM Document is a JSON object with a `definitions` array. Each entry pairs a Definition with its Ascriptions.

```json
{
  "gsm": "0.1",
  "definitions": [
    {
      "id": "0190a5e2-1c00-7aaa-8000-000000000001",
      "subjectType": "STRUCTURE",
      "ascriptions": [
        {
          "id": "0190a5e2-1c00-7aaa-8000-0000000000a1",
          "definitionId": "0190a5e2-1c00-7aaa-8000-000000000001",
          "archetypeId": "0190a5e2-1c00-7aaa-8000-0000000000ff",
          "version": 1,
          "status": "ACTIVE",
          "timestamp": "2026-06-23T10:00:00Z",
          "statement": { "purpose": "payment-service" }
        }
      ]
    },
    {
      "id": "0190a5e2-1c00-7aaa-8000-000000000002",
      "subjectType": "DIRECTIVE",
      "ascriptions": [
        {
          "id": "0190a5e2-1c00-7aaa-8000-0000000000b1",
          "definitionId": "0190a5e2-1c00-7aaa-8000-000000000002",
          "archetypeId": "0190a5e2-1c00-7aaa-8000-0000000000fe",
          "version": 1,
          "status": "ACTIVE",
          "timestamp": "2026-06-23T10:01:00Z",
          "statement": {
            "structure": "0190a5e2-1c00-7aaa-8000-000000000003",
            "modal": "MUST",
            "verb": "ENSURE",
            "qualifier": "0190a5e2-1c00-7aaa-8000-0000000000fd",
            "purpose": "payment-service"
          }
        }
      ]
    },
    {
      "id": "0190a5e2-1c00-7aaa-8000-000000000004",
      "subjectType": "NORM",
      "ascriptions": [
        {
          "id": "0190a5e2-1c00-7aaa-8000-0000000000c1",
          "definitionId": "0190a5e2-1c00-7aaa-8000-000000000004",
          "archetypeId": "0190a5e2-1c00-7aaa-8000-0000000000fc",
          "version": 1,
          "status": "ACTIVE",
          "timestamp": "2026-06-23T10:02:00Z",
          "statement": {
            "structure": "0190a5e2-1c00-7aaa-8000-000000000001",
            "qualifier": "0190a5e2-1c00-7aaa-8000-0000000000fd",
            "applicability": "SecurityProperties.exposure == \"external\"",
            "assertion": "SecurityProperties.mtls == true",
            "toleranceMode": "INSTANTANEOUS"
          }
        }
      ]
    }
  ]
}
```

- The top-level `gsm` member **MUST** carry the specification version the document targets.
- `subjectType` and `status` **MUST** use the exact enumeration spellings of §6.3 and §12.1.
- A document **MAY** contain a single in-effect Ascription per Definition, or a lineage; consumers **MUST** select the in-effect Ascription (`ACTIVE`, else `DEPRECATED`) when one is required.

## 19. Security considerations

- **Schema-resolution invariant.** A processor **MUST NOT** resolve `$schema` URIs supplied in tenant Archetype schemas for validation. It **MUST** validate against its own bundled seed Archetype. A tenant-declared `$schema` is a declaration, not an enforcement vector.
- **Rule sandboxing.** Mechanism rules **MUST** execute under the constraints of §14.2.3 (no `load()`, literal-only Archetype arguments, bounded steps). CEL expressions **MUST** be side-effect-free.
- **Data protection.** `$gsm:dataProtection` governs at-rest and in-transit treatment; protected fields **MUST NOT** be indexed in cleartext (§11). Reads of protected fields SHOULD emit a governance audit event.
- **Integrity.** Identity-bound values and terminal statuses are immutable; processors **MUST** reject mutations that violate these invariants.
- **Anticipatory governance.** Because `Directive.purpose` is unverified by design, consumers **MUST NOT** treat the existence of a Directive as proof that the governed Structure exists.

## 20. Versioning and governance

- This specification uses semantic versioning. Pre-1.0 drafts MAY introduce breaking changes; from 1.0, breaking changes **MUST** increment the major version.
- The canonical model artifacts (the eight base Archetype schemas and the seed meta-schema) are versioned with the specification.
- **IP and licensing (proposed).** For CNCF contribution, the intended posture is: specification text under **CC BY 4.0**, and reference schemas/implementations under **Apache-2.0**, governed by the CNCF IP Policy. This posture is a proposal of the originating maintainer and is **not** a completed licensing change; the repository's current licenses remain authoritative until formally updated.
- **Change process (proposed).** Post-contribution, changes would proceed by public proposal and review under a CNCF project governance charter. No such charter is in force at the time of this draft.

## 21. References

### 21.1 Normative

- [RFC 2119] — Key words for use in RFCs.
- [RFC 8174] — Ambiguity of uppercase vs lowercase in RFC 2119 key words.
- [RFC 8259] — The JavaScript Object Notation (JSON) Data Interchange Format.
- [RFC 9562] — Universally Unique IDentifiers (UUIDs), including UUIDv7.
- [JSON Schema 2020-12] — JSON Schema core and validation.
- [CEL] — Common Expression Language specification.
- [Starlark] — Starlark language specification.

### 21.2 Informative

- [Manifesto](manifesto.md) — The GSM Manifesto (design intent and principles).
- [SIE](https://docs.poesis.cloud/sie-definition/) — Reference implementation overview.
- OpenTelemetry; CloudEvents — complementary CNCF specifications.

---

## Appendix A. Conformance checklist (informative)

A conforming **Processor** should be able to answer "yes" to each:

- [ ] Are the eight `DefinitionSubjectType` values implemented exactly, with no additions?
- [ ] Is Definition identity immutable, with at most one in-effect Ascription per Definition?
- [ ] Are identity-bound values enforced across versions?
- [ ] Is the nine-status, twelve-transition lifecycle implemented with no extra edges?
- [ ] Are referee preconditions enforced before every transition?
- [ ] Are constitutive / governing / dependent cascades implemented per §12.5?
- [ ] Is `archetypeId` rejected when it resolves to a rootless Archetype?
- [ ] Is the seed Archetype bundled and used to validate incoming Archetypes?
- [ ] Are unknown `$gsm:*` keywords rejected?
- [ ] Is `$gsm:queryable` + `atRest.encryption` rejected on the same property?
- [ ] Are Mechanism ports auto-derived from rules and never authored independently?
- [ ] Are Norm CEL profiles and the Starlark sandbox enforced?
- [ ] Are Directive/Norm conflicts and floating Norms rejected?
- [ ] Is every transition recorded as an immutable audit entry?

## Appendix B. Glossary cross-reference (informative)

| GSM term | Nearest classical analogue | Note |
|---|---|---|
| Structure | VSM "system" / GST "open system" | GSM Structures are flat; systemness is derived. |
| Directive | Deontic obligation; VSM System 5 policy | Constitutive scope-opener, not a mere constraint. |
| Norm | Policy / compliance rule | Machine-evaluable assertion, lifecycle-versioned. |
| Ascription | Versioned configuration/binding | The governed snapshot; the unit of change. |
| Archetype | Type / schema / metamodel | One construct, three relational roles. |
| Interaction | Cybernetic channel / coupling | The single primitive relation. |
