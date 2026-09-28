# 実装で宣言されている依存ライブラリ

[調査トップ](../README.md) / [資料台帳](../sources.md)

このページは、公開実装の manifest と関連コードを確認した結果。ライブラリの一般的な選択肢は[基盤技術](infrastructure.md)へ分けた。下の版は **対象コミット内の package 宣言値**であり、PyPI/npm の最新リリースを意味しない。動的に決まる版や workspace の仮の版を製品バージョンと誤認しない。

宣言された依存の全文は [dependency-inventory.json](../evidence/dependency-inventory.json) に保存する。本文は役割が分かるよう要約した。JSON には runtime、optional、development、build、requirements snapshot の別を残す。依存解決・インストールは行っていない。

## システム別の構成

| 対象・宣言版 | 中核の依存と用途 | 追加依存・注意 | 根拠 |
|---|---|---|---|
| Mem0 2.2.1 | qdrant-client:索引、Pydantic:型、OpenAI:抽出、SQLAlchemy:保存補助、httpx | vector-stores / llms / extras は任意。PostHog も基礎依存として宣言 | [M-M0] |
| Graphiti 0.30.2 | Neo4j、Pydantic、OpenAI、NumPy、Tenacity | FalkorDB、Neptune、GLiNER2、Sentence Transformers、Voyage AI、tracing は extra。Kuzu 非推奨の記載 | [M-GRAPHITI] |
| Letta Code 0.33.5 | Letta Agent SDK/client、MCP SDK、Bun/TypeScript、Ink/React、node-pty | ripgrep は optional。旧 Python server の依存一覧を流用しない | [M-LETTA] |
| LangMem 0.0.30 | LangChain、LangGraph、Trustcall、LangSmith、checkpoint | DB そのものはこの package の役割ではない | [M-LANGMEM] |
| Cognee 1.6.1 | Pydantic、Instructor、LiteLLM、RDFLib、SQLAlchemy、LanceDB、Ladybug、NetworkX、FastEmbed | Neo4j、Neptune、Postgres、GLiNER、Docling 等を extra で拡張。OS/Python による条件付き依存あり | [M-COGNEE] |
| Hindsight API slim 0.10.1 | asyncpg、pgvector、SQLAlchemy、FastAPI、FastMCP、dateparser、OpenTelemetry | local-ml / local-onnx / embedded-db は追加。`hindsight-api` wrapper だけを読まない | [M-HIND] |
| Honcho 3.2.1 | FastAPI、SQLAlchemy、pgvector、Redis、turbopuffer、Langfuse、provider SDK | Qdrant、LanceDB、surprisal 用 scikit-learn は extra。存在する依存と既定の実行経路を区別 | [M-HONCHO] |
| Memobase server 0.1.0 | FastAPI、SQLAlchemy、pgvector、Redis、OpenAI、OpenTelemetry | repo root と server package を区別 | [M-MBASE] |
| MemTensor/MemOS 2.0.33 | transformers、OpenAI、Ollama、SQLAlchemy、FastAPI、FastMCP、scikit-learn | Neo4j、Redis/pika、Chonkie/MarkItDown、Milvus/datasketch 等は機能別 extra。配布名は `MemoryOS` | [M-MEMOS] |
| EverMemOS 1.4.1 | LanceDB、PyArrow、SQLite/SQLModel、Pydantic、OpenAI、`everalgo-*` | algorithm package 群へ処理を委譲。今回はその全ソース未監査 | [M-EVER] |
| Supermemory monorepo | AI SDK、provider SDK、Hono 関連、Zod、Drizzle、Postgres clients、Cloudflare | manifest はアプリ・統合を含む。非公開 engine の実装依存と同一視しない | [M-SUPER] |
| GraphRAG 3.2.0 | graspologic-native:Leiden、NetworkX、Pandas、PyArrow、spaCy、Pydantic | LLM package は LiteLLM、vector package は LanceDB/Azure 系。root は 0.0.0 の workspace | [M-MSGRAPH] [M-MSLLM] [M-MSVECTOR] |
| LightRAG 動的版 | NetworkX、nano-vectordb、NumPy、Pandas、json-repair、tiktoken | API、offline、評価、観測の追加 group。DB adapter と必須 install を区別 | [M-LIGHT] |
| HippoRAG 2 repo | python-igraph、NetworkX、PyTorch、transformers、OpenAI、LiteLLM、SciPy | この台帳は requirements を取得。特定の deploy の最小 runtime set ではない | [M-HIPPO] |
| A-MEM 0.0.1 | Sentence Transformers、ChromaDB、rank_bm25、NLTK、LiteLLM、NumPy | core manifest に pre-commit も含まれる。宣言場所だけで実行時の使用を断定しない | [M-AMEM] |
| SimpleMem 動的版 | text: OpenAI、Pydantic、LanceDB、Sentence Transformers、dateparser、Tantivy | setup.py の INSTALL_REQUIRES を静的抽出。現行 default は音声・画像用依存も含む。requirements は比較・評価依存も混在 | [M-SIMPLE] |
| Basic Memory 動的版 | markdown-it-py、frontmatter、SQLAlchemy、SQLite、FastEmbed、sqlite-vec、FastMCP | Postgres 接続系も宣言。Milvus、Redis、文書 parser 等は extra | [M-BASIC] |
| MCP server-memory 0.6.3 | MCP SDK、Zod、Node.js ファイル I/O | 外部 vector DB は基礎依存にない | [M-MCP] |
| memU 0.11.0-beta.3 | httpx、NumPy、OpenAI、Pydantic、SQLModel、Alembic、Pendulum | Postgres extra。OpenAI SDK の宣言だけでは LLM 抽出を実行している証拠にならない | [M-MEMU] [D-MEMU] |
| OpenViking 動的版 | SDK、Pydantic、Scrapy、Trafilatura、PDF parser、Tree-sitter 群、LiteLLM、MCP、OpenTelemetry | Python に加えて build に CMake/Maturin。`eval` 等は別 group | [M-OPENV] |
| MemMachine server 動的版 | Neo4j、pgvector、SQLAlchemy、sqlite-vec、USearch、Instructor、rank-bm25、FastMCP | Qdrant、Milvus、hnswlib、GPU 用 Sentence Transformers 等は extra | [M-MACHINE] |

## 共通して現れる技術の意味

**構造化生成。** Pydantic / Zod は型と入力検査、Instructor / Trustcall は schema に沿った抽出や修正、json-repair は壊れた JSON の復旧を支える。どれも「正しい事実を保証するライブラリ」ではない。JSON の修復成功と、元の根拠に一致した抽出を別指標にする。

**検索とグラフ。** Chroma、Qdrant、LanceDB、pgvector は保存・検索の部品。NetworkX / igraph はグラフ算法の部品。これらの採用をもって、OWL 推論・双時間・矛盾解消が備わると判断しない。GraphRAG の community detection、HippoRAG の PPR、Graphiti の typed fact は異なるグラフ利用である。[C-MSCLUSTER] [C-HIPPO] [C-GUPDATE]

**処理制御。** Tenacity は再試行、Redis/pika は queue や cache の実装候補、Alembic は DB schema migration。再試行の部品があっても、同じ入力が二重登録されない保証にはならない。idempotency と版の比較は更新 API の契約に必要。

**観測。** OpenTelemetry、LangSmith、Langfuse、PostHog が複数の manifest に現れる。採用時は trace と telemetry の送信設定、原資料・ユーザー情報の含まれ方をコードで確認する。依存しているだけで、すべてのデータを外へ送ると推測しない。

## ライセンスと保守の確認

取得した root LICENSE では、Mem0 / Graphiti / Letta / Cognee / MemOS / EverMemOS / Memobase は Apache-2.0、LangMem / Hindsight / GraphRAG / LightRAG / HippoRAG / A-MEM / Supermemory は MIT、Honcho / Basic Memory / OpenViking は AGPL 系だった。適用範囲は package や component により異なり得るため、個別ファイルと配布物で確認する。完全な版と LICENSE のリンクは [repository-snapshots.json](../evidence/repository-snapshots.json) に含める。

SimpleMem は取得した root LICENSE が MIT である一方、setup.py の classifier は Apache を記載している。この不一致を消さず、配布対象ごとの確認事項として残す。[M-SIMPLE]

API adapter が存在すること、依存が install できること、本番で同じ機能が使えることは三つの異なる確認である。今回の台帳は宣言の監査までであり、互換性テスト結果を含まない。

## 関連ドキュメント

- [システム比較](../systems/README.md)
- [周辺ライブラリと基盤の選択](infrastructure.md)
- [依存宣言を含む根拠データの読み方](../evidence/README.md)

[M-M0]: https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/pyproject.toml
[M-GRAPHITI]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/pyproject.toml
[M-LETTA]: https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/package.json
[M-LANGMEM]: https://github.com/langchain-ai/langmem/blob/9d033b47d9ce53e37e92c92241b0496c0278932e/pyproject.toml
[M-COGNEE]: https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/pyproject.toml
[M-HIND]: https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/pyproject.toml
[M-HONCHO]: https://github.com/plastic-labs/honcho/blob/2eb27b6cc0595d3f8deb693f0560e7241c2aeaff/pyproject.toml
[M-MBASE]: https://github.com/memodb-io/memobase/blob/358c16bbc6d687937d79bc2f984a11c3be8da901/src/server/api/pyproject.toml
[M-MEMOS]: https://github.com/MemTensor/MemOS/blob/a7367d07e55db61099f7b4e2c1108bc5831a24f3/pyproject.toml
[M-EVER]: https://github.com/EverMind-AI/EverMemOS/blob/462ebf9fd59b55c03fefb8eec855c62500f2a3cf/pyproject.toml
[M-SUPER]: https://github.com/supermemoryai/supermemory/blob/cfa6c7cb17476d19ea896867406c80e8186a72ec/package.json
[M-MSGRAPH]: https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag/pyproject.toml
[M-MSLLM]: https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag-llm/pyproject.toml
[M-MSVECTOR]: https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag-vectors/pyproject.toml
[M-LIGHT]: https://github.com/HKUDS/LightRAG/blob/453dce83d6d0354a06e46c8d4029a0895c4e054b/pyproject.toml
[M-HIPPO]: https://github.com/OSU-NLP-Group/HippoRAG/blob/1438aba3fc44ff10573e5a5e1e7cc3c7f9794aff/requirements.txt
[M-AMEM]: https://github.com/agiresearch/A-mem/blob/ceffb860f0712bbae97b184d440df62bc910ca8d/pyproject.toml
[M-SIMPLE]: https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/setup.py
[M-BASIC]: https://github.com/basicmachines-co/basic-memory/blob/22d31e96e2defa465d3703620a537c9125e7d364/pyproject.toml
[M-MCP]: https://github.com/modelcontextprotocol/servers/blob/f46d9578190b476b3501923ea8977d899e8db2cb/src/memory/package.json
[M-MEMU]: https://github.com/NevaMind-AI/memU/blob/2c050bc9681a4c0aff1af211a000e73d14f33356/pyproject.toml
[D-MEMU]: https://github.com/NevaMind-AI/memU/blob/2c050bc9681a4c0aff1af211a000e73d14f33356/README.md
[M-OPENV]: https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/pyproject.toml
[M-MACHINE]: https://github.com/MemMachine/MemMachine/blob/d57f5cb36a357c01085f571a0cdd2dfbc9882f89/packages/server/pyproject.toml
[C-MSCLUSTER]: https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag/graphrag/index/operations/cluster_graph.py
[C-HIPPO]: https://github.com/OSU-NLP-Group/HippoRAG/blob/1438aba3fc44ff10573e5a5e1e7cc3c7f9794aff/src/hipporag/HippoRAG.py
[C-GUPDATE]: https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/utils/maintenance/edge_operations.py
