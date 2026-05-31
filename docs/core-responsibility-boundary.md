# kotonoha-core Responsibility Boundary

## Status

**Informative — architecture boundary.** This document defines repository responsibilities for the convergence phase. It does **not** add normative interchange fields or RDE category semantics. If this document disagrees with normative text under `docs/` (for example [`architecture.md`](architecture.md), [`rde-review-output.md`](rde-review-output.md)), **normative documents win**.

Parent context: [`current-official-architecture.md`](current-official-architecture.md).  
Implementation mirror: [`kotonoha-core` `docs/responsibility-boundary.md`](https://github.com/zyx-corporation/kotonoha-core/blob/main/docs/responsibility-boundary.md).

## Purpose

`kotonoha-core` is the **shared implementation layer** between normative specifications and user-facing runtimes (CLI, UI adapters, orchestrator services).

This document prevents:

- `kotonoha-core` becoming a de facto specification.
- CLI command contracts leaking into the core crate API without spec alignment.
- UI policy (Obsidian metadata write mode, VSCode thin-console behaviour) being implemented in core.
- Orchestrator HTTP semantics being encoded as canonical Kotonoha interchange rules in core.

## Layer model

```text
kotonoha-spec          normative semantics, contracts, conformance language
       ↓ conforms to
kotonoha-core          shared Rust implementation (validation, lineage, RDE helpers, optional persistence)
       ↓ used by
kotonoha-cli           first stable runtime (command contracts, exit codes, user-visible behaviour)
       ↓ invoked by
UI adapters            Obsidian / VSCode policy and human-review UX
       ↓ optional
kotonoha-orchestrator  expansion backend (HTTP API semantics — separate boundary)
```

**Dependency direction:** spec → core → CLI → UI. Orchestrator and gateway consume core/CLI behaviour; they do not redefine spec semantics.

## What `kotonoha-core` is

| Property | Definition |
| --- | --- |
| Role | Shared implementation layer for SLS primitives used by CLI and services |
| Artefact | Rust crate `kotonoha_core`, migrations, developer docs, tests |
| Authority | **Implementation source of truth** for shipped library behaviour — not normative meaning |
| Alignment | Must trace to `kotonoha-spec` via [`spec-traceability`](https://github.com/zyx-corporation/kotonoha-core/blob/main/docs/spec-traceability.md) |

## What `kotonoha-core` MAY own

The following belong in core when they implement **published spec rules** or **CLI-independent pure functions** shared by multiple runtimes:

| Capability | Examples (current crate) | Notes |
| --- | --- | --- |
| Schema validation helpers | `rde::validate_json`, `interchange::validate_interchange_json` | Enforce rules defined in spec; do not invent new categories or fields |
| Typed data models | `LineageUnit`, `InterchangeDocument`, RDE scaffold types | Serde shapes follow spec; `deny_unknown_fields` is a **tooling guard**, not spec evolution |
| RDE audit data model helpers | `rde_impl`, `observation_rde`, `rde_attach` | Evaluation scaffolding and attachment helpers — not full semantic understanding claims |
| Context / handoff construction helpers | `context_pack`, export-oriented structs | Wire formats still defined in spec or CLI docs |
| Project / principal identity helpers | Identity resolution used by CLI and store | **Identifiers and lookup** — not governance policy |
| Common error types | Validation errors, persistence errors | Stable for library callers; not CLI exit-code mapping |
| Optional persistence | `store::postgres`, migrations | Operational DDL is informative in core; normative correlation clauses remain in spec |
| Git read helpers | `git` module | Read-only context; no Git write ownership |
| CLI-independent pure functions | Validators, normalizers, deterministic evaluators | Safe for orchestrator or tests to import without pulling CLI UX |

### Allowed core modules (current)

| Module | Responsibility |
| --- | --- |
| `interchange` | Non-normative `kotonoha.interchange.v1` envelope validation |
| `lineage` | Minimal `LineageUnit` (SLS-3) |
| `rde` | RDE review output JSON validation (SLS-4, Phase 2 profile) |
| `rde_impl` | Conservative RDE evaluator scaffold (SLS-5) |
| `rde_attach` | RDE attachment helpers |
| `observation_rde` | Observation-side RDE helpers |
| `semantic_lineage` | Semantic lineage operations |
| `context_pack` | Context pack structures (M5+) |
| `git` | Read-only Git context |
| `store` (feature `postgres`) | Validated persistence |

New modules MUST be justified against this boundary before merge.

## What `kotonoha-core` MUST NOT own

| Forbidden ownership | Belongs in |
| --- | --- |
| Normative schema definitions (field meaning, RDE categories, sidecar layout) | `kotonoha-spec` |
| CLI command names, flags, exit codes, stdout/stderr contracts | `kotonoha-cli` (`cli-definition.md`) |
| Obsidian UI behaviour (apply flow, busy cursor, metadataWriteMode, gitMode policy) | `obsidian-kotonoha-console` |
| VSCode extension behaviour (commands palette, thin Developer Console UX) | `kotonoha-vscode` |
| Orchestrator HTTP route semantics (`/v1/rde/evaluate`, agent registry) | `kotonoha-orchestrator` (+ spec promotion when stable) |
| Human review policy (approve / reject / revise rules for users) | UI adapters + spec review contracts |
| Autonomous AI agent loops | Out of scope for core |
| Undocumented sidecar formats | Must not be introduced in core without spec issue |

**Rule:** Core MAY ship schema **helpers** (parse, validate, serialize) but MUST NOT define schema **semantics**. Semantic changes start in `kotonoha-spec`.

## Boundaries by repository

### vs `kotonoha-spec`

| spec | core |
| --- | --- |
| Defines what a valid RDE record means | Validates JSON against published rules |
| Owns frontmatter / sidecar / handoff contracts | May implement readers/writers after spec acceptance |
| Wins on conflict | Updates via spec issue → core PR + traceability |

### vs `kotonoha-cli`

| CLI | core |
| --- | --- |
| Command surface and user-visible behaviour | Library functions CLI calls |
| Exit code mapping | Returns structured errors |
| `context export`, `rde emit`, installer UX | Pure validation and domain logic |

CLI MUST NOT fork incompatible validation rules. If CLI needs new behaviour visible to users, spec + core + CLI change together.

### vs UI adapters (Obsidian, VSCode)

| UI adapter | core |
| --- | --- |
| Human confirmation, UX policy, adapter-specific settings | No UI types, no Obsidian/VSCode APIs |
| May call CLI or duplicate **conforming** validation for local/mock paths | Provides shared validators optionally imported by services — not required in Obsidian TS plugin today |
| Sidecar **paths** and write timing policy | Sidecar **record shape** validation only when shared |

UI adapters MUST NOT treat core or CLI behaviour as a substitute for spec when documenting semantics to users.

### vs `kotonoha-orchestrator`

| orchestrator | core |
| --- | --- |
| HTTP API, agent orchestration, backend deployment | May reuse core validators in Rust services |
| Engine selection, model routing, evaluate endpoint contract | Does not define RDE category vocabulary |
| Expansion layer — secondary until stable | Core stays free of HTTP route definitions |

Orchestrator API semantics are documented in orchestrator spec/docs until promoted into `kotonoha-spec`. See [`orchestrator-api-stability-boundary.md`](orchestrator-api-stability-boundary.md) for stable vs experimental tiers and fallback policy.

## Decision checklist (future core changes)

Before merging a `kotonoha-core` PR, verify:

1. **Spec link:** Is there an open or merged `kotonoha-spec` issue/PR if observable semantics change?
2. **Not normative creep:** Does the PR introduce new public field meanings not yet in spec?
3. **Not CLI creep:** Does the PR embed command names, exit codes, or CLI-only UX?
4. **Not UI creep:** Does the PR import Obsidian/VSCode concepts or human-review UX policy?
5. **Not orchestrator creep:** Does the PR hard-code HTTP routes or backend-specific agent semantics?
6. **Traceability:** Is [`spec-traceability.md`](https://github.com/zyx-corporation/kotonoha-core/blob/main/docs/spec-traceability.md) updated?
7. **Shared use:** Is the code used (or plausibly shared) by CLI **and** at least one other consumer, or clearly a spec-mandated library concern?

If the change is CLI-only glue with no reuse, it belongs in `kotonoha-cli`, not core.

## Non-goals (core)

- Replace `kotonoha-spec` as the meaning authority.
- Replace `kotonoha-cli` as the primary user execution surface.
- Implement Obsidian or VSCode UI policy.
- Define orchestrator REST contracts.
- Claim that conservative/local validation equals full RDE semantic evaluation.
- Silent auto-mutation of user notes or workspace files from library code.

## RDE Check

### Preserved Elements

Kotonoha remains spec-driven: context portability, semantic audit, RDE-based review, and human acceptance of meaning changes. Core implements published rules; it does not authorize final truth.

### Authorized Transformations

Shared validation and lineage logic are centralized in core so CLI and services do not diverge. Schema helpers are explicit **implementations**, not alternate specifications.

### Inferred Extensions

The boundary introduces a explicit checklist and module allow-list so future contributors can reject responsibility drift in review.

### Unresolved Elements

- Which orchestrator evaluate responses will be promoted to spec (tracked under orchestrator boundary issue).
- Long-term split between sidecar filesystem layout (spec) and Postgres store (core migrations).
- Whether Obsidian TS will call Rust core via FFI/bindings or continue CLI/orchestrator paths only.

### Drift Risks

- **`deny_unknown_fields` mistaken for spec evolution** — rejecting unknown JSON keys is tooling strictness, not a normative versioning policy by itself.
- **Core-led schema changes** — adding fields in Rust before spec acceptance makes core the de facto spec.
- **RDE scaffold over-claim** — `ConservativeRdeEvaluator` must not be documented as full RDE evaluation.
- **Persistence DDL as semantics** — Postgres columns must not redefine RDE categories or review decisions.

### Next Revision Policy

Revise when core modules change materially, when orchestrator contracts stabilize, or when a spec section explicitly assigns new shared implementation ownership to core.

## Related documents

| Document | Role |
| --- | --- |
| [current-official-architecture.md](current-official-architecture.md) | Multi-repo roles and phase priorities |
| [repository-governance.md](repository-governance.md) | Spec vs implementation source of truth |
| [architecture.md](architecture.md) | SLS-2 normative architecture |
| [kotonoha-core spec-traceability](https://github.com/zyx-corporation/kotonoha-core/blob/main/docs/spec-traceability.md) | Section ↔ module mapping |
| [kotonoha-cli cli-definition](https://github.com/zyx-corporation/kotonoha-cli/blob/main/docs/cli-definition.md) | CLI command contract |

## Change policy

Updates follow [`git_operation_rules.md`](git_operation_rules.md): issue → branch → PR → merge to `main`. Link `kotonoha-management` boundary issues (#163+) when revising.
