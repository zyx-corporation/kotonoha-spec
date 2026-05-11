# Architecture (logical)

## Overview

SLS, as specified for Phase 1, comprises **logical responsibilities** that implementations MAY map to separate modules or combine. This document does not mandate deployment topology.

*Non-normative Japanese illustrative companion:* [architecture_ja.md](architecture_ja.md).

## Concept list *(informative only)*

The following table summarizes the concepts used by this architecture document. It is a reading aid; normative definitions remain in [introduction.md](introduction.md) and the relevant Phase 1 documents.

| Concept | Role in this architecture | Primary specification anchor |
| --- | --- | --- |
| **Kotonoha** | Ecosystem and institutional framing for SLS specifications, policies, and implementations | [introduction.md](introduction.md#kotonoha) |
| **SLS** | System family for recording semantic lineage across meaning-relevant changes | [introduction.md](introduction.md#semantic-lineage-system-sls) |
| **Semantic lineage** | Directed, inspectable relationship among lineage units | [introduction.md](introduction.md#semantic-lineage) |
| **Lineage unit** | Minimal persisted subject of semantic lineage | [semantic-lineage-model.md](semantic-lineage-model.md) |
| **ΔM** | Meaning-relevant change, distinct from raw textual diff | [introduction.md](introduction.md#δm-semantic-change) |
| **RDE review** | Structured observation of semantic deviation using RDE categories | [rde-review-output.md](rde-review-output.md) |
| **Interchange** | Serialized representation for exchanging lineage and RDE payloads between tools | [rde-review-output.md](rde-review-output.md), [versioning.md](versioning.md) |
| **Representation of loss** | Explicit surfacing of semantic elements that are removed, weakened, or no longer represented | [representation-of-loss.md](representation-of-loss.md) |
| **Audit correlation** | Correlatable relationship between review outputs, lineage records, and audit trails | [audit-trail-relationship.md](audit-trail-relationship.md) |
| **Human authority** | Human judgment and accountability for publication, approval, rejection, or revision | [introduction.md](introduction.md#human-accountability) |

## Figures *(informative only)*

Diagrams below **illustrate** relationships described in prose. If a diagram and a normative section conflict, **the normative text prevails.**

### Figure 1 — Logical responsibilities and interchange

```mermaid
flowchart TB
    subgraph ext["Existing collaboration tools (Phase 1 does not replace)"]
        VCS[VCS / diffs etc.]
        ISS[Issues / trackers]
        BRD[Project boards]
    end

    subgraph p1["Phase 1 responsibilities (normative prose in this repo)"]
        LR[Lineage representation]
        RDE[RDE review output]
        IX[Interchange]
        AUD[Audit correlation]
    end

    VCS -. MAY feed hints external to lineage .-> LR
    ISS -. MAY provide subject context .-> RDE

    LR <-->|serializes lineage-relevant payloads| IX
    RDE -->|serialized review observations per rde-review-output.md| IX
    RDE --> AUD
    IX --> AUD
```

### Figure 2 — RDE observations vs human authority

```mermaid
flowchart LR
    T[RDE-related tooling implementations]
    REC[RDE review output interchange record]
    H[Human judgment approval accountability]

    T --> REC
    REC -.->|MUST NOT by itself authorize or reject| H
    REC -. MAY inform discussion .-> H
```

## Logical components

### Lineage representation

**Responsibility:** persist and expose **lineage units** and their relationships according to [semantic-lineage-model.md](semantic-lineage-model.md).

**Non-requirements:** replacing Git, issue trackers, or project boards. Those tools **MAY** coexist; SLS addresses semantic lineage they do not fully capture.

### RDE review output

**Responsibility:** produce or consume **RDE review outputs** structured per [rde-review-output.md](rde-review-output.md), covering the observation categories listed there.

**Boundary:** an RDE review output records observations; it **MUST NOT**, by itself, normatively **authorize** or **reject** human decisions.

### Interchange

**Responsibility:** serialize RDE review outputs and lineage data for exchange between tools using the minimal interchange rules defined in Phase 1. Detailed schema evolution is governed by [versioning.md](versioning.md).

### Audit correlation

**Responsibility:** implementations that maintain audit trails **SHOULD** preserve a correlatable relationship between RDE review outputs and audit records as described in [audit-trail-relationship.md](audit-trail-relationship.md).

## Trust and scope boundaries

Phase 1 specifies **structural and documentary obligations**. It does **not** specify threat models, authentication, or availability targets (future phases MAY extend the specification).

## Traceability

Implementations **SHOULD** maintain explicit references (for example section identifiers) between behavior and the relevant normative sections in this repository.

## Implementation structuring (informative only)

Phase 1 **does not** mandate repository layout, module shapes, or the use of object-oriented **design patterns** in code.

The following **evolution signals** MAY warrant implementer-side **refactoring** — for example clearer decomposition, composition, or per-version validation paths — as long as externally visible obligations in this specification **do not** silently change ([versioning.md](versioning.md); traceability preserved):

| Signal | Typical direction *(examples only; not requirements)* |
| --- | --- |
| Multiple concurrently supported **interchange** shapes or **`spec_bundle` / spec version** validation lines | Separate validation per format or bundle (e.g. `enum` / `match`, small modules, traits) instead of one intersecting routine |
| **RDE** rules spread independently and regressions multiply | Factor rules into testable units; avoid an unnecessary monolithic validator |
| More than one **persistent or transport backend** behind the same behavioral contract | Shared interface with adapter-specific implementations |
| The same conformance logic is **duplicated** across crates or CLIs | Extract shared helpers or types **without** relaxing normative checks |

Prefer idiomatic structure in each language (e.g. Rust: traits, enums, composition). Internal engineering mirrors MAY expand on timing; they are **not** normative here.

## Reference interchange (informative only)

OSS implementations MAY attach conformance metadata via the **`kotonoha.interchange.v1`** vocabulary defined in **[`src/interchange.rs`](https://github.com/zyx-corporation/kotonoha-core/blob/main/src/interchange.rs)** in **`kotonoha-core`**. This format guides validators and tooling; when it diverges from normative wording in Phase 1, **the Phase 1 English documents prevail** until superseded according to [versioning.md](versioning.md).

**Informative tightening in `kotonoha-core` (Phase 2 tooling):** current releases **`#[serde(deny_unknown_fields)]` on deserialization** — the interchange JSON object **MUST NOT** carry unknown **top‑level** keys (`format`, `spec_bundle`, `lineage_unit`, `rde_document` only); when `lineage_unit` is present, that nested object **`id` / `prior_unit_id` only**. This guards against unstructured extensions being mistaken for contract evolution **without** extending Phase 1 normative requirements in `docs/` here.

The official **[`kotonoha` CLI definition](https://github.com/zyx-corporation/kotonoha-cli/blob/main/docs/cli-definition.md)** documents command surfaces, interchange validation/storage paths, and exit-code semantics referenced from contributor guides; it likewise remains informative relative to canonical requirements in `docs/`.
