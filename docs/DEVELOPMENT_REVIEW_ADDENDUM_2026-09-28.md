# Development review addendum — 2026-09-28

## Status, baseline, and effect

**Reviewed source/base:** local `main`, local `origin/main`, and fresh `git ls-remote origin refs/heads/main` all resolved to `2173ab5fe7702e012588c27f06fecf005d90ca7e`. The commit date is 2026-09-25 and its subject is `docs: define accountability evidence architecture and conformance gates`. The committed `standards/` tree is `d7bede155734827d88776421eadc8ac7839e5379`.

There are **zero committed changes** and zero tracked path changes after the requested base. The changes reviewed since that baseline are concurrent staged/unstaged documentation and local untracked strategy/research context. No existing dirty file was edited, staged, unstaged, committed, reset, stashed, or cleaned by this review.

If published, this addendum is the current development-status qualification of `docs/DEVELOPMENT_REVIEW_2026-09-25.md` and the staged 2026-09-21 review. Earlier dated reviews remain immutable historical evidence. The Sep25 conclusion still holds: this is a design catalog, not an implementation-certified standards suite. This review adds one material conformance finding: five checked-in v0.2.0 example values violate their own declared UTF-8 byte widths.

The local agent-asset commerce research and proposed strategy companion were inspected only as untracked context. This review does not present them as published, accepted, source-strategy changes, or verified statements about external protocol status.

## Concurrent work preserved

At review start the existing worktree contained:

- staged `docs/ARCHITECTURE.md` — index blob `dad904ca4a08cb988f493259f8b628c7e3e5ab90`;
- staged `docs/DEVELOPMENT_PLAN.md` — index blob `d74d8caa54183b0cf91f28274693de05154a545a`;
- staged new `docs/DEVELOPMENT_REVIEW_2026-09-21.md` — index blob `ba29509e0978b560b6b4a9c9dec35fc5d356e115`;
- unstaged `docs/standards-market-evaluation.md`;
- untracked `docs/AGENT_ASSET_COMMERCE_RESEARCH_2026-09-25.md`, `docs/PROPOSED_STRATEGY_UPDATE_AGENT_ASSETS_2026-09.md`, and `vision.md`.

The index SHA-256 before these two Sep28 documents were created was `f9e59b785c8271d4da5bbf8c583458129c0bcc9090e3d134a5ddf5f2c4f91a68`.

## Executed evidence

A dependency-free read-only probe was written and run from the Hermes scratch directory, not the repository. It parsed standards with duplicate-key rejection; validated declared Nexus fields, defaults, required sets, examples, enums, patterns, strict integer ranges, and UTF-8 byte widths; inventoried query filters and local Markdown links; checked manifest/gate presence; and recomputed the declared Solana layout and product field mapping.

| Check | Fresh result |
|---|---|
| Source/remote identity | **PASS** — local HEAD, local `origin/main`, and remote `main` are `2173ab5fe7702e012588c27f06fecf005d90ca7e` |
| Committed delta since base | **PASS / none** — 0 commits and 0 tracked paths |
| Standards tree | **PASS / unchanged** — `d7bede155734827d88776421eadc8ac7839e5379` |
| Strict JSON | **PASS (syntax)** — 21/21 parsed; 0 duplicate-key files; 0 parse errors |
| Canonical IDs | **PASS (uniqueness only)** — 21/21 documents have unique `$id` values |
| v0.2.0 declarations/defaults | **PASS (narrow)** — 8 Nexus files, 13 logical types, 0 declaration/default type, mutability, definition, enum, pattern, UTF-8-width, or unsigned-range errors |
| Wire-version declaration | **PASS (declaration only)** — 13/13 types declare exact immutable `schema-ver: 0.2.0` |
| Mandatory wire version | **FAIL** — 0/13 include `schema-ver` in `required`; Solana product also omits it |
| Positive examples present | **FAIL** — 10/13 types represented; article chunk, social reaction, and ride agreement absent |
| Strict-valid positive examples | **FAIL** — only 7/13 types have at least one example passing the declared type/required/enum/pattern/UTF-8-width checks |
| Legacy identity | **AMBIGUOUS** — 12/12 legacy documents say semantic `0.1.0` while `$id` ends `.v1.json`; 11/12 omit lifecycle `status` |
| Supersession graph | **FAIL** — 8/8 v0.2.0 `supersedesStandard` strings are not exact known canonical IDs or repository paths; no manifest exists |
| Query recipes | **FAIL / descriptive** — 22 filter strings inventoried; 3 use SQL `LIKE`; 0 use documented `results.` paths; no pinned-core transport/pagination fixtures exist |
| Local Markdown targets | **PASS** — 67 local targets checked; 0 missing before these addenda were added |
| Solana physical table | **PASS (declaration only)** — offsets contiguous; computed end 545 bytes equals the declared table |
| Product width mapping | **PASS (declaration only)** — 24/24 Nexus logical fields have width-compatible Solana rows under the document's address mapping |
| Product required/mutability parity | **FAIL** — equal required arrays both omit `schema-ver`; Nexus `self-addr` mutable post-create versus Solana immutable at init |
| Product revision identity | **FAIL** — PDA seeds contain no revision/predecessor identity although `supersede` promises a new account |
| Checked-in gate | **ABSENT** — no manifest, `tools/validate_standards.py`, tests directory, or standards workflow |
| Repository whitespace | **PASS before addenda** — `git diff --check` and `git diff --cached --check` returned no findings |

No Nexus serializer, node query, profile/session, invoice, transaction, wallet, Solana compiler, Anchor validator, deployment, or public API was executed. Structural success is not chain evidence.

## New conformance finding: checked-in examples violate declared widths

The strict probe found five example values whose UTF-8 byte lengths exceed their own `maxlength` declarations:

1. `standards/article-standard.v0.2.0.json` — the fallback `distordia-article` example's `next` is 60 bytes; field maximum is 56.
2. `standards/nexgo-rating-standard.v0.2.0.json` — the `nexgo-rating` example's `self-addr` is 64 bytes; field maximum is 56.
3. `standards/nexgo-rating-standard.v0.2.0.json` — the same example's `agreement` is 60 bytes; field maximum is 56.
4. `standards/nexgo-taxi-standard.v0.2.0.json` — the `nexgo-taxi` example's `license-cred` is 60 bytes; field maximum is 56.
5. `standards/nexgo-taxi-standard.v0.2.0.json` — the same example's `insurance-cred` is 60 bytes; field maximum is 56.

These are fixture defects, not proof of a live serialization failure. They matter because the examples are the repository's only positive evidence for those three logical types. Combined with the three absent examples, six of 13 types lack a strict-valid checked-in positive example.

**Exit:** repair the five values using canonical valid fixture identities, add the three missing logical-type examples, and make the validator execute all examples. Do not weaken field limits to accommodate placeholder strings without a pinned Nexus measurement.

## Findings and priority

### P1 — Batch 1A still blocks every later profile

The manifest, mandatory wire versions, strict validator, valid/invalid fixtures, and CI are still absent. The local research broadens future interoperability work but supplies no reason to bypass this gate. Adding production-looking agent, NFT, payment, escrow, attestation, or private-data schemas before canonical identity and executable conformance would multiply unresolved aliases and unverifiable claims.

**Exit:** one clean-checkout command validates all 21 documents, all 13 v0.2.0 logical types, manifest digests/edges, examples, negative fixtures, links, and declared parity; CI invokes the same command.

### P1 — Agent identity, authority, commerce, and settlement remain conflated in legacy fields

`agent-standard.json` is a legacy `0.1.0` design with no immutable wire discriminator. Its capability, endpoint, transaction, delegation, signing, and kill-switch values are declarations. `nft-standard.json` cannot safely serve as an agent identity or functional-agent transfer model. `swarm-standard.json` uses mutable escrow/progress/outcome/payment claims without an executable settlement state machine.

The local research supports a layered target: compact identity handle; signed service manifest; scoped authorization; optional workload evidence; protocol-neutral commerce session; rail-specific settlement; artifact/provenance/rights references; accountability events; deterministic reputation. It does not establish that any named external adapter is accepted or implemented.

**Exit after Batch 1A:** draft agent v0.2.0 as a logical identity envelope and keep A2A, external registries, authorization, commerce, payment, runtime-attestation, provenance, rights, metadata, job escrow, and private-functional-asset mappings in separately versioned profiles with positive and adversarial fixtures.

### P1 — Query and cross-chain claims remain descriptive

Three SQL-style `LIKE` occurrences remain: legacy player, v0.2.0 ride, and v0.2.0 taxi. None of the 22 inventoried filter strings uses the documented `results.` path form. This inventory does not itself prove that every non-`LIKE` string is invalid; the executable grammar must be pinned to a target core.

The 545-byte Solana table and 24/24 width mapping remain useful design checks, but the revision PDA collision and `self-addr` lifecycle mismatch prevent literal equivalence. No compiled program, golden vector, authority test, target network identity, or verified mirror edge exists.

**Exit after Batch 1A:** execute multi-page Nexus request/response fixtures against an isolated pinned core; then repair product revision identity and compile/test the Solana realization. Cross-chain equality requires an explicit directional mirror edge and finality evidence, not matching business keys.

### P0 before value movement — financial fields are not settlement controls

This repository contains no fund-moving implementation, so the review found specification hazards rather than an executed fund-loss path. No code should infer authorization, funding, escrow, payment, completion, slashing, release, or reputation finality from mutable catalog fields.

**Exit before any value-moving implementation:** exact chain-qualified asset/account identity, strict integer base units, persisted intent, reservation, idempotency identity, submitted and outcome-unknown states, restart-safe reconciliation, least-privilege resolver/executor separation, and authoritative read-back must be specified and exercised on an isolated network. No transaction or deployment is authorized by this review.

## Concrete coding sequence

1. **Manifest skeleton:** add all 21 current JSON documents to `standards/manifest.json`, preserving bytes and recording canonical ID, semantic version, lifecycle, logical standard, chain profile, path, SHA-256, aliases, and exact graph edges.
2. **Strict loader and meta-schema:** add duplicate-key rejection and dependency-neutral declaration/default/example validation in `tools/validate_standards.py`; diagnostics identify repository path, logical type, example, and field.
3. **Mandatory versions:** update the eight Nexus v0.2.0 files plus Solana product so all 13 logical types and the Solana required set require exact immutable `schema-ver: 0.2.0`.
4. **Positive fixtures:** repair the five width-invalid example references and add article-chunk, reaction, and ride-agreement examples; require 13/13 strict-valid types.
5. **Negative fixtures:** cover duplicate keys/fields/types, unsupported scalar, bad mutability metadata, UTF-8 boundary-plus-one, strict integer and unsigned bounds, enum/pattern mismatch, missing definitions/required values, wrong/unknown versions, inconsistent examples, stale digest, alias collision, unresolved edge, broken local reference, and product parity mismatch.
6. **One gate and CI:** expose one documented offline command; add unit tests and `.github/workflows/standards.yml` that invokes exactly that command; include `git diff --check` and local-link verification.
7. **Pinned Nexus queries:** only after Batch 1A, replace prose filters with exact transport fixtures, `results.<field>` grammar, wildcard/escaping rules, stable ordering/deduplication, complete pagination, and later-page error as `incomplete`.
8. **Future logical/profile work:** only after those exits, draft agent v0.2.0 and separate interoperability profiles. Accountability/reputation follows typed identity, authority, artifact, and settlement references. Fund-moving implementation remains later and independently gated.

## Source and context hashes

| Source/context | SHA-256 or Git identity |
|---|---|
| Source HEAD/base/remote `main` | `2173ab5fe7702e012588c27f06fecf005d90ca7e` |
| Committed `standards/` tree | `d7bede155734827d88776421eadc8ac7839e5379` |
| `.git/index` before Sep28 addenda | `f9e59b785c8271d4da5bbf8c583458129c0bcc9090e3d134a5ddf5f2c4f91a68` |
| staged `docs/ARCHITECTURE.md` worktree | `9f6fd9c1f7c5c8c0402768eea71879e60e72e4a044dbb8a218fa966082e2d8e9` |
| staged `docs/DEVELOPMENT_PLAN.md` worktree | `f49f08f0338204912f274214421a17f61ad1490cb5934f4f43657304701edc8d` |
| staged `docs/DEVELOPMENT_REVIEW_2026-09-21.md` worktree | `7fac5b37d503ec9d88a12f54558423629f06ce9d2632826cd94e36582341b1af` |
| unstaged `docs/standards-market-evaluation.md` | `9fd3cfa320f76184a446fdae49a90d52b6a58e1db54195f78425ec3c1f368ec3` |
| local research note | `dfd60c860a1f3b18b2bd45dff7fcb23b55af335ac5a886ea896b022a22fd5157` |
| local proposed strategy companion | `7208413fc5bd263c3f42c3cd99dad6deb9f4c68f84aaa6129764c906cd9a434b` |
| local `vision.md` | `2231687ca457642e60410a80d54ee6309e769b606f143a619d6ce406e7ea1de1` |
| committed Sep25 architecture addendum | `314e3a710393903eda9209fc8b2149e82e4a652794b5a61eb9f6c5f179a2591d` |
| committed Sep25 development review | `01424607bfd94b0a8d828ddb481ccd8ab7ca9014651793d71f0b09a2ff3d1944` |

The complete per-standard SHA-256 inventory and machine-readable diagnostics are in the local scratch findings artifacts returned with this review. Those artifacts are evidence for the review and are not repository publication dependencies.
