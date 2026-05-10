# Specification index

English is the **primary normative language** for documents in this directory. Japanese summaries may be added later as `*_ja.md` alongside sources.

| Document | Title | Status |
| --- | --- | --- |
| [repository-governance.md](repository-governance.md) | Ecosystem roles, dependency direction, informative governance | **Informative** |
| [git_operation_rules.md](git_operation_rules.md) | Kotonoha Git/Issue/branch/PR rules (Japanese; duplicate of **`kotonoha-management` canonical**[^gitrules]) | **Informative — process** |

[^gitrules]: Canonical: [`docs/04_git_operation_rules.md` in **`kotonoha-management`**](https://github.com/zyx-corporation/kotonoha-management/blob/main/docs/04_git_operation_rules.md). Update canon first, then mirror here.

## Phase 1 — Public specification MVP (bundle **0.1**)

| Document | Title | Status |
| --- | --- | --- |
| [introduction.md](introduction.md) | Scope, definitions, conformance keywords | Normative (Phase 1) |
| [introduction_ja.md](introduction_ja.md) | Introduction (JA summary companion; illustrative figures + terminology notes) | **Informative — Japanese only** |
| [architecture.md](architecture.md) | Logical architecture and component responsibilities | Normative (Phase 1) |
| [architecture_ja.md](architecture_ja.md) | Architecture (JA summary companion with figures) | **Informative — Japanese only** |
| [semantic-lineage-model.md](semantic-lineage-model.md) | Minimal semantic lineage unit and identifiers | Normative (Phase 1) |
| [rde-review-output.md](rde-review-output.md) | RDE review categories and minimal interchange record | Normative (Phase 1) |
| [representation-of-loss.md](representation-of-loss.md) | Requirements for representing lost semantic elements | Normative (Phase 1) |
| [audit-trail-relationship.md](audit-trail-relationship.md) | Relationship between RDE outputs and audit trails | Normative (Phase 1) |
| [versioning.md](versioning.md) | Specification versioning and evolution | Normative (Phase 1) |

### Recommended reading order

New readers SHOULD follow **[introduction → architecture → lineage model → RDE output → representation of loss → audit trail correlation → versioning]**, optionally returning to introduction for definitions. Editors preparing pull requests SHOULD touch [versioning.md](versioning.md) whenever normative headings move or materially change wording.

Public issue **[#3 — representation of lost elements](https://github.com/zyx-corporation/kotonoha-spec/issues/3)** tracks the interchange encoding work that complements [representation-of-loss.md](representation-of-loss.md).

## Incremental work (explicitly deferred)

- Full JSON Schema published for **all** interchange fields (beyond the minimal interchange record in [rde-review-output.md](rde-review-output.md)).
- Typed schemas that fix every optional lineage property enumerated in Phase 2+ increments.
- Wire-level APIs and network protocols (typically after interchange versioning stabilizes).
- Authentication, tenancy, scalability, or threat-model obligations (planned for broader reliability phases referenced from [architecture.md](architecture.md)).

See [versioning.md](versioning.md) for how incompatible changes are gated.
