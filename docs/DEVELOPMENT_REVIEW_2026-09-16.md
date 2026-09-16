# Development and architecture review — 2026-09-16

## Baseline and verdict

**Reviewed HEAD:** `d0b9d04155035e970d1ba4149465a5838a2ca63b` (`main`), equal to freshly fetched `origin/main` (`0 0` ahead/behind). The only commit after the 2026-09-15 reviewed HEAD `00aa5649b52cd5e52dd13abc0e6e33dda4ea5891` is that review's documentation publication. No standard changed; `standards/` remains tree `d7bede155734827d88776421eadc8ac7839e5379`.

**Verdict: useful design catalog; not implementation-certified.** Fresh structural checks pass, but no blocker exited and a systemic version-contract gap was isolated. No chain, wallet, profile/session, deployment or production call was made.

## Fresh evidence

| Gate | Result |
|---|---|
| Fetch/identity | **PASS** — local and `origin/main` both `d0b9d041…`; clean start; `0 0` ahead/behind |
| Git integrity/whitespace | **PASS** — `git fsck --no-dangling --no-progress`; pre-edit `git diff --check` |
| Narrow standards probe | **PASS (structural only)** — all 21 `standards/*.json` files parsed; 0 duplicate names in inspected `nexusFields` groups |
| Local Markdown links | **PASS** — 13 Markdown files checked with URL-decoded local targets; 0 missing |
| Version-required probe | **FAIL** — 13 logical v0.2.0 Nexus asset types define immutable `schema-ver`; all 13 omit it from `required` |
| Checked-in conformance/CI | **ABSENT** — no executable validator/test/CI contract or checked-in YAML workflow |
| Pinned-core queries, serializers, Anchor, consumer acceptance | **NOT RUN / ABSENT** |

Selected source hashes remain unchanged: NexGo taxi `0807c99d…`, rating `8909c0cf…`, ride `0c47571d…`, Solana product `0cc9ab01…`, README `5e7a9ba8…`.

## Findings and exits

1. **P1 — version discrimination contradicts the stated fail-closed policy.** All 13 v0.2.0 logical Nexus asset types carry `schema-ver: "0.2.0"` as an immutable field but omit `schema-ver` from `required`. A validator following the catalog can therefore accept an unversioned record even though the architecture requires unknown versions to reject or remain read-only.
   - **Next repair:** make the exact version discriminator mandatory in every versioned type and implement one validator/CI command.
   - **Exit tests:** missing, wrong and unknown versions fail for every logical type; valid examples and supported compatibility fixtures pass.
2. **P1 — no executable conformance or normative query contract exists.** Parse/link checks do not validate UTF-8 sizes, integer bounds, examples, relationships or current SQL-style `LIKE`/unprefixed NexGo filters against a pinned Nexus grammar.
   - **Exit tests:** clean checkout rejects duplicate, overlength, out-of-range, mismatched-example, invalid-query and broken-reference fixtures; isolated pinned core executes open-request, offer, taxi-area and rating discovery across multiple pages.
3. **P1 — “verified ride” is not established by mutable agreement status.** The passenger owns the agreement and can mutate `status` to `completed`; the rating draft references that agreement but does not itself establish canonical ownership links or paid invoice evidence. Physical completion is explicitly an oracle boundary.
   - **Next repair:** define a validation graph over canonical request owner/passenger namespace, driver-owned offer/vehicle, passenger-owned agreement and exact paid invoice terms/evidence. Define one-rating-per-agreement duplicate handling; do not treat `status=completed` alone as proof.
   - **Exit tests:** forged owners/namespaces, unrelated/withdrawn offer, unpaid or wrong-recipient/provider/account/token/currency/amount invoice, mutable-status-only completion and duplicate ratings reject.
4. **P1/P2 — prior identity/serialization and migration blockers remain.** Solana product revisions still share one PDA seed set; physical serialization/authority parity is unproved; NexGo still writes legacy version-1 raw rides.
   - **Exit tests:** distinct immutable revision addresses and unauthorized-supersession rejection; compiled Nexus/Anchor boundary vectors; consumer adapters that round-trip each supported version and reject forged relations.

## Handoff

Implement Batch 1 first: exact mandatory version discriminators, one executable validator/CI command and transport-ready paginated query fixtures. Then resolve product revision identity, serialization/authority parity and the paid-invoice/verified-rating relationship before consumer migration or draft promotion.