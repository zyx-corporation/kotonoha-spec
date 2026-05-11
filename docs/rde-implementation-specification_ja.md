# SLS-5 RDE implementation specification —参考・日本語 companion

**正本:** 規範文は英語版 [`rde-implementation-specification.md`](rde-implementation-specification.md) です。本ページは日本語で理解を補助する参考文書です。英語版と矛盾する場合は英語版が優先されます。

## SLS-5.1 目的

RDE implementation は、意味に関わる変化を評価し、構造化された review observation を出力する実装または実装上の責務です。RDE は、人間の判断、承認、説明責任を置き換えるものではありません。

RDE conformance を主張する実装は、少なくとも次を満たす必要があります。

- review subject または subject reference を受け取れること。
- 利用可能な場合、source material と changed material を検査できること。
- [`SLS-4`](rde-review-output.md) で定義された RDE category に従って observation を分類すること。
- RDE review output record を emit または validate できること。
- audit correlation に必要な traceability を保持できること。
- RDE observation と policy enforcement / final approval を区別すること。

## SLS-5.2 スコープ

### SLS-5.2.1 対象範囲

この文書は次を扱います。

- RDE implementation の責務。
- 最小 pipeline stage。
- input / output への期待。
- category classification の義務。
- validation boundary。
- memory layer および audit correlation との相互作用。
- human authority boundary。

### SLS-5.2.2 対象外

この文書は次を規定しません。

- 特定の LLM、SLM、rule engine、classifier、symbolic algorithm。
- model weight や prompt。
- latency、throughput、cost、hosting topology。
- すべての internal feature に対する完全 JSON Schema。
- policy engine、safety filter、human review の置換。

## SLS-5.3 実装上の役割

### SLS-5.3.1 RDE evaluator

RDE evaluator は、RDE observation を生成する component です。出力は [`SLS-4.3`](rde-review-output.md) の category key を用いて分類されなければなりません。

RDE evaluator は、change を accept、reject、publish、approve する最終権限を持つと主張してはなりません。

### SLS-5.3.2 Subject adapter

Subject adapter は、review 対象を RDE evaluation に渡せる形に整えます。対象には document revision、patch、pull request、design decision、generated text、lineage unit などが含まれます。

利用可能な場合、実装は安定した `subject_ref` を保持することが推奨されます。

### SLS-5.3.3 Context provider

Context provider は、previous lineage unit、source text、issue context、prior RDE output、audit reference などの prior material を供給します。

実装は partial context でも動作してよいですが、context 不足が review quality に影響する場合は、その不確実性や missing context を observation として表面化することが推奨されます。

### SLS-5.3.4 Output validator

Output validator は、emitted RDE review output record が [`SLS-4.4`](rde-review-output.md) の最小 interchange obligation を満たすかを確認します。

Validation は、malformed record と、well-formed だが semantic に弱いまたは不完全な record を区別しなければなりません。

## SLS-5.4 最小 pipeline

RDE implementation は、処理を次の logical stage として構造化することが推奨されます。内部では stage を結合しても構いませんが、外部から観測される責務は明確であるべきです。

### SLS-5.4.1 Subject intake

implementation は review 対象を受け取り、または解決します。

最小期待:

- `subject_ref` を識別する。
- 利用可能な source reference を保持する。
- raw text diff を完全な semantic subject として扱わない。

### SLS-5.4.2 Context assembly

implementation は relevant prior state と review context を集めます。

context には次が含まれます。

- prior lineage unit。
- original text または design intent。
- changed text または proposed artifact。
- issue、PR、decision context。
- prior RDE output。
- audit correlation identifier。

### SLS-5.4.3 Semantic observation

implementation は candidate semantic change を観測します。semantic change を raw lexical difference だけに還元してはなりません。

semantic observation では次を考慮できます。

- intent preservation。
- scope change。
- responsibility shift。
- unresolved tension。
- ambiguity や rationale の喪失。
- 追加された assumption。
- drift risk。

### SLS-5.4.4 Category classification

implementation は observation を [`SLS-4.3`](rde-review-output.md) の RDE category に分類します。

- `preserved`
- `transformed`
- `complemented`
- `intentionally_unresolved`
- `lost`
- `deviation_risk`
- `next_update_policy`

各 category は空でも構いませんが、compliant interchange output では category structure を保持しなければなりません。

### SLS-5.4.5 Output emission

implementation は [`SLS-4.4`](rde-review-output.md) に従う RDE review output record を emit します。

各 observation item は human-readable な `summary` を含むことが推奨されます。実装は evidence reference、confidence note、source span などの追加 field を含めても構いません。

### SLS-5.4.6 Validation and traceability

implementation は output shape を validate し、subject および audit trail との correlation を保持します。

実装は次を記録することが推奨されます。

- `subject_ref`
- RDE output identifier（利用可能な場合）
- related lineage unit identifier（利用可能な場合）
- source または prior unit reference（利用可能な場合）
- audit correlation identifier（保持している場合）

## SLS-5.5 Input requirements

### SLS-5.5.1 Minimum input

RDE implementation は、少なくとも一つの review subject または `subject_ref` を受け付けなければなりません。

### SLS-5.5.2 Prior state

prior state が利用可能な場合、implementation は preservation、transformation、loss、drift を区別するためにそれを使うことが推奨されます。

prior state が利用できない場合、implementation は結論を過剰に強めることを避け、missing context を unresolved または deviation-risk observation として記録できます。

### SLS-5.5.3 Human-provided context

intent statement や review note など、人間が提供した context は review input として用いることができます。その context が observation に重要な影響を与える場合、implementation は traceability を保持することが推奨されます。

## SLS-5.6 Output requirements

### SLS-5.6.1 Required output shape

compliant implementation は、[`SLS-4.4`](rde-review-output.md) で定義された logical RDE review output shape を emit または ingest できなければなりません。

### SLS-5.6.2 Observation item content

Observation item は簡潔な human-readable summary を含むことが推奨されます。

Observation item には次を含めても構いません。

- evidence reference。
- source span。
- confidence note。
- reviewer note。
- implementation-specific metadata。

implementation-specific metadata は、最小 RDE category を理解するために必須であってはなりません。

### SLS-5.6.3 Empty categories

空の category は valid でなければなりません。それは該当 category に material item が報告されていないことを示し、category が unsupported であることを意味しません。

## SLS-5.7 Memory layer interaction

RDE implementation は memory layer を使っても構いませんが、RDE は memory そのものではありません。

implementation は次を store または retrieve できます。

- prior RDE output。
- lineage unit reference。
- subject reference。
- loss observation。
- audit-correlation identifier。

memory layer は final human authority として扱われてはなりません。保存された observation は review record であり、approval decision ではありません。

## SLS-5.8 Audit correlation

implementation が audit trail を保持する場合、次の間に correlatable relationship を保持することが推奨されます。

- review subject。
- RDE review output。
- related lineage unit。
- human decision または approval。
- audit record。

Audit correlation は、RDE output だけが decision を authorize または reject することを意味してはなりません。

## SLS-5.9 Human authority boundary

RDE implementation は、人間の judgment、approval、rejection、publication responsibility、institutional accountability を置換すると表現してはなりません。

RDE implementation は、observation、missing context、loss、drift risk を表面化することで human review を支援できます。

## SLS-5.10 Policy and safety boundary

RDE は policy engine や safety filter とは異なります。

implementation は RDE observation を policy workflow に渡しても構いません。しかし RDE output 自体は semantic review record であり、自動的な refusal、approval、enforcement action、access-control decision ではありません。

## SLS-5.11 Implementation conformance

この RDE implementation specification への conformance を主張する implementation は、次を文書化しなければなりません。

- どの subject を evaluate できるか。
- context をどのように取得または受信するか。
- observation を RDE category にどのように mapping するか。
- RDE review output をどのように emit または validate するか。
- traceability をどのように保持するか。
- どの部分が automated で、どの部分に human review が必要か。

## SLS-5.12 実装パターン例（参考）

この節は informative です。

RDE implementation pattern には、たとえば次があります。

- structured review note に対する rule-based classifier。
- schema validation を伴う LLM-assisted evaluator。
- extractor と human review workflow の hybrid。
- CI-integrated documentation drift checker。
- PR を approve せず、RDE observation だけを emit する pull-request assistant。

これらは例であり、conforming implementation を制約するものではありません。
