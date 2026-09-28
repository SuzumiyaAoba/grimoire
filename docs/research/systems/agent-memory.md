# エージェント、ファイル、マネージドサービスの記憶

[調査トップ](../README.md) / [システム比較](README.md) / [資料台帳](../sources.md)

## Letta: MemGPT からファイル・Git を使う実装へ

MemGPT は文脈を階層的に管理し、モデルが tool call を通じて外側の記憶を利用する研究である。従来の Letta の core blocks と archival / recall memory はこの系譜にある。[P-MGPT]

再調査時の `letta-ai/letta` README は、旧 V1 API server を `archive` に置き、開発先を `letta-ai/letta-code` と明示している。確認した `letta-code` は 0.33.5。memory block の読み込みに加え、agent ごとの memory filesystem、`system` ディレクトリ、Git を使う履歴・同期処理を持つ。[C-LMOV] [C-LBLOCK] [C-LMEM] [C-LGIT]

**強み。** 人と agent が読める記憶、差分、履歴を組み込める。作業上のルール、プロジェクトの状態、継続タスクに合う。**制約。** Git の差分は文書変更の追跡であり、「誰のどの主張がいつから有効か」という意味的履歴を自動で提供しない。ファイルの競合を解消しても、相互に矛盾する主張の真偽は別に判定する必要がある。旧 API のチュートリアルを現行 runtime の仕様として採用しない。

## Basic Memory と MCP reference Memory

**Basic Memory** は Markdown を人と AI の共通の正本とし、同期・検索用の構造を作る。現行 manifest には Markdown parser、frontmatter、SQLAlchemy、SQLite、FastEmbed、sqlite-vec がある。任意のノートエディタと組み合わせやすく、記憶の直接編集と持ち出しが容易。半面、自由記述に含まれる否定・時間・根拠をどこまで構造化するかは別の設計になる。[D-BASIC]

**MCP servers の Memory** は entity、relation、observation を JSON Lines に保存し、明示 CRUD と検索を提供する。確認した実装の `searchNodes` は entity の名前・型・observations に対する文字列検索。ファイル全体を読んで保存する簡素な構成で、LLM 抽出器や高度な矛盾解決を内蔵するわけではない。小さな比較基準として有用だが、大規模な同時利用では永続化・権限・索引の条件を追加する必要がある。[C-MCP]

**共通の示唆。** テキストを人が修正できる設計は重要な対照方式である。大規模な graph pipeline の性能を評価する時にも、Markdown＋全文検索＋短い要約を基準線に置く。MCP はデータの提供手段であり、二つの Memory 実装が同じ意味論を共有することは保証しない。[D-MCP]

## Honcho

Honcho は peer と session を単位に、観測したメッセージから結論・要約・peer card を形成する。`(workspace, observer, observed)` によって、誰から見た誰の表現かを区別する。Dreaming は結論の統合、変化への対応、パターン抽出を背景で行う機能として説明されている。[D-HONCHO] [D-HDREAM]

**強み。** 同じ人物を複数の主体が異なる情報で理解する問題を直接モデル化している。**制約。** 公式資料が使う deduction / induction / abduction という分類から、すべての出力が証明器で検証された結論だとは言えない。推定した人物像は本人の明示的自己申告と分ける必要がある。Dreaming は資料上も実験的とされる。暗黙推定が誤った場合の撤回・利用停止・根拠提示を比較する。

## Supermemory

公式 README は文書と memory の hybrid search、static / dynamic user profile、更新と期限、コネクタ、ローカル実行を提供すると説明する。公開 repo の package manifest には TypeScript、AI SDK、Hono、Zod、Drizzle、PostgreSQL client 等がある。ただしこの manifest から、配布されたローカルバイナリやクラウド内部の graph engine の全実装まで確認できたとは言えない。[D-SUPER]

**強み（公式仕様に基づく）。** 一つの API で profile、文書、会話の利用を始められる。**制約。** provider の「#1」表記や Recall@15 は QA accuracy と区別する。SDK と plugin の公開範囲、サービスの範囲、自己ホストの条件を分けて評価する。原文・主張・派生 profile をまとめて export し、別実装へ移行できるかが今回の比較点。

## Mastra Observational Memory

Observer がメッセージを日付付きの観察へ変換し、Reflector が観察を整理・圧縮する。質問ごとに top-k を取り出す代わりに、観察ログを文脈に配置して継続的に使う。参照時刻を補い、文脈の prefix を安定させるという発想である。[D-MASTRA]

**強み。** 毎回の検索失敗を回避でき、文脈 caching の効果を得やすい。**制約。** 観察が膨らむと再圧縮が必要になり、古い細部の保持は Observer / Reflector に依存する。アプリの作業メモリーとしての良さと、事実を時点指定で監査できる知識ベースとしての良さは別。公開スコアの回答モデルと抽出モデルは[評価方法](../evaluation.md)に記載する。

## フレームワークが提供する境界

| フレームワーク | 確認した機構 | 自分で定める部分 |
|---|---|---|
| LangGraph | thread の checkpointer と cross-thread の Store を区別。InMemorySaver は再起動で失われる | 知識抽出、schema、更新ルール、保存実装。[D-LANGGRAPH] |
| LlamaIndex | StaticMemoryBlock、FactExtractionMemoryBlock、VectorMemoryBlock。抽出 facts が上限を超えると要約する | 要約による情報損失、訂正の歴史、証拠の保存。[D-LLAMA] |
| Google ADK | MemoryService へ session、event delta、明示 entry を投入できる。Memory Bank 等の実装を交換 | Service 間の保証差。InMemoryMemoryService は RAM と単純語彙検索で、永続ストアと同一ではない。[D-ADK] |

## クラウド API と会話製品

| 対象 | 参考になる設計 | 比較時に注意する境界 |
|---|---|---|
| Amazon Bedrock AgentCore Memory | 短期イベントから summary、facts、preference 等を作る strategy。customization の入口 | AWS のサービス境界と、自分の抽出器・保存層の境界を確認。[D-AWS] [D-AWSSTRAT] |
| Microsoft Foundry Memory | agent の長期記憶を Memory Store API として提供 | 調査時の公式ページは preview。API や利用条件を本番仕様として固定しない。[D-AZURE] |
| ChatGPT | 記憶の確認・訂正・削除、チャットと保存記憶の扱い | 消費者向け機能と開発者 API は別。内部方式は公開ヘルプから確定できない。[D-CHATGPT] |
| Claude | チャット検索と要約記憶、利用者による制御 | サービス利用時の UX を参照し、公開メモリーエンジンとして扱わない。[D-CLAUDE] |
| Gemini Apps | 過去チャットや接続元からの個人化、訂正・削除操作 | 元チャットと接続元からの再取得を含むライフサイクルを考える。[D-GEMINI] |

料金や順位の比較はしていない。今回必要なのは、記憶の単位、訂正可能性、監査、可搬性、評価条件を比較できることである。

## 関連ドキュメント

- [メモリーと知識の基本概念](../foundations/concepts.md)
- [実装の依存関係](../implementation/dependencies.md)
- [共通の評価方法](../evaluation.md)

[P-MGPT]: https://arxiv.org/abs/2310.08560v2
[C-LMOV]: https://github.com/letta-ai/letta/blob/5bcdd177d70fa2b31a754cfcd801e77b2e1ab16a/README.md
[C-LBLOCK]: https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/src/agent/memory.ts
[C-LMEM]: https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/src/agent/memory-filesystem.ts
[C-LGIT]: https://github.com/letta-ai/letta-code/blob/c864f1532b328aab4bb76cc68a86d5f014de27b8/src/agent/memory-git.ts
[D-BASIC]: https://github.com/basicmachines-co/basic-memory/blob/22d31e96e2defa465d3703620a537c9125e7d364/README.md
[C-MCP]: https://github.com/modelcontextprotocol/servers/blob/f46d9578190b476b3501923ea8977d899e8db2cb/src/memory/index.ts
[D-MCP]: https://modelcontextprotocol.io/specification/2026-07-28
[D-HONCHO]: https://github.com/plastic-labs/honcho/blob/2eb27b6cc0595d3f8deb693f0560e7241c2aeaff/docs/v3/documentation/core-concepts/representation.mdx
[D-HDREAM]: https://github.com/plastic-labs/honcho/blob/2eb27b6cc0595d3f8deb693f0560e7241c2aeaff/docs/v3/documentation/features/advanced/dreaming.mdx
[D-SUPER]: https://github.com/supermemoryai/supermemory/blob/cfa6c7cb17476d19ea896867406c80e8186a72ec/README.md
[D-MASTRA]: https://mastra.ai/research/observational-memory
[D-LANGGRAPH]: https://docs.langchain.com/oss/python/langgraph/persistence
[D-LLAMA]: https://developers.llamaindex.ai/python/framework/module_guides/deploying/agents/memory/
[D-ADK]: https://adk.dev/sessions/memory/
[D-AWS]: https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-types.html
[D-AWSSTRAT]: https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/memory-custom-strategy.html
[D-AZURE]: https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/what-is-memory
[D-CHATGPT]: https://help.openai.com/en/articles/8590148-memory-in-chatgpt
[D-CLAUDE]: https://support.claude.com/en/articles/11817273-use-claude-s-chat-search-and-memory-to-build-on-previous-context
[D-GEMINI]: https://support.google.com/gemini/answer/16598469?hl=en
