# 喪失の表現（参考・日本語要約）

**正本:** [**`representation-of-loss.md`**（SLS-6）](representation-of-loss.md)

**規範性:** informative。義務は英語正本。

## 要約

- 意味系譜を扱う実装は、喪失が既知または意図的に受容されたとき、**明示的な場所**に **lost** 意味要素を置かなければならない（MUST）。字句差分だけから推論するだけでは足りない。
- 喪失の例: 曖昧さ・未解決の論点の削除、射程の省略、責任・リスクの消失、AI/人間の書き換えによる責任文脈の不明瞭化 など。
- 字句 diff は付随してよいが、喪失情報の**唯一**の载体にしてはならない（MUST NOT）。RDE の **`lost`** カテゴリ（SLS-4）が interchange 利用時の入口。
- 不確実なときも `intentionally_unresolved` または `lost` に要約を残すことが望ましい（SHOULD）。
- **SLS-6.5（2026-05）:** 当面は RDE **`lost`** で SLS-6.1 を満たす interim。独立スキーマは [#3](https://github.com/zyx-corporation/kotonoha-spec/issues/3)（親 [#25](https://github.com/zyx-corporation/kotonoha-spec/issues/25)）で追跡。
