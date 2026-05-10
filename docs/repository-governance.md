# Repository governance (informative)

**Status:** Informative — not normative. If this document disagrees with [`docs/introduction.md`](introduction.md) or other normative text, **the normative documents win**.

This file summarizes **how this repository relates** to implementations and auxiliary public repositories. It complements [CONTRIBUTING.md](../CONTRIBUTING.md).

---

## 1. Source of truth (public ecosystem)

From the perspective of **external reviewers and implementers**:

| Concern | Authoritative surface |
| --- | --- |
| **Normative specifications** (semantics, interchange shapes, conformance language) | **This repository (`kotonoha-spec`)** — English-first under [`docs/`](README.md). |
| **Shipped runtime behavior** of the OSS core crate | **[`kotonoha-core`](https://github.com/zyx-corporation/kotonoha-core)** — code is the artefact readers execute; alignment with specifications is enforced through issues, PRs, and **[spec traceability](https://github.com/zyx-corporation/kotonoha-core/blob/main/docs/spec-traceability.md)**. |
| **`kotonoha` CLI** contract (commands, exit codes, user-visible behaviour) | **[`kotonoha-cli`](https://github.com/zyx-corporation/kotonoha-cli)** — **`docs/cli-definition.md`**. The CLI definition does **not** replace normative spec text when they conflict. **`kotonoha-spec` wins.** |
| **Manuals, tutorials, procedural guides** (non‑normative) | **[`kotonoha-docs`](https://github.com/zyx-corporation/kotonoha-docs)** — must **not** redefine normative fields; cite this repository for exact semantics. |

Early drafting and non-public coordination may live **outside** this repository; only material that satisfies the **[public-boundary rules](introduction.md)** (and editorial quality for review) belongs under `docs/` as normative or clearly marked informative text.

---

## 2. Dependency direction

1. **Implementations should follow normative sections in `kotonoha-spec`** when distributing behaviour to users.
2. If a change **alters externally visible semantics** described (or implied) by normative documents, treat **`kotonoha-spec` as the place to resolve the design** — open or update issues/PRs here **before** or **in lockstep with** implementation PRs.
3. **Informative** documents may be updated after normative clarification as long as they stay consistent with Phase 1 scope in [`introduction.md`](introduction.md).

---

## 3. RDE and governance

Repository governance is **procedural**: it routes work and ownership. It must **not** narrow **RDE** (Review / Deviation / Evolution observation) to “whatever the current tool accepts.” Normative meaning of RDE categories and interchange records remains in **[`rde-review-output.md`](rde-review-output.md)** and related documents.

---

## 4. Traceability

When implementation behaviour is tied to a normative section, maintainers track mappings in **`kotonoha-core`** ([`docs/spec-traceability.md`](https://github.com/zyx-corporation/kotonoha-core/blob/main/docs/spec-traceability.md)) and may reference this repository’s document paths in issues and PRs.

---

## Changelog (document level)

| Date | Change |
| --- | --- |
| 2026-05-10 | Initial English summary derived from an internal governance draft (contributor-visible lineage). |
