# Contributing to kotonoha-spec

Thank you for improving the public specifications for the Semantic Lineage System (SLS).

## Principles

1. **English-first** normative text under [`docs/`](docs/README.md). Keep Japanese or other translations alongside as `*_ja.md` when added.
2. **Normative vs informative:** repository [`README.md`](README.md) states policy; **`docs/`** holds normative Phase 1 material unless a section is explicitly marked non-normative.
3. **Public boundary:** do not require private repository names, internal codenames, or undisclosed assets in normative requirements (see [`docs/introduction.md`](docs/introduction.md)).
4. **RDE definitions** must remain operational (categories, interchange)—not metaphor-only.
5. **Cross-repo roles** (spec vs core vs CLI vs docs): see the informative summary in [`docs/repository-governance.md`](docs/repository-governance.md).
6. **Lost elements modelling** beyond Phase 1’s minimum (`docs/representation-of-loss.md`, related RDE wording): incremental normative additions may correlate with **[`kotonoha-spec` issue #3](https://github.com/zyx-corporation/kotonoha-spec/issues/3)** (tracking / discussion).
7. **Git workflow** (Issues, branches, PRs to `main` only — detailed rules): [`docs/git_operation_rules.md`](docs/git_operation_rules.md) (Japanese; self-contained document in this repo).

### Semantic / RDE review cues (informative)

Open or broaden discussion **before merging** materially normative wording when:

- **Semantic-loss or deviation wording** downstream validators depend on (for example **`lost`** checks and reviewer-facing categories referenced from [`docs/rde-review-output.md`](docs/rde-review-output.md)).
- The same editorial decision appears **misaligned across partner repositories** (spec versus docs/tooling narration), signalling an intent vs captured-result gap worth reconciling explicitly.
- A change would reshape **`kotonoha.interchange.v1`**-shaped artifacts or tooling validation paths exercised by **`kotonoha-cli` CI**—coordinate with downstream traceability tracked in governance notes ([`repository-governance.md`](repository-governance.md)).

### RDE-style review triggers (informative, Phase 3 W-1)

Beyond the bullets above, prefer opening or widening discussion **early** when work touches **multi-repository normative posture**, **interchange-shaped artifacts**, or **CLI exit-code / validation meaning**—even if the eventual merge is only in this repository. **PR checklist fields are recommended** until the meta-track [kotonoha-management#34](https://github.com/zyx-corporation/kotonoha-management/issues/34) converges on which trace rows are mandatory.

## Workflow

1. Open an **Issue** describing the spec gap or erratum.
2. Open a **Pull Request** with focused edits; link the Issue.
3. For breaking normative changes, discuss in the Issue first and update [`CHANGELOG.md`](CHANGELOG.md) and [`docs/versioning.md`](docs/versioning.md) as appropriate.

## Reviews

Reviewers should verify alignment with Phase 1 scope in [`docs/introduction.md`](docs/introduction.md) and check loss/deviation/human-accountability clauses where relevant. When **RDE category shapes** or JSON validation expectations would change in [`kotonoha-core`](https://github.com/zyx-corporation/kotonoha-core), flag follow-up for **[`unit_testing_guidelines.md`](https://github.com/zyx-corporation/kotonoha-core/blob/main/docs/unit_testing_guidelines.md)** (and `spec-traceability`) or open a linked issue there before merging if behaviour must move ahead of spec text.

### Partner implementation PRs (**M3.5 / P3-5b**, informative)

**[`kotonoha-core`](https://github.com/zyx-corporation/kotonoha-core)** and **[`kotonoha-cli`](https://github.com/zyx-corporation/kotonoha-cli)** pull requests SHOULD link an open **`kotonoha-spec` Issue or PR** when they alter observable interchange validation (`kotonoha.interchange.v1` / `console_event`), **CLI exit semantics** surfaced in **`cli-definition`**, or other behaviour this repository’s **`docs/`** normative prose is intended to constrain. Prefer **specific spec Issues** tied to each change; for cross-cutting escalation themes use rollup **[Issue #25](https://github.com/zyx-corporation/kotonoha-spec/issues/25)** (Phase 3 M3.5 index).

## License

Unless stated otherwise in a file, contributions are accepted under the same terms as the repository ([Apache License 2.0](LICENSE)).
