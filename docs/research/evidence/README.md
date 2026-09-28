# 根拠データの読み方

[調査トップ](../README.md) / [調査方法](../methodology.md) / [資料台帳](../sources.md)

2026-09-28 の調査で記録した機械可読な台帳です。出典の一覧は[資料台帳](../sources.md)、調査の採否基準と未確認範囲は[調査方法](../methodology.md)を参照してください。

## ファイルの役割

| ファイル | 保存している情報 | 主なキー |
|---|---|---|
| [sources.json](sources.json) | 本文の参照 ID に対応する論文・標準・公式文書・コードの出典と確認の深さ | `sources[]` の `id`、`title`、`url`、`kind`、`accessed`、`review_depth` |
| [repository-snapshots.json](repository-snapshots.json) | 公開実装の固定コミット、取得したファイルの URL・サイズ・SHA-256、取得に失敗した要求 | `repositories[]` の `repository`、`commit`、`commit_date`、`retrieved_files`、`unsuccessful_requests` |
| [dependency-inventory.json](dependency-inventory.json) | 固定コミットの manifest に宣言された依存、版、ライセンスなど | `manifests[]` の `repository`、`commit`、`path`、`source_url`、`source_sha256`、`format` |

出典の種類によって項目は異なります。論文には `arxiv_version` や `submission_history`、コードには `repository`、`commit`、`path`、`sha256` などがあります。依存宣言も manifest の形式に応じて `runtime`、`optional`、`development`、`build`、`peer` などに分かれます。

## 本文から根拠を辿る

1. 本文の参照 ID（例: `C-M0`）を[資料台帳](../sources.md)または `sources.json` の `sources[].id` で探します。
2. `url` と `review_depth` を確認します。コードの出典なら `repository`、`commit`、`path` を使い、`repository-snapshots.json` の対象リポジトリと `retrieved_files` に対応させます。
3. 依存ライブラリを調べる場合は、同じリポジトリ・コミットの manifest を `dependency-inventory.json` で確認します。本文での役割の説明は[実装の依存関係](../implementation/dependencies.md)にあります。

## 記録の範囲

- リポジトリの記録は、取得したファイルのメタデータです。ソースコード全体のコピーや、ビルド・実行・ベンチマークの結果を含みません。
- 依存一覧は manifest の宣言です。対象環境で解決した lockfile や、推移的な依存を網羅する SBOM ではありません。
- URL、コミット、ハッシュは確認した対象を特定する情報です。各資料をどこまで確認したかは、`review_depth` と本文の未確認事項を合わせて読みます。

## 関連ドキュメント

- [調査方法と根拠の扱い](../methodology.md)
- [実装で宣言されている依存ライブラリ](../implementation/dependencies.md)
- [資料台帳](../sources.md)
