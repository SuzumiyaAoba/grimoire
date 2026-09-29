# クラウド・実行基盤のメモリー比較

[調査トップ](../README.md) / [関連システム再調査](extended-landscape.md) / [資料台帳](../sources.md)

確認日: 2026-09-29。開発者向けの 10 系統を調べた。消費者向けチャットの記憶機能と API の保証は別に扱う。公式文書の機能説明と、CrewAI の選択ファイルの静的確認に基づく。サービスの内部実装、課金、性能、削除完了は検証していない。

## 比較の前提

会話を再開するための **session / checkpoint**、会話から作る **長期記憶**、過去の作業を次に使う **経験・ファイル記憶**を分ける。同じ提供元でも、これらを別の API が担う。scope、namespace、user ID は整理や検索の単位であり、どの利用者にその値を許すかという認可は別に確認する。

**10 系統の役割**

|                                   | 中心となる記憶                                | 提供形態                         | 比較する論点                          |
| --------------------------------- | -------------------------------------- | ---------------------------- | ------------------------------- |
| Amazon Bedrock AgentCore Memory   | イベントから facts・好み・要約・episode を抽出         | マネージド API                    | strategy・namespace・明示削除         |
| Google Agent Platform Memory Bank | scope ごとの統合済み fact と revision          | マネージド API                    | 抽出・統合・改訂・rollback               |
| Microsoft Foundry Memory          | user profile・会話要約・手続き記憶                | preview のマネージド API           | scope の解決・TTL・個別 CRUD           |
| OpenAI Agents / Conversations     | 会話状態と sandbox の経験ファイル                  | SDK とサービス機能                  | Session と Sandbox Memory の寿命の違い |
| Anthropic Claude memory           | クライアント側ファイル／managed 文書ストア              | tool 契約と beta サービス           | 実行責任・読み取り権限・文書の版                |
| LangGraph                         | thread checkpoint と cross-thread Store | アプリに組み込む部品                   | durable backend と抽出方針を別に選ぶ      |
| Microsoft Agent Framework         | context provider と履歴検索                 | アプリに組み込む部品                   | 保存 scope と検索 scope の違い          |
| Agno                              | user\_id に結び付くユーザーメモリー                 | アプリと DB                      | 自動抽出と agent 主導編集の選択             |
| CrewAI                            | 階層 scope の統合 Memory                    | 公開実装と交換可能なストア                | 統合・削除・深い検索・scope の実装差           |
| Oracle Agent Memory               | 短期 context card と長期 fact               | Oracle AI Database を用いるライブラリ | DB の永続性とアプリの認可責任                |

表の根拠: AWS [D-LC-AWS-STRATEGY]、Google [D-LC-GOOGLE-MEMORY] [D-LC-GOOGLE-REVISIONS]、Foundry [D-LC-FOUNDRY-MEMORY]、OpenAI [D-LC-OAI-RUN] [D-LC-OAI-SANDBOX]、Anthropic [D-LC-CLAUDE-TOOL] [D-LC-CLAUDE-STORE]、LangGraph [D-LC-LANGGRAPH]、Microsoft Agent Framework [D-LC-MAF-CONTEXT] [D-LC-MAF-HISTORY]、Agno [D-LC-AGNO]、CrewAI [D-LC-CREWAI]、Oracle [D-LC-ORACLE]。

## マネージド抽出・更新サービス

### Amazon Bedrock AgentCore Memory

短期イベントから長期記憶を作る strategy を設定する。組み込み方式は user preferences、semantic、session summaries、episodic。episodic は行動と結果を持つエピソードから reflection を作るため、個人化だけでなく経験の再利用とも比較できる。namespace の設計と strategy の選択を独立した構成要素として見る。[D-LC-AWS-MEMORY] [D-LC-AWS-STRATEGY]

`DeleteMemoryRecord` は個別の長期記憶を消す API。これだけで、元イベント、他の派生記憶、再抽出による復活まで一括して制御できるとは確認していない。今回の設計では外部 ID と原資料・派生物の対応をアプリ側に残し、削除の対象と完了条件を実測する候補になる。[D-LC-AWS-DELETE]

### Google Agent Platform Memory Bank

現行の公式文書は Gemini Enterprise Agent Platform 配下にあり、従来の Vertex AI Agent Engine / ADK の Memory Bank と名前だけで別製品に数えない。LLM による抽出・consolidation、scope 付き検索、継続的な取り込み、TTL、IAM 条件を説明している。どの情報を長期化するかをサービスに委ねる比較候補である。[D-LC-GOOGLE-MEMORY]

改訂履歴は現行 fact と過去の `MemoryRevision` を分け、生成時に抽出された情報も記録する。rollback を提供するが、revision は設定で無効化でき、期限切れにもなる。削除後の revision 参照・復元には公式に 48 時間の窓がある。したがって「Delete が返れば履歴も直ちに消える」「履歴は永久保存される」のどちらも前提にできない。[D-LC-GOOGLE-REVISIONS]

**設計上の判断:** この revision は監査に有用だが、実世界での有効期間とシステムが知った時点を区別する双時間の主張モデルと同じではない。入力・抽出・統合結果を関連付ける参照元として評価する。

### Microsoft Foundry Memory

確認した公式文書は preview。`chat_summary`、`user_profile`、`procedural_memory` を設定でき、個別 memory の CRUD、既定 TTL、直接の remember / forget を説明する。抽出済み記憶を直接操作できることは、agent にすべてを任せない運用で比較する価値がある。[D-LC-FOUNDRY-MEMORY]

memory search tool での user ID 解決と、低水準 API の明示 scope は別の経路である。アプリは認証済み主体と scope の対応を決める必要がある。プレビュー文書の API を固定仕様とは扱わず、導入時の提供条件と、削除後の関連記憶の挙動を追加確認する。[D-LC-FOUNDRY-MEMORY]

## 会話状態と経験ファイル

### OpenAI Agents / Conversations / Sandbox Memory

Agents SDK の `session`、サービスの `conversationId`、`previousResponseId` は会話状態の持ち方を選ぶ仕組みであり、すべてを同時に重ねるものではない。Conversations はメッセージや tool call / output を保存する。これらを使うだけで、別途定義した型付き事実や訂正履歴が構築されるとは言えない。[D-LC-OAI-RUN] [D-LC-OAI-STATE]

一方、現行 SDK 文書には **Sandbox Memory** がある。過去の実行から得た経験を `memory_summary.md`、`MEMORY.md`、rollout summary として段階的に読む。読み取り、生成、更新の設定を持ち、会話 Session とは目的が異なる。再利用には同じ sandbox の再開、snapshot、永続ストレージ等によるファイル保持が必要で、新しい空の環境に自動継承されるわけではない。[D-LC-OAI-SANDBOX]

**設計上の判断:** タスク経験の記憶を検討する際の比較対象に加える。原資料の正本やオントロジー適合性は、会話サービスや生成された Markdown とは別に管理する。ChatGPT の個人向け memory の仕様をこれらの API に転用しない。

### Anthropic Claude memory tool / Managed Agents memory

memory tool は、モデルがファイル操作を要求する **クライアント側 tool 契約**。アプリが保存先を持ち、コマンドを実行する。同じ保存先の継続利用がセッションをまたぐ記憶につながる。API 側が任意のユーザーファイルを管理してくれる機能とは区別する。[D-LC-CLAUDE-TOOL]

Managed Agents の memory は別の beta 機能で、workspace の文書ストアを session に接続する。変更ごとの版、復元、read-only / read-write、redaction / delete を説明する。クラウドの mount と自己管理 sandbox のローカル copy / 同期には違いがある。[D-LC-CLAUDE-STORE]

**設計上の判断:** 人や agent が編集しやすい文書記憶、文書版の巻き戻しの比較候補。版の保存・redaction の API があることから、バックアップや派生物を含む消去の完全性、双時間の主張管理、OWL / SHACL による制約まで推定しない。

## 組み込みフレームワークとデータベース連携

### LangGraph

thread の状態を保つ checkpointer と、会話をまたいで使う Store を分ける。開発用の in-memory 実装と、PostgreSQL 等を用いる永続実装の寿命は異なる。今回の設計では実行再開と利用者別の記憶を接続する候補であり、LangMem が扱う抽出・更新方針とは別の層にある。[D-LC-LANGGRAPH]

採用しても、Store に何を入れ、どう根拠・訂正・権限を保つかはアプリの設計事項として残る。[既存の LangMem 調査](fact-memory.md)と組にして比較する。

### Microsoft Agent Framework

context provider は実行前後に文脈を供給・保存する拡張点。FileMemoryProvider は既定の session scope と、利用者 ID 等を渡す共有 scope を区別する。ChatHistoryMemoryProvider は履歴を検索に使い、保存タグを決める scope と、検索対象を決める scope を別に持つ。[D-LC-MAF-CONTEXT] [D-LC-MAF-HISTORY]

検索 scope を広げられることは便利だが、許可主体の判定も同時に設計する必要がある。履歴の検索結果を LLM に渡す経路と、信頼できる事実を主張として昇格する経路を分けて評価する。Foundry Memory サービスを使うことと、この実行フレームワークを使うことは同一ではない。

### Agno

DB 上で `user_id` ごとに保持するメモリーを、`session_id` と分ける。各 run の自動抽出と、agent が `update_user_memory` を呼ぶ方式を選べる。agent 主導方式の既定は作成・更新で、削除・全消去の許可を別に設定する。両方式を有効にした場合は agent 主導方式が優先され、自動抽出が省略される。[D-LC-AGNO]

**設計上の判断:** 書き込み判断を常時処理にするか tool call にするかの比較に向く。user ID があることだけでアプリ全体の認可や、原資料撤回後の依存関係管理まで解決するとは扱わない。

### CrewAI

固定コミット `deaa71e168069a1d5307340172875def4330e75b` の v1.15.23 文書は、旧来の複数 memory 型に代わる統合 `Memory` を説明する。階層 scope、複数 scope の slice、類似度・新しさ・重要度の複合順位、類似記憶の consolidation を持つ。古い解説にある short-term / long-term / entity の保存構成を現行仕様として転載しない。[D-LC-CREWAI]

公開コードでは `recall()` が未完了の保存を待ち、`forget()` が storage の削除を呼ぶ。ストアは LanceDB / Qdrant Edge や factory による差し替えを扱う。source と private のフィルターはあるが、呼び出し元が指定する API 値であるため、認証済みユーザーとの結び付けは別に必要になる。[C-LC-CREWAI-MEMORY] [C-LC-CREWAI-FACTORY]

**文書とコードの差:** 文書の read-only slice 例は `PermissionError` を示す一方、確認した `MemorySlice.remember()` は `None` を返して何もしない。これは静的確認で見つけた差であり、実行による全経路の監査ではない。ファイル単位のハッシュを台帳に残した。本体 LICENSE は MIT 表記だが、依存・モデル・商用サービスの条件は別に確認する。[D-LC-CREWAI] [C-LC-CREWAI-SCOPE] [C-LC-CREWAI-LICENSE]

### Oracle Agent Memory

短期の会話要約・context card と、長期の facts / preferences / rules を分け、Oracle AI Database を保存基盤にする。公式文書は任意の LLM / embedding provider や MCP 連携を説明する。同名に近い他の Oracle 製品の session 保存機能とは区別する。[D-LC-ORACLE]

エンドユーザーの認証・認可・scope はアプリの責任と明記されている。DB の機能が豊富なことから、このライブラリ自身に双時間の主張管理や根拠撤回が実装されているとは推定しない。既に Oracle を運用している環境での比較対象とする。[D-LC-ORACLE]

## PageIndex と組み合わせるときの判断

上記の記憶・実行基盤を使う場合も、文書内の根拠箇所を探す PageIndex は別の検索アダプターになる。session が過去の回答を持っているだけでは、現行の原資料版・アクセス権・引用先が正しいとは分からない。[PageIndex の詳細](pageindex.md)と[推奨設計](../../design/ontology-knowledge-system.md)で定義した原資料版・EvidenceRef へ戻して確認する。

比較実験では、モデル、入力会話、書き込み予算、検索予算を揃え、①会話を再開できるか、②訂正した事実を取り出せるか、③消した根拠に由来する記憶が再出現しないか、④許可された原資料版だけを引用するかを別々に測る。revision、scope、TTL という同名の設定があっても、同じ保証を持つと仮定しない。

## 確認の範囲

公式文書は確認日のスナップショットに相当する説明であり、サービスを実行した証拠ではない。選択的にコードを確認したのは CrewAI のみ。このページで触れた各 SDK 全体の license、依存、運用条件の監査はしていない。採用候補を絞った後で[評価方法](../evaluation.md)に沿って比較する。

[D-LC-AWS-MEMORY]: https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/long-term-memory-long-term.html

[D-LC-AWS-STRATEGY]: https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/long-term-configuring-built-in-strategies.html

[D-LC-AWS-DELETE]: https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/long-term-delete-memory-records.html

[D-LC-GOOGLE-MEMORY]: https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank

[D-LC-GOOGLE-REVISIONS]: https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/memory-bank/revisions

[D-LC-FOUNDRY-MEMORY]: https://learn.microsoft.com/en-us/azure/foundry/agents/how-to/memory-usage

[D-LC-OAI-RUN]: https://developers.openai.com/api/docs/guides/agents/running-agents

[D-LC-OAI-STATE]: https://developers.openai.com/api/docs/guides/conversation-state

[D-LC-OAI-SANDBOX]: https://developers.openai.com/api/docs/guides/agents/sandboxes

[D-LC-CLAUDE-TOOL]: https://platform.claude.com/docs/en/agents-and-tools/tool-use/memory-tool

[D-LC-CLAUDE-STORE]: https://platform.claude.com/docs/en/managed-agents/memory

[D-LC-LANGGRAPH]: https://docs.langchain.com/oss/python/langgraph/add-memory

[D-LC-MAF-CONTEXT]: https://learn.microsoft.com/en-us/agent-framework/concepts/agents/conversations/context-providers

[D-LC-MAF-HISTORY]: https://learn.microsoft.com/en-us/agent-framework/concepts/agents/conversations/chat-history-memory-provider

[D-LC-AGNO]: https://docs.agno.com/memory/overview

[D-LC-ORACLE]: https://docs.oracle.com/en/database/oracle/agent-memory/26.6/guide/about.html

[D-LC-CREWAI]: https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/docs/v1.15.23/en/concepts/memory.mdx

[C-LC-CREWAI-MEMORY]: https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/lib/crewai/src/crewai/memory/unified_memory.py

[C-LC-CREWAI-SCOPE]: https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/lib/crewai/src/crewai/memory/memory_scope.py

[C-LC-CREWAI-FACTORY]: https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/lib/crewai/src/crewai/memory/storage/factory.py

[C-LC-CREWAI-LICENSE]: https://github.com/crewAIInc/crewAI/blob/deaa71e168069a1d5307340172875def4330e75b/LICENSE
