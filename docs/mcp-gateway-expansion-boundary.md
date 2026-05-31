# MCP and Gateway Expansion Boundary

## Status

**Informative — expansion integration boundary.** This document defines when and how `kotonoha-mcp` and `kotonoha-gateway` may expand relative to spec, CLI, Obsidian, orchestrator, and web-console boundaries (#163–#165). It does **not** add normative interchange fields.

Parent context: [`current-official-architecture.md`](current-official-architecture.md).  
Implementation mirrors:

- [`kotonoha-mcp` `docs/expansion-boundary.md`](https://github.com/zyx-corporation/kotonoha-mcp/blob/main/docs/expansion-boundary.md)
- [`kotonoha-gateway` `docs/expansion-boundary.md`](https://github.com/zyx-corporation/kotonoha-gateway/blob/main/docs/expansion-boundary.md)

## Purpose

MCP and gateway are **external integration surfaces** for tools such as Cursor, Claude Desktop, ChatGPT Apps, and HTTP clients. They extend interoperability **without** becoming primary Kotonoha interfaces or semantic authorities.

This document prevents:

- Connector convenience redefining RDE categories or sidecar meaning.
- Experimental orchestrator routes exposed as stable integration contracts.
- Silent mutation or autonomous approval through tool calls.
- MCP/gateway bypassing human review and RDE sidecar records.

## Stability tiers (ecosystem)

| Tier | Surfaces | Role |
| --- | --- | --- |
| **Normative interchange** | `kotonoha-spec` — RDE categories, sidecar schemas, identity, handoff | Meaning authority |
| **Stable runtime / adapter** | `kotonoha-cli`; orchestrator `GET /health`, `GET /v1/agents`, `POST /v1/rde/evaluate` | Execution and stable HTTP adapter |
| **Expansion integration** | **`kotonoha-mcp`**, **`kotonoha-gateway`** | External tool bridge — not primary UI |
| **Experimental** | `/v1/proposals/generate`; task/handoff automation; UIB integration; cross-tool autonomous workflows | May change; not stable integration foundation |

See also: [core boundary](core-responsibility-boundary.md), [orchestrator API boundary](orchestrator-api-stability-boundary.md), [web-console priority](web-console-priority-boundary.md).

## Layer model

```text
kotonoha-spec          normative semantics
       ↓
kotonoha-cli / core    first stable runtime
       ↓
orchestrator           stable adapter (+ experimental routes)
       ↓
kotonoha-mcp           MCP tool surface (stdio)
kotonoha-gateway       HTTP tool surface (/v1/tools/{name})
       ↓
external clients       Cursor, Claude Desktop, ChatGPT Apps, custom HTTP
       ↑
obsidian-kotonoha-console   first usable UI (not replaced)
```

**Dependency direction:** spec → CLI/core → stable adapters → MCP/gateway → external clients. Integration layers **follow** upstream contracts; they do not define them.

## Relationship to orchestrator

| Integration | Orchestrator usage |
| --- | --- |
| MCP | **MAY** call stable adapter endpoints when a tool explicitly documents orchestrator delegation; default path is CLI delegation |
| Gateway | **MAY** proxy stable adapter calls in addition to CLI tool mapping; **MUST NOT** collapse experimental and stable API tiers |
| Both | **MUST NOT** expose `/v1/proposals/generate` as a stable public integration contract |

Obsidian `HttpKotonohaClient` may auto-detect orchestrator via `GET /v1/agents`; gateway default port (**8787**) is distinct from orchestrator and web-console API ports — do not assume one backend implies another.

## Relationship to external tools

External clients (Cursor, Claude Desktop, ChatGPT Apps) **MAY** connect via MCP or gateway HTTP. They are **not** Kotonoha primary surfaces.

| Rule | Policy |
| --- | --- |
| Client UX | Client-specific; must not become normative Kotonoha semantics |
| Tool descriptions | Explicit, auditable; cite spec for field meaning |
| Human review | Preserved — external agents do not replace human approve/reject |
| Local-first | Git workdir + CLI delegation remain the default execution path for MCP/gateway tools today |

Product UX guidance may live in `kotonoha-management` (e.g. ChatGPT app docs) — **informative only**, not spec replacement.

## kotonoha-mcp

### Role

MCP server exposing Kotonoha tools that **delegate to `kotonoha` CLI** (see [`kotonoha-mcp` README](https://github.com/zyx-corporation/kotonoha-mcp)). Expansion integration layer — **not** primary interface.

### MCP MAY

- Expose read-only context export (`kotonoha context export`)
- Expose RDE review output retrieval / validation (`kotonoha rde validate`, attach flows)
- Expose proposal / audit / review sidecar lookup via CLI export paths
- Call stable CLI commands documented 1:1 in tool catalog
- Call stable orchestrator adapter endpoints when explicitly documented per tool
- Provide explicit tool descriptions for external clients
- Support human-reviewed handoff workflows (`kotonoha_prepare_human_review`, human-path `review.*` without `--agent-run-id`)

### MCP MUST NOT

- Define Kotonoha semantics (categories, sidecar layout, identity meaning)
- Silently approve, reject, or apply proposals
- Mutate notes or workspace files without explicit user action
- Treat LLM output as accepted lineage
- Depend on experimental proposal generation (`/v1/proposals/generate`) as stable
- Bypass RDE / review sidecar records
- Become the primary Kotonoha interface
- Expose autonomous agent review with `--agent-run-id` on human-path tools
- Spawn arbitrary shell — only documented `kotonoha` binary invocation

### Current tool baseline (informative)

Existing MCP tools map to CLI subcommands — see [`kotonoha-mcp` README](https://github.com/zyx-corporation/kotonoha-mcp#mcp-tools). New tools require expansion checklist (below) and contract doc update.

## kotonoha-gateway

### Role

HTTP gateway exposing the **same tool names** as MCP via `POST /v1/tools/{name}`. Expansion integration layer — **not** semantic authority.

Default: `http://127.0.0.1:8787`. See [`kotonoha-gateway` README](https://github.com/zyx-corporation/kotonoha-gateway).

### Gateway MAY

- Proxy stable adapter calls (when documented)
- Expose authenticated tool endpoints (`KOTONOHA_GATEWAY_API_KEYS`, principal/project mapping)
- Perform request validation and rate limiting
- Normalize external tool calls to MCP-equivalent tool names
- Enforce project / principal identity boundary on CLI child env
- Mediate access to CLI / core / orchestrator stable surfaces
- Emit structured audit log lines for tool invocation

### Gateway MUST NOT

- Become normative schema source
- Redefine RDE categories or sidecar meaning
- Own project identity semantics (spec + CLI resolution authoritative)
- Silently mutate storage or Git state
- Collapse experimental and stable orchestrator APIs into one undifferentiated surface
- Make external HTTP clients the primary authority for review decisions
- Bypass human review requirements
- Expose `/v1/proposals/generate` as stable gateway tool without experimental labeling

### Current HTTP baseline (informative)

| Method | Path | Tier |
| --- | --- | --- |
| `GET` | `/health` | Stable integration |
| `GET` | `/v1/tools` | Stable integration |
| `POST` | `/v1/tools/{name}` | Stable integration (per-tool CLI mapping) |
| `GET` | `/openapi.yaml` | Documentation |

Tool semantics follow MCP catalog; not a separate normative protocol.

## Expansion prerequisites

MCP or gateway expansion **MAY proceed** for a new tool or route only when **all** apply:

1. **Schemas:** Relevant interchange shapes are defined in `kotonoha-spec`, or explicitly marked experimental with no normative claim.
2. **Stable upstream:** Backing behaviour exists in CLI or orchestrator **stable adapter** tier — not experimental-only routes.
3. **Identity:** Project / principal behaviour is explicit (`KOTONOHA_PRINCIPAL_ID`, gateway key mapping, RBAC).
4. **Mutation:** Write paths are human-reviewed or explicitly read-only; no silent apply.
5. **RDE / review:** Sidecar handling preserved — audit and review records not bypassed.
6. **Auditability:** External client / tool invocation is logged and traceable.
7. **Boundary docs:** This document and repo mirror updated if tier or MUST NOT rules change.

**Timing:** MCP/gateway feature work **MAY continue** on existing M5/M6 tool sets without waiting for web-console write paths. **New semantic surfaces** wait for spec clarification.

## Required upstream contracts (checklist)

Before promoting an integration feature from experimental to stable:

| Upstream | Required for |
| --- | --- |
| `kotonoha-spec` SLS-4 / sidecar docs | RDE display, validate, attach tools |
| `kotonoha-spec` identity / handoff | Agent run, review, export tools |
| `kotonoha-cli` `cli-definition.md` | Every MCP/gateway tool delegation |
| [CLI version policy](https://github.com/zyx-corporation/kotonoha-docs/blob/main/ja/manual/cli_version_policy.md) | Minimum CLI for tools |
| [Orchestrator stable adapter](orchestrator-api-stability-boundary.md) | Optional orchestrator-backed tools |
| [`kotonoha-mcp` contract](https://github.com/zyx-corporation/kotonoha-mcp/blob/main/docs/mcp-server-contract.md) | Human review path, no shell |
| [`kotonoha-gateway` contract](https://github.com/zyx-corporation/kotonoha-gateway/blob/main/docs/gateway-contract.md) | HTTP auth, audit log |

## RDE drift checklist (external integrations)

Before merging MCP/gateway changes, verify:

1. **No new category meaning** introduced in tool descriptions or response shaping.
2. **No silent apply** — user or human-review widget explicit.
3. **No experimental route** documented as stable for external clients.
4. **Sidecar path preserved** — proposal → audit → review loop intact where applicable.
5. **Engine labeling** — orchestrator vs local vs LLM output distinguished in UX copy.
6. **Not primary UI** — integration docs state Obsidian/CLI primacy.
7. **CLI-only spawn** — no arbitrary shell execution.

## Decision checklist (new tool or route)

1. Does it define new **semantic** fields? → `kotonoha-spec` issue first.
2. Is backing API **experimental** only? → label experimental; do not add to stable catalog without tier upgrade.
3. Does it **write** data? → human review or read-only justification required.
4. Does it bypass **RDE / review** sidecars? → reject.
5. Could an external client treat this as **primary** Kotonoha? → reject or defer.
6. Is there a **1:1 CLI mapping** or documented stable adapter call? → required for merge.

## Non-goals (current phase)

- MCP as primary Kotonoha interface.
- Gateway as semantic authority or identity source.
- Connector-specific normative schemas before spec stabilization.
- Stable public exposure of `/v1/proposals/generate`.
- Autonomous cross-tool workflows without human review gates.

## RDE Check

### Preserved Elements

Kotonoha remains spec-first, human-reviewed, and RDE-auditable. MCP and gateway extend interoperability without becoming sources of semantic truth. Obsidian first UI and CLI first runtime unchanged.

### Authorized Transformations

External integration is allowed as an expansion layer after core, orchestrator, and web-console boundaries are defined (#163–#165).

### Inferred Extensions

Stable/experimental tiering applied to external surfaces prevents connector convenience from redefining Kotonoha semantics.

### Unresolved Elements

- Exact MCP tool list for post-M6 expansion.
- Gateway authentication and authorization policy details.
- Project/principal authorization matrix across multi-tenant deployment.
- External client UX policy (Cursor vs ChatGPT Apps).
- Audit log retention and replay policy for gateway.

### Drift Risks

- MCP becoming the primary interface too early.
- Gateway becoming a semantic authority.
- External clients treating experimental proposal generation as stable.
- Silent mutation through tool calls.
- Bypassing human review and RDE sidecars.

### Next Revision Policy

Revise after the first concrete post-boundary MCP tool set or gateway API surface is proposed, or when orchestrator experimental routes are promoted.

## Related documents

| Document | Role |
| --- | --- |
| [orchestrator-api-stability-boundary.md](orchestrator-api-stability-boundary.md) | Stable vs experimental HTTP |
| [web-console-priority-boundary.md](web-console-priority-boundary.md) | Non-primary web UI |
| [core-responsibility-boundary.md](core-responsibility-boundary.md) | Core vs integration split |
| [kotonoha-mcp README](https://github.com/zyx-corporation/kotonoha-mcp) | Current tool catalog |
| [kotonoha-gateway README](https://github.com/zyx-corporation/kotonoha-gateway) | HTTP tool API |

## Change policy

Updates follow [`git_operation_rules.md`](git_operation_rules.md): issue → branch → PR → merge to `main`. Link `kotonoha-management` #166 when revising.

Governance: [kotonoha-management #166](https://github.com/zyx-corporation/kotonoha-management/issues/166)
