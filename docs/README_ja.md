# 仕様書索引（参考・日本語）

**正本:** 規範言語・索引の一次資料は **[`README.md`](README.md)（英語）** です。本ページは読み順とファイル対応を日本語で補足します。**規範文の解釈は英語 `docs/` 配下が優先**です。

---

## repository-governance

| ファイル | メモ |
| --- | --- |
| [repository-governance.md](repository-governance.md) | インフォメーティブ。エコシステム間の依存方向など（文言は英文） |
| [documentation-placement-policy.md](documentation-placement-policy.md) | `kotonoha-spec` と `kotonoha-docs` の役割分担 |
| [documentation-content-specification.md](documentation-content-specification.md) | 文書分類・配置仕様 |

## Phase 1 — 公開バンドル 0.1

Phase 1 の規範仕様文書には、`SLS-1.4.4` や `SLS-5.4.3` のような階層的な節番号を付与しています。

| 番号 | ファイル | メモ |
| --- | --- | --- |
| SLS-1 | [introduction.md](introduction.md) | **規範**／スコープ・定義・適合語 |
| SLS-1-ja | [introduction_ja.md](introduction_ja.md) | **参考（日本語要約・図）**／正は `introduction.md` |
| SLS-2 | [architecture.md](architecture.md) | **規範**／論理責務 |
| SLS-2-ja | [architecture_ja.md](architecture_ja.md) | **参考（日本語ノードの図）**／正は `architecture.md` |
| SLS-3 | [semantic-lineage-model.md](semantic-lineage-model.md) | **規範**／最小 lineage unit |
| SLS-3-ja | [semantic-lineage-model_ja.md](semantic-lineage-model_ja.md) | **参考（日本語 companion）** |
| SLS-4 | [rde-review-output.md](rde-review-output.md) | **規範**／RDE カテゴリ interchange |
| SLS-4-ja | [rde-review-output_ja.md](rde-review-output_ja.md) | **参考（日本語 companion）** |
| SLS-5 | [rde-implementation-specification.md](rde-implementation-specification.md) | **規範**／RDE 実装責務と境界 |
| SLS-5-ja | [rde-implementation-specification_ja.md](rde-implementation-specification_ja.md) | **参考（日本語 companion）**／正は `rde-implementation-specification.md` |
| SLS-ja-note | [rde-boundary-notes_ja.md](rde-boundary-notes_ja.md) | **参考（日本語補助）**／RDE 境界・source context・reviewability の要点 |
| SLS-6 | [representation-of-loss.md](representation-of-loss.md) | **規範**／喪失の表現要求 |
| SLS-6-ja | [representation-of-loss_ja.md](representation-of-loss_ja.md) | **参考（日本語 companion）** |
| SLS-7 | [audit-trail-relationship.md](audit-trail-relationship.md) | **規範**／監査との関係 |
| SLS-7-ja | [audit-trail-relationship_ja.md](audit-trail-relationship_ja.md) | **参考（日本語 companion）** |
| SLS-8 | [versioning.md](versioning.md) | **規範**／版管理・互換変更 |
| SLS-8-ja | [versioning_ja.md](versioning_ja.md) | **参考（日本語 companion）** |

### 推奨読順

**SLS-1 → SLS-2 → SLS-3 → SLS-4 → SLS-5 → SLS-6 → SLS-7 → SLS-8** の順で読むことを推奨します。

用語だけ先に済ませるなら [introduction_ja](introduction_ja.md) を、論理責務を視覚的に把握するなら [architecture_ja](architecture_ja.md) を参照できます。RDE 境界の要点を日本語で確認する場合は [rde-boundary-notes_ja](rde-boundary-notes_ja.md) を参照してください。

**喪失表現と interchange の追跡:** [Issue **#3**](https://github.com/zyx-corporation/kotonoha-spec/issues/3)（親 [#25](https://github.com/zyx-corporation/kotonoha-spec/issues/25)）

## Phase 2 — interchange 硬化

| 番号 | ファイル | メモ |
| --- | --- | --- |
| SLS-9 | [phase2-interchange-hardening.md](phase2-interchange-hardening.md) | **規範**（Phase 2 検証プロファイル） |
| SLS-9-ja | [phase2-interchange-hardening_ja.md](phase2-interchange-hardening_ja.md) | **参考（日本語 companion）** |
| Schema | [../schemas/rde-review-output.phase2.schema.json](../schemas/rde-review-output.phase2.schema.json) | Phase 2 最小 JSON Schema（規範アーティファクト） |

推奨読順は Phase 1 のあと **SLS-9** を読むこと（英語正本が検証義務の一次資料）。

**日本語 companion バックログ:** [Issue **#21**](https://github.com/zyx-corporation/kotonoha-spec/issues/21)

## 増分・後送（概要）

詳細リストは **`README.md` の Incremental work** に従います。概要として:

- interchange 全項目の公開 JSON Schema
- lineage の型付き細目の段階的固定
- ネットワーク API・プロトコル詳細（interchange が安定後を想定）
- 認証・テナンシ・SLO／脅威モデルなど信頼性フェーズ側の項目
- 必要に応じた RDE implementation profile の追加