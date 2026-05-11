# Documentation placement policy

Status: **Informative — repository governance**.

This document defines how public Kotonoha documents are placed between `kotonoha-spec` and `kotonoha-docs`. It is a governance rule for documentation organization. It does not change the normative content of the Phase 1 specification bundle.

Tracked by: [zyx-corporation/kotonoha-spec#17](https://github.com/zyx-corporation/kotonoha-spec/issues/17)

## Principle

`kotonoha-spec` is the canonical public specification surface for the Semantic Lineage System (SLS). It should contain normative requirements, schemas, interface contracts, versioning rules, and other externally stable specification material.

`kotonoha-docs` is the public explanatory documentation surface. It should contain conceptual explanations, tutorials, manuals, acceptance demos, method documents, reader guides, diagrams, and other non-normative materials.

When a document primarily explains a concept rather than defining a stable requirement, it should normally live in `kotonoha-docs`. `kotonoha-spec` may link to that document when the explanatory background helps readers interpret the specification.

## Repository roles

| Repository | Primary role | Typical content |
| --- | --- | --- |
| `kotonoha-spec` | Canonical public specification | Normative requirements, minimal data models, interchange obligations, versioning, conformance language, repository governance |
| `kotonoha-docs` | Public explanatory documentation | Concept guides, tutorials, manuals, acceptance demos, method documents, glossaries, explanatory diagrams, public learning materials |

## What belongs in `kotonoha-spec`

Documents belong in `kotonoha-spec` when they define or constrain implementation obligations, public interchange behavior, compatibility, conformance, or governance that affects the specification surface.

Examples:

- Scope and terminology required for conformance.
- Logical architecture responsibilities.
- Minimal semantic lineage model.
- RDE review output categories and interchange records.
- Representation of loss requirements.
- Audit trail relationship requirements.
- Versioning policy.
- Specification governance and placement policy.

`kotonoha-spec` may contain informative documents when they support specification governance or reading order. Informative documents must be clearly marked so they do not appear to override normative specification text.

## What belongs in `kotonoha-docs`

Documents belong in `kotonoha-docs` when they help readers understand, learn, operate, validate, or communicate the Kotonoha ecosystem without defining normative implementation obligations.

Examples:

- Conceptual explanations of SLS, semantic lineage, ΔM, RDE, memory layer, and auditability.
- Tutorials and learning paths.
- Manuals and operational guides.
- Acceptance demos and command walkthroughs.
- Public diagrams, especially SVG-based explanatory figures.
- Method documents explaining how the project uses SLS and RDE in its own work.
- Japanese and English reader-facing companions.

## Link direction

`kotonoha-spec` may link to `kotonoha-docs` for explanatory background. Such links are informative unless a normative specification section explicitly states otherwise.

`kotonoha-docs` should link back to `kotonoha-spec` whenever a reader needs canonical semantics, stable requirements, schemas, or conformance rules.

If explanatory material conflicts with normative specification text, `kotonoha-spec` prevails.

## Language structure in `kotonoha-docs`

`kotonoha-docs` should maintain repository-root language areas:

- `en/` for English public documentation.
- `ja/` for Japanese public documentation.

The two language areas should normally use the same structure and hold translation pairs or equivalent companion documents. When one language leads temporarily, the corresponding location in the other language should indicate that the translation is pending or link to the available source.

Legacy or transitional locations may remain for compatibility during migration, but new conceptual and explanatory documentation should prefer the `en/` and `ja/` language-root structure.

## Language structure in `kotonoha-spec`

`kotonoha-spec` remains English-primary. Japanese companion documents may exist alongside English sources with `_ja.md` suffixes, but they are informative unless explicitly specified otherwise.

This differs intentionally from `kotonoha-docs`, where `en/` and `ja/` are reader-facing language areas.

## Migration rule

When an existing `kotonoha-spec` document is primarily explanatory, future work should consider moving or summarizing that material in `kotonoha-docs` and replacing the `kotonoha-spec` copy with a concise link or specification-facing reference.

Such movement must be tracked by an Issue and PR. The PR should state what meaning was preserved, transformed, complemented, left unresolved, and what drift risk was introduced.

## RDE-oriented placement check

Before adding or moving a document, reviewers should ask:

| Question | Placement implication |
| --- | --- |
| Does this define a conformance obligation? | Prefer `kotonoha-spec`. |
| Does this define a schema, interchange shape, or versioning rule? | Prefer `kotonoha-spec`. |
| Does this explain a concept for readers without fixing behavior? | Prefer `kotonoha-docs`. |
| Does this teach a workflow or usage pattern? | Prefer `kotonoha-docs`. |
| Does this contain diagrams or narrative explanation? | Prefer `kotonoha-docs`, linking back to `kotonoha-spec` if needed. |
| Would moving it change normative meaning? | Keep or summarize in `kotonoha-spec`; move explanatory expansion to `kotonoha-docs`. |

## Issue-first requirement

Documentation placement changes must follow the repository workflow: Issue first, branch second, PR third, merge after review or self-check. Documentation changes are meaning changes and should be traceable as such.
