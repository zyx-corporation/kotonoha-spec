# SLS-8 Normative references and versioning

## SLS-8.1 Specification bundle

The Phase 1 publication is labeled **spec_version `0.1`** for RDE review output interchange records (see [SLS-4](rde-review-output.md)).

Phase 2 introduces a normative validation profile in [SLS-9](phase2-interchange-hardening.md) and a normative schema artifact at [`schemas/rde-review-output.phase2.schema.json`](../schemas/rde-review-output.phase2.schema.json). This profile does **not** by itself change the RDE review output `spec_version` value, because the minimum record shape remains compatible with Phase 1.

## SLS-8.2 Document evolution

Individual Markdown documents under `docs/` carry implicit patch-level updates until a numbered release process is published. Breaking changes to normative meaning **MUST** be called out in commit messages and **SHOULD** include an entry in [CHANGELOG.md](../CHANGELOG.md).

## SLS-8.3 Semantic versioning intent

Future releases of this specification bundle **intend** to follow semantic versioning semantics at the bundle level:

- **MAJOR**: incompatible normative changes for conforming implementations.
- **MINOR**: additive capabilities without breaking prior minimum requirements.
- **PATCH**: clarifications that do not change obligations.

Until a formal release tagging policy is published, readers **SHOULD** rely on repository revision history for precise deltas.

## SLS-8.4 Phase 1 compatibility rules

For Phase 1 interchange labeled with `spec_version`, implementations **SHOULD** interpret compatibility as follows:

- Patch-level changes include editorial clarifications, examples, non-normative notes, and wording changes that do not change obligations.
- Minor-level changes include backward-compatible additions, such as optional fields or explicitly extension-safe vocabulary values.
- Major-level changes include required field changes, removal of fields, semantic reinterpretation of existing fields, incompatible enum changes, or changes that invalidate previously conforming minimum records.

Closed enums **MUST NOT** be extended in a minor version. A vocabulary is extension-safe only when the specification explicitly marks it as open or extension-safe. The `source_context_status` vocabulary defined in [SLS-4.4.3](rde-review-output.md#sls-443-source-context-status-vocabulary) is a closed enum for Phase 1 and Phase 2.

Implementations that validate `spec_version` **SHOULD** fail closed for unknown major versions and **MAY** accept unknown minor or patch versions only when the fields and vocabularies used by the record remain compatible with the implementation's declared conformance level.

## SLS-8.5 Phase 2 validation profile versioning

SLS Phase 2 RDE validation conformance is a validator/profile claim, not an automatic change to the nested `rde_review_output.spec_version` value.

A validator claiming Phase 2 conformance **MUST** follow [SLS-9](phase2-interchange-hardening.md) for required categories, unknown category rejection, category item shape, and `source_context_status` validation.

If a later release changes the RDE review output record shape itself, that release **SHOULD** introduce a new `spec_version` value according to the compatibility rules above.

## SLS-8.6 References to external standards

Certain documents reference [BCP 14](https://www.rfc-editor.org/info/bcp14) for conformance keywords (see [SLS-1.3](introduction.md)).

The Phase 2 schema artifact uses JSON Schema Draft 2020-12.

## SLS-8.7 Section identifiers

Phase 1 and Phase 2 specification documents use stable hierarchical section identifiers such as `SLS-1.4.4`, `SLS-5.4.3`, and `SLS-9.6`. Editors **SHOULD** preserve these identifiers when changing wording. When a section is moved or split, the PR **SHOULD** describe the identifier change and update cross-references.
