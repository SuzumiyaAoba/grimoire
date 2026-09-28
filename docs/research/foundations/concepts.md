# メモリー、知識、オントロジーの関係

[調査トップ](../README.md) / [資料台帳](../sources.md)

## 今回のシステム境界

外部メモリーとは、モデルの一回の呼び出しが終わった後にも残り、次の判断に利用できる情報とその管理機構である。保存データだけでなく、選別、抽出、統合、検索、削除の方針も含めて考える。既存研究では形態・機能・変化の仕方が混在して「memory」と呼ばれるため、三つを独立に整理する。[P-SURVEY]

| 軸 | 区分 | 例 |
|---|---|---|
| 表現形態 | テキスト／構造化レコード／グラフ／潜在表現／モデル重み | Markdown、JSON、RDF、KV cache、学習済みパラメータ |
| 用途 | 知識／経験・技能／作業状態 | 居住地、過去の失敗と対処、未完了タスク |
| 時間範囲 | 一呼び出し／一セッション／複数セッション | コンテキスト、checkpoint、永続ストア |
| 更新主体 | アプリ／モデルの tool call／背景処理／人 | バッチ抽出、反省、レビュー |
| 保証 | 保存／検索／履歴／意味的整合性 | DB transaction と主張の真偽は異なる保証 |

## 取り違えやすい概念

**会話ログと意味記憶。** ログは「ユーザーが東京在住と述べた」という出来事を保持する。意味記憶は「現在の居住地は東京」という解釈であり、主語、時点、情報源、確度が加わる。引用、推測、仮定の発言まで事実化すると、この変換で誤りが生じる。

**RAG と長期記憶。** RAG は外部情報を取得して生成に使う手順で、長期記憶の読み出しに利用できる。RAG 自体がユーザー別・時間付き・更新可能な構成を禁じるわけではない。「RAG は必ず静的」「memory は必ず動的」という二分は正確でない。比較すべきは取り込み・更新・読み出しの実際の処理である。[P-HIPPO2] [D-LANGMEM]

**知識グラフとオントロジー。** グラフは実体と関係の表現構造。オントロジーは語彙の意味、公理、型間の関係を規定する。`manages` という文字列をエッジに付けただけでは、OWL の意味論は得られない。一方、OWL ontology には用語だけでなく個体についての公理も含められるので、「オントロジーにはインスタンスを一切含まない」とも定義しない。[S-OWL]

**DB schema と論理的制約。** JSON Schema/Pydantic は出力構造を、SHACL は RDF データの形を検査する。OWL reasoner は含意と整合性を扱う。いずれも入力文が現実に正しいことを保証する機構ではない。[S-SHACL] [S-OWL]

**context engineering と memory management。** 文脈に何を入れるかを調整することと、長期の正本を変更することは独立。Mastra OM は観察ログを文脈へ継続配置し、ACE は経験から得た方略を増分編集する。この二つも同じ記憶操作ではない。[D-MASTRA] [P-ACE]

## 古典的な背景が現在の実装に与える示唆

| 系譜 | 得られる視点 | 外部メモリーへの対応 |
|---|---|---|
| 認知アーキテクチャと CoALA | 作業、意味、エピソード、手続きの役割を分ける | 全情報を一つのテキスト欄へ圧縮しない。[P-COALA] |
| 信念改訂 | 新情報を受けた時、どの信念を残すか | 最新発話で無条件上書きする前に優先順位を定義。[T-AGM] |
| Truth Maintenance System | 結論を支える理由と仮定を追跡する | 根拠を撤回した時に派生結論も再評価。[T-TMS] |
| データベース provenance | 結果と入力の依存関係を表す | 同じソースの転載を独立した支持と数えない。[T-PROVENANCE] |
| 双時間 DB | 世界で有効な期間と、DB がそう認識した期間を持つ | 遅れて届いた訂正と過去時点の再現。[D-XTDB] |
| Zettelkasten／ファイル型知識管理 | 人が編集できるノートとリンクを積み重ねる | 可搬性、レビュー、過去の判断理由を重視。[P-AMEM] [D-BASIC] |

## モデル内部の「記憶」との境界

Titans はテスト時に更新するニューラルメモリー、Engram は n-gram を使った条件付き lookup を研究する。これらはモデルの計算構造を変える研究であり、公開 API の LLM に追加する主張ストアとは実装条件が異なる。KV cache や prompt caching も再計算を減らす手段であって、ユーザーによる事実訂正・出典管理を自動的に提供しない。[P-TITANS] [P-ENGRAM]

外部記憶の設計では、まずモデルを交換しても残せる内容と、特定のモデル・埋め込みに依存する派生データを区別する。埋め込みモデルを変更するなら、原文や構造化主張から再計算できることが重要になる。LightRAG もモデル変更時の再埋め込みの必要性を明記している。[D-LIGHT]

## 関連ドキュメント

- [オントロジーと知識表現](ontology.md)
- [知識更新の理論と実装](knowledge-updates.md)
- [研究の系譜](../papers.md)

[P-SURVEY]: https://arxiv.org/abs/2512.13564v2
[P-HIPPO2]: https://arxiv.org/abs/2502.14802v2
[D-LANGMEM]: https://langchain-ai.github.io/langmem/concepts/conceptual_guide/
[S-OWL]: https://www.w3.org/TR/owl2-primer/
[S-SHACL]: https://www.w3.org/TR/shacl/
[D-MASTRA]: https://mastra.ai/research/observational-memory
[P-ACE]: https://arxiv.org/abs/2510.04618v3
[P-COALA]: https://arxiv.org/abs/2309.02427v3
[T-AGM]: https://doi.org/10.2307/2274239
[T-TMS]: https://www.sciencedirect.com/science/article/pii/0004370279900080
[T-PROVENANCE]: https://web.cs.ucdavis.edu/~green/papers/pods07.pdf
[D-XTDB]: https://docs.xtdb.com/concepts/key-concepts.html
[P-AMEM]: https://arxiv.org/abs/2502.12110v11
[D-BASIC]: https://github.com/basicmachines-co/basic-memory/blob/22d31e96e2defa465d3703620a537c9125e7d364/README.md
[P-TITANS]: https://arxiv.org/abs/2501.00663v1
[P-ENGRAM]: https://arxiv.org/abs/2601.07372v2
[D-LIGHT]: https://github.com/HKUDS/LightRAG/blob/453dce83d6d0354a06e46c8d4029a0895c4e054b/README.md
