# Semantic lineage model（最小）—参考・日本語 companion

**正本:** 規範文は英語版 [`semantic-lineage-model.md`](semantic-lineage-model.md) です。本ページは日本語で理解を補助する参考文書です。英語版と矛盾する場合は英語版が優先されます。

## Lineage unit

**Lineage unit** は、semantic lineage（意味の系譜）において、実装が識別し関係づけられなければならない最小の記録単位です。lineage feature への適合を主張する実装は、この単位を扱える必要があります。

## Identifier

各 lineage unit は **`id`** を持ちます。この `id` は、その実装が対象とする deployment または dataset の範囲内で一意でなければなりません。可能な場合は URI または IRI を使い、より広い一意性を確保することが推奨されます。

Phase 1 では `id` の厳密な構文は実装定義です。将来の interoperability profile が、より具体的な制約を加える可能性があります。

## Optional properties

Phase 1 では、`id` と lineage unit を関係づける能力を越える固定 schema は要求しません。実装は timestamp、author reference、他 unit との関係など、追加の machine-readable property を付与しても構いません。

## Relationships

ある lineage unit が別の lineage unit を継承する、またはそこから派生することを記録する場合、実装は prior unit の `id` を明示的に参照することが推奨されます。例として `prior_unit_id` や graph edge があります。ただし Phase 1 では、graph topology 全体は固定しません。

## Semantic change と lexical change

Semantic lineage を raw character-level diff だけと同一視してはいけません。lineage unit は VCS object ID など外部の change carrier を hint として参照してもよいですが、semantic observation は [`rde-review-output.md`](rde-review-output.md) または明示的な lineage field に保持する必要があります。解釈されない diff だけに閉じてはなりません。

## Incremental specification

`id` 以外の必須 field や標準 relation type など、より豊かな typed schema は、後続の specification version で追加される可能性があります。Phase 1 は、最小限の相互運用可能な土台を定める段階です。
