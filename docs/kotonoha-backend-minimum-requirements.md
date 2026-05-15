# Kotonoha backend minimum requirements draft

Status: **Informative draft**. This document is not a Phase 1 normative specification. It records candidate requirements for a future Kotonoha backend specification derived from the architecture terminology currently documented in `kotonoha-docs`.

If this document conflicts with Phase 1 normative documents, the Phase 1 normative document prevails.

## 1. Purpose

Kotonoha backend is the system layer that receives external events, normalizes them into meaning-bearing records, relates them to semantic lineage and review context, and produces auditable recommendations without replacing human decision authority.

The backend is not merely a classifier, summarizer, or autonomous agent. Its core responsibility is to make meaning-relevant change inspectable and reviewable.

## 2. Scope

This draft covers the following backend concepts:

- MeaningEvent
- Event Normalizer
- Meaning Event Bus
- Meaning Extractor
- Relation Resolver
- Delta-M Detector
- RDE Evaluator
- Policy Boundary
- Action Proposal
- Human Review
- Feedback Loop
- relation_store
- lineage_store
- audit_log

This draft does not define complete JSON Schema, wire protocols, authentication, tenancy, deployment topology, model choices, or storage engine requirements.

## 3. Requirement language

The terms MUST, MUST NOT, SHOULD, SHOULD NOT, and MAY are used in this draft as **candidate requirement language** for future normative work. They are not yet Phase 1 conformance obligations.

## 4. Architectural boundary

The backend SHOULD maintain the following responsibility separation:

1. RDE evaluation observes semantic change and deviation risk.
2. Policy Boundary decides whether an operation is allowed, blocked, escalated, or requires review.
3. Action Proposal prepares possible actions but does not itself imply execution authority.
4. Human Review remains the accountable decision point for strong or irreversible operations.

A conforming future backend profile SHOULD NOT collapse these responsibilities into a single opaque agent decision.

## 5. Component requirements

### 5.1 MeaningEvent

A `MeaningEvent` is the common internal representation of an event that may carry meaning-relevant change.

Candidate requirements:

- A MeaningEvent MUST preserve a traceable source reference.
- A MeaningEvent MUST include at least an event identifier, source type, source reference, actor or origin, timestamp or ordering reference, and payload reference or payload summary.
- A MeaningEvent SHOULD distinguish observed source content from inferred interpretation.
- A MeaningEvent SHOULD be serializable for audit and replay.
- A MeaningEvent MUST NOT present inferred context as source fact.

Deferred work:

- Formal JSON Schema.
- Required identifier format.
- Canonical timestamp and ordering semantics.

### 5.2 Event Normalizer

The Event Normalizer converts external inputs into MeaningEvents.

Candidate requirements:

- The Event Normalizer MUST preserve enough source metadata to trace the normalized event back to its origin.
- The Event Normalizer SHOULD support email, chat, calendar, issue, document, and sensor-derived event classes without requiring all implementations to support all classes.
- The Event Normalizer MUST NOT perform final RDE or Policy Boundary decisions.
- The Event Normalizer SHOULD mark lossy normalization when source content is summarized, truncated, filtered, or otherwise reduced.

### 5.3 Meaning Event Bus

The Meaning Event Bus connects backend components without requiring point-to-point coupling.

Candidate requirements:

- The bus SHOULD support dispatch of MeaningEvents and derived observations.
- The bus SHOULD preserve correlation identifiers across component outputs.
- The bus MAY be implemented as an in-process event dispatcher, message queue, IPC layer, or service bus.
- The bus MUST NOT erase provenance or correlation metadata required for audit.

### 5.4 Meaning Extractor

The Meaning Extractor identifies intent, requests, topics, stance, unresolved points, and other meaning-relevant features.

Candidate requirements:

- The Meaning Extractor SHOULD output structured observations rather than only prose summaries.
- The Meaning Extractor SHOULD label uncertainty or unresolved elements when present.
- The Meaning Extractor MUST distinguish source-derived observations from inferred extensions.
- The Meaning Extractor MUST NOT authorize actions.

### 5.5 Relation Resolver

The Relation Resolver associates events with actors, projects, documents, organizations, or prior relation history.

Candidate requirements:

- The Relation Resolver SHOULD output relation identifiers or relation candidates.
- The Relation Resolver SHOULD preserve confidence or ambiguity when multiple relations may apply.
- The Relation Resolver MUST NOT silently merge distinct actors, projects, or contexts without traceable justification.
- The Relation Resolver SHOULD integrate with relation_store when available.

### 5.6 Delta-M Detector

The Delta-M Detector identifies meaning-relevant change between prior and current states.

Candidate requirements:

- The Delta-M Detector SHOULD compare current observations against prior meaning state when available.
- The Delta-M Detector SHOULD separate textual change from semantic change.
- The Delta-M Detector SHOULD identify preserved, changed, weakened, omitted, unresolved, or newly introduced elements.
- The Delta-M Detector MUST pass change observations to RDE evaluation rather than treating detection as final judgment.

Deferred work:

- Formal Delta-M representation.
- Threshold semantics for similarity, drift, loss, or risk.

### 5.7 RDE Evaluator

The RDE Evaluator evaluates semantic deviation using RDE categories.

Candidate requirements:

- The RDE Evaluator MUST keep semantic evaluation separate from execution authorization.
- The RDE Evaluator SHOULD use or map to the RDE categories defined in Phase 1 RDE documents.
- The RDE Evaluator SHOULD record preserved, transformed, inferred, unresolved, suspicious, and critical elements when applicable.
- The RDE Evaluator MUST NOT be treated as final human approval.
- The RDE Evaluator SHOULD emit structured output suitable for audit correlation.

### 5.8 Policy Boundary

The Policy Boundary determines whether proposed operations are allowed, blocked, escalated, or require human review.

Candidate requirements:

- The Policy Boundary MUST distinguish evaluation results from authorization results.
- The Policy Boundary SHOULD consider RDE output, operation type, authority level, risk, and user or organization policy.
- The Policy Boundary MUST require human review for strong or irreversible operations unless a future normative profile explicitly allows automation.
- The Policy Boundary SHOULD support at least `allow`, `require_review`, `block`, and `escalate` outcomes.

Strong operations include but are not limited to external posting, message sending, deletion, calendar finalization, permission changes, and contractual or financial commitments.

### 5.9 Action Proposal

Action Proposal converts evaluated context into candidate next actions.

Candidate requirements:

- Action Proposal MUST represent proposed actions as proposals, not implicit execution commands.
- Action Proposal SHOULD include rationale, required authority, source references, and unresolved points.
- Action Proposal SHOULD be reviewable before execution.
- Action Proposal MUST NOT execute strong operations without Policy Boundary approval and, where required, Human Review.

### 5.10 Human Review

Human Review is the accountable decision layer for proposed actions and significant semantic changes.

Candidate requirements:

- Human Review SHOULD support approve, reject, edit, and defer outcomes.
- Human Review SHOULD record reviewer identity or accountable role when available.
- Human Review SHOULD preserve the rationale for material decisions.
- Human Review MUST NOT be replaced by automated RDE output for accountable decisions.

### 5.11 Feedback Loop

The Feedback Loop returns human decisions and corrections to stores and future evaluation context.

Candidate requirements:

- Feedback Loop SHOULD record human correction of classification, relation, risk, or action proposal.
- Feedback Loop SHOULD update relation_store, lineage_store, or audit_log as appropriate.
- Feedback Loop MUST distinguish user correction from observed source content.

## 6. Store requirements

### 6.1 relation_store

The relation_store preserves relation context across time.

Candidate requirements:

- relation_store SHOULD track actors, organizations, projects, documents, or contexts as relation entities.
- relation_store SHOULD record relation state changes with time or order references.
- relation_store MAY track trust, stability, context affinity, or similar implementation-defined measures.
- relation_store MUST NOT treat inferred relation state as immutable fact.

### 6.2 lineage_store

The lineage_store preserves semantic lineage across MeaningEvents and derived meaning states.

Candidate requirements:

- lineage_store SHOULD preserve relationships between source events, meaning states, Delta-M observations, and RDE outputs.
- lineage_store SHOULD support later inspection of what was preserved, transformed, complemented, left unresolved, or lost.
- lineage_store MUST keep enough references to correlate with Phase 1 lineage units when applicable.

### 6.3 audit_log

The audit_log records the reviewable history of evaluations, proposals, policy outcomes, and human decisions.

Candidate requirements:

- audit_log SHOULD record RDE outputs, Policy Boundary decisions, Action Proposals, Human Review outcomes, and relevant references.
- audit_log SHOULD preserve correlation identifiers across the backend flow.
- audit_log MUST NOT be overwritten in a way that destroys accountability for prior decisions.

## 7. Initial backend flow

A minimal backend flow SHOULD follow this pattern:

```text
External input
  -> Event Normalizer
  -> MeaningEvent
  -> Meaning Event Bus
  -> Meaning Extractor / Relation Resolver / Delta-M Detector
  -> RDE Evaluator
  -> Policy Boundary
  -> Action Proposal
  -> Human Review when required
  -> relation_store / lineage_store / audit_log
```

Implementations MAY reorder or combine internal steps if they preserve the same responsibility boundaries and audit correlations.

## 8. Out of scope

This draft does not define:

- Complete schema definitions.
- API endpoints.
- Wire protocols.
- Authentication or authorization protocol details.
- Storage engine choices.
- UI requirements.
- Model selection or model evaluation benchmarks.
- Specific integration requirements for email, Slack, calendar, GitHub, Obsidian, or sensors.

## 9. Future normative split

Future work SHOULD split this draft into smaller specification documents:

1. `meaning-event-schema.md`
2. `rde-evaluation-result-schema.md`
3. `action-proposal-schema.md`
4. `policy-boundary-requirements.md`
5. `human-review-feedback-flow.md`
6. `backend-store-logical-model.md`

These documents may become normative only after schema stability, terminology alignment, and conformance language are reviewed.

## 10. RDE-oriented review notes

### Preserved elements

- Kotonoha remains a semantic lineage and review system, not an autonomous agent framework.
- RDE observes semantic change but does not authorize execution.
- Human accountability remains separate from automated evaluation.

### Authorized transformations

- Conceptual terms from `kotonoha-docs` are translated into candidate requirements.
- Backend terms are placed in `kotonoha-spec` as an informative draft rather than direct normative obligations.

### Inferred extensions

- Policy Boundary, Action Proposal, and backend stores are given initial candidate requirement language.

### Unresolved gaps

- Formal schemas remain undefined.
- Conformance levels remain undefined.
- Integration-specific requirements remain deferred.

### Drift risks

- Implementers may read this informative draft as normative.
- Action Proposal may be mistaken for autonomous execution authority.
- RDE may be misread as a policy or safety filter.

### Next update policy

The next update should define the minimal MeaningEvent schema and the minimal RDE evaluation result schema.

## 11. Related

- Issue: #27
- Phase 1 RDE output: [rde-review-output.md](rde-review-output.md)
- Phase 1 audit relationship: [audit-trail-relationship.md](audit-trail-relationship.md)
- System overview: [requirements-overview.md](requirements-overview.md)
