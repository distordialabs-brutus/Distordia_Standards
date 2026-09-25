# Development and architecture review — 2026-09-25

## Baseline, scope and verdict

**Reviewed source:** local `main` at `7b42a03d40dbe60eeaca7c08fa5212bb95092d44`, equal to the requested commit and the local `origin/main` tracking ref at review start. The committed `standards/` tree is `d7bede155734827d88776421eadc8ac7839e5379`. There is no tracked or untracked standards delta from HEAD.

**Concurrent work preserved:** the index already contained only `docs/ARCHITECTURE.md`, `docs/DEVELOPMENT_PLAN.md`, and `docs/DEVELOPMENT_REVIEW_2026-09-21.md`. Untracked `vision.md` was inspected as strategic input. None of those four files was changed, staged, unstaged, committed, stashed, reset, or cleaned by this review. This review adds only this file and `docs/ARCHITECTURE_ADDENDUM_2026-09-25.md`, both untracked.

**Verdict:** the repository remains a useful design catalog, not an implementation-certified standard suite. The staged 2026-09-21 assessment remains substantially correct, but the untracked vision raises an additional architectural priority: the catalog has no accountability-bond, staked-claim, challenge, verdict, or reproducible reputation contract. Existing mutable stake, escrow, reputation, permission, mission and payment fields are declarations, not enforceable evidence. The first implementation batch should still be the canonical manifest and offline conformance gate; no fund-moving accountability implementation should precede it.

This was a documentation-only, offline review. It made no Nexus, Solana, wallet, profile/session, deployment, transaction, invoice, escrow, token, remote API, or production call. Historical remote/CI claims in the staged 2026-09-21 review were read as historical evidence and were not represented as fresh results.

## Evidence executed on 2026-09-25

| Check | Fresh result |
|---|---|
| Source identity | **PASS** — `HEAD` and requested SHA are `7b42a03d40dbe60eeaca7c08fa5212bb95092d44`; branch `main` |
| Standards scope | **PASS / unchanged** — `git diff HEAD -- standards` is empty; tree `d7bede155734827d88776421eadc8ac7839e5379` |
| Repository integrity | **PASS** — `git fsck --no-dangling --no-progress` returned no findings |
| Existing worktree/index whitespace | **PASS** — `git diff --check` and `git diff --cached --check` returned no findings before creating these documents |
| Strict JSON | **PASS (syntax only)** — 21/21 standards parsed with duplicate object-key rejection; zero duplicate canonical `$id` values |
| v0.2.0 field/default structure | **PASS (narrow)** — eight Nexus v0.2.0 files contain 13 logical types; no duplicate logical field name, missing field definition, unsupported declared Nexus scalar, invalid mutability flag, invalid default scalar, default UTF-8 overflow, or default unsigned-range error was found |
| v0.2.0 wire discriminator declaration | **PASS (declaration only)** — 13/13 logical types declare immutable exact `schema-ver: 0.2.0` |
| v0.2.0 mandatory discriminator | **FAIL** — 0/13 logical types include `schema-ver` in `required` |
| Positive example coverage | **FAIL** — 10/13 logical types have a positive example; missing `distordia-article-chunk`, `distordia-reaction`, and `nexgo-ride-agreement` |
| Canonical version graph | **FAIL** — 8/8 v0.2.0 `supersedesStandard` strings are unresolved free text; no manifest exists |
| Legacy identity | **AMBIGUOUS** — 12 documents report semantic version `0.1.0` while `$id` ends `.v1.json`; 11/12 omit lifecycle `status` |
| Checked-in conformance gate | **ABSENT** — no `standards/manifest.json`, `tools/validate_standards.py`, `tests/`, or workflow file exists |
| Local Markdown targets | **PASS before these artifacts** — 17 Markdown files, 67 local targets checked, zero missing; this included untracked `vision.md` and its four local strategic-source references |
| Nexus query recipes | **FAIL / unpinned** — three SQL-style `LIKE` occurrences were found: v0.2 ride, v0.2 taxi, and legacy player. Current documented Nexus query grammar uses `results.<field>` and `*` wildcards; no pinned-core request/pagination fixture exists |
| Solana product layout | **PASS (declared table only)** — offsets are contiguous and total 545 bytes |
| Product logical width mapping | **PASS (declared table only)** — 24/24 Nexus logical fields have a Solana row with compatible declared width under the document's address mapping |
| Product required-set parity | **EQUAL BUT INVALID** — Nexus and Solana arrays match, but both omit `schema-ver` |
| Product mutability parity | **FAIL** — `self-addr` is mutable post-create on Nexus and immutable at initialization on Solana despite the blanket identical-mutability guarantee |
| Product revision identity | **FAIL** — `supersede` promises a new account while PDA seeds contain no revision/predecessor identity |
| Compiled/live chain evidence | **ABSENT / not run** — no Anchor program, Borsh golden vectors, pinned Nexus serializer measurement, isolated-core query fixture, consumer contract suite, or accountability testnet lifecycle exists |

The independent temporary probe exited `1` with eight release-blocker classes. That expected nonzero status is a review result, not a repository test failure: there is no checked-in repository gate. Probe SHA-256 was `954e98d7ace370762947c70fa4c02183734fe4915c4dbde471ba1dbc79866949`; captured output SHA-256 was `fd40fcc3ddbbf1729dac2a431e9ab28322870ba496e42d075086355df602bba7`.

A later supplemental one-off command intended to enumerate every query-filter string was denied by unattended approval policy. It was not retried through another shell shape, Python path, or subagent. Therefore this review records the three freshly counted `LIKE` occurrences and source-inspected unprefixed examples, but does not claim a fresh exhaustive count of every nonconforming query string.

## Findings and executable exits

### P0 — Economic and authority fields are claims, not controls

The current catalog contains money- and authority-adjacent fields without an enforceable state model:

- `namespace-standard.v0.2.0.json` has mutable `stake`, `tier`, `slashing-events`, and `reputation`, plus fee and lock-period tables, but no canonical token identity, decimals, escrow/lock reference, funding proof, unbonding transition, slash authority, or payout evidence.
- `agent-standard.json` has mutable `can-transact`, `max-tx-value`, delegation/signing flags, kill-switch and kill-authority strings. No runtime policy engine or chain authority binding proves that those limits are enforced.
- `swarm-standard.json` has mutable `escrow`, mission progress/outcome/final payment and owner-writable track-record counters, and states that 2% is retained by Distordia. It defines neither custody primitive nor settlement state machine, idempotency, unknown-outcome recovery, exact token identity, independent verdict authority, nor read-back reconciliation.
- `README.md` presents verification fees, mission escrow, payment guarantees and a list of dApps “using” standards more strongly than the repository's evidence supports.

These are not proven fund-loss paths because this repository contains no implementation. They are specification hazards: a consumer could mistake self-authored metadata for escrow, authorization, reputation, or settlement proof. This also conflicts with the reviewed strategic requirement that Distordia remain a standard-setter, avoid custody, and derive trust from attributable events.

**Exit:** maintained documentation labels all economic tables and execution claims as proposed until chain evidence exists. Validators and consumer tests must refuse to infer funding, permission, completion, slashing, or payment from mutable catalog fields. Before any value-moving implementation is enabled, exact asset identity and integer units, intent-first states, idempotency identity, timeout/unknown-outcome handling, slash/payout authority, human disposition, and authoritative transaction/read-back evidence must be specified and exercised on an isolated network.

### P1 — Canonical specification/version identity and conformance are still missing

The 13 v0.2.0 logical types declare a discriminator but make it optional. Legacy identity mixes `v1` IDs with `0.1.0` semantic versions. Supersession and Nexus/Solana equivalence are prose strings rather than graph edges. There is no canonical registry, executable meta-schema, negative fixture corpus, test command, or CI workflow.

**Exit command target:**

```bash
python3 tools/validate_standards.py --repo .
python3 -m unittest discover -s tests -p 'test_*.py'
git diff --check
```

All three commands must pass from a clean checkout and in CI. The validator must reject duplicate JSON keys, duplicate canonical IDs and aliases, stale manifest digests, unresolved version/equivalence edges, missing or unknown wire versions, duplicate logical fields/types, unsupported scalars, invalid mutability metadata, UTF-8 byte overflow, unsigned under/overflow, enum/pattern mismatch, inconsistent examples, broken local references, and product-profile parity violations. Parameterized fixtures must cover all 13 v0.2.0 types.

### P1 — The vision's accountability event graph does not exist

The reviewed `vision.md`, staked-accountability strategy and infrastructure buildout call for four public primitives: accountability bond, staked claim, challenge, and verdict. No current standard defines any of them. Existing `agent`, `namespace`, and `swarm` records cannot be stretched into these roles without conflating identity, collateral, assertion, objection, resolution and execution authorities.

**Exit command target:**

```bash
python3 -m unittest discover -s tests/accountability -p 'test_*.py'
```

The suite must validate one complete bond → claim → challenge → verdict graph and reject wrong owner/delegation, wrong chain/network, unfunded/expired/insufficient bond, scope mismatch, late or duplicate challenge, self-resolution, unknown verifier method/version, verdict without retained evidence, duplicate or conflicting verdict, unauthorized slash instruction, and any attempt to treat proposed/approved execution as settled. Schemas must stay within measured Nexus limits and use exact immutable wire versions.

### P1 — Reputation is mutable metadata instead of a reproducible view

`namespace.reputation`, `namespace.slashing-events`, `swarm.reputation`, `swarm.missions-*`, and product `dq-score` are mutable counters. Their writers, event inputs, ordering, finality, algorithm version and replay rules are not normative. They cannot satisfy the vision's independently reproducible reputation requirement.

**Exit command target:**

```bash
python3 -m unittest discover -s tests/reputation -p 'test_*.py'
```

Golden event streams must produce an exact versioned score and component breakdown under forward and shuffled ingestion, idempotent replay, duplicate events, chain reorganization/finality changes, unresolved challenges, appeals and algorithm upgrades. The authoritative output is a derived view over immutable attributable events; mutable scores in legacy records remain untrusted display hints.

### P1 — Product cross-chain equivalence is overclaimed

The single Solana realization is descriptive JSON containing a Rust sketch, not a deployed program. Its 545-byte offset table and 24-field width mapping are useful design evidence, but not chain equivalence:

- chain/network/program/register identity is absent from a canonical manifest;
- field equality such as `(mfr, art-nr)` or `gtin/mpn` cannot prove that two records are mirrors or that their authorities correspond;
- Nexus `self-addr` stamping and Solana PDA initialization have different lifecycle and mutability;
- Solana timestamps are signed `i64` while Nexus system timestamps are described as `uint64`;
- PDA seeds do not distinguish revisions and do not specify canonical UTF-8 normalization;
- zero padding, interior NUL, UTF-8, enums, authority, predecessor and close/reinitialize behavior are not program-tested;
- the Token-2022 alternative loses numeric typing and offset parity and therefore needs a separate realization profile, not an informal alternative under the same guarantee.

**Exit command targets:**

```bash
python3 tools/verify_cross_chain.py --manifest standards/manifest.json --vectors tests/product/cross-chain
cargo test --locked --manifest-path implementations/solana/product-registry/Cargo.toml
(cd implementations/solana/product-registry && anchor test)
```

Golden vectors must bind logical spec digest, chain, network, canonical source address, destination account/program, mapper version and authority evidence. Revisions A and B must derive distinct immutable addresses; duplicate, wrong-predecessor, unauthorized, forked-successor and close/reinitialize cases must reject. “Equivalent” may be asserted only by an explicit mirror/attestation edge with verified effects on both chains, never by matching business fields alone.

### P1 — Query, pagination and relationship semantics remain descriptive

Three SQL-style `LIKE` strings are not transport-ready under the current documented Nexus query grammar. Other inspected recipes also use unprefixed logical fields rather than pinned `results.<field>` request fixtures. No query pins a core SHA, escaping rules, complete page enumeration or later-page failure behavior.

NexGo's typed request/offer/agreement redesign improves consent, pricing and location privacy. It still cannot prove a ride or payment from mutable agreement state. Rating validity requires authoritative request, offer, vehicle, owner/delegation, invoice, recipient/provider/account/token/currency/amount and transaction evidence, plus deterministic duplicate handling. Physical trip completion remains an oracle boundary.

**Exit command targets:**

```bash
python3 -m unittest discover -s tests/nexus/queries -p 'test_*.py'
python3 -m unittest discover -s tests/nexgo -p 'test_*.py'
python3 tools/run_nexus_acceptance.py --suite queries,nexgo --network private --core-sha "$NEXUS_CORE_SHA"
```

The live-isolated suite must enumerate more than one page and hold completeness on any later-page error. It must reject SQL-only syntax, malformed escaping, forged ownership/reference, withdrawn offers, mismatched or unpaid invoices, mutable-status-only completion, and duplicate ratings. No profile credential may be sent to a foreign/public node.

### P2 — Maintained documentation and adoption claims lag the evidence model

`README.md` still leads readers to the 2026-09-08 review, says all core data uses JSON despite documented raw legacy standards, says every standard includes version identifiers despite undiscriminated wire records, presents the public API and economic amounts as operational facts, and labels repositories as using the standards without contract-test/version evidence. These are maintained-document issues, not defects to rewrite inside dated historical reviews.

**Exit:** add a documentation-claim checker or explicit reviewed inventory to the offline gate. Every deployment, testnet, mainnet, consumer-support, fee, token, API and adoption statement must cite a current artifact or be labeled illustrative/proposed. Consumer matrices must record exact canonical ID, digest, writer shape, accepted readers, chain/network and test evidence.

## Prioritized coding plan

1. **Truth containment, documentation-only:** correct maintained README claims and mark financial/economic examples proposed. Do not rewrite historical reviews or legacy schemas.
2. **Batch 1A, conformance foundation:** add canonical manifest/digests/aliases, make all 13 v0.2.0 discriminators mandatory, complete positive examples, add strict validator/negative fixtures/CI, and define chain realization profiles.
3. **Accountability logical contracts:** define actor/authority/reference/asset-unit/state-transition rules, then add bond, claim, challenge and verdict drafts plus complete invalid fixtures. Reuse namespace and agent identity only through verified owner/delegation edges.
4. **Derived reputation:** publish an event vocabulary and versioned deterministic scoring package; treat all existing mutable scores as non-authoritative legacy hints.
5. **Nexus execution profile:** implement intent-first bond/funding/challenge/verdict/settlement adapters against a pinned isolated core; require human disposition before slash execution and exact read-back afterward. Keep Distordia non-custodial.
6. **Transport/indexer profile:** replace prose queries with pinned request/response/pagination fixtures; define reorg/finality, checkpoints, incomplete scans and replay.
7. **Product cross-chain profile:** separate stable logical identity from chain record identity, repair revision PDAs, compile the program, publish golden vectors and explicit mirror attestations.
8. **NexGo and consumer migration:** execute the full ownership/payment graph and publish per-consumer version matrices before changing draft/adoption status.

Do not collapse these into one schema rewrite. Each batch exits through its command and fixtures before the next batch can claim implementation status.

## Positive controls worth retaining

- Native Nexus namespace ownership is correctly treated as more authoritative than an attestation string in the v0.2.0 namespace draft.
- The v0.2.0 content/social/article direction moves bulk content off-chain while retaining integrity anchors.
- NexGo v0.2.0 separates passenger request, driver offer and passenger agreement and removes precise public coordinates.
- Product v0.2.0 deliberately separates a stable identity layer from companion MRP layers.
- The Solana layout table is internally contiguous at 545 bytes and provides deterministic offsets as a useful implementation target.
- Draft status and historical v0.1.0 files are preserved rather than silently upgraded.

None of these controls establishes deployment, authorization, settlement, consumer adoption or cross-chain equivalence without the missing executable evidence.

## Reviewed source hashes

### Concurrent and strategic inputs

| File | SHA-256 / Git identity |
|---|---|
| `.git/index` at review start | `8290d32dc875a354d5f9ae236b2ac52c58b0e45c58487084d44566aaed9e4804` |
| `docs/ARCHITECTURE.md` worktree | `9f6fd9c1f7c5c8c0402768eea71879e60e72e4a044dbb8a218fa966082e2d8e9` |
| `docs/ARCHITECTURE.md` index blob | `dad904ca4a08cb988f493259f8b628c7e3e5ab90` |
| `docs/DEVELOPMENT_PLAN.md` worktree | `f49f08f0338204912f274214421a17f61ad1490cb5934f4f43657304701edc8d` |
| `docs/DEVELOPMENT_PLAN.md` index blob | `d74d8caa54183b0cf91f28274693de05154a545a` |
| `docs/DEVELOPMENT_REVIEW_2026-09-21.md` worktree | `7fac5b37d503ec9d88a12f54558423629f06ce9d2632826cd94e36582341b1af` |
| `docs/DEVELOPMENT_REVIEW_2026-09-21.md` index blob | `ba29509e0978b560b6b4a9c9dec35fc5d356e115` |
| `vision.md` | `2231687ca457642e60410a80d54ee6309e769b606f143a619d6ce406e7ea1de1` |
| `Distordia_Labs_Business_Thesis_and_Strategy_v2.docx` | `552e92c195a4df391070a66aa1cbc09705e67b9ae4464fa2a17423fdcfd408ae` |
| `Distordia_Customer_Problem_Atlas_v2.docx` | `bee0948eb60959bbe76ad3bc433e4570cc6e4d3a90a77f0801d1f6f953a6be52` |
| `staked-accountability-rails.md` | `c48f6d6f40408f2006fd262b035fd3784976eacc633421c84930bbdf61432a5a` |
| `infrastructure-buildout.md` | `2813c68f605cbf147b00fb736dffcc70cbd84a55a6093ebe7a3e29cbf1d9248d` |
| `README.md` | `5e7a9ba870835b041da7f60c9390479bd4c9180fcc2e6233f49a9d89900a46ea` |

### Standards files

```text
cbb71be0f099e932572940152b659a22d7656391a2a7a85dbf0b3f9737999875  standards/agent-standard.json
6ca0ac3067b1257074d25860ea33971cd4a39768b87af09cd9f7846a28e42a44  standards/article-standard.json
5742aea2a5292dd255c50502e56f86b349451dc3f0fd9e7b7b8019b7b2a14595  standards/article-standard.v0.2.0.json
7a96e3dceefa9c40015fea5218daed48fa32c7a649e8a960411348d1130876c4  standards/content-standard.json
97df5ed0628018e7f67e839fde0bf3a6ad0ad735d1c7ad956e20296040cf0466  standards/content-standard.v0.2.0.json
9a59bb690f932aead4cca729b1b94dc2a0c7a7987e92fcca84ed116441e30bd0  standards/namespace-standard.json
43bc8c8a21c345fddd648e3e216ee5e7b980d48fdedd96813283511c5fa9ff81  standards/namespace-standard.v0.2.0.json
3900ba1f388be1e1cffe15625ee26a5e7579db89c8beca88642ee8aec1c95e92  standards/nexgo-rating-standard.json
8909c0cf88cb5ea67cad4064a613da67e2bdbda3a609b6c90b80fee18e824748  standards/nexgo-rating-standard.v0.2.0.json
37a90dd2587ac7e892945cf5622764cc45439b971d805ab83e57a79ec8ed4b4e  standards/nexgo-ride-standard.json
0c47571dace004d9e9b1ce78231fe3cf0ed9ec21d6178850c569ab139a775d47  standards/nexgo-ride-standard.v0.2.0.json
cb0f6d03d936d078eafcdc71cfa0f366a9c8824dd1696f66f765b1415c3d20ec  standards/nexgo-taxi-standard.json
0807c99d4f4e23a9e3a4072345d6a7dd1c6aef5db67fa7ef5c19d46edcc1e3b4  standards/nexgo-taxi-standard.v0.2.0.json
d76df31fbbf459f6d01723ecec5892ac842df5f86796912b798f277a56780b07  standards/nft-standard.json
5e4a1c9acb63445aa1c0dc18edbdd45bb7ec2126fec6f20840c1fa70f837c567  standards/player-standard.json
104132e8ea5b139b33ba6dafc06f0e1ab940e1a721db6402b5c5bcea67a47a60  standards/product-standard.json
10a79cebc5a14c408e3c491eb4eba112b862ceb94c9f630e4dfddbc6078fb9ea  standards/product-standard.v0.2.0.json
0cc9ab0148cfc440a37ac06ac115d266cba30882d2bfb996aa1a178e7d3dde2e  standards/product-standard.v0.2.0.solana.json
1930da007153b1746e2b792db4fde4ed95d0d8638dd4c881ad14011cc1d19025  standards/social-standard.json
6563b0d541961ed585e2b418746a61a560103f3071ded815c26de5d9d57ed62e  standards/social-standard.v0.2.0.json
810dd0428e74e630ab44d1159f0973c24ec8439eff276216242c824073af0bc5  standards/swarm-standard.json
```
