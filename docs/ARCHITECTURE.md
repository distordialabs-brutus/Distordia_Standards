# Standards architecture and implementation contract

Reviewed 2026-09-10 at pre-publication local/remote HEAD `e8f890e3f02e6aa1fda6a1af76ef3ac0df5c60bc`. See [review](DEVELOPMENT_REVIEW_2026-09-10.md) and [development plan](DEVELOPMENT_PLAN.md).

## Scope

This repository contains specifications and design notes, not a deployed Nexus validator, a Solana Anchor program, or an application conformance gate. Draft v0.2.0 schemas are target contracts. Existing unversioned standards and consuming applications must not be silently relabeled v0.2.0. Namespace naming improvements in the latest implementation commits are useful, but do not prove that identity attestation, delegated authority or cross-chain equality is enforced. The only deltas after source baseline `83b9f0902a062d9a97089c2729889ea565d7af82` are the 2026-09-08 and 2026-09-09 documentation publications; the `standards/` tree remains exactly `d7bede155734827d88776421eadc8ac7839e5379`.

## Layers and authorities

1. **Logical schema:** versioned business identity, units, mutability, required fields and relationship semantics, independent of storage. Publish a machine-readable conformance profile rather than assuming the descriptive `nexusFields` catalog is a JSON Schema validator.
2. **Chain realization:** map logical fields to Nexus register serialization or Solana account layout. Specify all deliberate differences (addresses, system metadata, initialization-only self identity), maximum encoded bytes and update rules. A native string namespace and an account owner/program authority are different concepts.
3. **Verified identity:** canonical chain address plus chain/network identity is the primary locator. Treat mutable `self-addr`, namespace, manufacturer and driver fields as claims until validated against authoritative register/account state and owner/delegation evidence. `self-addr` conventions may support indexing but must never override the returned canonical address. Persist create identity and reconcile an incomplete stamping operation without creating a duplicate register.
4. **Relationships and lifecycle:** product revisions are append-only records; ride requests, driver offers, passenger agreements and invoices retain independent ownership. A reference alone proves neither authorization nor payment. Readers validate the complete relationship and reject wrong-chain, wrong-owner, conflicting, cyclic or unknown-version records.
5. **Application adapters:** NexGo currently creates legacy raw `nexgo-ride` data, not the new typed handshake. ShipGen and other consumers must pin the actual standard version/hash they use and describe any subset or extension explicitly. Solana product account examples remain illustrative and are not deployment identities.

## Unresolved architecture decisions

### Product identity versus revision identity

The Solana draft currently derives a product PDA solely from `product`, authority and article number, yet promises a NEW account for each revision. Define a stable product-head identity separately from immutable revision identity, or add a bounded canonical revision seed (with a stated compatibility/versioning change). Specify uniqueness, history order, concurrent supersession policy, prior-record ownership and non-reuse after closure. The present draft cannot promise distinct revision accounts under identical seed inputs.

### Encoding and enforceable parity

The inspected Solana offset table is internally contiguous and totals 545 bytes. That does not prove a compiled serializer has the same layout, updates enforce authority, or Nexus serialization fits a universal 1 KB budget. Conformance must measure actual encoded data under pinned core/program versions, including UTF-8 and serialization overhead. Define the intentionally different lifecycle of Nexus mutable `self-addr` and Solana immutable initialized `self_addr`; do not claim literal mutability equality where the specifications differ.

### Deployment evidence

Pin core branch/SHA, network and registered methods before adopting API recipes. Core read-by-address and any filter restrictions must be verified directly, not inferred from older website examples. No public/foreign node receives profile credentials. A Solana program release needs source, tests, authority constraints and a verified program/cluster identity; a JSON string containing Rust is not such a release.

## Acceptance

**Design catalog, not implementation-certified standard.** Acceptance requires a reproducible conformance gate, two-party ownership/payment fixtures where relevant, serializer vectors and actual consuming-adapter tests. Current verification and prioritized work are in the linked 2026-09-10 review and plan.
