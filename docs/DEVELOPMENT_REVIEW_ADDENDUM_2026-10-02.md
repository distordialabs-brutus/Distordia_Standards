# Development review addendum — unchanged source and Batch 1A handoff — 2026-10-02

## Baseline, scope, and verdict

**Requested baseline and reviewed source:** local `main` at `db3da5626f270d5d411c4f9eb4b6d9e706891b10`; committed `standards/` tree `d7bede155734827d88776421eadc8ac7839e5379`. The range `db3da5626f270d5d411c4f9eb4b6d9e706891b10..HEAD` contains zero commits and zero tracked path changes.

**Verdict:** no implementation or specification progress exists after the baseline. Batch 1A remains absent and is still the release-blocking next slice. The accepted progress is documentation precision: exact 21-document manifest scope, explicit dialect adapters, repository-global example resolution, strict-positive coverage, stable diagnostics, adversarial fixtures, and identical local/CI commands are now concrete acceptance contracts. Agent-asset/commerce research remains design, not implementation.

This review changed documentation only. It did not edit any standard, runtime, test, workflow, or pre-existing dirty document; it did not commit, push, reset, stash, clean, stage, unstage, call a chain, or deploy anything. See [the architecture addendum](ARCHITECTURE_ADDENDUM_2026-10-02.md) and [current market-evaluation qualification](STANDARDS_MARKET_EVALUATION_ADDENDUM_2026-10-02.md).

## Concurrent work preserved

At review start the repository already contained:

- staged `docs/ARCHITECTURE.md`, worktree SHA-256 `9f6fd9c1f7c5c8c0402768eea71879e60e72e4a044dbb8a218fa966082e2d8e9`, index blob `dad904ca4a08cb988f493259f8b628c7e3e5ab90`;
- staged `docs/DEVELOPMENT_PLAN.md`, worktree SHA-256 `f49f08f0338204912f274214421a17f61ad1490cb5934f4f43657304701edc8d`, index blob `d74d8caa54183b0cf91f28274693de05154a545a`;
- staged new `docs/DEVELOPMENT_REVIEW_2026-09-21.md`, worktree SHA-256 `7fac5b37d503ec9d88a12f54558423629f06ce9d2632826cd94e36582341b1af`, index blob `ba29509e0978b560b6b4a9c9dec35fc5d356e115`;
- unstaged `docs/standards-market-evaluation.md`, SHA-256 `a15c9e4d86bb45d414871141fa85122d98a7c5e20d5e5ef7ed1925bd95a0efe9`;
- local-only agent research, September 30 addenda, proposed strategy, and `vision.md`; these were review context only and are not publication dependencies.

Initial `.git/index` SHA-256 was `84da138f38995af17b2f5ac53cb8807d6f7480c9dd3c3bdde4e397d62802d96d`. These paths are preserved inputs, not accepted implementation.

## Executed evidence

| Check | Result |
|---|---|
| Source range | **PASS / empty** — baseline equals HEAD; zero commits and tracked paths after it |
| Standards identity | **PASS / unchanged** — tree `d7bede155734827d88776421eadc8ac7839e5379`; all 21 source hashes match the prior executed review |
| Worktree/index whitespace | **PASS** before these addenda — `git diff --check` and `git diff --cached --check` |
| Manifest | **ABSENT** — no `standards/manifest.json` |
| Validator | **ABSENT** — no `tools/validate_standards.py` |
| Collected tests | **ABSENT** — `python3 -m unittest discover -s tests -p 'test_*.py'` exited 1 because `tests` is not importable |
| Workflow | **ABSENT** — no `.github/workflows` directory |
| Advertised validator command | **ABSENT** — `python3 tools/validate_standards.py --repo .` exited 2 because the file does not exist |
| Fresh custom semantic probe | **BLOCKED, not rerouted** — unattended approval denied `execute_code` before execution; no fresh custom-probe result is claimed |
| Source inspection | **COMPLETE for v0.2 documents** — all eight Nexus v0.2 files and the Solana product file were read directly |

Because the source commit, standards tree, and every source-standard SHA-256 are unchanged, the 2026-09-30 executed validation remains applicable as unchanged-source evidence rather than a fresh run: 21/21 strict JSON parse, 13 Nexus logical types, 0/13 mandatory `schema-ver`, 12 examples, nine strict-valid objects, seven strict-valid logical types, five UTF-8 overflows, and no checked-in conformance gate. The table above preserves the distinction between fresh checks and carried-forward execution; no fresh custom semantic probe is claimed.

## Current strict-positive matrix

| Logical type | Status | Required repair |
|---|---|---|
| `content` | valid | retain cross-file article/content resolution test |
| `distordia-article` | invalid | replace 60-byte `next` with a valid <=56-byte identity |
| `distordia-article-chunk` | missing | add a complete positive example |
| `distordia-follow` | valid | retain |
| `distordia-post` | valid | retain |
| `distordia-reaction` | missing | add a complete positive example |
| `namespace` | valid | retain |
| `nexgo-rating` | invalid | repair 64-byte `self-addr` and 60-byte `agreement` to <=56 bytes |
| `nexgo-ride-agreement` | missing | add a complete positive example |
| `nexgo-ride-offer` | valid | retain |
| `nexgo-ride-request` | valid | retain |
| `nexgo-taxi` | invalid | repair 60-byte `license-cred` and `insurance-cred` to <=56 bytes |
| `product` | valid | retain |

The first article example is a valid `content` candidate declared in another file. Build the full logical registry before validating it. Do not count two valid `content` objects or two valid `namespace` objects as additional type coverage.

## Precise coding batches

### Batch 1A.1 — Manifest and dialect inventory

Create `standards/manifest.json`, loader/model/adapters under `tools/conformance/`, and collected manifest/adapter tests.

- Inventory exactly the 21 reviewed source-standard documents; exclude the manifest from its own inventory.
- Record repository path, raw SHA-256, canonical `$id`, semver, lifecycle, logical standard, chain/profile, explicit dialect, aliases, and typed canonical-ID edges.
- Preserve legacy bytes and classify undiscriminated wire data as `legacy-unversioned`.
- Support explicit Nexus v0.2 single/multi, Solana product, and legacy typed/raw/custom dialects. Unknown dialects reject.

**Exit:** exact entry set, unique IDs/aliases, every edge resolved, stale-byte mutation emits `MANIFEST_DIGEST_MISMATCH`.

### Batch 1A.2 — Strict loader, global registry, and diagnostics

Create `tools/validate_standards.py` plus loader/check/diagnostic modules and `tests/test_validate_standards.py`.

- Reject duplicate keys while loading raw bytes.
- Normalize every declaration before any example validation.
- Enforce exact JSON scalar types; `bool` is not an integer.
- Enforce unsigned widths, UTF-8 byte lengths, enum membership, full-match patterns, required values, constants, and undeclared fields.
- Emit deterministically sorted code/path/type/example/field diagnostics and nonzero status.

**Exit:** the article-file `content` example resolves across files; unknown types and dialects fail; repeated runs produce identical diagnostics.

### Batch 1A.3 — Mandatory versions and positive fixtures

Update all eight Nexus v0.2 files and `product-standard.v0.2.0.solana.json` in one candidate.

- Add `schema-ver` to all 13 Nexus logical required arrays and the Solana product required array.
- Repair the five over-width values; do not loosen declarations to fit placeholders.
- Add article-chunk, reaction, and ride-agreement positives.
- Parameterize missing, malformed, wrong, and unknown `schema-ver` for every type.

**Exit:** 13 valid, zero invalid, zero missing Nexus types; exact supported versions pass and all invalid version classes fail.

### Batch 1A.4 — Negative corpus and product parity

Add focused fixtures under `tests/fixtures/valid/` and `tests/fixtures/invalid/`, normally one primary defect per fixture.

Required classes: duplicate JSON key; missing/unexpected manifest path; stale digest; duplicate ID/alias; unresolved/ambiguous edge; unsupported dialect/scalar; duplicate type/field/required name; missing/orphan definition; invalid mutability; strict scalar mismatch; boolean-as-integer; unsigned under/overflow for every used width; UTF-8 exact-limit and multibyte plus-one; enum/pattern and invalid-pattern failures; undeclared example field; unknown example type; broken local link; required-set mismatch; missing product mapping; unannotated representation/lifecycle difference; offset gap/overlap; wrong total size.

**Exit:** each fixture asserts a stable primary code and CLI nonzero status. Product parity reports annotated string/Pubkey, `self-addr`, and timestamp differences separately from compiled/runtime evidence.

### Batch 1A.5 — One command and CI

Add a dependency-neutral wrapper such as `scripts/check-standards`; make README and `.github/workflows/standards.yml` invoke that exact path.

The wrapper runs collected tests, repository validation, local-reference checks, and `git diff --check`. CI runs on pull requests and pushes with no network, wallet, chain, or secret dependency.

**Exit:** clean checkout and CI execute the same command and report 21 inventoried documents, 13/13 strict-valid positives, complete negative fixtures, resolved graph, and explicit product parity.

## Deferred and not accepted

Batch 1A does not close pinned Nexus query grammar/pagination, Nexus serialized size, compiled Solana/Borsh behavior, revision-safe PDA identity, program authority, consumer compatibility, agent identity v0.2, functional-agent transfer, wallet authorization, commerce/payment settlement, accountability, reputation, deployment, or adoption. Local agent-commerce research is useful design input but remains non-normative until a separately versioned profile and fixtures land.

No status transition is authorized by this review.