# 調査方法と根拠の扱い

[調査トップ](README.md) / [資料台帳](sources.md)

## 調査する問い

- 何を記憶単位にし、何を保存せず捨てているか。
- 追加情報によって既存知識がどう変化し、変更理由・過去状態・出典を追えるか。
- 型・関係・制約・時間・不確実性をどの層で表現するか。
- どの検索方法を使い、検索前後のどこで権限や有効期間を適用するか。
- 性能の主張は、どのデータ、モデル、採点器、費用条件に依存するか。
- どの公開ライブラリが実際に使われ、どこからがクラウド固有機能なのか。

## 資料の収集と採用

学術検索と公式サイト検索から候補を挙げ、論文の関連研究と現行リポジトリへたどった。2023 年以前の基礎概念、2023–2025 年の主要実装、2026-09-28 までの最近の研究を対象にする。サーベイは分類と探索に使い、個別実装の機能は当該コード・公式資料で確認する。検索スニペット、人気順、GitHub のスター数だけで採否や優劣を決めない。[P-SURVEY]

主要リポジトリは GitHub のコミット情報から対象版を確定し、README、manifest、関連する更新・検索コードを取得した。コードはインストールせず静的に確認した。取得ファイルの URL、コミット、SHA-256 を evidence に保存している。サブエージェントは使用していない。

主要論文のうち本文を確認したものは、arXiv HTML の手法・実験・制約の関連箇所を読んだ。資料台帳の `review_depth` が「要旨・書誌」のものについて、本文のアブレーションを確認したかのようには扱わない。出版社の書誌公開日と arXiv 初稿日も区別する。

## 根拠の種類

| 表示 | 意味 | そこから結論できないこと |
|---|---|---|
| コード確認 | 固定したコミットの実装に該当する処理がある | バグがない、全構成で同じ挙動、運用性能が高い |
| 公式仕様 | 提供者が機能・API・制限を明示している | 非公開内部処理の正確なアルゴリズム |
| 論文報告 | 著者らの実験条件下で得られた結果 | 他製品の現行版に対する優位、自社データでの再現 |
| 本調査の分析 | 確認した構造から導いた制約・適性 | 実験で観察された不具合 |
| 未確認 | 対象ファイル・資料では確定できない | その機能が存在しないこと |

個別項目に「未確認」とある場合、欠如と断定しない。たとえば API に削除があっても、派生要約・キャッシュ・バックアップまでの削除を検証したことにはならない。逆に、ベクトル DB を使っているだけで時間処理ができないとも判断しない。処理とデータモデルを読む必要がある。

## 再調査で修正した重要点

| 項目 | 再調査で確認したこと | 文書への反映 |
|---|---|---|
| Mem0 の更新 | 論文は四操作による更新。確認した 2.2.1 の通常 `add` は一回の抽出、追加、重複除去、実体リンクが中心 | 歴史的手法、OSS 現行コード、Platform を区別。[P-M0] [C-M0] |
| Mem0 Graph Memory | 固定した現行 docs は Platform の共起リンク方式を説明し、型付き関係を付けないと明記 | 旧 Neo4j 統合との「文書の食い違い」で止めず、移行後の意味を説明。[D-M0GRAPH] |
| Letta | 旧 repo は `archive` に旧 API server を置き、開発先を `letta-code` に案内 | MemGPT/旧 blocks と、現行ファイル・Git メモリーを区別。[C-LMOV] [C-LMEM] |
| Cognee | RDFLib ベースの ontology resolver と strict な照合処理が存在 | 単なるグラフ生成として扱わず、語彙との接続を調査。[C-COGONTO] [C-COGSTRICT] |
| Hindsight | 論文の四ネットワークと現行の observations / mental models の説明には粒度の違いがある | 紙面上のモデルとコード側のデータ型を対応付ける。[P-HIND] [D-HIND] |
| 研究の評価 | 高い recall、QA 正答、低い幻覚率は別指標 | データ分割、質問カテゴリ、回答器、採点器、棄却率を記述 |

## 限界

これは設計のための広範な文献・静的コード調査であり、全論文を列挙する形式的な systematic review ではない。全 transitive dependency、クラウド内部、全 DB adapter、モデル重みの挙動は未検証。論文ベンチマークも再実行していない。引用数を調査品質の代理指標にせず、判断を支える箇所まで追える構成にした。

## 関連ドキュメント

- [資料台帳](sources.md)
- [根拠データの読み方](evidence/README.md)

[P-SURVEY]: https://arxiv.org/abs/2512.13564v2
[P-M0]: https://arxiv.org/abs/2504.19413v1
[C-M0]: https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/mem0/memory/main.py
[D-M0GRAPH]: https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/docs/platform/features/graph-memory.mdx
[C-LMOV]: https://github.com/letta-ai/letta/blob/5bcdd177d70fa2b31a754cfcd801e77b2e1ab16a/README.md
[C-LMEM]: https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/src/agent/memory-filesystem.ts
[C-COGONTO]: https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/modules/ontology/rdf_xml/RDFLibOntologyResolver.py
[C-COGSTRICT]: https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/modules/ontology/construct_data_points_and_edges_with_ontology.py
[P-HIND]: https://arxiv.org/abs/2512.12818v1
[D-HIND]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/README.md
