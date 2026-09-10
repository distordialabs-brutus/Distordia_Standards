# Development review — 2026-09-10

## Baseline and delta

**Reviewed pre-publication HEAD:** `e8f890e3f02e6aa1fda6a1af76ef3ac0df5c60bc` (`main`), equal to `origin/main` at review start.

**Prior dated review's source HEAD:** `d0e328cccebf15727e1152905898dc810742ac6e`.

`git diff --name-status d0e328c..e8f890e` contains only the 2026-09-09 architecture, plan and review documents. `standards/` is unchanged with Git tree `d7bede155734827d88776421eadc8ac7839e5379`. The worktree was clean. Retained findings below are unchanged design/implementation blockers, not regressions introduced since the prior review.

No Nexus/Solana call, profile/session action, deployment, wallet operation, invoice, program-address lookup, schema edit or production mutation was performed. Illustrative addresses remain non-deployment identities.

## Verdict

**Useful design catalog; not an implementation-certified standard.** The repository still has no executable conformance package/CI, deployed validator or Anchor program, and the Solana product revision contract cannot derive distinct append-only revisions from its documented PDA seeds.

## Retained findings and repair exits

1. **P1 — Solana product revision identity is unresolved.** `standards/product-standard.v0.2.0.solana.json` promises a new account for `supersede`, but its PDA seeds remain `product + authority + art-nr`; immutable `rev` is absent. The same authority/article therefore cannot derive distinct successor accounts under the stated contract.
   - **Exit:** choose a stable head plus revision PDAs or a versioned revision-inclusive seed contract; prove revisions A/B differ, duplicate A fails, B references unchanged A, unauthorized and concurrent supersession fail, and closure cannot reuse identity unexpectedly.
2. **P1 — conformance remains descriptive.** There is no checked-in validator/test command or CI workflow for required fields, types, UTF-8 limits, integer bounds, examples, lifecycle/reference semantics or links.
   - **Exit:** one clean-checkout command rejects deliberately invalid duplicate, overlength, out-of-range, mismatched example and broken-reference fixtures.
3. **P1 — logical/physical parity claims remain too strong.** Nexus `self-addr` is mutable for post-create stamping while Solana `self_addr` is initialization-only. A contiguous 545-byte declared layout does not prove compiled Borsh layout, authority enforcement or Nexus encoded-size compliance.
   - **Exit:** publish a field-by-field equivalence table, golden serializers, pinned Nexus byte measurements and compiled Anchor layout/authority tests; canonical chain address/owner must override claimed self identity.
4. **P2 — consumer migration is unproved.** NexGo still writes a legacy raw version-1 ride blob, not the v0.2.0 typed request/offer/agreement handshake.
   - **Exit:** publish a consumer/version matrix and executable writer/reader fixtures that reject unknown versions, forged ownership/cross-references and invoice term mismatches.

## Executed evidence

| Gate | 2026-09-10 result |
|---|---|
| Branch/remote identity and clean start | **PASS** — local and remote `e8f890e3f02e6aa1fda6a1af76ef3ac0df5c60bc`; clean worktree |
| Prior-review delta and standards tree identity | **PASS** — documentation-only delta; standards tree unchanged |
| Temporary offline structural probe | **PASS (narrow)** — 21 JSON files parsed; no detected duplicate field names, missing required fields/definitions, unsupported Nexus scalar types, default/enum byte overruns, integer-default range errors or Solana offset gaps; declared Solana layout 545 bytes; `rev` still absent from PDA seeds |
| Ordered `standards/*.json` SHA-256 manifest | **PASS** — `da20bade072dafb4bc41389f63724e41937904a6c31a936ecf5be0d78887fbb0` |
| Local Markdown links | **PASS** — 10 tracked Markdown files, 61 local links, 0 broken before edits |
| `git diff --check` before edits | **PASS** |
| Checked-in conformance/CI, compiled Anchor and live chain acceptance | **ABSENT / NOT RUN** |

Commit tree at the reviewed head: `d5b3135c422cb20dfd6d57ac0d666d097dbe37eb`. The temporary probe is not a release gate and does not establish semantic examples, actual serialized byte budgets, ownership/delegation, chain identity, invoice binding or consumer compatibility.

## Repair handoff

Owner should land four independently reviewable batches: (1) executable conformance and CI, (2) revision identity decision plus tests before Anchor work, (3) cross-chain identity/serialization vectors, then (4) consumer migration and isolated local-validator/test-profile acceptance. Draft status must not be promoted from structural parsing alone.
