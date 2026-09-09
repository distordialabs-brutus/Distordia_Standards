# Development review — 2026-09-09

## Baseline and delta

**Prior review's explicit source baseline:** `83b9f0902a062d9a97089c2729889ea565d7af82`

**Fetched local/remote HEAD reviewed:** `d0e328cccebf15727e1152905898dc810742ac6e` on `main` / `origin/main`, 0 ahead and 0 behind after `git fetch --prune origin`

**Verdict: no standards implementation delta; draft/unverified status and prior blockers are retained, not new regressions.** The only intervening commit is `d0e328cccebf15727e1152905898dc810742ac6e` (`docs: define conformance and revision identity repair plan`). It changes `README.md`, adds `docs/ARCHITECTURE.md`, `docs/DEVELOPMENT_PLAN.md`, and the 2026-09-08 review, but does not change `standards/`.

## Source identity and continuity

- Commit tree: `656793e2261cd7bb7e48e55d7c912d9533f86698`
- `standards/` Git tree at both the prior source baseline and current HEAD: `d7bede155734827d88776421eadc8ac7839e5379`
- Ordered SHA-256 manifest of all 21 `standards/*.json` files: `da20bade072dafb4bc41389f63724e41937904a6c31a936ecf5be0d78887fbb0`
- `standards/product-standard.v0.2.0.json`: `10a79cebc5a14c408e3c491eb4eba112b862ceb94c9f630e4dfddbc6078fb9ea`
- `standards/product-standard.v0.2.0.solana.json`: `0cc9ab0148cfc440a37ac06ac115d266cba30882d2bfb996aa1a178e7d3dde2e`
- `standards/namespace-standard.v0.2.0.json`: `43bc8c8a21c345fddd648e3e216ee5e7b980d48fdedd96813283511c5fa9ff81`
- `standards/nexgo-ride-standard.v0.2.0.json`: `0c47571dace004d9e9b1ce78231fe3cf0ed9ec21d6178850c569ab139a775d47`

`git diff --quiet 83b9f0902a062d9a97089c2729889ea565d7af82..HEAD -- standards` exited 0, and baseline/current standards tree IDs are identical.

## Retained actionable findings

1. **P1 — resolve Solana revision identity before implementation.** `product-standard.v0.2.0.solana.json` still derives a product PDA from `product`, authority, and `art-nr`; immutable `rev` is absent. The same authority/article cannot initialize the promised distinct successor account under those seeds. Choose a stable head plus revision PDAs or a versioned revision-inclusive seed contract, then test duplicate, unauthorized, and concurrent supersession.
2. **P1 — replace descriptive conformance with an executable gate.** The checkout still has no validator package/test command, CI workflow, or Anchor program. Add one gate for required fields, types, UTF-8 byte limits, integer bounds, examples, lifecycle rules, references, and links, including deliberately invalid fixtures.
3. **P1 — make logical/physical parity explicit and measurable.** Nexus `self-addr` is mutable for post-create stamping while Solana `self_addr` is initialization-only; the draft still claims identical mutability. Publish a field-by-field equivalence table, serializer golden vectors, pinned Nexus byte measurements, and compiled Anchor/Borsh layout tests. Bind claimed self-address to canonical address/owner and fail closed on mismatch.
4. **P2 — prove consumer migration rather than inferred adoption.** The sibling NexGo writer evidence from the prior review remains a legacy raw `nexgo-ride` version 1 flow, not the v0.2.0 typed request/offer/agreement handshake. Add writer/reader fixtures for every supported version and reject unknown versions, forged ownership/cross-references, and invoice mismatches.

The ordered repairs and exits remain in [`DEVELOPMENT_PLAN.md`](DEVELOPMENT_PLAN.md); authoritative boundaries remain in [`ARCHITECTURE.md`](ARCHITECTURE.md).

## Executed evidence

No live Nexus/Solana network mutation, wallet operation, profile/session call, deployment, invoice, or program-address lookup was performed. Illustrative addresses were not treated as deployed identities.

| Exact command | Result |
|---|---|
| `git fetch --prune origin && git rev-parse HEAD && git rev-parse origin/main && git rev-list --left-right --count HEAD...origin/main && git status --short --branch` | **PASS** — local and remote both `d0e328cccebf15727e1152905898dc810742ac6e`; `0 0`; clean before review edits |
| `git diff --quiet 83b9f0902a062d9a97089c2729889ea565d7af82..HEAD -- standards` plus baseline/current `git rev-parse <ref>:standards` | **PASS** — no standards delta; both tree IDs `d7bede155734827d88776421eadc8ac7839e5379` |
| `python3 /tmp/distordia_structural_probe_2026-09-09.py` | **PASS (narrow structural baseline)** — 21 JSON files parsed; 25 Nexus field sets; no duplicate names, required names absent from field sets, definition keys absent from field sets, unsupported types, default/enum byte overruns, integer-default range errors, or Solana offset gaps; Solana layout totals 545 bytes; revision absent from PDA seeds; manifest hash recorded above |
| `python3 /tmp/local_markdown_link_check_2026-09-09.py . && git diff --check` before review edits | **PASS — 9 Markdown files, 59 local links, 0 broken; whitespace check passed** |
| `git rev-parse HEAD^{tree} && git rev-parse HEAD:standards && sha256sum standards/*.json \| sha256sum` | **PASS** — commit tree, standards tree, and manifest hashes recorded above |
| `python3 /tmp/distordia_structural_probe_2026-09-09.py && python3 /tmp/local_markdown_link_check_2026-09-09.py . && git diff --check` after review edits | **PASS — structural results unchanged; 10 Markdown files, 61 local links, 0 broken; whitespace check passed** |
| `git status --short --branch` plus `git diff --cached --name-status` and `git diff --quiet -- standards README.md` | **PASS — only the two intended tracked docs plus the untracked 2026-09-09 review are present; staged set empty; standards/README worktree diff exit 0** |

The structural probe is temporary review tooling, not a checked-in conformance gate. Passing it does not prove semantic example validity, complete byte budgets, owner/delegation enforcement, chain identity, invoice binding, Nexus serialization, Anchor execution, or consumer compatibility.

## Exit decision and publication status

The repository remains a useful design catalog, not an implementation-certified standard. This review updates `docs/ARCHITECTURE.md` and `docs/DEVELOPMENT_PLAN.md` and adds this file; no schema/source file changed. The parent publishes. No staging, commit, or push was attempted here.
