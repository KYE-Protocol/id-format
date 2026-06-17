# KYE™ ID Format

KYE™ entities are addressed by stable, immutable identifiers. This document defines the **format** of those identifiers.

The rules for how identifiers are minted, tombstoned, recovered after revocation, or carried across trust domains are part of the normative specification.

## Canonical form

A KYE identifier is a URN of the form:

```
kye:<class>:<trust-domain>:<subclass>:<local-part>
```

| Segment | Description |
|---|---|
| `kye` | Constant scheme prefix |
| `<class>` | Identifier class — see below |
| `<trust-domain>` | The trust domain that minted the identifier |
| `<subclass>` | Class-specific subtype (for example, the entity type) |
| `<local-part>` | Locally unique opaque identifier |

The `local-part` is opaque to consumers. Common practice is to use ULID, UUIDv7, or a similar lexicographically sortable identifier; the format does not require any particular generator.

## Identifier classes

### Hierarchy classes — the tenant tree (v3-final, locked 2026-05-14)

These classes form the parent-required containment tree. See `public/vocabulary/entity-types.md` §Hierarchy.

**Tenant-level classes (5):**

| Class | Meaning |
|---|---|
| `tenant`  | Tenant — billing + isolation root (e.g. `kye:tenant:acme`) |
| `lglent`  | Legal Entity — registered legal person under a Tenant |
| `bill`    | Billing Account — Stripe customer + payment method under a Tenant |
| `dom`     | Domain — verified DNS domain owned by the Tenant |
| `pol`     | Policy Bundle — active Operating Model™ / rule bundles under a Tenant |

**Workspace-level classes (9):**

| Class | Meaning |
|---|---|
| `wsp`       | Workspace — environment / dataspace under a Tenant |
| `prj`       | Project — unit of work under a Workspace |
| `team`      | Team — RBAC + ownership group under a Workspace |
| `res`       | Resource — owned D1 table / R2 bucket / queue / connector under a Workspace |
| `prin`      | Principal — the acting party (Human / System / Agent) under a Workspace |
| `model`     | Model — versioned inference artefact under a Workspace (v3 — first-class) |
| `tool`      | Tool — named callable with declared side-effects under a Workspace (v3 — first-class) |
| `extapp`    | External App — 3rd-party integration or partner connector (v3 — first-class) |
| `audstream` | Audit Stream — append-only event-log destination under a Workspace (v3 — new) |

**State machine class:**

| Class | Meaning |
|---|---|
| `sm` | State Machine — registered machine governing an entity class's lifecycle |

In total the v3 hierarchy has **14 entity classes** across the tree,
plus `sm` for state machine records. The `prin` class in v3 covers
only `human`, `system`, and `agent` subtypes.

### Operational classes

| Class | Meaning |
|---|---|
| `ent` | Entity (generic; legacy + cross-class references) |
| `idn` | Identifier (e.g., email, domain, account reference) |
| `rel` | Relationship edge |
| `del` | Delegation |
| `acr` | Access right |
| `scp` | Scope |
| `cred` | Credential |
| `att` | Attestation |
| `pdc` | Policy decision |
| `rte` | Runtime event |
| `aud` | Audit event |
| `prf` | Proof bundle |
| `trn` | Transfer |
| `rev` | Revocation |
| `sig` | Signal |
| `td`  | Trust domain |
| `iss` | Issuer |
| `ts`  | Transparency service |

Profile-specific classes (for example, payment authorities) are defined in their respective profiles.

## Examples

```
kye:ent:acme.example:ai_agent:01JY3J1D4E5A7K3JQFK4E0Q1XZ
kye:del:acme.example:01JYABC0000000000000000000
kye:scp:acme.example:01JYFINANCE000000000000000
kye:cred:acme.example:verifiable_credential:01JYCRED0000000000000000
kye:td:acme.example
```

## Properties

The format is designed so that identifiers are:

- **stable** — once minted, the string never changes
- **opaque** — consumers must not parse the local part for meaning
- **trust-domain-scoped** — the issuing trust domain is visible
- **classified** — the identifier class is visible
- **lexicographically comparable** — when ULID/UUIDv7 are used for the local part

The mechanisms that ensure stability (minting authority, append-only history, tombstoning rules) are part of the normative specification.

## Comparison and equality

Two KYE identifiers are equal iff their byte sequences are equal after the following normalization:

- the `kye` prefix is compared case-insensitively
- all other segments are compared case-sensitively
- whitespace is not permitted in any segment
- percent-encoding is not used

## What this document does not specify

This document deliberately does not specify:

- the algorithm used to mint local parts
- the audit-chain construction that ties identifiers to registration events
- the rules for transferring identifiers across trust domains
- the rules for tombstoning and successor binding

Those are part of the normative specification and the mechanism designs.
