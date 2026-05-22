# 版管理（参考・日本語要約）

**正本:** [**`versioning.md`**（SLS-8）](versioning.md)

**規範性:** informative。

## 要約

- Phase 1 公開バンドル: RDE レビュー出力の **`spec_version` は `0.1`**（SLS-4）。
- Phase 2: [**SLS-9**](phase2-interchange-hardening.md) と [`schemas/rde-review-output.phase2.schema.json`](../schemas/rde-review-output.phase2.schema.json) が **検証プロファイル**として normative。ネストされた `spec_version` 値そのものは Phase 1 互換のまま変えない。
- バンドル将来は semantic versioning 意図（MAJOR/MINOR/PATCH）。正式タグ運用前はリビジョン履歴を参照（SHOULD）。
- **閉じた列挙**（例: `source_context_status`）は minor で拡張してはならない（MUST NOT）。
- Phase 2 適合は **validator/profile の宣言**であり、自動的に `spec_version` を上げる意味ではない。
