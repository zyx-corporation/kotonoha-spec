# MeaningEvent schema draft

Status: **Informative draft**. This document is a future normative candidate derived from [kotonoha-backend-minimum-requirements.md](kotonoha-backend-minimum-requirements.md). It does not create Phase 1 conformance obligations.

If this document conflicts with Phase 1 normative documents, the Phase 1 normative document prevails.

## 1. Purpose

`MeaningEvent` is the minimal internal event record used by Kotonoha backend components to represent an external or internal event that may carry meaning-relevant change.

A `MeaningEvent` is not itself an RDE evaluation, a policy decision, an action proposal, or a human approval record. It is the traceable input unit from which those later records may be derived.

## 2. Design goals

A `MeaningEvent` should support the following goals:

1. Preserve provenance from external sources.
2. Separate observed source facts from inferred interpretation.
3. Provide stable correlation identifiers for downstream review, RDE, policy, and audit records.
4. Support audit and replay without requiring all implementations to store full raw content.
5. Remain small enough for early implementations.

## 3. Candidate requirement language

The terms MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY are used as candidate requirement language for future normative work. They are not yet Phase 1 conformance obligations.

## 4. Minimal object shape

A `MeaningEvent` candidate object SHOULD have the following top-level shape:

```json
{
  "event_id": "mevt_01HZY...",
  "schema_version": "0.1-draft",
  "source": {
    "type": "mail",
    "ref": "gmail:message:abc123",
    "uri": null
  },
  "actor": {
    "kind": "person",
    "id": "contact_123",
    "display_name": "Example Sender"
  },
  "occurred_at": "2026-05-15T18:30:00Z",
  "observed": {
    "summary": "Sender requested confirmation of the revised delivery date.",
    "content_ref": "sha256:...",
    "language": "en"
  },
  "context": {
    "thread_id": "thread_456",
    "relation_id": null,
    "project_id": null
  },
  "interpretation": {
    "intent_candidates": ["confirmation_request"],
    "unresolved_points": ["delivery date has not been verified"],
    "confidence": 0.72
  },
  "correlation": {
    "lineage_unit_id": null,
    "trace_id": "trace_789",
    "parent_event_ids": []
  },
  "normalization": {
    "normalizer": "example-normalizer/0.1",
    "lossy": true,
    "loss_notes": ["HTML signature removed"]
  }
}
```

This shape is illustrative. Future normative versions may publish a formal JSON Schema.

## 5. Field definitions

| Field | Status | Type | Description |
| --- | --- | --- | --- |
| `event_id` | Required | string | Stable identifier for the event record. |
| `schema_version` | Required | string | Schema or draft version used to interpret the record. |
| `source` | Required | object | Source type and reference for provenance. |
| `actor` | Required | object or null | Actor or origin associated with the event, when available. |
| `occurred_at` | Recommended | string or null | Timestamp from the source or ordering layer. |
| `observed` | Required | object | Source-derived content summary or reference. |
| `context` | Recommended | object | Thread, relation, project, document, or task context. |
| `interpretation` | Optional | object | Inferred meaning candidates. Must remain separate from `observed`. |
| `correlation` | Recommended | object | IDs used to correlate with lineage, RDE, policy, and audit records. |
| `normalization` | Recommended | object | Metadata about the normalizer and any lossy transformation. |

## 6. Source object

The `source` object identifies where the event came from.

Candidate requirements:

- `source.type` MUST identify the source class, such as `mail`, `chat`, `calendar`, `issue`, `document`, `sensor`, `manual`, or `system`.
- `source.ref` MUST preserve a traceable source reference when the source system provides one.
- `source.uri` MAY be used when a durable URI can be safely exposed.
- Implementations MUST NOT require public URLs for private or sensitive sources.

Example:

```json
{
  "type": "chat",
  "ref": "slack:C123:T456:1680000000.000000",
  "uri": null
}
```

## 7. Actor object

The `actor` object identifies the person, organization, system, device, or process associated with the event.

Candidate requirements:

- `actor.kind` SHOULD use values such as `person`, `organization`, `system`, `device`, `agent`, or `unknown`.
- `actor.id` SHOULD be stable within an implementation when available.
- `actor.display_name` MAY be included for review readability.
- Implementations MUST preserve ambiguity when the actor is uncertain.

## 8. Observed content

The `observed` object contains source-derived content or references.

Candidate requirements:

- `observed.summary` SHOULD summarize the observed event without adding interpretation.
- `observed.content_ref` SHOULD point to a content hash, storage reference, or redacted source record when full raw content is not embedded.
- `observed.language` MAY record a BCP 47 language tag or implementation-defined language marker.
- Implementations MUST NOT place inferred intent, risk, or policy judgment in `observed`.

## 9. Interpretation object

The `interpretation` object contains inferred meaning candidates derived from the observed event.

Candidate requirements:

- Interpretation MUST remain separate from observed source facts.
- `intent_candidates` MAY contain zero or more inferred intent labels.
- `unresolved_points` SHOULD record important uncertainty or missing information.
- `confidence` MAY be included, but implementations SHOULD avoid false precision.
- Interpretation MUST NOT be treated as RDE evaluation, policy authorization, or human approval.

## 10. Context object

The `context` object links the event to surrounding work or relationship context.

Candidate requirements:

- `thread_id` MAY connect events within a conversation or workflow thread.
- `relation_id` MAY connect the event to relation_store.
- `project_id`, `document_id`, `task_id`, or similar fields MAY be added by implementations.
- Context resolution SHOULD preserve uncertainty when multiple contexts are plausible.

## 11. Correlation object

The `correlation` object allows downstream records to point back to the event.

Candidate requirements:

- `trace_id` SHOULD connect event normalization, extraction, RDE evaluation, policy evaluation, and audit entries.
- `lineage_unit_id` MAY connect to a Phase 1 lineage unit.
- `parent_event_ids` MAY represent derived or aggregated events.
- Implementations SHOULD preserve correlation IDs across Meaning Extractor, RDE Evaluator, Policy Boundary, Action Proposal, and audit_log records.

## 12. Normalization metadata

The `normalization` object records how the event was produced.

Candidate requirements:

- `normalizer` SHOULD identify the component or version that created the MeaningEvent.
- `lossy` SHOULD be true when the normalized event omits, summarizes, redacts, or filters source material.
- `loss_notes` SHOULD describe known omissions or reductions when `lossy` is true.
- Implementations MUST NOT hide lossy normalization when it is known.

## 13. Minimal required fields

A future minimal normative schema SHOULD consider the following fields required:

```text
event_id
schema_version
source.type
source.ref
observed.summary or observed.content_ref
```

The following fields are strongly recommended but may remain optional in early profiles:

```text
actor
occurred_at
context
correlation.trace_id
normalization
```

## 14. Non-goals

`MeaningEvent` does not define:

- RDE category output.
- Policy authorization result.
- Action execution command.
- Human review decision.
- Storage engine format.
- Complete source connector schemas.
- Privacy or retention policy.

## 15. Validation expectations

A validator for a future MeaningEvent schema SHOULD check:

1. Required identifiers exist.
2. Source reference exists.
3. Observed source content and inferred interpretation are separated.
4. Correlation identifiers are well-formed when present.
5. Lossy normalization is marked when declared by the normalizer.
6. The record does not include action authorization as if it were part of the event.

## 16. RDE-oriented review notes

### Preserved elements

- MeaningEvent remains a traceable input unit.
- Observed facts and inferred interpretation remain separated.
- Downstream RDE, Policy, Action, and Human Review are kept outside the event object.

### Authorized transformations

- The conceptual `MeaningEvent` definition is converted into candidate schema language.
- Required and recommended fields are separated.

### Inferred extensions

- `normalization.lossy` and `loss_notes` are introduced to protect against hidden source compression.
- `correlation.trace_id` is introduced as a cross-component audit hook.

### Unresolved gaps

- Formal JSON Schema remains undefined.
- Canonical ID format remains undefined.
- Privacy, retention, and redaction rules remain deferred.

### Drift risks

- Implementations may overfill `interpretation` and treat it as fact.
- Implementations may treat a MeaningEvent as an authorization record.
- Lossy normalization may be hidden by polished summaries.

### Next update policy

The next update should define the minimal RDE evaluation result schema and its relationship to `MeaningEvent.event_id` and `correlation.trace_id`.

## 17. Related

- Issue: #29
- Backend requirements draft: [kotonoha-backend-minimum-requirements.md](kotonoha-backend-minimum-requirements.md)
- RDE review output: [rde-review-output.md](rde-review-output.md)
- Audit relationship: [audit-trail-relationship.md](audit-trail-relationship.md)
