# kotonoha-spec

**Semantic Lineage System（SLS）の公開仕様**を置くリポジトリです。Kotonoha エコシステムのうち、外部レビュー・OSS 実装・第三者連携の正本となる文書面を担います。

**英語版（主文書）:** [README.md](README.md)

## このリポジトリに置くもの

- 概念仕様・アーキテクチャ仕様
- インターフェース定義、スキーマ、フォーマット仕様
- 公開可能な設計根拠と外部レビュー用の説明
- コントリビューター向けガイド（必要に応じて）
- 公開ロードマップのうち対外向けに固定したい部分

内部の検討メモや未整理の草案は [`kotonoha-project`](https://github.com/zyx-corporation/kotonoha-project) で扱い、安定・公開可能になった内容をここへ移します。

## 関連リポジトリ

| リポジトリ | 役割 |
| --- | --- |
| [`kotonoha-project`](https://github.com/zyx-corporation/kotonoha-project) | 非公開のプロジェクト文書・設計検討の置き場 |
| **kotonoha-spec（本リポジトリ）** | 公開仕様の正本 |
| [`kotonoha-core`](https://github.com/zyx-corporation/kotonoha-core) | SLS の OSS コア実装 |

実装は、可能な限り本リポジトリの公開仕様に沿って [`kotonoha-core`](https://github.com/zyx-corporation/kotonoha-core) で進めます。

## 言語について

**`kotonoha-spec` 以下の文書は、デフォルトでは英文とします。** 日本語版は英語版に併置します。英語を主文書とし、日本語ファイルには原則として `*_ja.md` などの言語サフィックスを付け、英語版との対応が分かるようにします（例: `README.md` / `README_ja.md`）。

## ライセンス

特段の記載がない限り、リポジトリ内のコンテンツは [Apache License 2.0](LICENSE) に従います。ファイル単位で SPDX や追加条件を置く場合は、各文書側で明記します。

## リンク

- 本リポジトリ: https://github.com/zyx-corporation/kotonoha-spec
