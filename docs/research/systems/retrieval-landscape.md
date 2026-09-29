# 文書 RAG・retrieval・構築 framework の追加調査

[調査トップ](../README.md) / [横断比較](README.md) / [資料台帳](../sources.md)

調査基準日: 2026-09-29。公式 README、公式ドキュメント、論文、および3リポジトリの commit 固定コードを確認した。コードの静的読解以外の実験・導入・ベンチマーク再現は行っていない。性能・規模・精度の数値や「高精度」「production-ready」等は供給者または論文の主張として扱い、採用判断へ転用していない。

対象は15系統。製品一式、部品化 framework、グラフ検索、視覚ページ retrieval、memory-assisted retrieval は同じ種類の候補ではない。Microsoft GraphRAG、LightRAG、HippoRAG、RAPTOR、KAG は既存調査の比較軸として使い、今回の再調査対象には数えない。既存の説明は[文書のグラフ検索と企業向け知識基盤](knowledge-retrieval.md)、PageIndex の固有仕様は[PageIndex 調査](pageindex.md)、検索部品と実装境界は[実装候補](../implementation/infrastructure.md)を参照。

**今回確認した15系統の役割**

|                           | 役割と検索対象                                             | 更新・権限の境界                                           | PageIndexとの関係                                |
| ------------------------- | --------------------------------------------------- | -------------------------------------------------- | -------------------------------------------- |
| RAGFlow                   | 文書解析・chunk・citationをまとめるself-host RAG engine        | source同期と文書削除の経路がある。完全な原子性は未確認                     | corpus retrievalの外側と文書内tree案内を組み合わせる候補       |
| Dify Knowledge            | AI app用knowledge base、chunk・metadata retrieval      | 文書/chunk管理とKB permission。原システムACLとは分けて確認           | KB管理と長文内evidence locatorは別責務として組み合わせ可能       |
| Onyx                      | 社内app横断のconnector検索・assistant                       | 原システムpermission同期はEnterprise。Liteはindexなし          | corpus discoveryとACL同期の層から適格文書を渡す構成          |
| LlamaIndex                | RAG/agent用node・index・retriever framework            | ref-doc upsertと選択可能なdelete戦略。ACLはstore/アプリ側        | retriever/pipelineへ文書内tree locatorを組む設計余地    |
| Haystack                  | Document Storeとpipelineによる組み替え                      | CRUD interfaceあり。metadata filterはACL保証でなくbackend依存 | pipelineに別retrieverとして文書内tree locatorを加える余地  |
| txtai                     | dense/sparse index、graph、workflowをまとめるframework     | ID upsert/deleteあり。利用者単位の認可は別層                     | corpus候補選びと長文内tree検索を組み合わせる候補                |
| Pathway / llm-app         | live data同期型のRAG template / stream処理                | connector更新・削除同期を主張。BSL条件と永続化を確認                   | live corpus更新後の文書内検索に追加できる                   |
| R2R                       | REST API中心の文書RAG・hybrid・graph                       | user/access controlの宣言から細粒ACLを推定しない                | retrieval APIの補完候補。直接統合は未確認                  |
| Neo4j GraphRAG Python     | graph DB上のvector/hybrid/traversal/KG SDK            | KG builderはexperimental。DB ACL・document CRUDは別     | corpus-wide関係探索とsingle-document tree検索は別役割   |
| FalkorDB GraphRAG-SDK     | documentからgraphを作りsource chunkを結ぶSDK                | provenance edgeは削除・ACL伝播保証ではない                     | graph根拠から文書内位置へ戻す補完案                         |
| FastGraphRAG              | graphとpersonalized PageRankの検索                      | incremental-updateはREADME claim、未検証                | PageIndexのPageRankとは別物。tree traversalと目的が異なる |
| nano-graphrag             | 小さく読みやすいGraphRAG実装                                  | prototype/比較軸。運用ACL・source lifecycleは未確認           | graph検索の比較対象で、文書tree検索とは異なる                  |
| RAG-Anything              | LightRAG上のtext/image/table/equation等のmultimodal RAG | backend変更時にmodal entity再処理が必要になり得る                 | multimodal抽出・根拠を文書内locatorへ補う                |
| Byaldi / ColPali・ColQwen2 | PDFページ画像のvisual retrieval                           | pre-release・非圧縮index・GPU推奨                         | visual page retrievalを適格PDFで比較・補完            |
| MemoRAG                   | global memoryからquery-specific clueを作る研究RAG          | 業務memoryのsource ACL/update/deleteは未確認              | 曖昧な問いからcorpus候補を探す補助案。統合未確認                  |

## 製品としての文書取り込みと横断検索

**RAGFlow** は自己ホスト可能な RAG engine で、複雑な文書の解析、chunk可視化、引用表示、複数検索の融合、Agent機能をまとめる。公式資料では Confluence、Notion、Drive 等の同期と、設定を有効にした場合に外部で削除された内容を知識ベースからも削除する機能を説明している。[D-LR-RAGFLOW-README] [D-LR-RAGFLOW-CONFIG] [D-LR-RAGFLOW-SYNC]

commit `d64b84c7095b1edb81987bbf82cdc95b36a28cbf` の `internal/service/document/document_crud.go` では、dataset access確認、chunk indexの `doc_id` 削除、文書行・カウンタ削除、metadata cleanupと派生物の削除通知が実装されていた。一部の派生物cleanupは警告ログで継続するbest-effort経路だった。削除APIがあることだけから、全consumerの派生データが即時かつ原子的に失効すると推定できない。[C-LR-RAGFLOW-CRUD]

**Dify Knowledge** は knowledge baseをworkflowやappへ接続し、標準/カスタム取り込み、chunkの編集・有効/無効化・アーカイブ、metadata filter、retrieval設定を提供する。文書・chunkの削除は不可逆と説明され、knowledge-base設定にはworkspace member単位のpermissionがある。これは知識QAの管理面を含む製品候補だが、権限は本番構成とeditionを分けて確認する。[D-LR-DIFY-KNOWLEDGE] [D-LR-DIFY-MANAGE] [D-LR-DIFY-SETTINGS] [D-LR-DIFY-README]

固定した `api/controllers/service_api/dataset/dataset.py` では `only_me` / `all_team_members` / `partial_members` と `high_quality` / `economy` の設定、permission変更時の検査が定義されていた。Console用 knowledge filesystem のoperation registryも read/write scope と dataset role を関連付ける。これらのコード確認は権限経路の一部であり、各edition・全API・外部retrieval providerまで含む完全なsecurity auditではない。[C-LR-DIFY-DATASET] [C-LR-DIFY-KBFS]

**Onyx** は複数の社内アプリをconnectorで取り込み、ベクトル＋keyword検索やassistantへつなぐ。connector文書の変更同期を説明し、原システムのユーザーpermissionの尊重はEnterprise edition限定としている。Community EditionはMITでself-hostでき、Cloudも提供されるが、Lite modeは文書indexを持たない。コネクタ数の多さより、利用中のconnectorで ACL change・削除・再認証がどの遅延で検索面へ届くかを評価する。[D-LR-ONYX-CONNECTORS] [D-LR-ONYX-README]

## 組み立てる framework と同期 pipeline

**LlamaIndex** はデータ読込・変換・index・retriever・response synthesisを組み合わせる framework。document management docsは多くのindexでinsert/delete/update/refreshを説明し、`ref_doc_id` を元documentに結びつくnode管理へ使う。固定したingestion pipelineでは、persisted docstoreのhashとIDで更新対象を見つけ、任意の `UPSERTS_AND_DELETE` 戦略では入力からなくなったref-docもdocstoreとvector storeから削除する。戦略の選択とstore adapterの実際の削除対応が前提で、ACLはアプリ・storeのfilterで設計する。[D-LR-LLAMA-DOCS] [D-LR-LLAMA-INGESTION] [C-LR-LLAMA-PIPELINE]

LlamaCloudはparse・extract・index・retrievalを提供する別のmanaged / self-hosted serviceとして案内される。OSS frameworkとクラウドサービスのデータ取扱い、料金、デプロイ選択を同一視しない。[D-LR-LLAMACLOUD]

**Haystack** はpipeline orchestrationとDocument Store / Retrieverの抽象化を重視する。document store protocolはID付き文書のwrite/overwrite、filter、deleteを規定する一方、metadata filterの演算子は各backendで違う。フレームワーク自身がアクセス制御を保証するわけではなく、権限をretriever後段だけで適用しないようstore側の能力を固定する。[D-LR-HAYSTACK-DOCSTORE] [D-LR-HAYSTACK-FILTER] [D-LR-HAYSTACK-PIPELINES] [D-LR-HAYSTACK-REPO]

**txtai** はPythonのall-in-one framework。embedding databaseにdense/sparse index、graph、relational storeを組み合わせ、workflowやAPIとして動かせる。公式index guideの `upsert` はID指定の挿入/更新、`delete` はID削除を説明する。小規模な組込みや自前pipelineには柔軟だが、tenant ACL・元ファイル同期・監査を完成済みの製品機能とみなさない。[D-LR-TXTAI-OVERVIEW] [D-LR-TXTAI-INDEX]

**Pathway / llm-app** はデータが変わると索引を追従させるtemplate群とstream processing framework。公式llm-app READMEはfilesystem、Drive、SharePoint、S3、Kafka、PostgreSQL等の追加・更新・削除同期と、vector/hybrid/full-text searchを記載する。Pathway CommunityはBSL 1.1のsource-available licenseで、制限付きのproduction grant、resource limit、hosted service除外等がある。従って「GitHubに公開＝Apache/MITの自由なhosted再販」と扱わず、採用版の条件・必要connector・永続化・削除伝播を確認する。[D-LR-PATHWAY-APP] [D-LR-PATHWAY-LICENSE]

**R2R** はREST API/SDKで文書取り込み、semantic＋keyword search、citation付きRAG、knowledge graph、analyticsをまとめるOSS候補。公式READMEはlocal Docker/self-hostを案内し、guideはuser management/access controlを掲げる。宣言された機能から per-document ACLの粒度、外部providerへ送るpayload、失効・削除完了を推定しない。これらは API境界の確認項目にする。[D-LR-R2R-README] [D-LR-R2R-GUIDE]

## GraphRAG と異なる retrieval architecture

**Neo4j GraphRAG for Python** は Neo4j向け first-party SDKであり、Vector/Hybrid/Vector-Cypher/Text2Cypher等のretrieverと実験的KG builderを持つ。builderはloader、splitter、文書/chunkをつなぐlexical graph、schema guided extraction、entity resolutionなどを組み合わせる。現在のdocumentに実験的と明記され、graph DBを運用する必要がある。graph traversalが必要なcross-document QAへ比較するが、SDK単独をconnector、source ACL、文書更新台帳の代替とみなさない。[D-LR-NEO4J-README] [D-LR-NEO4J-RAG] [D-LR-NEO4J-KGBUILDER]

**FalkorDB GraphRAG-SDK** はschemaを使ってdocumentからentity/relation graphを作り、自然言語queryから回答を得るPython SDK。READMEは `MENTIONED_IN` 等のsource chunkへのprovenance edgeを明示するため、claimから元証拠へ戻る設計の参考になる。READMEの精度・ベンチマーク優位表現は供給者主張として扱い再現していない。根拠edgeを保持することと、chunk・graph・回答cacheの削除やACLを一貫して伝播する保証は別である。[D-LR-FALKOR-README] [D-LR-FALKOR-START]

**FastGraphRAG** は graphとpersonalized PageRankによる探索を組み合わせる軽量framework。READMEはincremental updateをうたうが、今回その運用保証や費用・精度を測っていない。名前に Page が含まれても、ここでのPageRankはグラフ上の候補展開で、PageIndexの文書階層を辿る処理ではない。**nano-graphrag** は小さく読めるGraphRAG実装で、素早い改変とアルゴリズム比較に向く。両者ともsource ACL、運用監査、削除後の全派生物除去まで備えたenterprise productとして選定しない。[D-LR-FASTGRAPH] [D-LR-NANO]

## 視覚資料と memory-assisted retrieval

**RAG-Anything** はLightRAGをbackendとして、テキスト、画像、表、数式などを横断的に処理するmultimodal pipeline。既存LightRAGと別の一般purpose graph storeとして数を水増しせず、「画像・表・数式を読む解析層」として位置づける。READMEはstorage backendを変えた時、処理済み文書のmultimodal entity/relationを新backendに再生成するには明示的なforce再処理が必要と説明する。出典ページ座標と図表自体の根拠を保持できるかを試験する価値がある。[D-LR-RAGANYTHING] [P-LR-RAGANYTHING]

**Byaldi（ColPali / ColQwen2）** はPDFをページ画像として扱うlate-interaction visual retriever。結果に `doc_id` と1始まりの `page_num` を返し、表・図・レイアウトが本文テキスト化で壊れる時のページ候補生成を補える。Byaldi README自身がpre-release、非圧縮index、GPUなしではencodeが遅いと記す。これは検索器であり回答・利用者認可・ソース同期を含むRAG製品ではない。選んだページを別のVLM/QA段へ渡し、引用根拠を確認する必要がある。[D-LR-BYALDI] [P-LR-COLPALI]

**MemoRAG** は長い入力をmemory modelで俯瞰し、query固有のclueやsurrogate queryを作り、それをpassage retrieverに渡す研究framework。標準のquery-document類似度で拾いにくい曖昧要求を扱う案だが、「memory」はここでは検索用の圧縮・clue生成表現であり、ユーザーの権限、source ID、訂正履歴を管理する長期業務memoryではない。論文とREADMEの性能・最大contextの主張は原著条件に依存し、今回再測定していない。[D-LR-MEMORAG] [P-LR-MEMORAG]

## PageIndexとの組み合わせと選定への含意

今回の比較では、PageIndexを文書ごとの階層構造と本文ページをたどる文書内検索として扱う。今回の候補には、source connectorとdocument CRUDを持つ製品、複数文書の候補を拾うtext/vector retriever、entity relationによるmulti-hop探索、画像ページ単位の視覚検索、global clue生成がある。問題の単位が違うので、最初からどれか一つに集約しない。

運用経路は、まず利用権限のあるcorpusと出典IDで候補文書を選び、通常のテキスト検索・metadata filter、または既知documentの直接取得を基準線にする。適格な長文PDFはPageIndexを文書内のevidence locatorとして比較し、ページ/段落の引用を残す。画像・表・数式が落ちるPDFだけをByaldiやRAG-Anything等の視覚parser/retriever候補へ回し、複数資料を横断する関係質問に限ってgraph方式を追加する。全文の意味抽出やknowledge graph構築を文書登録の必須gateにしない。

source\_id → document revision → page/chunk/span → extracted claim・graph node/edgeという対応を保持し、元文書の更新・削除、tenant/ACL変更、再索引を同一の失効契約でテストする。検索器のmetadata filter、graphのprovenance edge、citation表示はそれぞれ役に立つが、それだけで権限隔離・証拠の真偽・派生物の撤回完了を保証しない。比較実験ではretrieval qualityだけでなく、connector同期遅延、削除後の再露出、出典位置の精度、索引更新費用も測る。

今回確認した15系統すべてについて、数値主張の条件差を揃えた再現benchmarkは実施していない。実装選択は[共通評価方法](../evaluation.md)に沿って、既知documentとcorpus検索を別々に測る。

## 関連資料

- [文書のグラフ検索と企業向け知識基盤](knowledge-retrieval.md)
- [PageIndex 調査](pageindex.md)
- [実装候補](../implementation/infrastructure.md)
- [設計方向と評価仮説](../design-directions.md)
- [共通評価方法](../evaluation.md)

[D-LR-RAGFLOW-README]: https://github.com/infiniflow/ragflow/blob/d64b84c7095b1edb81987bbf82cdc95b36a28cbf/README.md

[D-LR-RAGFLOW-CONFIG]: https://github.com/infiniflow/ragflow/blob/main/docs/guides/dataset/configuration.md

[D-LR-RAGFLOW-SYNC]: https://github.com/infiniflow/ragflow/blob/main/docs/guides/data_source/data_source_configuration.md

[C-LR-RAGFLOW-CRUD]: https://github.com/infiniflow/ragflow/blob/d64b84c7095b1edb81987bbf82cdc95b36a28cbf/internal/service/document/document_crud.go

[D-LR-DIFY-KNOWLEDGE]: https://docs.dify.ai/en/cloud/use-dify/knowledge/readme

[D-LR-DIFY-MANAGE]: https://docs.dify.ai/en/cloud/use-dify/knowledge/manage-knowledge/maintain-knowledge-documents

[D-LR-DIFY-SETTINGS]: https://docs.dify.ai/en/cloud/use-dify/knowledge/manage-knowledge/introduction

[D-LR-DIFY-README]: https://github.com/langgenius/dify

[C-LR-DIFY-DATASET]: https://github.com/langgenius/dify/blob/e6f1d77ed5869e659e426b5459439d44bdabdbe5/api/controllers/service_api/dataset/dataset.py

[C-LR-DIFY-KBFS]: https://github.com/langgenius/dify/blob/e6f1d77ed5869e659e426b5459439d44bdabdbe5/api/services/knowledge_fs_operations.py

[D-LR-ONYX-CONNECTORS]: https://docs.onyx.app/overview/core_features/connectors

[D-LR-ONYX-README]: https://github.com/onyx-dot-app/onyx

[D-LR-LLAMA-DOCS]: https://developers.llamaindex.ai/python/framework/module_guides/indexing/document_management/

[D-LR-LLAMA-INGESTION]: https://developers.llamaindex.ai/python/framework/module_guides/loading/ingestion_pipeline/

[D-LR-LLAMACLOUD]: https://github.com/run-llama/llama_index/blob/main/docs/src/content/docs/framework/index.md

[C-LR-LLAMA-PIPELINE]: https://github.com/run-llama/llama_index/blob/9ca9664a7c3ddb8216edc5df0941be40aa63af2e/llama-index-core/llama_index/core/ingestion/pipeline.py

[D-LR-HAYSTACK-DOCSTORE]: https://docs.haystack.deepset.ai/docs/document-store

[D-LR-HAYSTACK-FILTER]: https://docs.haystack.deepset.ai/docs/metadata-filtering

[D-LR-HAYSTACK-PIPELINES]: https://docs.haystack.deepset.ai/docs/pipelines

[D-LR-HAYSTACK-REPO]: https://github.com/deepset-ai/haystack

[D-LR-TXTAI-OVERVIEW]: https://neuml.github.io/txtai/index.html

[D-LR-TXTAI-INDEX]: https://neuml.github.io/txtai/embeddings/indexing/

[D-LR-PATHWAY-APP]: https://github.com/pathwaycom/llm-app

[D-LR-PATHWAY-LICENSE]: https://pathway.com/license

[D-LR-R2R-README]: https://github.com/SciPhi-AI/R2R

[D-LR-R2R-GUIDE]: https://github.com/SciPhi-AI/R2R/blob/main/docs/introduction/guides/what-is-r2r.md

[D-LR-NEO4J-README]: https://github.com/neo4j/neo4j-graphrag-python

[D-LR-NEO4J-RAG]: https://neo4j.com/docs/neo4j-graphrag-python/current/user_guide_rag.html

[D-LR-NEO4J-KGBUILDER]: https://neo4j.com/docs/neo4j-graphrag-python/current/user_guide_kg_builder.html

[D-LR-FALKOR-README]: https://github.com/FalkorDB/GraphRAG-SDK

[D-LR-FALKOR-START]: https://github.com/FalkorDB/GraphRAG-SDK/blob/main/docs/getting-started.mdx

[D-LR-FASTGRAPH]: https://github.com/circlemind-ai/fast-graphrag

[D-LR-NANO]: https://github.com/gusye1234/nano-graphrag

[D-LR-RAGANYTHING]: https://github.com/HKUDS/RAG-Anything

[P-LR-RAGANYTHING]: https://arxiv.org/abs/2510.12323

[D-LR-BYALDI]: https://github.com/AnswerDotAI/byaldi/blob/main/README.md

[P-LR-COLPALI]: https://arxiv.org/abs/2407.01449

[D-LR-MEMORAG]: https://github.com/qhjqhj00/MemoRAG

[P-LR-MEMORAG]: https://arxiv.org/abs/2409.05591
