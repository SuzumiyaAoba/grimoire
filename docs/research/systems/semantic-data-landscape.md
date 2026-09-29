# 意味データ・オントロジー・版管理システムの比較

[調査トップ](../README.md) / [横断比較](extended-landscape.md) / [資料台帳](../sources.md)

確認日: 2026-09-29

## 範囲と読み方

既存の知識検索資料は GraphRAG 系の検索方式、インフラ資料は Neo4j、Stardog、TerminusDB、XTDB、TypeDB などの製品名を挙げている。本ページでは、製品名の一覧から一歩進み、どこが保存エンジンで、どこがオントロジー管理・データ統合・業務アプリ層なのか、また推論・制約・時間・根拠のどれを扱うかを比較する。既存資料ですでに概要があった Stardog、TypeDB、TerminusDB、XTDB、Palantir は「概要のみ・今回深化」、それ以外は「新規」とした。

製品の挙動は2026-09-29時点で読めた公式公開仕様を中心に整理した。ベンダー資料は提供者の説明であり、非公開内部実装の確認ではない。代表的なコード確認は TerminusDB の固定コミットに絞り、対象ファイルの docstring を静的に読んだ。インストール、API 接続、ビルド、テスト、性能測定は実行していない。商用サービスは公開仕様を読む深さにとどまり、実環境の構成や導入契約は比べていない。

## 比較マトリクス

**意味データ・統合・履歴の境界（公開仕様の比較）**

|                           | 保存モデル                                        | 推論・制約                                                                                                           | データ統合                                                                                         | 版・時間                                                                                                               | 導入・ライセンス確認                                         |
| ------------------------- | -------------------------------------------- | --------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------- |
| GraphDB                   | RDF / SPARQL グラフDB                           | 前向きルール推論をロード時にmaterialize。RDFS / OWL 2 RL・QL等のrulesetとSHACL検証 D-LS-GDB-REASON D-LS-GDB-SHACL                    | RDF repositoryにロード。ChatGPT retrieval connectorはRDFのテキスト化とベクトル索引同期を行う実験機能 D-LS-GDB-CONNECTOR   | 推論の再計算・撤回は仕様にあるが、確認した範囲でbitemporal fact storeは示されない                                                                | サーバ製品。現行契約・ライセンス詳細は比較していない                         |
| RDFox                     | RDF、規則、OWL 2 / SWRL公理を扱うエンジン                 | materialize方式。増分更新・導出factのproof explanation・SHACL検証 D-LS-RDFOX-FEATURES                                         | CSV、RDB、Solr等の外部sourceを参照                                                                     | 保存状態の復元はできるが、実世界の有効時刻を持つ履歴モデルは今回確認していない                                                                            | プロプライエタリ製品。評価ライセンスを公開 D-LS-RDFOX-LICENSE           |
| Stardog / Voicebox        | RDFナレッジグラフ。Virtual Graphを同じSPARQL面に含められる     | 推論はquery-timeのlazy reasoningで、導出tripleをDBに常時materializeしない D-STARDOG                                            | SQL等へのvirtual mapping。Voiceboxはオントロジー・mapping案の生成と人によるレビューを支援 D-VIRTUAL D-LS-STARDOG-VOICEBOX | 確認した仕様だけではbitemporal fact historyは判定できない                                                                           | 商用DBの公開ドキュメントを確認。契約条件は未比較                          |
| TriplyDB                  | ホスト型RDFデータセット、ETL、編集・可視化                     | SHACL shapeから編集フォームとデータ検証を構成 D-LS-TRIPLY-EDIT D-LS-TRIPLY-ETL                                                   | ETLでextract / transform / assert / enrich / validate / publish                                | 保存クエリには明示的な版番号がある。編集した資源の履歴も示すが、全データセットの時間DBとはみなさない D-LS-TRIPLY-SAVED                                              | ホスト型サービス。基盤のライセンス条件は未確認                            |
| eccenca Corporate Memory  | Build / Explore / cmemcを組み合わせる企業向けKG統合・管理基盤  | OWL語彙に基づくRDF統合。ExploreにSHACLに基づく編集・検証                                                                           | SQL・ファイル等をRDFスキーマへ写像し、外部triple storeも構成できる D-LS-ECCENCA-ARCH D-LS-ECCENCA-BUILD               | Marketplace packageは語彙、shapes、graphs、Build project等のversioned artifact。factのvalid-timeとは別 D-LS-ECCENCA-MARKETPLACE | 企業向け配布基盤。利用条件は未確認                                  |
| TopBraid EDG              | RDF / OWL / SPARQL / SHACLを使うデータ・語彙ガバナンス製品   | OWLの推論的な意味とSHACLの検証を区別して扱う D-LS-W3C-OWL S-SHACL                                                                 | asset collection、語彙、参照データの編集・承認workflow                                                       | working copyをレビュー後に承認・commitしてproduction copyへ反映 D-LS-TOPBRAID-WORKFLOW                                            | 商用製品の公開文書を確認。版・契約条件は未比較                            |
| Ontop                     | RDBをRDFとして見せるVirtual Knowledge Graph / OBDA  | RDFS・OWL 2 QLをquery rewritingで扱う                                                                                | R2RML / Ontop mappingsからSPARQLをSQLへ変換し、通常はデータをコピーしない D-LS-ONTOP-GUIDE                         | 履歴は元RDBの責務。Ontop自体の事実履歴は今回確認していない                                                                                  | Apache-2.0、OSS D-LS-ONTOP-GUIDE                    |
| TypeDB                    | entity / relation / attributeを持つネイティブ型付きグラフ  | schemaと型・値制約。TypeDB 3 functionsはqueryに必要な結果をgoal-drivenに計算し、全結果を事前materializeしない D-TYPEDB D-LS-TYPEDB-FUNCTIONS | 独自TypeQLスキーマとデータモデル。RDF / OWL / SHACLとは別                                                      | 一般的なvalid-time / system-time履歴は今回確認していない                                                                           | Community EditionはMPL-2.0 D-LS-TYPEDB-REPO         |
| TerminusDB                | JSON / RDF文書グラフ                              | 型スキーマ・制約とWOQL。RDF reasonerと同列の推論機能として扱わない                                                                       | JSON-LD・グラフの文書データを取り込み・問い合わせ                                                                  | immutable commit、branch、diff、merge、過去commitの読取り D-LS-TERMINUS-VERSION。固定commitのコードも静的確認 C-LS-TERMINUS-LAYER        | Community repositoryはApache-2.0 D-LS-TERMINUS-REPO |
| XTDB                      | SQL中心の汎用DB                                   | SQLによるデータ問い合わせ。オントロジー・OWL推論エンジンではない                                                                             | SQLデータとしてアプリケーションのモデルを保存                                                                      | 全テーブルでsystem-timeとvalid-timeの二時制を扱う D-LS-XTDB-TIME                                                                 | OSS、MPL-2.0 D-LS-XTDB-LICENSE                      |
| Palantir Foundry Ontology | table datasetをobject / link / actionで表す業務意味層 | 業務オブジェクトと操作の型。公開資料の意味モデルはW3C OWL/RDFの論理推論とは別 D-LS-PALANTIR-INTRO D-LS-PALANTIR-ONTOLOGY                         | backing dataset、アプリ、API、action、lineage、permissionを一体化                                         | Ontology編集や操作履歴は業務アプリ側の機能。RDF factのbitemporal semanticsは確認していない                                                    | 商用エンタープライズ基盤の公開仕様のみ                                |

### 行別の根拠ID

- GraphDB: [D-LS-GDB-REASON] [D-LS-GDB-SHACL] [D-LS-GDB-CONNECTOR]
- RDFox: [D-LS-RDFOX-FEATURES] [D-LS-RDFOX-LICENSE]
- Stardog / Voicebox: [D-STARDOG] [D-VIRTUAL] [D-LS-STARDOG-VOICEBOX]
- TriplyDB: [D-LS-TRIPLY-ETL] [D-LS-TRIPLY-SAVED] [D-LS-TRIPLY-EDIT]
- eccenca Corporate Memory: [D-LS-ECCENCA-ARCH] [D-LS-ECCENCA-BUILD] [D-LS-ECCENCA-MARKETPLACE]
- TopBraid EDG: [D-LS-TOPBRAID-INTRO] [D-LS-TOPBRAID-WORKFLOW] [D-LS-W3C-OWL] [S-SHACL]
- Ontop: [D-LS-ONTOP-GUIDE] [D-LS-ONTOP-REPO]
- TypeDB: [D-TYPEDB] [D-LS-TYPEDB-FUNCTIONS] [D-LS-TYPEDB-REPO]
- TerminusDB: [D-LS-TERMINUS-VERSION] [D-LS-TERMINUS-REPO] [C-LS-TERMINUS-LAYER]
- XTDB: [D-LS-XTDB-TIME] [D-XTDB] [D-LS-XTDB-LICENSE]
- Palantir Foundry Ontology: [D-LS-PALANTIR-INTRO] [D-LS-PALANTIR-ONTOLOGY] [D-LS-PALANTIR-PERMISSIONS]

## 比較から分かる境界

### 推論、型制約、検証は同じ機能ではない

RDF/OWL推論器は、明示されたtripleと公理から論理的に帰結するtripleを導出する。GraphDBとRDFoxは前向き推論をmaterializeし、Stardogはquery-timeに導出する。RDFoxが説明するproofは推論規則から結論が導けた論理的説明であり、原資料の該当ページや人が確認した根拠を自動で保証するものではない。[D-LS-GDB-REASON] [D-LS-RDFOX-FEATURES] [D-STARDOG]

SHACLはグラフが指定された形状・制約に合うか検証する標準で、OWLの推論意味論とは別の層にある。TypeDBも型・関係・属性の整合性を独自の型システムで制約するが、RDF/OWL/SHACLと互換な意味論だとは読み替えない。[S-SHACL] [D-LS-W3C-OWL] [D-TYPEDB]

GraphDBのTalk to Your GraphとChatGPT Retrieval Connectorは実験機能として文書化されている。GraphDB自体のRDF推論ストアや、connectorが同期する外部ベクトル索引とは別の機能境界であり、単独のGraphRAGデータベースとして数えない。[D-LS-GDB-TALK] [D-LS-GDB-CONNECTOR]

### 仮想化・統合層は元データの正本を置き換えない

OntopとStardog Virtual Graphsはrelational sourceをRDF/SPARQLの語彙で問い合わせる仮想化方式で、mappingとquery rewritingが中核となる。eccenca Buildは複数の原形式を統合グラフへ写像する。いずれも「統合モデルをどこに定義するか」と「元データの正本・履歴をどこで保持するか」を分けて設計する必要がある。[D-LS-ONTOP-GUIDE] [D-VIRTUAL] [D-LS-ECCENCA-ARCH]

TriplyDBとTopBraid EDGはRDF/SHACLを編集・ガバナンスに使う体験を提供し、eccenca Corporate MemoryはBuild/Exploreと外部triple storeを束ねる。これはGraphDB/RDFoxのようなエンジンそのものと同じ製品境界ではない。Palantir FoundryのOntologyもオブジェクト、リンク、アクションをbacking datasetに結び付ける業務アプリ向け意味層であり、名称にontologyが含まれることだけでOWLクラス・公理体系と同一視できない。[D-LS-TRIPLY-EDIT] [D-LS-TOPBRAID-WORKFLOW] [D-LS-ECCENCA-ARCH] [D-LS-PALANTIR-INTRO]

### バージョン、事実の時間、有効期間を区別する

TriplyDBの保存クエリ版、TopBraid EDGのworking-copy承認、eccenca Marketplaceのversioned packageは、クエリ・編集提案・配布物の版を扱う。一方、TerminusDBはデータのcommitとbranchを対象にし、XTDBはtransaction/system timeとvalid timeの両軸でレコードを時制管理する。これらは「過去にどの文書を根拠に何を主張し、誰がいつ採否を判断したか」という意味的な主張履歴と同義ではない。[D-LS-TRIPLY-SAVED] [D-LS-TOPBRAID-WORKFLOW] [D-LS-ECCENCA-MARKETPLACE] [D-LS-TERMINUS-VERSION] [D-LS-XTDB-TIME]

TerminusDBの固定ソースでは、named graphのheadが指すlayerをsnapshot isolation相当の読取りとして取得し、書込みbuilderを親layerの子として作り、差分をcommitして新しいlayerにするAPIの説明を確認した。これはcommit/layerがデータ状態を表す静的な証拠である。原資料の出典、ページ番号、factのvalid-time、競合する主張の採否を自動管理する機能まで確認したものではない。[C-LS-TERMINUS-LAYER]

出典・来歴はさらに別の軸である。W3C PROV-OはEntity、Activity、Agentを使うprovenanceの語彙を定義するが、その語彙を保存するだけで一次資料への引用、アクセス制御、抽出の正確性、真偽の決定が保証されるわけではない。推論proof、ETLのdata lineage、DB commit、原資料の引用をひとつのprovenance欄にまとめず、必要な粒度で結び付ける。[S-PROV] [D-LS-RDFOX-FEATURES] [D-LS-PALANTIR-INTRO]

## 設計に持ち帰ること

- RDF/OWLが必要なら、推論を保存時にmaterializeするかquery-timeに解くかを選ぶ。どちらの方式も、モデルが正しいか・根拠が実在するかまでは判定しない。
- RDBを正本のまま意味的に問い合わせたい場合はOntopやStardog Virtual Graphsが候補になる。統合グラフを構築・管理したい場合はeccenca Buildのような別層が候補になる。
- schema authoringと検証にはOWL、SHACL、TypeDB型制約のどれを使うかを明示し、推論と検証を要件上も分ける。
- 過去データの参照が目的なら、branch/commit型のTerminusDBと二時制モデルのXTDBは異なる解を持つ。どちらも主張の出典・訂正理由・承認者を設計せずに与えるものではない。
- LLMによる抽出や記憶更新の正否は、いずれの製品のデータベース機能とも同一視しない。原文書、版、ページ引用、抽出結果、レビュー、採否履歴を共通のアプリケーションモデルで管理する。

## 歴史的参考：RecallGraph と後継 Minigraf

公式 [RecallGraph README][D-LS-RECALLGRAPH-README] はリポジトリが2026-04-25にarchiveされたと示し、ArangoDB 3.x上のFoxx microserviceとしてイベント履歴、過去時点のgraph queryを説明する。同じREADMEはvalid-timeやbranch/tagをroadmap扱いし、valid-timeは未実装だったと明記する。後継 [Minigraf README][D-LS-MINIGRAF-README] は自らをspiritual successorでありportではないと説明し、embedded graph storeとしてtransaction timeとvalid timeの両方を掲げる。RecallGraphの制約をMinigrafへ引き継いだとは扱わず、RecallGraphを歴史的参考、Minigrafを追加監視候補として11件の主比較から分ける。

名前だけで検索すると、別のPyPIパッケージrecallgraph 0.0.2も見つかる。そのリンク先はIndhar01/MemoGraphへ移るため、公式RecallGraph/RecallGraphとは同一視しない。[D-LS-RECALLGRAPH-PYPI] [D-LS-RECALLGRAPH-SAME-NAME]

## 根拠と確認範囲

出典IDは[資料台帳](../sources.md)に、固定コミットの取得記録は[repository snapshots](../evidence/repository-snapshots.json)に統合する調査フラグメントに対応する。TerminusDBの対象ファイルはraw URLから取得した固定コミットのソースで、64,391 bytes、SHA-256はa2f40ae488be8fd37b11ec2747689854128fb291a4b31987914ceeb760dbb5d8。docstringを静的確認しただけで実行はしていない。ほかのシステムも公開仕様・製品文書の確認であり、コード監査ではない。性能、正確性、可用性、契約上の条件はここから推定しない。

[D-LS-W3C-OWL]: https://www.w3.org/TR/owl2-overview/

[S-SHACL]: https://www.w3.org/TR/shacl/

[S-PROV]: https://www.w3.org/TR/prov-o/

[D-LS-GDB-REASON]: https://graphdb.ontotext.com/documentation/11.0/reasoning.html

[D-LS-GDB-TALK]: https://graphdb.ontotext.com/documentation/11.0/talk-to-graph.html

[D-LS-GDB-CONNECTOR]: https://graphdb.ontotext.com/documentation/11.0/retrieval-graphdb-connector.html

[D-LS-GDB-SHACL]: https://graphdb.ontotext.com/documentation/11.0/pdf/GraphDB.pdf

[D-LS-RDFOX-FEATURES]: https://docs.oxfordsemantic.tech/features-and-requirements.html

[D-LS-RDFOX-LICENSE]: https://www.oxfordsemantic.tech/rdfox-evaluation-license

[D-STARDOG]: https://docs.stardog.com/inference-engine/

[D-VIRTUAL]: https://docs.stardog.com/virtual-graphs/

[D-LS-STARDOG-VOICEBOX]: https://docs.stardog.com/voicebox/guided-ontology-creation-and-mapping/

[D-LS-TRIPLY-ETL]: https://docs.triply.cc/triply-etl/validate/

[D-LS-TRIPLY-SAVED]: https://docs.triply.cc/triply-db-getting-started/saved-queries/

[D-LS-TRIPLY-EDIT]: https://docs.triply.cc/triply-db-getting-started/editing-data/

[D-LS-ECCENCA-ARCH]: https://documentation.eccenca.dev/latest/deploy-and-configure/system-architecture/

[D-LS-ECCENCA-BUILD]: https://documentation.eccenca.dev/latest/build/introduction-to-the-user-interface/

[D-LS-ECCENCA-MARKETPLACE]: https://documentation.eccenca.dev/latest/distribution/marketplace/

[D-LS-TOPBRAID-WORKFLOW]: https://docs.topquadrant.com/8.0/user_guide/workflows/index.html

[D-LS-TOPBRAID-INTRO]: https://docs.topquadrant.com/7.4/introduction/index.html

[D-LS-ONTOP-GUIDE]: https://ontop-vkg.org/guide/

[D-LS-ONTOP-REPO]: https://github.com/ontop/ontop

[D-TYPEDB]: https://typedb.com/docs/core-concepts/typeql/schema-data/

[D-LS-TYPEDB-FUNCTIONS]: https://typedb.com/docs/typeql-reference/functions/functions-vs-rules/

[D-LS-TYPEDB-REPO]: https://github.com/typedb/typedb

[D-LS-TERMINUS-VERSION]: https://terminusdb.org/docs/version-control-operations/

[D-LS-TERMINUS-REPO]: https://github.com/terminusdb/terminusdb

[C-LS-TERMINUS-LAYER]: https://github.com/terminusdb/terminusdb/blob/57f2093baeafd65e16004e84b7b58e0c5cf72858/src/library/terminus_store.pl#L171-L307

[D-LS-XTDB-TIME]: https://docs.xtdb.com/about/time-in-xtdb.html

[D-XTDB]: https://docs.xtdb.com/concepts/key-concepts.html

[D-LS-XTDB-LICENSE]: https://docs.xtdb.com/

[D-LS-PALANTIR-INTRO]: https://www.palantir.com/docs/foundry/getting-started/introductory-concepts

[D-LS-PALANTIR-ONTOLOGY]: https://www.palantir.com/docs/foundry/ontology/overview

[D-LS-PALANTIR-PERMISSIONS]: https://www.palantir.com/docs/foundry/object-permissioning/overview

[D-LS-RECALLGRAPH-README]: https://github.com/RecallGraph/RecallGraph

[D-LS-MINIGRAF-README]: https://github.com/project-minigraf/minigraf

[D-LS-RECALLGRAPH-PYPI]: https://pypi.org/project/recallgraph/

[D-LS-RECALLGRAPH-SAME-NAME]: https://github.com/Indhar01/RecallGraph
