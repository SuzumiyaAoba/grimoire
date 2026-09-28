# 関連技術と実装候補

[調査トップ](../README.md) / [資料台帳](../sources.md)

## 入力を知識へ変換する

| 段階 | ライブラリ・技術 | 実装上の判断 |
|---|---|---|
| 文書解析 | Docling、MarkItDown、PyPDF 系 | 文字だけでなく表、見出し、ページ、画像領域を保持。Markdown 化で出典座標が失われないか。[D-DOCLING] [D-MARKIT] |
| コード解析 | Tree-sitter 群 | 関数・class・参照の構文境界で分割。OpenViking の manifest に複数言語 parser がある。[M-OPENV] |
| 日本語分割 | Sudachi.rs / SudachiPy、spaCy 系 | 全半角、敬称、姓・名、辞書版、複合語の粒度。古い SudachiPy repo と現行実装を区別。[D-SUDACHI] [D-SPACY] |
| 言及抽出 | spaCy NER、GLiNER/GLiNER2 | entity mention の抽出と、既存 entity ID への同定を別段階にする。[D-SPACY] [D-GLINER] |
| 型付き抽出 | Pydantic、Instructor、Trustcall、JSON Schema | 構文、型、必須項目を検査。根拠 span と意味的一致は別の検証。[M-COGNEE] [C-LANG] |
| スキーマへの接続 | RDFLib、OntoGPT、LinkML | 既存 IRI に grounding し、新語は候補として扱う。[C-COGONTO] [D-ONTOGPT] |
| 日付解釈 | dateparser、python-dateutil、Pendulum | 基準時刻と timezone を渡す。不明を現在時刻で埋めない。[M-HIND] [M-MEMU] |
| 重複検出 | hash、MinHash/datasketch、embedding、実体照合 | hash は同一表記、MinHash は近似字句、embedding は意味類似。どれも単独で同一事実・同一人物を証明しない。[C-M0] [M-MEMOS] |

LLM provider の切り替えには LiteLLM や各社 SDK が広く使われる。同じ JSON schema を指定しても、抽出 recall や推測の強さ、相対時刻の解決が同等とは限らない。モデル変更時は知識の再抽出と旧版の維持方針を決める。

## 検索アルゴリズム

| 技術 | 解く問題 | 外部メモリーでの制約 |
|---|---|---|
| BM25 / 全文検索 | 固有名詞、識別子、厳密な語彙一致 | 言い換えと暗黙の関係に弱い。日本語 tokenizer に依存 |
| Dense embedding | 意味的な類似 | 否定、数値、時点、同名実体を区別し切れない |
| Sparse learned / multi-vector | 語彙重みや token 単位の対応 | 索引サイズ・計算量が増える。BGE-M3 は複数方式を提供。[D-BGE] |
| HNSW / IVF / quantization | 大規模近傍検索の計算量削減 | ANN の近似誤差と、モデルの意味誤差を分ける。[P-HNSW] [D-PGV] [D-FAISS] |
| RRF | 異なる検索器の順位融合 | score の尺度を合わせる必要は減るが、真偽や時刻は別に扱う。[T-RRF] |
| Cross-encoder rerank | 問いと候補を一緒に読んで関連度を判定 | 候補から漏れた正解は復活できず、追加推論費用がある。[D-SBERT] |
| PPR / graph traversal | 関係の連鎖から候補を探す | hub の過剰露出、誤エッジの伝播。[C-HIPPO] |
| Community / tree retrieval | コーパスを異なる粒度で見る | 集約時の意味損失と削除後の再計算。[C-MSCLUSTER] [P-RAPTOR] |

RRF の典型形は `score(d) = Σ_i 1/(k + rank_i(d))`。これは順位を混ぜる関数で、出典の信頼度を推定する関数ではない。Graphiti と Hindsight の使い方を参照できる。[C-GSEARCH] [C-HFUSION]

ACL・tenant・有効期間は、本来候補生成から適用したい。ANN 後に厳しく filter すると、取得済み候補が空になり recall が落ちる。事前 filter、探索幅拡大、候補の追加取得、時点別索引などを実データで比較する。embedding の cosine 値を source confidence と足すなら、それぞれの尺度と根拠を説明できる必要がある。

## ベクトル・検索基盤の選択肢

| 候補 | 参考になる機能 | コストと検証点 |
|---|---|---|
| PostgreSQL + pgvector | 通常テーブル・transaction と exact/ANN search を近くに置ける | selective filter 後の recall、HNSW/IVFFlat、更新と索引サイズ。[D-PGV] |
| Qdrant | payload、dense/sparse、hybrid query | tenant filter、分散構成、更新可視性。[D-QDRANT] |
| LanceDB | 列指向データと vector / scalar / FTS index | 小規模ローカルとクラウドの違い、再索引、整合性境界。[D-LANCE] |
| Chroma | A-MEM 等が使う collection 型の検索基盤 | 研究の小規模条件と実運用条件を区別。[M-AMEM] |
| Milvus | 複数ベクトル field の hybrid search | 多モダリティ、分散運用、フィルタと一貫性。[D-MILVUS] |
| Weaviate | vector と BM25 の hybrid、fusion | fusion と threshold の意味を固定して評価。[D-WEAVIATE] |
| Vespa | retrieval と複数段の ranking を別に構成 | 複雑なランキングを制御できるが設計項目が多い。[D-VESPA] |
| FAISS / hnswlib / USearch | 埋め込める ANN ライブラリ | DB の認証・transaction・backup を自動では提供しない。[D-FAISS] [M-MACHINE] |

## グラフと推論基盤の選択肢

| 候補 | 役割 | 重要な境界 |
|---|---|---|
| Neo4j | property graph、Cypher、full-text/vector index | 2026 系で vector API が変化。対象版の query と index を固定。[D-NEO4J] |
| FalkorDB | property graph と Cypher 系の操作 | Graphiti の対応と DB のライセンス・運用範囲を別に確認。[D-FALKOR] [M-GRAPHITI] |
| Ladybug | Cognee が採用する graph 保存 | manifest に platform による pin がある。旧 Kuzu の情報を流用しない。[M-COGNEE] |
| Memgraph / Amazon Neptune | 既存の graph backend 候補 | LightRAG/Graphiti の adapter があることと同機能を保証することは別。[D-LIGHT] [M-GRAPHITI] |
| Oxigraph | Rust/Python 等からの RDF・SPARQL | RDF 保存と OWL/SHACL の全機能を同一視しない。[D-OXI] |
| Apache Jena / Fuseki | Java の RDF/SPARQL と rule/inference 基盤 | reasoner ごとのサポート範囲を選ぶ。[D-JENA] [D-JENARULE] |
| RDF4J | RDF store と transaction の SHACL 検証 | commit 時に関連データも検査。shape 変更時の再検査コスト。[D-RDF4J] |
| TypeDB | role を持つ型付き entity / relation / attribute | RDF/OWL と同じ query・意味論を前提にしない。[D-TYPEDB] |
| Stardog | 推論・virtual graph による既存データ照会 | データ複製の削減と、元 DB への依存のトレードオフ。[D-STARDOG] [D-VIRTUAL] |
| TerminusDB / XTDB | 履歴・diff／双時間 | 自然言語主張の採否は別層。[D-TERMINUS] [D-XTDB] |

ontology の編集には Protégé / ROBOT、Python 操作には RDFLib / Owlready2、Java API には OWL API、OWL 2 EL の分類には ELK、データ検証には pySHACL が候補。OWL reasoner、validator、DB をそれぞれ目的に応じて選ぶ。[D-PROTEGE] [D-ROBOT] [D-OWLREADY] [D-OWLAPI] [D-ELK] [D-PYSHACL]

## パイプライン運用と監査

LLM の外部呼び出しを DB transaction の中に長く保持すると、競合と再試行の扱いが難しくなる。候補の抽出、変更計画、短い確定 transaction、派生索引への outbox という分離を比較する。分散した各処理に event ID と revision を付け、重複を無害化する。[D-OUTBOX]

検索 trace には、候補の取得元、除外理由、最終的に使った claim revision、費用を記録する。原文を丸ごと全 trace に複製するより、必要な範囲へ戻れる source ID を保持する。MCP/REST はこの契約の公開面になるが、権限判定・来歴・訂正の仕様を別に持つ必要がある。[D-MCP]

外部記憶の取り込みと読み出しは prompt injection の入り口でもある。外部資料を行動ルールとして昇格させず、書き込み時の型検査、出典の信頼区分、取得時の権限と用途、ツール実行時の権限をそれぞれ評価する。[P-POISON] [D-OWASP]

## 関連ドキュメント

- [オントロジー・ナレッジシステムの推奨設計](../../design/ontology-knowledge-system.md)
- [オントロジーと知識表現](../foundations/ontology.md)
- [各システムが宣言する依存ライブラリ](dependencies.md)
- [設計の選択肢と検証仮説](../design-directions.md)

[D-DOCLING]: https://github.com/docling-project/docling/blob/main/docs/index.md
[D-MARKIT]: https://github.com/microsoft/markitdown
[M-OPENV]: https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/pyproject.toml
[D-SUDACHI]: https://github.com/WorksApplications/sudachi.rs
[D-SPACY]: https://spacy.io/usage/linguistic-features
[D-GLINER]: https://github.com/urchade/GLiNER
[M-COGNEE]: https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/pyproject.toml
[C-LANG]: https://github.com/langchain-ai/langmem/blob/9d033b47d9ce53e37e92c92241b0496c0278932e/src/langmem/knowledge/extraction.py
[C-COGONTO]: https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/modules/ontology/rdf_xml/RDFLibOntologyResolver.py
[D-ONTOGPT]: https://github.com/monarch-initiative/ontogpt/blob/main/docs/custom.md
[M-HIND]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/pyproject.toml
[M-MEMU]: https://github.com/NevaMind-AI/memU/blob/2c050bc9681a4c0aff1af211a000e73d14f33356/pyproject.toml
[C-M0]: https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/mem0/memory/main.py
[M-MEMOS]: https://github.com/MemTensor/MemOS/blob/a7367d07e55db61099f7b4e2c1108bc5831a24f3/pyproject.toml
[D-BGE]: https://huggingface.co/BAAI/bge-m3/blob/main/README.md
[P-HNSW]: https://arxiv.org/abs/1603.09320v4
[D-PGV]: https://github.com/pgvector/pgvector
[D-FAISS]: https://github.com/facebookresearch/faiss
[T-RRF]: https://research.google/pubs/reciprocal-rank-fusion-outperforms-condorcet-and-individual-rank-learning-methods/
[D-SBERT]: https://www.sbert.net/
[C-HIPPO]: https://github.com/OSU-NLP-Group/HippoRAG/blob/1438aba3fc44ff10573e5a5e1e7cc3c7f9794aff/src/hipporag/HippoRAG.py
[C-MSCLUSTER]: https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag/graphrag/index/operations/cluster_graph.py
[P-RAPTOR]: https://arxiv.org/abs/2401.18059v1
[C-GSEARCH]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/search/search_config.py
[C-HFUSION]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/hindsight_api/engine/search/fusion.py
[D-QDRANT]: https://qdrant.tech/documentation/search/hybrid-queries/
[D-LANCE]: https://docs.lancedb.com/api-reference/index/create-index
[M-AMEM]: https://github.com/agiresearch/A-mem/blob/ceffb860f0712bbae97b184d440df62bc910ca8d/pyproject.toml
[D-MILVUS]: https://blog.milvus.io/docs/multi-vector-search.md
[D-WEAVIATE]: https://docs.weaviate.io/weaviate/concepts/search/hybrid-search
[D-VESPA]: https://docs.vespa.ai/en/learn/tutorials/hybrid-search
[M-MACHINE]: https://github.com/MemMachine/MemMachine/blob/d57f5cb36a357c01085f571a0cdd2dfbc9882f89/packages/server/pyproject.toml
[D-NEO4J]: https://neo4j.com/docs/cypher-manual/current/indexes/semantic-indexes/vector-indexes/
[D-FALKOR]: https://github.com/FalkorDB/docs
[M-GRAPHITI]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/pyproject.toml
[D-LIGHT]: https://github.com/HKUDS/LightRAG/blob/453dce83d6d0354a06e46c8d4029a0895c4e054b/README.md
[D-OXI]: https://github.com/oxigraph/oxigraph
[D-JENA]: https://jena.apache.org/
[D-JENARULE]: https://jena.apache.org/documentation/inference/
[D-RDF4J]: https://rdf4j.org/documentation/programming/shacl/
[D-TYPEDB]: https://typedb.com/docs/core-concepts/typeql/schema-data/
[D-STARDOG]: https://docs.stardog.com/inference-engine/
[D-VIRTUAL]: https://docs.stardog.com/virtual-graphs/
[D-TERMINUS]: https://terminusdb.org/docs/version-controlled-json/
[D-XTDB]: https://docs.xtdb.com/concepts/key-concepts.html
[D-PROTEGE]: https://protege.stanford.edu/software/
[D-ROBOT]: https://github.com/ontodev/robot
[D-OWLREADY]: https://owlready2.readthedocs.io/en/latest/
[D-OWLAPI]: https://github.com/owlcs/owlapi
[D-ELK]: https://github.com/liveontologies/elk-reasoner
[D-PYSHACL]: https://github.com/RDFLib/pySHACL
[D-OUTBOX]: https://debezium.io/documentation/reference/stable/transformations/outbox-event-router.html
[D-MCP]: https://modelcontextprotocol.io/specification/2026-07-28
[P-POISON]: https://arxiv.org/abs/2407.12784v1
[D-OWASP]: https://cheatsheetseries.owasp.org/cheatsheets/RAG_Security_Cheat_Sheet.html
