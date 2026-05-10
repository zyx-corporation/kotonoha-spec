# 仕様書索引（参考・日本語）

**正本:** 規範言語・索引の一次資料は **[`README.md`](README.md)（英語）** です。本ページは読み順とファイル対応を日本語で補足します。**規範文の解釈は英語 `docs/` 配下が優先**です。

---

## repository-governance

| ファイル | メモ |
| --- | --- |
| [repository-governance.md](repository-governance.md) | インフォメーティブ。エコシステム間の依存方向など（文言は英文） |

## Phase 1 — 公開バンドル 0.1

| ファイル | メモ |
| --- | --- |
| [introduction.md](introduction.md) | **規範**／スコープ・定義・適合語 |
| [introduction_ja.md](introduction_ja.md) | **参考（日本語要約・図）**／正は `introduction.md` |
| [architecture.md](architecture.md) | **規範**／論理責務 |
| [architecture_ja.md](architecture_ja.md) | **参考（日本語ノードの図）**／正は `architecture.md` |
| [semantic-lineage-model.md](semantic-lineage-model.md) | **規範**／最小 lineage unit |
| [rde-review-output.md](rde-review-output.md) | **規範**／RDE カテゴリ interchange |
| [representation-of-loss.md](representation-of-loss.md) | **規範**／喪失の表現要求 |
| [audit-trail-relationship.md](audit-trail-relationship.md) | **規範**／監査との関係 |
| [versioning.md](versioning.md) | **規範**／版管理・互換変更 |

### 推奨読順（読み応え順）

[**Introduction（英または日要約）** → **Architecture** → lineage model → RDE output → loss → audit ↔ versioning**。  
用語だけ先に済ませるなら [introduction\_ja の Figure C と同順](introduction_ja.md) を、[architecture\_ja の図](architecture_ja.md) で論理責務を視覚的に補足できます。

**喪失表現と interchange の追跡:** [Issue **#3**](https://github.com/zyx-corporation/kotonoha-spec/issues/3)

## 増分・後送（概要）

詳細リストは **`README.md` の Incremental work** に従います。概要として:

- interchange 全項目の公開 JSON Schema
- lineage の型付き細目の段階的固定
- ネットワーク API・プロトコル詳細（interchange が安定後を想定）
- 認証・テナンシ・SLO／脅威モデルなど信頼性フェーズ側の項目
