# SLS-7 Relationship to audit trails

## SLS-7.1 Purpose

Many deployments maintain **audit trails** (append-only logs, signed journals, or comparable records) distinct from semantic lineage stores. This document specifies abstract expectations for alignment—not concrete log formats.

## SLS-7.2 Correlation

Implementations that maintain both **RDE review outputs** (see [SLS-4](rde-review-output.md)) and **audit trails** **SHOULD** preserve enough metadata to correlate:

- an audit record pertaining to a change or publication action, and  
- the corresponding RDE review output **subject_ref**, when such a review exists.

Exact mapping keys (event IDs, hashes) are **implementation-defined** in Phase 1.

## SLS-7.3 Non-duplication

Audit trails **MAY** embed summaries of RDE categories but **SHOULD NOT** be treated as replacing structured RDE interchange where interoperability is required.

## SLS-7.4 Privacy and retention

Policies for retention, access control, and personally identifiable information are **out of scope** for Phase 1 normative text but **MUST NOT** contradict the obligation that public specification text remains interpretable without private repositories (see [SLS-1.7](introduction.md)).
