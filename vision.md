# Distordia Standards — Vision

## Portfolio roadmap authority

The master Distordia project also owns `PORTFOLIO_DEVELOPMENT_PLAN.md` and its strategy-decision register. The full order is **master strategy/customer evidence → portfolio roadmap/decisions → this vision → architecture/development plan → tasks/code/tests/release evidence**. Read the [portable repository alignment](docs/DISTORDIA_ALIGNMENT.md) for objective, customer-evidence, ownership and dependency mapping. Master-source paths below are local workspace references, not promised GitHub links. This section adds portfolio sequencing; it does not certify the envisioned behavior or amend unresolved master strategy assumptions.

The canonical Business Thesis DOCX governs strategic intent and the Customer Problem Atlas classifies observed problems. The maintained portfolio plan is the required intermediate layer for sequencing and unresolved decisions; it does not amend either DOCX. The Atlas currently supports industrial asset-information/provenance profiles with Class A evidence. It does not validate mobility, agent commerce, tokenized accountability, reputation markets, collateral/slashing, regulatory treatment, or adoption. Venture notes for those areas are hypotheses only.

## Accountability venture context — not canonical authority

Local-only, non-link venture sources `/home/brutus/projects/Distordia/staked-accountability-rails.md` and `/home/brutus/projects/Distordia/infrastructure-buildout.md` are venture hypotheses and dependency-design context. They do not amend canonical strategy or prove enforceable collateral/slashing, non-custody, regulatory status, reputation, or adoption. Interpret unresolved claims through the master portfolio decision register (SD-002–SD-008); feasibility, legal assessment and human decisions remain required.

## Purpose

Software, interfaces, and content are becoming abundant. Scarcity moves to coordination, verifiable identity, settlement, accountability, and risk. This repository turns that Distordia thesis into open, implementable standards: shared semantics by which people, organizations, agents, and applications can make claims, inspect evidence, assign responsibility, and verify outcomes without depending on a Distordia-controlled execution path.

Distordia is a **standard-setter, not a gatekeeper**. Specifications, validation rules, reference fixtures, and execution interfaces should be public and independently implementable. Distordia may issue attestations or build services on these standards, but conformance must not require its permission, custody, private database, or preferred application.

## Authority and decision hierarchy

Work in this repository follows this order of authority:

1. **Canonical originals** — local-only, non-link `/home/brutus/projects/Distordia/Distordia_Labs_Business_Thesis_and_Strategy_v2.docx` governs strategy; local-only, non-link `/home/brutus/projects/Distordia/Distordia_Customer_Problem_Atlas_v2.docx` classifies customer evidence and supports only the profiles its evidence names.
2. **Maintained portfolio layer** — the master `PORTFOLIO_DEVELOPMENT_PLAN.md` records cross-repository sequencing and explicit unresolved decisions. Venture research and accountability/buildout notes are hypotheses feeding this layer, not canonical amendments.
3. **This vision** defines what this standards repository exists to accomplish and the boundaries it must preserve.
4. **Architecture and development plans** — notably [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) and [`docs/DEVELOPMENT_PLAN.md`](docs/DEVELOPMENT_PLAN.md) — translate the vision into implementation contracts and sequenced work.
5. **Schemas, tests, and adoption evidence** establish the exact behavior and the level of implementation or adoption actually proved.

When sources conflict, the higher source governs intent and the lower source must be corrected or explicitly marked unresolved; do not silently reconcile incompatible meanings. Evidence remains authoritative about status: a strategy or plan cannot make a draft deployed, conformant, or adopted. A schema that conflicts with architecture is nonconforming; a passing structural test cannot prove chain behavior; an adoption claim requires identifiable consumer and execution evidence.

## Standards direction

### Namespace-rooted identity

Trust begins with a namespace and the authority behind it, not with an isolated asset or display name. On Nexus, the native name or namespace register is the source of truth for uniqueness, ownership, and transfer. Distordia attestations add verification tier, stake, scope, and evidence; they do not replace native ownership. Namespace transfer invalidates owner-bound attestations until re-issued. Agents and sub-identities should be attributable to names under an accountable parent namespace, with delegation and authority independently checkable.

### Accountable agents and claims

Agent capability declarations are claims, not proof. The standards should support a composable lifecycle of:

- **accountability bonds** that identify collateral, scope, parties, expiry, and enforceable settlement references;
- **staked claims** bound to an exact artifact or action, an accountable identity, a bond, a verification method, and a challenge window;
- **challenges** with attributable grounds and stake, so objections also carry accountability; and
- **verdicts** with evidence, resolver identity, deterministic outcome semantics, and an explicit settlement or slash instruction.

These records must preserve who asserted what, under which authority, against which evidence, and what happened next. Objective verification should be deterministic and reproducible. Subjective arbitration and human execution gates must be represented explicitly rather than disguised as automatic truth.

### Reproducible, portable reputation

Reputation is a derived view over attributable events—claims, challenges, verdicts, fulfillment, and slashing—not a self-authored score. The scoring algorithm, inputs, ordering, and version must be public enough for an independent indexer to reproduce the same result from authoritative records. Portable semantics require canonical specification identities, explicit versions and chain realizations, content digests, stable references, and provenance-bearing evidence. No single indexer or Distordia API is the source of truth.

### Open edges, verifiable core

Schemas should remain useful to unaffiliated implementations and compatible with external identity, credential, and agent protocols where semantics align. Commercial analytics, interfaces, or resolution tooling may exist above the standard, but the identity, evidence, state transitions, and reputation inputs needed to verify a result must remain inspectable and independently computable. Inspectable record standards can be designed without company custody or underwriting. Enforceable collateral and slashing without that custody is a target hypothesis/design constraint, not an established capability; source-level feasibility, legal assessment and the master portfolio decisions must resolve it before financial implementation.

## Nexus grounding

Nexus is the first chain realization, not permission to blur logical semantics with platform behavior. A Nexus profile must account for:

- the 1 KB asset-data limit and measured serialized size, not estimates alone;
- flat `JSON` typed fields or an explicitly defined `raw` encoding—no assumed nested objects or arrays;
- native register address, owner/genesis, name/namespace state, and network as authoritative physical identity;
- native namespaces being globally unique, transferable, and restricted to lowercase letters, digits, and periods; hyphens are invalid;
- post-create `self-addr` stamping as a query aid, not a substitute for validating the native register;
- specification version, immutable wire discriminator, and Nexus register revision as distinct concepts;
- explicit mutability, authorization, lifecycle, relationship, pagination, and settlement rules tested against a pinned Nexus implementation.

Cross-chain realizations may encode the same logical standard differently. Equivalence must be demonstrated field by field; it is never implied by similar JSON or names.

## Evidence and scope discipline

This repository may contain historical standards, drafts, proposed commands, and target contracts. Their presence proves design intent only. “Validated,” “testnet,” “mainnet,” “consumer-supported,” and “independently adopted” are separate states and must be supported by corresponding artifacts. Customer problems outside directly observed evidence classes remain hypotheses until validated. Never claim deployed adoption, economic activity, or third-party reliance without durable evidence.

## Development grounding checklist

Before merging a standard or changing its status:

- Trace the change to the canonical strategy and this vision; record any unresolved conflict.
- Define the logical identity, actors, authority, evidence, state transitions, and failure modes before chain encoding.
- Bind namespace and agent claims to authoritative owner/delegation evidence; handle transfer, revocation, expiry, and recovery.
- Assign a canonical specification ID, semantic version, lifecycle status, content digest, wire discriminator, and explicit chain realization.
- For bonds, claims, challenges, and verdicts, specify references, windows, stakes, verification method, resolver authority, and settlement behavior.
- Make reputation derivation deterministic, versioned, replayable, and independent of mutable self-reported counters.
- Enforce Nexus size, type, mutability, authorization, namespace, query, and serialization constraints with positive and negative fixtures.
- Test broken references, forged ownership, unsupported versions, duplicate events, reordering, replay, and incomplete index/query results.
- Separate schema validation from testnet execution, mainnet execution, consumer conformance, and unaffiliated adoption; cite only the level proved.
- Keep normative semantics open, implementation-neutral where possible, and independently verifiable without privileged Distordia infrastructure.
