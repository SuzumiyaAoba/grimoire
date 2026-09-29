# 根拠データの読み方

[調査トップ](../README.md) / [調査方法](../methodology.md) / [資料台帳](../sources.md)

2026-09-28 の調査で作成し、2026-09-29 に概念の基礎・応用の確認記録を追加した機械可読な台帳です。出典の一覧は[資料台帳](../sources.md)、調査の採否基準と未確認範囲は[調査方法](../methodology.md)を参照してください。

## ファイルの役割

| ファイル                                                   | 保存している情報                                        | 主なキー                                                                                             |
| ------------------------------------------------------ | ----------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| [sources.json](sources.json)                           | 本文の参照 ID に対応する論文・標準・公式文書・コードの出典と確認の深さ           | `sources[]` の `id`、`title`、`url`、`kind`、`accessed`、`review_depth`                                |
| [repository-snapshots.json](repository-snapshots.json) | 公開実装の固定コミット、取得したファイルの URL・サイズ・SHA-256、取得に失敗した要求 | `repositories[]` の `repository`、`commit`、`commit_date`、`retrieved_files`、`unsuccessful_requests` |
| [dependency-inventory.json](dependency-inventory.json) | 固定コミットの manifest に宣言された依存、版、ライセンスなど             | `manifests[]` の `repository`、`commit`、`path`、`source_url`、`source_sha256`、`format`               |
| [system-landscape.json](system-landscape.json) | 関連システム再調査の候補、従来の扱い、用途、確認の深さ、保留理由 | `systems[]`、`counts_by_group`、`supplemental_records`、`deferred_records` |
| [landscape-searches.json](landscape-searches.json) | 分野別の代表クエリ、直接参照、選定・保留の記録 | `groups[]`。検索結果全件や順位を保存したデータではない |
| [frontier-research.json](frontier-research.json) | 実装公開を条件にしない研究比較。手法・評価・限界、コード/重み/データの公開状態、探索と保留 | `studies[]`、`sources[]`、`searches[]`、`deferred[]`。実行再現の記録ではない |
| [第2次調査の統合台帳](../20260929193749-related-systems-resurvey/survey-evidence.json) | 追加4分野の候補、件数、既存掲載・確認範囲と分野別台帳への参照 | `systems[]`、`counts`、`groups[]`。前回50系統の台帳とは別の調査記録 |

出典の種類によって項目は異なります。論文には `arxiv_version` や `submission_history`、コードには `repository`、`commit`、`path`、`sha256` などがあります。依存宣言も manifest の形式に応じて `runtime`、`optional`、`development`、`build`、`peer` などに分かれます。

## 追加調査の記録

2026-09-29 の基礎・応用の追加調査は、`sources.json` に追記しています。新規資料には今回の `accessed` と `review_depth` を保存し、既存資料を読み直した場合は、その資料の `additional_reviews[]` を追加します。

同日の PageIndex 追加調査は [repository-snapshots.json](repository-snapshots.json) に固定 commit とファイルごとのサイズ・SHA-256 を記録しています。対象は PageIndex 本体22ファイルと、評価関連2リポジトリから取得した README / `eval.py` 計3ファイルです。これは選択ファイルの静的確認用 snapshot で、リポジトリ全体のコピー、依存解決結果、テスト・製品 API・ベンチマークの実行記録ではありません。確認方法は[調査方法](../methodology.md)、コード上の要点は[PageIndex の詳細](../systems/pageindex.md)を参照してください。

| 項目                                            | 意味                         |
| --------------------------------------------- | -------------------------- |
| トップレベルの `accessed`                            | 初期台帳を作った調査の確認日             |
| トップレベルの `updated` / `update_scope`            | 台帳を更新した日と、追加調査の対象範囲        |
| 各資料の `accessed` / `review_depth`              | その資料を最初に台帳へ記録した時の確認日・確認の深さ |
| `additional_reviews[].accessed`               | 同じ URL の資料を追加で確認した日        |
| `additional_reviews[].review_depth` / `scope` | 今回どこまで読んで、何の説明に使ったか        |
| `checked_sections`                            | 確認対象の節・論点。新規資料または追加レビューに記録 |

最新の確認範囲を読む時は、元の記録と追加レビューの両方を確認してください。追加レビューがあっても、元のコードのコミットやハッシュを取り直したことにはなりません。URL が別の版へ変わる場合は、同じ記録の URL を無条件に置換せず、版の違いが分かる ID や記録を用意します。

同日の[関連システム再調査](../systems/extended-landscape.md)では、四領域の追加・深掘り候補を `system-landscape.json` に記録した。これは今回の比較範囲であり、既存の全システムを重ねて数えた総製品数ではない。研究のみ・探索保留の記録も区別する。資料と選択コードは既存台帳へ追記したが、`dependency-inventory.json` の全候補・全推移依存の監査を更新したものではない。

## 本文から根拠を辿る

[最先端研究の追加調査](../papers.md#最先端研究への拡張)では、`frontier-research.json`の各研究に初稿日、確認版、査読情報、既存掲載、手法、著者の評価、限界、設計への示唆を記録します。`artifact_status`はコード・重み・データを別々に持ち、根拠URLと確認範囲を伴います。「非公開と明記」「公開予定」「今回の確認範囲で見つからない」を区別し、理論提案の未実装と、実装はあるが配布されていない状態も混同しません。同じURLの出典は`sources.json`の既存IDと過去の確認記録を保ち、今回の確認だけを追加します。

[第2次調査](../20260929193749-related-systems-resurvey/index.mdx)の分野別JSONは文書と同じディレクトリに置いた。出典はこのディレクトリの `sources.json` にも統合している。主比較の候補、隣接ツール、探索保留は分けて集計し、過去資料との単純な件数の加算は行わない。

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
