# RDE assessment ストレージ増分（参考・日本語要約）

**正本:** [`rde-assessment-storage-increment.md`](rde-assessment-storage-increment.md) · **規範性:** informative

## 要約

- **SLS-4** の RDE レビュー出力（interchange）と、**MeaningDelta に紐づく JSONB 評価**（実装の `rde_assessments`）は別概念。
- 将来の公開仕様候補として、ストレージ行の論理フィールド（`payload`, `payload_schema_version`, `source_kind`, `audit_correlation_id`, `rde_document_id` 等）を整理。
- Phase 1/2 の検証プロファイル（SLS-9）や `lost` interim（[#3](https://github.com/zyx-corporation/kotonoha-spec/issues/3)）は置き換えない。
- 追跡: [#47](https://github.com/zyx-corporation/kotonoha-spec/issues/47)
