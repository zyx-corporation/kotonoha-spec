# Documentation content specification

Status: **Informative — documentation governance**.

This document gives a concrete placement specification for public Kotonoha documents. It refines [documentation-placement-policy.md](documentation-placement-policy.md) by listing document classes, expected repositories, language structure, and migration handling.

Tracked by: [zyx-corporation/kotonoha-spec#19](https://github.com/zyx-corporation/kotonoha-spec/issues/19)

## Purpose

Kotonoha uses more than one public repository. This document prevents conceptual explanations, tutorials, manuals, schemas, and conformance rules from drifting into ambiguous locations.

The core rule is:

- Put **normative or conformance-bearing specification content** in `kotonoha-spec`.
- Put **reader-facing explanation, learning, operation, and publication content** in `kotonoha-docs`.
- Link between them instead of duplicating authority.

## Repository responsibility matrix

| Document class | Primary repository | Preferred location | Status expectation | Notes |
| --- | --- | --- | --- | --- |
| Specification scope and terminology | `kotonoha-spec` | `docs/introduction.md` and related normative files | Normative when marked so | Defines terms needed for conformance. |
| Logical architecture responsibilities | `kotonoha-spec` | `docs/architecture.md` | Normative for Phase 1 | Diagrams may be informative, but responsibility prose is specification-facing. |
| Minimal data models | `kotonoha-spec` | `docs/*model*.md` | Normative when marked so | Example: lineage unit requirements. |
| Interchange records and compatibility | `kotonoha-spec` | `docs/*interchange*`, `docs/rde-review-output.md`, `docs/versioning.md` | Normative when marked so | Schemas and versioning belong in spec. |
| RDE category definitions used for conformance | `kotonoha-spec` | `docs/rde-review-output.md` | Normative when marked so | Explanatory essays on RDE belong in docs. |
| Audit correlation requirements | `kotonoha-spec` | `docs/audit-trail-relationship.md` | Normative when marked so | Operational audit examples may live in docs. |
| Representation of loss requirements | `kotonoha-spec` | `docs/representation-of-loss.md` | Normative when marked so | Conceptual examples may live in docs. |
| Repository governance | `kotonoha-spec` | `docs/repository-governance.md`, `docs/documentation-placement-policy.md`, this document | Informative governance unless otherwise marked | Defines public repo roles and placement rules. |
| Git/Issue/PR workflow | `kotonoha-spec` or org-level docs | `docs/git_operation_rules.md` until moved to a shared governance repo | Informative process | May later move to shared organization governance. |
| Concept explanations | `kotonoha-docs` | `en/concepts/`, `ja/concepts/` | Non-normative | Link back to spec for exact definitions. |
| Glossary for general readers | `kotonoha-docs` | `en/glossary/`, `ja/glossary/` or `en/concepts/glossary.md`, `ja/concepts/glossary.md` | Non-normative | Should not redefine normative terms. |
| Tutorials | `kotonoha-docs` | `en/tutorials/`, `ja/tutorials/` | Non-normative | Step-by-step learning. |
| Manuals | `kotonoha-docs` | `en/manual/`, `ja/manual/` | Non-normative | Operational usage and troubleshooting. |
| Acceptance demos | `kotonoha-docs` | `en/acceptance/`, `ja/acceptance/` | Non-normative validation material | May cite spec obligations and expected behavior. |
| Method documents | `kotonoha-docs` | `en/method/`, `ja/method/` | Non-normative | Explains how the project uses SLS/RDE. |
| SVG diagrams and explanatory visuals | `kotonoha-docs` | `en/assets/`, `ja/assets/`, or shared `assets/` with language-specific pages | Non-normative unless embedded from spec | Spec may link to stable diagrams, but diagrams do not override prose. |
| Public site HTML and generated pages | `kotonoha-docs` | `en/`, `ja/`, `site/`, or generated publication folders | Non-normative | Build rules belong in docs tooling. |
| Implementation build docs tied to code | Implementation repo | Example: `kotonoha-core/docs/` | Implementation-specific | Link to spec and docs as needed. |

## Concrete placement guidance

### `kotonoha-spec`

Keep `kotonoha-spec` small and stable. A file belongs here when it would be wrong for two conforming implementations to interpret it differently.

Recommended classes:

1. **Normative specification files**
   - Scope and definitions.
   - Logical responsibilities.
   - Minimal lineage model.
   - RDE output categories.
   - Interchange and versioning.
   - Representation of loss.
   - Audit correlation.

2. **Specification governance files**
   - Repository role policy.
   - Documentation placement policy.
   - Versioning and compatibility policy.
   - Contribution process where it affects the specification surface.

3. **Informative companions that support specification reading**
   - Requirements overview.
   - Japanese companion summaries.
   - High-level diagrams that help read the specification.

Informative files in `kotonoha-spec` must be clearly marked. They must not create new obligations accidentally.

### `kotonoha-docs`

Use `kotonoha-docs` for documents that help people understand, learn, operate, communicate, or publish Kotonoha.

Recommended classes:

1. **Concepts**
   - What is Kotonoha?
   - What is SLS?
   - What is semantic lineage?
   - What is ΔM?
   - What is RDE?
   - What is the memory layer?
   - What does auditability mean in Kotonoha?

2. **Tutorials**
   - First CLI session.
   - First lineage review.
   - Reading RDE output.
   - Moving from Git diff to semantic lineage.

3. **Manuals**
   - Installation and configuration.
   - Operating CLI workflows.
   - Troubleshooting.
   - Release or migration checklists.

4. **Acceptance demos**
   - Phase-specific validation procedures.
   - Expected command results.
   - Public behavior checks.

5. **Method documents**
   - SLS + RDE development method.
   - Kotonoha Method chapters.
   - Process explanations and diagrams.

6. **Visual/publication assets**
   - SVG diagrams.
   - HTML pages.
   - CSS assets.
   - Generated static pages.

## Language structure

### `kotonoha-spec`

`kotonoha-spec` is English-primary. Japanese files may exist as `_ja.md` companions. Japanese companion documents are informative unless explicitly stated otherwise.

### `kotonoha-docs`

`kotonoha-docs` uses repository-root language areas:

```text
en/
ja/
```

Both language areas should normally mirror structure:

```text
en/concepts/
ja/concepts/
en/tutorials/
ja/tutorials/
en/manual/
ja/manual/
en/acceptance/
ja/acceptance/
en/method/
ja/method/
```

When a translation is pending, the corresponding file or index should state that it is pending and link to the available source.

## Migration specification

Existing material should migrate gradually. Do not break public links without a compatibility path.

| Current pattern | Target pattern | Migration approach |
| --- | --- | --- |
| `docs/tutorials/` | `en/tutorials/` | Move or mirror through PR; leave compatibility links if needed. |
| `docs/tutorials_ja/` | `ja/tutorials/` | Move or mirror through PR; keep Japanese structure aligned with English. |
| `docs/acceptance/` | `en/acceptance/` | Move or mirror through PR. |
| `docs/acceptance_ja/` | `ja/acceptance/` | Move or mirror through PR. |
| `docs/method/` | `en/method/` | Move or mirror when publication structure stabilizes. |
| `docs/method_ja/` | `ja/method/` | Move or mirror when publication structure stabilizes. |
| Conceptual material currently in `kotonoha-spec` | `en/concepts/`, `ja/concepts/` | Keep concise spec references; move expanded explanation to docs. |

## Edge cases

### A concept is both explanatory and specification-critical

Put the precise definition in `kotonoha-spec`. Put examples, metaphors, diagrams, and reader-facing explanation in `kotonoha-docs`.

### A tutorial requires exact schema behavior

Keep the tutorial in `kotonoha-docs`. Link to the exact `kotonoha-spec` section for the schema behavior.

### An SVG diagram explains a normative concept

Prefer storing explanatory SVGs in `kotonoha-docs`. If the diagram is included in `kotonoha-spec`, mark it informative unless the specification explicitly defines otherwise. Prose controls over diagrams.

### A Japanese document is written first

This may happen in `kotonoha-docs`. Create the corresponding `en/` path and mark translation as pending if necessary. In `kotonoha-spec`, English remains primary.

## PR checklist

Every PR that adds, moves, or reclassifies public documentation should answer:

1. Is this document normative, informative governance, or explanatory?
2. Which repository is authoritative for this document class?
3. Does the document duplicate normative text that should instead be linked?
4. Is there an English/Japanese counterpart or pending marker?
5. Are diagrams informative unless explicitly specified otherwise?
6. Does the PR preserve old links or explain migration impact?
7. What meaning was preserved, transformed, complemented, left unresolved, or exposed to drift risk?
