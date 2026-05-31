# Orchestrator API Stability Boundary

## Status

**Informative — expansion-layer boundary.** This document defines API stability tiers and schema ownership for `kotonoha-orchestrator`. It does **not** make the orchestrator normative. RDE category semantics remain in [`rde-review-output.md`](rde-review-output.md).

Parent context: [`current-official-architecture.md`](current-official-architecture.md), [`core-responsibility-boundary.md`](core-responsibility-boundary.md).  
Implementation mirror: [`kotonoha-orchestrator` `orchestrator/docs/api-stability-boundary.md`](https://github.com/zyx-corporation/kotonoha-orchestrator/blob/main/orchestrator/docs/api-stability-boundary.md).

## Purpose

The orchestrator is an **expansion-layer backend** for multi-agent supervision, RDE evaluation services, and optional LLM proxying. UI adapters (especially `obsidian-kotonoha-console`) may call it over HTTP but **must not treat orchestrator convenience as canonical Kotonoha semantics**.

This document answers four convergence questions:

1. Is `POST /v1/rde/evaluate` stable or experimental?
2. What does `POST /v1/proposals/generate` guarantee as an LLM proxy?
3. Where is the canonical source for request/response schemas?
4. How do local fallback and orchestrator results relate?

## Stability tiers

| Tier | Meaning | Change policy |
| --- | --- | --- |
| **Normative interchange** | Field meaning and RDE categories | `kotonoha-spec` only |
| **Stable adapter contract** | HTTP routes and envelopes required by shipped UI adapters | Breaking changes require adapter issue + migration note |
| **Experimental** | Convenience endpoints, optional flags, internal engine heuristics | May change without spec promotion |
| **Internal / ops** | Daemon subprocesses, audit tail, watcher | Orchestrator repo only |

**Rule:** Stable adapter contract ≠ normative spec. Promotion to `kotonoha-spec` is a separate governance step ([management spec escalation workflow](https://github.com/zyx-corporation/kotonoha-management/blob/main/docs/17_spec_escalation_workflow.md)).

## Endpoint inventory

| Method | Path | Tier | Primary consumer | Notes |
| --- | --- | --- | --- | --- |
| GET | `/health` | Stable adapter | All HTTP clients | Liveness probe |
| GET | `/v1/agents` | Stable adapter | Backend auto-detect | Presence implies orchestrator mode |
| POST | `/v1/rde/evaluate` | **Stable adapter** (RDE path) | Obsidian Console, orchestrator UI | See § RDE evaluate |
| POST | `/v1/proposals/generate` | **Experimental** | Obsidian Console | See § Proposals generate |
| GET | `/v1/tasks` | Experimental | Orchestrator console | Task board |
| GET | `/v1/tasks/{id}` | Experimental | Orchestrator console | |
| POST | `/v1/tasks/{id}/transition` | Experimental | Orchestrator console | |
| GET | `/v1/handoffs` | Experimental | Orchestrator console | |
| POST | `/v1/handoffs` | Experimental | Orchestrator console / daemon bridge | |
| POST | `/v1/context/build` | Experimental | Orchestrator console | |
| GET | `/v1/escalations` | Experimental | Orchestrator console | |
| GET | `/v1/audit/tail` | Internal / ops | Ops / console | |
| POST | `/v1/audit/verify` | Internal / ops | Ops | |
| POST | `/v1/watcher/check` | Internal / ops | Ops | |
| POST | `/v1/rde/attach` | Experimental | Orchestrator attach flow | Delegates to CLI/core |
| POST | `/v1/uib/evaluate` | Experimental | UIB hooks | |
| POST | `/v1/uib/check-escalate` | Experimental | UIB hooks | |

Gateway-only routes (different repo) such as `/v1/tools/kotonoha_context_export` are out of scope here.

## Focus: `POST /v1/rde/evaluate`

### Classification

**Stable adapter contract** for the Obsidian dogfood path (v0.2.13+).  
**Not normative spec** — the HTTP envelope is owned by `kotonoha-orchestrator` until promoted.

| Aspect | Stability | Owner |
| --- | --- | --- |
| Route path `/v1/rde/evaluate` | Stable adapter | orchestrator |
| Request fields `subject_ref`, `meaning_changes` | Stable adapter | orchestrator (documented here + orchestrator mirror) |
| Optional flags `strict`, `phase2_strict` | Experimental tuning | orchestrator |
| Response wrapper `{ "rde_review_output": … }` | Stable adapter | orchestrator |
| **`rde_review_output` semantics** | Normative interchange | **`kotonoha-spec`** (SLS-4, Phase 2 schema) |
| Engine implementation (`RDEEvaluator`) | Experimental internal | orchestrator `rde-engine` |

### Request (stable adapter minimum)

```json
{
  "subject_ref": "obsidian://notes/example.md#abc123",
  "meaning_changes": {
    "preserved": ["…"],
    "transformed": ["…"],
    "complemented": ["…"],
    "unresolved": ["…"],
    "deviation_risk": ["…"]
  }
}
```

Optional: `strict`, `phase2_strict` — when true, output should validate against [`rde-review-output.phase2.schema.json`](https://github.com/zyx-corporation/kotonoha-spec/blob/main/schemas/rde-review-output.phase2.schema.json).

### Response (stable adapter minimum)

```json
{
  "rde_review_output": {
    "spec_version": "0.1",
    "subject_ref": "…",
    "categories": { }
  }
}
```

**Semantic authority:** category keys and meanings are defined in `kotonoha-spec`, not by orchestrator README prose.

### What stable does NOT mean

- Orchestrator evaluation is **not claimed** to equal full human RDE review.
- Engine heuristics may change while keeping the same HTTP envelope.
- Strict validation failure is an orchestrator error (HTTP 500), not a normative review decision.

## Focus: `POST /v1/proposals/generate`

### Classification

**Experimental — best-effort LLM proxy** for Obsidian generative operations (`summarize`, `rewrite`, `expand`, `custom`).

Added after orchestrator-spec v0.3 API table; treat as **adapter convenience**, not a Kotonoha interchange contract.

### Guaranteed behaviour (minimum)

| Guarantee | Detail |
| --- | --- |
| Accepts Obsidian Console generate body | `operation`, `instruction`, `language`, `context` (see orchestrator `ProposalGenerateBody`) |
| Returns proposal text | `{ "proposal": { "proposedText", "summary?", "uncertaintyNote?" }, "audit"?: … }` |
| Rejects `rde_audit` | HTTP 400 — must use `/v1/rde/evaluate` |
| Local fallback | When `OPENAI_API_KEY` unset or LLM call fails → rule-based draft with `uncertaintyNote` |
| Dummy key path | `sk-dummy-*` → deterministic mock text, no external LLM |

### Not guaranteed

| Non-guarantee | Detail |
| --- | --- |
| Model vendor / model id stability | Controlled by env (`OPENAI_MODEL`, etc.) |
| Semantic quality or fidelity to source | Best-effort generative rewrite only |
| Equivalence to CLI `context export` + local proposal | Different code path |
| RDE audit in response | `audit` field optional; Obsidian re-audits via `/v1/rde/evaluate` when omitted |
| Normative proposal record shape | Not a `kotonoha-spec` sidecar contract |
| Availability | Obsidian must survive 404 / missing route (local fallback) |

**UI rule:** Surface `uncertaintyNote` and do not label LLM proxy output as validated RDE.

## Schema ownership

| Artifact | Canonical owner | Notes |
| --- | --- | --- |
| `rde_review_output` JSON | **`kotonoha-spec`** | SLS-4 + Phase 2 schema |
| RDE audit sidecar (Obsidian) | **`kotonoha-spec`** (+ Obsidian policy docs) | Written after local merge |
| `/v1/rde/evaluate` HTTP request/response envelope | **`kotonoha-orchestrator`** | Stable adapter tier; mirror doc + OpenAPI-like tables in orchestrator repo |
| `/v1/proposals/generate` request/response | **`kotonoha-orchestrator`** | Experimental; no spec promotion yet |
| `agent.v0.1`, `task.v0.1`, `handoff.v0.1` | **`kotonoha-orchestrator`** `schemas/` | Orchestration domain, not Kotonoha interchange |
| CLI command contracts | **`kotonoha-cli`** | e.g. `rde emit`, `rde validate` |
| Shared validation logic | **`kotonoha-core`** | Implements spec rules |

**Promotion rule:** When an orchestrator HTTP envelope carries interchange semantics relied on by multiple adapters, add an **informative annex** in `kotonoha-spec` (like CLI version policy). Until then, orchestrator docs are authoritative for HTTP shape; spec is authoritative for meaning.

## Local fallback vs orchestrator result

### Obsidian HTTP client behaviour (reference)

Documented behaviour in `obsidian-kotonoha-console` `HttpKotonohaClient`:

| Step | Orchestrator available | Orchestrator unavailable / error |
| --- | --- | --- |
| Generate (`summarize`, etc.) | `POST /v1/proposals/generate` → optional LLM; 404 → local anchors | Local rule-based draft + uncertainty note |
| RDE audit / re-audit | `POST /v1/rde/evaluate` → normalize to CLI emit shape → **merge with local guardrails** | **Local rule-based audit only** (`engine: "local"`) |
| `rde_audit` operation | Direct `/v1/rde/evaluate` | Local source review path |

### Equivalence policy

| Statement | Policy |
| --- | --- |
| Local audit vs orchestrator audit | **Not equivalent.** UI must record engine source (`orchestrator` vs `local`). |
| Orchestrator output alone | **Insufficient** for Obsidian sidecar — merged through local guardrails (`performRdeAudit`, structural diff merge). |
| User-facing claims | Must not say “full RDE evaluation” for local rule-based or conservative engine paths. |
| Dogfood requirement | Orchestrator optional; local fallback must remain functional. |

### Merge model (informative)

```text
structural diff (local) ──┐
                          ├──► performRdeAudit + merge ──► RdeAudit sidecar
orchestrator evaluate ────┘      (guardrails always apply)
```

Orchestrator supplies SLS-4-shaped categories; Obsidian applies adapter policy (decision recommendation, drift risks, human review prompts).

## Error model (adapter expectations)

| Condition | Expected adapter behaviour |
| --- | --- |
| `/health` fails | Treat backend unavailable; use mock/cli/local paths |
| `/v1/rde/evaluate` 5xx / network error | Fall back to local rule-based audit; notify user |
| `/v1/proposals/generate` 404 | Orchestrator without LLM proxy → local draft (Obsidian) |
| `/v1/proposals/generate` 502 | LLM failure → orchestrator may return local draft; adapter may surface error |
| Invalid `rde_audit` on generate route | 400 — adapter must route to evaluate |

## Versioning policy

1. **URL prefix `/v1/`** — breaking HTTP changes require `/v2/` or explicit adapter migration period.
2. **Stable adapter fields** — additive JSON fields preferred; removing or renaming stable fields requires changelog + adapter bump.
3. **Experimental endpoints** — may change in minor orchestrator releases without spec update.
4. **Normative interchange** — versioned via `spec_version` / `kotonoha-spec` SLS-8, not orchestrator release tag.

## Non-goals

- Make orchestrator the normative semantic source.
- Require orchestrator for Obsidian dogfood or CLI workflows.
- Remove local fallback in UI adapters.
- Define LLM prompt templates as Kotonoha spec text.

## Decision checklist (orchestrator API changes)

1. Does the change alter **RDE category meaning**? → `kotonoha-spec` first.
2. Does it break **Obsidian stable adapter** fields? → adapter issue + migration note.
3. Is it **experimental** only? → document in orchestrator mirror; no spec PR required.
4. Will multiple adapters depend on the HTTP shape? → plan spec informative annex.
5. Does UI still work **without** orchestrator? → mandatory for merge.

## RDE Check

### Preserved Elements

Kotonoha remains spec-driven. RDE review output meaning stays in `kotonoha-spec`. Human review authority and sidecar contracts are unchanged. Local-first and fallback paths remain valid.

### Authorized Transformations

The orchestrator is explicitly labeled an expansion layer with tiered stability: stable HTTP adapter surface for `/v1/rde/evaluate`, experimental LLM proxy for `/v1/proposals/generate`, normative semantics only in spec.

### Inferred Extensions

Obsidian merge model documents that orchestrator output is input to adapter guardrails, not a final audit verdict.

### Unresolved Elements

- Informative spec annex for `/v1/rde/evaluate` envelope (post dogfood stabilization).
- Whether `meaning_changes` key set becomes spec-defined interchange vs orchestrator bridge only.
- Gateway vs orchestrator unified backend detection long term.

### Drift Risks

- Treating experimental LLM proxy output as audited Kotonoha proposal records.
- Orchestrator engine heuristics documented as if they were SLS-4 normative rules.
- Skipping local guardrails when orchestrator returns 200.
- Backend-specific category keys leaking before spec acceptance.

### Next Revision Policy

Revise when orchestrator endpoints change tier, when spec annex is promoted, or when a second UI adapter depends on the same HTTP contract.

## Related documents

| Document | Role |
| --- | --- |
| [core-responsibility-boundary.md](core-responsibility-boundary.md) | Core vs orchestrator split |
| [current-official-architecture.md](current-official-architecture.md) | Expansion layer priority |
| [rde-review-output.md](rde-review-output.md) | Normative RDE output |
| [orchestrator-specification-v0.3](https://github.com/zyx-corporation/kotonoha-orchestrator/blob/main/orchestrator/docs/orchestrator-specification-v0.3.md) | FR-5 and v0.1 API target |
| [obsidian cli-runtime-compatibility](https://github.com/zyx-corporation/obsidian-kotonoha-console/blob/main/docs/cli-runtime-compatibility.md) | Obsidian backend modes |

## Change policy

Updates follow [`git_operation_rules.md`](git_operation_rules.md): issue → branch → PR → merge to `main`. Link `kotonoha-management` #164 when revising.

Governance: [kotonoha-management #164](https://github.com/zyx-corporation/kotonoha-management/issues/164)
