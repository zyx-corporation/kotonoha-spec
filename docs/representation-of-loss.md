# SLS-6 Representation of loss

## SLS-6.1 Requirement

Implementations that claim to capture semantic lineage **MUST** provide an **explicit** place for **lost** semantic elements when such loss is known or deliberately accepted—not merely infer loss from deleted characters in a lexical diff.

## SLS-6.2 Lost semantic elements

Loss includes, without limitation:

- Ambiguity, tension, or unresolved debate removed by clarification or simplification.
- Scope or stakeholder pain points omitted for brevity or readability.
- Responsibility or risk explicitly carried in prior text that disappears in a rewrite.

## SLS-6.3 Relationship to diffs

Lexical diffs (for example from version-control systems) **MAY** accompany lineage records but **MUST NOT** be the sole carrier of information about loss. The `lost` category in [SLS-4](rde-review-output.md) exists to satisfy this obligation when using RDE interchange.

## SLS-6.4 Partial information

When loss is suspected but not fully characterized, implementations **SHOULD** still record the uncertainty under `intentionally_unresolved` or `lost` with an explicit summary rather than omitting the category.

## SLS-6.5 Interim encoding (Phase 3 gate — 2026-05)

Until a dedicated interchange encoding for lost elements beyond the RDE `lost` category is published, implementations **SHOULD** satisfy [SLS-6.1](representation-of-loss.md#sls-61-requirement) through the **`lost` category** and related fields defined in [SLS-4](rde-review-output.md). Standalone identifiers or payloads that are independent of an enclosing RDE record remain **out of scope** for this specification bundle; track normative follow-up under public Issue [#3](https://github.com/zyx-corporation/kotonoha-spec/issues/3) (child of rollup [#25](https://github.com/zyx-corporation/kotonoha-spec/issues/25)). This section does **not** weaken [SLS-6.1](representation-of-loss.md#sls-61-requirement); it records the **approved interim** so implementers do not invent parallel loss vocabularies in code alone.
