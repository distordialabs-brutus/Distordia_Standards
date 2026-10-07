# Standards architecture and implementation contract

## Governing vision and portfolio traceability

Read [the repository vision](../vision.md) and [Distordia alignment/dependency map](DISTORDIA_ALIGNMENT.md) before assigning work. Authority is master Distordia strategy/customer evidence → portfolio roadmap/strategy decisions → repository vision → this architecture → tasks/code/tests/external evidence and human release.

**Portfolio purpose:** O1 independently implementable standards; O2 namespace authority; O3 reproducible evidence. Canonical versions, explicit chain realizations, strict conformance and independently replayable authority/evidence semantics. Distordia verification is optional evidence, not mandatory execution permission. The alignment map supplies customer-evidence qualification, batch ownership, upstream prerequisites and human gates. Each material task must name those fields alongside its exact production paths and collected acceptance tests. This documentation alignment changes no runtime, test result or release status; dated evidence below remains evidence for its stated snapshot only.

Published source reviewed here is detached remote tip `e590a2af627c8f2b6c8a0fa9c6f5642f04652982`. Fresh local-only `f30b183a34af4fd7a9f5757ade15a3038f8cd2ce` is a divergent sibling, not an unpublished commit ahead of remote: the lines fork at `db3da5626f270d5d411c4f9eb4b6d9e706891b10`, with one local-only commit and two remote-only commits. Local agent/commerce research remains hypothesis context and is not published-source authority. Both lines leave `standards/` at tree `d7bede155734827d88776421eadc8ac7839e5379`. See the historical [review](DEVELOPMENT_REVIEW_2026-09-17.md), current [development plan](DEVELOPMENT_PLAN.md), and the [conformance acceptance addendum](ARCHITECTURE_ADDENDUM_2026-10-02.md).

## Scope

This repository contains specifications and design notes, not a deployed Nexus validator, a Solana Anchor program, or an application conformance gate. Draft v0.2.0 schemas are target contracts. Existing unversioned standards and consuming applications must not be silently relabeled v0.2.0. Namespace naming improvements in the latest implementation commits are useful, but do not prove that identity attestation, delegated authority or cross-chain equality is enforced. No standards source has changed after baseline `83b9f0902a062d9a97089c2729889ea565d7af82`; later commits through the reviewed HEAD are review documentation only.

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

### Executable Nexus query contract

Normative discovery examples must use the pinned core's actual query DSL. Current API documentation uses `results.<field>` and `*` string wildcards; it does not document SQL `LIKE`. The v0.2.0 NexGo ride/taxi filters and the mobility design note instead use unprefixed fields plus `LIKE`, so geo-sharded discovery is not currently an executable contract. Publish transport-ready request fixtures separately from explanatory pseudocode, validate pagination/completeness, and run them against an isolated pinned core before claiming queryability.

### Version discrimination and verified-ride evidence

Every one of the 13 inspected v0.2.0 logical Nexus asset types declares an immutable `schema-ver` field but omits it from its `required` list. A conformance reader therefore cannot both follow those required lists and enforce the stated unknown-version fail-closed rule. Make `schema-ver` required with the exact supported value in every versioned type, and add missing/wrong/unknown-version rejection fixtures.

The Solana product realization has the same contract omission: its account layout includes `schema-ver`, its `required` array omits it, and its equivalence declaration promises identical required-set parity. Correct the canonical Nexus and Solana product contracts together; do not repair only the Nexus catalog while retaining a contradictory parity claim.

A passenger-owned ride agreement's mutable `status=completed` is also not independent evidence that a ride occurred. Rating validation must traverse canonical request, driver-owned offer, passenger-owned agreement and the exact paid invoice; verify owner/namespace bindings, request/offer/agreement terms, recipient/provider/account/token/currency/integer amount and payment evidence; and define duplicate-rating selection. Treat physical trip completion as the documented oracle boundary rather than claiming that a mutable agreement status alone creates a verified ride.

## Next implementation slice

Batch 1A updates all eight Nexus v0.2.0 standard files, including `social-standard.v0.2.0.json`, plus `product-standard.v0.2.0.solana.json`, adds `tools/validate_standards.py`, valid/invalid fixtures and `.github/workflows/standards.yml`, and requires exact immutable version discrimination. Batch 1B makes the NexGo ride/taxi/rating query sections transport-ready under a pinned core grammar with multi-page fixtures. Product revision identity and the paid-invoice verified-ride graph remain later independent batches; neither may be hidden inside the validator change.

An independent scratch validator was necessary because the published tree contains no maintained validator, test collection or workflow. Against the unchanged 21-document tree it parsed all documents, normalized the 13 v0.2 Nexus logical types globally, and confirmed checked-in strict-positive coverage of **7 valid, 3 represented only by invalid examples, and 3 missing**. Generated declaration-complete positives passed 13/13, showing the immediate contradiction is in required/version contracts and examples rather than an inability to instantiate the declared fields. Of 106 generated adversarial cases, 93 rejected; the only 13 accepted negative cases were omission of `schema-ver`, once for every logical type. This is diagnostic evidence outside the repository, not a shipped validator, test suite or conformance certification.

## Acceptance

**Design catalog, not implementation-certified standard.** Acceptance requires a reproducible conformance gate, mandatory exact version discriminators, executable pinned-core query fixtures, two-party ownership/payment fixtures where relevant, serializer vectors and actual consuming-adapter tests. The exact next repair is Batch 1A: check in one validator/CI command plus invalid fixtures and mandatory version corrections. Exit only when a clean checkout rejects missing/wrong/unknown versions, duplicate/overlength/out-of-range/example/link failures and proves Nexus/Solana product required-set parity. Batch 1B then executes open-request, offer, taxi-area and rating discovery across multiple pages on an isolated pinned core. Current evidence and remaining batches are in the linked 2026-09-17 review and plan.
