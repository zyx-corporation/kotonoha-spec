# Phase and milestone definition（日本語版）

Status: **Informative — roadmap and implementation-alignment guidance の日本語 companion**。

この文書は、このリポジトリで使う **Phase** と **Milestone** という語を整理し、仕様成熟度、実装到達点、関連リポジトリのプロジェクト管理が混同されないようにするための説明文書である。

規範的要求は、将来のpull requestでこのロードマップの一部が明示的にnormative textへ昇格されない限り、`docs/` 以下のSLS文書に置かれる。

## 1. 用語

### 1.1 Phase

**Phase** は、仕様の成熟段階である。外部の読者、実装者、レビュー担当者が、どの公開仕様面を安定したものとして解釈できるかを示す。

Phaseは、それ自体ではrelease tag、crate version、CLI version、database migration number、GitHub project milestoneではない。

### 1.2 Milestone

**Milestone** は、実装、検証、文書化、またはガバナンス上の到達点である。Milestoneは、`kotonoha-core`、`kotonoha-cli`、`kotonoha-docs`、management repositoryなどの関連リポジトリで完了されることがある。

MilestoneはPhaseを支えることがあるが、Milestoneが完了しても、それだけで新しいnormative specification requirementが発生するわけではない。

### 1.3 Gate

**Gate** は、後送された作業をどう扱うかを判断する地点である。Gateでは、対象の作業が次のいずれかに分類される。

- informativeのまま維持する。
- normative proseを変更せず、tooling上で実装する。
- normative specification textへ昇格する。
- child issueまたは将来のspecification documentへ分割する。

## 2. Phase定義

### 2.1 Phase 1 — Public specification MVP

**目的:** Semantic Lineage System（SLS）の最小限の公開・再審可能な仕様面を確立する。

**Normative anchor:** このリポジトリのSLS-1からSLS-8。

**Bundle:** `spec_version` / bundle `0.1`。

**範囲:**

- 制度的フレーミングと定義。
- raw logやdiffとは異なるsemantic lineage。
- 最小lineage unitの義務。
- RDE review categoryと最小interchange record。
- lost semantic elementsの明示的表現。
- audit correlationの境界。
- human accountabilityの境界。
- 最小implementation conformance level。
- Phase 1 version compatibility rules。

**非目標:**

- すべてのinterchange fieldに対する完全なJSON Schema。
- wire protocol。
- storage engine上の義務。
- authentication、tenancy、scalability、retention、threat model requirements。
- automated RDE outputがhuman judgmentを承認・拒否・置換するという最終的主張。

**Exit criteria:**

- SLS-1からSLS-8が、公開されたPhase 1 normative documentとして存在する。
- 最小RDE review outputが外部から理解可能である。
- private repositoryに依存せず、Phase 1 conformanceを表明できる。
- 意図的に後送された作業が、incremental workまたはfuture normative workとして追跡されている。

### 2.2 Phase 2 — Interchange and schema hardening

**目的:** Phase 1の最小仕様を、Phase 1互換性を壊さずに、より厳密なvalidationとtyped interchange artifactへ発展させる。

**範囲候補:**

- 最小RDE review outputのJSON Schema baseline。
- `source_context_status` のようなclosed vocabularyのvalidation。
- implementation toolingにおけるstrict envelope validation。
- typed optional lineage properties。
- `minimal`、`standard`、`full` conformanceに対応するimplementation profile alignment。
- public interchange artifactに対するconformance test。

**非目標:**

- 広範なproduct UI specification。
- 最終的なstorage topology。
- network protocol commitment。
- 明示的レビューなしにimplementation-only envelopeをnormative SLS interchangeへ昇格すること。

**Exit criteria:**

- Phase 1 recordに対して、schemaとvalidator behaviorが一致している。
- unknown field、closed enum、version compatibilityが一貫して処理される。
- CLIとcoreのvalidation pathが、`kotonoha-spec`のsectionへtrace可能な形でfailureを報告する。

### 2.3 Phase 3 — Rich semantic modeling beyond minimal RDE categories

**目的:** Phase 1が意図的に最小RDE category内で表現している意味構造を、より豊かにmodel化する。

**範囲候補:**

- RDEの`lost` categoryを超えた、lost semantic elements専用表現。
- 正当化される場合のloss record用standalone identifierまたはpayload。
- より豊かなresponsibility-bearing context modeling。
- loss、audit、lineage、source context、human decisionの強いcorrelation。
- 既存validationへ委譲しつつnormative driftを避けるconsoleまたはagent ingest path。

**非目標:**

- すべてのimplementation transport wrapperをnormativeとして扱うこと。
- semantic lossを完全に網羅的にmodel化できると仮定すること。
- humanまたはinstitutional reviewを置換すること。

**Exit criteria:**

- 後送されたloss modelingが、公開normative proposalを持つか、明示的にinformativeとして維持されている。
- 当面のloss encodingは [SLS-6.5](representation-of-loss.md#sls-65-interim-encoding-phase-3-gate--2026-05) で公開済み。より網羅的なmodelingは [issue #3](https://github.com/zyx-corporation/kotonoha-spec/issues/3)（rollup [issue #25](https://github.com/zyx-corporation/kotonoha-spec/issues/25) の子）で追跡する。
- より豊かなsemantic objectがversion管理され、Phase 1 lineageおよびRDE outputとcorrelateされている。
- implementation側のPhase 3 wrapperが、SLS normative interchangeを黙って再定義していない。

### 2.4 Phase 4 — Cross-repository consolidation and reliability governance

**目的:** Phase 1からPhase 3のsurfaceが安定した後、仕様、実装、文書、ガバナンスを横断的に統合する。

**範囲候補:**

- `kotonoha-spec`、`kotonoha-core`、`kotonoha-cli`、`kotonoha-docs`、および関連実装リポジトリ間のcross-repository alignment。
- issueおよびpull requestからnormative adoptionへ至る公開ガバナンス。
- external implementer readiness。
- authentication、tenancy、scalability、retention、threat modeling、operational audit profileなどのreliability requirements。
- 必要に応じたconformance test suiteまたはcertification-style check。

**非目標:**

- internal management planを遡及的にnormative化すること。
- public conformanceのためにprivate repositoryへのアクセスを要求すること。
- product roadmap commitmentとspecification obligationを混同すること。

**Exit criteria:**

- 外部実装者がprivate contextなしにpublic specを読んでconformanceを実装できる。
- implementation behaviorがpublic sectionへ安定してtraceできる。
- 残るfuture workが、public issue、future SLS document、または明示的にnon-normativeなimplementation guidanceとして追跡されている。

## 3. Milestone families

以下のMilestoneは説明的なcheckpointである。normative SLS sectionから参照されない限り、それ自体はnormative requirementではない。

| Milestone | Family | Meaning | Typical repository |
| --- | --- | --- | --- |
| M1 | Workspace and lineage foundation | Git workspace、project context、database-backed MeaningDelta、review-decision primitives | `kotonoha-cli`, `kotonoha-core` |
| M2 | RDE metadata and export | RDE assessment metadata、validation report persistence、export formats | `kotonoha-cli`, `kotonoha-core` |
| M3 | Console / event ingest preparation | 既存RDE/interchange validationへ委譲するtransport wrapper、console-equivalent event、minimal editor UI | `kotonoha-cli`, `kotonoha-vscode`（IDE中心。チーム向けWeb Consoleは **M7** の [`kotonoha-web-console`](https://github.com/zyx-corporation/kotonoha-web-console)） |
| M3.5 | Normative backlog linkage | 将来normative化され得るthemeのpublic rollup — [issue #25](https://github.com/zyx-corporation/kotonoha-spec/issues/25) | `kotonoha-spec` |
| M4 | External tool correlation | GitHub Issue/PR correlationと関連audit reference | `kotonoha-cli`, `kotonoha-core` |
| M5 | AgentRun gateway | agent context、capability check、MCP/gateway route、agent-scoped MeaningDelta | `kotonoha-cli`, `kotonoha-core`, `kotonoha-mcp`, `kotonoha-gateway` |
| M6 | Team / principal mode | principal、project、role-based operational scoping | `kotonoha-cli`, `kotonoha-core`, gateway repos |
| M7 | Team-mode UI | project-scoped viewing/exportのためのweb-consoleまたはeditor surface。通常、writeまたはexportをCLI/core pathへ委譲する | `kotonoha-web-console`, `kotonoha-vscode` |

Milestone番号はimplementation-roadmap labelである。Specification conformanceを主張するときに、SLS section identifierの代替として用いてはならない。

## 4. PhaseとMilestoneの関係

| Specification Phase | Main concern | Supporting milestones | Normative status |
| --- | --- | --- | --- |
| Phase 1 | Public MVP、最小のreviewable SLS surface | M1/M2がその一部を実装上確認する場合がある | SLS-1からSLS-8でnormative |
| Phase 2 | Interchange and schema hardening | M2およびvalidator work | `kotonoha-spec`へ昇格された場合のみnormative |
| Phase 3 | Rich semantic modeling and transport wrappers | M3/M3.5およびloss-modeling work | 混在。wrapperは昇格されない限りnon-normative |
| Phase 4 | Consolidation and reliability governance | M4/M5/M6/M7およびpublic governance work | Future。明示的に追加されない限り未normative |

## 5. 現在の実装alignment check

この節は、この文書の導入時およびwrapper/UI repositoryへの拡張時に行ったalignment checkを記録する。

### 5.1 `kotonoha-core`

Observed alignment:

- `kotonoha-core` はinterchange validationにおいて `kotonoha-spec` bundle `0.1` を対象としている。
- `kotonoha-core` のRDE validationは、Phase 1の7つのcategory keyを検査する。
- `kotonoha-core` は未知のRDE category keyを拒否する。
- `kotonoha-core` はPhase 1 RDE validationにおいて `spec_version == 0.1` を強制する。
- `kotonoha-core` は、SLS-4と整合する形で、category item内のimplementation-specific keyを許容する。
- `kotonoha-core` は、`source_context_status` が存在する場合、それをPhase 1のclosed vocabularyとしてvalidationする。

Resolved discrepancy:

- このalignment update以前、`kotonoha-core` は新たに固定された `source_context_status` closed vocabularyをvalidationしていなかった。validatorは更新され、非string値または未知値はvalidation failureとなる。

Remaining note:

- `kotonoha-core` のstrict envelope behaviorはimplementation hardening layerである。それ自体は、対応する文言が `kotonoha-spec` に昇格されない限り、Phase 1 normative requirementを拡張しない。

### 5.2 `kotonoha-cli`

Observed alignment:

- `kotonoha-cli` はRDEおよびinterchange validationを `kotonoha-core` へ委譲する。
- 公開CLI定義は、conflictがある場合 `kotonoha-spec` を上位権威として扱う。
- Phase 2 minimum behaviorとPhase 3 ingest wrapperを、新しいSLS normative proseではなくimplementation behaviorとして文書化している。
- `kotonoha_core` dependencyは、`source_context_status` closed-vocabulary validationを含むrevisionへ更新されている。

Remaining note:

- M1、M2、M4、M5、M6などのCLI milestone labelはimplementation roadmap labelであり、SLS specification phaseとして読んではならない。

### 5.3 `kotonoha-mcp`

Observed alignment:

- `kotonoha-mcp` はtool executionをofficial `kotonoha` CLIへ委譲し、arbitrary shell commandを実行しない。
- `kotonoha_rde_validate` は `kotonoha rde validate --strict` へ委譲するため、Phase 1 RDE validationはCLI/coreに集中している。
- Human review toolは、human path上でのみ `kotonoha review approve|hold|reject` を呼び出し、`--agent-run-id` を付けない。
- Human review pathは、child process environmentから `KOTONOHA_AGENT_RUN_ID` を削除する。
- READMEは、management UX contractを、SLS normative proseの置換ではなくimplementation guidanceとして説明するよう更新されている。
- READMEの最小CLI versionは `kotonoha` 0.2.9+ に引き上げられ、wrapper validationが現在のPhase 1 `source_context_status` closed-vocabulary behaviorを含むようになっている。

Resolved discrepancy:

- `docs/mcp-server-contract.md` は以前、すべてのreview MCP toolを禁止しているように記述していた。契約文書は修正され、自律的review with agent contextは引き続き禁止しつつ、human-path review toolはagent contextなしの場合に限り許可される。

Remaining note:

- `ui://kotonoha/rde-summary` のようなMCP resourceやstructured content payloadはUI/tooling surfaceである。`kotonoha-spec`へ昇格されない限り、SLS normative interchangeを再定義しない。

### 5.4 `kotonoha-gateway`

Observed alignment:

- `kotonoha-gateway` は、`kotonoha-mcp` と同じtool nameのHTTP surfaceを公開し、official `kotonoha` CLIへ委譲する。
- `docs/gateway-contract.md` は、process spawningをgateway CLI delegation moduleへ制限し、arbitrary shell、direct `git`、direct `gh`、agent context付きのautonomous reviewを禁止する。
- Gateway environment variableはAPI keyをprincipal/projectへ対応付け、M6 behaviorのために `KOTONOHA_PRINCIPAL_ID` / `KOTONOHA_PROJECT_ID` をchild CLI processへ渡す。
- READMEは、management UX contractを、SLS normative proseの置換ではなくimplementation guidanceとして説明するよう更新されている。
- READMEの最小CLI versionは `kotonoha` 0.2.9+ に引き上げられ、gateway validationが現在のPhase 1 `source_context_status` closed-vocabulary behaviorを含むようになっている。

Remaining note:

- Gateway OpenAPI routeおよびtool wrapperはimplementation transport surfaceである。`kotonoha-spec`へ昇格されない限り、新しいSLS normative wire protocolを定義しない。

### 5.5 `kotonoha-vscode`

Observed alignment:

- `kotonoha-vscode` はMeaningDelta、RDE assessment、human review workflowのためのeditor UIである。
- 操作は、configured `kotonoha` CLIへsingle CLI helper pathを通じて委譲される。
- environment mappingは、設定されている場合、`DATABASE_URL`、`KOTONOHA_DECIDED_BY`、`KOTONOHA_PRINCIPAL_ID`、`KOTONOHA_PROJECT_ID` をchild CLI processへ渡す。
- READMEの最小CLI requirementは `kotonoha` 0.2.9+ に引き上げられ、現在のPhase 1 RDE validationには `source_context_status` validationを含むcore revisionが必要だと明記されている。

Remaining note:

- VS Code UI panel、keybinding、wireframeはimplementation UXである。追加のSLS normative obligationを作らない。

### 5.6 `kotonoha-web-console`

Observed alignment:

- `kotonoha-web-console` は現在、M7 Team Mode minimal web consoleおよびread-only scaffoldとして文書化されている。
- serverはprojectおよびdelta informationをdatabaseから読み、M6 exportを `kotonoha` CLIへ委譲する。
- project-scoped endpointは `KOTONOHA_PRINCIPAL_ID` を要求し、delta listまたはM6 data exportの前にproject visibility checkを行う。
- READMEはすでに `kotonoha` CLI 0.2.9+ を要求している。

Remaining note:

- web consoleのdirect database readは、read-only project viewのためのimplementation behaviorである。`kotonoha-spec`へ昇格されない限り、SLS normative storage requirementを定義しない。

### 5.7 Known non-differences

以下は意図的な差異であり、specification conflictではない。

- `kotonoha.interchange.v1` はcore-supported implementation envelopeであり、Phase 1 normative SLS interchange replacementではない。
- `kotonoha.console_event.v0` はCLI documentationにおけるPhase 3-style ingest wrapperであり、normative `kotonoha-spec` proseではない。
- MCP tools、HTTP gateway routes、VS Code panels、web-console APIs、database migrations、PostgreSQL tables、GitHub correlation tables、AgentRun tables、review-decision storageは、将来normative specification textへ昇格されない限りimplementation artifactである。

## 6. Maintenance rule

将来のpull requestが以下を変更する場合、この文書をreviewするべきである。

- 新しいSLS specification Phaseが導入される場合。
- public spec documentへmilestone languageが追加される場合。
- implementation-only envelopeがnormative promotionの候補になる場合。
- conformance levelが変更される場合。
- `kotonoha-core` または `kotonoha-cli` が、`kotonoha-spec` alignmentを主張するobservable validation behaviorを変更する場合。
- `kotonoha-mcp`、`kotonoha-gateway`、`kotonoha-vscode`、または `kotonoha-web-console` が、tool execution、review authority、principal/project scoping、validation delegation behaviorを変更する場合。
