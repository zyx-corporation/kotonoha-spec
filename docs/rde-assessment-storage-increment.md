# RDE assessment storage increment (informative draft)

**Status:** **Informative — future normative candidate.** Not part of Phase 1 bundle **0.1** or Phase 2 validation profile (SLS-9).

**Tracking:** [kotonoha-spec #47](https://github.com/zyx-corporation/kotonoha-spec/issues/47) (parent [#25](https://github.com/zyx-corporation/kotonoha-spec/issues/25)).

---

## 1. Problem statement

Phase 1 normative interchange defines **RDE review output** ([SLS-4](rde-review-output.md)) — a minimal `rde_review_output` record for exchange and validation.

Implementations also persist **assessments bound to semantic lineage** (MeaningDelta, audit correlation, human review). That persistence shape is **not** identical to the interchange document:

| Surface | Role | Normative today |
| --- | --- | --- |
| `rde_documents` + `rde_review_output` | Validated interchange row | SLS-4 / SLS-9 |
| `rde_assessments.payload` (JSONB) | Evaluation bound to a delta | **Implementation** ([`kotonoha-core` M1 DDL](https://github.com/zyx-corporation/kotonoha-core/blob/main/migrations/20260520000000_m1_semantic_lineage.sql)) |

This increment describes **what a future public spec might norm** without changing Phase 1/2 obligations.

---

## 2. Relationship to SLS-4 / SLS-5

- **SLS-4** remains the interchange entry for category arrays and `subject_ref`.
- **SLS-5** implementation responsibilities (traceability, audit correlation) **SHOULD** be satisfiable whether the assessment is stored as interchange-only, JSONB-only, or both with `rde_document_id` FK.
- A normative assessment storage profile **MUST NOT** replace human review authority (SLS-1.8, SLS-5.8).

---

## 3. Proposed storage record (draft)

Logical fields aligned with current PostgreSQL (`rde_assessments` + M2 metadata):

| Field | Required | Notes |
| --- | --- | --- |
| `id` | yes | UUID |
| `meaning_delta_id` | yes | Parent lineage anchor |
| `payload` | yes | JSONB — **MAY** embed `rde_review_output` or a superset evaluation object |
| `payload_schema_version` | SHOULD | e.g. `0.1` when payload contains Phase 1 interchange |
| `source_kind` | SHOULD | Closed set: `cli`, `llm`, `import`, `replay` (implementation today) |
| `validation_report` | MAY | Machine-readable strict/phase2 profile results |
| `audit_correlation_id` | MAY | Aligns with `audit_events.correlation_ref` ([SLS-7](audit-trail-relationship.md)) |
| `rde_document_id` | MAY | FK to validated `rde_documents` when materialized |

**Versioning (SLS-8):** new optional top-level keys in `payload` **MAY** be added in a minor increment; closed enumerations (`source_kind`) **MUST NOT** widen without a documented migration.

---

## 4. Non-goals (this increment)

- Defining MeaningDelta / MeaningState JSON Schema (management M1 concept; core DDL).
- Replacing [SLS-6.5](representation-of-loss.md#sls-65-interim-encoding-phase-3-gate--2026-05) / [#3](https://github.com/zyx-corporation/kotonoha-spec/issues/3) lost-element encoding.
- Wire APIs for gateway, MCP, or web-console ([SLS-9.11](phase2-interchange-hardening.md#sls-911-explicit-non-promotion)).

---

## 5. Escalation path

| Step | Action |
| --- | --- |
| 1 | Stabilize this draft via [#47](https://github.com/zyx-corporation/kotonoha-spec/issues/47) checklist |
| 2 | Optional JSON Schema artifact under `schemas/` (separate from `rde-review-output.phase2.schema.json`) |
| 3 | Promotion per [kotonoha-management `17`](https://github.com/zyx-corporation/kotonoha-management/blob/main/docs/17_spec_escalation_workflow.md) only when implementation and review agree |

**Implementation traceability:** [`kotonoha-core` spec-traceability](https://github.com/zyx-corporation/kotonoha-core/blob/main/docs/spec-traceability.md) — RDEAssessment row.
