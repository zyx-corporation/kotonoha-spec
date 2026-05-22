# SLS-9 Phase 2 interchange and schema hardening

Status: **Normative (Phase 2 validation profile)**.

This document promotes Phase 2 from roadmap language into public `kotonoha-spec` normative text. It hardens the Phase 1 RDE review output interchange without replacing the Phase 1 conceptual model.

## SLS-9.1 Purpose

Phase 2 defines a validation profile for implementations that exchange or validate RDE review output records. It preserves Phase 1 record compatibility while making validator behavior, closed vocabularies, and schema references explicit.

Phase 2 does not claim that every future SLS interchange object is fully modeled. It standardizes the current minimum validation surface so independent tools can reject incompatible records consistently.

## SLS-9.2 Relationship to Phase 1

Phase 2 **MUST** remain compatible with the Phase 1 minimum RDE review output defined in [SLS-4](rde-review-output.md).

A Phase 2 validator **MUST** accept a valid Phase 1 minimum RDE review output record when:

- `spec_version` is `0.1`;
- `subject_ref` is a non-empty string;
- all seven required RDE categories are present;
- each category value is an array;
- no unknown category key is present;
- any present `source_context_status` value belongs to the Phase 1 closed vocabulary.

A Phase 2 validator **MUST NOT** reinterpret Phase 1 categories or make automated RDE output final authority over human judgment. Human accountability remains governed by [SLS-1.8](introduction.md#sls-18-human-accountability).

## SLS-9.3 Normative schema artifact

The normative JSON Schema artifact for the Phase 2 RDE review output validation profile is:

- [`schemas/rde-review-output.phase2.schema.json`](../schemas/rde-review-output.phase2.schema.json)

Implementations claiming Phase 2 RDE review output validation conformance **MUST** either:

- validate records against that schema; or
- implement equivalent validation behavior for the normative constraints in this document and [SLS-4](rde-review-output.md).

If a schema artifact and normative prose conflict, the normative prose in `docs/` prevails until the conflict is resolved by a subsequent normative change.

## SLS-9.4 Required category set

A Phase 2 validator **MUST** require exactly the following RDE category keys under `rde_review_output.categories`:

- `preserved`
- `transformed`
- `complemented`
- `intentionally_unresolved`
- `lost`
- `deviation_risk`
- `next_update_policy`

A Phase 2 validator **MUST** reject unknown category keys under `categories`.

A Phase 2 validator **MUST** accept empty arrays for any required category. Empty arrays mean no item was reported for that category; they do not mean the category is unsupported.

## SLS-9.5 Category item shape

Each category item **MUST** be a JSON object.

Each category item **SHOULD** include a non-empty human-readable `summary`. A validator **MAY** treat missing or empty `summary` as a warning in non-strict mode and as an error in strict mode, provided the mode is documented.

Implementations **MAY** include implementation-specific fields inside category items. Such fields **MUST NOT** be required to understand the seven normative categories.

## SLS-9.6 Source context status validation

When a category item contains `source_context_status`, a Phase 2 validator **MUST** require it to be a string from the closed vocabulary defined in [SLS-4.4.3](rde-review-output.md#sls-443-source-context-status-vocabulary):

- `supplied`
- `resolved`
- `inferred`
- `missing`
- `partial`
- `contested`

A Phase 2 validator **MUST** reject non-string values or unknown strings for `source_context_status`.

The `source_context_status` vocabulary remains a closed enum for Phase 2. It **MUST NOT** be extended without a later normative versioning decision under [SLS-8](versioning.md).

## SLS-9.7 Version handling

For this Phase 2 validation profile, `spec_version` remains `0.1` because the underlying minimum RDE review output record shape remains compatible with Phase 1.

Phase 2 validation conformance is a validator/profile claim, not an automatic bump of the RDE review output `spec_version` field.

A later specification bundle **MAY** introduce a new `spec_version` value when the interchange record itself changes in a way governed by [SLS-8](versioning.md).

## SLS-9.8 Implementation envelopes

Implementation envelopes such as `kotonoha.interchange.v1` are not automatically normative SLS interchange records.

A tool **MAY** wrap an RDE review output record in an implementation-specific envelope when exchanging data between local components. The wrapper remains non-normative unless this repository explicitly promotes it to normative text.

When a wrapper contains an RDE review output record and claims Phase 2 RDE validation conformance, the nested RDE review output record **MUST** satisfy this document.

## SLS-9.9 CLI and core validation behavior

A Phase 2 implementation profile **SHOULD** document:

- whether validation is strict or non-strict by default;
- whether missing `summary` is a warning or error;
- whether records are accepted through raw RDE JSON, an implementation envelope, or both;
- which `spec_version` values are accepted;
- how validation failures are surfaced to callers.

A validator **MUST** distinguish malformed records from well-formed records that are semantically weak or incomplete.

## SLS-9.10 Conformance statement

An implementation claiming **SLS Phase 2 RDE validation conformance** **MUST** state:

- the specification bundle or commit it targets;
- the supported input shape or shapes;
- the validator mode or modes;
- whether it uses the normative schema artifact or equivalent hand-written validation;
- how it handles `source_context_status`;
- how it preserves the human accountability boundary defined by Phase 1.

## SLS-9.11 Out of scope for Phase 2

The following remain outside Phase 2 unless explicitly promoted by a later normative document:

- complete schemas for every future SLS object;
- wire-level network protocols;
- database schemas or storage topology;
- product UI layout or interaction design;
- MCP resources, HTTP gateway routes, VS Code panels, web-console APIs, or console event wrappers as normative SLS protocols;
- authentication, tenancy, scalability, retention, or threat-model obligations.
