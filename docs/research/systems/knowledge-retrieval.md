# 文書のグラフ検索と企業向け知識基盤

[調査トップ](../README.md) / [システム比較](README.md) / [資料台帳](../sources.md)

## PageIndex

PageIndex は文書ごとの階層 tree を作り、LLM agent が章節の構造をたどって必要なページ本文を取得する retrieval 方式である。文書内の階層検索が中心で、実体・関係を結ぶ corpus-wide knowledge graph とは目的が異なる。固定版の SDK、Flash indexing、保存方式と未確認点は[PageIndex の調査](pageindex.md)を参照。

## Microsoft GraphRAG

文書を chunk に分け、entity / relationship を抽出し、community を作り、その要約を検索・回答に利用する。global search はコーパス全体のテーマ、local search は実体近傍、DRIFT は community 情報を利用して局所探索を広げる。大きな資料集合へ「全体としてどんな傾向があるか」と問う用途が出発点である。[P-MSGRAPH] [D-MSGRAPH]

確認した現行本体 package は 3.2.0。`cluster_graph.py` は hierarchical Leiden を用い、workspace は LLM・chunking・storage・vectors を別 package に分ける。`update_entities_relationships.py` には増分 workflow があるため、「GraphRAG は更新できない」と分類するのは不正確。[C-MSCLUSTER] [C-MSUPDATE]

**強み。** chunk 単位の類似検索では拾いにくいコーパスの構造を回答へ利用できる。**制約。** コミュニティ要約を再利用するほど、原文変更時にどの要約を再計算するかが重要になる。増分追加があることは、遡及訂正や全派生物の完全削除が解決済みという意味ではない。小さな個人メモリーでは索引作成費用が見合うか別途測る。

## LightRAG

論文は low-level の実体と high-level の関係・テーマを組み合わせる。現行実装は chunk、entity、relation の索引と複数の query mode を持つ。README は文書削除時に索引作成時の LLM cache を用いて影響した実体・関係を再構築する方針を説明している。[P-LIGHT] [D-LIGHT] [C-LIGHT]

**強み。** 共有エンティティを消し過ぎず、削除した資料に由来する記述を引き直す設計は、メモリーの撤回に参考になる。**制約。** entity/relation と元 chunk の対応を失うと再構築が不正確になる。現行 docs は source ID 数や説明更新に上限設定があることを説明しており、長期運用でこの上限が来歴に与える影響を確認する必要がある。

既定の JsonKV / NanoVectorDB / NetworkX / JsonDocStatus は小規模用で、全体がプロセス RAM に載るという説明がある。また embedding を変えると chunk・entity・relation の再埋め込みが必要。導入の容易さと、本番で使えるデータ量を区別する。[D-LIGHT]

## HippoRAG 2

初代は OpenIE で作る graph と Personalized PageRank（PPR）を利用。HippoRAG 2 は passage node を graph に組み込み、質問から検索した triples を LLM の recognition memory で選別し、passage と phrase を seed として PPR を回す。関連の連鎖を使いながら、単純な fact retrieval の劣化を抑える設計である。[P-HIPPO] [P-HIPPO2] [C-HIPPO]

**強み。** 問いに直接似ていない中継事実を経由した多段 retrieval。**制約。** PPR の到達確率は主張が真である確率ではない。OpenIE の誤り、別名リンク、質問からの seed の誤りが検索を左右する。関係経路を見つける処理と、相反する知識を採否する処理を区別して組み込む。

## RAPTOR と KAG

| 方式            | 仕組み                                                 | 取り入れると有効な場面           | 限界                                                                     |
| ------------- | --------------------------------------------------- | --------------------- | ---------------------------------------------------------------------- |
| RAPTOR        | chunk の embedding・clustering・要約を再帰的に繰り返し、複数抽象度の木を作る | 長い文書の局所情報と全体要約を使い分ける  | 要約の誤りや欠落が上位へ伝播する。変更時の再計算が必要。[P-RAPTOR]                                 |
| KAG / OpenSPG | 知識と原文 chunk の相互索引、型・意味整合、logical form に導かれる検索と推論    | 業務語彙があり、関係制約や計算を伴う QA | schema 整備と問いの分解が必要。LLM が作る logical form を無条件に正解とはみなせない。[P-KAG] [D-KAG] |

## 既存の企業向け知識基盤から学べる点

**Palantir Foundry Ontology。** object、link、action を業務操作へ接続する。action はオブジェクトの状態を変更するトランザクションとして扱われる。ここでいう ontology は OWL 公理集と同義ではなく、データと操作をつなぐ業務層として見るべきである。会話の意味を抽出する技術より、型に従って安全に状態を変える設計が参考になる。[D-FOUNDRY]

**Stardog。** RDFS/OWL schema とルールを使った問い合わせ時の推論、既存 DB を RDF として参照する virtual graph を提供する。すべての業務データをメモリーへコピーする前に、必要な時に権威ある元システムへ照会する選択肢を与える。問い合わせ時の可用性、遅延、時間スナップショットの一貫性が制約になる。[D-STARDOG] [D-VIRTUAL]

**TerminusDB / XTDB。** 前者は JSON データの履歴・branch・diff、後者は system time / valid time をデータベースの機構として持つ。両方とも「LLM が何を信じるべきか」は自動決定しないが、訂正・監査を支える基盤として比較する価値がある。Git 的な履歴と双時間は異なる保証である。[D-TERMINUS] [D-XTDB]

## 関連ドキュメント

- [オントロジーと知識表現](../foundations/ontology.md)
- [検索・グラフ基盤の選択肢](../implementation/infrastructure.md)
- [共通の評価方法](../evaluation.md)

[P-MSGRAPH]: https://arxiv.org/abs/2404.16130v2

[D-MSGRAPH]: https://microsoft.github.io/graphrag/query/overview/

[C-MSCLUSTER]: https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag/graphrag/index/operations/cluster_graph.py

[C-MSUPDATE]: https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag/graphrag/index/workflows/update_entities_relationships.py

[P-LIGHT]: https://arxiv.org/abs/2410.05779v3

[D-LIGHT]: https://github.com/HKUDS/LightRAG/blob/453dce83d6d0354a06e46c8d4029a0895c4e054b/README.md

[C-LIGHT]: https://github.com/HKUDS/LightRAG/blob/453dce83d6d0354a06e46c8d4029a0895c4e054b/lightrag/operate.py

[P-HIPPO]: https://arxiv.org/abs/2405.14831v3

[P-HIPPO2]: https://arxiv.org/abs/2502.14802v2

[C-HIPPO]: https://github.com/OSU-NLP-Group/HippoRAG/blob/1438aba3fc44ff10573e5a5e1e7cc3c7f9794aff/src/hipporag/HippoRAG.py

[P-RAPTOR]: https://arxiv.org/abs/2401.18059v1

[P-KAG]: https://arxiv.org/abs/2409.13731v3

[D-KAG]: https://github.com/OpenSPG/KAG

[D-FOUNDRY]: https://www.palantir.com/docs/foundry/object-edits/overview

[D-STARDOG]: https://docs.stardog.com/inference-engine/

[D-VIRTUAL]: https://docs.stardog.com/virtual-graphs/

[D-TERMINUS]: https://terminusdb.org/docs/version-controlled-json/

[D-XTDB]: https://docs.xtdb.com/concepts/key-concepts.html
