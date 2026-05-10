# Representation of loss

## Requirement

Implementations that claim to capture semantic lineage **MUST** provide a **explicit** place for **lost** semantic elements when such loss is known or deliberately accepted—not merely infer loss from deleted characters in a lexical diff.

## Lost semantic elements

Loss includes, without limitation:

- Ambiguity, tension, or unresolved debate removed by clarification or simplification.
- Scope or stakeholder pain points omitted for brevity or readability.
- Responsibility or risk explicitly carried in prior text that disappears in a rewrite.

## Relationship to diffs

Lexical diffs (for example from version-control systems) **MAY** accompany lineage records but **MUST NOT** be the sole carrier of information about loss. The `lost` category in [rde-review-output.md](rde-review-output.md) exists to satisfy this obligation when using RDE interchange.

## Partial information

When loss is suspected but not fully characterized, implementations **SHOULD** still record the uncertainty under `intentionally_unresolved` or `lost` with an explicit summary rather than omitting the category.
