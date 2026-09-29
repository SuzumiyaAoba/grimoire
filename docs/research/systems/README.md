# 既存システムの比較地図

[調査トップ](../README.md) / [資料台帳](../sources.md)

同じデータをどの単位で保存し、どの操作で更新するかを比較する。下表は機能の有無を保証する製品チェックリストではなく、各方式の中心を示す。対象版・根拠・未確認点は詳細ページに記載した。

**2026-09-29 の追加:** [関連システムの再調査](extended-landscape.md)で比較範囲を拡大した。以下の既存主要システムに加え、[メモリーと経験](memory-landscape.md)、[文書検索・RAG](retrieval-landscape.md)、[意味データ基盤](semantic-data-landscape.md)、[クラウド・実行基盤](managed-memory-landscape.md)を確認した。新規と概要の深掘りを分け、全候補の資料 ID と未確認点を[候補台帳](../evidence/system-landscape.json)に記録している。

## システム一覧

| システム                      | 記憶の単位                            | 更新の中心                          | 今回の目的への適性                                          | 詳細                             |
| ------------------------- | -------------------------------- | ------------------------------ | -------------------------------------------------- | ------------------------------ |
| Mem0 OSS                  | 抽出テキスト＋実体リンク                     | 現行確認版は追加・重複除去、明示 CRUD          | 軽い個人化の比較基準。論文版との違いが大きい                             | [事実抽出](fact-memory.md)         |
| LangMem                   | 任意の型付き文書・profile                 | Trustcall による抽出・patch・削除候補     | 独自スキーマと方針を実装する部品                                   | [事実抽出](fact-memory.md)         |
| Memobase                  | ユーザー profile とイベント               | topic ごとの抽出・merge              | 人物情報を管理する用途                                        | [事実抽出](fact-memory.md)         |
| SimpleMem                 | 単独で読める圧縮記憶単位                     | 文脈解決・統合・検索計画                   | 低トークンの対話記憶                                         | [事実抽出](fact-memory.md)         |
| A-MEM                     | 属性とリンクを持つノート                     | 新ノートに応じて既存ノートを進化               | スキーマが流動的な知識組織化                                     | [事実抽出](fact-memory.md)         |
| Graphiti / Zep            | episode、entity、fact edge         | 同定・重複解決・時間付き無効化                | 関係と時間を重視する主要な比較対象                                  | [構造化記憶](structured-memory.md)  |
| Cognee                    | 文書・DataPoint・グラフ                 | 取り込み、型・語彙との照合                  | オントロジーを入力に持つ構成                                     | [構造化記憶](structured-memory.md)  |
| Hindsight                 | facts、observations、mental models | retain・統合・recall・reflect       | 根拠付きの抽象化と複数検索                                      | [構造化記憶](structured-memory.md)  |
| MemOS                     | 異種記憶の MemCube 等                  | 統合、スケジューリング、移動                 | 広範な記憶管理の設計を参照                                      | [構造化記憶](structured-memory.md)  |
| EverMemOS                 | MemCell、MemScene、profile         | エピソードから意味への統合                  | 会話・人物像・予定の継続的更新                                    | [構造化記憶](structured-memory.md)  |
| Letta                     | blocks とファイル記憶                   | agent の編集、Git 管理               | inspect/edit 可能な作業・手続き記憶                           | [エージェント記憶](agent-memory.md)    |
| Basic Memory / MCP Memory | Markdown／小さな graph               | 人または agent の明示書き込み             | 可搬性と低い導入コスト                                        | [エージェント記憶](agent-memory.md)    |
| Honcho                    | peer ごとの結論・人物像                   | 背景推論と consolidation            | 観測者ごとの視点・個人化                                       | [エージェント記憶](agent-memory.md)    |
| Supermemory               | 文書、記憶、profile                    | 公式 API 上の抽出・更新                 | 統合サービスとの比較                                         | [エージェント記憶](agent-memory.md)    |
| Mastra OM                 | 日付付き観察と反省                        | 背景圧縮、常駐文脈の整理                   | グラフを使わない重要な対照方式                                    | [エージェント記憶](agent-memory.md)    |
| PageIndex                 | 文書ごとの階層 tree とページ本文              | 文書を index し、LLM が tree とページを探索 | 長い PDF の章節・ページをたどる retrieval。文書横断の知識グラフや更新管理とは別の問題 | [PageIndex](pageindex.md)      |
| GraphRAG                  | 文書、実体、community report           | 抽出・集約・増分 workflow              | コーパス全体の要約・横断質問                                     | [知識検索](knowledge-retrieval.md) |
| LightRAG                  | chunk、entity、relation            | merge とソース由来の再構築               | 増分投入・文書削除                                          | [知識検索](knowledge-retrieval.md) |
| HippoRAG 2                | passage と phrase のグラフ            | 検索時の seed 選択と PPR              | 関連をたどる多段 QA                                        | [知識検索](knowledge-retrieval.md) |
| RAPTOR / KAG              | 要約木／型付き知識と chunk                 | 階層要約／論理形式による検索                 | 抽象度や制約を使う検索                                        | [知識検索](knowledge-retrieval.md) |

## 詳細ページ

| 分類                                    | 主な対象                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| [関連システムの横断比較](extended-landscape.md) | 役割別の追加候補、PageIndex との配置、採否・保留と確認範囲 |
| [メモリーと経験の追加調査](memory-landscape.md) | OpenViking、MemoryOS、ReMe、Memori、Memvid、手順記憶研究等 |
| [文書検索・RAG の追加調査](retrieval-landscape.md) | RAGFlow、Dify、Onyx、LlamaIndex、Haystack、視覚検索等 |
| [意味データ基盤の追加調査](semantic-data-landscape.md) | GraphDB、RDFox、Stardog、Ontop、TypeDB、XTDB 等 |
| [クラウド・実行基盤の追加調査](managed-memory-landscape.md) | AgentCore、Memory Bank、Foundry、OpenAI、Claude、各 framework |
| [事実抽出・個人化メモリー](fact-memory.md)        | Mem0、LangMem、Memobase、SimpleMem、A-MEM                        |
| [時間・グラフ・統合メモリー](structured-memory.md) | Graphiti、Cognee、Hindsight、MemOS、EverMemOS                    |
| [エージェント・ファイル・サービス](agent-memory.md)   | Letta、Basic Memory、MCP Memory、Honcho、Supermemory、Mastra、クラウド |
| [文書グラフと知識基盤](knowledge-retrieval.md)  | GraphRAG、LightRAG、HippoRAG 2、RAPTOR、KAG、企業向け基盤               |
| [文書内の階層検索：PageIndex](pageindex.md)    | SDK、Flash indexing、ローカル保存、tree を辿る retrieval                 |
| [追加候補と調査範囲](additional-systems.md)    | MemoryOS、MemMachine、memU、OpenViking、MIRIX                    |

## 比較に必須の七つの質問

1. 新しい情報が来た時、何を追加し、どの旧レコードを変えるか。
2. 「旧情報が誤りだった」と「実世界が変わった」を区別できるか。
3. 同定・統合・要約の判断を、入力まで戻って説明できるか。
4. 時刻は単なるソートキーか、有効期間・知識取得時点として使われるか。
5. 型・語彙はプロンプトのヒントか、保存時の制約か、論理推論の公理か。
6. 元データを消した後、要約・実体リンク・索引はどう更新されるか。
7. 応答を生成する agent、保存基盤、抽出モデルを替えても同じ条件で比較できるか。

Graphiti、Cognee、Hindsight のコードはこの問いに具体的な答えを与える一方、どれも本調査だけで全要件を満たすと認定できるものではない。[C-GUPDATE] [C-COGSTRICT] [C-HCONSOL]

## 関連ドキュメント

- [比較の前提となる知識更新の考え方](../foundations/knowledge-updates.md)
- [実装の依存関係](../implementation/dependencies.md)
- [共通の評価方法](../evaluation.md)

[C-GUPDATE]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/utils/maintenance/edge_operations.py

[C-COGSTRICT]: https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/modules/ontology/construct_data_points_and_edges_with_ontology.py

[C-HCONSOL]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/hindsight_api/engine/consolidation/consolidator.py
