# 事実抽出と個人化を中心にしたメモリー

[調査トップ](../README.md) / [システム比較](README.md) / [資料台帳](../sources.md)

## Mem0: 論文の更新方式と現行 OSS を分けて読む

**論文。** 2025 年の方式は、会話と要約から事実候補を抽出し、各候補に対して近い既存記憶を検索し、LLM が ADD / UPDATE / DELETE / NOOP を選ぶ。実験は過去メッセージ数・候補数を各 10、主な処理を GPT-4o-mini としている。関係グラフを追加した Mem0ᵍ も扱う。[P-M0]

**確認した現行コード。** `mem0ai` 2.2.1、コミット `94c3fe9f…` の通常 `Memory.add()` → `_add_to_vector_store()` は次の流れだった。[C-M0]

1. 同一 scope の直近 10 メッセージと、既存記憶上位 10 件を取得。
2. `ADDITIVE_EXTRACTION_PROMPT` を使い一回の LLM 呼び出しで新しい記憶テキストを抽出。
3. 一括 embedding、テキスト hash の重複除去、ベクトル保存。
4. 成功した保存だけを履歴へ記録。
5. 抽出した実体を既存実体と照合し、`linked_memory_ids` を追加。
6. 元メッセージを履歴 DB へ保存して ADD 結果を返す。

この経路では、論文のように各候補の ADD/UPDATE/DELETE/NOOP を選択して既存 fact を変更する処理は確認できない。明示的な `update` / `delete` API は別に存在する。`add` の docstring には旧来の説明が残るため、コメントだけで実装を判断しない。実体側のリンク更新を、意味記憶そのものの訂正と取り違えない。[C-M0]

**Graph Memory。** 固定した docs は OSS の外部 graph store を現在の機能として提供していないとし、Platform の現行 graph を「実体と記憶の共起リンク」と説明する。型付きの `manages` などの関係を付ける方式ではなく、検索の combined score に影響する。旧 `relations` 出力の前提で設計しない。[D-M0OSS] [D-M0GRAPH] [C-M0CFG]

**強み（コードからの分析）。** adapter が多く、素朴な profile / fact memory の比較基準にしやすい。明示 CRUD と変更履歴を備え、入力 scope を持つ。抽出・保存を一括化する実装は、LLM 呼び出し回数を抑える方向にある。

**制約（コードからの分析）。** 上位 10 件から漏れた重複は候補照合できない。本文 hash の一致は言い換えの重複を保証しない。vector と SQLite history は別保存なので、両方の原子的成功をこの関数だけで保証していない。scope ID を指定することは認証・権限チェックの代替ではない。期限日と双時間の履歴も異なる機能である。

**比較時の固定事項。** Python/TypeScript、OSS/Platform、コミット、infer の有無、graph の世代、検索候補数を記録する。2025 年の論文スコアを 2.2.1 の期待性能として転記しない。

## LangMem / LangGraph

LangMem は profile、文書集合、経験、プロンプトの改善を扱う部品群。確認した `MemoryManager` は Trustcall の `create_extractor` に既存文書と schema を渡し、insert/update/remove の候補を返す。削除可否は `enable_deletes` で制御し、既定値は false。結果を返す処理と Store へ永続化する層を分けている。[C-LANG] [D-LANGMEM]

**強み。** 自分の `Claim` 型を使い、保存前の承認・検証を追加しやすい。LangGraph checkpoint は実行状態、Store は thread をまたぐ知識という役割分担にできる。抽出用ライブラリと DB を強く結び付けたくない場合の比較対象になる。

**制約。** namespace の設計、権限、競合、過去時点の保存、根拠撤回の伝播はアプリで定める必要がある。Trustcall の構造化 patch が通ることと、変更内容が意味的に正しいことは別。prompt optimizer は行動方針を変えるため、事実の保存とは異なる評価が必要になる。

## Memobase

ユーザーを中心に、会話の buffer から profile とイベントを形成する。profile は topic / subtopic に整理され、merge の制御点がある。確認した server manifest には SQLAlchemy、pgvector、Redis、OpenAI、FastAPI がある。[D-MBASE] [C-MBASE]

**強み。** 「このユーザーの好みや基本情報を毎回渡す」という問いに合い、任意の文書群から都度 top-k を選ぶより必要なフィールドを定めやすい。profile schema を用途に合わせて制約できる。

**制約。** 人物 profile を中心とした表現を、業務文書の出典付き主張や複雑な出来事へそのまま広げるのは難しい。profile merge の結果に旧情報を含めるか、根拠が競合した時にどちらを採るかを確認する必要がある。取得した最新版のコミットは 2026-01-11 だが、日付だけからプロジェクト全体の停止を断定しない。

## SimpleMem

論文は、(1) 文脈を補って自立した意味単位へ圧縮、(2) セッション内で関連情報を統合、(3) 質問に応じた検索計画、の三段階。代名詞を解決し、相対日付を具体化して、後から単体で読める記憶にする発想が有用である。「semantic lossless」は著者の方式名であり、情報を一切失わない数学的保証としては扱わない。[P-SIMPLE]

現行 repo は text、OmniSimpleMem、EvolveMem 等を含む。今回の論文比較は text 方式を対象にし、別実装のマルチモーダル機能や進化機能を初稿の実験結果に混ぜない。text 側には会話窓を処理する MemoryBuilder、検索計画と追加検索を行う HybridRetriever、LanceDB を扱う VectorStore がある。requirements 全体には評価や比較方式の依存も入り、LangMem が載っていることだけで中核処理が LangMem 依存と断定できない。[D-SIMPLE] [C-SIMPLEBUILD] [C-SIMPLER] [C-SIMPLEDB]

**強み。** 圧縮時に時間・指示対象を補うことは、小さな検索単位でも意味を保つ助けになる。**制約。** 圧縮で失った否定・例外・出典を、後の検索改善だけで回復することはできない。保存時の抽出精度と問い合わせ費用を分計して比べる。

## A-MEM

ノートに本文、時刻、keywords、tags、context、リンク、進化履歴などを持たせ、新しいノートとの関連から既存ノートを再編する。確認した実装には ChromaRetriever、Sentence Transformers、BM25、LLM controller がある。[P-AMEM] [C-AMEM]

**強み。** 固定された業務スキーマがなくても、発見的に関連を作りやすい。知識探索やアイデア整理に向く。**制約。** リンクは形式的な含意や同一性の証明ではない。自動的な「記憶の進化」で過去の文脈が変わるため、原文と生成された context を分け、誤ったリンクの取り消しが結果に反映されるかを調べるべきである。

## 関連ドキュメント

- [知識更新の理論と実装](../foundations/knowledge-updates.md)
- [実装の依存関係](../implementation/dependencies.md)
- [共通の評価方法](../evaluation.md)

[P-M0]: https://arxiv.org/abs/2504.19413v1
[C-M0]: https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/mem0/memory/main.py
[D-M0OSS]: https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/docs/open-source/configuration.mdx
[D-M0GRAPH]: https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/docs/platform/features/graph-memory.mdx
[C-M0CFG]: https://github.com/mem0ai/mem0/blob/94c3fe9f238f3dbf29c9ce98643bd71eb13077cd/mem0/configs/base.py
[C-LANG]: https://github.com/langchain-ai/langmem/blob/9d033b47d9ce53e37e92c92241b0496c0278932e/src/langmem/knowledge/extraction.py
[D-LANGMEM]: https://langchain-ai.github.io/langmem/concepts/conceptual_guide/
[D-MBASE]: https://github.com/memodb-io/memobase/blob/358c16bbc6d687937d79bc2f984a11c3be8da901/readme.md
[C-MBASE]: https://github.com/memodb-io/memobase/blob/358c16bbc6d687937d79bc2f984a11c3be8da901/src/server/api/memobase_server/controllers/modal/chat/merge.py
[P-SIMPLE]: https://arxiv.org/abs/2601.02553v3
[D-SIMPLE]: https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/README.md
[C-SIMPLEBUILD]: https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/simplemem/core/memory_builder.py
[C-SIMPLER]: https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/simplemem/core/hybrid_retriever.py
[C-SIMPLEDB]: https://github.com/aiming-lab/SimpleMem/blob/db80b6a7c591e0ea730a058e9f5fc4eb06572299/simplemem/core/database/vector_store.py
[P-AMEM]: https://arxiv.org/abs/2502.12110v11
[C-AMEM]: https://github.com/agiresearch/A-mem/blob/ceffb860f0712bbae97b184d440df62bc910ca8d/agentic_memory/memory_system.py
