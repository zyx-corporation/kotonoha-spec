# Normative references and versioning

## Specification bundle

The Phase 1 publication is labeled **spec_version `0.1`** for interchange records (see [rde-review-output.md](rde-review-output.md)).

## Document evolution

Individual Markdown documents under `docs/` carry implicit patch-level updates until a numbered release process is published. Breaking changes to normative meaning **MUST** be called out in commit messages and **SHOULD** include an entry in [CHANGELOG.md](../CHANGELOG.md).

## Semantic versioning intent

Future releases of this specification bundle **intend** to follow semantic versioning semantics at the bundle level:

- **MAJOR**: incompatible normative changes for conforming implementations.
- **MINOR**: additive capabilities without breaking prior minimum requirements.
- **PATCH**: clarifications that do not change obligations.

Until a formal release tagging policy is published, readers **SHOULD** rely on repository revision history for precise deltas.

## References to external standards

Certain documents reference [BCP 14](https://www.rfc-editor.org/info/bcp14) for conformance keywords (see [introduction.md](introduction.md)).
