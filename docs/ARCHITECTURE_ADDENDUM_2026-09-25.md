# Architecture addendum — accountability, versioning and cross-chain realization — 2026-09-25

## Status and purpose

This is a current addendum for source commit `7b42a03d40dbe60eeaca7c08fa5212bb95092d44`. It does not rewrite the staged 2026-09-21 architecture, plan, or review. It incorporates the direction in untracked `vision.md` (SHA-256 `2231687ca457642e60410a80d54ee6309e769b606f143a619d6ce406e7ea1de1`) while preserving the evidence rule that strategy does not make a schema implemented, deployed, adopted, funded, settled, or cross-chain equivalent.

The target is an open standards stack in which independently operated implementations can answer:

1. Which exact specification and chain profile governs this record?
2. Who asserted what, under which current authority?
3. What collateral or other evidence backed the assertion?
4. Who challenged it, under what rule and window?
5. Which evidence and resolver produced the disposition?
6. Which authoritative chain transition, if any, executed settlement?
7. Can an independent indexer replay the same event history and derive the same reputation result?

## Architectural invariants

1. **A record is not proof of its own authority.** Namespace, agent, owner, capability, stake, escrow, reputation, completion and payment fields are claims until checked against authoritative chain state and the applicable delegation/settlement graph.
2. **Specification identity and record identity are separate.** A document digest identifies a schema release. A chain/network/address identifies a record. A logical business key is neither one.
3. **Cross-chain similarity is not equivalence.** Matching product fields or hashes do not prove common authority, provenance or settlement. Equivalence requires an explicit verified edge.
4. **Reputation is derived.** Mutable score and counter fields are never authoritative inputs merely because they are on-chain.
5. **Status is not evidence.** A mutable `paid`, `completed`, `funded`, `verified` or `slashed` value cannot replace authoritative transaction and relationship evidence.
6. **Financial outcomes fail closed.** Timeout, malformed response, partial enumeration and unverified settlement remain pending/unknown; they do not authorize retry, release, refund, slash, payout or reputation finalization.
7. **Distordia is not required for conformance.** Canonical schemas, validation rules, algorithms, fixtures and chain profiles are public. Commercial indexers or analytics may accelerate reads but do not become the source of truth.
8. **No Distordia custody by implication.** A standard may describe peer-to-peer escrow and disposition. It must not imply company custody, underwriting or unilateral settlement without an explicit separately reviewed implementation and legal model.
9. **Nexus constraints are measured.** The 1 KB limit, JSON/raw encoding, field mutability, query grammar and transaction behavior are proved against a pinned core, not inferred from field-count arithmetic.
10. **Evidence levels remain distinct.** Structural validation, offline semantic conformance, isolated-chain execution, public testnet execution, mainnet execution, consumer support and unaffiliated adoption are different statuses.

## Layered target architecture

```text
Strategy and vision (intent; never implementation evidence)
                         |
                         v
+---------------------------------------------------------------+
| L0  Canonical specification registry                          |
| IDs, semver, lifecycle status, digest, aliases, supersession  |
+---------------------------------------------------------------+
                         |
                         v
+---------------------------------------------------------------+
| L1  Logical contracts                                        |
| identity, actors, authority, evidence, states, relationships  |
| product/content/social/NexGo + bond/claim/challenge/verdict   |
+---------------------------------------------------------------+
                         |
                         v
+---------------------------------------------------------------+
| L2  Chain realization profiles                               |
| Nexus JSON/raw and system metadata; Solana account/program    |
| exact encoding, limits, authority, finality, query semantics  |
+---------------------------------------------------------------+
                         |
                         v
+---------------------------------------------------------------+
| L3  Authoritative event and settlement evidence              |
| tx/register identities, funding, challenge, verdict, execute  |
| pending/unknown states and read-back reconciliation           |
+---------------------------------------------------------------+
                         |
                         v
+---------------------------------------------------------------+
| L4  Permissionless index and deterministic derived views     |
| complete enumeration, reorg handling, versioned reputation    |
+---------------------------------------------------------------+
                         |
                         v
+---------------------------------------------------------------+
| L5  Consumer adapters and commercial overlays                |
| SDK/CLI, apps, analytics, portals, verifier modules           |
| each pins IDs/digests/profiles and exposes evidence level     |
+---------------------------------------------------------------+
```

No upper layer may silently repair ambiguity in a lower layer. An indexer cannot manufacture authority absent from chain evidence. A consumer cannot relabel an undiscriminated legacy record as v0.2.0. Analytics cannot turn an unresolved challenge into a final reputation outcome.

## Canonical identities

### Specification identity

The manifest should key every release by:

```text
SpecKey = (
  canonical_id,
  semantic_version,
  lifecycle_status,
  logical_standard,
  chain_profile_or_none,
  sha256_digest
)
```

Required manifest data:

- local path and exact SHA-256;
- canonical `$id` and semantic version;
- `draft`, `candidate`, `stable`, `deprecated`, or `withdrawn` status;
- logical standard and chain realization profile;
- exact historical aliases;
- exact `supersedes`, `equivalent-profile`, and `derived-from` edges;
- immutable wire discriminator and recognized legacy adapter IDs;
- validator/profile version needed to interpret the file.

The current free-text strings such as `product.v0.1.0.json (retained for history...)` are prose, not graph identities. Legacy `.v1.json` IDs must be resolved explicitly rather than rewritten or inferred.

### Chain record identity

```text
RecordKey = (
  chain,
  network_or_cluster,
  chain_profile,
  canonical_register_or_account_address
)
```

Nexus owner/genesis, register revision and transaction/contract coordinates are evidence attached to the key; they do not replace it. Solana program ID, runtime owner, account authority and transaction signature are likewise distinct attributes. Namespaces, article numbers, GTINs, MPNs and display names remain searchable business claims and may collide or change ownership.

### Cross-chain mirror identity

```text
MirrorEdge = (
  source_record_key,
  destination_record_key,
  source_spec_digest,
  destination_profile_digest,
  mapper_id_and_version,
  asserting_authority,
  source_finality_evidence,
  destination_finality_evidence,
  edge_status
)
```

A mapper may establish a verified mirror; it may not collapse two chain records into one identity. The edge must state direction, authority, normalization rules, omitted or transformed fields, finality requirements, revocation/supersession, and replay protection. Matching `(mfr, art-nr)`, GTIN, MPN or payload hash is useful candidate discovery, never sufficient proof.

## Identity and delegation graph

Namespace trust must resolve through native chain state:

```text
native Nexus name/namespace register
    -> current owner/genesis
    -> current Distordia attestation bound to that owner
    -> delegated names/agents under the namespace
    -> exact action authority and scope
```

Required checks before accepting an agent or namespace claim:

- chain and network are expected;
- native register exists and current ownership matches the attestation binding;
- attestation version/status/expiry are supported;
- transfer, revocation, suspension and recovery are handled;
- parent/delegation reference resolves and covers the action scope;
- actor record owner matches or has a separately validated delegation;
- no mutable self-reference overrides canonical returned identity.

The current agent `can-transact`, `can-sign`, `can-delegate`, `kill-switch`, and transaction-cap fields should remain capability declarations until an execution policy profile binds them to actual signing/authorization controls. A public client must not treat the fields themselves as enforcement.

## Accountability event graph

The four new logical standards should remain separate records with explicit authorities:

```text
namespace/agent
      |
      v
accountability-bond -----> authoritative collateral/escrow evidence
      |
      v
staked-claim ------------> exact subject/artifact + verification method
      |
      +------------------------------+
      |                              |
      v                              v
challenge + challenger stake     unchallenged finalization
      |
      v
verdict + evidence + resolver disposition
      |
      v
settlement execution evidence (slash/release/payout), if applicable
      |
      v
derived reputation event
```

### Common envelope

Every new versioned logical record should define:

- immutable `distordia-type` and exact required `schema-ver`;
- canonical actor/subject references;
- creation time and expiry/window semantics;
- chain-independent evidence hashes with an explicit algorithm/encoding;
- immutable links to predecessor or related records;
- a lifecycle whose transitions name the authorized actor and required evidence;
- a chain-profile mapping for canonical address, owner, revision and transaction identity.

`self-addr` is a Nexus realization detail with create-pending and stamped states, not a universally mutable logical field.

### Accountability bond

Minimum logical semantics:

- accountable namespace and agent;
- beneficiary/counterparty scope or a hashed scope policy;
- exact collateral identity: chain, network, token/register/mint/program identity and decimals;
- exact integer base-unit amount;
- authoritative escrow/conditional-contract reference;
- activation, expiry, unbonding and challenge-hold windows;
- claim class/scope and maximum aggregate exposure;
- terms/payout policy digest and version;
- status derived from funding and settlement evidence.

A bond record must not mark itself funded. Activation requires authoritative collateral read-back. Aggregate claim exposure must not exceed independently verified available collateral. Release remains blocked while any covered claim/challenge/verdict/unknown execution can consume the collateral.

### Staked claim

Minimum logical semantics:

- claimant and authority/delegation evidence;
- bond reference and reserved exposure amount;
- exact subject identity, artifact digest and schema/profile digest;
- claim type and canonical claim digest;
- verifier method identifier and version;
- objective/subjective classification;
- challenge-window start/end and finality basis;
- counterparty/harmed-party reference where applicable;
- immutable terms snapshot.

A claim cannot become final because a writer changed `status`. Finalization requires complete enumeration through the window, no unresolved challenge, and the required finality/read-back evidence.

### Challenge

Minimum logical semantics:

- target claim;
- challenger identity and current authority;
- exact challenger stake identity/amount and funding proof;
- grounds code and evidence digest/URI commitment;
- filing time, deadline and unique challenge identity;
- resolver route and applicable method/version;
- status derived from accepted filing, verdict and execution evidence.

Duplicate, late, unfunded, self-challenged or wrong-scope challenges reject. Multiple legitimate challenges need a deterministic aggregation or independent-disposition rule; first observed order from a mutable index is not sufficient.

### Verdict

Minimum logical semantics:

- exact claim and challenge set;
- resolver identity, authority and conflict-of-interest policy;
- deterministic verifier method/module digest and input evidence digest, or explicit subjective arbitration profile;
- outcome vocabulary and reason/evidence commitment;
- slash/release/payout instruction reference and policy version;
- human disposition identity where required;
- appeal window and predecessor verdict;
- execution state backed by authoritative settlement read-back.

A verdict is a disposition, not a transfer. `proposed`, `approved`, `submitted`, `outcome-unknown`, `executed`, `reversed`, and `manual-review` must not be collapsed. Retrying after a timeout requires reconciliation by durable settlement identity.

## Financial and state-machine rules

Any implementation that locks or moves collateral must use a durable protocol:

```text
persist intent
  -> validate authority, limits and current evidence
  -> reserve exposure
  -> submit once
  -> persist remote transaction/contract identity
  -> reconcile unknown outcome
  -> verify exact chain effect
  -> finalize event and derived reputation
```

Mandatory invariants:

- amounts are strict integers in base units; symbols are display metadata;
- token identity includes chain/network and canonical token/register/mint/program identity;
- collateral, challenge stake and payouts never rely on mutable declared balances;
- liabilities include all submitted, unknown, disputed and appeal-pending outcomes;
- idempotency covers transaction/contract identity, not only a human claim ID;
- timeout or malformed success is `outcome-unknown`, never safe retry or failure;
- duplicate slash, release or payout is prevented across restart and restored storage;
- resolver approval and settlement execution use least-privilege distinct authorities;
- Distordia software may propose or relay, but custody and unilateral discretion are not implied;
- every external mutation is read back from the authoritative chain before completion;
- reputation updates only after the event reaches its defined final evidence state.

The existing swarm mission record cannot serve as this protocol. Its mutable escrow, progress, final-payment and outcome fields should be treated as descriptive legacy claims until a separate settlement profile proves them.

## Deterministic reputation architecture

Reputation is a versioned function over retained events:

```text
ReputationResult = score(
  algorithm_id,
  algorithm_version,
  ordered_final_events,
  subject_record_key,
  evaluation_height_or_time
)
```

The public output should include score, component values, event-set commitment, evaluation boundary and algorithm digest. Independent indexers must reproduce it from authoritative records.

Rules:

- owner-written `reputation`, `missions-done`, `missions-failed`, `slashing-events`, and `dq-score` are not authoritative inputs;
- only finalized events under supported specification/profile versions enter a final score;
- open, disputed, appealed and unknown-execution events are visible but do not masquerade as final outcomes;
- duplicates are eliminated by canonical event identity;
- event ordering and same-height tie-breaking are deterministic;
- chain reorganizations or reversed verdicts trigger deterministic recomputation;
- algorithm changes create a new result version and never rewrite the historical result;
- indexer incompleteness makes the result `incomplete`, not zero or green.

## Nexus realization profile

A Nexus profile must pin the exact LLL-TAO source SHA, network, registered endpoints, query grammar, serialization behavior and finality policy.

Required implementation evidence:

- actual create/update/read-back measurements at ASCII and multibyte boundaries;
- duplicate/overlength/underflow/overflow/unknown-field rejection;
- create-pending and `self-addr` stamp recovery without duplicate creation;
- canonical address and owner overriding forged user fields;
- native namespace ownership and transfer invalidating stale attestations;
- transport-ready `where` requests using the pinned `results.<field>` grammar and `*` wildcard semantics;
- complete pagination with query identity, stable deduplication and later-page failure held as incomplete;
- isolated profiles and data directory; no production credentials;
- transaction/invoice/conditional-contract result reconciled by tx/register/contract identity;
- all mutations read back before the logical event finalizes.

A public Distordia API may provide convenience reads, but conformance cannot require it or trust it over a controlled node.

## Solana product realization profile

The current 545-byte table is a target, not an implementation. The profile should be split into:

1. canonical product logical schema;
2. Nexus product realization;
3. Solana PDA realization;
4. optional Token-2022 metadata realization as a separate lossy profile;
5. mirror-edge schema connecting independently identified records.

Required corrections:

- include exact program ID and cluster in profile identity;
- add revision/predecessor identity to PDA derivation or introduce a stable head plus immutable revision PDAs;
- specify seed normalization, byte limits and collision policy;
- validate UTF-8, zero padding, interior NUL, enum values and unused bytes in program code;
- enforce signer, owner, PDA, authority, predecessor and relationship constraints;
- define close/reinitialize and concurrent successor behavior;
- replace literal mutability equality with field/lifecycle mappings;
- map signed Solana timestamps and Nexus unsigned system timestamps explicitly;
- make `self-addr` a profile-specific physical mapping;
- generate offsets and Rust layout from one source, with Borsh golden vectors;
- verify transaction finality and exact account effects after initialize/update/supersede/close;
- define mirror authority and provenance rather than inferring equality from business fields.

Token-2022 additional metadata is not type-equivalent to the fixed account layout: numeric fields become strings, extension behavior matters, and mint plus cluster plus token-program ID forms identity. It must declare this loss rather than inherit the strict parity guarantee.

## Query and index architecture

A query recipe is normative only when it includes:

- chain/core/profile version;
- endpoint, transport and exact parameter encoding;
- query grammar and escaping;
- result schema and version discrimination;
- page/offset/cursor semantics;
- query identity and ordering;
- duplicate handling;
- finality/reorg policy;
- completeness result: complete, incomplete, or unsupported;
- executable fixture and expected response.

Indexers must persist admitted events and checkpoint advancement atomically. A page limit, timeout, malformed row, unavailable transaction read, unsupported version or later-page error holds completeness. Derived reputation, open-claim discovery and exposure calculations must fail closed when enumeration is incomplete.

## Version evolution and compatibility

- Preserve legacy files and content digests.
- Treat records without an immutable recognized discriminator as `legacy-unversioned` unless an explicit named adapter proves the writer shape.
- Readers may expose unknown versions as opaque/read-only; writers and semantic validators reject them.
- Additive fields require a new semantic release when requiredness, meaning, mutability, authority or chain encoding changes.
- A chain realization change gets a new profile version even when logical semantics stay stable.
- Alias and supersession edges are manifest data, not filename heuristics.
- Migration produces a new record or explicit adapter evidence; it never relabels old on-chain bytes.
- Consumer support is a matrix of exact spec digest, chain profile, writer, reader, migration and passing contract tests.

## Acceptance matrix

| Exit | Required command | Evidence required |
|---|---|---|
| Canonical catalog | `python3 tools/validate_standards.py --repo .` | manifest, exact digests, aliases/edges, 13 versioned positive types, negative fixtures |
| Offline suite | `python3 -m unittest discover -s tests -p 'test_*.py'` | schema, relationship, state-machine, query, reputation and parity fixtures |
| Accountability graph | `python3 -m unittest discover -s tests/accountability -p 'test_*.py'` | complete and adversarial bond/claim/challenge/verdict graphs |
| Reputation replay | `python3 -m unittest discover -s tests/reputation -p 'test_*.py'` | golden score, replay, reorder, duplicate, reorg, appeal and version-upgrade vectors |
| Nexus profile | `python3 tools/run_nexus_acceptance.py --suite accountability,queries,nexgo --network private --core-sha "$NEXUS_CORE_SHA"` | pinned isolated node, multi-page reads, crash/restart/unknown outcome and exact read-back |
| Cross-chain vectors | `python3 tools/verify_cross_chain.py --manifest standards/manifest.json --vectors tests/product/cross-chain` | explicit mappings and mirror edges; wrong chain/network/authority negatives |
| Solana program | `cargo test --locked --manifest-path implementations/solana/product-registry/Cargo.toml` | serialization, validation, authority, seeds, migration and golden vectors |
| Solana local validator | `(cd implementations/solana/product-registry && anchor test)` | initialize/update/supersede/close effects and adversarial accounts |
| Repository quality | `git diff --check` | no whitespace errors; CI invokes the same documented commands |

These commands are acceptance targets; the corresponding tools, tests and implementation directories are absent at the reviewed commit and therefore were not claimed as passing.

## Delivery order

1. Correct maintained truth claims without rewriting historical evidence.
2. Land the canonical manifest, mandatory wire versions, strict validator, fixtures and CI.
3. Define common identity, authority, reference, exact-unit and lifecycle semantics.
4. Add the four accountability logical drafts and invalid graph fixtures.
5. Add deterministic reputation events and replay implementation.
6. Add a pinned Nexus realization and execute one full non-custodial lifecycle on an isolated network, with human verdict disposition and read-back.
7. Add permissionless index/query completeness and recovery tests.
8. Repair and compile the Solana product realization and explicit mirror profile.
9. Migrate NexGo and other consumers only through exact version/profile matrices and contract tests.
10. Promote status only to the evidence level actually achieved.

The 2026-09-21 Batch 1A remains the immediate coding gate. This addendum expands what follows it; it does not convert untracked vision into deployed behavior or authorize financial side effects.
