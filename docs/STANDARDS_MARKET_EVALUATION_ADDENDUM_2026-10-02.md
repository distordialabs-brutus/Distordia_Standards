# Standards market-evaluation addendum — evidence qualification — 2026-10-02

## Scope and current authority

> **Current source qualification:** published source is now remote tip `e590a2af627c8f2b6c8a0fa9c6f5642f04652982`; `standards/` is unchanged at tree `d7bede155734827d88776421eadc8ac7839e5379`. Local-only `f30b183a34af4fd7a9f5757ade15a3038f8cd2ce` is a divergent sibling from merge base `db3da5626f270d5d411c4f9eb4b6d9e706891b10`, not an ahead commit or published evidence. Its agent/commerce research remains venture hypothesis context. The source conclusions below still apply because the standards bytes are unchanged.

This documentation-only addendum qualifies `docs/standards-market-evaluation.md` against source commit `db3da5626f270d5d411c4f9eb4b6d9e706891b10` and committed `standards/` tree `d7bede155734827d88776421eadc8ac7839e5379`. The requested baseline equals reviewed HEAD, so there is no committed implementation or specification delta to change a readiness verdict.

The existing market comparison remains design analysis. External protocol versions and lifecycle labels in the local agent-asset/commerce research were not revalidated in this repository review. That research is not published implementation evidence and does not promote any Distordia standard.

## Current evaluation

The repository remains a design catalog, not an implementation-certified suite. The earlier market-fit labels should be read with the following evidence qualifier:

- **Beta** means the logical direction may be credible; it does not mean canonical wire identity, strict examples, executable conformance, chain behavior, consumer compatibility, deployment, or adoption has been proved.
- **Prototype** means material architecture or trust boundaries remain unresolved; checked-in JSON does not demonstrate enforceable execution.
- No standard is **near-production** on repository evidence because the common offline conformance foundation is absent.

The immediate cross-cutting market risk is not another feature gap. It is the lack of a canonical manifest, mandatory wire discrimination, strict-valid fixtures, and a checked-in conformance/CI gate. Until Batch 1A lands, consumers cannot deterministically answer which specification and chain profile a record implements or whether a checked-in example satisfies its own declaration.

An independent strict-positive/adversarial scratch probe confirms that blocker without changing the verdict: all 21 JSON documents parse; the 13 generated declaration-complete positives pass; checked-in examples cover only 7/13 types strictly (3 invalid-only, 3 missing); and 93/106 adversarial cases reject. All 13 unexpected accepts are missing-`schema-ver`, one per logical type. The probe is diagnostic only, lives outside the repository, and is neither the missing maintained validator nor shipped conformance evidence.

## Accepted design progress, not implementation

The following conclusions are accepted as architecture direction only:

1. **Agent identity and functional-agent assets are distinct.** A compact identity/control handle must not be treated as a container for prompts, memory, credentials, private data, or transaction history. Transfer of a token does not prove transfer of encrypted functionality, deletion of seller copies, access continuity, rights, provenance, quality, or safe behavior.
2. **Identity, authority, commerce, and settlement are separate evidence layers.** Current owner-authored agent permission fields, swarm escrow/payment fields, and mutable status values do not prove enforcement, funding, completion, or settlement.
3. **Protocol integrations require exact versioned profiles.** Agent cards, authorization, workload evidence, provenance/rights, commerce sessions, payment rails, accountability, and reputation must be separately versioned adapters with positive and adversarial fixtures. A protocol name or receipt string is not semantic acceptance.
4. **Reputation is derived evidence.** Mutable reputation, mission, slashing, quality, or payment counters remain display claims until a deterministic versioned view replays finalized attributable events.
5. **Offline conformance precedes new schemas.** Agent v0.2, NFT functional-asset transfer, commerce/payment, accountability, and reputation work must not precede Batch 1A.

These conclusions do not establish that any named external standard is accepted, compatible, implemented, deployed, or current.

## Evidence-adjusted readiness

| Area | Current qualification | Evidence blocker |
|---|---|---|
| Namespace, content, social, product, article, NexGo v0.2 drafts | design candidates only | optional `schema-ver`; no canonical manifest or executable gate |
| Strict-positive examples | incomplete | only 7/13 Nexus logical types have a complete valid checked-in example |
| Agent registration | legacy beta design, not an authorization control | no immutable wire version; no wallet/endpoint/runtime enforcement profile |
| NFT / functional-agent transfer | prototype | no private-data handoff, access epoch, verifier, recovery, rights, or transfer-reset profile |
| Swarm / commerce | beta concept, non-executable | escrow, progress, outcome, and payment remain mutable declarations |
| Product Solana realization | descriptive draft | no revision-safe PDA, compiled serializer, authority tests, or local-validator evidence |
| NexGo discovery/rating/payment claims | descriptive drafts | unpinned query grammar/pagination and no authoritative ownership/payment graph fixtures |
| Deployment/adoption | unproved | no consumer matrix, exact-profile contract tests, or exact-head CI |

## Strict-positive market-readiness blocker

The unchanged source has 13 Nexus v0.2 logical types but only seven with a strict-valid positive example. Three are present but invalid because five values exceed declared UTF-8 widths: article root `next`; rating `self-addr` and `agreement`; taxi `license-cred` and `insurance-cred`. Three more types have no example: article chunk, social reaction, and ride agreement.

This is not merely editorial. A market-facing standard whose only example violates its own declared wire constraint gives implementers contradictory guidance. Repair examples and enforce them in CI before expanding profile scope or readiness claims.

## Priority order

1. **P0 evidence containment:** do not infer funding, permission, completion, settlement, custody, deployment, or adoption from descriptive fields or research notes.
2. **P1 Batch 1A:** 21-document manifest; explicit dialect adapters; global logical registry; mandatory versions; 13/13 strict-valid positives; adversarial fixtures; stable diagnostics; one offline command and CI.
3. **P1 transport/chain evidence:** pinned Nexus query and pagination fixtures; measured Nexus serialization; revision-safe Solana PDA; compiled serialization/authority/local-validator tests.
4. **P1 agent and commerce profiles:** only after Batch 1A, draft minimal agent identity plus separate authority, artifact, commerce, settlement, accountability, and reputation profiles.
5. **P2 consumer/adoption evidence:** publish exact canonical IDs/digests and contract-test matrices before changing any readiness or adoption label.

The exact coding and fixture split, source-identity qualification, and gate outcomes are in [the 2026-10-02 development review addendum](DEVELOPMENT_REVIEW_ADDENDUM_2026-10-02.md); the architectural acceptance boundary is in [the companion architecture addendum](ARCHITECTURE_ADDENDUM_2026-10-02.md).
