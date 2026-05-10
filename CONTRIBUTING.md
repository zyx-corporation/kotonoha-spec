# Contributing to kotonoha-spec

Thank you for improving the public specifications for the Semantic Lineage System (SLS).

## Principles

1. **English-first** normative text under [`docs/`](docs/README.md). Keep Japanese or other translations alongside as `*_ja.md` when added.
2. **Normative vs informative:** repository [`README.md`](README.md) states policy; **`docs/`** holds normative Phase 1 material unless a section is explicitly marked non-normative.
3. **Public boundary:** do not require private repository names, internal codenames, or undisclosed assets in normative requirements (see [`docs/introduction.md`](docs/introduction.md)).
4. **RDE definitions** must remain operational (categories, interchange)—not metaphor-only.
5. **Cross-repo roles** (spec vs core vs CLI vs docs): see the informative summary in [`docs/repository-governance.md`](docs/repository-governance.md).

## Workflow

1. Open an **Issue** describing the spec gap or erratum.
2. Open a **Pull Request** with focused edits; link the Issue.
3. For breaking normative changes, discuss in the Issue first and update [`CHANGELOG.md`](CHANGELOG.md) and [`docs/versioning.md`](docs/versioning.md) as appropriate.

## Reviews

Reviewers should verify alignment with Phase 1 scope in [`docs/introduction.md`](docs/introduction.md) and check loss/deviation/human-accountability clauses where relevant.

## License

Unless stated otherwise in a file, contributions are accepted under the same terms as the repository ([Apache License 2.0](LICENSE)).
