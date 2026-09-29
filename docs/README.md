# ドキュメントガイド

[リポジトリトップ](../README.md) / [調査トップ](research/README.md)

現在の文書は、外部メモリー・知識更新の調査と、それに基づくシステム設計を扱います。先行研究は[調査の概要と目次](research/README.md)、構築方針は[オントロジー・ナレッジシステムの推奨設計](design/ontology-knowledge-system.md)から読めます。設計では PostgreSQL を知識の正本にし、原資料版を参照する文書 QA と必要箇所の主張抽出・メモリー更新を分けます。長い PDF の内部検索には任意の PageIndex アダプターを使い、文書集合検索は metadata/全文検索を基本として dense 検索・pgvector を比較候補にし、アクセス権・原資料版・原ページ引用を共通層で管理します。

## 文書の配置

公開実装の有無によらず先行研究を比較する場合は、[最先端研究の追加調査](research/papers.md#最先端研究への拡張)を参照してください。非公開の中核実装や理論提案も含め、論文の評価条件と再現可能な範囲を分けています。

関連システムを広く比較する場合は、[メモリー・文書検索・意味データ基盤の再調査](research/systems/extended-landscape.md)から、四領域の詳細と一次資料へ進めます。

その調査を補う[第2次調査](research/20260929193749-related-systems-resurvey/index.mdx)は、`research/20260929193749-related-systems-resurvey/`にMDX形式で置いています。同じディレクトリの4分野の詳細をmdxrで閲覧でき、出典と探索記録はJSON台帳にもまとめています。

パスは `docs/` からの相対パスです。

| 置き場所 | 内容 |
|---|---|
| [design/ontology-knowledge-system.md](design/ontology-knowledge-system.md) | 技術スタック、構成、知識の表現・更新、用途別の設計、実装順序 |
| [research/README.md](research/README.md) | 調査の要約、読む順序、全資料の目次 |
| [research/methodology.md](research/methodology.md) | 調査方法、根拠の扱い、確認範囲 |
| `research/foundations/` | 基本概念、オントロジー、知識更新の理論 |
| [research/systems/README.md](research/systems/README.md) と同じディレクトリ | システム比較の一覧と分類別の詳細 |
| `research/implementation/` | 確認した依存ライブラリと、周辺基盤の選択肢 |
| [research/papers.md](research/papers.md) | 研究の系譜、各論文の手法と限界 |
| [research/evaluation.md](research/evaluation.md) | ベンチマーク、報告値、再現条件、評価シナリオ |
| [research/design-directions.md](research/design-directions.md) | 設計の選択肢、検証仮説、実験計画 |
| [research/sources.md](research/sources.md) | 人が読む資料台帳。参照 ID、出典、対象版 |
| [research/evidence/](research/evidence/README.md) | 機械可読な出典・コミット・依存宣言の記録と読み方 |

## 追加・更新のルール

1. **内容に合う場所へ置く。** 調査資料は `research/`、具体的なシステム構成・データ契約・実装方針は `design/` に置きます。調査内では、論文の紹介は `papers.md`、評価条件と指標は `evaluation.md`、比較する設計仮説は `design-directions.md` にまとめます。
2. **入口から辿れるようにする。** 文書を追加したら[調査トップ](research/README.md)の目次を更新します。システムの詳細資料は[比較一覧](research/systems/README.md)からも案内します。
3. **相対リンクでつなぐ。** 各ページの冒頭に調査トップへのリンクを置き、本文末尾に関連資料を案内します。移動・改名時にはリンク元も更新します。
4. **本文と根拠を対応させる。** 引用は文書内の参照定義と[資料台帳](research/sources.md)、[sources.json](research/evidence/sources.json)で ID と URL を揃えます。調査方法や確認範囲の書き方は[調査方法](research/methodology.md)に合わせます。
5. **記録の意味を保つ。** 再調査した場合は確認日・コミット・確認の深さも更新します。配置だけの変更で、根拠データの確認日やハッシュを書き換えません。

本文は日本語、ファイル名は内容を表す英小文字とハイフンを基本とします。見出しは各文書で一つの `#` から始め、本文、関連資料、参照定義の順に配置します。
