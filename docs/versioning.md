# SLS-8 Normative references and versioning

## SLS-8.1 Specification bundle

The Phase 1 publication is labeled **spec_version `0.1`** for interchange records (see [SLS-4](rde-review-output.md)).

## SLS-8.2 Document evolution

Individual Markdown documents under `docs/` carry implicit patch-level updates until a numbered release process is published. Breaking changes to normative meaning **MUST** be called out in commit messages and **SHOULD** include an entry in [CHANGELOG.md](../CHANGELOG.md).

## SLS-8.3 Semantic versioning intent

Future releases of this specification bundle **intend** to follow semantic versioning semantics at the bundle level:

- **MAJOR**: incompatible normative changes for conforming implementations.
- **MINOR**: additive capabilities without breaking prior minimum requirements.
- **PATCH**: clarifications that do not change obligations.

Until a formal release tagging policy is published, readers **SHOULD** rely on repository revision history for precise deltas.

## SLS-8.4 References to external standards

Certain documents reference [BCP 14](https://www.rfc-editor.org/info/bcp14) for conformance keywords (see [SLS-1.3](introduction.md)).

## SLS-8.5 Section identifiers

Phase 1 specification documents use stable hierarchical section identifiers such as `SLS-1.4.4` and `SLS-5.4.3`. Editors **SHOULD** preserve these identifiers when changing wording. When a section is moved or split, the PR **SHOULD** describe the identifier change and update cross-references.
