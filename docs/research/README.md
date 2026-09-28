# AI 外部メモリーと知識更新の先行研究

[リポジトリトップ](../../README.md) / [ドキュメントガイド](../README.md)

**実装・文献の再調査: 2026-09-28 / 基礎・応用の追加調査: 2026-09-29 / 対象: 会話・文書・ツール結果から知識を蓄積し、根拠を保ちながら更新するシステム。**

本調査は、実装方式、知識表現、更新意味論、検索、評価を横断して比較する。主要実装はコミットを固定して静的に読んだ。論文の実験を再実行した結果ではない。確認方法と未確認範囲は[調査方法](methodology.md)、出典と版は[資料台帳](sources.md)に記載した。

## 基本から応用までの学習ガイド

2026-09-29 に概念の基礎と応用を追加調査した。**オントロジーは意味を共有するための概念・関係・公理、メモリーは情報を保持して再利用するための表現と管理機構**として読み分ける。型・制約・来歴・更新方針を区別すると、両者をどこで組み合わせるべきかが明確になる。

| 段階    | 文書                                                                  | 読んだ後に説明できること                        |
| ----- | ------------------------------------------------------------------- | ----------------------------------- |
| 入門    | [メモリー、知識、オントロジーの関係](foundations/concepts.md)                        | 原資料、主張、意味の定義、検索・回答の違い               |
| 基礎〜中級 | [オントロジーと知識表現](foundations/ontology.md)                              | クラス・個体・関係・公理、RDF/OWL/SHACL、意味論と設計工程 |
| 基礎〜中級 | [メモリーの基礎と応用](foundations/memory.md)                                 | 記憶の分類、モデル内部と外部、RAG、書き込み・読み出し・忘却     |
| 中級    | [知識更新の理論と実装](foundations/knowledge-updates.md)                      | 世界の変化と訂正、双時間、根拠の撤回、削除               |
| 応用    | [オントロジーとメモリーを組み合わせる実例](foundations/ontology-memory-applications.md) | 原資料から主張・SHACL・検索・訂正をつなぎ、用途別に構成を選ぶ   |

入門から順に読み、実装候補は[システム比較](systems/README.md)、効果の測り方は[評価方法](evaluation.md)、具体的な技術スタックは[推奨設計](../design/ontology-knowledge-system.md)へ進む。本文の例示・設計判断と、原論文・仕様で確認した事実を区別して記載した。

## 目的別の読み方

| 目的                 | 読む順序                                                                                                                                                                                              |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| オントロジー・知識システムを構築する | [推奨設計と用途別構成](../design/ontology-knowledge-system.md) → [設計・実験案](design-directions.md) → [評価方法](evaluation.md)                                                                                     |
| 実装候補と未解決点を把握する     | [システム比較](systems/README.md) → [知識更新](foundations/knowledge-updates.md) → [評価方法](evaluation.md) → [設計・実験案](design-directions.md)                                                                   |
| 基礎から理解する           | [基本概念](foundations/concepts.md) → [オントロジー](foundations/ontology.md)・[メモリー](foundations/memory.md) → [知識更新](foundations/knowledge-updates.md) → [応用例](foundations/ontology-memory-applications.md) |
| 実装に使う技術を検討する       | [システム比較](systems/README.md) → [実装の依存関係](implementation/dependencies.md) → [周辺ライブラリと基盤](implementation/infrastructure.md)                                                                          |
| 研究・比較実験を計画する       | [研究の系譜](papers.md) → [評価方法](evaluation.md) → [設計・実験案](design-directions.md)                                                                                                                       |
| 判断の根拠を確認する         | [調査方法](methodology.md) → [資料台帳](sources.md) → [根拠データの読み方](evidence/README.md)                                                                                                                     |

## 調査から得られた判断

1. **「メモリー」という名称だけでは比較できない。** 会話から事実を抽出する Mem0、時間付きエッジを更新する Graphiti、作業文脈を編集する Letta、文書集合を要約する GraphRAG は、それぞれ異なる問題を解く。検索・保存・知識更新・行動改善を個別に評価する。[C-M0] [C-GEDGE] [C-LMEM] [D-MSGRAPH]
2. **論文時点の方式と現在の製品を区別する必要がある。** 調査した Mem0 2.2.1 の `add` は追加中心の処理で、2025 年論文の ADD/UPDATE/DELETE/NOOP と同一ではない。Letta の旧 API server は退役扱いで、現行開発先は `letta-code` へ移動している。[P-M0] [C-M0] [C-LMOV]
3. **オントロジーは更新方針を明確にできるが、導入だけで正確性が上がるとは限らない。** 2026 年の Fortunate Recall は型別のライフサイクル管理を扱う一方、型を取り除いた比較から、改善の一部を汎用的な失効・置換メタデータへ帰属させている。型体系の効果と、時間・撤回処理の効果を切り分けて測るべきである。[P-FR]
4. **検索結果の良さと、更新の正しさは別の能力である。** HaluMem は抽出・更新・回答を分解し、MemoryArena は過去の経験を次の行動に使えるかを測る。会話 QA だけでは今回の目標を十分評価できない。[P-HALU] [P-ARENA]
5. **有望な研究対象は、根拠と変更理由を保った知識更新である。** 双時間、真理維持、出典追跡、語彙制約を組み合わせる余地がある。ただしこれは調査からの設計仮説であり、新規性や性能が実証されたという意味ではない。[T-TMS] [T-PROVENANCE] [D-XTDB]

## 文書の構成

### 基礎と理論

| 文書                                                             | 内容                                         |
| -------------------------------------------------------------- | ------------------------------------------ |
| [基本概念と歴史](foundations/concepts.md)                             | 初学者向けの定義・用語・学習順序、原資料と主張、RAG・認知アーキテクチャ      |
| [オントロジーと標準](foundations/ontology.md)                           | クラス・個体・公理から RDF/OWL/SHACL、設計・検証・進化と応用まで    |
| [メモリーの基礎と応用](foundations/memory.md)                            | 人間の記憶との類比、機械の記憶の分類、ライフサイクル、代表研究、評価         |
| [オントロジーとメモリーの応用例](foundations/ontology-memory-applications.md) | 所属情報を題材にした RDF・SHACL・SPARQL、訂正・撤回、用途別の設計判断 |
| [知識更新の理論と実装](foundations/knowledge-updates.md)                 | 信念改訂、真理維持、双時間、撤回、実体統合、同時更新                 |

### システム比較

| 文書                                            | 内容                                                           |
| --------------------------------------------- | ------------------------------------------------------------ |
| [システム比較の一覧](systems/README.md)                | 各方式の比較軸と詳細文書への案内                                             |
| [事実抽出・個人化メモリー](systems/fact-memory.md)        | Mem0、LangMem、Memobase、SimpleMem、A-MEM                        |
| [時間・グラフ・統合メモリー](systems/structured-memory.md) | Graphiti、Cognee、Hindsight、MemOS、EverMemOS                    |
| [エージェント・ファイル・サービス](systems/agent-memory.md)   | Letta、Basic Memory、MCP Memory、Honcho、Supermemory、Mastra、クラウド |
| [文書グラフと知識基盤](systems/knowledge-retrieval.md)  | GraphRAG、LightRAG、HippoRAG 2、RAPTOR、KAG、企業向け基盤               |
| [PageIndex：文書内の階層検索](systems/pageindex.md)    | 文書ごとの階層 tree と、LLM が章節・ページを辿る retrieval                      |
| [追加候補](systems/additional-systems.md)         | MemoryOS、MemMachine、memU、OpenViking、MIRIX                    |

### 実装基盤

| 文書                                                | 内容                       |
| ------------------------------------------------- | ------------------------ |
| [実装の依存関係](implementation/dependencies.md)         | パッケージの役割、必須・任意依存、版とライセンス |
| [周辺ライブラリと基盤の選択](implementation/infrastructure.md) | 抽出、検索、推論、DB、データ取り込み、観測   |

### 論文・評価・設計

| 文書                                                             | 内容                                 |
| -------------------------------------------------------------- | ---------------------------------- |
| [研究の系譜と最近の論文](papers.md)                                       | 2023–2026 年の研究、主要仮説、本文から分かる限界      |
| [評価方法](evaluation.md)                                          | ベンチマーク、報告値の読み方、再現条件、独自試験           |
| [設計の選択肢と研究計画](design-directions.md)                            | 代替アーキテクチャ、検証仮説、段階的な比較実験            |
| [オントロジー・ナレッジシステムの推奨設計](../design/ontology-knowledge-system.md) | 調査の適用先。推奨スタック、構成、用途別の設計、データ契約、実装順序 |

### 調査方法と根拠

| 文書                              | 内容                            |
| ------------------------------- | ----------------------------- |
| [調査方法・再調査での修正](methodology.md)  | 採否基準、根拠の種類、版の固定、検証範囲          |
| [資料台帳](sources.md)              | 論文、標準、公式文書、確認したコードへのリンク       |
| [根拠データの読み方](evidence/README.md) | JSON 台帳の役割、主要項目、本文の参照 ID との対応 |

機械可読な記録は [repository-snapshots.json](evidence/repository-snapshots.json)、[dependency-inventory.json](evidence/dependency-inventory.json)、[sources.json](evidence/sources.json) に置いた。依存一覧は取得した manifest の宣言を保存しており、対象環境で解決済みの lockfile や完全な SBOM ではない。

[C-M0]: https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/mem0/memory/main.py

[C-GEDGE]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/edges.py

[C-LMEM]: https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/src/agent/memory-filesystem.ts

[D-MSGRAPH]: https://microsoft.github.io/graphrag/query/overview/

[P-M0]: https://arxiv.org/abs/2504.19413v1

[C-LMOV]: https://github.com/letta-ai/letta/blob/5bcdd177d70fa2b31a754cfcd801e77b2e1ab16a/README.md

[P-FR]: https://arxiv.org/abs/2609.10413v1

[P-HALU]: https://arxiv.org/abs/2511.03506v3

[P-ARENA]: https://arxiv.org/abs/2602.16313v2

[T-TMS]: https://www.sciencedirect.com/science/article/pii/0004370279900080

[T-PROVENANCE]: https://web.cs.ucdavis.edu/~green/papers/pods07.pdf

[D-XTDB]: https://docs.xtdb.com/concepts/key-concepts.html
