# Development review — 2026-09-08

## Baseline and scope

`main` at `83b9f0902a062d9a97089c2729889ea565d7af82`; exact origin branch matched on read-back. No commits since the prior job timestamp 2026-09-07 06:12:57. Latest implementation-related changes remain the August namespace alignment. No earlier dated cron review exists in this checkout, so this report establishes a baseline and does not attribute old gaps to new work.

Inspected README, file inventory, product Nexus/Solana v0.2.0 definitions, NexGo v0.2.0 ride definition, and current sibling NexGo writer. All 21 standards JSON files were parsed by an executed structural probe. This was not a complete semantic audit of every protocol or live-chain certification.

## Findings and repair direction

### P1 — Solana product revision promise contradicts its PDA identity

`standards/product-standard.v0.2.0.solana.json:21-24,103-117` derives a PDA from `product`, authority and `art-nr`. `rev` is immutable and absent from the seed tuple. The `supersede` section requires initialization of a **new** product PDA for a new revision, but gives no alternate seed derivation. For an unchanged authority/article/program, these specified deterministic inputs identify the same PDA; a new revision cannot be a distinct initialized account under that contract. This is a specification inconsistency, not a demonstrated failure of a deployed program (there is none in this checkout).

Resolve stable product identity versus immutable revision identity before implementation. Specify canonical revision seeds or a head/revision model and test duplicate, unauthorized and concurrent supersession. See ordered [plan](DEVELOPMENT_PLAN.md).

### P1 — Conformance claims exceed available enforcement

The Git inventory contains documentation/JSON definitions but no executable validator, package/test command, Anchor project or CI workflow. Parsing JSON does not validate `required`, mutability, byte budgets, ownership or examples. The Solana draft promises identical mutability while Nexus `self-addr` is mutable and Solana `self_addr` is initialized immutable (`product-standard.v0.2.0.json:13`, Solana `:30`). This difference can be legitimate, but must be an explicit lifecycle mapping rather than literal physical parity.

The README's mutable `self-addr` is “frozen by convention” (`README.md:98-121`), not cryptographically frozen. Readers must bind it to canonical chain identity/owner; a copied self-address must not impersonate the linked object. Draft byte/cost figures and embedded Rust are not executed serializer/program evidence.

### P2 — Consumer compatibility remains unproven

The sibling NexGo implementation at `7a9c481` emits raw `nexgo-ride` with `version: 1`, exact coordinates and labels (`src/api/nexusAPI.js:446-479`). The v0.2.0 specification instead defines separate typed request/offer/agreement assets with coarse geohashes and locked fares (`standards/nexgo-ride-standard.v0.2.0.json`). Keep both statuses explicit and add consumer conformance fixtures before asserting ecosystem adoption.

## Executed evidence

`python3 /tmp/repo-review-20260908.py` parsed the repository JSON and recursively checked declared Nexus field sets for duplicate names, default string lengths and enum string lengths:

```text
parsed_json_files: 21
nexus_field_sets: 25
field_issues: []
solana_layout_bytes: 545
revision_in_pda_seeds: false
```

The Solana offset table was verified contiguous by asserting each offset equals the preceding end and summing `bytes`. The resulting 545 agrees with the draft; it is not an Anchor/Borsh compiled-size test. The probe is temporary review tooling, not a checked-in gate. Its narrow checks do not establish example validity, byte overhead, numeric bounds, ownership, chain identity or full conformance.

No live network mutation, wallet operation, deployment, or real program-address lookup was performed. Illustrative addresses were left unchanged and were not treated as valid deployment identifiers.

## Verdict

The layered base-product and NexGo handshake direction is useful, but implementation acceptance remains **draft / unverified**. New [architecture](ARCHITECTURE.md) and [development plan](DEVELOPMENT_PLAN.md) distinguish logical schema, physical encoding, canonical identity and consuming adapters, and give executable exit criteria before rollout. No source/schema files were changed by this review.
