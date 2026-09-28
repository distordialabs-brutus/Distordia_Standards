# Architecture addendum — agent assets, commerce evidence, and conformance order — 2026-09-28

## Status and authority

This is a documentation-only architecture addendum for repository source commit `2173ab5fe7702e012588c27f06fecf005d90ca7e` and committed `standards/` tree `d7bede155734827d88776421eadc8ac7839e5379`.

If published, this addendum is the current qualification of `docs/ARCHITECTURE.md` and `docs/ARCHITECTURE_ADDENDUM_2026-09-25.md` for agent identity, digital assets, authorization, commerce, settlement, and the immediate implementation order. Earlier dated reviews remain historical evidence. This addendum does not amend a standard, approve an external protocol profile, certify implementation, or change the evidence level of any schema.

The following local files were read as design context only and are not treated as published or accepted strategy:

- `docs/AGENT_ASSET_COMMERCE_RESEARCH_2026-09-25.md`, SHA-256 `dfd60c860a1f3b18b2bd45dff7fcb23b55af335ac5a886ea896b022a22fd5157`;
- `docs/PROPOSED_STRATEGY_UPDATE_AGENT_ASSETS_2026-09.md`, SHA-256 `7208413fc5bd263c3f42c3cd99dad6deb9f4c68f84aaa6129764c906cd9a434b`;
- `vision.md`, SHA-256 `2231687ca457642e60410a80d54ee6309e769b606f143a619d6ce406e7ea1de1`;
- unstaged `docs/standards-market-evaluation.md`, SHA-256 `9fd3cfa320f76184a446fdae49a90d52b6a58e1db54195f78425ec3c1f368ec3`.

External protocol versions and lifecycle labels described in those local notes were not independently revalidated by this repository review. They become normative only through a separately reviewed, version-pinned profile with fixtures.

## Current qualification of the architecture

The September 25 evidence architecture remains directionally sound, with two sharper conclusions:

1. **Batch 1A remains the first coding milestone.** Agent, NFT, commerce, payment, attestation, rights, and private-data profiles must not precede the canonical manifest, mandatory wire versions, strict validator, fixtures, and CI.
2. **The current positive-example baseline is weaker than previously recorded.** Examples are present for 10 of 13 v0.2.0 Nexus logical types, but strict checks found five UTF-8 width violations across the only examples for three of those types. Only 7 of 13 types currently have at least one positive example that passes the fields' declared type, required, enum, pattern, and UTF-8-width constraints.

The new local research narrows the target rather than changing the repository's evidence hierarchy: Distordia should standardize attributable evidence relationships and conformance, not become an assumed wallet, marketplace, storage provider, payment processor, custodian, or universal agent registry.

## Architectural separations

### 1. Agent identity handle

An agent identity handle is a compact public record for discovery and control continuity. Its future logical contract should identify:

- canonical agent identity and parent namespace/controller;
- exact schema and chain-profile identity;
- lifecycle, predecessor, transfer epoch, revocation, and recovery;
- signed Agent Card or equivalent service-manifest URI, digest, media type, and profile version;
- chain-qualified wallet/account reference and proof method;
- authorization-policy reference and supported protocol-profile IDs.

Model name, prompts, memory, credentials, complete transcripts, detailed skills, bearer tokens, and transaction history do not belong in this handle. Current `can-transact`, `max-tx-value`, `can-sign`, `can-delegate`, `kill-switch`, and endpoint fields remain owner-authored declarations until an execution profile proves enforcement.

### 2. Functional-agent asset

A transferable identity token and a transferable functional agent are not the same asset class. A functional-agent asset may refer to encrypted model, memory, character, or configuration data, but its profile must separately prove or disclose:

- ciphertext/content commitments and availability policy;
- verifier/proof implementation and exact version;
- re-encryption and recipient access acknowledgement;
- sealed-key recipient and access epoch;
- authorization reset on transfer;
- key, verifier, storage, and recovery failure policy.

Transfer cannot prove deletion of every seller copy, copyright, legal title, model quality, safe behavior, or future availability. Ordinary token transfer must not bypass a required data-transfer ceremony.

### 3. Artifact, provenance, rights, and storage

An artifact record binds bytes and attributable statements; it does not make those statements true. Keep distinct references for:

- canonical artifact manifest, content address, digest algorithm, digest, media type, byte length, and canonicalization profile;
- encryption and key-reference metadata without publishing keys or secrets;
- retention class and independently checkable availability evidence;
- provenance manifest, signer/trust result, ingredients/actions, and validation time;
- rights-policy issuer, target, assignee, permissions, prohibitions, duties, constraints, validity, and supersession.

Token ownership, access to encrypted bytes, authorship/provenance, copyright or legal title, and licensed use are independent facts.

### 4. Authorization, workload, and execution

Identity is not authority, and a wallet signature is not running-code identity. A future authorization profile must bind:

```text
principal/controller
  -> delegate key and account
  -> audience, targets, methods, assets, and exact chain/network
  -> per-action, rolling, and aggregate integer limits
  -> validity, nonce, replay domain, revocation, and escalation
  -> deterministic policy evaluator version
  -> optional workload/runtime evidence
  -> execution receipt and authoritative settlement reconciliation
```

Runtime-attestation and transparency receipts are optional evidence classes. Attestation may prove a measured state at a time; transparency may prove statement registration or inclusion. Neither proves correct future behavior, actual policy enforcement, settlement, or truth.

### 5. Commerce session and settlement

Commerce lifecycle, delegated intent, payment negotiation, and rail settlement are separate layers. Use namespaced protocol identifiers and exact versions; never use a bare ambiguous acronym as canonical identity.

The protocol-neutral state model is:

```text
intent/mandate persisted
  -> quote or commerce-session commitment
  -> bounded execution authorization
  -> settlement intent reserved
  -> submitted remote identity
  -> outcome-unknown | confirmed | reverted | refunded
  -> authoritative recipient/asset/amount read-back
  -> receipt
  -> accountability event
```

A checkout state, HTTP success, invoice status, facilitator receipt, mutable `paid` flag, transaction hash string, or user-written escrow amount is not exact settlement evidence. Timeout and malformed success remain `outcome-unknown`; they do not authorize retry, refund, release, slash, or reputation finalization.

### 6. Accountability and reputation

Bond, claim, challenge, verdict, and execution remain separate records and authorities. Commerce, authorization, tool, runtime, provenance, rights, artifact, and payment records may be referenced as evidence but are never automatically trusted outcomes.

Reputation remains a deterministic, versioned view over finalized attributable events. Transfer must either reset or epoch-scope controller proofs, wallet bindings, mandates, sessions, delegated authority, and reputation aggregation boundaries. Existing mutable reputation, mission, progress, escrow, and quality counters remain untrusted display claims.

## Canonical reference envelopes

The logical layer should standardize compact references rather than add one field per external protocol:

```text
SpecRef       = canonical_id + semver + lifecycle + digest + profile
ActorRef      = chain + network + canonical record/account + authority epoch
ArtifactRef   = manifest profile + URI/CID + digest + media type + availability class
AuthorityRef  = issuer + principal + delegate + audience + scope digest + validity + revocation
CommerceRef   = namespaced protocol/version + merchant/service + session/order + state evidence
SettlementRef = rail/profile + intent identity + asset + integer amount + payee + remote identity + read-back
EvidenceRef   = method/profile/version + subject + evidence digest + verifier + time/finality result
```

Unknown versions or critical constraints may be retained as opaque evidence but fail closed for semantic validation and execution.

## Chain realization boundaries

### Nexus

A Nexus profile must pin core source, network, registered endpoints, JSON/raw encoding, size measurement, query grammar, pagination, finality, register/contract identity, and owner/genesis semantics. Profile credentials stay on a controlled node. Every mutation requires exact read-back. The public Distordia API may be a convenience index, never the conformance authority.

### Solana

The current 545-byte product layout remains a descriptive target. Program ID, cluster, PDA revision identity, canonical seed encoding, account authority, predecessor constraints, serialization, zero padding, UTF-8, close/reinitialize behavior, and exact account effects require compiled tests and local-validator evidence. Token-2022 metadata and fixed product accounts are distinct realizations. Matching business fields or hashes do not create a cross-chain identity.

### Other chains and external profiles

No generic signature, token ownership, registry entry, or protocol receipt establishes a bridge or cross-chain settlement. Each adapter must identify its trust model, canonical chain/account/asset identity, authority, finality, replay protection, unknown-outcome handling, and read-back source.

## Immediate implementation sequence

### Batch 1A.1 — Freeze canonical specification identity

Add `standards/manifest.json` with all 21 current JSON documents. Record canonical `$id`, semantic version, lifecycle status, logical standard, chain profile, repository path, SHA-256, exact aliases, and exact graph edges. Preserve legacy bytes; classify undiscriminated records as `legacy-unversioned` unless a named adapter proves a writer shape.

### Batch 1A.2 — Add the strict validator core

Add a dependency-neutral duplicate-key rejecting loader, explicit specification-format schema, and `tools/validate_standards.py`. It must validate declarations, defaults, examples, local references, manifest digests/edges, and Nexus/Solana declared parity with stable path/type diagnostics.

### Batch 1A.3 — Make the wire version mandatory

Add `schema-ver` to `required` for all 13 Nexus v0.2.0 logical types and the Solana product required set. Parameterized checks must reject missing, malformed, wrong, and unknown versions for every type. Numeric Nexus register `version` never satisfies this requirement.

### Batch 1A.4 — Repair and complete positive fixtures

Correct the five current width-invalid references and add examples for `distordia-article-chunk`, `distordia-reaction`, and `nexgo-ride-agreement`. Every logical type must have at least one fixture that passes strict integer/type, UTF-8 byte width, enum, pattern, required-field, and example/type consistency checks.

### Batch 1A.5 — Add adversarial fixtures and CI

Add invalid fixtures for duplicate keys/fields/types, unsupported scalars, bad mutability metadata, UTF-8 boundary-plus-one, unsigned under/overflow, enum/pattern mismatch, missing definitions/required fields, inconsistent examples, stale digest, ambiguous alias, unresolved edge, broken link, and product-profile parity mismatch. Document one offline command and make CI invoke that exact command.

Only after Batch 1A passes may work proceed to pinned Nexus query fixtures, agent v0.2.0 identity design, external interoperability profiles, accountability records, or any fund-moving implementation.

## Exit conditions

The immediate architecture exit is satisfied only when a clean checkout and CI run the same command and prove:

- all 21 specification documents resolve through the manifest;
- all 13 v0.2.0 Nexus types and Solana product require exact supported wire versions;
- all 13 types have strict-valid positive examples;
- all required negative classes fail with stable diagnostics and nonzero status;
- local references and declared cross-chain mappings are deterministic;
- no status is promoted beyond offline conformance.

This addendum authorizes no transaction, deployment, custody, bridge, mainnet/testnet claim, or autonomous settlement path.
