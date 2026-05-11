# kotonoha-spec

**Public specifications for the Semantic Lineage System (SLS).** This repository is the canonical, reviewable surface for external stakeholders, OSS implementations, and integrations in the Kotonoha ecosystem.

**Japanese:** [README_ja.md](README_ja.md)（日本語版）

## What belongs here

- Normative specifications and conformance language
- Interface definitions, schemas, and format specifications
- Specification governance and externally stable public roadmap material
- Public design rationale when it directly supports specification interpretation
- Contributor-facing guides for specification work (as needed)

Conceptual explanations, tutorials, manuals, acceptance demos, and reader-facing guides normally belong in [`kotonoha-docs`](https://github.com/zyx-corporation/kotonoha-docs). This repository may link to those explanatory documents when useful. See [Documentation placement policy](docs/documentation-placement-policy.md).

Drafts and notes that are not yet ready for public review are refined outside this repository; only material that is ready for stable, public publication is added here.

## Specification index (normative — Phase 1)

The Phase 1 **public specification MVP** (bundle **0.1**) lives under [`docs/`](docs/README.md). Start with [Requirements overview](docs/requirements-overview.md) for the full architecture picture and use cases, then read [Introduction](docs/introduction.md) (includes the **[Phase 1 document map](docs/introduction.md#phase-1-document-map)**), architecture, lineage model, RDE output, loss, audit correlation, and versioning.

| Document | Description |
| --- | --- |
| [docs/README.md](docs/README.md) | Full index and deferred work |
| [docs/documentation-placement-policy.md](docs/documentation-placement-policy.md) | Role split between `kotonoha-spec` and `kotonoha-docs` |
| [docs/requirements-overview.md](docs/requirements-overview.md) | System requirements overview, full architecture picture, and use cases |
| [docs/introduction.md](docs/introduction.md) | Scope, definitions, conformance keywords |
| [docs/architecture.md](docs/architecture.md) | Logical architecture |
| [docs/semantic-lineage-model.md](docs/semantic-lineage-model.md) | Minimal lineage unit |
| [docs/rde-review-output.md](docs/rde-review-output.md) | RDE categories and interchange record |
| [docs/representation-of-loss.md](docs/representation-of-loss.md) | Requirements for lost elements |
| [docs/audit-trail-relationship.md](docs/audit-trail-relationship.md) | RDE vs audit trails |
| [docs/versioning.md](docs/versioning.md) | Versioning policy |

**Non‑normative Japanese companions** (diagrams/summary/read order): [`docs/README_ja.md`](docs/README_ja.md), [`docs/introduction_ja.md`](docs/introduction_ja.md), [`docs/architecture_ja.md`](docs/architecture_ja.md).

Contributors: see [CONTRIBUTING.md](CONTRIBUTING.md). Release notes: [CHANGELOG.md](CHANGELOG.md).

## Repository governance (informative)

How this repository interacts with implementations and auxiliary repos (source of truth, change flow, RDE scope) is summarized in English in **[`docs/repository-governance.md`](docs/repository-governance.md)** — *informative only*; normative text remains under [`docs/`](docs/).

The public document placement rule is summarized in **[`docs/documentation-placement-policy.md`](docs/documentation-placement-policy.md)**. In short: stable specification obligations belong here; explanatory and conceptual documents belong in [`kotonoha-docs`](https://github.com/zyx-corporation/kotonoha-docs), with links back to this repository for canonical semantics.

## Related repositories

Public cross-references only.

| Repository | Role |
| --- | --- |
| **kotonoha-spec (this repository)** | Canonical public specifications |
| [`kotonoha-core`](https://github.com/zyx-corporation/kotonoha-core) | OSS core implementation of SLS |
| [`kotonoha-cli`](https://github.com/zyx-corporation/kotonoha-cli) | Official `kotonoha` CLI ([definition](https://github.com/zyx-corporation/kotonoha-cli/blob/main/docs/cli-definition.md)) |
| [`kotonoha-docs`](https://github.com/zyx-corporation/kotonoha-docs) | Non-specification public docs (manuals, tutorials, guides, conceptual explanations) |

Implementations in [`kotonoha-core`](https://github.com/zyx-corporation/kotonoha-core) should follow the public specifications in this repository whenever possible.

## Language policy

**By default, documents under `kotonoha-spec` are written in English.** Japanese translations may be provided alongside the English source. When a Japanese version is added, keep English as the primary document and use the `*_ja.md` suffix for Japanese files so pairs stay obvious (for example, `README.md` / `README_ja.md`).

## License

Unless otherwise stated in a specific file, repository content is licensed under the [Apache License 2.0](LICENSE).

## Links

- Repository: https://github.com/zyx-corporation/kotonoha-spec
- Normative specs: [`docs/`](docs/README.md)
- GitHub Projects (organization workflow): [`docs/github_projects_policy.md`](docs/github_projects_policy.md)
