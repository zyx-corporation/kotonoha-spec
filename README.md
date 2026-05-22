# kotonoha-spec

**Public specifications for the Semantic Lineage System (SLS).** This repository is the canonical, reviewable surface for external stakeholders, OSS implementations, and integrations in the Kotonoha ecosystem.

SLS is not merely a semantic diff or data interchange format. It specifies a reviewable institutional surface for recording meaning-relevant change, source context, limits of observation, and what remains open to later re-examination.

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

The Phase 1 **public specification MVP** (bundle **0.1**) lives under [`docs/`](docs/README.md). The specification uses stable hierarchical identifiers such as `SLS-1.4.4` and `SLS-5.4.3`.

| Number | Document | Description |
| --- | --- | --- |
| SLS-1 | [docs/introduction.md](docs/introduction.md) | Scope, definitions, conformance keywords |
| SLS-2 | [docs/architecture.md](docs/architecture.md) | Logical architecture |
| SLS-3 | [docs/semantic-lineage-model.md](docs/semantic-lineage-model.md) | Minimal lineage unit |
| SLS-4 | [docs/rde-review-output.md](docs/rde-review-output.md) | RDE categories and interchange record |
| SLS-5 | [docs/rde-implementation-specification.md](docs/rde-implementation-specification.md) | RDE implementation responsibilities and boundaries |
| SLS-6 | [docs/representation-of-loss.md](docs/representation-of-loss.md) | Requirements for lost elements |
| SLS-7 | [docs/audit-trail-relationship.md](docs/audit-trail-relationship.md) | RDE vs audit trails |
| SLS-8 | [docs/versioning.md](docs/versioning.md) | Versioning policy |

Start with [SLS-1 Introduction](docs/introduction.md), then read SLS-2 through SLS-8 in order. [docs/README.md](docs/README.md) contains the full index and deferred work.

**Non‑normative Japanese companions** (diagrams/summary/read order): [`docs/README_ja.md`](docs/README_ja.md), [`docs/introduction_ja.md`](docs/introduction_ja.md), [`docs/architecture_ja.md`](docs/architecture_ja.md), [`docs/rde-implementation-specification_ja.md`](docs/rde-implementation-specification_ja.md).

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
