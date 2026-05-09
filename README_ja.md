# kotonoha-spec

**Semantic Lineage System（SLS）の公開仕様**を置くリポジトリです。Kotonoha エコシステムのうち、外部レビュー・OSS 実装・第三者連携の正本となる文書面を担います。

**英語版（主文書）:** [README.md](README.md)

## このリポジトリに置くもの

- 概念仕様・アーキテクチャ仕様
- インターフェース定義、スキーマ、フォーマット仕様
- 公開可能な設計根拠と外部レビュー用の説明
- コントリビューター向けガイド（必要に応じて）
- 公開ロードマップのうち対外向けに固定したい部分

公開レビューに載せる前の草案や検討メモは当リポジトリ外で整理し、公開可能に安定した内容だけをここに追加します。

## 仕様書索引（normative・Phase 1）

規範テキストは **英語** の [`docs/`](docs/README.md) にあります（バンドル **0.1**）。入口は [Introduction（英語）](docs/introduction.md)。コントリビューションは [CONTRIBUTING.md（英語）](CONTRIBUTING.md)、変更履歴は [CHANGELOG.md（英語）](CHANGELOG.md) を参照してください。

## 関連リポジトリ

公開リポジトリ同士の参照のみ記載します。

| リポジトリ | 役割 |
| --- | --- |
| **kotonoha-spec（本リポジトリ）** | 公開仕様の正本 |
| [`kotonoha-core`](https://github.com/zyx-corporation/kotonoha-core) | SLS の OSS コア実装 |
| [`kotonoha-cli`](https://github.com/zyx-corporation/kotonoha-cli) | 公式 CLI（[`CLI 定義`](https://github.com/zyx-corporation/kotonoha-cli/blob/main/docs/cli-definition.md)） |
| [`kotonoha-docs`](https://github.com/zyx-corporation/kotonoha-docs) | 仕様に含まない公開ドキュメント（マニュアル・チュートリアル等） |

実装は、可能な限り本リポジトリの公開仕様に沿って [`kotonoha-core`](https://github.com/zyx-corporation/kotonoha-core) で進めます。

## 言語について

**`kotonoha-spec` 以下の文書は、デフォルトでは英文とします。** 日本語版は英語版に併置します。英語を主文書とし、日本語ファイルには原則として `*_ja.md` などの言語サフィックスを付け、英語版との対応が分かるようにします（例: `README.md` / `README_ja.md`）。

## ライセンス

特段の記載がない限り、リポジトリ内のコンテンツは [Apache License 2.0](LICENSE) に従います。ファイル単位で SPDX や追加条件を置く場合は、各文書側で明記します。

## リンク

- 本リポジトリ: https://github.com/zyx-corporation/kotonoha-spec
