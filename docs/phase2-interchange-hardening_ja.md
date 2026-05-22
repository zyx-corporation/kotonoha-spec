# Phase 2 interchange 硬化（参考・日本語要約）

**正本:** [**`phase2-interchange-hardening.md`**（SLS-9）](phase2-interchange-hardening.md) — **Normative（Phase 2 検証プロファイル）**

本ページは日本語での読み補助のみ。検証義務の文言は英語正本。

## 要約

- Phase 2 は **RDE レビュー出力の検証プロファイル**。Phase 1 最小レコード形状は維持（`spec_version` **0.1**）。
- 必須: 7 カテゴリ、各値は配列、未知カテゴリ拒否、項目はオブジェクト、存在する **`source_context_status`** は閉じた語彙のみ。
- 規範スキーマ: [`rde-review-output.phase2.schema.json`](../schemas/rde-review-output.phase2.schema.json)。 prose と矛盾するときは **`docs/` の prose が優先**。
- **スコープ外（SLS-9.11）:** 将来の全オブジェクト schema、wire protocol、DB スキーマ、製品 UI、MCP/gateway/VS Code/web-console API、`kotonoha.interchange.v1`、console event wrapper — **昇格されない限り normative ではない**。
- 人間の最終判断を RDE 出力が置き換えてはならない（SLS-1.8 継承）。
