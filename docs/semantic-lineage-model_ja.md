# 意味系譜モデル（参考・日本語要約）

**正本:** [**`semantic-lineage-model.md`**（SLS-3）](semantic-lineage-model.md)

**規範性:** informative。

## 要約

- **Lineage unit** は意味系譜の最小単位。識別子と（任意の）先行 unit 参照で関係を表す。
- 通常のログや raw diff とは別次元。意図・不確実性・責任・喪失・逸脱などを後から検査可能にする。
- Phase 1 では正式な全フィールド JSON Schema は後送；最小プロパティは英語正本の SLS-3 を参照。
- 実装側の `kotonoha.interchange.v1` 等は **implementation envelope**（normative ではない）。
