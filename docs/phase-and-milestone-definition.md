# Phase and milestone definition

Status: **Informative — roadmap and implementation-alignment guidance**.

This document defines how this repository uses **Phase** and **Milestone** language so readers do not confuse specification maturity, implementation delivery, and partner-repository project management.

Normative requirements remain in the SLS documents under `docs/` unless a future pull request explicitly promotes part of this roadmap into normative text.

## 1. Terminology

### 1.1 Phase

A **Phase** is a specification maturity stage. It describes which public specification surface is stable enough for external interpretation, implementation, or review.

A Phase is not a release tag, crate version, CLI version, database migration number, or GitHub project milestone by itself.

### 1.2 Milestone

A **Milestone** is an implementation, validation, documentation, or governance checkpoint. Milestones may be completed in partner repositories such as `kotonoha-core`, `kotonoha-cli`, `kotonoha-docs`, or management repositories.

A Milestone may support a Phase, but completing a Milestone does not automatically create new normative specification requirements.

### 1.3 Gate

A **Gate** is a decision point where deferred work is either:

- kept informative;
- implemented in tooling without changing normative prose;
- promoted into normative specification text;
- split into a child issue or future specification document.

## 2. Phase definitions

### 2.1 Phase 1 — Public specification MVP

**Purpose:** establish the minimal public, reviewable specification surface for the Semantic Lineage System (SLS).

**Normative anchor:** SLS-1 through SLS-8 in this repository.

**Bundle:** `spec_version` / bundle `0.1`.

**Scope:**

- institutional framing and definitions;
- semantic lineage as distinct from raw logs or diffs;
- minimal lineage unit obligations;
- RDE review categories and minimal interchange record;
- explicit representation of lost semantic elements;
- audit correlation boundaries;
- human accountability boundaries;
- minimal implementation conformance levels;
- Phase 1 version compatibility rules.

**Non-goals:**

- full JSON Schema for every interchange field;
- wire protocols;
- storage-engine obligations;
- authentication, tenancy, scalability, retention, or threat-model requirements;
- final claims that automated RDE output authorizes, rejects, or replaces human judgment.

**Exit criteria:**

- SLS-1 to SLS-8 exist as public Phase 1 normative documents;
- minimal RDE review output is externally understandable;
- Phase 1 conformance can be stated without private repositories;
- intentionally deferred work is tracked as incremental or future normative work.

### 2.2 Phase 2 — Interchange and schema hardening

**Purpose:** turn the Phase 1 minimum into stricter validation and typed interchange artifacts without breaking Phase 1 compatibility.

**Scope candidates:**

- JSON Schema baseline for minimal RDE review output;
- validation of closed vocabularies such as `source_context_status`;
- strict envelope validation in implementation tooling;
- typed optional lineage properties;
- implementation profile alignment for `minimal`, `standard`, and `full` conformance;
- conformance tests for public interchange artifacts.

**Non-goals:**

- broad product UI specification;
- final storage topology;
- network protocol commitments;
- promotion of implementation-only envelopes into normative SLS interchange unless explicitly reviewed.

**Exit criteria:**

- schema and validator behavior agree for Phase 1 records;
- unknown fields, closed enums, and version compatibility are handled consistently;
- CLI and core validation paths report failures in a way traceable to `kotonoha-spec` sections.

### 2.3 Phase 3 — Rich semantic modeling beyond minimal RDE categories

**Purpose:** model semantic structures that Phase 1 intentionally represents only through minimal RDE categories.

**Scope candidates:**

- dedicated representation for lost semantic elements beyond the RDE `lost` category;
- standalone identifiers or payloads for loss records when justified;
- richer responsibility-bearing context modeling;
- stronger correlation among loss, audit, lineage, source context, and human decisions;
- console or agent ingest paths that delegate to existing validation while avoiding normative drift.

**Non-goals:**

- treating every implementation transport wrapper as normative;
- assuming all semantic loss can be exhaustively modeled;
- replacing human or institutional review.

**Exit criteria:**

- deferred loss modeling has a public normative proposal or is explicitly kept informative;
- interim loss encoding via [SLS-6.5](representation-of-loss.md#sls-65-interim-encoding-phase-3-gate--2026-05) is published; fuller modeling is tracked under [issue #3](https://github.com/zyx-corporation/kotonoha-spec/issues/3) (child of rollup [issue #25](https://github.com/zyx-corporation/kotonoha-spec/issues/25));
- richer semantic objects are versioned and correlated with Phase 1 lineage and RDE outputs;
- implementation-side Phase 3 wrappers do not silently redefine SLS normative interchange.

### 2.4 Phase 4 — Cross-repository consolidation and reliability governance

**Purpose:** consolidate specification, implementation, documentation, and governance once Phase 1 through Phase 3 surfaces have stabilized.

**Scope candidates:**

- cross-repository alignment among `kotonoha-spec`, `kotonoha-core`, `kotonoha-cli`, `kotonoha-docs`, and related implementation repositories;
- public governance for normative adoption from issues and pull requests;
- external implementer readiness;
- reliability requirements such as authentication, tenancy, scalability, retention, threat modeling, and operational audit profiles;
- conformance test suites or certification-style checks, if needed.

**Non-goals:**

- retroactively making internal management plans normative;
- requiring private repositories for public conformance;
- conflating product roadmap commitments with specification obligations.

**Exit criteria:**

- external implementers can read public specs and implement conformance without private context;
- implementation behavior has stable traceability to public sections;
- remaining future work is tracked as public issues, future SLS documents, or explicitly non-normative implementation guidance.

## 3. Milestone families

The following milestones are descriptive checkpoints. They are not normative requirements unless referenced from a normative SLS section.

| Milestone | Family | Meaning | Typical repository |
| --- | --- | --- | --- |
| M1 | Workspace and lineage foundation | Git workspace, project context, database-backed MeaningDelta and review-decision primitives | `kotonoha-cli`, `kotonoha-core` |
| M2 | RDE metadata and export | RDE assessment metadata, validation report persistence, export formats | `kotonoha-cli`, `kotonoha-core` |
| M3 | Console / event ingest preparation | Transport wrappers, console-equivalent events, or minimal editor UI that delegate to existing RDE/interchange validation | `kotonoha-cli`, `kotonoha-vscode` (IDE-focused; team web console is **M7** in [`kotonoha-web-console`](https://github.com/zyx-corporation/kotonoha-web-console)) |
| M3.5 | Normative backlog linkage | Public rollup for themes that may become normative later — [issue #25](https://github.com/zyx-corporation/kotonoha-spec/issues/25) | `kotonoha-spec` |
| M4 | External tool correlation | GitHub Issue/PR correlation and related audit references | `kotonoha-cli`, `kotonoha-core` |
| M5 | AgentRun gateway | Agent context, capability checks, MCP/gateway routes, agent-scoped MeaningDelta | `kotonoha-cli`, `kotonoha-core`, `kotonoha-mcp`, `kotonoha-gateway` |
| M6 | Team / principal mode | principal, project, and role-based operational scoping | `kotonoha-cli`, `kotonoha-core`, gateway repos |
| M7 | Team-mode UI | web-console or editor surfaces for project-scoped viewing and export, normally delegating writes or exports to CLI/core paths | `kotonoha-web-console`, `kotonoha-vscode` |

Milestone numbers are implementation-roadmap labels. They MUST NOT be used as substitutes for SLS section identifiers when claiming specification conformance.

## 4. Relationship between Phases and Milestones

| Specification Phase | Main concern | Supporting milestones | Normative status |
| --- | --- | --- | --- |
| Phase 1 | Public MVP, minimal reviewable SLS surface | M1/M2 may exercise parts of it | Normative in SLS-1 to SLS-8 |
| Phase 2 | Interchange and schema hardening | M2 and validator work | Normative only when promoted into `kotonoha-spec` |
| Phase 3 | Rich semantic modeling and transport wrappers | M3/M3.5 and loss-modeling work | Mixed; wrappers remain non-normative unless promoted |
| Phase 4 | Consolidation and reliability governance | M4/M5/M6/M7 and public governance work | Future; not yet normative unless explicitly added |

## 5. Current implementation alignment check

This section records the alignment check performed when this document was introduced and subsequently extended to wrapper/UI repositories.

### 5.1 `kotonoha-core`

Observed alignment:

- `kotonoha-core` targets `kotonoha-spec` bundle `0.1` for interchange validation.
- `kotonoha-core` RDE validation checks the seven Phase 1 category keys.
- `kotonoha-core` rejects unknown RDE category keys.
- `kotonoha-core` enforces `spec_version == 0.1` for Phase 1 RDE validation.
- `kotonoha-core` permits implementation-specific keys inside category items, consistent with SLS-4.
- `kotonoha-core` now validates `source_context_status` as the Phase 1 closed vocabulary when that field is present.

Resolved discrepancy:

- Before this alignment update, `kotonoha-core` did not validate the newly fixed `source_context_status` closed vocabulary. The validator has been updated so non-string or unknown values fail validation.

Remaining note:

- `kotonoha-core` strict envelope behavior is an implementation hardening layer. It does not by itself widen Phase 1 normative requirements unless the corresponding text is promoted in `kotonoha-spec`.

### 5.2 `kotonoha-cli`

Observed alignment:

- `kotonoha-cli` delegates RDE and interchange validation to `kotonoha-core`.
- Its public CLI definition treats `kotonoha-spec` as the higher authority when conflicts arise.
- It documents Phase 2 minimum behavior and Phase 3 ingest wrappers as implementation behavior rather than new SLS normative prose.
- Its `kotonoha_core` dependency has been bumped to a revision that includes `source_context_status` closed-vocabulary validation.

Remaining note:

- CLI milestone labels such as M1, M2, M4, M5, and M6 are implementation roadmap labels. They should not be read as SLS specification phases.

### 5.3 `kotonoha-mcp`

Observed alignment:

- `kotonoha-mcp` delegates tool execution to the official `kotonoha` CLI and does not execute arbitrary shell commands.
- `kotonoha_rde_validate` delegates to `kotonoha rde validate --strict`, so Phase 1 RDE validation remains centralized in CLI/core.
- Human review tools call `kotonoha review approve|hold|reject` on the human path only, without `--agent-run-id`.
- The human review path clears `KOTONOHA_AGENT_RUN_ID` from child process environment.
- The README now describes the management UX contract as implementation guidance, not as replacement normative SLS prose.
- The README minimum CLI version has been raised to `kotonoha` 0.2.9+ so wrapper validation includes the current Phase 1 `source_context_status` closed-vocabulary behavior.

Resolved discrepancy:

- `docs/mcp-server-contract.md` previously described all review MCP tools as forbidden. The contract has been corrected: autonomous review with agent context remains forbidden, while human-path review tools are allowed only without agent context.

Remaining note:

- MCP resources such as `ui://kotonoha/rde-summary` and structured content payloads are UI/tooling surfaces. They do not redefine SLS normative interchange unless promoted in `kotonoha-spec`.

### 5.4 `kotonoha-gateway`

Observed alignment:

- `kotonoha-gateway` exposes an HTTP surface over the same tool names as `kotonoha-mcp` and delegates to the official `kotonoha` CLI.
- `docs/gateway-contract.md` restricts process spawning to the gateway CLI delegation module and forbids arbitrary shell, direct `git`, direct `gh`, and autonomous review with agent context.
- Gateway environment variables map API keys to principals/projects and pass `KOTONOHA_PRINCIPAL_ID` / `KOTONOHA_PROJECT_ID` to child CLI processes for M6 behavior.
- The README now describes the management UX contract as implementation guidance, not as replacement normative SLS prose.
- The README minimum CLI version has been raised to `kotonoha` 0.2.9+ so gateway validation includes the current Phase 1 `source_context_status` closed-vocabulary behavior.

Remaining note:

- Gateway OpenAPI routes and tool wrappers are implementation transport surfaces. They do not define new SLS normative wire protocol unless promoted in `kotonoha-spec`.

### 5.5 `kotonoha-vscode`

Observed alignment:

- `kotonoha-vscode` is an editor UI over MeaningDelta, RDE assessment, and human review workflows.
- It delegates operations to the configured `kotonoha` CLI through a single CLI helper path.
- Its environment mapping passes `DATABASE_URL`, `KOTONOHA_DECIDED_BY`, `KOTONOHA_PRINCIPAL_ID`, and `KOTONOHA_PROJECT_ID` to child CLI processes when configured.
- The README minimum CLI requirement has been raised to `kotonoha` 0.2.9+ and now states that current Phase 1 RDE validation requires a core revision with `source_context_status` validation.

Remaining note:

- VS Code UI panels, keybindings, and wireframes are implementation UX. They do not create additional SLS normative obligations.

### 5.6 `kotonoha-web-console`

Observed alignment:

- `kotonoha-web-console` is currently documented as an M7 Team Mode minimal web console and read-only scaffold.
- The server reads project and delta information from the database and delegates M6 export to the `kotonoha` CLI.
- Project-scoped endpoints require `KOTONOHA_PRINCIPAL_ID` and perform project visibility checks before listing deltas or exporting M6 data.
- The README already requires `kotonoha` CLI 0.2.9+.

Remaining note:

- Direct database reads in the web console are implementation behavior for read-only project views. They do not define SLS normative storage requirements unless promoted in `kotonoha-spec`.

### 5.7 Known non-differences

The following are intentional differences, not specification conflicts:

- `kotonoha.interchange.v1` is a core-supported implementation envelope, not a Phase 1 normative SLS interchange replacement.
- `kotonoha.console_event.v0` is a Phase 3-style ingest wrapper in CLI documentation, not normative `kotonoha-spec` prose.
- MCP tools, HTTP gateway routes, VS Code panels, web-console APIs, database migrations, PostgreSQL tables, GitHub correlation tables, AgentRun tables, and review-decision storage are implementation artifacts unless later promoted to normative specification text.

## 6. Maintenance rule

When a future pull request changes any of the following, this document SHOULD be reviewed:

- a new SLS specification Phase is introduced;
- milestone language is added to a public spec document;
- implementation-only envelopes are proposed for normative promotion;
- conformance levels change;
- `kotonoha-core` or `kotonoha-cli` changes observable validation behavior that claims alignment with `kotonoha-spec`;
- `kotonoha-mcp`, `kotonoha-gateway`, `kotonoha-vscode`, or `kotonoha-web-console` changes tool execution, review authority, principal/project scoping, or validation delegation behavior.
