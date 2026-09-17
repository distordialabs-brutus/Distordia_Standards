# Standards development plan

Updated 2026-09-17 at HEAD `a5f94125ebc07f02120f29b39d811ea3d67c658f`. [Architecture](ARCHITECTURE.md) · [Review evidence](DEVELOPMENT_REVIEW_2026-09-17.md).

## Status at this baseline

No standards source changed after the 2026-09-16 reviewed source; the current `standards/` tree is still `d7bede155734827d88776421eadc8ac7839e5379`. A fresh 21-file JSON/duplicate-name probe and final 15-file Markdown-link check passed, but these narrow checks do not establish conformance. All 13 v0.2.0 logical Nexus asset types still define `schema-ver` without requiring it. Fresh parity inspection also found the same omission in the Solana product required array despite its identical-required-set guarantee. All batches below remain retained work, not regressions introduced since the prior review. **Next repair:** Batch 1A, before schema or status promotion.

## Batch 1A — Mandatory versions and offline conformance gate (P1)

- Update `standards/article-standard.v0.2.0.json`, `content-standard.v0.2.0.json`, `namespace-standard.v0.2.0.json`, `nexgo-rating-standard.v0.2.0.json`, `nexgo-ride-standard.v0.2.0.json`, `nexgo-taxi-standard.v0.2.0.json`, `product-standard.v0.2.0.json` and `product-standard.v0.2.0.solana.json`. Require the exact immutable `schema-ver` discriminator for all 13 Nexus logical types and preserve the declared Nexus/Solana product required-set parity.
- Add `tools/validate_standards.py`, `tests/fixtures/valid/`, `tests/fixtures/invalid/`, a documented command in `README.md` and `.github/workflows/standards.yml` for JSON syntax, unique field/type identity, required-field coverage, supported scalar types, default/enum UTF-8 length and integer bounds, example validation, version references and local Markdown links.
- Add an explicit schema/validator for the specification format. Do not claim ordinary JSON Schema validation can enforce `nexusFields` and custom field-definition semantics without a custom schema/validator.
- Separate descriptive examples, deliberately invalid fixtures, and normative data. Unknown schema versions reject or remain read-only rather than being implicitly accepted.
- Exit: clean checkout runs the whole offline gate; deliberate missing/wrong/unknown version, Nexus/Solana required-parity mismatch, duplicate-field/type, UTF-8 overlength, out-of-range integer, enum/pattern mismatch, inconsistent example and broken-reference fixtures fail. The historical parsing probe is only a baseline, not this gate.

## Batch 1B — Transport-ready Nexus query contract (P1)

- Update query sections in `standards/nexgo-ride-standard.v0.2.0.json`, `nexgo-taxi-standard.v0.2.0.json`, `nexgo-rating-standard.v0.2.0.json` and `docs/nexgo-cluster-design-note.md`; add pinned-core metadata and request fixtures under `tests/nexus/queries/`.
- Validate normative requests against the pinned query grammar: `results.<field>`, supported operators, `*` wildcards, escaping and explicit pagination. Reject undocumented SQL-style `LIKE` or label it non-executable pseudocode.
- Exit: an isolated pinned core executes open-request, offer, taxi-area and rating filters across more than one page; later-page failure reports incomplete rather than empty/complete, and the offline grammar gate rejects malformed fixtures.

## Batch 2 — Resolve product revision identity before an Anchor implementation (P1)

- Amend `product-standard.v0.2.0.solana.json` and `product-masterdata-mrp-analysis.md` jointly. Choose a stable head PDA plus revision PDAs, or a versioned revision-inclusive seed contract. Specify canonical UTF-8 normalization, seed length/encoding and immutable revision identifiers.
- Define `supersede` arguments, derived addresses, authority constraints and predecessor validation. Old product records must remain readable; duplicate revision initialization must reject deterministically. Address closure/reinitialization and whether more than one successor is legal.
- Exit: same authority/article with revisions A and B derives distinct revision addresses, duplicate A fails, B points to A, unauthorized supersession fails, and predecessor data is unchanged. Run an actual local validator/Anchor test once a program exists; do not call a seed-list inspection an on-chain execution.

## Batch 3 — Identity, serialization and cross-chain contract (P1)

- Replace the unqualified identical-mutability claim with a field-by-field logical/physical equivalence table. Treat self-identity initialization, timestamps, authority metadata and chain-specific addresses explicitly. Normalize logical comparison without discarding chain/network identity.
- Add binary golden vectors for the specified Solana layout and actual pinned Nexus serialization measurements at boundary/multibyte sizes. Derive account size and query offsets from one schema source. Recompute rent only for a real chosen cluster, not from illustrative constants.
- Define create/stamp/read-back recovery for Nexus `self-addr`; readers reject a claimed self-address that differs from canonical address. Revalidate namespace ownership/delegation before acting on claims.
- Exit: round-trip values and update immutability match declared semantics, encoded boundaries are measured, and forged self-address/owner/namespace fixtures are rejected.

## Batch 4 — Consumer migration and release acceptance (P2)

- Publish a matrix for each consumer, starting with NexGo: actual writer shape, accepted reader versions, target schema and missing adapter tests. The existing NexGo `version: 1` raw blob is not a v0.2.0 typed request/offer/agreement.
- Require exact invoice recipient/provider/account/token/currency/integer-amount binding and read-back before treating an agreement as paid. Rating validity must traverse canonical request, driver-owned offer, passenger-owned agreement and paid invoice evidence; mutable `agreement.status=completed` alone is insufficient. Define deterministic duplicate-rating handling and document the physical-completion oracle boundary. Document privacy-preserving precise-location handoff rather than public coordinate history.
- Pin Nexus core/wallet versions and source-verified endpoint/response fixtures. Use isolated test profiles and devnet/local validator identities; no illustrative address is a production program ID.
- Exit: a consumer contract test writes, reads and validates each supported version and rejects an unknown/missing version, forged owner/namespace or cross-reference, unpaid/mismatched invoice and duplicate rating. Draft status only changes after its stated protocol and live acceptance evidence exists.
