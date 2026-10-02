# Architecture addendum — conformance acceptance boundary — 2026-10-02

## Status and authority

This documentation-only addendum qualifies the maintained architecture against source commit `db3da5626f270d5d411c4f9eb4b6d9e706891b10` and committed `standards/` tree `d7bede155734827d88776421eadc8ac7839e5379`. The requested baseline and reviewed HEAD are identical: there is no committed implementation or specification delta to accept.

This addendum does not change a standard, example, status, chain profile, runtime, validator, workflow, or adoption claim. The pre-existing staged, unstaged, and untracked documents were preserved. In particular, local agent-asset/commerce research remains design context, not an implemented or accepted protocol profile.

The source-identity qualification, gate outcomes, and coder handoff are summarized in the [companion development review](DEVELOPMENT_REVIEW_ADDENDUM_2026-10-02.md). The [market-evaluation addendum](STANDARDS_MARKET_EVALUATION_ADDENDUM_2026-10-02.md) states the corresponding current readiness qualification.

## Accepted architecture progress

The 2026-10-02 review establishes a precise target architecture even though no code implements it yet. The following design decisions are accepted for Batch 1A:

1. **Source inventory is explicit.** `standards/manifest.json` inventories exactly the 21 source-standard documents at this baseline. The manifest is registry metadata, not a 22nd standard, and must not hash itself. Meta-schemas and fixtures live outside the source-standard inventory or carry an explicit non-standard artifact class.
2. **Physical dialects normalize before semantic checks.** The validator supports explicit adapters for Nexus v0.2 single- and multi-type documents, the Solana product profile, and legacy typed, raw, and custom/multi-definition forms. An unknown shape fails closed; it is not guessed into the nearest adapter.
3. **Logical declarations are repository-global.** All declarations are loaded into one normalized registry before any example is resolved. This is mandatory because `article-standard.v0.2.0.json` intentionally contains a `content` example whose declaration is in `content-standard.v0.2.0.json`.
4. **Positive coverage is per logical type.** An example counts only if one complete object passes required fields, exact JSON scalar typing, unsigned bounds, UTF-8 byte limits, enums, full-match patterns, exact type/version constants, and undeclared-field rejection. Duplicate valid examples do not increase type coverage.
5. **Diagnostics and CI are part of conformance.** Stable machine codes, deterministic locations/order, nonzero invalid exits, adversarial fixtures, and one clean-checkout command invoked identically by CI are release requirements.
6. **Cross-chain parity is mapped, not asserted.** Product parity requires explicit field, representation, authority, and lifecycle mappings. Nexus strings versus Solana `Pubkey`, Nexus post-create `self-addr` stamping versus Solana initialization, and unsigned versus signed timestamp representation are deliberate differences, not literal equality.

These decisions are architecture progress only. No manifest, validator, tests, fixtures, wrapper, or workflow is present at the reviewed source.

## Current invariant failures

Exact source-byte identity with the executed 2026-09-30 evidence, plus direct inspection of the nine v0.2.0 documents, confirms that the target has not moved:

- all 13 Nexus v0.2.0 logical types still declare immutable exact `schema-ver: 0.2.0`;
- all 13 still omit `schema-ver` from `required`; the Solana product required set also omits it;
- 12 checked-in Nexus example objects yield nine strict-valid objects but only seven covered logical types;
- `distordia-article`, `nexgo-rating`, and `nexgo-taxi` are represented only by width-invalid examples;
- `distordia-article-chunk`, `distordia-reaction`, and `nexgo-ride-agreement` have no positive example;
- no canonical manifest resolves the 12 legacy `.v1.json` IDs, semantic `0.1.0` declarations, or eight prose supersession strings;
- the declared Solana layout ends at 545 bytes, but no compiled serializer, authority test, revision-safe PDA, or local-validator evidence exists.

The five known UTF-8 overflows remain fixture defects: article root `next` is 60/56 bytes; rating `self-addr` is 64/56 and `agreement` is 60/56; taxi `license-cred` and `insurance-cred` are each 60/56. Do not enlarge limits only to preserve placeholders.

## Normative Batch 1A pipeline

```text
raw repository bytes
  -> duplicate-key-rejecting JSON loader
  -> explicit manifest/path/digest/canonical-ID check
  -> explicit document-dialect adapter
  -> normalized global logical-type registry
  -> declaration/default checks
  -> globally resolved strict-positive examples
  -> version-graph and local-reference checks
  -> explicit chain-profile parity mapping
  -> deterministic diagnostics and exit status
```

A normalized record must retain source path and ordinal so diagnostics can identify the declaring document, example document, logical type, example number, and field. Empty optional-string sentinels need an explicit policy: only a non-required field may bypass a non-empty pattern by being empty; non-empty values still full-match the pattern.

## Delivery and acceptance boundary

Batch 1A is one reviewable release candidate, even if authored as several local commits:

1. manifest plus exact dialect inventory;
2. duplicate-key loader, adapters, normalized registry, diagnostics, and CLI;
3. mandatory version changes and strict-positive example repairs/additions;
4. adversarial fixtures and explicit product-profile mapping;
5. one offline wrapper, README command, and workflow invoking that same wrapper.

Generate manifest digests only after all standard/example edits. Batch 1A exits only when a clean checkout and CI prove:

- exactly 21 source documents inventoried without a self-hash cycle;
- all supported dialects classified and an unknown dialect rejected;
- all 13 Nexus logical types require exact immutable `schema-ver: 0.2.0`;
- 13 valid, zero invalid, and zero missing strict-positive Nexus logical types;
- missing, malformed, wrong, and unknown versions reject for every type;
- duplicate key/ID/alias/type/field/required entry, stale digest, unresolved edge, unsupported scalar, bad mutability/definition, strict type mismatch, boolean-as-integer, integer under/overflow, UTF-8 plus-one, enum/pattern error, unknown example field/type, broken local link, and parity mismatch each produce a stable diagnostic and nonzero status;
- intentional Nexus/Solana representation and lifecycle differences are explicit;
- no draft, deployment, adoption, settlement, or production status is promoted by offline conformance alone.

Pinned Nexus queries, compiled Solana behavior, agent identity v0.2, functional-agent transfer, authorization, commerce/payment, accountability, and reputation remain later profiles. None may be inferred from owner-authored fields or local research notes.