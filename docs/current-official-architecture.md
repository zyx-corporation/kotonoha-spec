# Kotonoha Current Official Architecture

## Status

Kotonoha is currently organized as a multi-repository system composed of a normative specification, shared runtime components, a CLI, and thin UI adapters.

This document defines the current official architecture and responsibility boundaries.

## Repository Roles

| Repository | Role | Status |
| --- | --- | --- |
| `kotonoha-spec` | Normative source for file formats, sidecars, identity, handoff, and RDE audit contracts | Canonical |
| `kotonoha-core` | Shared implementation logic | Runtime core |
| `kotonoha-cli` | First stable runtime interface | Primary execution layer |
| `obsidian-kotonoha-console` | First usable UI for context, review, and RDE audit | Dogfood target |
| `kotonoha-vscode` | Thin Developer Console over CLI/Core | Secondary UI adapter |
| `kotonoha-docs` | Human-facing documentation | Public guidance |
| `kotonoha-management` | Planning and issue governance | Project control |
| `kotonoha-orchestrator` | RDE/orchestration backend | Expansion layer |
| `kotonoha-mcp` | MCP integration surface | Expansion layer |
| `kotonoha-gateway` | Gateway / integration boundary | Expansion layer |
| `kotonoha-web-console` | Web UI candidate | Expansion layer |

## Architectural Principles

### 1. Spec is normative

`kotonoha-spec` is the canonical source for:

- frontmatter contract
- sidecar layout
- proposal record
- review record
- RDE audit record
- handoff bundle
- project identity
- principal identity
- interoperability requirements

Runtime behavior must conform to the spec, not redefine it.

### 2. CLI is the first stable runtime

`kotonoha-cli` is the primary execution surface for stable workflows.

UI adapters should call or conform to CLI/Core behavior rather than reimplementing incompatible workflows.

### 3. Obsidian is the first usable UI

`obsidian-kotonoha-console` is the first dogfood UI for:

- active note review
- proposal creation
- RDE audit
- human approval / rejection
- sidecar inspection
- metadata write policy

It is not a replacement for the CLI.

### 4. VSCode is a thin Developer Console

`kotonoha-vscode` should expose developer workflows over CLI/Core.

It should not become an autonomous AI coding agent.

It may support:

- project/principal context
- CLI status
- context export
- RDE/audit command invocation
- Git diff capture
- development handoff

It should not implement:

- independent AI agent loops
- autonomous code modification
- separate canonical context model
- undocumented sidecar formats

### 5. Expansion layers come after stabilization

The following should remain secondary until spec, CLI, and Obsidian dogfood are stable:

- orchestrator
- gateway
- MCP server
- web console

## Priority Order

1. Stabilize `kotonoha-spec`.
2. Stabilize `kotonoha-cli`.
3. Dogfood `obsidian-kotonoha-console`.
4. Keep `kotonoha-vscode` thin.
5. Publish the current official architecture in `kotonoha-docs`.
6. Expand orchestrator / MCP / gateway / web console only after the above contracts are stable.

## Non-goals for the current phase

- Kotonoha VSCode is not an AI coding agent.
- Obsidian Kotonoha Console is not responsible for replacing the CLI.
- The CLI is not the normative specification.
- The orchestrator is not the canonical spec.
- The web console is not the primary UI yet.
- MCP integration should not redefine Kotonoha semantics.

## RDE Check

### Preserved Elements

Kotonoha remains a system for context portability, semantic audit, RDE-based review, and human acceptance of meaning changes.

### Authorized Transformations

The original single-surface idea is transformed into a layered architecture: spec, core, CLI, and UI adapters.

### Inferred Extensions

The responsibility split introduces an explicit stability order and prevents UI surfaces from becoming competing sources of truth.

### Unresolved Elements

*(None for convergence child issues #163–#166.)*

Resolved (informative): [`kotonoha-core` boundary](core-responsibility-boundary.md) (#163), [orchestrator API stability](orchestrator-api-stability-boundary.md) (#164), [web-console priority](web-console-priority-boundary.md) (#165), [MCP/gateway expansion](mcp-gateway-expansion-boundary.md) (#166).

### Drift Risks

- UI proliferation may obscure the canonical semantics.
- VSCode may drift into an AI coding agent.
- CLI behavior may become de facto spec unless checked against `kotonoha-spec`.
- Orchestrator may introduce backend-specific semantics too early.

### Next Revision Policy

Revise this document whenever a repository changes its architectural responsibility or when a stable contract is promoted from implementation to specification.

## CLI runtime compatibility (informative)

This section is operational guidance, not normative spec.

| Scope | Version |
| --- | --- |
| Recommended | **v0.3.1** |
| Minimum (standalone CLI) | v0.3.0 |
| Minimum (Obsidian CLI backend, VSCode) | v0.3.1 |

Human-facing detail and update policy: [`kotonoha-docs` `ja/manual/cli_version_policy.md`](https://github.com/zyx-corporation/kotonoha-docs/blob/main/ja/manual/cli_version_policy.md).

The CLI is the first stable runtime. It must conform to `kotonoha-spec`; its release tag does not define interchange semantics.

## Related documents

| Document | Role |
| --- | --- |
| [core-responsibility-boundary.md](core-responsibility-boundary.md) | `kotonoha-core` vs spec / CLI / UI / orchestrator (informative) |
| [orchestrator-api-stability-boundary.md](orchestrator-api-stability-boundary.md) | Orchestrator API tiers and schema ownership (informative) |
| [web-console-priority-boundary.md](web-console-priority-boundary.md) | Web console non-primary UI priority (informative) |
| [mcp-gateway-expansion-boundary.md](mcp-gateway-expansion-boundary.md) | MCP/gateway expansion timing (informative) |
| [kotonoha-docs CLI version policy](https://github.com/zyx-corporation/kotonoha-docs/blob/main/ja/manual/cli_version_policy.md) | Recommended/minimum CLI (informative) |
| [repository-governance.md](repository-governance.md) | Ecosystem roles (informative) |
| [architecture.md](architecture.md) | SLS-2 normative architecture |
| [rde-review-output.md](rde-review-output.md) | RDE output contract |

## Change policy

Updates follow [`git_operation_rules.md`](git_operation_rules.md): issue → branch → PR → merge to `main`.
