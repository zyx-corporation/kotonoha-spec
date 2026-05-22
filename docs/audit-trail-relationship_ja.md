# 監査証跡との関係（参考・日本語要約）

**正本:** [**`audit-trail-relationship.md`**（SLS-7）](audit-trail-relationship.md)

**規範性:** 本ページは **informative** です。義務の解釈は英語正本に従います。

## 要約

- 多くの運用では、意味系譜ストアとは別に **監査証跡**（append-only ログ等）を持つ。
- RDE レビュー出力（SLS-4）と監査証跡の両方を持つ実装は、変更・公開アクションに関する監査記録と、対応する RDE の **`subject_ref`** を **相関できるメタデータ**を残すことが望ましい（SHOULD）。
- Phase 1 では相関キーの具体形は **implementation-defined**。
- 監査証跡に RDE カテゴリの要約を埋め込んでもよいが、**interchange 上の構造化 RDE を置き換えてはならない**（SHOULD NOT）。
- 保持・アクセス制御・PII は Phase 1 規範のスコープ外。ただし公開仕様が private リポジトリなしで解釈可能であること（SLS-1.7）と矛盾してはならない（MUST NOT）。
