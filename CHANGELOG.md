# Changelog

All notable changes to this specification repository are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed

- [versioning.md](docs/versioning.md): reflected Phase 2 validation profile versioning; Phase 2 conformance does not by itself change nested `rde_review_output.spec_version` from `0.1`.
- [phase-and-milestone-definition.md](docs/phase-and-milestone-definition.md) / Japanese companion: Phase 2 is now a promoted normative validation profile, not only roadmap language.
- [architecture.md](docs/architecture.md): **Reference interchange** に `kotonoha-core` がトップレベル／**`lineage_unit`** で未知 JSON フィールドを拒否する **informative** 補足（Phase 1 規範の拡張ではない）。

- [architecture.md](docs/architecture.md): added informative **Mermaid** diagrams (logical responsibilities ↔ interchange; RDE outputs vs human authority); reference interchange / CLI appendix unchanged in intent.
- [introduction.md](docs/introduction.md): Phase 1 document map; informative **Mermaid** figures for concept layering, applicability envelope, and reading order; incremental obligations clarified; OSS informative pointers (`kotonoha-core` interchange, `kotonoha-cli` definition).
- [docs/README.md](docs/README.md): recommended reading sequence and richer deferred-work list.

### Added

- [phase2-interchange-hardening.md](docs/phase2-interchange-hardening.md): promoted Phase 2 interchange and schema hardening as **SLS-9**, a normative RDE validation profile.
- [schemas/rde-review-output.phase2.schema.json](schemas/rde-review-output.phase2.schema.json): added normative Phase 2 JSON Schema artifact for the minimum RDE review output validation profile.
- [docs/README.md](docs/README.md): indexed Phase 2 normative validation profile and schema artifact.
- Non‑normative Japanese companion docs: [`docs/README_ja.md`](docs/README_ja.md) (reading order helpers), [`docs/introduction_ja.md`](docs/introduction_ja.md) and [`docs/architecture_ja.md`](docs/architecture_ja.md) (diagrams mapped to normative English); cross‑links from root [`README`](README.md) / [`README_ja`](README_ja.md) and `docs/README.md` ([discussion / tracking #4](https://github.com/zyx-corporation/kotonoha-spec/issues/4)).
- Informative [`docs/repository-governance.md`](docs/repository-governance.md) summarizing ecosystem roles (spec / core / CLI / docs) for public contributors.
- Public tracking **[#3 — representation of lost elements](https://github.com/zyx-corporation/kotonoha-spec/issues/3)** linked from implementation CONTRIBUTING/traceability flows.

## [0.1.0] — 2026-05-10

### Added

- Phase 1 **public specification MVP**: normative documents under `docs/` covering introduction, logical architecture, minimal semantic lineage model, RDE review output interchange, representation of loss, audit-trail relationship, and versioning policy.
