# Standards development plan

Updated 2026-09-10 at pre-publication local/remote HEAD `e8f890e3f02e6aa1fda6a1af76ef3ac0df5c60bc`. [Architecture](ARCHITECTURE.md) · [Review evidence](DEVELOPMENT_REVIEW_2026-09-10.md).

## Status at this baseline

No standards source changed after `83b9f0902a062d9a97089c2729889ea565d7af82`; the current `standards/` tree is still `d7bede155734827d88776421eadc8ac7839e5379`. The 2026-09-08 and 2026-09-09 commits added and linked review documentation only. All batches below remain open retained work, not regressions introduced since the prior review.

## Batch 1 — Executable conformance gate (P1)

- Add one documented command and CI for all standards: JSON syntax, unique field names/type identifiers, required-field coverage, supported scalar types, default/enum UTF-8 length and integer bounds, example validation, version references and local Markdown links.
- Add an explicit schema/validator for the specification format. Do not claim ordinary JSON Schema validation can enforce `nexusFields` and custom field-definition semantics without a custom schema/validator.
- Separate descriptive examples, deliberately invalid fixtures, and normative data. Unknown schema versions reject or remain read-only rather than being implicitly accepted.
- Exit: clean checkout runs the whole gate; deliberate duplicate-field, out-of-range integer, mismatched example and broken-reference fixtures fail. The present offline parsing/enum probe is only a baseline, not this gate.

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
- Require exact invoice recipient/provider/account/token/amount binding and read-back before treating an agreement as paid; document privacy-preserving precise-location handoff rather than public coordinate history.
- Pin Nexus core/wallet versions and source-verified endpoint/response fixtures. Use isolated test profiles and devnet/local validator identities; no illustrative address is a production program ID.
- Exit: a consumer contract test writes, reads and validates each supported version and rejects an unknown version or forged cross-reference. Draft status only changes after its stated protocol and live acceptance evidence exists.
