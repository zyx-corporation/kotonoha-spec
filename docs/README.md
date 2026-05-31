# Specification index

English is the **primary normative language** for documents in this directory. Japanese summaries may be added later as `*_ja.md` alongside sources.

| Document | Title | Status |
| --- | --- | --- |
| [repository-governance.md](repository-governance.md) | Ecosystem roles, dependency direction, informative governance | **Informative** |
| [current-official-architecture.md](current-official-architecture.md) | Current multi-repo roles and phase priorities (2026-05 convergence) | **Informative — current architecture** |
| [documentation-placement-policy.md](documentation-placement-policy.md) | Role split between `kotonoha-spec` and `kotonoha-docs`; placement rules for public documents | **Informative — repository governance** |
| [documentation-content-specification.md](documentation-content-specification.md) | Concrete document classes, placement inventory, and migration rules | **Informative — documentation governance** |
| [phase-and-milestone-definition.md](phase-and-milestone-definition.md) | Phase vs milestone vocabulary, roadmap boundaries, and current implementation alignment check | **Informative — roadmap / alignment** |
| [phase-and-milestone-definition_ja.md](phase-and-milestone-definition_ja.md) | Phase and milestone definition Japanese companion | **Informative — Japanese companion** |
| [requirements-overview.md](requirements-overview.md) | Kotonoha system requirements overview, full architecture picture, and use cases | **Informative** |
| [kotonoha-backend-minimum-requirements.md](kotonoha-backend-minimum-requirements.md) | Kotonoha backend minimum requirements draft for MeaningEvent / RDE / Policy Boundary flow | **Informative draft — future normative candidate** |
| [meaning-event-schema.md](meaning-event-schema.md) | Minimal MeaningEvent schema draft | **Informative draft — future normative candidate** |
| [git_operation_rules.md](git_operation_rules.md) | Git/Issue/branch/PR workflow (Japanese) | **Informative — process** |
| [T-RDE_v1.0.md](T-RDE_v1.0.md) | T-RDE v1.0 — meaning audit for vibe coding (ΔM, trace map, quality gates) | **Informative — method (canonical, theory)** |
| [t_rde_実行プロンプト_v_1_論文準拠.md](t_rde_実行プロンプト_v_1_論文準拠.md) | T-RDE execution prompt v1.1a (T-RDE v1.2 paper-aligned) for LLM audit sessions | **Informative — method (canonical, operational)** |

## Phase 1 — Public specification MVP (bundle **0.1**)

Phase 1 specification documents use stable hierarchical section identifiers: `SLS-<document>.<major>.<minor>`.

| Number | Document | Title | Status |
| --- | --- | --- | --- |
| SLS-1 | [introduction.md](introduction.md) | Scope, definitions, conformance keywords | Normative (Phase 1) |
| SLS-1-ja | [introduction_ja.md](introduction_ja.md) | Introduction (JA summary companion; illustrative figures + terminology notes) | **Informative — Japanese only** |
| SLS-2 | [architecture.md](architecture.md) | Logical architecture and component responsibilities | Normative (Phase 1) |
| SLS-2-ja | [architecture_ja.md](architecture_ja.md) | Architecture (JA summary companion with figures) | **Informative — Japanese only** |
| SLS-3 | [semantic-lineage-model.md](semantic-lineage-model.md) | Minimal lineage unit and identifiers | Normative (Phase 1) |
| SLS-3-ja | [semantic-lineage-model_ja.md](semantic-lineage-model_ja.md) | Semantic lineage model (JA companion) | **Informative — Japanese only** |
| SLS-4 | [rde-review-output.md](rde-review-output.md) | RDE categories and minimal interchange record | Normative (Phase 1) |
| SLS-4-ja | [rde-review-output_ja.md](rde-review-output_ja.md) | RDE review output (JA companion) | **Informative — Japanese only** |
| SLS-5 | [rde-implementation-specification.md](rde-implementation-specification.md) | RDE implementation responsibilities and boundaries | Normative (Phase 1 implementation profile) |
| SLS-5-ja | [rde-implementation-specification_ja.md](rde-implementation-specification_ja.md) | RDE implementation specification (JA companion) | **Informative — Japanese only** |
| SLS-6 | [representation-of-loss.md](representation-of-loss.md) | Requirements for representing lost semantic elements | Normative (Phase 1) |
| SLS-6-ja | [representation-of-loss_ja.md](representation-of-loss_ja.md) | Representation of loss (JA companion) | **Informative — Japanese only** |
| SLS-7 | [audit-trail-relationship.md](audit-trail-relationship.md) | Relationship between RDE outputs and audit trails | Normative (Phase 1) |
| SLS-7-ja | [audit-trail-relationship_ja.md](audit-trail-relationship_ja.md) | Audit trail relationship (JA companion) | **Informative — Japanese only** |
| SLS-8 | [versioning.md](versioning.md) | Specification versioning and evolution | Normative (Phase 1) |
| SLS-8-ja | [versioning_ja.md](versioning_ja.md) | Versioning (JA companion) | **Informative — Japanese only** |

## Phase 2 — Interchange and schema hardening

Phase 2 promotes RDE validation hardening into public normative text without changing the Phase 1 RDE review output `spec_version` value.

| Number | Document / artifact | Title | Status |
| --- | --- | --- | --- |
| SLS-9 | [phase2-interchange-hardening.md](phase2-interchange-hardening.md) | Phase 2 RDE validation profile | **Normative (Phase 2 validation profile)** |
| SLS-9-ja | [phase2-interchange-hardening_ja.md](phase2-interchange-hardening_ja.md) | Phase 2 interchange hardening (JA companion) | **Informative — Japanese only** |
| Schema | [../schemas/rde-review-output.phase2.schema.json](../schemas/rde-review-output.phase2.schema.json) | Phase 2 RDE review output minimum JSON Schema | **Normative schema artifact** |
| Schema (draft) | [../schemas/rde-assessment-storage.v0.1.draft.schema.json](../schemas/rde-assessment-storage.v0.1.draft.schema.json) | RDE assessment storage record (informative; [#47](https://github.com/zyx-corporation/kotonoha-spec/issues/47)) | **Informative draft** |

### Recommended reading order

New readers SHOULD follow **[SLS-1 → SLS-2 → SLS-3 → SLS-4 → SLS-5 → SLS-6 → SLS-7 → SLS-8 → SLS-9]**, optionally returning to SLS-1 for definitions. Editors preparing pull requests SHOULD touch [SLS-8](versioning.md) whenever normative headings move, section identifiers change, or materially change wording.

Public issue **[#3 — representation of lost elements](https://github.com/zyx-corporation/kotonoha-spec/issues/3)** tracks interchange encoding beyond the interim in [SLS-6.5](representation-of-loss.md#sls-65-interim-encoding-phase-3-gate--2026-05). Normative backlog rollup: [#25](https://github.com/zyx-corporation/kotonoha-spec/issues/25).

## Incremental work (explicitly deferred)

- [**RDE assessment storage**](rde-assessment-storage-increment.md) (informative draft; [#47](https://github.com/zyx-corporation/kotonoha-spec/issues/47)) — JSONB bound to MeaningDelta vs SLS-4 interchange.
- Full JSON Schema published for **all** future interchange fields beyond the Phase 2 RDE review output minimum schema.
- Typed schemas that fix every optional lineage property enumerated in later increments.
- Wire-level APIs and network protocols (typically after interchange versioning stabilizes).
- Authentication, tenancy, scalability, or threat-model obligations (planned for broader reliability phases referenced from [SLS-2](architecture.md)).
- RDE implementation profiles for specific deployment environments beyond the Phase 2 validation profile, if needed later.

See [SLS-8](versioning.md) for how incompatible changes are gated.
