# SLS-4 RDE review output

## SLS-4.1 Overview

An **RDE review output** is a structured interchange record capturing observations about a **subject** (for example a document revision, patch proposal, or design decision artifact). It operationalizes RDE without relying on metaphor alone.

## SLS-4.2 Subject reference

Each RDE review output **MUST** identify the **subject** under review with a **subject_ref** string. Implementations **SHOULD** use a URI or URI-reference resolvable within their deployment context.

## SLS-4.3 Observation categories

Implementations **MUST** classify observations into the following categories. Each category **MAY** be empty when there is nothing material to report for that category.

| Category key | Meaning |
| --- | --- |
| `preserved` | Elements of intent, constraint, or responsibility deliberately kept intact. |
| `transformed` | Elements whose expression or structure changed while carrying meaning forward in altered form. |
| `complemented` | New explanations, assumptions, or structure not present before the change. |
| `intentionally_unresolved` | Deliberately deferred tensions, ambiguity, or open questions (distinct from “not yet worked”). |
| `lost` | Semantic elements weakened or erased—including ambiguity, pain points, responsibility, or scope—**not** inferable from deleted text alone (see [SLS-6](representation-of-loss.md)). |
| `deviation_risk` | Risks of unacceptable drift (for example scope shrink not agreed, metaphor overload, or operational narrowing of RDE). |
| `next_update_policy` | Explicit carry-forward items for the next edit, design decision, or publication step. |

## SLS-4.4 Minimal interchange record

Implementations **MUST** be able to emit or ingest a record with at least the following logical shape. Field names are normative for interchange labeled **compliant with SLS Phase 1 RDE interchange**.

```json
{
  "rde_review_output": {
    "spec_version": "0.1",
    "subject_ref": "https://example.invalid/subject/123",
    "categories": {
      "preserved": [],
      "transformed": [],
      "complemented": [],
      "intentionally_unresolved": [],
      "lost": [],
      "deviation_risk": [],
      "next_update_policy": []
    }
  }
}
```

Each array element **SHOULD** be an object with at least:

| Field | Requirement |
| --- | --- |
| `summary` | Human-readable text summarizing the observation. |

Implementations **MAY** add implementation-specific keys inside category items.

### SLS-4.4.1 Empty categories

Empty arrays **MUST** be treated as “no items reported for this category,” not as syntactic errors.

## SLS-4.5 Machine-readable serialization

JSON is used as an **example** serialization. Implementations **MAY** use equivalent logical structures in other formats if they preserve the categories and minimum fields.

## SLS-4.6 Relationship to implementation

Tools **MAY** generate or validate RDE review outputs. Implementations that claim RDE implementation behavior are further constrained by [SLS-5](rde-implementation-specification.md). No tool **MUST** be assumed complete or authoritative over human judgment; see [SLS-1.8](introduction.md).
