# KYE™ ID Format

URN identifier format for [KYE Protocol™](https://github.com/KYE-Protocol). Defines stable, immutable identifiers for entities, delegations, scopes, credentials, attestations, policies, decisions, runtime events, audit events, proof bundles, signals, transfers, trust domains, issuers, and transparency services.

The detailed spec is in [`SPEC.md`](SPEC.md).

## At a glance

```
kye:<class>:<trust-domain>:<subclass>:<local-part>
```

Examples:

```
kye:ent:acme.example:ai_agent:01JY3J1D4E5A7K3JQFK4E0Q1XZ
kye:del:acme.example:01JYABC0000000000000000000
kye:scp:acme.example:01JYFINANCE000000000000000
kye:td:acme.example
```

Identifiers are:

- **stable** — once minted, the string never changes
- **opaque** — consumers must not parse the local part for meaning
- **trust-domain-scoped** — the issuing trust domain is visible
- **classified** — the identifier class is visible

The mechanisms that ensure stability (minting authority, append-only history, tombstoning) are part of the normative specification, not this repository.

## License

Apache License 2.0 — see [`LICENSE`](LICENSE).

KYE™, KYE Protocol™, KYE Passport™, KYE Gateway™, KYE Payments™, and KYE Certified™ are trademarks of the KYE Protocol™ maintainers. The Apache 2.0 license does not grant trademark rights.

## Patent notice

KYE Protocol™ is the subject of pending patent applications. This repository deliberately publishes only the **identifier format**. It does not publish the algorithms used to mint, tombstone, transfer, or chain identifiers. See the [org profile](https://github.com/KYE-Protocol) for the full patent notice.
