# SLS-5 RDE implementation specification

Status: **Normative (Phase 1 implementation profile)** unless a section is explicitly marked informative.

This document specifies implementation obligations for components that claim to implement RDE (**Resonant Deviation Evaluator**) behavior in the Kotonoha / SLS ecosystem. It complements [SLS-4 RDE review output](rde-review-output.md) by defining implementation responsibilities and boundaries.

## SLS-5.1 Purpose

An RDE implementation evaluates meaning-relevant change and emits structured review observations. It does not replace human judgment, authorization, or accountability.

An implementation claiming RDE conformance **MUST** be able to:

- accept a review subject or subject reference;
- inspect source and changed material, when available;
- classify observations using the RDE categories defined in [SLS-4](rde-review-output.md);
- emit or validate an RDE review output record;
- preserve enough traceability for audit correlation;
- distinguish RDE observations from policy enforcement or final approval.

## SLS-5.2 Scope

### SLS-5.2.1 In scope

This document covers:

- RDE implementation responsibilities;
- minimum pipeline stages;
- input and output expectations;
- category classification obligations;
- validation boundaries;
- memory-layer and audit-correlation interactions;
- source-context and partial-context handling;
- human-authority boundaries.

### SLS-5.2.2 Out of scope

This document does not mandate:

- a specific LLM, SLM, rules engine, classifier, or symbolic algorithm;
- model weights or prompts;
- latency, throughput, cost, or hosting topology;
- full JSON Schema for every internal feature;
- replacement of policy engines, safety filters, or human review;
- automatic final determination of complete source meaning for every subject.

## SLS-5.3 Implementation roles

### SLS-5.3.1 RDE evaluator

The RDE evaluator is the component that produces RDE observations. It **MUST** classify output using the category keys defined in [SLS-4.3](rde-review-output.md).

It **MUST NOT** claim final authority to accept, reject, publish, or approve a change.

### SLS-5.3.2 Subject adapter

A subject adapter prepares the item under review for RDE evaluation. Examples include document revisions, patches, pull requests, design decisions, generated text, or lineage units.

An implementation **SHOULD** preserve a stable `subject_ref` when one is available.

A subject adapter **MUST NOT** treat the adapter's extracted representation as the complete semantic subject when materially relevant source context is missing or contested.

### SLS-5.3.3 Context provider

A context provider supplies optional prior material such as previous lineage units, source text, issue context, prior RDE outputs, or audit references.

An implementation **MAY** operate with partial context, but it **SHOULD** surface material uncertainty or missing context in observations when that absence affects review quality.

A context provider **SHOULD** preserve references to human-provided intent statements, issue discussions, prior lineage units, or institutional constraints when they materially shape the source context.

### SLS-5.3.4 Output validator

An output validator checks that emitted RDE review output records satisfy the minimal interchange obligations in [SLS-4.4](rde-review-output.md).

Validation **MUST** distinguish malformed records from records that are well-formed but semantically weak or incomplete.

## SLS-5.4 Minimum pipeline

An RDE implementation **SHOULD** structure processing into the following logical stages. Implementations may combine stages internally, but the observable responsibilities should remain clear.

### SLS-5.4.1 Subject intake

The implementation receives or resolves the subject under review.

Minimum expectation:

- identify `subject_ref`;
- preserve source references when available;
- avoid treating a raw text diff as the complete semantic subject.

### SLS-5.4.2 Context assembly

The implementation gathers relevant prior state and review context.

Context may include:

- prior lineage unit;
- original text or design intent;
- changed text or proposed artifact;
- issue, PR, or decision context;
- prior RDE output;
- audit correlation identifiers.

Context assembly **SHOULD** distinguish supplied source context from inferred source context. When inference is used, the implementation **SHOULD** keep the inferred material inspectable or summarize its uncertainty in the RDE observations.

### SLS-5.4.3 Semantic observation

The implementation observes candidate semantic changes. It **MUST NOT** reduce semantic change to raw lexical difference alone.

Semantic observation may consider:

- intent preservation;
- scope changes;
- uncertainty shifts;
- responsibility shifts;
- value or institutional tension;
- unresolved tension;
- loss of ambiguity or rationale;
- added assumptions;
- drift risk.

### SLS-5.4.4 Category classification

The implementation classifies observations into the RDE categories in [SLS-4.3](rde-review-output.md):

- `preserved`
- `transformed`
- `complemented`
- `intentionally_unresolved`
- `lost`
- `deviation_risk`
- `next_update_policy`

Each category **MAY** be empty, but the implementation **MUST** preserve the category structure in compliant interchange output.

### SLS-5.4.5 Output emission

The implementation emits an RDE review output record conforming to [SLS-4.4](rde-review-output.md).

Each observation item **SHOULD** include a human-readable `summary`. Implementations **MAY** include additional fields, such as evidence references, confidence notes, or source spans, provided the minimum record remains interpretable.

### SLS-5.4.6 Validation and traceability

The implementation validates the output shape and preserves correlation to the subject and any audit trail.

An implementation **SHOULD** record:

- `subject_ref`;
- RDE output identifier, if available;
- related lineage unit identifier, if available;
- source or prior unit reference, if available;
- audit correlation identifier, if maintained.

## SLS-5.5 Input requirements

### SLS-5.5.1 Minimum input

An RDE implementation **MUST** accept at least one review subject or `subject_ref`.

### SLS-5.5.2 Prior state

When prior state is available, the implementation **SHOULD** use it to distinguish preservation, transformation, loss, and drift.

When prior state is not available, the implementation **SHOULD** avoid overstating conclusions and may record missing context as an unresolved or deviation-risk observation.

### SLS-5.5.3 Human-provided context

Human-provided context, such as intent statements or review notes, **MAY** be used as review input. Implementations **SHOULD** preserve traceability to such context when it materially affects observations.

### SLS-5.5.4 Source construction

Source context is constructed from available references; it is not assumed to be complete, singular, or automatically settled.

An RDE implementation **SHOULD** distinguish at least the following situations when materially relevant:

- source context supplied directly by the caller;
- source context resolved from lineage records or repository references;
- source context inferred by the implementation;
- source context missing, partial, or contested.

When source context is inferred, partial, or contested, the implementation **SHOULD** avoid reporting `preserved`, `lost`, or `deviation_risk` as final conclusions without qualification. It may instead record the issue as `intentionally_unresolved`, `deviation_risk`, or a qualified observation item.

## SLS-5.6 Output requirements

### SLS-5.6.1 Required output shape

A compliant implementation **MUST** emit or ingest the logical RDE review output shape defined in [SLS-4.4](rde-review-output.md).

### SLS-5.6.2 Observation item content

Observation items **SHOULD** include concise human-readable summaries.

Observation items **MAY** include:

- evidence references;
- source spans;
- confidence notes;
- reviewer notes;
- source-context status;
- implementation-specific metadata.

Implementation-specific metadata **MUST NOT** be required to understand the minimum RDE categories.

### SLS-5.6.3 Empty categories

Empty categories **MUST** remain valid. They indicate that no material item was reported for that category, not that the category is unsupported.

## SLS-5.7 Memory layer interaction

RDE implementations may use a memory layer, but RDE is not identical to memory.

An implementation **MAY** store or retrieve:

- prior RDE outputs;
- lineage unit references;
- subject references;
- source-context references;
- loss observations;
- audit-correlation identifiers.

The memory layer **MUST NOT** be treated as final human authority. Stored observations remain review records, not approval decisions.

## SLS-5.8 Audit correlation

When an implementation maintains audit trails, it **SHOULD** preserve a correlatable relationship between:

- the review subject;
- the RDE review output;
- related lineage units;
- source-context references, when material;
- human decisions or approvals;
- audit records.

Audit correlation **MUST NOT** imply that RDE output alone authorizes or rejects a decision.

## SLS-5.9 Human authority boundary

An RDE implementation **MUST NOT** present itself as replacing human judgment, approval, rejection, publication responsibility, or institutional accountability.

An RDE implementation **MAY** assist human review by surfacing observations, missing context, loss, or drift risk.

An implementation **SHOULD** support review reopening or contestation by preserving enough traceability for a later reviewer to challenge or revise the recorded observation.

## SLS-5.10 Policy and safety boundary

RDE is distinct from a policy engine or safety filter.

An implementation **MAY** feed RDE observations into a policy workflow, but the RDE output itself is a semantic review record. It is not automatically a refusal, approval, enforcement action, or access-control decision.

## SLS-5.11 Implementation conformance

An implementation claiming conformance to this RDE implementation specification **MUST** document:

- which subjects it can evaluate;
- how it obtains or receives context;
- how it maps observations to RDE categories;
- how it emits or validates RDE review output;
- how it preserves traceability;
- how it distinguishes supplied, resolved, inferred, missing, or contested source context when material;
- which parts are automated and which require human review.

## SLS-5.12 Informative implementation patterns

This section is informative.

Possible RDE implementation patterns include:

- rule-based classifier over structured review notes;
- LLM-assisted evaluator with schema validation;
- hybrid extractor plus human review workflow;
- CI-integrated documentation drift checker;
- pull-request assistant that emits RDE observations without approving the PR.

These patterns are examples only. They do not constrain conforming implementations.