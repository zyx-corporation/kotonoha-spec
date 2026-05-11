# SLS-3 Semantic lineage model (minimal)

## SLS-3.1 Lineage unit

A **lineage unit** is the smallest addressable record of semantic lineage that implementations **MUST** be able to identify and relate when claiming conformance to lineage features.

### SLS-3.1.1 Identifier

Each lineage unit **MUST** carry an **`id`** that is unique within the **scope** of the deployment or dataset to which the implementation applies. Global uniqueness **SHOULD** be achieved by using a URI or IRI when feasible.

**NOTE:** The exact syntax of `id` is implementation-defined in Phase 1; interoperability profiles MAY constrain it later.

### SLS-3.1.2 Optional properties (Phase 1)

Implementations **MAY** attach further machine-readable properties (for example timestamps, author references, relations to other units). Phase 1 **does not** require a fixed schema beyond `id` and the ability to relate units.

### SLS-3.1.3 Relationships

When an implementation records that one lineage unit succeeds or derives from another, it **SHOULD** reference the prior unit’s `id` explicitly (for example `prior_unit_id` or linked graph edges). Exact graph topology is **not** fully fixed in Phase 1.

## SLS-3.2 Semantic change vs lexical change

Implementations **MUST NOT** equate semantic lineage with raw character-level diffs alone. Lineage units **MAY** reference external change carriers (for example VCS object IDs) as **hints**, but **MUST** keep semantic observations in [SLS-4](rde-review-output.md) or explicit lineage fields—not solely in uninterpreted diffs.

## SLS-3.3 Incremental specification

Richer typed schemas for lineage units (mandatory fields beyond `id`, standardized relation types) **MAY** be added in later specification versions without breaking the Phase 1 minimum.
