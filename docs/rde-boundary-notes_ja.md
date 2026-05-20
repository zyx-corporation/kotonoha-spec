# RDE 境界ノート（参考・日本語）

**正本:** 規範文は英語版 [`introduction.md`](introduction.md)、[`architecture.md`](architecture.md)、[`rde-review-output.md`](rde-review-output.md)、[`rde-implementation-specification.md`](rde-implementation-specification.md) です。本ページは日本語での理解補助であり、英語版と矛盾する場合は英語版が優先されます。

## 要点

SLS は、意味そのものを固定物として完全保存する仕組みではありません。SLS が扱うのは、意味に関わる変化を後から追跡し、異議申し立てし、再審できるようにする関係と記録です。

Kotonoha は、SLS の仕様・方針・実装を束ねるエコシステム上の枠組みです。Kotonoha という名称だけで、RDE の自動評価が完成済み、権威的、または最終判断として十分であるとはみなしません。

Semantic lineage は、通常ログや raw diff そのものではありません。ログは「何が起きたか」を示し、diff は「文字列がどう変わったか」を示します。しかし semantic lineage は、意図、射程、不確実性、責任、制度上の張力、喪失、逸脱リスクなどが変更の前後でどう扱われたかを後から点検できるようにする関係です。

Source context は、変更を解釈するための先行資料、意図、lineage reference、review note、issue / pull request context、制度的制約などです。単一・完全・自明とは限らないため、supplied、resolved、inferred、missing、partial、contested などの状態を区別する必要があります。

RDE review output は structured observation です。人間または制度的 reviewer が、受容、差し戻し、保留、再審、修正を行うための材料であり、それ自体が最終的な意味判断ではありません。

## Reviewability metadata

PR #38 では、RDE category item に任意の reviewability metadata を持たせる説明が追加されました。例として、`evidence_ref`、`source_context_status`、`confidence_note`、`reviewer_note`、`decision_ref` があります。

これらは Phase 1 では任意です。存在しないからといって不確実性がないとは限らず、存在するからといって最終承認を意味するわけでもありません。

## 役割境界

| 役割 | 境界の要点 |
| --- | --- |
| Kotonoha framing | エコシステムを名付けるが、実装の完全性を証明しない |
| Lineage representation | semantic lineage を通常ログや raw diff に還元しない |
| Memory layer | 記録基盤であり、決定権限ではない |
| Subject adapter | 抽出した対象を完全な semantic subject とみなさない |
| Context provider | missing / partial / contested context を隠さない |
| RDE implementation | 最終判断の権威を主張しない |
| Generator | 自分自身の独立 evaluator とみなさない |
| Console / UI | 自動観測を人間または制度の最終承認に見せない |
| Human / institutional review | 最終的な公開・承認・設計判断の責任を担う |

## Phase 1 の対象外

Phase 1 は、意味を固定物として完全保存できること、RDE 出力が最終的な semantic judgment であること、画像・音声・動画などを含む完全な multimodal semantic-change evaluation が完成していることを主張しません。
