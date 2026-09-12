# Development and architecture review — 2026-09-12

## Baseline and delta

**Reviewed pre-publication HEAD:** `326ba2f63e363ed7fa9ea4111f5229fe931b42f6` (`main`), equal to `origin/main` at review start.

**Prior 2026-09-10 review source HEAD:** `e8f890e3f02e6aa1fda6a1af76ef3ac0df5c60bc`. **Standards source baseline:** `83b9f0902a062d9a97089c2729889ea565d7af82`.

No commit follows the 2026-09-10 review publication. `git diff 326ba2f..HEAD` is empty, the worktree was clean, and `standards/` remains Git tree `d7bede155734827d88776421eadc8ac7839e5379`. Findings are pre-existing design/implementation blockers, not post-review regressions. No Nexus/Solana call, profile/session action, deployment, wallet operation, invoice, program-address lookup, schema edit or production mutation was performed.

## Verdict

**Useful design catalog; not an implementation-certified standard.** Prior revision-identity, conformance, serialization/authority and consumer-migration blockers remain. This review adds one actionable architecture gap: the advertised Nexus discovery strings conflict with the documented query DSL, so geo-sharded NexGo discovery is currently pseudocode rather than a verified callable contract.

## Findings and repair exits

1. **P1 — executable conformance remains absent (retained).** There is no validator/test command or CI workflow for required fields, scalar types, UTF-8 limits, integer bounds, examples, relationships, lifecycle or links.
   - **Exit:** one clean-checkout command rejects deliberately invalid duplicate, overlength, out-of-range, mismatched-example, invalid-query and broken-reference fixtures.
2. **P1 — normative Nexus query syntax is not pinned or executable (newly documented, pre-existing).** Current Nexus query documentation uses `results.<field>` and `*` wildcards (`NexGo/Nexus API docs/QUERIES.md:7-50`). The Standards repository instead uses unprefixed fields throughout its `queryFilter` values and uses SQL-style `LIKE` for NexGo geo-discovery (`standards/nexgo-ride-standard.v0.2.0.json:154-162`, `standards/nexgo-taxi-standard.v0.2.0.json:71-76`, `docs/nexgo-cluster-design-note.md:125-131`). No checked-in test proves these strings are accepted.
   - **Exit:** separate explanatory pseudocode from transport-ready filters; pin core SHA/version; use the supported `results.<field>`/operator/wildcard grammar; escape values; add complete pagination; execute open-request, offer, taxi-area and rating fixtures against an isolated core.
3. **P1 — Solana product revision identity remains unresolved (retained).** `standards/product-standard.v0.2.0.solana.json:21-24` derives a PDA from `product + authority + art-nr`, while `:114-116` promises a new account for each revision; immutable `rev` is not a seed.
   - **Exit:** choose stable-head plus revision PDAs or a versioned revision-inclusive seed contract; prove distinct A/B revisions, duplicate rejection, predecessor immutability, authority enforcement, concurrency policy and identity non-reuse after closure.
4. **P1 — logical/physical parity remains unproved (retained).** Nexus `self-addr` is conventionally stamped through a mutable field while Solana initializes immutable `self_addr`; a declared 545-byte contiguous table is not compiled Borsh, authority or Nexus encoded-size evidence.
   - **Exit:** publish field-level equivalence, golden serializers, pinned Nexus byte measurements and compiled Anchor layout/authority tests; canonical chain address/owner must override claimed self identity.
5. **P2 — consumer migration remains unproved (retained).** NexGo still writes legacy raw `nexgo-ride` version 1 data, not the typed v0.2.0 request/offer/agreement handshake, and it has no conformance adapter.
   - **Exit:** publish a consumer/version matrix and executable writer/reader fixtures that reject unknown versions, forged ownership/cross-references and invoice term mismatches.

## Source identity

Commit tree: `c8350ae40a82815262c28c9b5de9de15880f05c7`.

| Reviewed file | SHA-256 |
|---|---|
| `standards/product-standard.v0.2.0.solana.json` | `0cc9ab0148cfc440a37ac06ac115d266cba30882d2bfb996aa1a178e7d3dde2e` |
| `standards/nexgo-ride-standard.v0.2.0.json` | `0c47571dace004d9e9b1ce78231fe3cf0ed9ec21d6178850c569ab139a775d47` |
| `standards/nexgo-taxi-standard.v0.2.0.json` | `0807c99d4f4e23a9e3a4072345d6a7dd1c6aef5db67fa7ef5c19d46edcc1e3b4` |
| `standards/nexgo-rating-standard.v0.2.0.json` | `8909c0cf88cb5ea67cad4064a613da67e2bdbda3a609b6c90b80fee18e824748` |
| `docs/nexgo-cluster-design-note.md` | `51e74c530c0ce708e72fd4c74201a4f8fd781ff8bd65ec158e31487566fc864f` |
| `README.md` | `5e7a9ba870835b041da7f60c9390479bd4c9180fcc2e6233f49a9d89900a46ea` |

## Executed evidence

| Gate | 2026-09-12 result |
|---|---|
| Branch/remote identity and clean start | **PASS** — local and remote `326ba2f63e363ed7fa9ea4111f5229fe931b42f6`; `0 0` ahead/behind; clean worktree |
| Delta and standards tree continuity | **PASS** — no commit/file delta; standards tree unchanged |
| Query-contract comparison | **FAIL (static contract)** — current Nexus docs specify `results.<field>` plus `*`; v0.2.0 NexGo discovery specifies unprefixed fields plus undocumented `LIKE`; no executable fixture exists |
| Git object and whitespace checks | **PASS** — `git fsck --no-dangling --no-progress` and pre-edit `git diff --check` exited 0 |
| Fresh structural/link probe | **NOT RUN** — the prior temporary probe is not a checked-in gate and the denied review probe was not rerouted |
| Checked-in conformance/CI, compiled Anchor and live chain acceptance | **ABSENT / NOT RUN** |

The 2026-09-10 temporary structural probe remains historical evidence only: 21 JSON files parsed with no detected narrow structural errors, and the declared Solana layout totaled 545 bytes. It did not validate query grammar, semantic examples, actual serialization, ownership, chain identity, invoice binding or consumer compatibility.

## Repair handoff

Land independently reviewable batches in this order: (1) executable conformance plus pinned query fixtures and CI, (2) product revision identity decision and tests, (3) cross-chain identity/serialization vectors, then (4) consumer migration and isolated local-validator/test-profile acceptance. Do not promote draft status from descriptive or parse-only evidence.