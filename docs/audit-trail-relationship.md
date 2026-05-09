# Relationship to audit trails

## Purpose

Many deployments maintain **audit trails** (append-only logs, signed journals, or comparable records) distinct from semantic lineage stores. This document specifies abstract expectations for alignment—not concrete log formats.

## Correlation

Implementations that maintain both **RDE review outputs** (see [rde-review-output.md](rde-review-output.md)) and **audit trails** **SHOULD** preserve enough metadata to correlate:

- an audit record pertaining to a change or publication action, and  
- the corresponding RDE review output **subject_ref**, when such a review exists.

Exact mapping keys (event IDs, hashes) are **implementation-defined** in Phase 1.

## Non-duplication

Audit trails **MAY** embed summaries of RDE categories but **SHOULD NOT** be treated as replacing structured RDE interchange where interoperability is required.

## Privacy and retention

Policies for retention, access control, and personally identifiable information are **out of scope** for Phase 1 normative text but **MUST NOT** contradict the obligation that public specification text remains interpretable without private repositories (see [introduction.md](introduction.md)).
