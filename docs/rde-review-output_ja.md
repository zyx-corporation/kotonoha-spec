# RDE レビュー出力（参考・日本語要約）

**正本:** [**`rde-review-output.md`**（SLS-4）](rde-review-output.md)

**規範性:** informative。カテゴリ名・フィールド名は英語正本どおり使用してください。

## 最小レコード（概要）

- トップレベルキー **`rde_review_output`**（オブジェクト）。
- **`spec_version`**: Phase 1 では **`0.1`**（文字列）。
- **`subject_ref`**: 空でない文字列（レビュー対象の安定参照）。
- **`categories`**: 次の **7 キーすべて必須**（各値は配列。空配列可）:
  - `preserved`, `transformed`, `complemented`, `intentionally_unresolved`, `lost`, `deviation_risk`, `next_update_policy`
- 未知のカテゴリキーは Phase 2 検証で拒否（SLS-9）。
- カテゴリ各項目はオブジェクト。少なくとも **`summary`**（SHOULD）。任意メタデータ例: `source_context_status`（閉じた語彙、SLS-4.4.3）、`evidence_ref` 等。

## `source_context_status`（閉じた語彙）

存在するときの値は次のみ: `supplied`, `resolved`, `inferred`, `missing`, `partial`, `contested`。詳細は英語 [SLS-4.4.3](rde-review-output.md#sls-443-source-context-status-vocabulary)。

Phase 2 検証: [SLS-9](phase2-interchange-hardening.md)。
