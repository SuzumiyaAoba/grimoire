# 資料台帳

[調査トップ](README.md) / [根拠データ](evidence/README.md)

確認日: 2026-09-28。本文の参照 ID は以下の出典へ直接リンクする。論文は書誌で確認した arXiv の版を固定したものと、要旨のみ確認した補足資料がある。コードの取得範囲と依存宣言の全文は evidence を参照。

## 目次

- [公開実装の固定版](#repositories)
- [論文](#papers)
- [標準仕様](#standards)
- [基礎研究](#foundational-research)
- [確認したコード・manifest](#code-and-manifests)
- [公式文書・技術資料](#official-documents)
- [機械可読な台帳](#machine-readable-records)

<a id="repositories"></a>

## 公開実装の固定版

GitHub の可変な `main` ではなくコミット URL を使った。最新版コミットは必ずしもリリース済みとは限らない。次表の件数は取得したファイル数であり、全ファイルの精読数ではない。

| Repository | Commit | コミット日時（UTC） | 取得ファイル |
|---|---|---|---|
| [BAI-LAB/MemoryOS](https://github.com/BAI-LAB/MemoryOS/tree/587ed7755c7aed179965792830ff1b5ad9a6fa92) | `587ed7755c7a` | 2026-07-07T12:32:18Z | 2 |
| [EverMind-AI/EverMemOS](https://github.com/EverMind-AI/EverMemOS/tree/462ebf9fd59b55c03fefb8eec855c62500f2a3cf) | `462ebf9fd59b` | 2026-09-24T10:31:07Z | 5 |
| [HKUDS/LightRAG](https://github.com/HKUDS/LightRAG/tree/453dce83d6d0354a06e46c8d4029a0895c4e054b) | `453dce83d6d0` | 2026-09-26T08:33:35Z | 4 |
| [MemMachine/MemMachine](https://github.com/MemMachine/MemMachine/tree/d57f5cb36a357c01085f571a0cdd2dfbc9882f89) | `d57f5cb36a35` | 2026-09-25T19:15:26Z | 6 |
| [MemTensor/MemOS](https://github.com/MemTensor/MemOS/tree/a7367d07e55db61099f7b4e2c1108bc5831a24f3) | `a7367d07e55d` | 2026-09-22T01:51:03Z | 4 |
| [NevaMind-AI/memU](https://github.com/NevaMind-AI/memU/tree/2c050bc9681a4c0aff1af211a000e73d14f33356) | `2c050bc9681a` | 2026-09-21T11:45:36Z | 3 |
| [OSU-NLP-Group/HippoRAG](https://github.com/OSU-NLP-Group/HippoRAG/tree/1438aba3fc44ff10573e5a5e1e7cc3c7f9794aff) | `1438aba3fc44` | 2026-09-03T12:15:38Z | 4 |
| [agiresearch/A-mem](https://github.com/agiresearch/A-mem/tree/ceffb860f0712bbae97b184d440df62bc910ca8d) | `ceffb860f071` | 2025-12-12T21:15:29Z | 5 |
| [aiming-lab/SimpleMem](https://github.com/aiming-lab/SimpleMem/tree/db80b6a7c591e0ea730a058e9f5fc4eb06572299) | `db80b6a7c591` | 2026-07-24T07:40:38Z | 8 |
| [basicmachines-co/basic-memory](https://github.com/basicmachines-co/basic-memory/tree/22d31e96e2defa465d3703620a537c9125e7d364) | `22d31e96e2de` | 2026-09-24T00:54:32Z | 3 |
| [getzep/graphiti](https://github.com/getzep/graphiti/tree/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a) | `6b4b56ff6f4b` | 2026-09-27T08:49:42Z | 10 |
| [langchain-ai/langmem](https://github.com/langchain-ai/langmem/tree/9d033b47d9ce53e37e92c92241b0496c0278932e) | `9d033b47d9ce` | 2026-09-09T06:44:42Z | 5 |
| [letta-ai/letta](https://github.com/letta-ai/letta/tree/5bcdd177d70fa2b31a754cfcd801e77b2e1ab16a) | `5bcdd177d70f` | 2026-09-10T17:59:06Z | 2 |
| [letta-ai/letta-code](https://github.com/letta-ai/letta-code/tree/c864f1532b328aab4bb76cc68a86d5f014de27b8) | `c864f1532b32` | 2026-09-28T04:15:42Z | 8 |
| [mem0ai/mem0](https://github.com/mem0ai/mem0/tree/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd) | `94c3fe9f238f` | 2026-09-25T17:34:12Z | 9 |
| [memodb-io/memobase](https://github.com/memodb-io/memobase/tree/358c16bbc6d687937d79bc2f984a11c3be8da901) | `358c16bbc6d6` | 2026-01-11T03:43:56Z | 5 |
| [microsoft/graphrag](https://github.com/microsoft/graphrag/tree/769542fbf1d8e5b4c6a8677fefc34621c87894c5) | `769542fbf1d8` | 2026-09-23T22:13:34Z | 9 |
| [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers/tree/f46d9578190b476b3501923ea8977d899e8db2cb) | `f46d9578190b` | 2026-09-22T14:30:15Z | 5 |
| [plastic-labs/honcho](https://github.com/plastic-labs/honcho/tree/2eb27b6cc0595d3f8deb693f0560e7241c2aeaff) | `2eb27b6cc059` | 2026-09-25T18:18:15Z | 7 |
| [supermemoryai/supermemory](https://github.com/supermemoryai/supermemory/tree/cfa6c7cb17476d19ea896867406c80e8186a72ec) | `cfa6c7cb1747` | 2026-09-25T22:00:12Z | 3 |
| [topoteretes/cognee](https://github.com/topoteretes/cognee/tree/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b) | `c4cd8ceb9509` | 2026-09-27T18:17:51Z | 9 |
| [vectorize-io/hindsight](https://github.com/vectorize-io/hindsight/tree/8924a5bcfd6ff64fb20cace098021a3b61e76391) | `8924a5bcfd6f` | 2026-09-28T10:09:25Z | 10 |
| [volcengine/OpenViking](https://github.com/volcengine/OpenViking/tree/1f4f7039fc394c5d04637828166f4e4e74e249e0) | `1f4f7039fc39` | 2026-09-28T12:29:08Z | 3 |

完全な SHA、個々のファイル、SHA-256、取得に失敗したパスは [repository-snapshots.json](evidence/repository-snapshots.json) に記録。パス移動で 404 になったものも、機能の不存在を意味しない。

<a id="papers"></a>

## 論文

arXiv の comment に会議名がある場合も、それは版の書誌情報として保持する。掲載記録を確認できないものを査読済みとは扱わない。要旨のみの資料を本文精読と表記しない。

| ID | 論文 | 初稿日 | 版 | 確認範囲 |
|---|---|---|---|---|
| P-M0 | [Mem0: Building Production-Ready AI Agents with Scalable Long-Term Memory](https://arxiv.org/abs/2504.19413v1) | 2025/04/28 | v1 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-ZEP | [Zep: A Temporal Knowledge Graph Architecture for Agent Memory](https://arxiv.org/abs/2501.13956v1) | 2025/01/20 | v1 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-HIPPO2 | [From RAG to Memory: Non-Parametric Continual Learning for Large Language Models](https://arxiv.org/abs/2502.14802v2) | 2025/02/20 | v2 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-SIMPLE | [SimpleMem: Efficient Lifelong Memory for LLM Agents](https://arxiv.org/abs/2601.02553v3) | 2026/01/05 | v3 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-HIND | [Hindsight is 20/20: Building Agent Memory that Retains, Recalls, and Reflects](https://arxiv.org/abs/2512.12818v1) | 2025/12/14 | v1 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-ACE | [Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models](https://arxiv.org/abs/2510.04618v3) | 2025/10/06 | v3 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-FR | [Fortunate Recall: Ontology-Driven Memory Lifecycle Management for Persistent Coherence in LLMs](https://arxiv.org/abs/2609.10413v1) | 2026/09/09 | v1 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-LME | [LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory](https://arxiv.org/abs/2410.10813v2) | 2024/10/14 | v2 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-LOCOMO | [Evaluating Very Long-Term Conversational Memory of LLM Agents](https://arxiv.org/abs/2402.17753v1) | 2024/02/27 | v1 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-MAB | [Evaluating Memory in LLM Agents via Incremental Multi-Turn Interactions](https://arxiv.org/abs/2507.05257v4) | 2025/07/07 | v4 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-HALU | [HaluMem: Evaluating Hallucinations in Memory Systems of Agents](https://arxiv.org/abs/2511.03506v3) | 2025/11/05 | v3 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-ARENA | [Benchmarking Agent Memory in Interdependent Multi-Session Agentic Tasks](https://arxiv.org/abs/2602.16313v2) | 2026/02/18 | v2 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-AUTOSCHEMA | [AutoSchemaKG: Autonomous Knowledge Graph Construction through Dynamic Schema Induction from Web-Scale Corpora](https://arxiv.org/abs/2505.23628v3) | 2025/05/29 | v3 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-SURVEY | [Memory in the Age of AI Agents](https://arxiv.org/abs/2512.13564v2) | 2025/12/15 | v2 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-EVER | [EverMemOS: A Self-Organizing Memory Operating System for Structured Long-Horizon Reasoning](https://arxiv.org/abs/2601.02163v2) | 2026/01/05 | v2 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-MEMRL | [MemRL: Self-Evolving Agents via Runtime Reinforcement Learning on Episodic Memory](https://arxiv.org/abs/2601.03192v2) | 2026/01/06 | v2 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-MR1 | [Memory-R1: Enhancing Large Language Model Agents to Manage and Utilize Memories via Reinforcement Learning](https://arxiv.org/abs/2508.19828v5) | 2025/08/27 | v5 | 本文取得、手法・実験・制約の関連箇所を確認 |
| P-GEN | [Generative Agents: Interactive Simulacra of Human Behavior](https://arxiv.org/abs/2304.03442v2) | 2023/04/07 | v2 | 要旨・書誌を確認 |
| P-MBANK | [MemoryBank: Enhancing Large Language Models with Long-Term Memory](https://arxiv.org/abs/2305.10250v3) | 2023/05/17 | v3 | 要旨・書誌を確認 |
| P-MGPT | [MemGPT: Towards LLMs as Operating Systems](https://arxiv.org/abs/2310.08560v2) | 2023/10/12 | v2 | 要旨・書誌を確認 |
| P-COALA | [Cognitive Architectures for Language Agents](https://arxiv.org/abs/2309.02427v3) | 2023/09/05 | v3 | 要旨・書誌を確認 |
| P-REFLEX | [Reflexion: Language Agents with Verbal Reinforcement Learning](https://arxiv.org/abs/2303.11366v4) | 2023/03/20 | v4 | 要旨・書誌を確認 |
| P-MSGRAPH | [From Local to Global: A Graph RAG Approach to Query-Focused Summarization](https://arxiv.org/abs/2404.16130v2) | 2024/04/24 | v2 | 要旨・書誌を確認 |
| P-LIGHT | [LightRAG: Simple and Fast Retrieval-Augmented Generation](https://arxiv.org/abs/2410.05779v3) | 2024/10/08 | v3 | 要旨・書誌を確認 |
| P-HIPPO | [HippoRAG: Neurobiologically Inspired Long-Term Memory for Large Language Models](https://arxiv.org/abs/2405.14831v3) | 2024/05/23 | v3 | 要旨・書誌を確認 |
| P-AMEM | [A-MEM: Agentic Memory for LLM Agents](https://arxiv.org/abs/2502.12110v11) | 2025/02/17 | v11 | 要旨・書誌を確認 |
| P-MEMOS | [MemOS: An Operating System for Memory-Augmented Generation (MAG) in Large Language Models](https://arxiv.org/abs/2505.22101v1) | 2025/05/28 | v1 | 要旨・書誌を確認 |
| P-MIRIX | [MIRIX: Multi-Agent Memory System for LLM-Based Agents](https://arxiv.org/abs/2507.07957v1) | 2025/07/10 | v1 | 要旨・書誌を確認 |
| P-MSKILLS | [Memento-Skills: Let Agents Design Agents](https://arxiv.org/abs/2603.18743v1) | 2026/03/19 | v1 | 要旨・書誌を確認 |
| P-TITANS | [Titans: Learning to Memorize at Test Time](https://arxiv.org/abs/2501.00663v1) | 2024/12/31 | v1 | 要旨・書誌を確認 |
| P-ENGRAM | [Conditional Memory via Scalable Lookup: A New Axis of Sparsity for Large Language Models](https://arxiv.org/abs/2601.07372v2) | 2026/01/12 | v2 | 要旨・書誌を確認 |
| P-RAPTOR | [RAPTOR: Recursive Abstractive Processing for Tree-Organized Retrieval](https://arxiv.org/abs/2401.18059v1) | 2024/01/31 | v1 | 要旨・書誌を確認 |
| P-KAG | [KAG: Boosting LLMs in Professional Domains via Knowledge Augmented Generation](https://arxiv.org/abs/2409.13731v3) | 2024/09/10 | v3 | 要旨・書誌を確認 |
| P-LLMOL | [LLMs4OL: Large Language Models for Ontology Learning](https://arxiv.org/abs/2307.16648v2) | 2023/07/31 | v2 | 要旨・書誌を確認 |
| P-LLMOL26 | [pro-team at LLMs4OL 2026 Tasks Flagship and Reuse](https://arxiv.org/abs/2608.27101v2) | 2026/08/27 | v2 | 要旨・書誌を確認 |
| P-NOLIMA | [NoLiMa: Long-Context Evaluation Beyond Literal Matching](https://arxiv.org/abs/2502.05167v3) | 2025/02/07 | v3 | 要旨・書誌を確認 |
| P-PERSONA | [Know Me, Respond to Me: Benchmarking LLMs for Dynamic User Profiling and Personalized Responses at Scale](https://arxiv.org/abs/2504.14225v2) | 2025/04/19 | v2 | 要旨・書誌を確認 |
| P-EVERBENCH | [EverMemBench: Benchmarking Long-Term Interactive Memory in Large Language Models](https://arxiv.org/abs/2602.01313v3) | 2026/02/01 | v3 | 要旨・書誌を確認 |
| P-LOCOPLUS | [Locomo-Plus: Beyond-Factual Cognitive Memory Evaluation Framework for LLM Agents](https://arxiv.org/abs/2602.10715v1) | 2026/02/11 | v1 | 要旨・書誌を確認 |
| P-POISON | [AgentPoison: Red-teaming LLM Agents via Poisoning Memory or Knowledge Bases](https://arxiv.org/abs/2407.12784v1) | 2024/07/17 | v1 | 要旨・書誌を確認 |
| P-MEMGATE | [Beyond Similarity: Trustworthy Memory Search for Personal AI Agents](https://arxiv.org/abs/2606.06054v1) | 2026/06/04 | v1 | 要旨・書誌を確認 |
| P-HNSW | [Efficient and robust approximate nearest neighbor search using Hierarchical Navigable Small World graphs](https://arxiv.org/abs/1603.09320v4) | 2016/03/30 | v4 | 要旨・書誌を確認 |
| P-RAG | [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401v4) | 2020/05/22 | v4 | 要旨・書誌を確認 |
| P-VOYAGER | [Voyager: An Open-Ended Embodied Agent with Large Language Models](https://arxiv.org/abs/2305.16291v2) | 2023/05/25 | v2 | 要旨・書誌を確認 |
| P-MEMORYOS | [Memory OS of AI Agent](https://arxiv.org/abs/2506.06326v1) | 2025/05/30 | v1 | 要旨・書誌を確認 |
| P-VIKING | [VikingMem: A Memory Base Management System for Stateful LLM-based Applications](https://arxiv.org/abs/2605.29640v3) | 2026/05/28 | v3 | 要旨・書誌を確認 |
| P-CONVOMEM | [Convomem Benchmark: Why Your First 150 Conversations Don't Need RAG](https://arxiv.org/abs/2511.10523v1) | 2025/11/13 | v1 | 要旨・書誌を確認 |
| P-BEAM | [Beyond a Million Tokens: Benchmarking and Enhancing Long-Term Memory in LLMs](https://arxiv.org/abs/2510.27246v2) | 2025/10/31 | v2 | 要旨・書誌を確認 |

<a id="standards"></a>

## 標準仕様

| ID | 資料 | 確認範囲 |
|---|---|---|
| S-RDF | [RDF 1.1 Concepts](https://www.w3.org/TR/rdf11-concepts/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-OWL | [OWL 2 Primer](https://www.w3.org/TR/owl2-primer/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-PROFILES | [OWL 2 Profiles](https://www.w3.org/TR/owl2-profiles/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-SHACL | [Shapes Constraint Language](https://www.w3.org/TR/shacl/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-SKOS | [SKOS Reference](https://www.w3.org/TR/skos-reference/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-PROV | [PROV-O](https://www.w3.org/TR/prov-o/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-TIME | [Time Ontology in OWL](https://www.w3.org/TR/owl-time/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-SPARQL | [SPARQL 1.1 Overview](https://www.w3.org/TR/sparql11-overview/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-JSONLD | [JSON-LD 1.1](https://www.w3.org/TR/json-ld11/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-RDF12 | [RDF 1.2 Concepts, Candidate Recommendation 2026-04-07](https://www.w3.org/TR/2026/CR-rdf12-concepts-20260407/) | 関連する意味論・制約・仕様ステータスを確認 |
| S-NARY | [Defining N-ary Relations on the Semantic Web](https://www.w3.org/TR/swbp-n-aryRelations/) | 関連する意味論・制約・仕様ステータスを確認 |

<a id="foundational-research"></a>

## 基礎研究

| ID | 資料 | 確認範囲 |
|---|---|---|
| T-AGM | [AGM: Partial meet contraction and revision functions (1985)](https://doi.org/10.2307/2274239) | 出版元・著者公開資料の関連記述を確認 |
| T-TMS | [Doyle: A truth maintenance system (1979)](https://www.sciencedirect.com/science/article/pii/0004370279900080) | 出版元・著者公開資料の関連記述を確認 |
| T-UPDATE | [Friedman and Halpern: Modeling Belief in Dynamic Systems, Part II](https://arxiv.org/abs/cs/9903016) | 出版元・著者公開資料の関連記述を確認 |
| T-PROVENANCE | [Green et al.: Provenance Semirings (2007)](https://web.cs.ucdavis.edu/~green/papers/pods07.pdf) | 出版元・著者公開資料の関連記述を確認 |
| T-RRF | [Cormack et al.: Reciprocal Rank Fusion (2009)](https://research.google/pubs/reciprocal-rank-fusion-outperforms-condorcet-and-individual-rank-learning-methods/) | 出版元・著者公開資料の関連記述を確認 |
| T-CRDT | [Shapiro et al.: Convergent and Commutative Replicated Data Types (2011)](https://dsf.berkeley.edu/cs286/papers/crdt-tr2011.pdf) | 研究報告の定義と収束条件を確認 |
| T-DRED | [Gupta et al.: Maintaining views incrementally (1993)](https://doi.org/10.1145/170036.170066) | 出版社の書誌・概要を確認 |

<a id="code-and-manifests"></a>

## 確認したコード・manifest

| ID | 資料 | 確認範囲 |
|---|---|---|
| C-M0 | [mem0ai/mem0 / mem0/memory/main.py](https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/mem0/memory/main.py) | 対象ファイルを静的に確認（実行していない） |
| C-M0CFG | [mem0ai/mem0 / mem0/configs/base.py](https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/mem0/configs/base.py) | 対象ファイルを静的に確認（実行していない） |
| D-M0OSS | [mem0ai/mem0 / docs/open-source/configuration.mdx](https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/docs/open-source/configuration.mdx) | 対象ファイルを静的に確認（実行していない） |
| D-M0GRAPH | [mem0ai/mem0 / docs/platform/features/graph-memory.mdx](https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/docs/platform/features/graph-memory.mdx) | 対象ファイルを静的に確認（実行していない） |
| C-GEDGE | [getzep/graphiti / graphiti_core/edges.py](https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/edges.py) | 対象ファイルを静的に確認（実行していない） |
| C-GUPDATE | [getzep/graphiti / graphiti_core/utils/maintenance/edge_operations.py](https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/utils/maintenance/edge_operations.py) | 対象ファイルを静的に確認（実行していない） |
| C-GNODE | [getzep/graphiti / graphiti_core/utils/maintenance/node_operations.py](https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/utils/maintenance/node_operations.py) | 対象ファイルを静的に確認（実行していない） |
| C-GSEARCH | [getzep/graphiti / graphiti_core/search/search_config.py](https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/search/search_config.py) | 対象ファイルを静的に確認（実行していない） |
| C-GMAIN | [getzep/graphiti / graphiti_core/graphiti.py](https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/graphiti_core/graphiti.py) | 対象ファイルを静的に確認（実行していない） |
| C-LMOV | [letta-ai/letta / README.md](https://github.com/letta-ai/letta/blob/5bcdd177d70fa2b31a754cfcd801e77b2e1ab16a/README.md) | 対象ファイルを静的に確認（実行していない） |
| C-LMEM | [letta-ai/letta-code / src/agent/memory-filesystem.ts](https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/src/agent/memory-filesystem.ts) | 対象ファイルを静的に確認（実行していない） |
| C-LGIT | [letta-ai/letta-code / src/agent/memory-git.ts](https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/src/agent/memory-git.ts) | 対象ファイルを静的に確認（実行していない） |
| C-LBLOCK | [letta-ai/letta-code / src/agent/memory.ts](https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/src/agent/memory.ts) | 対象ファイルを静的に確認（実行していない） |
| C-LANG | [langchain-ai/langmem / src/langmem/knowledge/extraction.py](https://github.com/langchain-ai/langmem/blob/9d033b47d9ce53e37e92c92241b0496c0278932e/src/langmem/knowledge/extraction.py) | 対象ファイルを静的に確認（実行していない） |
| C-COGONTO | [topoteretes/cognee / cognee/modules/ontology/rdf_xml/RDFLibOntologyResolver.py](https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/modules/ontology/rdf_xml/RDFLibOntologyResolver.py) | 対象ファイルを静的に確認（実行していない） |
| C-COGSTRICT | [topoteretes/cognee / cognee/modules/ontology/construct_data_points_and_edges_with_ontology.py](https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/modules/ontology/construct_data_points_and_edges_with_ontology.py) | 対象ファイルを静的に確認（実行していない） |
| C-COG | [topoteretes/cognee / cognee/api/v1/cognify/cognify.py](https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/cognee/api/v1/cognify/cognify.py) | 対象ファイルを静的に確認（実行していない） |
| C-HFUSION | [vectorize-io/hindsight / hindsight-api-slim/hindsight_api/engine/search/fusion.py](https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/hindsight_api/engine/search/fusion.py) | 対象ファイルを静的に確認（実行していない） |
| C-HCONSOL | [vectorize-io/hindsight / hindsight-api-slim/hindsight_api/engine/consolidation/consolidator.py](https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/hindsight_api/engine/consolidation/consolidator.py) | 対象ファイルを静的に確認（実行していない） |
| C-HSEARCH | [vectorize-io/hindsight / hindsight-api-slim/hindsight_api/engine/search/retrieval.py](https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/hindsight_api/engine/search/retrieval.py) | 対象ファイルを静的に確認（実行していない） |
| D-HIND | [vectorize-io/hindsight / README.md](https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/README.md) | 対象ファイルを静的に確認（実行していない） |
| D-HRETAIN | [vectorize-io/hindsight / skills/hindsight-docs/references/developer/retain.md](https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/skills/hindsight-docs/references/developer/retain.md) | 対象ファイルを静的に確認（実行していない） |
| D-HONCHO | [plastic-labs/honcho / docs/v3/documentation/core-concepts/representation.mdx](https://github.com/plastic-labs/honcho/blob/2eb27b6cc0595d3f8deb693f0560e7241c2aeaff/docs/v3/documentation/core-concepts/representation.mdx) | 対象ファイルを静的に確認（実行していない） |
| D-HDREAM | [plastic-labs/honcho / docs/v3/documentation/features/advanced/dreaming.mdx](https://github.com/plastic-labs/honcho/blob/2eb27b6cc0595d3f8deb693f0560e7241c2aeaff/docs/v3/documentation/features/advanced/dreaming.mdx) | 対象ファイルを静的に確認（実行していない） |
| D-MBASE | [memodb-io/memobase / readme.md](https://github.com/memodb-io/memobase/blob/358c16bbc6d687937d79bc2f984a11c3be8da901/readme.md) | 対象ファイルを静的に確認（実行していない） |
| C-MBASE | [memodb-io/memobase / src/server/api/memobase_server/controllers/modal/chat/merge.py](https://github.com/memodb-io/memobase/blob/358c16bbc6d687937d79bc2f984a11c3be8da901/src/server/api/memobase_server/controllers/modal/chat/merge.py) | 対象ファイルを静的に確認（実行していない） |
| D-SUPER | [supermemoryai/supermemory / README.md](https://github.com/supermemoryai/supermemory/blob/cfa6c7cb17476d19ea896867406c80e8186a72ec/README.md) | 対象ファイルを静的に確認（実行していない） |
| C-MSCLUSTER | [microsoft/graphrag / packages/graphrag/graphrag/index/operations/cluster_graph.py](https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag/graphrag/index/operations/cluster_graph.py) | 対象ファイルを静的に確認（実行していない） |
| C-MSUPDATE | [microsoft/graphrag / packages/graphrag/graphrag/index/workflows/update_entities_relationships.py](https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag/graphrag/index/workflows/update_entities_relationships.py) | 対象ファイルを静的に確認（実行していない） |
| D-LIGHT | [HKUDS/LightRAG / README.md](https://github.com/HKUDS/LightRAG/blob/453dce83d6d0354a06e46c8d4029a0895c4e054b/README.md) | 対象ファイルを静的に確認（実行していない） |
| C-LIGHT | [HKUDS/LightRAG / lightrag/operate.py](https://github.com/HKUDS/LightRAG/blob/453dce83d6d0354a06e46c8d4029a0895c4e054b/lightrag/operate.py) | 対象ファイルを静的に確認（実行していない） |
| C-HIPPO | [OSU-NLP-Group/HippoRAG / src/hipporag/HippoRAG.py](https://github.com/OSU-NLP-Group/HippoRAG/blob/1438aba3fc44ff10573e5a5e1e7cc3c7f9794aff/src/hipporag/HippoRAG.py) | 対象ファイルを静的に確認（実行していない） |
| C-AMEM | [agiresearch/A-mem / agentic_memory/memory_system.py](https://github.com/agiresearch/A-mem/blob/ceffb860f0712bbae97b184d440df62bc910ca8d/agentic_memory/memory_system.py) | 対象ファイルを静的に確認（実行していない） |
| D-BASIC | [basicmachines-co/basic-memory / README.md](https://github.com/basicmachines-co/basic-memory/blob/22d31e96e2defa465d3703620a537c9125e7d364/README.md) | 対象ファイルを静的に確認（実行していない） |
| C-MCP | [modelcontextprotocol/servers / src/memory/index.ts](https://github.com/modelcontextprotocol/servers/blob/f46d9578190b476b3501923ea8977d899e8db2cb/src/memory/index.ts) | 対象ファイルを静的に確認（実行していない） |
| D-MEMOS | [MemTensor/MemOS / README.md](https://github.com/MemTensor/MemOS/blob/a7367d07e55db61099f7b4e2c1108bc5831a24f3/README.md) | 対象ファイルを静的に確認（実行していない） |
| D-EVER | [EverMind-AI/EverMemOS / README.md](https://github.com/EverMind-AI/EverMemOS/blob/462ebf9fd59b55c03fefb8eec855c62500f2a3cf/README.md) | 対象ファイルを静的に確認（実行していない） |
| C-EVER | [EverMind-AI/EverMemOS / src/everos/memory/extract/pipeline/user_memory.py](https://github.com/EverMind-AI/EverMemOS/blob/462ebf9fd59b55c03fefb8eec855c62500f2a3cf/src/everos/memory/extract/pipeline/user_memory.py) | 対象ファイルを静的に確認（実行していない） |
| D-SIMPLE | [aiming-lab/SimpleMem / README.md](https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/README.md) | 対象ファイルを静的に確認（実行していない） |
| D-MACHINE | [MemMachine/MemMachine / README.md](https://github.com/MemMachine/MemMachine/blob/d57f5cb36a357c01085f571a0cdd2dfbc9882f89/README.md) | 対象ファイルを静的に確認（実行していない） |
| D-MEMU | [NevaMind-AI/memU / README.md](https://github.com/NevaMind-AI/memU/blob/2c050bc9681a4c0aff1af211a000e73d14f33356/README.md) | 対象ファイルを静的に確認（実行していない） |
| D-OPENV | [volcengine/OpenViking / README.md](https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/README.md) | 対象ファイルを静的に確認（実行していない） |
| D-MEMORYOS | [BAI-LAB/MemoryOS / README.md](https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/README.md) | 対象ファイルを静的に確認（実行していない） |
| C-SIMPLER | [aiming-lab/SimpleMem / simplemem/core/hybrid_retriever.py](https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/simplemem/core/hybrid_retriever.py) | 対象ファイルを静的に確認（実行していない） |
| C-SIMPLEDB | [aiming-lab/SimpleMem / simplemem/core/database/vector_store.py](https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/simplemem/core/database/vector_store.py) | 対象ファイルを静的に確認（実行していない） |
| C-SIMPLEBUILD | [aiming-lab/SimpleMem / simplemem/core/memory_builder.py](https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/simplemem/core/memory_builder.py) | 対象ファイルを静的に確認（実行していない） |
| M-M0 | [mem0ai/mem0 / pyproject.toml](https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-GRAPHITI | [getzep/graphiti / pyproject.toml](https://github.com/getzep/graphiti/blob/6b4b56ff6f4b1e4e69c3c3c5487cf1b8762c483a/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-LETTA | [letta-ai/letta-code / package.json](https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/package.json) | 対象ファイルを静的に確認（実行していない） |
| M-LANGMEM | [langchain-ai/langmem / pyproject.toml](https://github.com/langchain-ai/langmem/blob/9d033b47d9ce53e37e92c92241b0496c0278932e/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-COGNEE | [topoteretes/cognee / pyproject.toml](https://github.com/topoteretes/cognee/blob/c4cd8ceb9509dff6bddfabdadbeab7cc040bc32b/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-HIND | [vectorize-io/hindsight / hindsight-api-slim/pyproject.toml](https://github.com/vectorize-io/hindsight/blob/8924a5bcfd6ff64fb20cace098021a3b61e76391/hindsight-api-slim/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-HONCHO | [plastic-labs/honcho / pyproject.toml](https://github.com/plastic-labs/honcho/blob/2eb27b6cc0595d3f8deb693f0560e7241c2aeaff/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-MBASE | [memodb-io/memobase / src/server/api/pyproject.toml](https://github.com/memodb-io/memobase/blob/358c16bbc6d687937d79bc2f984a11c3be8da901/src/server/api/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-MEMOS | [MemTensor/MemOS / pyproject.toml](https://github.com/MemTensor/MemOS/blob/a7367d07e55db61099f7b4e2c1108bc5831a24f3/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-EVER | [EverMind-AI/EverMemOS / pyproject.toml](https://github.com/EverMind-AI/EverMemOS/blob/462ebf9fd59b55c03fefb8eec855c62500f2a3cf/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-SUPER | [supermemoryai/supermemory / package.json](https://github.com/supermemoryai/supermemory/blob/cfa6c7cb17476d19ea896867406c80e8186a72ec/package.json) | 対象ファイルを静的に確認（実行していない） |
| M-MSGRAPH | [microsoft/graphrag / packages/graphrag/pyproject.toml](https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-MSVECTOR | [microsoft/graphrag / packages/graphrag-vectors/pyproject.toml](https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag-vectors/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-MSLLM | [microsoft/graphrag / packages/graphrag-llm/pyproject.toml](https://github.com/microsoft/graphrag/blob/769542fbf1d8e5b4c6a8677fefc34621c87894c5/packages/graphrag-llm/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-LIGHT | [HKUDS/LightRAG / pyproject.toml](https://github.com/HKUDS/LightRAG/blob/453dce83d6d0354a06e46c8d4029a0895c4e054b/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-HIPPO | [OSU-NLP-Group/HippoRAG / requirements.txt](https://github.com/OSU-NLP-Group/HippoRAG/blob/1438aba3fc44ff10573e5a5e1e7cc3c7f9794aff/requirements.txt) | 対象ファイルを静的に確認（実行していない） |
| M-AMEM | [agiresearch/A-mem / pyproject.toml](https://github.com/agiresearch/A-mem/blob/ceffb860f0712bbae97b184d440df62bc910ca8d/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-SIMPLE | [aiming-lab/SimpleMem / setup.py](https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/setup.py) | 対象ファイルを静的に確認（実行していない） |
| M-BASIC | [basicmachines-co/basic-memory / pyproject.toml](https://github.com/basicmachines-co/basic-memory/blob/22d31e96e2defa465d3703620a537c9125e7d364/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-MCP | [modelcontextprotocol/servers / src/memory/package.json](https://github.com/modelcontextprotocol/servers/blob/f46d9578190b476b3501923ea8977d899e8db2cb/src/memory/package.json) | 対象ファイルを静的に確認（実行していない） |
| M-MEMU | [NevaMind-AI/memU / pyproject.toml](https://github.com/NevaMind-AI/memU/blob/2c050bc9681a4c0aff1af211a000e73d14f33356/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-OPENV | [volcengine/OpenViking / pyproject.toml](https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |
| M-MACHINE | [MemMachine/MemMachine / packages/server/pyproject.toml](https://github.com/MemMachine/MemMachine/blob/d57f5cb36a357c01085f571a0cdd2dfbc9882f89/packages/server/pyproject.toml) | 対象ファイルを静的に確認（実行していない） |

<a id="official-documents"></a>

## 公式文書・技術資料

| ID | 資料 | 確認範囲 |
|---|---|---|
| D-WIKIDATA | [Wikidata Data model](https://www.wikidata.org/wiki/Help:Data_model) | 仕様・説明を確認 |
| D-SCHEMA | [Schema.org Data model](https://schema.org/docs/datamodel.html) | 仕様・説明を確認 |
| D-BFO | [BFO 2020](https://github.com/BFO-ontology/BFO-2020) | 仕様・説明を確認 |
| D-DOLCE | [DOLCE — Laboratory for Applied Ontology](https://www.loa.istc.cnr.it/index.php/dolce/) | 仕様・説明を確認 |
| D-SUMO | [Suggested Upper Merged Ontology](https://www.ontologyportal.org/) | 仕様・説明を確認 |
| D-CYC | [Cyc Technology Overview](https://cyc.com/wp-content/uploads/2021/04/Cyc-Technology-Overview.pdf) | 仕様・説明を確認 |
| D-CONCEPT | [ConceptNet](https://conceptnet.io/) | 仕様・説明を確認 |
| D-ONTOGPT | [OntoGPT custom schemas](https://github.com/monarch-initiative/ontogpt/blob/main/docs/custom.md) | 仕様・説明を確認 |
| D-ONTOCHECK | [OntoGPT operation](https://github.com/monarch-initiative/ontogpt/blob/main/docs/operation.md) | 仕様・説明を確認 |
| D-LANGMEM | [LangMem concepts](https://langchain-ai.github.io/langmem/concepts/conceptual_guide/) | 仕様・説明を確認 |
| D-MASTRA | [Mastra Observational Memory research](https://mastra.ai/research/observational-memory) | 仕様・説明を確認 |
| D-LME-CODE | [LongMemEval official evaluation](https://github.com/xiaowu0162/LongMemEval) | 仕様・説明を確認 |
| D-LOCO-CODE | [LoCoMo official dataset and evaluation](https://github.com/snap-research/locomo) | 仕様・説明を確認 |
| D-MSGRAPH | [GraphRAG query overview](https://microsoft.github.io/graphrag/query/overview/) | 仕様・説明を確認 |
| D-KAG | [OpenSPG / KAG](https://github.com/OpenSPG/KAG) | 仕様・説明を確認 |
| D-AWS | [AgentCore Memory types](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-types.html) | 仕様・説明を確認 |
| D-AWSSTRAT | [AgentCore memory strategies](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-custom-strategy.html) | 仕様・説明を確認 |
| D-AZURE | [Microsoft Foundry Memory](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-memory) | 仕様・説明を確認 |
| D-CHATGPT | [Memory in ChatGPT](https://help.openai.com/en/articles/8590148-memory-in-chatgpt) | 仕様・説明を確認 |
| D-CLAUDE | [Claude chat search and memory](https://support.claude.com/en/articles/11817273-use-claude-s-chat-search-and-memory-to-build-on-previous-context) | 仕様・説明を確認 |
| D-GEMINI | [Gemini past-chat memory](https://support.google.com/gemini/answer/16598469?hl=en) | 仕様・説明を確認 |
| D-FOUNDRY | [Palantir Object edits and materializations](https://www.palantir.com/docs/foundry/object-edits/overview) | 仕様・説明を確認 |
| D-STARDOG | [Stardog Inference Engine](https://docs.stardog.com/inference-engine/) | 仕様・説明を確認 |
| D-VIRTUAL | [Stardog Virtual Graphs](https://docs.stardog.com/virtual-graphs/) | 仕様・説明を確認 |
| D-TERMINUS | [TerminusDB version-controlled JSON](https://terminusdb.org/docs/version-controlled-json/) | 仕様・説明を確認 |
| D-XTDB | [XTDB time semantics](https://docs.xtdb.com/concepts/key-concepts.html) | 仕様・説明を確認 |
| D-EVENT | [Fowler: Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html) | 仕様・説明を確認 |
| D-OUTBOX | [Debezium Outbox Event Router](https://debezium.io/documentation/reference/stable/transformations/outbox-event-router.html) | 仕様・説明を確認 |
| D-PGV | [pgvector](https://github.com/pgvector/pgvector) | 仕様・説明を確認 |
| D-QDRANT | [Qdrant Hybrid Queries](https://qdrant.tech/documentation/search/hybrid-queries/) | 仕様・説明を確認 |
| D-NEO4J | [Neo4j Vector indexes](https://neo4j.com/docs/cypher-manual/current/indexes/semantic-indexes/vector-indexes/) | 仕様・説明を確認 |
| D-FALKOR | [FalkorDB docs](https://github.com/FalkorDB/docs) | 仕様・説明を確認 |
| D-OXI | [Oxigraph](https://github.com/oxigraph/oxigraph) | 仕様・説明を確認 |
| D-JENA | [Apache Jena](https://jena.apache.org/) | 仕様・説明を確認 |
| D-TYPEDB | [TypeDB schema and data](https://typedb.com/docs/core-concepts/typeql/schema-data/) | 仕様・説明を確認 |
| D-PROTEGE | [Protégé / WebProtégé](https://protege.stanford.edu/software/) | 仕様・説明を確認 |
| D-ROBOT | [ROBOT](https://github.com/ontodev/robot) | 仕様・説明を確認 |
| D-PYSHACL | [pySHACL](https://github.com/RDFLib/pySHACL) | 仕様・説明を確認 |
| D-SPACY | [spaCy NER / Entity Linking](https://spacy.io/usage/linguistic-features) | 仕様・説明を確認 |
| D-GLINER | [GLiNER](https://github.com/urchade/GLiNER) | 仕様・説明を確認 |
| D-FAISS | [FAISS](https://github.com/facebookresearch/faiss) | 仕様・説明を確認 |
| D-SBERT | [Sentence Transformers](https://www.sbert.net/) | 仕様・説明を確認 |
| D-DOCLING | [Docling](https://github.com/docling-project/docling/blob/main/docs/index.md) | 仕様・説明を確認 |
| D-MARKIT | [MarkItDown](https://github.com/microsoft/markitdown) | 仕様・説明を確認 |
| D-RAGAS | [Ragas metrics](https://docs.ragas.io/en/latest/concepts/metrics/available_metrics/) | 仕様・説明を確認 |
| D-MCP | [Model Context Protocol specification](https://modelcontextprotocol.io/specification/2026-07-28) | 仕様・説明を確認 |
| D-OWASP | [OWASP RAG Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/RAG_Security_Cheat_Sheet.html) | 仕様・説明を確認 |
| D-LANGGRAPH | [LangGraph persistence](https://docs.langchain.com/oss/python/langgraph/persistence) | 仕様・説明を確認 |
| D-LLAMA | [LlamaIndex Memory](https://developers.llamaindex.ai/python/framework/module_guides/deploying/agents/memory/) | 仕様・説明を確認 |
| D-ADK | [Google ADK MemoryService](https://adk.dev/sessions/memory/) | 仕様・説明を確認 |
| D-DBPEDIA | [DBpedia Ontology](https://www.dbpedia.org/resources/ontology/) | 仕様・説明を確認 |
| D-WORDNET | [Princeton WordNet](https://wordnet.princeton.edu/) | 仕様・説明を確認 |
| D-OBO | [OBO Foundry principles](https://obofoundry.org/principles/fp-000-summary.html) | 仕様・説明を確認 |
| D-LANCE | [LanceDB index types](https://docs.lancedb.com/api-reference/index/create-index) | 仕様・説明を確認 |
| D-MILVUS | [Milvus multi-vector hybrid search](https://blog.milvus.io/docs/multi-vector-search.md) | 仕様・説明を確認 |
| D-WEAVIATE | [Weaviate hybrid search](https://docs.weaviate.io/weaviate/concepts/search/hybrid-search) | 仕様・説明を確認 |
| D-VESPA | [Vespa hybrid search](https://docs.vespa.ai/en/learn/tutorials/hybrid-search) | 仕様・説明を確認 |
| D-RDF4J | [RDF4J commit-time SHACL validation](https://rdf4j.org/documentation/programming/shacl/) | 仕様・説明を確認 |
| D-OWLAPI | [OWL API](https://github.com/owlcs/owlapi) | 仕様・説明を確認 |
| D-ELK | [ELK OWL 2 EL reasoner](https://github.com/liveontologies/elk-reasoner) | 仕様・説明を確認 |
| D-OWLREADY | [Owlready2](https://owlready2.readthedocs.io/en/latest/) | 仕様・説明を確認 |
| D-SUDACHI | [Sudachi.rs / SudachiPy](https://github.com/WorksApplications/sudachi.rs) | 仕様・説明を確認 |
| D-BGE | [BGE-M3 model card](https://huggingface.co/BAAI/bge-m3/blob/main/README.md) | 仕様・説明を確認 |
| D-JENARULE | [Jena inference support](https://jena.apache.org/documentation/inference/) | 仕様・説明を確認 |
| D-FASTAPI | [FastAPI Features](https://fastapi.tiangolo.com/features/) | 設計案作成時に型付き API・入力検証の説明を確認。実行・互換性試験は未実施 |
| D-PYDANTIC | [Pydantic Models](https://pydantic.dev/docs/validation/latest/concepts/models/) | 設計案作成時にモデルと検証の説明を確認。実行・互換性試験は未実施 |
| D-SQLALCHEMY | [SQLAlchemy 2.0 Transactions and Connection Management](https://docs.sqlalchemy.org/en/20/orm/session_transaction.html) | 設計案作成時に Session のトランザクション管理を確認。実行試験は未実施 |
| D-ALEMBIC | [Alembic Tutorial](https://alembic.sqlalchemy.org/en/latest/tutorial.html) | 設計案作成時に migration と revision の管理を確認。実行試験は未実施 |
| D-RDFLIB | [RDFLib Overview](https://rdflib.readthedocs.io/en/stable/) | 設計案作成時に RDF 操作・入出力の概要を確認。実行試験は未実施 |
| D-PG-RLS | [PostgreSQL 18 Row Security Policies](https://www.postgresql.org/docs/18/ddl-rowsecurity.html) | 設計案作成時に RLS の適用範囲と所有者・BYPASSRLS の例外を確認。実行試験は未実施 |
| D-PG-FTS | [PostgreSQL 18 Controlling Text Search](https://www.postgresql.org/docs/18/textsearch-controls.html) | 設計案作成時に tsvector・tsquery・ランキング関数の説明を確認。日本語の構成は未実測 |
| D-PG-SELECT | [PostgreSQL 18 SELECT](https://www.postgresql.org/docs/18/sql-select.html) | 設計案作成時に SKIP LOCKED のジョブ取得への適用と照会上の制約を確認。実行試験は未実施 |
| D-UV | [uv Locking and syncing](https://docs.astral.sh/uv/concepts/projects/sync/) | 設計案作成時に lockfile と環境の同期の説明を確認。依存解決は未実施 |

<a id="machine-readable-records"></a>

## 機械可読な台帳

- [sources.json](evidence/sources.json): source ID、種別、URL、確認方法、論文書誌、コードの版。
- [repository-snapshots.json](evidence/repository-snapshots.json): 公開リポジトリから取得したファイルと内容の hash。
- [dependency-inventory.json](evidence/dependency-inventory.json): 取得した manifest の依存宣言。

論文本文やリポジトリ一式のコピーは配布物に含めていない。資料が後日変わった場合、固定した URL と hash を手掛かりに対象版を再確認できる。
