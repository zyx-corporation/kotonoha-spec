# kotonoha-web-console Priority Boundary

## Status

**Informative — expansion-layer boundary.** This document defines the current priority and non-primary UI status of `kotonoha-web-console`. It does **not** promote the web console to primary UI or add normative interchange fields.

Parent context: [`current-official-architecture.md`](current-official-architecture.md), [`orchestrator-api-stability-boundary.md`](orchestrator-api-stability-boundary.md).  
Implementation mirror: [`kotonoha-web-console` `docs/priority-boundary.md`](https://github.com/zyx-corporation/kotonoha-web-console/blob/main/docs/priority-boundary.md).

## Purpose

`kotonoha-web-console` is a **Web UI candidate** in the expansion layer. It must not compete with:

- **`kotonoha-spec`** — normative source
- **`kotonoha-cli`** — first stable runtime
- **`obsidian-kotonoha-console`** — first usable UI (dogfood complete)

This document fixes the convergence-phase priority: web-console **MAY** grow as read-only / demo surfaces; it **MUST NOT** become the primary authoring or semantic authority surface yet.

## Tier classification

| Property | Value |
| --- | --- |
| Layer | **Expansion layer / non-primary UI** |
| Primary UI | `obsidian-kotonoha-console` |
| Stable runtime | `kotonoha-cli` (+ `kotonoha-core`) |
| Normative source | `kotonoha-spec` |
| Web-console role | Read-oriented dashboards, result viewers, team-mode demo — **not** first usable UI |

## Priority order (current phase)

1. Stabilize `kotonoha-spec`.
2. Stabilize `kotonoha-cli` (see [CLI version policy](https://github.com/zyx-corporation/kotonoha-docs/blob/main/ja/manual/cli_version_policy.md)).
3. Dogfood `obsidian-kotonoha-console` (sign-off complete).
4. Keep `kotonoha-vscode` thin.
5. **Defer web-console primary workflows** until #166 MCP/gateway timing is resolved and write-capable boundaries are revised.

Web-console implementation work **MAY proceed** for low-risk read-only and demo flows without changing this priority statement.

## Responsibility split

```text
kotonoha-spec          RDE semantics, sidecar meaning, identity contracts
       ↓
kotonoha-orchestrator  HTTP envelopes (stable adapter + experimental tiers)
       ↓
kotonoha-web-console   UI display, read-only dashboards, demo flows
       ↑
obsidian-kotonoha-console   first usable UI (authoring, review, apply — not replaced)
kotonoha-cli                first stable runtime (writes, export — delegated)
```

| Concern | Owner |
| --- | --- |
| RDE category semantics | **`kotonoha-spec`** |
| HTTP request/response envelope | **`kotonoha-orchestrator`** (until spec annex) |
| UI rendering and navigation | **`kotonoha-web-console`** |
| Authoring + human apply loop | **`obsidian-kotonoha-console`** |
| Durable writes / export commands | **`kotonoha-cli`** |

## What web-console MAY do (current phase)

| Allowed workflow | Notes |
| --- | --- |
| Display project / principal status | Read from CLI-backed API or DB views; do not redefine identity |
| Display RDE review outputs | Render spec-shaped records; cite spec for category meaning |
| Display proposal / audit / review records | Read-only sidecar viewers aligned with `kotonoha-spec` |
| Read-only dashboards | Team mode, delta lists, export previews (M7 scope) |
| Demo / documentation flows | Onboarding, architecture explanation, non-production demos |
| Call **stable adapter** orchestrator endpoints | e.g. `GET /health`, `GET /v1/agents`, `POST /v1/rde/evaluate` per [orchestrator API boundary](orchestrator-api-stability-boundary.md) |
| Delegate writes to CLI | Pattern established in M4 boundary verification — UI read-only, CLI writes |

## What web-console MUST NOT do (current phase)

| Forbidden | Reason |
| --- | --- |
| Become the **first usable UI** | Reserved for Obsidian dogfood path |
| Replace Obsidian authoring workflows | Notes, apply, metadataWriteMode, git-aware review |
| Define normative schemas | Belongs in `kotonoha-spec` |
| Define RDE category semantics | Belongs in `kotonoha-spec` |
| Depend on `POST /v1/proposals/generate` as **stable** | [Experimental tier](orchestrator-api-stability-boundary.md) — not a web-console UX foundation |
| Introduce web-console-only sidecar formats | All interchange shapes from spec |
| Silently mutate project records | Writes via explicit CLI / human action only |
| Become source of truth for project / principal identity | Identity contracts in spec; CLI/core authoritative for runtime |
| Claim equivalence to full RDE evaluation | Display only; conservative/local/engine paths are not normative verdicts |

## Orchestrator usage rules

| Endpoint tier | Web-console policy |
| --- | --- |
| Stable adapter (`/v1/rde/evaluate`, `/health`, `/v1/agents`) | **MAY** use for dashboards and RDE display |
| Experimental (`/v1/proposals/generate`, task/handoff UI) | **MAY NOT** treat as stable UX prerequisite; demo-only with uncertainty labels |
| Internal / ops | Ops tooling only; not end-user web-console core |

When showing orchestrator RDE output, web-console **MUST** label engine source and avoid presenting UI copy as spec normative text.

## Relationship to gateway and MCP (unresolved — #166)

| Integration | Current stance |
| --- | --- |
| `kotonoha-gateway` | Separate port/deployment; web-console default API (8790) must not assume gateway semantics as canonical |
| `kotonoha-mcp` | Expansion timing deferred; web-console must not embed MCP-only context models as Kotonoha interchange |

Revise this section when [#166](https://github.com/zyx-corporation/kotonoha-management/issues/166) MCP/gateway boundary is documented.

## When web-console work may proceed

| Work type | Gate |
| --- | --- |
| Read-only dashboards, viewers, i18n, demo | **Allowed now** — must comply with this boundary |
| Write-capable web workflows | Requires explicit boundary revision + Obsidian/CLI parity review |
| Normative schema or RDE meaning changes | **`kotonoha-spec` issue first** — never web-console-led |
| Production primary UI promotion | **Out of scope** for current convergence phase |

Dependency statement: web-console **depends on** spec/CLI/Obsidian stabilization for **semantic** correctness; it does **not** block those tracks.

## Allowed initial workflows (M7 reference)

Aligned with current M7 Team Mode scaffold ([`kotonoha-web-console`](https://github.com/zyx-corporation/kotonoha-web-console)):

- Project list (read-only)
- MeaningDelta list with principal RBAC
- M6 export via CLI delegation
- i18n (`en` / `ja`) display shells

These are **implementation/audit surfaces**, not primary UI replacement. See [`m4-boundary-verification.md`](https://github.com/zyx-corporation/kotonoha-web-console/blob/main/docs/m4-boundary-verification.md).

## Expansion criteria (future)

Web-console **MAY** expand beyond read-only when **all** apply:

1. [#166](https://github.com/zyx-corporation/kotonoha-management/issues/166) MCP/gateway timing documented.
2. Write paths mirror CLI contracts with explicit human confirmation.
3. No web-console-only semantic model introduced.
4. This document revised with RDE check and management issue link.

## Decision checklist (web-console changes)

1. Does this make web-console feel like the **primary** UI? → reject or defer.
2. Does it define new field **meaning**? → `kotonoha-spec` first.
3. Does it require **experimental** orchestrator routes as stable? → reject.
4. Does it write data without CLI/spec path? → reject for current phase.
5. Does Obsidian dogfood already cover the human-review loop? → web-console should complement, not duplicate as authority.

## Non-goals (current phase)

- Promote web console to primary UI.
- Duplicate Obsidian dogfood apply/re-audit/metadataWriteMode flows prematurely.
- New sidecar schemas outside `kotonoha-spec`.
- Web-only project identity authority.
- Autonomous AI authoring agent in the browser.

## RDE Check

### Preserved Elements

Kotonoha remains spec-first. Obsidian remains the first usable UI. CLI remains the first stable runtime. Context portability and semantic audit architecture unchanged.

### Authorized Transformations

Web-console is positioned as a **later expansion surface** (read-only / demo first) rather than rejected entirely.

### Inferred Extensions

Read-only dashboards and result viewers are allowed as low-risk early web-console use cases.

### Unresolved Elements

- Exact web-console write workflows (post read-only phase).
- Authentication model.
- Deployment model (single-tenant vs team hosting).
- Relationship to gateway (#166).
- Relationship to MCP (#166).

### Drift Risks

- Web-console becoming primary UI too early.
- Experimental proposal generation (`/v1/proposals/generate`) treated as stable UX.
- UI convenience copy redefining RDE semantics.
- Web-console-specific sidecar variants outside spec.

### Next Revision Policy

Revise after #166 MCP/gateway timing is resolved, or when web-console moves from read-only/demo to write-capable workflows.

## Related documents

| Document | Role |
| --- | --- |
| [current-official-architecture.md](current-official-architecture.md) | Multi-repo roles |
| [orchestrator-api-stability-boundary.md](orchestrator-api-stability-boundary.md) | Stable vs experimental API |
| [core-responsibility-boundary.md](core-responsibility-boundary.md) | Core vs UI split |
| [obsidian dogfood acceptance](https://github.com/zyx-corporation/obsidian-kotonoha-console/blob/main/docs/dogfood-acceptance.md) | First usable UI criteria |

## Change policy

Updates follow [`git_operation_rules.md`](git_operation_rules.md): issue → branch → PR → merge to `main`. Link `kotonoha-management` #165 when revising.

Governance: [kotonoha-management #165](https://github.com/zyx-corporation/kotonoha-management/issues/165)
