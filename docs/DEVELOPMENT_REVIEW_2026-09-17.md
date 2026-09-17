# Development and architecture review — 2026-09-17

## Baseline and verdict

**Reviewed HEAD:** `a5f94125ebc07f02120f29b39d811ea3d67c658f` (`main`). The commit after the 2026-09-16 reviewed source HEAD `d0b9d04155035e970d1ba4149465a5838a2ca63b` is that review's documentation publication. A path-limited diff confirms no change under `standards/`; its tree remains `d7bede155734827d88776421eadc8ac7839e5379`.

**Verdict: unchanged useful design catalog; not implementation-certified.** No standard or release blocker changed. Verification was offline and made no chain, wallet, profile/session, deployment or remote request.

## Fresh offline verification

| Gate | Result |
|---|---|
| Identity and standards delta | **PASS** — HEAD `a5f94125…`; no `standards/` delta from the 2026-09-16 reviewed source |
| Git integrity / whitespace / scope | **PASS** — `git fsck --no-dangling --no-progress`, `git diff --check`; clean baseline and only the three intended review documents changed afterward |
| JSON and field-name structure | **PASS (narrow)** — all 21 standards JSON files parsed; 0 duplicate names across inspected `nexusFields` groups |
| Project Markdown links | **PASS** — 15 Markdown files after this review was written; 0 missing local targets |
| Nexus version-required probe | **FAIL** — 13 logical v0.2.0 Nexus asset types define immutable `schema-ver`; all 13 omit it from `required` |
| Solana product parity probe | **FAIL** — `accountLayout` contains `schema-ver`, `required` omits it, while `equivalentTo.guarantee` claims identical required-set parity with Nexus |
| Checked-in validator / fixtures / CI | **ABSENT** — no executable validator, test command, negative fixtures or tracked workflow |
| Pinned-core queries, serializers, Anchor and consumer acceptance | **NOT RUN / ABSENT** |

## Findings

1. **P1 — version discrimination remains internally contradictory and also crosses the claimed Solana parity boundary.** The previously identified 13 Nexus logical types still permit omission of `schema-ver` according to their own `required` arrays. Fresh inspection also confirms `standards/product-standard.v0.2.0.solana.json` makes the same omission while claiming required-set equivalence. This is added evidence on unchanged source, not a new regression.
2. **P1 — no executable conformance or normative query contract exists.** JSON parsing cannot establish UTF-8 byte bounds, integer ranges, examples, references or relationship semantics. The NexGo v0.2.0 specifications still publish unprefixed SQL-style `LIKE` filters without a pinned executable Nexus grammar or pagination fixture.
3. **P1 — “verified ride” still lacks a payment/ownership proof graph.** A passenger-owned agreement can mutate to `completed`; the rating reference alone does not prove canonical request/offer/agreement ownership, exact paid invoice terms, or one-rating-per-agreement uniqueness.
4. **P1/P2 — product revision and chain-realization claims remain designs.** Solana product revision creation still uses the same authority/article seed set, so the promised new account per revision is not derivable as specified. No compiled serializer, Anchor authority test or Nexus boundary measurement exists.
5. **P2 — consumer migration remains unimplemented.** NexGo still writes legacy raw version-1 rides rather than the draft typed request/offer/agreement protocol.

## Prioritized coding batches

### Batch 1A — mandatory versions and offline conformance gate (P1)

**Targets:** the seven Nexus v0.2.0 files `article`, `content`, `namespace`, `nexgo-rating`, `nexgo-ride`, `nexgo-taxi` and `product`; `standards/product-standard.v0.2.0.solana.json`; add `tools/validate_standards.py`, `tests/fixtures/valid/`, `tests/fixtures/invalid/`, and `.github/workflows/standards.yml`; document the command in `README.md`.

**Acceptance:** every one of the 13 logical Nexus types requires immutable exact `schema-ver: 0.2.0`; the Solana product required set matches its canonical logical contract; clean-checkout validation passes valid examples and rejects missing/wrong/unknown version, duplicate field/type identity, UTF-8 overlength, integer overflow, enum/pattern mismatch, inconsistent example and broken local reference fixtures.

### Batch 1B — transport-ready Nexus query contract (P1)

**Targets:** query sections in `standards/nexgo-ride-standard.v0.2.0.json`, `nexgo-taxi-standard.v0.2.0.json`, `nexgo-rating-standard.v0.2.0.json`, and `docs/nexgo-cluster-design-note.md`; add pinned metadata and requests under `tests/nexus/queries/`.

**Acceptance:** explanatory pseudocode is labeled non-executable; normative fixtures use the pinned core's actual `results.<field>` grammar, supported wildcard/escaping and explicit page/cursor semantics; an isolated pinned core executes open-request, offer, taxi-area and rating discovery across more than one page, and a later-page failure returns incomplete rather than empty/complete.

### Batch 2 — product revision identity and physical parity (P1)

**Targets:** `standards/product-standard.v0.2.0.json`, `standards/product-standard.v0.2.0.solana.json`, `docs/product-masterdata-mrp-analysis.md`; add binary golden vectors and future Anchor tests under `tests/product/`.

**Acceptance:** revisions A and B derive distinct immutable identities, duplicate A and unauthorized supersession reject, B references unchanged A, seed normalization/length is specified, and compiled Nexus/Solana serializers match measured boundary vectors and authority/mutability rules.

### Batch 3 — verified-ride graph and consumer migration (P1/P2)

**Targets:** the three NexGo v0.2.0 standards, `docs/nexgo-cluster-design-note.md`, conformance fixtures under `tests/nexgo/`, and a version matrix for `/home/brutus/github/NexGo`.

**Acceptance:** validation traverses canonical request owner/passenger, driver-owned offer/vehicle, passenger-owned agreement and exact paid invoice identity/recipient/provider/account/token/currency/integer amount; forged, withdrawn, unpaid, mismatched, mutable-status-only and duplicate-rating cases reject; NexGo contract tests round-trip each explicitly supported version and never relabel legacy raw rides as v0.2.0.
