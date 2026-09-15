# Development and architecture review — 2026-09-15

## Baseline and verdict

**Reviewed HEAD:** `00aa5649b52cd5e52dd13abc0e6e33dda4ea5891` (`main`), equal to freshly fetched `origin/main` (`0 0` ahead/behind). The only commit after the prior reviewed baseline `326ba2f63e363ed7fa9ea4111f5229fe931b42f6` is the 2026-09-12 review publication. The standards tree remains `d7bede155734827d88776421eadc8ac7839e5379`, unchanged from source baseline `83b9f0902a062d9a97089c2729889ea565d7af82`.

**Verdict: useful design catalog; not implementation-certified.** No source regression was found, but no blocker exited. No chain, wallet, profile/session, deployment or production call was made.

## Fresh evidence

| Gate | Result |
|---|---|
| Fetch/identity | **PASS** — local and `origin/main` both `00aa5649…`; clean start |
| Git integrity/whitespace | **PASS** — `git fsck --no-dangling --no-progress`; pre-edit `git diff --check` |
| Narrow standards probe | **PASS (structural only)** — all 21 `standards/*.json` files parsed; 0 duplicate names in arrays composed entirely of named field definitions |
| Local Markdown links | **PASS** — 12 Markdown files checked; 0 missing local targets |
| Checked-in conformance/CI | **ABSENT** — repository search found no executable validator/test/CI contract |
| Pinned-core queries, serializers, Anchor, consumer acceptance | **NOT RUN / ABSENT** |

Source identity is commit tree `a335d826431e3f1824b88495796205908d43b9fc`; the key source hashes recorded on 2026-09-12 remain byte-identical, including Solana product `0cc9ab…`, NexGo ride `0c4757…`, taxi `0807c9…`, rating `8909c0…`, design note `51e74c…`, and README `5e7a9b…`.

## Unchanged blockers and exact exits

1. **P1 — no executable conformance or normative query contract.** Parse/link checks do not validate required fields, UTF-8 sizes, integer bounds, examples, relations or the documented `LIKE`/unprefixed NexGo filters against the pinned Nexus grammar.
   - **Next repair:** check in one validator command and CI; separate pseudocode from transport-ready `results.<field>` fixtures with supported wildcards, escaping and pagination.
   - **Exit tests:** clean checkout rejects duplicate, overlength, out-of-range, mismatched-example, invalid-query and broken-reference fixtures; isolated pinned core executes open-request, offer, taxi-area and rating discovery across multiple pages.
2. **P1 — Solana product revisions still share one PDA seed set.** `rev` is not in the `product + authority + art-nr` derivation although `supersede` promises a new account.
   - **Exit tests:** revisions A/B derive distinct addresses; duplicate A, unauthorized supersession and identity reuse after closure reject; B links A without mutating A.
3. **P1 — physical serialization, authority and canonical identity parity remain unproved.** The 545-byte table is descriptive, and mutable Nexus `self-addr` remains a claim until canonical read-back.
   - **Exit tests:** compiled Anchor and pinned Nexus golden vectors pass boundary/multibyte round trips; forged self-address/owner/namespace fixtures reject.
4. **P2 — consumers are not migrated.** NexGo still writes legacy raw version-1 rides rather than the v0.2.0 request/offer/agreement protocol.
   - **Exit tests:** versioned adapters round-trip every supported writer/reader shape and reject unknown versions, forged references/owners and invoice-term mismatches.

## Handoff

Implement Batch 1 first; do not promote draft status from the fresh structural checks. Then resolve revision identity, serialization/authority parity, and consumer migration in that order.