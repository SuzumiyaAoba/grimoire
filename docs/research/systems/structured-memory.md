# 時間・グラフ・統合を中心にしたメモリー

[調査トップ](../README.md) / [システム比較](README.md) / [資料台帳](../sources.md)

## Graphiti と Zep

Graphiti は公開ライブラリ、Zep はそれを基盤とするサービスとして扱う。サービスのレイテンシやアクセス制御を、そのまま Graphiti ライブラリの保証にしない。

**データと更新。** `EpisodicNode` が入力の文脈を、entity が同定した実体を、`EntityEdge` が自然言語の fact と関係を保持する。確認した edge には `episodes`、`created_at`、`expired_at`、`valid_at`、`invalid_at` がある。カスタム実体・関係は Pydantic 型を介して定義できる。[C-GEDGE] [C-GMAIN]

取り込み時には実体を照合し、既存の同じ端点のエッジを重複候補に、より広い検索結果を矛盾候補にする。完全一致なら既存 edge に episode を追加し、それ以外は LLM に同一性と矛盾を判定させる。矛盾候補が時間的に重ならなければ無効化を避け、必要な場合に有効区間を閉じる。検索は複数方式と RRF/MMR 等を組み合わせられる。[C-GNODE] [C-GUPDATE] [C-GSEARCH]

**強み。** 「現在の勤務先」と「以前の勤務先」を、入力エピソードと結び付けて表現しやすい。型の指定と時間付き fact の更新は今回の目的に近い。旧エッジを消すだけの方式より、変化を問い合わせやすい。

**制約（静的分析）。** 矛盾候補は検索で選ぶため、取得されなかった矛盾は判定されない。同じ端点・同じ文でも別の時期の出来事かを確認する必要がある。四つの timestamp があることだけで、全変更を immutable revision として保存する双時間 DB 相当の保証は得られない。元 episode の参照は、原文の正確な span までの来歴と同じではない。これらは欠如の断定ではなく、追加試験の対象である。

**依存と運用。** 0.30.2 では Neo4j driver が基礎依存、FalkorDB/Neptune 等は extras。Kuzu は manifest 上で非推奨とされる。取り込みは複数の抽出・照合を含むため、検索 p95 に加えて、入力を渡してから知識が検索可能になるまでを測る。[P-ZEP]、[依存台帳](../implementation/dependencies.md)。

## Cognee

Cognee は ingestion と cognify のパイプラインを持ち、文書、抽出した構造、グラフ、ベクトルを結び付ける。重要なのは「グラフを作れる」だけでなく、既存オントロジーとの照合を行うコードが存在することである。[C-COG]

`RDFLibOntologyResolver` は RDF/OWL を読み、名前照合と subgraph 抽出を行う。strict mode の処理は使用可能なオントロジーが空なら失敗させ、照合の成否とノード・エッジの採否を管理する。このことから、語彙に合わせた抽出結果の制約という用途が確認できる。完全な OWL DL 推論器が常に動作しているという意味ではない。[C-COGONTO] [C-COGSTRICT]

**強み。** 文書抽出と、手元にある業務語彙の橋渡しを試せる。RDFLib、Pydantic、Instructor、LiteLLM 等の利用点も調べやすい。**制約。** fuzzy matching で似たクラスを誤って選ぶ可能性、strict filtering で正しい新概念まで落とす可能性がある。オントロジーとの一致率に加えて、棄却された候補の recall を測る必要がある。DB adapter 間の完全互換も前提にしない。

## Hindsight

論文は world、experience、observation、opinion の四つの論理ネットワークを示し、retain / recall / reflect を分ける。現行 README では world facts、experiences、observations、mental models を説明しており、論文の分類をそのまま API の enum として用いない。[P-HIND] [D-HIND]

**更新と検索の実装。** 現行 consolidator は下位 facts から observation を作り、`source_memory_ids` を維持する。既存 observation の重複・支持・矛盾・精緻化を処理する。検索には semantic、全文、時間、graph の経路があり、fusion では `Σ 1/(k + rank)`、既定 k=60 の RRF を実装している。reflect は取得した記憶を使う推論で、recall より仕事量が多い。[C-HCONSOL] [C-HSEARCH] [C-HFUSION]

**強み。** 保存した根拠から観察・立場・継続的要約を作る層が明示的である。原 facts と抽象化を分ける設計の参考になる。**具体的な制約。** retain の mission が厳し過ぎて一文書から fact がゼロ件になった場合、原文は保存されても recall/reflect では到達できないという説明がある。成功ステータスだけで知識の収録成功と判断しない。[D-HRETAIN]

PostgreSQL と pgvector を中心とする公開実装であるが、確認した 0.10.1 の slim manifest には拡張用 extra もある。すべての構成で同じ検索・抽象化の保証があるとは断定しない。論文の高い QA スコアについては[評価方法](../evaluation.md)を参照。

## MemOS

MemOS は text、activation、parameter という異なる記憶を、MemCube を含む共通の管理対象として扱う構想。単に vector store を増やすだけでなく、記憶資源の読み込み、組織化、スケジューリングを視野に入れる。[P-MEMOS]

調査した repo には Python 本体と複数の TypeScript ローカル plugin が共存する。Python 2.0.33 の manifest は tree-memory 用 Neo4j、scheduler 用 Redis/RabbitMQ、reader 用 Chonkie/MarkItDown、preference memory 用 Milvus/MinHash などを機能別 extra にしている。これらを全て必須 DB と列挙するのは誤り。[D-MEMOS]、[依存一覧](../implementation/dependencies.md)。

**強み。** 記憶をアプリの付属機能ではなく管理可能な資源として考える設計が豊富。**制約。** アーキテクチャの射程が広く、今回必要な「根拠付き主張の更新」にどの実装が必要なのかを絞る必要がある。パラメータ記憶を扱える構想と、任意のモデル API の重みを更新できることは同一ではない。別プロジェクトの **MemoryOS**、**Memobase**、**EverMemOS** と名前で混同しない。

## EverMemOS

論文は、エピソード・atomic facts・期限付き foresight を持つ MemCell を作り、関連するセルを MemScene へまとめ、scene を手掛かりに必要な文脈を再構成する。予定や継続的な関心を、過去の事実と別の用途で扱う点が参考になる。[P-EVER]

現行 1.4.1 の manifest は LanceDB/SQLite 系と `everalgo-*` 群を宣言する。確認した user-memory pipeline も外部 algorithm package を利用している。過去の構成図や論文から現行の必須インフラを推測しない。本調査では `everalgo-*` の全内部実装までは追っていない。[C-EVER] [D-EVER]

**強み。** エピソードから安定した人物像・場面へ階層化する流れ。**制約。** scene の境界や統合で条件が失われないか、foresight の期限を過ぎた時に「予定していた事実」まで消さないかが評価点。論文の正答率だけから個々の更新操作の整合性は判断できない。

## 関連ドキュメント

- [オントロジーと知識表現](../foundations/ontology.md)
- [時間・訂正・根拠撤回の扱い](../foundations/knowledge-updates.md)
- [共通の評価方法](../evaluation.md)

[C-GEDGE]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/edges.py
[C-GMAIN]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/graphiti.py
[C-GNODE]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/utils/maintenance/node_operations.py
[C-GUPDATE]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/utils/maintenance/edge_operations.py
[C-GSEARCH]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/search/search_config.py
[P-ZEP]: https://arxiv.org/abs/2501.13956v1
[C-COG]: https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/api/v1/cognify/cognify.py
[C-COGONTO]: https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/modules/ontology/rdf_xml/RDFLibOntologyResolver.py
[C-COGSTRICT]: https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/modules/ontology/construct_data_points_and_edges_with_ontology.py
[P-HIND]: https://arxiv.org/abs/2512.12818v1
[D-HIND]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/README.md
[C-HCONSOL]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/hindsight_api/engine/consolidation/consolidator.py
[C-HSEARCH]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/hindsight_api/engine/search/retrieval.py
[C-HFUSION]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/hindsight_api/engine/search/fusion.py
[D-HRETAIN]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/skills/hindsight-docs/references/developer/retain.md
[P-MEMOS]: https://arxiv.org/abs/2505.22101v1
[D-MEMOS]: https://github.com/MemTensor/MemOS/blob/a7367d07e55db61099f7b4e2c1108bc5831a24f3/README.md
[P-EVER]: https://arxiv.org/abs/2601.02163v2
[C-EVER]: https://github.com/EverMind-AI/EverMemOS/blob/462ebf9fd59b55c03fefb8eec855c62500f2a3cf/src/everos/memory/extract/pipeline/user_memory.py
[D-EVER]: https://github.com/EverMind-AI/EverMemOS/blob/462ebf9fd59b55c03fefb8eec855c62500f2a3cf/README.md
