# 資料台帳

[調査トップ](README.md) / [根拠データ](evidence/README.md)

初期台帳の確認日: 2026-09-28。基礎・応用および PageIndex の追加確認日: 2026-09-29（末尾の追加調査を参照）。本文の参照 ID は以下の出典へ直接リンクする。論文は書誌で確認した arXiv の版を固定したものと、要旨のみ確認した補足資料がある。コードの取得範囲と依存宣言の全文は evidence を参照。

## 目次

- [公開実装の固定版](#repositories)
- [論文](#papers)
- [標準仕様](#standards)
- [基礎研究](#foundational-research)
- [確認したコード・manifest](#code-and-manifests)
- [公式文書・技術資料](#official-documents)
- [機械可読な台帳](#machine-readable-records)
- [基礎・応用の追加調査（2026-09-29）](#2026-09-29-の基礎応用の追加調査)
- [PageIndex の追加調査（2026-09-29）](#2026-09-29-の-pageindex-追加調査)

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

## 2026-09-29 の基礎・応用の追加調査

以下は今回の追加資料と、既存資料の追加確認の記録である。上の一覧は 2026-09-28 の調査時点の記録として保持する。仕様・論文は記載した範囲を確認し、製品全体や実験結果を再検証したとは扱わない。

**基礎と応用で確認した一次資料**

<a id="D-BFO"></a>

\[1] [D-BFO — BFO 2020](https://github.com/BFO-ontology/BFO-2020) 2026-09-29

確認範囲: 公式リポジトリを確認。個別領域の導入適合性や全公理は評価していない。
対象: Repository overview。
用途: 科学領域などで共通の上位分類が必要な場合に検討される上位オントロジーの例。

<a id="D-DBPEDIA"></a>

\[2] [D-DBPEDIA — DBpedia Ontology](https://www.dbpedia.org/resources/ontology/) 2026-09-29

確認範囲: プロジェクトのontology説明ページを確認。網羅性やデータ品質は評価していない。
対象: Ontology overview。
用途: 百科事典由来の構造化データ・語彙との連携候補の例。個別の現在事実に対する権威ある根拠とはしない。

<a id="D-DOLCE"></a>

\[3] [D-DOLCE — DOLCE — Laboratory for Applied Ontology](https://www.loa.istc.cnr.it/index.php/dolce/) 2026-09-29

確認範囲: プロジェクトページを開いて確認。理論的な定義やFOL/OWL版の比較は未確認。
対象: Project page。
用途: 上位オントロジーの選択肢の一例。本文では網羅的な理論比較を行わない。

<a id="D-DQV"></a>

\[4] [D-DQV — Data on the Web Best Practices: Data Quality Vocabulary](https://www.w3.org/TR/vocab-dqv/) 2026-09-29

確認範囲: Abstract、Introduction、品質測定・来歴の関連節を確認。
対象: Abstract、1 Introduction、4.1 Quality Measurement、6.2 Document the provenance of the quality metadata。
用途: 品質を用途への適合として扱い、単一スコアや語彙の導入を正しさの保証としない。

<a id="D-GRUBER"></a>

\[5] [D-GRUBER — A Translation Approach to Portable Ontology Specifications](https://tomgruber.org/writing/ontolingua-kaj-1993/) 2026-09-29

確認範囲: 著者ページの書誌情報と要旨のみ確認。論文全文は未確認。
対象: Abstract、publication metadata。
用途: 情報科学におけるオントロジーを、共有された領域の概念化と、そのクラス・関係などの語彙を形式的に記述する仕様として導入する。

<a id="D-HF-KVCACHE"></a>

\[6] [D-HF-KVCACHE — KV cache strategies — Hugging Face Transformers documentation](https://huggingface.co/docs/transformers/v5.17.0/kv_cache) 2026-09-29

確認範囲: 公式文書 v5.17.0 の cache strategy と反復生成に関する節を確認。
対象: KV cache overview、Cache strategies、Iterative generation。
用途: KV cache が attention 計算の key/value 状態を再利用し、速度・メモリー消費とのトレードオフを持つ実行時機構であることを説明する。意味的に整理された会話記憶とは区別する。

<a id="D-OBO"></a>

\[7] [D-OBO — OBO Foundry principles](https://obofoundry.org/principles/fp-000-summary.html) 2026-09-29

確認範囲: 原則の概要にある公開性、識別子、定義、再利用、保守に関する項目を確認。
対象: Summary of OBO Foundry Principles。
用途: OBO Foundryの範囲で、オントロジーの識別子、文書化、語彙再利用、保守を考える原則。

<a id="D-ONTO101"></a>

\[8] [D-ONTO101 — Ontology Development 101: A Guide to Creating Your First Ontology](https://protege.stanford.edu/publications/ontology_development/ontology101-noy-mcguinness.html) 2026-09-29

確認範囲: 定義、スコープ、competency questions、語彙再利用、クラス・プロパティ・個体、評価の関連節を確認。全文精読ではない。
対象: 2 What is in an ontology?、Step 1: Determine the domain and scope、Step 2: Consider reusing existing ontologies、Step 4: Define the classes and the class hierarchy、Step 5: Define the properties of classes、Step 6: Define the facets of the slots、Step 7: Create instances。
用途: 計算機科学での形式的な語彙定義、クラス・プロパティ・個体の区別、単一の唯一解ではない設計、用途とcompetency questionsを起点にした反復的な設計・評価。

<a id="D-ONTOCHECK"></a>

\[9] [D-ONTOCHECK — OntoGPT operation](https://github.com/monarch-initiative/ontogpt/blob/main/docs/operation.md) 2026-09-29

確認範囲: mutableなmain branch上の抽出後term validationの関連節を確認。
対象: Term validation、grounding validation。
用途: 抽出後の用語について存在・obsolete状態・ラベルやsynonymとの一致を確認する手順。候補IDの存在だけでは文脈的な意味の正しさは保証されない。

<a id="D-ONTOGPT"></a>

\[10] [D-ONTOGPT — OntoGPT custom schemas](https://github.com/monarch-initiative/ontogpt/blob/main/docs/custom.md) 2026-09-29

確認範囲: mutableなmain branch上のcustom schemaとterm groundingの関連節を確認。
対象: Custom schemas、id\_prefixes、annotators。
用途: 構造化抽出schemaと既存識別子へのgroundingを組み合わせるツール例。

<a id="D-SCHEMA"></a>

\[11] [D-SCHEMA — Schema.org Data model](https://schema.org/docs/datamodel.html) 2026-09-29

確認範囲: データモデルと適合性に関する関連節を確認。
対象: Data Model、Conformance。
用途: Schema.org をWeb上での共有に向けた実用語彙として紹介し、厳密な業務検証とは役割が異なる点。

<a id="D-WIKIDATA"></a>

\[12] [D-WIKIDATA — Wikidata Data model](https://www.wikidata.org/wiki/Help\:Data_model) 2026-09-29

確認範囲: statement、qualifier、reference、rank の説明箇所を確認。
対象: Statements、Qualifiers、References、Ranks。
用途: statement 中心のデータモデルと時間・根拠などを持つ qualifier/reference の例。rank は校正された確率値とは扱わない。

<a id="D-WORDNET"></a>

\[13] [D-WORDNET — Princeton WordNet](https://wordnet.princeton.edu/) 2026-09-29

確認範囲: 概要、lexical database の説明、保守状況の記述を確認。
対象: About WordNet、WordNet documentation、Development status。
用途: 英語の synset と語彙関係を持つ lexical database の例、および公式ページが新規開発停止とコミュニティ資源を案内する点。

<a id="D-XTDB"></a>

\[14] [D-XTDB — XTDB time semantics](https://docs.xtdb.com/concepts/key-concepts.html) 2026-09-29

確認範囲: 公式 key concepts の temporal columns 節を確認。
対象: Temporal Columns & Bitemporality、Built-in system and valid time columns、Closed-open periods。
用途: XTDBでは system time が変更監査と情報がシステムに入った時点を表し、valid time は利用者が管理して対象世界での有効時点を表す。各時刻の区間は開始を含み終了を含まない。

<a id="H-BADDELEY"></a>

\[15] [H-BADDELEY — Baddeley: The episodic buffer: a new component of working memory? (2000)](https://doi.org/10.1016/S1364-6613\(00\)01538-2) 2026-09-29

確認範囲: 出版社の書誌情報と要旨を確認。全文は未確認。
対象: Abstract、publication metadata。
用途: working memory と episodic buffer の限定容量・情報統合の概念を説明する。人間の認知研究であり、LLMの保存機構との同一性は示さない。

<a id="H-SQUIRE"></a>

\[16] [H-SQUIRE — Squire: Memory systems of the brain: a brief history and current perspective (2004)](https://doi.org/10.1016/j.nlm.2004.06.005) 2026-09-29

確認範囲: 出版社の書誌情報と要旨を確認。全文は未確認。
対象: Abstract、publication metadata。
用途: 記憶を単一のシステムとせず、意識的・非意識的過程など複数のシステムとして扱う見方を紹介する。

<a id="H-TULVING"></a>

\[17] [H-TULVING — Tulving: Episodic memory: from mind to brain (2002)](https://doi.org/10.1146/annurev.psych.53.100901.135114) 2026-09-29

確認範囲: 出版社の書誌情報と要旨を確認。本文の読み込みを完了できず、全文は未確認。
対象: Abstract、publication metadata。
用途: 人間の episodic memory が過去の経験を想起する神経認知システムとして論じられることを説明する。本文では人間と機械の類比の限界に用いる。

<a id="P-CLSURVEY"></a>

\[18] [P-CLSURVEY — Continual Learning of Large Language Models: A Comprehensive Survey](https://arxiv.org/abs/2404.16789v3) 2026-09-29

確認範囲: arXiv HTML v3 の破滅的忘却、継続事前学習・適応、評価に関する節を確認。
対象: Catastrophic Forgetting、Continual Pretraining、Domain Adaptation and Fine-Tuning、Evaluation。
用途: 継続学習を時間とともに新しいデータ・課題へ適応する研究領域として位置づけ、過去課題の性能保持と破滅的忘却を評価課題に含める説明を支える。

<a id="P-COALA"></a>

\[19] [P-COALA — Cognitive Architectures for Language Agents](https://arxiv.org/abs/2309.02427v3) 2026-09-29

確認範囲: arXiv HTML v3 のメモリー・検索・推論・学習・意思決定・事例・考察の関連節を確認。
対象: §4.1 Memory、§4.3 Retrieval、§4.4 Reasoning、§4.5 Learning、§4.6 Decision-making、Case Studies、Actionable Insights、Discussion。
用途: working、episodic、semantic、procedural memory とエージェントの読み書き操作を整理する枠組み、および procedural memory の書き換えに伴う危険を説明する。

<a id="P-GEN"></a>

\[20] [P-GEN — Generative Agents: Interactive Simulacra of Human Behavior](https://arxiv.org/abs/2304.03442v2) 2026-09-29

確認範囲: arXiv HTML v2 の記憶ストリーム、検索、reflection、評価と失敗例の関連節を確認。
対象: Memory and Retrieval、Reflection、Controlled Evaluation、Errors and Limitations。
用途: 記憶ストリームから関連度・新しさ・重要度で記録を検索し、reflection を計画に利用する設計例と、検索失敗や記憶にない細部の補完を説明する。

<a id="P-LIM"></a>

\[21] [P-LIM — Lost in the Middle: How Language Models Use Long Contexts](https://arxiv.org/abs/2307.03172v3) 2026-09-29

確認範囲: arXiv HTML v3 の要旨、複数文書 QA・KV retrieval の設定と結果を確認。
対象: Abstract、Multi-Document QA、KV Retrieval、Results。
用途: 評価されたモデルとタスクでは、必要情報の入力内位置が長文脈利用の成績に影響したことを説明する。全ての現行モデルに一般化しないという留保も支える。

<a id="P-LLMOL"></a>

\[22] [P-LLMOL — LLMs4OL: Large Language Models for Ontology Learning](https://arxiv.org/abs/2307.16648v2) 2026-09-29

確認範囲: 論文ページの書誌情報とabstractのみ確認。本文の実験結果は未確認。
対象: Abstract、publication metadata。
用途: ontology learning の課題を用語型付け、分類階層発見、非分類関係抽出などに分ける研究例。性能の一般化には使わない。

<a id="P-LME"></a>

\[23] [P-LME — LongMemEval: Benchmarking Chat Assistants on Long-Term Interactive Memory](https://arxiv.org/abs/2410.10813v2) 2026-09-29

確認範囲: arXiv HTML v2 の要旨、データセット構成、能力分類と評価設定を確認。
対象: Abstract、Benchmark Construction、Question Categories、Evaluation。
用途: 長い会話履歴に対する500問のベンチマークと、抽出・複数セッション・時間推論・知識更新・回答保留の評価範囲を説明する。

<a id="P-MBANK"></a>

\[24] [P-MBANK — MemoryBank: Enhancing Large Language Models with Long-Term Memory](https://arxiv.org/abs/2305.10250v3) 2026-09-29

確認範囲: arXiv HTML v3 の要旨、ログ・要約・プロフィールの保存、検索、更新・忘却の関連節と実験設定を確認。
対象: Abstract、Storage、Retrieval、Update and Forgetting、Experiments。
用途: 対話ログ・階層要約・プロフィールを併用する保存設計例、および忘却モデルが簡略化された探索的提案であるという限界を説明する。

<a id="P-MEMIT"></a>

\[25] [P-MEMIT — Mass-Editing Memory in a Transformer](https://arxiv.org/abs/2210.07229v2) 2026-09-29

確認範囲: arXiv HTML v2 の一括編集手法、GPT-J/GPT-NeoXでの評価と結果を確認。
対象: Abstract、MEMIT Method、Experiments、Results。
用途: Transformer重みの複数事実編集を対象とする研究例と、多数の編集時にも特異性などにトレードオフがあることを説明する。

<a id="P-MGPT"></a>

\[26] [P-MGPT — MemGPT: Towards LLMs as Operating Systems](https://arxiv.org/abs/2310.08560v2) 2026-09-29

確認範囲: arXiv HTML v2 のアーキテクチャ、context管理、ツール操作、評価の関連節を確認。
対象: Abstract、Main Context and External Context、Function Calls、Evaluation。
用途: 有限のモデル context と外部ストレージ間で情報を移動する仮想メモリー風の制御設計を説明する。情報の真偽判定や矛盾解消までは保証しない。

<a id="P-RAG"></a>

\[27] [P-RAG — Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401v4) 2026-09-29

確認範囲: arXiv HTML v4 の要旨、モデル構成、実験の関連節を確認。
対象: Abstract、RAG Model、Experiments。
用途: パラメーターに保持された知識と検索可能な非パラメトリック文書索引を組み合わせる方式を説明する。RAGが記憶そのものではなく情報取得・生成の方式であるという整理の根拠。

<a id="P-REFLEX"></a>

\[28] [P-REFLEX — Reflexion: Language Agents with Verbal Reinforcement Learning](https://arxiv.org/abs/2303.11366v4) 2026-09-29

確認範囲: arXiv HTML v4 の要旨、episodic memory、手法と実験結果の関連節を確認。
対象: Abstract、Episodic Memory、Method、Results。
用途: タスクのフィードバックを文章の振り返りとして episodic buffer に保存し、モデル重みの更新なしに次の試行で参照する設計例を説明する。

<a id="P-ROME"></a>

\[29] [P-ROME — Locating and Editing Factual Associations in GPT](https://arxiv.org/abs/2202.05262v5) 2026-09-29

確認範囲: arXiv HTML v5 の因果追跡、ROME編集手法、CounterFact評価と限界を確認。
対象: Abstract、Causal Tracing、ROME、CounterFact、Limitations。
用途: 特定のGPT系モデル・評価設定での事実関連付けの重み編集と、その局所性・一般化・スケール上の限界を説明する。外部記録の訂正や削除の代替とはしない。

<a id="P-VOYAGER"></a>

\[30] [P-VOYAGER — Voyager: An Open-Ended Embodied Agent with Large Language Models](https://arxiv.org/abs/2305.16291v2) 2026-09-29

確認範囲: arXiv HTML v2 の要旨、自動カリキュラム、技能ライブラリ、自己検証の関連節を確認。
対象: Abstract、Automatic Curriculum、Skill Library、Self-Verification。
用途: Minecraft環境でコード技能を作成・検索し、エージェント側の自己検証機構による成功判定を経て技能ライブラリへ加える設計例を説明する。形式検証や他環境での安全性の証拠とはしない。

<a id="S-NARY"></a>

\[31] [S-NARY — Defining N-ary Relations on the Semantic Web](https://www.w3.org/TR/swbp-n-aryRelations/) 2026-09-29

確認範囲: 関連をクラスとして導入する pattern と追加属性の例を確認。 / General issues と Pattern 1 の関連部分を確認。
対象: General issues、Pattern 1: Introducing a new class for a relation、Use Case 1: additional attributes describing a relation。
用途: 期間や追加属性を持つ多項関係を relation node / assertion node として表現する設計例。 追加の期間・根拠を持つ主張をノードにする独自例の設計パターン。

<a id="S-OWL"></a>

\[32] [S-OWL — OWL 2 Primer](https://www.w3.org/TR/owl2-primer/) 2026-09-29

確認範囲: 開世界、UNA、機能的プロパティ、sameAs、OWLの用途に関する節を確認。
対象: Open World Assumption、No Unique Name Assumption、Functional Properties、Same Individuals、OWL and Database Schemas。
用途: OWL の開世界と非UNAの前提、functional property の含意、`owl:sameAs` の同一性、OWLとデータ入力スキーマの役割の違い。

<a id="S-PROFILES"></a>

\[33] [S-PROFILES — OWL 2 Profiles](https://www.w3.org/TR/owl2-profiles/) 2026-09-29

確認範囲: EL、QL、RL の設計意図と適用領域の概要を確認。
対象: OWL 2 EL、OWL 2 QL、OWL 2 RL。
用途: OWL profile ごとの設計意図と表現上のトレードオフ。理論的計算特性から個別データでの実行速度を保証しない。

<a id="S-PROV"></a>

\[34] [S-PROV — PROV-O](https://www.w3.org/TR/prov-o/) 2026-09-29

確認範囲: Entity、Activity、Agent、派生・帰属の関連語彙を確認。 / Starting Point terms と派生・版の語彙定義を確認。
対象: Starting Point Terms、`prov:wasDerivedFrom`、`prov:wasAttributedTo`、3.1 Starting Point terms、`prov:wasRevisionOf`。
用途: 資料や主張の派生・帰属を表す語彙例。PROV の関係だけで出典の信頼性や主張の真偽を自動認定しない。 wasDerivedFrom の向きと意味、主張記録と原資料を `prov:Entity` として表す例。

<a id="S-PROV-PRIMER"></a>

\[35] [S-PROV-PRIMER — PROV Model Primer](https://www.w3.org/TR/prov-primer/) 2026-09-29

確認範囲: 本文の導入と関連節を確認。
対象: 1 Introduction、2.1 Entities、2.2 Activities、2.4 Agents and Responsibility、2.6 Derivation and Revision、2.8 Time。
用途: 原資料、抽出処理、主張の版と生成・派生の関係を説明する。真偽の自動認定や撤回処理の自動化は主張しない。

<a id="S-RDF"></a>

\[36] [S-RDF — RDF 1.1 Concepts](https://www.w3.org/TR/rdf11-concepts/) 2026-09-29

確認範囲: triple、graph/dataset、RDF terms と graph 名の意味に関する節を確認。
対象: RDF Graphs、RDF Datasets、RDF Terms、RDF Semantics。
用途: RDF triple・graph・dataset の基本説明、および named graph の名前だけで出典や時間の意味が確定しないという注意。

<a id="S-RDF-SEM"></a>

\[37] [S-RDF-SEM — RDF 1.1 Semantics](https://www.w3.org/TR/rdf11-mt/) 2026-09-29

確認範囲: 単調性に関する公式仕様の記述と補題を確認。
対象: Monotonicity Lemma、RDF Entailment。
用途: RDF entailment の単調性。知識追加で含意が増えることと、既存結論を訂正・撤回するアプリケーション方針は別の責務であること。

<a id="S-RDF12"></a>

\[38] [S-RDF12 — RDF 1.2 Concepts, Candidate Recommendation 2026-04-07](https://www.w3.org/TR/2026/CR-rdf12-concepts-20260407/) 2026-09-29

確認範囲: Snapshot status note と triple term に関する概要を確認。
対象: Status of This Document、RDF Triple Terms。
用途: リンク先の2026-04-07 Snapshot に triple term 等が記述され、当該文書は Recommendation ではないこと。現在の実装対応を保証する資料として使わない。

<a id="S-RDFS"></a>

\[39] [S-RDFS — RDF Schema 1.1](https://www.w3.org/TR/rdf-schema/) 2026-09-29

確認範囲: 型付け、クラス階層、domain/range と subproperty の意味を確認。
対象: Classes、Properties、`rdfs:domain`、`rdfs:range`、`rdfs:subClassOf`、`rdfs:subPropertyOf`。
用途: RDFS domain/range は関係の両端にクラス所属を含意し、入力違反を報告する SHACL 型の制約ではないこと。

<a id="S-SHACL"></a>

\[40] [S-SHACL — Shapes Constraint Language](https://www.w3.org/TR/shacl/) 2026-09-29

確認範囲: data graph と shapes graph の検証、型・個数制約、推論要件の関連節を確認。 / 型・個数の制約と検証の関連節を確認。
対象: Validation and Graphs、`sh:class`、Cardinality Constraint Components、Inference、3 Validation and Graphs、4.1.1 `sh:class`、4.2 Cardinality Constraint Components。
用途: SHACL によるデータ形状の検証と報告。SHACL検証は主張の現実世界での真偽を確かめず、RDFS推論も常に必須ではないこと。 人物・組織の型と根拠リンクを要求する独自 shape。真実、期間整合、兼業の禁止を検査するとは解釈しない。

<a id="S-SKOS"></a>

\[41] [S-SKOS — SKOS Reference](https://www.w3.org/TR/skos-reference/) 2026-09-29

確認範囲: SKOS概念体系、broader関係、mappingと個体同一性の違いを確認。
対象: SKOS Concepts、`skos:broader`、Mapping Properties、`owl:sameAs`。
用途: SKOS を分類体系・シソーラス等の概念モデルとして位置づけ、`skos:broader` と OWL subclass、SKOS mapping と `owl:sameAs` の意味を区別する。

<a id="S-SPARQL-QUERY"></a>

\[42] [S-SPARQL-QUERY — SPARQL 1.1 Query Language](https://www.w3.org/TR/sparql11-query/) 2026-09-29

確認範囲: SELECT と基本 graph pattern の導入・例を確認。仕様全文は未確認。 / 基本グラフパターン・SELECT・関数の関連節を確認。
対象: Making Simple Queries、Basic Graph Patterns、SELECT、2 Making Simple Queries、10.2 VALUES、17.4.1.1 bound。
用途: RDF graph 上の triple pattern を SELECT で照会する例。推論結果の対象範囲は SPARQL エンドポイントの構成に依存する。 独自の主張スキーマから主張と出典を取得する SELECT 例。照会前の権限・時間の絞り込みは例の前提として分離。

<a id="S-TIME"></a>

\[43] [S-TIME — Time Ontology in OWL](https://www.w3.org/TR/owl-time/) 2026-09-29

確認範囲: 時間的実体と開始・終了時刻を表す関連語彙を確認。
対象: Temporal Entity、hasBeginning、hasEnd。
用途: 時間点・時間区間を記述する語彙の例。アプリケーションの履歴保持・競合解決・更新方針は別途必要であること。

<a id="T-AGM"></a>

\[44] [T-AGM — AGM: Partial meet contraction and revision functions (1985)](https://doi.org/10.2307/2274239) 2026-09-29

確認範囲: 論文の書誌情報・要旨と部分 meet contraction の定義範囲を確認。
対象: Abstract、partial meet contraction and revision functions。
用途: 信念改訂・縮約と、部分 meet 縮約の公理的性質を基本理論として説明する。

<a id="T-PROVENANCE"></a>

\[45] [T-PROVENANCE — Green et al.: Provenance Semirings (2007)](https://web.cs.ucdavis.edu/~green/papers/pods07.pdf) 2026-09-29

確認範囲: 著者公開論文の要旨と書誌情報を確認。
対象: Abstract、PODS 2007 bibliographic header。
用途: 関係代数・Datalog の入力から出力へ伝播する来歴注釈を provenance semiring で表す理論を説明する。

<a id="T-TMS"></a>

\[46] [T-TMS — Doyle: A truth maintenance system (1979)](https://www.sciencedirect.com/science/article/pii/0004370279900080) 2026-09-29

確認範囲: 出版社の書誌情報と要旨を確認。
対象: Abstract、publication metadata。
用途: TMS がプログラムの信念を支える理由を記録し、新情報に応じて仮定と信念を改訂する考えを説明する。

<a id="T-UPDATE"></a>

\[47] [T-UPDATE — Friedman and Halpern: Modeling Belief in Dynamic Systems, Part II](https://arxiv.org/abs/cs/9903016) 2026-09-29

確認範囲: arXiv の書誌情報と要旨を確認。
対象: Abstract、Journal reference。
用途: belief revision と belief update の区別、Katsuno–Mendelzon の update に置かれる仮定と適用範囲を説明する。

## 2026-09-29 の PageIndex 追加調査

[調査本文](systems/pageindex.md)。本体と評価資料のコミットを固定し、公式文書の確認日を記録した。性能値は開発元報告であり、実行による追試ではない。

**PageIndex の出典と確認範囲**

<a id="D-PI-START"></a>

\[1] [D-PI-START — PageIndex Docs — Getting Started](https://docs.pageindex.ai/getting-started) 2026-09-29

確認範囲: Index / Retrieve、Local / Cloud、Quick Start と Integrations を確認。サービスは実行していない。

用途: 2026-09-18 更新の現行 SDK の入口。ローカル索引とクラウド索引の区別。

<a id="D-PI-CLIENT"></a>

\[2] [D-PI-CLIENT — PageIndex Docs — Client Configuration](https://docs.pageindex.ai/sdk/client) 2026-09-29

確認範囲: index / chat の設定、保存先、プロバイダー設定、backend overrides、パラメーター表を確認。接続は未実行。

用途: 索引と回答のモデルを分ける API、保存先 ./.pageindex、外部 LLM または互換エンドポイントの利用。

<a id="D-PI-DOCS"></a>

\[3] [D-PI-DOCS — PageIndex Docs — Document Processing](https://docs.pageindex.ai/sdk/documents) 2026-09-29

確認範囲: Index、Read、Manage、Metadata、Folders の関連節を確認。クラウドの内部処理と削除の完全性は未検証。

用途: 入力形式、同期・非同期、get\_tree のテキストと要約、ページ・ブロック、文書削除、metadata/folder の境界。

<a id="D-PI-CHAT"></a>

\[4] [D-PI-CHAT — PageIndex Docs — LLM Integration](https://docs.pageindex.ai/sdk/chat) 2026-09-29

確認範囲: Send a query、Streaming、Citations の関連節を確認。API protocols は概要のみ。回答や引用の正しさは未測定。

用途: 複数文書・会話履歴、citations=True、ページとブロック参照、引用の解決と表示。

<a id="D-PI-AGENTS"></a>

\[5] [D-PI-AGENTS — PageIndex Docs — Agent Integration](https://docs.pageindex.ai/sdk/agents) 2026-09-29

確認範囲: ツールと指示の公開、include\_management、SDK 別統合、document\_context / folder\_context の範囲を確認。統合コードは未実行。

用途: ツールは既定で読取用。文書・フォルダーの context は検索の誘導でありアクセス制限ではない。local の in-process MCP と managed remote MCP の区別。

<a id="D-PI-FLASH"></a>

\[6] [D-PI-FLASH — Introducing PageIndex Flash — Fast Local Tree Indexing for PDFs](https://pageindex.ai/blog/pageindex-flash) 2026-09-29

確認範囲: Flash の構造抽出、SDK、対象 PDF、Local / Cloud 比較、費用・評価の説明を確認。数値は開発元報告で再実行していない。

用途: PDF レイアウトを使う現行索引、索引用と検索用モデルの分離、画像・スキャン文書の制約。

<a id="D-PI-FILESYSTEM"></a>

\[7] [D-PI-FILESYSTEM — PageIndex File System: Massive-Scale Document Search](https://pageindex.ai/blog/pageindex-filesystem) 2026-09-29

確認範囲: 仮想ノード、質問依存の階層、dynamic flattening、Enterprise / Cloud の提供説明を確認。大規模実験や非公開エンジンは未検証。

用途: 文書間探索の上位層という設計説明。百万文書規模は提供者の主張であり、OSS ローカル版で確認した性能ではない。

<a id="D-PI-README"></a>

\[8] [D-PI-README — PageIndex README](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/README.md) 2026-09-29

確認範囲: Quickstart、Benchmarks、Cloud、更新案内を固定版で確認。実行していない。

用途: SDK Local / Flash の現行入口と提供者の性能主張。

<a id="C-PI-MANIFEST"></a>

\[9] [C-PI-MANIFEST — PageIndex package manifest](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pyproject.toml) 2026-09-29

確認範囲: manifest 全体の宣言を確認。実行していない。

用途: 版 0.2.10、Python 要件、runtime / extras / build の宣言。インストールは未実施。

<a id="C-PI-LICENSE"></a>

\[10] [C-PI-LICENSE — PageIndex MIT License](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/LICENSE) 2026-09-29

確認範囲: ライセンス本文を確認。実行していない。

用途: リポジトリ本体のライセンス。依存と Cloud の契約条件を含めない。

<a id="C-PI-REQUIREMENTS"></a>

\[11] [C-PI-REQUIREMENTS — PageIndex requirements](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/requirements.txt) 2026-09-29

確認範囲: 宣言全体を確認。実行していない。

用途: 一部固定された requirements と pyproject の範囲指定の違い。

<a id="D-PI-FLASH-README"></a>

\[12] [D-PI-FLASH-README — PageIndex Flash README](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/flash/README.md) 2026-09-29

確認範囲: Usage、Output、Benchmark の関連箇所を確認。実行していない。

用途: 低水準 API、ページ範囲、toc\_source、9 文書の索引評価の説明。

<a id="C-PI-FLASH-API"></a>

\[13] [C-PI-FLASH-API — PageIndex Flash public API](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/flash/api.py) 2026-09-29

確認範囲: page\_index\_flash、flash\_rejection\_reason、要約・最適化の呼出経路を静的確認。実行していない。

用途: LLM を使わない初期抽出と要約・expand、10 ノード超の平坦な結果の拒否、物理ページ範囲。

<a id="C-PI-CLIENT"></a>

\[14] [C-PI-CLIENT — PageIndexClient](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/client.py) 2026-09-29

確認範囲: client 初期化・公開メソッド・\_local\_doc\_scope・document\_context の関連箇所を静的確認。実行していない。

用途: SDK 入口、文書を対象にする指示と local chat の許可 ID 集合の違い。全 API 経路は未監査。

<a id="C-PI-LOCAL"></a>

\[15] [C-PI-LOCAL — PageIndex LocalAPI](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/local_api.py) 2026-09-29

確認範囲: submit\_document、\_index\_flash / standard、get\_tree、形式変換を静的確認。実行していない。

用途: PDF 限定、同期索引、metadata 保存、UUID 発行、ページ抽出、公開 tree への変換。

<a id="C-PI-STORE"></a>

\[16] [C-PI-STORE — PageIndex local DocStore](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/local_store.py) 2026-09-29

確認範囲: 保存・読取・削除・manifest の関連処理を静的確認。実行していない。

用途: JSON ファイル構成と削除経路。原本保存、完全消去、運用時の整合性は検証していない。

<a id="C-PI-TOOLS"></a>

\[17] [C-PI-TOOLS — PageIndex document tools and instructions](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/agent_tools.py) 2026-09-29

確認範囲: ツール定義、構造読取、\_tool\_specs、既定指示、文書指定の関連処理を静的確認。実行していない。

用途: ローカルの 4 読取ツール、応答分割、20 ページを境にした読取指示、外部 agent ツールの範囲。全例外経路は未監査。

<a id="C-PI-CHAT"></a>

\[18] [C-PI-CHAT — PageIndex own-model chat](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/local_chat.py) 2026-09-29

確認範囲: \_openai\_agent、\_chat\_agent、モデル接続・文書スコープの関連経路を静的確認。実行していない。

用途: ツールを LLM エージェントへ渡す検索・回答経路。全 protocol / provider の動作は未検証。

<a id="C-PI-OPT"></a>

\[19] [C-PI-OPT — PageIndex tree optimization](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/tree_optimize.py) 2026-09-29

確認範囲: 冒頭のコスト定義、merge / expand の入口、propose\_children を静的確認。実行していない。

用途: ページ単位の代理コスト、決定的 merge、LLM expand、見出しの本文照合。計算量・性能の証明や実測ではない。

<a id="C-PI-CLI"></a>

\[20] [C-PI-CLI — PageIndex CLI entry](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/run_pageindex.py) 2026-09-29

確認範囲: 引数、Flash / standard / Markdown 分岐、出力処理を静的確認。実行していない。

用途: CLI と SDK の受付形式の違い、Flash 既定、失敗時の standard 案内。

<a id="C-PI-CLASSIC"></a>

\[21] [C-PI-CLASSIC — PageIndex classic indexing](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/page_index_classic.py) 2026-09-29

確認範囲: 目次・開始ページ検証と meta\_processor / tree\_parser の関連箇所を静的確認。実行していない。

用途: standard 経路で LLM を利用する目次処理が残ること。全文精読や実行ではない。

<a id="C-PI-UTILS"></a>

\[22] [C-PI-UTILS — PageIndex LLM and summary utilities](https://github.com/VectifyAI/PageIndex/blob/619cbd89f6dd02681a8cfc5d00b1f9b4848e973e/pageindex/utils.py) 2026-09-29

確認範囲: llm\_completion / llm\_acompletion と leaf / parent summary のプロンプト・呼出を静的確認。実行していない。

用途: 本文・子要約を接続先 LLM へ渡す構造。実通信の記録・監査は未実施。

<a id="D-PI-OSS-BENCH"></a>

\[23] [D-PI-OSS-BENCH — GitHub - VectifyAI/PageIndex-OSS-Benchmark](https://github.com/VectifyAI/PageIndex-OSS-Benchmark/blob/ad4c0b92970a6f4801f09ff2e647389e8f5874fa/README.md) 2026-09-29

確認範囲: mainのcommit SHA/dateをGitHub APIで取得し、README.mdをSHA固定raw URLから保存して読了。全runnerの再実行はしていない。

用途: PageIndex local mode (PageIndexClient()、PageIndex Cloud API keyなし（LLM の認証は別）)、Flash indexing、OCRなし。62問・34 PDF・1,945ページ。running text内のlookup事実だけを対象とし、chart/table/figure/計算とFlashが拒否した文書を除外。全index treeはgpt-5.6-luna、MMLongBench-Doc-V2のreference-answer semantic-equivalence judge (元PDFは読まない) で採点する条件を確認。README表のaccuracyは各model/effort構成で85.5–100%。

<a id="D-PI-FINANCE"></a>

\[24] [D-PI-FINANCE — GitHub - VectifyAI/Mafin2.5-FinanceBench: FinanceBench evaluation of Mafin 2.5 (Powered by PageIndex)](https://github.com/VectifyAI/Mafin2.5-FinanceBench/blob/1c890d5e0fd9929953d38282614555847727011d/README.md) 2026-09-29

確認範囲: mainのcommit SHA/dateをGitHub APIで取得し、README.mdをSHA固定raw URLから保存して読了。

用途: 98.7%の帰属先がMafin 2.5であること、提供元の全件評価と2基盤モデル主張、比較対象のカバレッジ、正解曖昧性および単一文書寄りの限界。

<a id="D-PI-FINANCE-EVAL"></a>

\[25] [D-PI-FINANCE-EVAL — eval.py - Mafin2.5-FinanceBench](https://github.com/VectifyAI/Mafin2.5-FinanceBench/blob/1c890d5e0fd9929953d38282614555847727011d/eval.py) 2026-09-29

確認範囲: commit固定版eval.pyを保存して読み、既定判定器、正誤同値判定プロンプト、結果ファイル既定名とhybridの集約を確認。実行はしていない。

用途: 既定GPT-4o判定、丸め・推論・妥当解釈を広く認める基準、hybridではいずれかの判定器が正解とした場合に正解扱いする実装。

## 2026-09-29 の関連システム再調査

[横断比較](systems/extended-landscape.md)。機能の説明、選択コード、要旨のみの研究を区別して記録する。

**関連システムの出典と確認範囲**

<a id="D-LM-OPENV-SESSION"></a>

\[1] [D-LM-OPENV-SESSION — OpenViking: Session Concept](https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/docs/en/concepts/08-session.md) 2026-09-29

確認範囲: session の関連節と session 完了時の説明を確認。サービス API は実行していない。

固定版: `1f4f7039fc394c5d04637828166f4e4e74e249e0`。

<a id="C-LM-OPENV-SESSION"></a>

\[2] [C-LM-OPENV-SESSION — OpenViking: session.py](https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/openviking/session/session.py) 2026-09-29

確認範囲: session state と commit / memory processing の関連経路を静的確認。ファイル全体精読・実行はしていない。

固定版: `1f4f7039fc394c5d04637828166f4e4e74e249e0`。

<a id="C-LM-OPENV-LIFECYCLE"></a>

\[3] [C-LM-OPENV-LIFECYCLE — OpenViking: memory\_lifecycle.py](https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/openviking/retrieve/memory_lifecycle.py) 2026-09-29

確認範囲: hotness と半減期計算の実装を静的確認。実データで挙動は測っていない。

固定版: `1f4f7039fc394c5d04637828166f4e4e74e249e0`。

<a id="C-LM-OPENV-RETRIEVER"></a>

\[4] [C-LM-OPENV-RETRIEVER — OpenViking: hierarchical\_retriever.py](https://github.com/volcengine/OpenViking/blob/1f4f7039fc394c5d04637828166f4e4e74e249e0/openviking/retrieve/hierarchical_retriever.py) 2026-09-29

確認範囲: 階層 retrieval の候補選択・展開に関係する箇所を静的確認。実行はしていない。

固定版: `1f4f7039fc394c5d04637828166f4e4e74e249e0`。

<a id="C-LM-MOS-UPDATER"></a>

\[5] [C-LM-MOS-UPDATER — MemoryOS: updater.py](https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/memoryos-pypi/updater.py) 2026-09-29

確認範囲: FIFO buffer 切り出し、page 生成、連続性と要約の経路を静的確認。実行はしていない。

固定版: `587ed7755c7aed179965792830ff1b5ad9a6fa92`。

<a id="C-LM-MOS-RETRIEVER"></a>

\[6] [C-LM-MOS-RETRIEVER — MemoryOS: retriever.py](https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/memoryos-pypi/retriever.py) 2026-09-29

確認範囲: mid-term page、user long-term memory、assistant knowledge を組み合わせる検索箇所を静的確認。実行はしていない。

固定版: `587ed7755c7aed179965792830ff1b5ad9a6fa92`。

<a id="C-LM-MOS-LTM"></a>

\[7] [C-LM-MOS-LTM — MemoryOS: long\_term.py](https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/memoryos-pypi/long_term.py) 2026-09-29

確認範囲: profile / knowledge store の更新・容量管理と embedding retrieval を静的確認。全経路実行はしていない。

固定版: `587ed7755c7aed179965792830ff1b5ad9a6fa92`。

<a id="C-LM-MOS-CORE"></a>

\[8] [C-LM-MOS-CORE — MemoryOS: memoryos.py](https://github.com/BAI-LAB/MemoryOS/blob/587ed7755c7aed179965792830ff1b5ad9a6fa92/memoryos-pypi/memoryos.py) 2026-09-29

確認範囲: 主要 API と updater / retriever / store 接続を静的確認。実行・依存監査はしていない。

固定版: `587ed7755c7aed179965792830ff1b5ad9a6fa92`。

<a id="D-LM-REME-FILE"></a>

\[9] [D-LM-REME-FILE — ReMe: Memory as File](https://github.com/agentscope-ai/ReMe/blob/bebad3674573477ad294ca44eb15f508feea2665/docs/en/memory_as_file.md) 2026-09-29

確認範囲: Memory-as-File の構成、正本と rebuildable metadata、旧 MemoryScope 系譜を確認。製品は実行していない。

固定版: `bebad3674573477ad294ca44eb15f508feea2665`。

<a id="D-LM-REME-SEARCH"></a>

\[10] [D-LM-REME-SEARCH — ReMe: Memory Search](https://github.com/agentscope-ai/ReMe/blob/bebad3674573477ad294ca44eb15f508feea2665/docs/en/memory_search.md) 2026-09-29

確認範囲: 検索方式、rank fusion、索引更新・削除の説明を確認。動作検証はしていない。

固定版: `bebad3674573477ad294ca44eb15f508feea2665`。

<a id="C-LM-REME-AUTO"></a>

\[11] [C-LM-REME-AUTO — ReMe: auto\_memory.py](https://github.com/agentscope-ai/ReMe/blob/bebad3674573477ad294ca44eb15f508feea2665/reme/steps/evolve/auto_memory.py) 2026-09-29

確認範囲: session JSONL 保存と agent wrapper による daily note 作成・更新の呼び出し経路を静的確認。全体精読・実行はしていない。

固定版: `bebad3674573477ad294ca44eb15f508feea2665`。

<a id="C-LM-REME-SEARCH"></a>

\[12] [C-LM-REME-SEARCH — ReMe: search.py](https://github.com/agentscope-ai/ReMe/blob/bebad3674573477ad294ca44eb15f508feea2665/reme/steps/index/search.py) 2026-09-29

確認範囲: keyword/vector の並行検索と順位 fusion の関連経路を静的確認。実行していない。

固定版: `bebad3674573477ad294ca44eb15f508feea2665`。

<a id="D-LM-MEMU-CURRENT"></a>

\[13] [D-LM-MEMU-CURRENT — memU: repository README (current page)](https://github.com/NevaMind-AI/memU/blob/main/README.md) 2026-09-29

確認範囲: README の MemoryService と host-agent responsibilities の関連節を確認。固定 commit とコードは未確認。

<a id="D-LM-MIRIX-README"></a>

\[14] [D-LM-MIRIX-README — MIRIX: repository README](https://github.com/Mirix-AI/MIRIX/blob/8cb06a62bbb7c478beb33dd4f2815696a72df482/README.md) 2026-09-29

確認範囲: README の製品構成・更新説明を確認。コード・実行経路は未監査。

固定版: `8cb06a62bbb7c478beb33dd4f2815696a72df482`。

<a id="D-LM-MEMORI"></a>

\[15] [D-LM-MEMORI — Memori BYODB: How Memory Works](https://memorilabs.ai/docs/memori-byodb/concepts/how-memory-works/) 2026-09-29

確認範囲: BYODB の概念ページで attribution scope、抽出対象、auto-recall、augmentation 説明を確認。SDK/API は実行していない。

<a id="D-LM-MEMVID-INTRO"></a>

\[16] [D-LM-MEMVID-INTRO — Memvid v2: Glossary](https://docs.memvid.com/introduction/glossary) 2026-09-29

確認範囲: v2 glossary の frame、timeline、index、storage semantics を確認。SDK・ファイル操作はしていない。

<a id="D-LM-MEMVID-CLI"></a>

\[17] [D-LM-MEMVID-CLI — Memvid v2: CLI documentation](https://docs.memvid.com/cli) 2026-09-29

確認範囲: CLI docs の追加・検索・frame 状態の説明を確認。CLI は実行していない。

<a id="D-LM-MEMSEARCH-README"></a>

\[18] [D-LM-MEMSEARCH-README — zilliztech/memsearch: README](https://github.com/zilliztech/memsearch/blob/2a4652fa086fbd45e92bfd8da7781ebe1642baa7/README.md) 2026-09-29

確認範囲: README の Markdown source of truth、memory layout、index backend、private log guidance を確認。コードは未監査。

固定版: `2a4652fa086fbd45e92bfd8da7781ebe1642baa7`。

<a id="D-LM-MEMSEARCH-GETTING"></a>

\[19] [D-LM-MEMSEARCH-GETTING — zilliztech/memsearch: Getting started](https://github.com/zilliztech/memsearch/blob/2a4652fa086fbd45e92bfd8da7781ebe1642baa7/docs/getting-started.md) 2026-09-29

確認範囲: getting started の storage and search setup options を確認。コードは実行していない。

固定版: `2a4652fa086fbd45e92bfd8da7781ebe1642baa7`。

<a id="D-LM-ACONTEXT-LEARN"></a>

\[20] [D-LM-ACONTEXT-LEARN — Acontext: Quick Learn guide](https://docs.acontext.io/learn/quick) 2026-09-29

確認範囲: learning overview の skill learning / task artifacts 節を確認。サービスは実行していない。

<a id="D-LM-ACONTEXT-SPACE"></a>

\[21] [D-LM-ACONTEXT-SPACE — Acontext: Learning Spaces](https://docs.acontext.io/learn/learning-spaces) 2026-09-29

確認範囲: learning-space create / delete の関連節を確認。API は実行していない。

<a id="D-LM-ACE-REPO"></a>

\[22] [D-LM-ACE-REPO — ACE Agentic Context Engineering repository](https://github.com/ace-agent/ace/tree/82709de050e1db6e6ef2f07bcb0393560b94992a) 2026-09-29

確認範囲: repository README と主な入口説明を確認。実行・全コード監査なし。

固定版: `82709de050e1db6e6ef2f07bcb0393560b94992a`。

<a id="D-LM-MSKILLS-REPO"></a>

\[23] [D-LM-MSKILLS-REPO — Memento-Skills repository](https://github.com/Memento-Teams/Memento-Skills/tree/ee9b9a45efd093d669c06fe318b4b1dceb246d19) 2026-09-29

確認範囲: 固定版の repository README / runtime scope を確認。コード監査と実行なし。

固定版: `ee9b9a45efd093d669c06fe318b4b1dceb246d19`。

<a id="P-LM-SKILLWEAVER"></a>

\[24] [P-LM-SKILLWEAVER — SkillWeaver: Web Agents can Self-Improve by Discovering and Honing Skills](https://arxiv.org/abs/2504.07079v1) 2026-09-29

確認範囲: abstract、method、limitations and evaluation claims を確認。benchmark は再実行していない。

<a id="D-LM-SKILLWEAVER-REPO"></a>

\[25] [D-LM-SKILLWEAVER-REPO — OSU-NLP-Group/SkillWeaver](https://github.com/OSU-NLP-Group/SkillWeaver) 2026-09-29

確認範囲: 論文から参照される repository の概要を確認。現在の code paths は追跡・実行していない。

<a id="P-LM-AGENTKB"></a>

\[26] [P-LM-AGENTKB — Agent KB: Leveraging Cross-Domain Experience for Agentic Problem Solving](https://arxiv.org/abs/2507.06229v5) 2026-09-29

確認範囲: paper HTML の introduction、method、evaluation、limitations / conclusion を確認。測定は再実行していない。

<a id="D-LM-AGENTKB-REPO"></a>

\[27] [D-LM-AGENTKB-REPO — OPPO-PersonalAI/Agent-KB](https://github.com/OPPO-PersonalAI/Agent-KB) 2026-09-29

確認範囲: 論文から参照される repository overview を確認。固定 commit の code review はしていない。

<a id="P-LM-MACE"></a>

\[28] [P-LM-MACE — MACE: Memory-Agent Co-Evolution with Adaptive Memory Graphs for Multi-Agent Systems](https://arxiv.org/abs/2609.21533v1) 2026-09-29

確認範囲: 論文 HTML の方式、benchmark、appendix の比較データ出所と robustness caveat を確認。未査読・未再現。

<a id="P-LM-MEMORYDATA"></a>

\[29] [P-LM-MEMORYDATA — Are We Ready For An Agent-Native Memory System?](https://arxiv.org/abs/2606.24775v1) 2026-09-29

確認範囲: 論文 v1 の framework decomposition と evaluation scope を部分確認。結果を再計算せず、対象 dataset / systems を網羅確認していない。

<a id="D-LR-RAGFLOW-README"></a>

\[30] [D-LR-RAGFLOW-README — infiniflow/ragflow README](https://github.com/infiniflow/ragflow/blob/d64b84c7095b1edb81987bbf82cdc95b36a28cbf/README.md) 2026-09-29

確認範囲: READMEの機能・self-host/cloud・文書形式説明を確認。宣伝上の精度主張は再現していない

固定版: `d64b84c7095b1edb81987bbf82cdc95b36a28cbf`。

<a id="D-LR-RAGFLOW-CONFIG"></a>

\[31] [D-LR-RAGFLOW-CONFIG — RAGFlow Dataset Configuration](https://github.com/infiniflow/ragflow/blob/main/docs/guides/dataset/configuration.md) 2026-09-29

確認範囲: parserとknowledge-base設定項目を確認

<a id="D-LR-RAGFLOW-SYNC"></a>

\[32] [D-LR-RAGFLOW-SYNC — RAGFlow Data Source Configuration](https://github.com/infiniflow/ragflow/blob/main/docs/guides/data_source/data_source_configuration.md) 2026-09-29

確認範囲: Confluence/Notion等のsource permissionsと削除同期説明を確認

<a id="C-LR-RAGFLOW-CRUD"></a>

\[33] [C-LR-RAGFLOW-CRUD — infiniflow/ragflow internal/service/document/document\_crud.go](https://github.com/infiniflow/ragflow/blob/d64b84c7095b1edb81987bbf82cdc95b36a28cbf/internal/service/document/document_crud.go) 2026-09-29

確認範囲: commit固定ファイルの静的読解のみ。実行・統合試験・全consumer監査はしていない

固定版: `d64b84c7095b1edb81987bbf82cdc95b36a28cbf`。

<a id="D-LR-DIFY-KNOWLEDGE"></a>

\[34] [D-LR-DIFY-KNOWLEDGE — Dify Knowledge Overview](https://docs.dify.ai/en/cloud/use-dify/knowledge/readme) 2026-09-29

確認範囲: knowledge baseの作成・管理・retrieval利用の概要を確認

<a id="D-LR-DIFY-MANAGE"></a>

\[35] [D-LR-DIFY-MANAGE — Dify Manage Knowledge Content](https://docs.dify.ai/en/cloud/use-dify/knowledge/manage-knowledge/maintain-knowledge-documents) 2026-09-29

確認範囲: document/chunkの更新・削除・disable/archive項目を確認

<a id="D-LR-DIFY-SETTINGS"></a>

\[36] [D-LR-DIFY-SETTINGS — Dify Manage Knowledge Settings](https://docs.dify.ai/en/cloud/use-dify/knowledge/manage-knowledge/introduction) 2026-09-29

確認範囲: workspace role、knowledge base permission、index・embedding・retrieval settingsを確認

<a id="D-LR-DIFY-README"></a>

\[37] [D-LR-DIFY-README — langgenius/dify README](https://github.com/langgenius/dify) 2026-09-29

確認範囲: Cloud/self-hosted/Enterprise deployment説明とlicense案内を確認

<a id="C-LR-DIFY-DATASET"></a>

\[38] [C-LR-DIFY-DATASET — langgenius/dify api/controllers/service\_api/dataset/dataset.py](https://github.com/langgenius/dify/blob/e6f1d77ed5869e659e426b5459439d44bdabdbe5/api/controllers/service_api/dataset/dataset.py) 2026-09-29

確認範囲: commit固定ファイルのpermission/indexing fieldsと更新経路を静的確認のみ

固定版: `e6f1d77ed5869e659e426b5459439d44bdabdbe5`。

<a id="C-LR-DIFY-KBFS"></a>

\[39] [C-LR-DIFY-KBFS — langgenius/dify api/services/knowledge\_fs\_operations.py](https://github.com/langgenius/dify/blob/e6f1d77ed5869e659e426b5459439d44bdabdbe5/api/services/knowledge_fs_operations.py) 2026-09-29

確認範囲: commit固定のoperation declarationを静的確認のみ。全Console/API実行は未確認

固定版: `e6f1d77ed5869e659e426b5459439d44bdabdbe5`。

<a id="D-LR-ONYX-CONNECTORS"></a>

\[40] [D-LR-ONYX-CONNECTORS — Onyx Connectors Documentation](https://docs.onyx.app/overview/core_features/connectors) 2026-09-29

確認範囲: connectorの更新同期・permission同期のedition境界・default local processing記載を確認

<a id="D-LR-ONYX-README"></a>

\[41] [D-LR-ONYX-README — onyx-dot-app/onyx README](https://github.com/onyx-dot-app/onyx) 2026-09-29

確認範囲: Community/Enterprise, self-host/Cloud, Standard/Lite差を確認

<a id="D-LR-LLAMA-DOCS"></a>

\[42] [D-LR-LLAMA-DOCS — LlamaIndex Document Management](https://developers.llamaindex.ai/python/framework/module_guides/indexing/document_management/) 2026-09-29

確認範囲: document/ref-doc ID、insert/update/delete/refresh、index-specific caveatを確認

<a id="D-LR-LLAMA-INGESTION"></a>

\[43] [D-LR-LLAMA-INGESTION — LlamaIndex Ingestion Pipeline](https://developers.llamaindex.ai/python/framework/module_guides/loading/ingestion_pipeline/) 2026-09-29

確認範囲: transformations/cache/docstoreのingestion説明を確認

<a id="D-LR-LLAMACLOUD"></a>

\[44] [D-LR-LLAMACLOUD — LlamaIndex Framework Overview and LlamaCloud](https://github.com/run-llama/llama_index/blob/main/docs/src/content/docs/framework/index.md) 2026-09-29

確認範囲: OSS frameworkとmanaged/self-hosted LlamaCloud serviceの説明を確認

<a id="C-LR-LLAMA-PIPELINE"></a>

\[45] [C-LR-LLAMA-PIPELINE — run-llama/llama\_index ingestion/pipeline.py](https://github.com/run-llama/llama_index/blob/9ca9664a7c3ddb8216edc5df0941be40aa63af2e/llama-index-core/llama_index/core/ingestion/pipeline.py) 2026-09-29

確認範囲: commit固定ファイルのDocstoreStrategyとupsert/deleteの静的確認のみ。実行していない

固定版: `9ca9664a7c3ddb8216edc5df0941be40aa63af2e`。

<a id="D-LR-HAYSTACK-DOCSTORE"></a>

\[46] [D-LR-HAYSTACK-DOCSTORE — Haystack Document Store](https://docs.haystack.deepset.ai/docs/document-store) 2026-09-29

確認範囲: DocumentStore protocol、ID・overwrite・deleteの説明を確認

<a id="D-LR-HAYSTACK-FILTER"></a>

\[47] [D-LR-HAYSTACK-FILTER — Haystack Metadata Filtering](https://docs.haystack.deepset.ai/docs/metadata-filtering) 2026-09-29

確認範囲: metadata filteringとbackendごとのoperator差を確認

<a id="D-LR-HAYSTACK-PIPELINES"></a>

\[48] [D-LR-HAYSTACK-PIPELINES — Haystack Pipelines](https://docs.haystack.deepset.ai/docs/pipelines) 2026-09-29

確認範囲: pipeline orchestrationの説明を確認

<a id="D-LR-HAYSTACK-REPO"></a>

\[49] [D-LR-HAYSTACK-REPO — deepset-ai/haystack repository](https://github.com/deepset-ai/haystack) 2026-09-29

確認範囲: framework位置づけとApache-2.0 license表記を確認

<a id="D-LR-TXTAI-OVERVIEW"></a>

\[50] [D-LR-TXTAI-OVERVIEW — txtai Overview and Features](https://neuml.github.io/txtai/index.html) 2026-09-29

確認範囲: embeddings database、sparse/dense/graph/workflows/API、license記載を確認

<a id="D-LR-TXTAI-INDEX"></a>

\[51] [D-LR-TXTAI-INDEX — txtai Index Guide](https://neuml.github.io/txtai/embeddings/indexing/) 2026-09-29

確認範囲: index/upsert/delete/reindexのAPI説明を確認

<a id="D-LR-PATHWAY-APP"></a>

\[52] [D-LR-PATHWAY-APP — pathwaycom/llm-app README](https://github.com/pathwaycom/llm-app) 2026-09-29

確認範囲: source connectors、update/delete sync、search variants、templates記載を確認。scale claimsは未検証

<a id="D-LR-PATHWAY-LICENSE"></a>

\[53] [D-LR-PATHWAY-LICENSE — Pathway Licensing Terms and Conditions](https://pathway.com/license) 2026-09-29

確認範囲: Community/Scale/Enterprise license and grants/exclusions/resource termsを確認

<a id="D-LR-R2R-README"></a>

\[54] [D-LR-R2R-README — SciPhi-AI/R2R README](https://github.com/SciPhi-AI/R2R) 2026-09-29

確認範囲: RAG features, citations, API and local/Docker deploymentを確認。製品性能claimは未検証

<a id="D-LR-R2R-GUIDE"></a>

\[55] [D-LR-R2R-GUIDE — What is R2R?](https://github.com/SciPhi-AI/R2R/blob/main/docs/introduction/guides/what-is-r2r.md) 2026-09-29

確認範囲: document/search/analytics scope、claimed user management/access controlsを確認

<a id="D-LR-NEO4J-README"></a>

\[56] [D-LR-NEO4J-README — Neo4j GraphRAG for Python README](https://github.com/neo4j/neo4j-graphrag-python) 2026-09-29

確認範囲: first-party package scope, provider/store optional dependencies, license listingを確認

<a id="D-LR-NEO4J-RAG"></a>

\[57] [D-LR-NEO4J-RAG — Neo4j GraphRAG Python User Guide: RAG](https://neo4j.com/docs/neo4j-graphrag-python/current/user_guide_rag.html) 2026-09-29

確認範囲: retriever types, metadata filters and Neo4j server version caveatを確認

<a id="D-LR-NEO4J-KGBUILDER"></a>

\[58] [D-LR-NEO4J-KGBUILDER — Neo4j GraphRAG Python User Guide: Knowledge Graph Builder](https://neo4j.com/docs/neo4j-graphrag-python/current/user_guide_kg_builder.html) 2026-09-29

確認範囲: experimental label, loader/splitter/lexical graph/schema/extractor/resolver lifecycleを確認

<a id="D-LR-FALKOR-README"></a>

\[59] [D-LR-FALKOR-README — FalkorDB GraphRAG-SDK README](https://github.com/FalkorDB/GraphRAG-SDK) 2026-09-29

確認範囲: graph build/retrieval/provenance claim and Apache-2.0 licenseを確認。benchmark claimは未再現

<a id="D-LR-FALKOR-START"></a>

\[60] [D-LR-FALKOR-START — FalkorDB GraphRAG-SDK Getting Started](https://github.com/FalkorDB/GraphRAG-SDK/blob/main/docs/getting-started.mdx) 2026-09-29

確認範囲: SDK setup、graph ingestion、supported LLM/API dependencyを確認

<a id="D-LR-FASTGRAPH"></a>

\[61] [D-LR-FASTGRAPH — circlemind-ai/fast-graphrag README](https://github.com/circlemind-ai/fast-graphrag) 2026-09-29

確認範囲: PPR retrieval, incremental-update claim, MIT and managed-service statementを確認。claimsは未再現

<a id="D-LR-NANO"></a>

\[62] [D-LR-NANO — gusye1234/nano-graphrag README](https://github.com/gusye1234/nano-graphrag) 2026-09-29

確認範囲: project role, compactness and provider/database configurability statementを確認

<a id="D-LR-RAGANYTHING"></a>

\[63] [D-LR-RAGANYTHING — HKUDS/RAG-Anything README](https://github.com/HKUDS/RAG-Anything) 2026-09-29

確認範囲: LightRAG backend, modalities, source-page fields and backend migration/reprocess caveatを確認

<a id="P-LR-RAGANYTHING"></a>

\[64] [P-LR-RAGANYTHING — RAG-Anything: All-in-One RAG Framework](https://arxiv.org/abs/2510.12323) 2026-09-29

確認範囲: abstractとsystem framingを確認。reported benchmarkは再現していない

<a id="D-LR-BYALDI"></a>

\[65] [D-LR-BYALDI — AnswerDotAI/byaldi README](https://github.com/AnswerDotAI/byaldi/blob/main/README.md) 2026-09-29

確認範囲: pre-release, ColPali/ColQwen2, PDF image conversion, doc/page result fields, add-to-index and hardware notesを確認

<a id="P-LR-COLPALI"></a>

\[66] [P-LR-COLPALI — ColPali: Efficient Document Retrieval with Vision Language Models](https://arxiv.org/abs/2407.01449) 2026-09-29

確認範囲: paper abstract and page-image retrieval framingを確認。ViDoRe scoresは転載・再現していない

<a id="D-LR-MEMORAG"></a>

\[67] [D-LR-MEMORAG — qhjqhj00/MemoRAG README](https://github.com/qhjqhj00/MemoRAG) 2026-09-29

確認範囲: memory/clue-based retrieval design, prototype roadmap, model dependency and Apache-2.0 license statementを確認

<a id="P-LR-MEMORAG"></a>

\[68] [P-LR-MEMORAG — MemoRAG: Moving Towards Next-Gen RAG Via Memory-Inspired Knowledge Discovery](https://arxiv.org/abs/2409.05591) 2026-09-29

確認範囲: abstractのdual-system/global-memory/clue-guided retrieval framingを確認。reported evaluationsは再現していない

<a id="D-LS-W3C-OWL"></a>

\[69] [D-LS-W3C-OWL — OWL 2 Web Ontology Language Document Overview](https://www.w3.org/TR/owl2-overview/) 2026-09-29

確認範囲: W3C仕様の概念比較に使用。実装適合性は検査していない

<a id="D-LS-GDB-REASON"></a>

\[70] [D-LS-GDB-REASON — GraphDB 11.0 Reasoning](https://graphdb.ontotext.com/documentation/11.0/reasoning.html) 2026-09-29

確認範囲: 前向きmaterialization、ruleset、再推論・撤回の公開仕様を確認

<a id="D-LS-GDB-TALK"></a>

\[71] [D-LS-GDB-TALK — GraphDB 11.0 Talk to Your Graph](https://graphdb.ontotext.com/documentation/11.0/talk-to-graph.html) 2026-09-29

確認範囲: experimentalとする機能の公開説明を確認

<a id="D-LS-GDB-CONNECTOR"></a>

\[72] [D-LS-GDB-CONNECTOR — GraphDB 11.0 ChatGPT Retrieval Connector](https://graphdb.ontotext.com/documentation/11.0/retrieval-graphdb-connector.html) 2026-09-29

確認範囲: RDFから外部retrieval plugin / vector DBへの同期仕様とexperimental注記を確認

<a id="D-LS-GDB-SHACL"></a>

\[73] [D-LS-GDB-SHACL — GraphDB Documentation 11.0.2C, SHACL validation](https://graphdb.ontotext.com/documentation/11.0/pdf/GraphDB.pdf) 2026-09-29

確認範囲: 製品PDFのSHACL validation概要を確認

<a id="D-LS-RDFOX-FEATURES"></a>

\[74] [D-LS-RDFOX-FEATURES — RDFox Features and Requirements](https://docs.oxfordsemantic.tech/features-and-requirements.html) 2026-09-29

確認範囲: RDF / rules / OWL / SHACL、外部source、materialization、proof、transaction / ACLを確認

<a id="D-LS-RDFOX-LICENSE"></a>

\[75] [D-LS-RDFOX-LICENSE — RDFox Evaluation License](https://www.oxfordsemantic.tech/rdfox-evaluation-license) 2026-09-29

確認範囲: 評価ライセンスの存在を確認。商用条件の評価なし

<a id="D-LS-STARDOG-VOICEBOX"></a>

\[76] [D-LS-STARDOG-VOICEBOX — Stardog Voicebox: Guided Ontology Creation and Mapping](https://docs.stardog.com/voicebox/guided-ontology-creation-and-mapping/) 2026-09-29

確認範囲: ontology / mapping案とhuman-in-loop reviewの公開フローを確認

<a id="D-LS-TRIPLY-ETL"></a>

\[77] [D-LS-TRIPLY-ETL — TriplyETL Validate](https://docs.triply.cc/triply-etl/validate/) 2026-09-29

確認範囲: gold graph comparisonとSHACL validationのETL段を確認

<a id="D-LS-TRIPLY-SAVED"></a>

\[78] [D-LS-TRIPLY-SAVED — TriplyDB Saved Queries](https://docs.triply.cc/triply-db-getting-started/saved-queries/) 2026-09-29

確認範囲: 保存クエリに独立した版番号を持たせる説明を確認

<a id="D-LS-TRIPLY-EDIT"></a>

\[79] [D-LS-TRIPLY-EDIT — TriplyDB Editing (SKOS) Data](https://docs.triply.cc/triply-db-getting-started/editing-data/) 2026-09-29

確認範囲: SHACL shapeに基づくフォームと資源単位の変更履歴を確認

<a id="D-LS-ECCENCA-ARCH"></a>

\[80] [D-LS-ECCENCA-ARCH — eccenca Corporate Memory System Architecture](https://documentation.eccenca.dev/latest/deploy-and-configure/system-architecture/) 2026-09-29

確認範囲: Build / Explore / cmemc、source-to-RDF mapping、external triple storeの記述を確認

<a id="D-LS-ECCENCA-BUILD"></a>

\[81] [D-LS-ECCENCA-BUILD — eccenca Build Introduction to the User Interface](https://documentation.eccenca.dev/latest/build/introduction-to-the-user-interface/) 2026-09-29

確認範囲: mapping、transform、link、workflowの製品説明を確認

<a id="D-LS-ECCENCA-MARKETPLACE"></a>

\[82] [D-LS-ECCENCA-MARKETPLACE — eccenca Corporate Memory Marketplace](https://documentation.eccenca.dev/latest/distribution/marketplace/) 2026-09-29

確認範囲: versioned packageの構成と更新・置換の制約を確認

<a id="D-LS-TOPBRAID-INTRO"></a>

\[83] [D-LS-TOPBRAID-INTRO — TopBraid EDG Introduction](https://docs.topquadrant.com/7.4/introduction/index.html) 2026-09-29

確認範囲: asset collectionとsemantic governanceの概要。旧版ページなので現行SKUと断定しない

<a id="D-LS-TOPBRAID-WORKFLOW"></a>

\[84] [D-LS-TOPBRAID-WORKFLOW — TopBraid EDG Workflows](https://docs.topquadrant.com/8.0/user_guide/workflows/index.html) 2026-09-29

確認範囲: working copy、review / approval、production copyへのcommitを確認

<a id="D-LS-ONTOP-GUIDE"></a>

\[85] [D-LS-ONTOP-GUIDE — Ontop Guide](https://ontop-vkg.org/guide/) 2026-09-29

確認範囲: Virtual KG、R2RML / Ontop mapping、SPARQL / SQL rewriting、RDFS / OWL 2 QL、stable版とlicenseを確認

<a id="D-LS-ONTOP-REPO"></a>

\[86] [D-LS-ONTOP-REPO — ontop/ontop](https://github.com/ontop/ontop) 2026-09-29

確認範囲: 公開repo概要とApache-2.0表示を確認。コード監査対象外

<a id="D-LS-TYPEDB-FUNCTIONS"></a>

\[87] [D-LS-TYPEDB-FUNCTIONS — TypeDB Functions vs Rules](https://typedb.com/docs/typeql-reference/functions/functions-vs-rules/) 2026-09-29

確認範囲: TypeDB 3のgoal-driven computationと旧rule materializationとの差を確認

<a id="D-LS-TYPEDB-REPO"></a>

\[88] [D-LS-TYPEDB-REPO — typedb/typedb](https://github.com/typedb/typedb) 2026-09-29

確認範囲: 公開repositoryのCommunity Edition MPL-2.0表記を確認。コード監査対象外

<a id="D-LS-TERMINUS-VERSION"></a>

\[89] [D-LS-TERMINUS-VERSION — TerminusDB Version Control Operations](https://terminusdb.org/docs/version-control-operations/) 2026-09-29

確認範囲: commit / branch / merge / diff / historical readの公開仕様を確認

<a id="D-LS-TERMINUS-REPO"></a>

\[90] [D-LS-TERMINUS-REPO — terminusdb/terminusdb](https://github.com/terminusdb/terminusdb) 2026-09-29

確認範囲: 公開Community repository概要とApache-2.0表示を確認

<a id="C-LS-TERMINUS-LAYER"></a>

\[91] [C-LS-TERMINUS-LAYER — terminusdb/terminusdb src/library/terminus\_store.pl at fixed commit](https://github.com/terminusdb/terminusdb/blob/57f2093baeafd65e16004e84b7b58e0c5cf72858/src/library/terminus_store.pl#L171-L307) 2026-09-29

確認範囲: 固定commitのソースを静的確認。layer head、snapshot read、child layer builder、add/remove delta、commitのdocstring。実行なし

固定版: `57f2093baeafd65e16004e84b7b58e0c5cf72858`。

<a id="D-LS-XTDB-TIME"></a>

\[92] [D-LS-XTDB-TIME — Time in XTDB](https://docs.xtdb.com/about/time-in-xtdb.html) 2026-09-29

確認範囲: transaction/system timeとvalid timeの意味、過去訂正・as-of queryを確認

<a id="D-LS-XTDB-LICENSE"></a>

\[93] [D-LS-XTDB-LICENSE — XTDB Documentation](https://docs.xtdb.com/) 2026-09-29

確認範囲: 製品概要のオープンソース/MPL-2.0表記を確認

<a id="D-LS-PALANTIR-INTRO"></a>

\[94] [D-LS-PALANTIR-INTRO — Palantir Foundry Introductory Concepts](https://www.palantir.com/docs/foundry/getting-started/introductory-concepts) 2026-09-29

確認範囲: data layer / object layer、dataset lineage、object / link / actionを確認

<a id="D-LS-PALANTIR-ONTOLOGY"></a>

\[95] [D-LS-PALANTIR-ONTOLOGY — Palantir Foundry Ontology Overview](https://www.palantir.com/docs/foundry/ontology/overview) 2026-09-29

確認範囲: Object/link/action ontologyと編集・version機能への案内を確認

<a id="D-LS-PALANTIR-PERMISSIONS"></a>

\[96] [D-LS-PALANTIR-PERMISSIONS — Palantir Foundry Object Permissioning Overview](https://www.palantir.com/docs/foundry/object-permissioning/overview) 2026-09-29

確認範囲: object/link/action permissioningの公開説明を確認

<a id="D-LS-RECALLGRAPH-README"></a>

\[97] [D-LS-RECALLGRAPH-README — RecallGraph official repository README](https://github.com/RecallGraph/RecallGraph) 2026-09-29

確認範囲: archived 2026-04-25、ArangoDB Foxx/event history、point-in-time graph query、valid-timeとbranchingはroadmap、successor Minigrafへの案内を確認

<a id="D-LS-MINIGRAF-README"></a>

\[98] [D-LS-MINIGRAF-README — Minigraf official repository README](https://github.com/project-minigraf/minigraf) 2026-09-29

確認範囲: successor側の公開説明を確認。RecallGraphからのportではない。runtimeや性能は未検証

<a id="D-LS-RECALLGRAPH-PYPI"></a>

\[99] [D-LS-RECALLGRAPH-PYPI — recallgraph on PyPI](https://pypi.org/project/recallgraph/) 2026-09-29

確認範囲: 同名の0.0.2配布情報とリンク先を確認。公式RecallGraph/RecallGraphとは別候補

<a id="D-LS-RECALLGRAPH-SAME-NAME"></a>

\[100] [D-LS-RECALLGRAPH-SAME-NAME — Indhar01/RecallGraph project link redirects to MemoGraph](https://github.com/Indhar01/RecallGraph) 2026-09-29

確認範囲: PyPI候補の現在のGitHub destinationはMemoGraphへredirect。公式RecallGraphとの同一性なしと確認

<a id="D-LC-AWS-MEMORY"></a>

\[101] [D-LC-AWS-MEMORY — AgentCore: Long-term memory](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/long-term-memory-long-term.html) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: 長期記憶の resource、strategy、namespace、管理 API の説明。

<a id="D-LC-AWS-STRATEGY"></a>

\[102] [D-LC-AWS-STRATEGY — AgentCore: Configure built-in strategies](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/long-term-configuring-built-in-strategies.html) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: user preference / semantic / summary / episodic の各節と設定例。

<a id="D-LC-AWS-DELETE"></a>

\[103] [D-LC-AWS-DELETE — AgentCore: Delete memory records](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/long-term-delete-memory-records.html) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: DeleteMemoryRecord による個別記憶の削除。派生物の連鎖消去は未検証。

<a id="D-LC-GOOGLE-MEMORY"></a>

\[104] [D-LC-GOOGLE-MEMORY — Agent Platform Memory Bank overview](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: 現行サービス名、scope、抽出・統合、継続取り込み、TTL、IAM、ADK 統合。旧 Vertex AI URL は現行案内へ遷移した。

<a id="D-LC-GOOGLE-REVISIONS"></a>

\[105] [D-LC-GOOGLE-REVISIONS — Memory Bank: Memory revisions](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank/revisions) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: 現行状態と immutable revision、抽出中間結果、rollback、48時間の削除復元窓、改訂履歴の無効化と TTL。

<a id="D-LC-FOUNDRY-MEMORY"></a>

\[106] [D-LC-FOUNDRY-MEMORY — Microsoft Foundry: Use memory with agents](https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/memory-usage) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: preview、memory options、個別 CRUD、TTL、tool と低水準 API の scope 解決差。

<a id="D-LC-OAI-RUN"></a>

\[107] [D-LC-OAI-RUN — OpenAI Agents SDK: Running agents](https://developers.openai.com/api/docs/guides/agents/running-agents) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: history / session / conversationId / previousResponseId による会話状態管理の区別。

<a id="D-LC-OAI-STATE"></a>

\[108] [D-LC-OAI-STATE — OpenAI: Conversation state](https://developers.openai.com/api/docs/guides/conversation-state) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: Conversations の durable ID と messages、tool calls、tool outputs の保存。

<a id="D-LC-OAI-SANDBOX"></a>

\[109] [D-LC-OAI-SANDBOX — OpenAI Agents SDK: Sandboxes — Sandbox Memory](https://developers.openai.com/api/docs/guides/agents/sandboxes) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: Sandbox Memory の設定、読み取りの階層、session close 後の抽出・統合、永続性と Session との区別。全 sandbox 機能の監査ではない。

<a id="D-LC-CLAUDE-TOOL"></a>

\[110] [D-LC-CLAUDE-TOOL — Claude: Memory tool](https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: クライアント側 tool、アプリ管理の永続ファイル、コマンド実行責任。

<a id="D-LC-CLAUDE-STORE"></a>

\[111] [D-LC-CLAUDE-STORE — Claude Managed Agents: Memory](https://platform.claude.com/docs/en/managed-agents/memory) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: agent-memory-2026-07-22 beta、workspace store、read-only/read-write、版・復元・redaction・delete、managed と自己管理 sandbox 同期の違い。

<a id="D-LC-LANGGRAPH"></a>

\[112] [D-LC-LANGGRAPH — LangGraph: Add and manage memory](https://docs.langchain.com/oss/python/langgraph/add-memory) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: checkpointer、thread、cross-thread Store、in-memory と永続 backend の選択。

<a id="D-LC-MAF-CONTEXT"></a>

\[113] [D-LC-MAF-CONTEXT — Microsoft Agent Framework: Context providers](https://learn.microsoft.com/en-us/agent-framework/concepts/agents/conversations/context-providers) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: before / after hooks、FileMemoryProvider と session / user scope、実行状態との区別。

<a id="D-LC-MAF-HISTORY"></a>

\[114] [D-LC-MAF-HISTORY — Microsoft Agent Framework: Chat history memory provider](https://learn.microsoft.com/en-us/agent-framework/concepts/agents/conversations/chat-history-memory-provider) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: vector store の履歴、storageScope と searchScope、ユーザー等の filter、権限制御の確認事項。

<a id="D-LC-AGNO"></a>

\[115] [D-LC-AGNO — Agno: What is Memory?](https://docs.agno.com/memory/overview) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: user\_id と session\_id、自動抽出と agentic 更新、削除許可、両方式有効時の優先順。

<a id="D-LC-ORACLE"></a>

\[116] [D-LC-ORACLE — Oracle Agent Memory: About](https://docs.oracle.com/en/database/oracle/agent-memory/26.6/guide/about.html) 2026-09-29

確認範囲: 関連節を確認。サービス API・SDK は実行していない。

用途: 短期 context card と長期 facts、Oracle AI Database、provider と MCP、アプリ側の認証・認可責任。

<a id="D-LC-CREWAI"></a>

\[117] [D-LC-CREWAI — CrewAI v1.15.23: Memory](https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/docs/v1.15.23/en/concepts/memory.mdx) 2026-09-29

確認範囲: 統合 Memory、scope/slice、consolidation、検索順位、source/private、保存 backend と読み取り専用 slice の例を確認。

固定版: `deaa71e168069a1d5307340172875def4330e75b`。

<a id="C-LC-CREWAI-MEMORY"></a>

\[118] [C-LC-CREWAI-MEMORY — CrewAI: unified\_memory.py](https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/lib/crewai/src/crewai/memory/unified_memory.py) 2026-09-29

確認範囲: recall、forget、update、scope、slice と privacy filter の関連経路を静的に確認。全体精読・実行はしていない。

固定版: `deaa71e168069a1d5307340172875def4330e75b`。

<a id="C-LC-CREWAI-SCOPE"></a>

\[119] [C-LC-CREWAI-SCOPE — CrewAI: memory\_scope.py](https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/lib/crewai/src/crewai/memory/memory_scope.py) 2026-09-29

確認範囲: MemoryScope の委譲と MemorySlice.remember / recall の静的確認。read\_only は remember を no-op にする。

固定版: `deaa71e168069a1d5307340172875def4330e75b`。

<a id="C-LC-CREWAI-FACTORY"></a>

\[120] [C-LC-CREWAI-FACTORY — CrewAI: storage factory](https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/lib/crewai/src/crewai/memory/storage/factory.py) 2026-09-29

確認範囲: storage factory の登録・解決を確認。built-in LanceDB / Qdrant の説明と差し替え境界。実行していない。

固定版: `deaa71e168069a1d5307340172875def4330e75b`。

<a id="C-LC-CREWAI-LICENSE"></a>

\[121] [C-LC-CREWAI-LICENSE — CrewAI: LICENSE](https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/LICENSE) 2026-09-29

確認範囲: MIT 表記の本文を確認。依存・外部 API・商用サービスの条件は含めない。

固定版: `deaa71e168069a1d5307340172875def4330e75b`。

<a id="P-LX-DATABASE"></a>

\[122] [P-LX-DATABASE — Is Agent Memory a Database? Rethinking Data Foundations for Long-Term AI Agent Memory](https://arxiv.org/abs/2605.26252v1) 2026-09-29

確認範囲: arXiv の要旨・書誌のみ確認。本文・コード・実験は未確認。

用途: GEM と MemState の問題設定。製品比較の採用済み系統には数えない。

<a id="P-LX-WORKLOADS"></a>

\[123] [P-LX-WORKLOADS — Agent Memory: Characterization and System Implications of Stateful Long-Horizon Workloads](https://arxiv.org/abs/2606.06448v2) 2026-09-29

確認範囲: arXiv の要旨・書誌と 2026-09-22 の v2 改訂日を確認。本文・実験値・コードは未確認。

用途: 構築・検索・生成の段階別費用を比較する評価視点。性能値は転載していない。

既存 ID で再確認した資料: `D-STARDOG` ([公式資料](https://docs.stardog.com/inference-engine/)), `D-TYPEDB` ([公式資料](https://typedb.com/docs/core-concepts/typeql/schema-data/)), `D-VIRTUAL` ([公式資料](https://docs.stardog.com/virtual-graphs/)), `D-XTDB` ([公式資料](https://docs.xtdb.com/concepts/key-concepts.html)), `P-ACE` ([公式資料](https://arxiv.org/abs/2510.04618v3)), `P-MSKILLS` ([公式資料](https://arxiv.org/abs/2603.18743v1)), `S-PROV` ([公式資料](https://www.w3.org/TR/prov-o/)), `S-SHACL` ([公式資料](https://www.w3.org/TR/shacl/))。追加の確認範囲は sources.json の additional\_reviews に記録した。
